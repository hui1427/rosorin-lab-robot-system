# 硬件与源码对应关系

## 1. 总表

| 硬件 | 设备节点 | Jetson 驱动 | 话题/服务 | 消息 | STM32 功能码 | 证据 |
|---|---|---|---|---|---|---|
| STM32 主控 | `/dev/rrc`（udev 软链） | `ros_robot_controller_sdk.py:Board` | — | — | — | A |
| 驱动电机 ×4 | 经 STM32 | `mecanum.py` / `ackermann.py` + `odom_publisher` | `/ros_robot_controller/set_motor` | `MotorsState` | 3（sub 0x01） | A |
| 电机编码器 | 经 STM32 | **Jetson 端无解析代码** | 无 | 无 | 未知 | A（缺失） |
| 总线舵机（机械臂） | 经 STM32 | `servo_controller` | `/servo_controller`、`/ros_robot_controller/bus_servo/set_position` | `ServosPosition` | 5 | A |
| PWM 舵机（转向） | 经 STM32 | `odom_publisher` | `/ros_robot_controller/pwm_servo/set_state` | `SetPWMServoState` | 4（0x03/0x07） | A |
| IMU | 经 STM32 | `ros_robot_controller` | `/ros_robot_controller/imu_raw` → `/imu` | `sensor_msgs/Imu` | 7 | A |
| 电池 | 经 STM32 | `ros_robot_controller` | `/ros_robot_controller/battery` | `std_msgs/UInt16` | 0（sub 0x04） | A |
| 按键 | 经 STM32 | `ros_robot_controller` | `~/button` | `ButtonState` | 6 | A |
| 手柄 | USB 接收器 → STM32 | `ros_robot_controller` | `~/joy` | `sensor_msgs/Joy` | 8 | A |
| SBUS 遥控 | 经 STM32 | `ros_robot_controller` | `~/sbus` | `Sbus` | 9 | A |
| OLED | 经 STM32 | `ros_robot_controller` | `~/set_oled` | `OLEDState` | 10 | A |
| 蜂鸣器/LED/RGB | 经 STM32 | `ros_robot_controller` | `~/set_buzzer`、`~/set_led`、`~/set_rgb` | 对应 msg | 1/2 | A |
| 激光雷达 | `/dev/lidar`（udev 软链） | `sclidar_ros2` 等 | `/scan_raw` → `/scan` | `LaserScan` | — | A |
| 深度相机 | USB | `aurora930_node` | `/depth_cam/rgb0/*`、`/depth_cam/depth0/*` | `Image`/`CameraInfo` | — | A |
| USB 相机 | `/dev/video*` | `usb_cam` | `/usb_cam/image_raw` | `Image` | — | A |
| 麦克风 | USB | 讯飞离线模块 | `/vocal_detect/*` | 自定义 | — | B |

## 2. 设备名依赖链

```
udev 规则 ──► 设备软链 ──► 驱动默认值 ──► 节点能不能起来
```

| 软链 | 由谁创建 | 谁在用 | 风险 |
|---|---|---|---|
| `/dev/rrc` | `peripherals/scripts/99-ttyACM0.rules` | `Board(device="/dev/rrc")` | 该规则末行有语法错误（见下） |
| `/dev/lidar` | `peripherals/scripts/lidar.rules` | 雷达驱动 | 按 USB 物理端口匹配，换口失效 |
| `/dev/ttyACM0` | `driver/ros_robot_controller/scripts/99-ttyACM0.rules` | 无（SDK 用 /dev/rrc） | 两条规则重复且不一致 |

### udev 规则缺陷（A 级）

`peripherals/scripts/99-ttyACM0.rules` 末行：

```
ATTRS{idVendor}=="1a86", ATTRS{idProduct}=="55d4", ENV{ID_MM_PORT_IGNORE}="1"1"
```

结尾多了 `1"`，**整行可能被 udev 判为非法而忽略**，导致 `/dev/rrc` 不生成、SDK 打不开串口。
这可以解释"换机器/重装系统后串口找不到"一类问题。

## 3. 硬件动作 → 代码位置

| 用户可见动作 | 触发代码 | 最终帧 |
|---|---|---|
| 前进/后退/横移/自转 | `MecanumChassis.set_velocity` → `odom_publisher.cmd_vel_callback` | FUNC=3 |
| 手柄控制 | `joystick_control.axes_callback` | FUNC=3（经 cmd_vel） |
| 机械臂到指定位姿 | `set_pose_target` → IK → `ServosPosition` | FUNC=5 |
| 夹爪开合 | `ServosPosition(id=10)` | FUNC=5 |
| 阿克曼转向 | `SetPWMServoState(id=1)` | FUNC=4 |
| 开机提示音 | `startup_check.main` → `BuzzerState` | FUNC=2 |
| 屏幕显示 IP | `startup_check` → `OLEDState` | FUNC=10 |
| 语音唤醒 | 讯飞模块（STM32 之外的 USB 设备） | 不经 STM32 |

## 4. 未闭环的硬件环节

| 环节 | 状态 |
|---|---|
| 编码器 → 轮速 | ❌ Jetson 端无代码；`/odom_raw` 是速度指令积分 |
| 电池电压 → 低电保护 | ❌ 有数据、无消费逻辑 |
| 舵机温度/电压保护 | ⚠️ SDK 提供读取接口，业务层未使用 |
| 急停按钮 | ❌ 按键有上报，未被定义为急停 |
| 电机电流/堵转 | ❌ 无接口 |

## 5. 给课题组的接线建议

1. 上电前先确认 `ls -l /dev/rrc /dev/lidar`；
2. 若 `/dev/rrc` 不存在，优先检查那条 udev 规则的语法；
3. 底盘控制只走一个入口（`/controller/cmd_vel`），由仲裁器独占；
4. 电池单位用万用表标定一次后写进配置，再做低电策略。
