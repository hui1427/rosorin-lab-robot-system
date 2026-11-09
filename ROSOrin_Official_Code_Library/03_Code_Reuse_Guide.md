# 官方源码复用指南

针对官方 GitHub `Hiwonder/ROSOrin-Pro`、提交 `6c2a621fe628cc4928c48fc11d8d982594d471eb` 的第一版静态分析。每项路径相对于 `Original_Official_Source/`；本文件所述均为移植建议，不代表已编译、已部署、可即插即用或已在机器人上验证。分类副本不是可直接编译的工作空间；请从完整原厂快照复制完整 ROS 2 包及其消息包到独立工作空间，再检查依赖和配置。

## 分级方法

- **A 可以直接复用**：仅在目标系统、软件依赖、接口和硬件匹配，并完成相应验证后认定；本次没有足够验证证据，暂不指定 A。
- **B 修改后可以复用**：保留完整包，在调整参数、路径、接口、依赖或机型配置并验证后考虑使用；不是已经修改完成。
- **C 只能通过接口调用**：当所需实现只有预编译库或外部服务时，通过其提供的接口集成；架构、ABI、来源、权限和兼容性仍需核查。
- **D 暂时不建议使用**：依赖或行为不满足项目本地、自主集成要求，或安全/兼容性尚不足以判断；待满足条件后可重新评估。
- **E 仅供学习参考**：作为示例、配置或结构参考，不能视为经过验证的生产功能。

分级为当前评估，第三方运行条件未知和真实硬件未测试会降低确定性；逐文件 CSV 中未核实的代码应继续标注待核实。机器人的底盘、舵机、执行器与通信配置均有设备差异，不得直接照搬硬件参数。

## 按功能包逐项复用

下表的等级针对建议用途，而非该包每个文件功能已逐一确认。所有路径均以 `ROS2/src_ros2/src/` 为前缀。

| 包（原路径） | 初步等级 | 复用途径与条件 |
|---|---|---|
| `bringup` | B | 从 `launch/bringup.launch.py` 对照环境变量和依赖裁剪启动链；替换固定 `ros2_ws` 路径，核对硬件节点。 |
| `app` | E | 参考 `app/` 的遥控与应用调用，先确认各示例的订阅与驱动条件。 |
| `calibration` | B | 参考 IMU 与底盘校准脚本和 YAML，重新采集目标硬件参数并检查结果。 |
| `driver/controller` | B | 参考 `controller/odom_publisher_node.py`、`mecanum.py`、`ackermann.py`，适配车体模型、速度接口和参数；横向里程计问题须验证。 |
| `driver/ros_robot_controller` | B | 使用 `ros_robot_controller_node.py` 和 `ros_robot_controller_sdk.py` 理解话题到串口协议；重新验证控制板、协议、串口和消息兼容性。 |
| `driver/ros_robot_controller_msgs` | B | 保留完整接口包并按目标 ROS 发行版构建，与对应节点的消息定义共同版本化。 |
| `driver/servo_controller` | B | 用 `controller_manager.py`、`servo_controller.py` 适配舵机 ID、限位和控制板；检查初始化服务与未导入名称的疑点。 |
| `driver/servo_controller_msgs` | B | 保留完整接口包，检查与控制节点的消息版本及范围约束。 |
| `driver/kinematics` | C | 通过 `search_kinematics_solutions_node.py` 暴露的服务接口调用配套二进制运动学实现；CPU 架构、Python ABI、几何参数和兼容性待核实。若需可维护的完全自主实现，应另行研制并验证。 |
| `driver/kinematics_msgs` | B | 完整保留服务定义并与运动学服务端保持一致，先验证接口。 |
| `driver/sdk` | E | 参考通用工具实现并核对调用方，避免假设全部适用于新设备。 |
| `peripherals` | B | 从 `launch/lidar.launch.py` 和 `depth_camera.launch.py` 识别条件分支，适配相机、雷达驱动、USB/串口及话题；所需驱动是否另装以环境为准。 |
| `slam` | B | 参考 `launch/slam.launch.py` 和配置，按传感器、TF 与目标地图选择 SLAM Toolbox 或其他分支并重新调参。 |
| `navigation` | B | 参考 `launch/` 与 Nav2 配置；重新配置机器人 footprint、速度、地图、TF、传感器和控制器，进行仿真与安全测试。 |
| `simulations/rosorin_description` | B | 参考机器人模型与启动文件，对照实际尺寸、关节、TF、惯量及传感器位置修订；模型存在不等于已验证仿真。 |
| `large_models/large_models` | D | `large_models/agent_process.py` 使用可配置 OpenAI 兼容 API；如要自主本地运行，须先核验模型服务实际位置、网络、凭据、工具调用安全和离线行为。 |
| `large_models/large_models_msgs` | B | 将消息和服务定义与目标应用一起构建、检查版本；不要把接口定义等同本地模型实现。 |
| `large_models_examples` | E | 比对 `llm_control_move.py` 等示例中的结果话题、速度发布与语音控制，先在无实机输出环境评估。 |
| `openclaw_controller` | D | 含 OpenClaw 和飞书等外部集成；先评估本地可用性、外部服务依赖和数据流，再决定是否替换或接入。 |
| `xf_mic_asr_offline` | C | 通过提供的离线语音接口调用配套预编译库；先检查架构、库来源与兼容性，不能推断拥有其实现源码。 |
| `xf_mic_asr_offline_msgs` | B | 保留消息接口包并与语音节点的版本一起构建、验证。 |
| `interfaces` | B | 参考并完整构建自定义接口；具体消息使用方需按 CSV 和源码确认。 |
| `example` | E | 按子目录阅读视觉、设备及其他示例，选择必要部分移植；用途与硬件前提逐个核验。 |
| `multi` | E | 多机器人示例供理解命名空间与配置，须按实际设备与网络架构重新验证。 |

## 关键控制对象和证据

- **底盘**：`ROS2/src_ros2/src/driver/controller/controller/odom_publisher_node.py` 订阅 `controller/cmd_vel`、`app/cmd_vel` 和 `cmd_vel`，发布 `ros_robot_controller/set_motor`；`mecanum.py` 和 `ackermann.py` 处理相关运动学。节点里的里程计按速度积分，不能据此声称编码器闭环。
- **控制板与 STM32**：`ROS2/src_ros2/src/driver/ros_robot_controller/ros_robot_controller/ros_robot_controller_node.py` 接收电机、总线舵机等 ROS 指令；同目录 `ros_robot_controller_sdk.py` 打包串口数据。仓库的 `STM32/` 是固件 HEX，不能当作可修改的完整板端源代码。
- **机械臂**：`driver/servo_controller/servo_controller/controller_manager.py` 接收 `servo_controller` 或 `joint_controller`；`servo_controller.py` 向控制板发布总线舵机目标。`driver/kinematics/kinematics/search_kinematics_solutions_node.py` 提供目标位姿等服务；具体动作必须复核限位和设备。
- **相机和雷达**：`peripherals/launch/depth_camera.launch.py` 和 `peripherals/launch/lidar.launch.py` 按环境选择驱动；驱动是否在快照、外部工作空间还是系统安装，必须按实际环境核对。
- **视觉**：`app/app/`、`example/example/` 包含多种处理和识别示例、模型资源；其对应模型格式、算力、相机和输入输出需逐例检查。
- **SLAM、导航和状态估计**：`slam/launch/slam.launch.py` 选择建图分支；`navigation/launch/include/localization.launch.py` 和 `navigation_base.launch.py` 调用 Nav2；`driver/controller/launch/controller.launch.py` 组织 IMU 滤波与 `robot_localization`。各组件之间的 TF、时间同步、地图和话题需重新核验。
- **AI 与语音**：`large_models/large_models/large_models/agent_process.py` 接收 ASR 及相机输入、调用可配置模型接口；`large_models_examples/large_models_examples/llm_control_move.py` 把结果与速度和语音输出联系起来。另有 `openclaw_controller` 和讯飞离线语音包，它们是不同路径，不应当作统一必经链路。

## 二进制与外部依赖

`driver/kinematics/kinematics/forward_kinematics.so` 与 `inverse_kinematics.so` 是预编译二进制，本次快照未见对应实现源码；`xf_mic_asr_offline/lib/` 包含 x64、arm64 语音库，具体版本与支持环境待核实。仅凭源码缺失不能推断厂商的许可政策，也不能证明目标设备可运行。STM32 HEX 是可执行固件镜像，不是板端完整源码。

启动文件引用 ROS 生态的 `robot_localization`、Nav2、`cartographer_ros`、`imu_filter_madgwick`、设备驱动及其他程序；是否需要外部工作空间、系统安装以及版本应根据每个 `package.xml`、launch 引用和目标机实测确认。源码含 `/home/ubuntu/ros2_ws` 等固定路径、环境变量和系统服务假设，须替换或复核，不能断言仓库自带完整运行环境。

## 建议验证顺序

1. 在隔离工作空间固定 ROS 发行版、Python/系统架构、依赖和编译环境，先完整构建消息包；记录失败和缺失依赖。
2. 静态核对 URDF、话题、TF、传感器标定、车体与机械臂参数；在仿真或禁用硬件输出条件下验证接口。
3. 对底盘速度、横向里程计、舵机范围、串口设备及动作方向执行低速和急停测试，再逐步启用 SLAM 与导航。
4. AI 路径先隔离真实执行器，确认模型本地部署、语音数据流、工具调用边界及故障处理后，再接入受限控制。

五张根据源码整理的启动和数据流图、具体证据与发现的问题见 [04_Module_Relationships.md](04_Module_Relationships.md)。以上步骤是建议，均未在本次分析中运行。
