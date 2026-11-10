# 模块关系（Mermaid）

> 实线 = 源码中可验证的连接（A/B 级证据）；虚线 = 由配置或命名推断（C 级），需实机确认。

## 1. 整体模块架构

```mermaid
graph LR
    subgraph 输入
        V["语音 ASR"]
        CAM["相机 /depth_cam/*"]
        LID["雷达 /scan"]
        JOY["手柄 /joy"]
    end
    subgraph 决策
        AG["agent_process<br/>LLM + tools"]
        EX["llm_control / 各 demo"]
        OW["openclaw / 场景任务"]
    end
    subgraph 能力
        NAV["Nav2"]
        SLAM["SLAM"]
        KIN["kinematics 服务"]
        SRV["servo_controller"]
        APP["app 应用"]
    end
    subgraph 驱动
        ODOM["odom_publisher"]
        RRC["ros_robot_controller"]
    end
    subgraph 硬件
        STM["STM32"]
        MOT["电机/舵机"]
    end

    V --> AG
    CAM --> EX
    CAM --> APP
    LID --> APP
    LID --> SLAM
    JOY --> ODOM
    AG --> EX
    EX --> ODOM
    EX --> KIN
    OW --> KIN
    OW --> NAV
    APP --> ODOM
    NAV --> ODOM
    KIN --> SRV
    SRV --> RRC
    ODOM --> RRC
    RRC --> STM
    STM --> MOT
    SLAM -.->|/map, TF| NAV
```

## 2. 启动关系

```mermaid
graph TD
    SD["start_app_node.service"] --> BP["bringup.launch.py"]
    BP --> SC["startup_check"]
    BP --> CT["controller.launch.py"]
    BP --> DC["depth_camera.launch.py"]
    BP --> LD["lidar.launch.py"]
    BP --> RS["rosbridge_websocket"]
    BP --> WV["web_video_server"]
    BP --> AP["app/start_app.launch.py"]
    BP --> JC["joystick_control"]
    BP --> IP["init_pose"]
    CT --> IMF["imu_filter.launch.py"]
    CT --> ODL["odom_publisher.launch.py"]
    CT --> EKF["robot_localization ekf_node"]
    CT --> SCL["servo_controller.launch.py"]
    AP --> A1["lidar_node"]
    AP --> A2["line_following_node"]
    AP --> A3["object_tracking_node"]
    AP --> A4["self_driving_node"]
    DC --> AUR["aurora930_node"]
    LD --> SCLD["sclidar driver"]
    LD --> LF["laser_filters"]
```

## 3. 底盘控制

```mermaid
graph LR
    NAV2["Nav2 controller_server"] --> VN["cmd_vel_nav"]
    VN --> VS["velocity_smoother"]
    VS -->|"/cmd_vel"| APP["app_cmd_vel_callback<br/>限幅 0.2 / 0.5"]
    TELE["teleop_key_control"] -->|"/controller/cmd_vel"| CB
    JOY["joystick_control"] -->|"/controller/cmd_vel"| CB
    APPM["app (巡线/追踪/自走)"] -->|"/controller/cmd_vel"| CB
    LLM["大模型 demo（部分）"] -->|"/controller/cmd_vel"| CB
    ACKIN["/app/cmd_vel"] --> ACK["acker_cmd_vel_callback<br/>阿克曼/舵机换算"]
    ACKIN -.->|"linear.x==0 时发 PWM 舵机"| PWM2["/ros_robot_controller/pwm_servo/set_state"]
    ACK --> CB["cmd_vel_callback<br/>按 MACHINE_TYPE 分支"]
    APP --> CB
    CB --> MC["MecanumChassis.set_velocity"]
    MC --> MS["MotorsState"]
    MS -->|"/ros_robot_controller/set_motor"| RRC["ros_robot_controller<br/>set_motor_state"]
    RRC --> SDK["Board.set_motor_speed"]
    SDK -->|"AA55 帧 /dev/rrc 1Mbps"| STM["STM32"]
    STM --> PWM["电机 PWM"]
    CB -.->|"速度积分"| ODOM["/odom_raw"]
    ODOM --> EKF["EKF (two_d_mode)"]
    EKF --> OD2["/odom"]
```

## 4. 传感器数据流

```mermaid
graph LR
    RRC["ros_robot_controller"] -->|"~/imu_raw (无姿态)"| IC["imu_calib/apply_calib"]
    IC -->|"/imu_corrected"| MAD["imu_filter_madgwick"]
    MAD -->|"/imu"| EKF["EKF"]
    RRC -->|"~/battery"| BAT["/ros_robot_controller/battery"]
    RRC -->|"~/joy, ~/button, ~/sbus"| HID["控制输入"]
    LID["sclidar / ldlidar / ms200 ..."] -->|"/scan_raw"| LF["laser_filters (仅 Pro)"]
    LF -->|"/scan"| SLAM["SLAM / Nav2"]
    AUR["aurora930 / ascamera"] -->|"/depth_cam/rgb0/image_raw"| VIS["视觉节点"]
    AUR -->|"/depth_cam/depth0/image_raw"| VIS
    USB["usb_cam"] -.->|"/usb_cam/image_raw"| VIS
```

> `/scan` 只在 `MACHINE_TYPE` 含 `Pro` 时经过 laser_filters（`lidar.launch.py:42-77`）；
> 非 Pro 机型直接是驱动输出。

## 5. 视觉识别

```mermaid
graph TD
    IMG["/depth_cam/rgb0/image_raw"] --> CV["OpenCV 处理"]
    IMG --> MP["MediaPipe (example)"]
    IMG --> YOLO["yolo_node (TensorRT .engine)"]
    DEP["/depth_cam/depth0/image_raw"] --> P3D["像素→三维 (相机内参)"]
    CV --> P3D
    YOLO --> P3D
    P3D --> TG["/target_3d 或内部变量"]
    TG --> KIN["kinematics set_pose_target"]
    CV --> CMD["/controller/cmd_vel"]
    YOLO --> CMD
    MP --> CMD
```

> 原厂**没有统一的三维目标话题**：各 Demo 各自把 `(u,v,depth)` 换算后直接调服务或发速度。

## 6. SLAM 与导航

```mermaid
graph LR
    SCAN["/scan"] --> SLAM
    ODOM["/odom"] --> SLAM
    TF["TF: odom→base_footprint"] --> SLAM
    SLAM["slam_toolbox / cartographer / gmapping"] -->|"/map"| MAP["地图"]
    MAP --> AMCL["AMCL"]
    ODOM --> AMCL
    AMCL -->|"map→odom"| TF2["TF"]
    NAV["Nav2 planner/controller"] --> CMD["/cmd_vel"]
    MAP --> NAV
    TF2 --> NAV
    CMD --> OD["odom_publisher"]
```

## 7. 机械臂

```mermaid
graph LR
    CAM["深度相机"] --> PIX["像素+深度"]
    PIX --> IK["/kinematics/set_pose_target"]
    IK --> FKSO["forward/inverse_kinematics.so<br/>(闭源)"]
    IK -->|"目标脉宽 [0,1000]"| SC["/servo_controller (ServosPosition)"]
    SC --> CM["controller_manager<br/>位置/单位换算"]
    CM -->|"/ros_robot_controller/bus_servo/set_position"| RRC["ros_robot_controller"]
    RRC --> BUS["总线舵机 ID1-5 + 夹爪 ID10"]
    CM -->|"/controller_manager/joint_states"| JSS["joint_states"]
    JSS -.->|"反馈"| IK
    AG["ActionGroups (外部文件)"] -.->|"init_pose 动作组"| SC
```

## 8. AI 任务调用

```mermaid
sequenceDiagram
    participant U as 用户语音
    participant VD as vocal_detect
    participant AP as agent_process
    participant LLM as OpenAI/Ollama
    participant EX as llm_control (工具执行器)
    participant RB as 机器人

    U->>VD: 语音
    VD->>AP: /vocal_detect/asr_result
    AP->>LLM: llm_tools(prompt, tools)
    LLM-->>AP: tool_call(name, args)
    AP->>EX: /agent_process/tools
    EX->>RB: 导航 / 抓取 / 发 cmd_vel
    RB-->>EX: 结果
    EX->>AP: /agent_process/tools_result
    AP->>LLM: llm_tools_result
    LLM-->>AP: 最终回答
    AP->>RB: /agent_process/result → tts_node
```

> 这条链是**真实的**：工具有真实实现（`function_calling/llm_control.py` 中 `move_to_location`、
> `get_pixel_distance` 等）。但工具集合是**每个 Demo 硬编码**的，不是通用能力；
> 且执行器可以直接发 `/controller/cmd_vel`，没有安全层。
