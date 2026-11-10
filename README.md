# ROSOrin 机器人代码库

按用途分成两份，供逐层浏览和后续集成。

- [STM｜此前的代码资料](STM/)：保留原有资料，包含 STM32 固件镜像、控制板通信代码，以及此前一起整理的 ROS 代码；固件镜像不等于 STM32 固件工程源码。
- [ROS｜本次提供的 ROS 代码](ROS/)：保留原有 ROS 功能包层级，以下按用途导航。

| 机器人模块 | 对应代码 |
| --- | --- |
| 主控启动 | [bringup](ROS/src/bringup/) |
| 底盘、控制板、机械臂及舵机 | [driver](ROS/src/driver/) |
| 传感器与遥控 | [peripherals](ROS/src/peripherals/) |
| 标定 | [calibration](ROS/src/calibration/) |
| 建图和导航 | [slam](ROS/src/slam/) · [navigation](ROS/src/navigation/) |
| 视觉与应用 | [example](ROS/src/example/) · [app](ROS/src/app/) |
| 大模型与任务 | [large_models](ROS/src/large_models/) · [large_models_examples](ROS/src/large_models_examples/) |
| 语音 | [xf_mic_asr_offline](ROS/src/xf_mic_asr_offline/) |
| 机器人模型和仿真 | [simulations](ROS/src/simulations/) |
| 多机器人 | [multi](ROS/src/multi/) |
| 场景集成与接口 | [openclaw_controller](ROS/src/openclaw_controller/) · [interfaces](ROS/src/interfaces/) |

ROS 文件由单独附件及历史文件清单恢复：同名文件的归属依据顺序推断，尚未与原始压缩包逐文件哈希核对；原始密钥未上传。部分模型资源未上传，代码未进行编译或实机验证，具体状态见 [ROS 资源说明](ROS/README.md)。
