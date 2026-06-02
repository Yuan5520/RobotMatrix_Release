# RobotMatrix Inspector 参数说明

本文档用于发布仓库，解释 RobotMatrix Inspector 中主要参数的默认值和调参方向。默认值已按当前 Inspector 对齐。

## IK 基础参数

| 参数 | 默认值 | 调整方向 |
|------|--------|----------|
| Max Iterations | 150 | 提高可增强困难姿态求解能力，但会增加单次求解时间 |
| Position Threshold | 0.001 | 越小位置精度越高，收敛要求也越严格 |
| Rotation Threshold | 0.001 | 越小姿态精度越高，困难姿态可能需要更多迭代 |
| Damping Factor | 0.5 | 提高可增强稳定性，降低可增强响应速度 |
| Position Weight | 1 | 提高会更优先满足 TCP 位置 |
| Rotation Weight | 0.3 | 提高会更优先满足姿态，降低可让位置控制更稳 |

## 加权与限位

| 参数 | 默认值 | 调整方向 |
|------|--------|----------|
| Enable Weighted DLS | true | 建议保持开启，用于关节限位附近的运动抑制 |
| Soft Limit Activation Zone | 0.15 | 增大后更早进入限位保护 |
| Soft Limit Max Penalty | 25 | 增大后限位附近关节运动更受抑制 |
| Null Space Mid Range Gain | 0.6 | 增大后仅位置 IK 更倾向把关节拉回中位 |
| Limit Escape Zone | 0.25 | 增大后更早进入限位逃逸逻辑 |
| Limit Escape Scale | 0.25 | 增大后逃逸动作更明显 |
| Limit Escape Flip Count | 2 | 连续触发次数门槛 |
| Limit Escape Flip Scale | 1.15 | 限位逃逸翻转时的放大系数 |

## 多解搜索与解质量

| 参数 | 默认值 | 调整方向 |
|------|--------|----------|
| Multi Solution Trials | 12 | 增大可提升多解质量，但会增加计算量 |
| Two Phase Filter Top N | 0 | 0 表示关闭两阶段筛选；需要大量 trials 时再开启 |
| Good Threshold | 0.3 | 解质量分层阈值，越小越严格 |
| Easing Priority Coefficient | 0.2 | 增大后更偏向运动连续性 |
| Tier Decay Coefficient | 0.7 | 控制不同质量层级之间的衰减 |
| Joint Motion Preferences | 0.5, 1.2, 1.2, 1.1, 1, 0.9 | 越大表示越偏好该关节参与运动 |

## 奇异处理与突变抑制

| 参数 | 默认值 | 调整方向 |
|------|--------|----------|
| Enable Adaptive Damping | true | 建议保持开启，用于困难姿态稳定化 |
| Singularity Threshold | 0.01 | 越大越早认为接近奇异 |
| Adaptive Damping Scale | 1 | 增大后奇异附近阻尼更强 |
| Enable Targeted Perturbation | true | 建议开启，用于停滞和困难姿态逃逸 |
| Perturbation Singular Weight | 0.6 | 奇异因素在扰动中的权重 |
| Perturbation Limit Factor Weight | 0.4 | 限位因素在扰动中的权重 |
| Singularity Escape Magnitude | 0.05 | 扰动幅度，过大可能造成跳动 |
| Stagnation Threshold | 3 | 连续停滞触发门槛 |
| Enable Singularity Metrics | true | 开启后可输出奇异相关指标 |
| Singularity Score Weight | 0.15 | 解评分中奇异风险的权重 |
| Singular Step Floor | 0.25 | 深度奇异时的最小步长缩放 |
| Singular Step Damping | 0.45 | 对高奇异贡献关节的额外抑制 |
| Joint Mutation Threshold | 0.25 | 关节突变判断阈值 |
| Joint Mutation Penalty | 25 | 关节突变惩罚强度 |

## 运行时参数

| 参数 | 默认值 | 调整方向 |
|------|--------|----------|
| Compute Mode | Synchronous | 当前 Inspector 不暴露异步模式，运行时默认同步 |
| Delta Frame | TCP | IK 增量默认按 TCP 坐标系解释 |
| Enable Keyboard Input | true | 是否启用键盘输入 |
| FK Joint Speed | 30 deg/s | FK 关节手动控制速度 |
| IK Move Speed | 0.2 m/s | IK 末端平移速度 |
| IK Rotate Speed | 45 deg/s | IK 末端旋转速度 |
| Max Joint Interpolation Speed | 60 deg/s | IK 应用层关节插值限速，应高于 FK/IK 角速度 |

## 推荐调参顺序

1. 先调运行速度、迭代上限和位置/姿态阈值。
2. 再调限位保护、关节偏好和中位回归。
3. 最后针对困难姿态调整奇异处理、扰动和突变抑制。
4. 插值最大角速度应高于 FK 关节速度和 IK 旋转速度，否则松开按键后可能还会继续补运动。
5. 如果机械臂表现为“反应慢”，优先检查速度限制和阻尼。
6. 如果机械臂表现为“跳动”，优先检查关节偏好、突变惩罚和限位逃逸幅度。
