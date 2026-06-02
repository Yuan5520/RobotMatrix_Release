# CyberYuan RobotMatrix

CyberYuan RobotMatrix 是面向工业机器人上位控制、数字孪生和协作场景调试的软件层。它帮助开发者在 Unity 和 C# API 中统一管理机械臂模型、运行时控制、交互输入、运动指令、轨迹数据和外部算法融合。

![RobotMatrix digital twin showcase](images/robotmatrix-hero-digital-twin.png)

## 产品定位

RobotMatrix 的目标，是让机器人调试和数字孪生不再完全依赖机械臂内部运动控制卡。对于智能制造、协作机器人、非标工作站和算法验证场景，RobotMatrix 提供一个可调、可记录、可扩展的上位控制基础。

它适合：

- 工业机械臂数字孪生与虚拟调试。
- 协作场景、非标工站和智能制造方案验证。
- 机器人运动学、轨迹、数据库和外部算法融合。
- 需要通过 C# API 或 Unity Inspector 快速搭建机器人控制流程的项目。

## 核心能力

| 能力 | 说明 |
|------|------|
| 机械臂模型与运动学 | 提供机械臂模型、FK、IK、关节状态和求解结果 API |
| 上位运行时控制 | 通过 `RobotArmBehaviour` 和 `RobotArmController` 绑定 Unity 场景与机械臂控制逻辑 |
| 虚拟运动指令 | 支持 MoveJ、MoveL、MoveC 等运动指令与轨迹插补 |
| 交互调试 | 支持键盘、鼠标、手柄输入，便于调试关节和 TCP |
| 协作场景适配 | 支持关节限位、运动偏好、碰撞接口、奇异场景稳定化和轨迹清洗 |
| 数据闭环 | 集成录制、持久化、数据库桥接和轨迹清洗，支撑仿真与真实数据闭环 |

![RobotMatrix workstation showcase](images/robotmatrix-workstation-showcase.png)

## 模块组成

| 模块 | 定位 |
|------|------|
| `RobotMatrix.Kinematics` | FK / IK / 关节模型 / 求解结果数据 |
| `RobotMatrix.Controller` | 机械臂控制器、层级分析、运行时控制调度 |
| `RobotMatrix.Runtime` | Unity MonoBehaviour 集成、数据库桥接、录制与恢复流程 |
| `RobotMatrix.Interaction` | 输入处理、命令分发、关节与 TCP 交互命令 |
| `RobotMatrix.VirtualSimulation.Instructions` | MoveJ / MoveL / MoveC 指令、速度规划、路径参数 |
| `RobotMatrix.VirtualSimulation.IO` | 虚拟 IO、连接状态、电源状态和等待调度 |
| `RobotMatrix.TrajectoryCleaner` | 原始轨迹清洗、标定、可执行轨迹生成与数据库配置 |
| `RobotMatrix.Data` | Inspector 可序列化配置 |
| `RobotMatrix.Editor` | 自定义 Inspector 与编辑器工具 |

## 依赖

请先安装 BaseToolkit，再安装 RobotMatrix。

BaseToolkit 发布仓库：[Yuan5520/BaseToolkit_Release](https://github.com/Yuan5520/BaseToolkit_Release)

RobotMatrix 使用 BaseToolkit 提供的 `RobotMatrix.Math`、`IEngine`、`Collision`、`Persistence`、`Recorder`、`DBManager` 和 `BaseToolkit.TaskPool` 等基础能力。

## 快速开始

1. 导入 BaseToolkit 发布包。
2. 导入 RobotMatrix 发布包。
3. 在场景中创建或导入机器人模型。
4. 添加 `RobotArmBehaviour` 组件。
5. 配置关节 Transform、末端执行器、关节参数和运行时参数。
6. 激活 RobotMatrix License 后进入 Play Mode。

## Inspector 参数

RobotMatrix 的默认参数已按 6 轴工业臂稳定调试场景统一。当前 Inspector 默认使用同步 IK 计算；异步计算模式不作为用户默认调参入口暴露。

详细参数说明请参考 `docs/RobotMatrix_Inspector_Parameters.md`。

## License

首次使用需要激活 RobotMatrix License。请在 Unity 菜单中打开：

```text
CyberYuan > RM License > Activate
```

如果同时安装 BaseToolkit、RobotMatrix、RealDrive，可使用：

```text
CyberYuan > License Overview
```

查看统一授权状态。
