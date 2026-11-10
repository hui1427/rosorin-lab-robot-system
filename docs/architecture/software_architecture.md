# 软件架构

## 1. 分层

```mermaid
graph TD
    subgraph L4["应用层（人机交互/演示）"]
        A1["app: 雷达跟随/巡线/追踪/自走"]
        A2["large_models_examples: 大模型示例"]
        A3["openclaw_controller: 场景任务"]
        A4["example: 教学示例 141 文件"]
        A5["xf_mic_asr_offline: 语音控制"]
    end
    subgraph L3["决策/交互层"]
        B1["agent_process: LLM + function calling"]
        B2["vocal_detect / tts_node"]
        B3["llm_control: 工具执行器"]
    end
    subgraph L2["能力层"]
        C1["navigation: Nav2"]
        C2["slam: slam_toolbox/cartographer/gmapping"]
        C3["kinematics: 正逆解服务(闭源 .so)"]
        C4["servo_controller: 舵机管理"]
    end
    subgraph L1["驱动层"]
        D1["controller: 运动学 + odom + EKF"]
        D2["ros_robot_controller: 串口桥"]
        D3["peripherals: 雷达/相机/IMU/手柄"]
    end
    subgraph L0["硬件"]
        E1["STM32 (FreeRTOS)"]
        E2["电机 x4 / 总线舵机 / PWM 舵机"]
        E3["SCLIDAR / Aurora / IMU"]
    end

    A1 --> D1
    A2 --> B3
    A3 --> C3
    A4 --> D1
    A5 --> B2
    B1 --> B3
    B3 --> D1
    B3 --> C3
    B3 --> C1
    C1 --> D1
    C2 --> D3
    C3 --> C4
    C4 --> D2
    D1 --> D2
    D2 --> E1
    E1 --> E2
    D3 --> E3
```

实线均为源码中可验证的调用/话题关系；L4→L1 的交叉调用是原厂风格（应用层直接发 cmd_vel），不是分层设计。

## 2. 包职责与依赖

| 包 | 类型 | 职责 | 关键依赖 |
|---|---|---|---|
| `bringup` | 启动 | 总 launch + 开机自检 | controller/peripherals/app |
| `controller` | 驱动 | 运动学、odom、EKF、初始位姿 | ros_robot_controller_msgs、robot_localization |
| `ros_robot_controller` | 驱动 | 串口协议、IMU/电池/按键/舵机 | pyserial、ros_robot_controller_msgs |
| `peripherals` | 驱动 | 雷达/相机/IMU 滤波/手柄/键盘 | 各雷达与相机厂商包、laser_filters |
| `servo_controller` | 驱动 | 舵机位置管理、joint_states | ros_robot_controller_msgs、control_msgs |
| `kinematics` | 能力 | 机械臂 IK/FK 服务 | **闭源 .so**、kinematics_msgs |
| `navigation` | 能力 | Nav2 参数与 launch | nav2_* |
| `slam` | 能力 | 3 种 SLAM + rrt_explorer | slam_toolbox、cartographer、gmapping |
| `app` | 应用 | 雷达避障/跟随/警卫、巡线、目标追踪 | OpenCV、interfaces |
| `large_models` | 应用 | Asr→LLM→Tools 调度、TTS | `speech`（**不在仓库**） |
| `openclaw_controller` | 应用 | 底盘/机械臂/导航/抓取技能 | kinematics、servo_controller |
| `simulations/rosorin_description` | 模型 | URDF/xacro/mesh + RViz | 源码包内无 Gazebo launch（Gazebo 包在教程资源 `simulations.zip`） |
| `multi` / `test_stand` / `calibration` / `example` | 辅助 | 多机、测试架、标定、教学 | — |

## 3. 数据流分层

### 3.1 控制下行

```
Nav2 controller_server → cmd_vel_nav → velocity_smoother → /cmd_vel
                                                              │
                                                    app_cmd_vel_callback
                                                    （限幅 0.2 m/s / 0.5 rad/s）
                                                              │
遥控/键盘/app/大模型 ────────────────────────► /controller/cmd_vel
                                                              │
                                                    cmd_vel_callback
                                                    （按 MACHINE_TYPE 分支）
                                                              ▲
                             /app/cmd_vel ─► acker_cmd_vel_callback
                                             （换算后才进 cmd_vel_callback）
                                                              │
                                      MecanumChassis/AckermannChassis.set_velocity
                                                              │
                                              MotorsState → /ros_robot_controller/set_motor
                                                              │
                                     ros_robot_controller_node.set_motor_state
                                                              │
                                    Board.set_motor_speed → 串口 AA 55 … CRC8
                                                              │
                                                       STM32 → 电机
```

### 3.2 状态上行

```
STM32 → Board.recv_task（AA55 解析） → 各 queue
      → ros_robot_controller_node.pub_callback（50 Hz 线程）
      → /ros_robot_controller/{imu_raw,battery,button,joy,sbus}
      → imu_calib → imu_filter_madgwick → /imu
      → odom_publisher（速度积分）→ /odom_raw
      → robot_localization EKF（odom0=odom_raw, imu0=imu, two_d_mode）
      → /odom
```

## 4. 两个"名义 vs 实际"的差异

| 名义 | 实际 |
|---|---|
| `driver/sdk` 是"机器人 SDK" | 实际是通用工具包（`common.py` 图像/数学、`fps.py`、`led.py`、第三方 `pid.py`），**与 STM32 无关** |
| `servo_controller` 节点的名字 | launch 里 executable 叫 `servo_controller`，但代码构造时命名为 `controller_manager`（`controller_manager.py:116`），所以话题是 `/controller_manager/*` |

## 5. 与课题组 lab_ws 的接口边界

可直接复用：`controller`（运动学+odom）、`ros_robot_controller`、`servo_controller`、`peripherals` 的雷达/相机。
不建议复用：`app`、`bringup`、`large_models*`、`openclaw_controller`（应用层，直接抢底盘）。
必须重写：控制源仲裁、安全监督、任务编排（见 `06_reuse_guide/source_reuse_guide.md`）。
