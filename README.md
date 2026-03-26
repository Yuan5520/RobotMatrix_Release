# CyberYuan RobotMatrix

> **CyberYuan RobotMatrix** 是面向工业机器人的数字孪生仿真系统，提供运动学求解、机械臂控制、交互操作和指令插补等完整能力。

## 前置依赖

| 依赖仓库 | 说明 | 链接 |
|----------|------|------|
| **BaseToolkit** (必需) | 基础框架层，提供 IEngine、Collision、Persistence、Recorder、DBManager 等模块 | [BaseToolkit_Release](https://github.com/Yuan5520/BaseToolkit_Release) |

> **请先导入 BaseToolkit 仓库，再导入本仓库。** RobotMatrix 的运动学、控制器、运行时模块依赖 Base 提供的引擎抽象、碰撞检测、数据持久化和数据库管理功能。

## 关联产品

| 仓库 | 说明 |
|------|------|
| [RealTimeDrive_Release](https://github.com/Yuan5520/RealTimeDrive_Release) | 硬件实时驱动模块，可配合 RobotMatrix 实现仿真-实机联动 |

---

## 安装

1. 确保已导入 [BaseToolkit](https://github.com/Yuan5520/BaseToolkit)
2. 将本仓库克隆或下载到 Unity 项目的 `Assets/` 目录下
3. 确保以下目录结构正确放置：
   - `Assets/RobotMatrix/Kinematics/`
   - `Assets/RobotMatrix/Data/`
   - `Assets/RobotMatrix/Instructions/`
   - `Assets/RobotMatrix/Controller/`
   - `Assets/RobotMatrix/Interaction/`
   - `Assets/RobotMatrix/Runtime/`
   - `Assets/RobotMatrix/Editor/`
   - `Assets/RobotMatrix/License/`
   - `Assets/RobotMatrix/Tests/` (可选)
   - `Assets/FBX/`
   - `Assets/Scenes/`
   - `Assets/Plugins/RMLicense/`
   - `Assets/CyberYuanLicense/` (可选，需同时安装三个产品)
4. Unity 将自动识别 Assembly Definition 并完成编译
5. 首次使用需要激活许可证：菜单栏 `CyberYuan > RM License > Activate`

## 系统要求

- Unity 2021.3 LTS 或更高版本
- .NET Standard 2.1 / .NET Framework 4.x
- 已安装 [BaseToolkit_Release](https://github.com/Yuan5520/BaseToolkit_Release)

---

## 模块概览

### Kinematics -- 运动学引擎

工业机器人正/逆运动学求解核心。

- `FKSolver` -- 正运动学求解器，基于 DH 参数计算末端位姿
- `IKSolver` -- 逆运动学求解器，基于阻尼最小二乘法 (DLS) 的迭代求解
- `JacobianCalculator` -- 雅可比矩阵计算器，支持数值微分
- `JointWeightCalculator` -- 关节权重计算，用于冗余自由度优化
- `RobotArmModel` -- 机械臂模型定义（DH 参数、关节限位、速度限制）
- `JointConfig` / `JointState` -- 关节配置与运行时状态

### Instructions -- 运动指令

运动轨迹规划与插补。

- `CartesianInterpolator` -- 笛卡尔空间线性/圆弧插补
- `JointInterpolator` -- 关节空间插补
- `MotionEnums` -- 运动类型枚举（MoveJ / MoveL / MoveC）

### Controller -- 机械臂控制器

将运动学与引擎对象绑定，实现机械臂的逻辑驱动。

- `RobotArmController` -- 核心控制器，管理关节链、DH 参数、运动模式切换
- `JointHierarchyAnalyzer` -- 自动分析 Unity 层级结构，识别关节链

### Interaction -- 交互系统

支持键鼠、手柄等多种输入设备的机器人交互操作。

- `CommandDispatcher` -- 命令分发中心
- `InputManager` -- 输入管理器，协调多输入处理器
- **命令集：**
  - `JointAngleCommands` -- 单关节角度微调
  - `EndEffectorCommands` -- 末端执行器位姿控制
  - `JointDragKeepTCPCommand` -- 关节拖拽保持 TCP 不变
  - `ModeSwitchCommand` -- 运动模式切换
- **输入处理器：**
  - `KeyboardInputHandler` -- 键盘快捷键
  - `MouseInputHandler` -- 鼠标拖拽
  - `HandleInputHandler` -- 手柄/操纵杆

### Runtime -- 运行时集成

MonoBehaviour 层，将所有模块集成到 Unity 场景中。

- `RobotArmBehaviour` -- 机械臂主组件，Inspector 中配置所有参数
- `DatabaseUploadBridge` -- 运动数据自动上传到数据库
- `DatabaseUploadConfig` -- 上传参数配置
- `JointColliderOverrideEntry` -- 碰撞体覆盖配置

### Editor -- 编辑器扩展

- `RobotArmEditor` -- 自定义 Inspector，提供可视化调参界面

### Tests -- 测试套件

覆盖运动学、数学、控制器、交互等模块的单元测试和集成测试。

---

## 产品功能

- **完整运动学求解**：支持 6 轴及以下工业机器人的正逆运动学，基于标准 DH 参数建模
- **多种运动模式**：MoveJ（关节运动）、MoveL（直线运动）、MoveC（圆弧运动）
- **实时碰撞检测**：集成 Base 的 Collision 模块，运动过程中实时检测自碰撞和环境碰撞
- **多输入交互**：键盘、鼠标、手柄多种输入方式，支持关节拖拽、末端控制等操作模式
- **数据录制与回放**：集成 Base 的 Recorder 模块，录制关节轨迹并支持回放分析
- **数据库集成**：运动数据可自动上传至 MySQL 数据库，用于数据分析和远程监控

## 产品亮点

1. **工业级运动学** -- 基于 DH 参数标准建模，阻尼最小二乘法逆运动学求解，收敛稳定
2. **模块化架构** -- 11 个独立 Assembly Definition，编译隔离，按需引用
3. **引擎无关设计** -- 运动学核心不依赖 Unity，理论上可移植到任何 C# 运行时
4. **开箱即用** -- 内含 ABB IRB 120、IRB 1410 等机器人 FBX 模型和示例场景
5. **完整测试覆盖** -- 数学库、运动学、控制器均有单元测试保障正确性
6. **许可证保护** -- RSA-2048 + AES-256-CBC 加密的许可证系统

---

## 快速开始

1. 打开示例场景 `Assets/Scenes/SampleScene.unity`
2. 在场景中选择机器人 GameObject
3. 在 Inspector 中找到 `RobotArmBehaviour` 组件
4. 点击 Play 进入运行模式
5. 使用键盘/鼠标操控机械臂：
   - 数字键 `1-6`：选择关节
   - `Q/E`：调整当前关节角度
   - 鼠标拖拽：直接操控关节/末端

## 许可证激活

1. 菜单栏选择 `CyberYuan > RM License > Activate`
2. 在弹出窗口中输入邮箱并发送申请
3. 收到许可证密钥后，输入密钥完成激活
4. 可通过 `CyberYuan > RM License > Check Status` 查看许可证状态

## 统一许可证总览（可选）

如果同时安装了 RobotMatrix 和 RealDrive 两个产品，可通过菜单 `CyberYuan > License Overview` 查看所有产品的许可证状态。

## 技术支持

如有问题请联系 CyberYuan 技术支持团队。
