# sensors · 传感器

## 1. 驱动矩阵

| 传感器 | 默认 | 可选驱动 | 入口 |
|---|---|---|---|
| 激光雷达 | `SCLIDAR` | LD19、MS200、A1、G4、LD14P、STL06N | `peripherals/launch/lidar.launch.py` |
| 深度相机 | `aurora`（Aurora 930） | ascamera、hp60c、usb_cam | `peripherals/launch/depth_camera.launch.py` |
| IMU | STM32 内置 | 无 | `peripherals/launch/imu_filter.launch.py` |
| 麦克风 | WonderEchoPro | 讯飞离线模块 | `xf_mic_asr_offline/launch/*` |
| 手柄 | 随 bringup | — | `peripherals/launch/joystick_control.launch.py` |
| 键盘 | 手动 | — | `peripherals/launch/teleop_key_control.launch.py` |

## 2. 雷达链路

```
驱动节点（sclidar / ldlidar / oradar / sllidar / ydlidar）
      │  发布 /scan_raw
      ▼
laser_filters/scan_to_scan_filter_chain   ← 仅当 MACHINE_TYPE 含 "Pro"
      │  remap: scan→scan_raw, scan_filtered→scan
      ▼
    /scan  → SLAM / Nav2
```

证据：`peripherals/launch/lidar.launch.py:42-77`。

**注意**：非 Pro 机型不经过滤波，`/scan` 直接由驱动发布；且第 80 行把
`scan_raw` 这个 `LaunchConfiguration` 重新赋值成字符串，靠 Python 变量遮蔽实现分支。

### 已知问题

| 问题 | 位置 | 说明 |
|---|---|---|
| `MS200` 分支加载的是 LD19 的 launch | `lidar.launch.py:28-30` | `ms200_scan.launch.py` 被注释掉，实际跑 LD19 驱动 |
| 缺少 `MS200` 的串口参数 | 同上 | LD19 驱动默认参数未必匹配 MS200 |
| 滤波配置按机型硬编码 | 第 56-67 行 | 改机型需改代码 |

## 3. 相机链路

`depth_camera.launch.py` 按 `DEPTH_CAMERA_TYPE` 三选一：

| 类型 | 节点 | 话题前缀 |
|---|---|---|
| `aurora` | `deptrum-ros-driver-aurora930:aurora930_node` | `/depth_cam/rgb0/image_raw`、`/depth_cam/depth0/image_raw`、`/depth_cam/depth0/camera_info` |
| `ascamera` | `ascamera:ascamera_node` | 同上（由 camera_name 决定） |
| 其他 | `usb_cam:usb_cam_node_exe` + 静态 TF | `/usb_cam/image_raw` |

证据：`peripherals/launch/include/aurora930.launch.py`、`app/app/object_tracking.py:146`、
`example/example/rgbd_function/track_and_grab.py:149-151`。

> 同一目录下存在 `aurora930.launch copy.py`（带空格和 copy），属于**重复文件**，不应入库。

## 4. IMU 链路

```
STM32 ──(FUNC=7)──► ros_robot_controller_node
                        │ /ros_robot_controller/imu_raw（orientation 全 0）
                        ▼
                 imu_calib/apply_calib（读 calibration/config/imu_calib.yaml）
                        │ /imu_corrected
                        ▼
                 imu_filter_madgwick
                        │ /imu（含姿态四元数）
                        ▼
                     EKF
```

证据：`peripherals/launch/imu_filter.launch.py`；`ros_robot_controller_node.py:328-358`。

关键点：**原始 IMU 没有姿态**（四元数全 0），姿态完全由 madgwick 积分得到；
`imu_calib` 的标定文件由 `command` 第 23 行的 `do_calib` 生成，属于一次性人工步骤。

## 5. 输入设备

| 设备 | 节点 | 输出 | 备注 |
|---|---|---|---|
| 手柄 | `peripherals:joystick_control` | `/controller/cmd_vel` | 限速 0.2 m/s、2.0 rad/s（可参数化） |
| 键盘 | `peripherals:teleop_key_control` | `/controller/cmd_vel` | 无速度限制 |
| 手机 APP | 经 rosbridge | `/controller/cmd_vel` 或 `/app/cmd_vel` | 取决于 APP 实现 |

## 6. udev 规则

| 文件 | 作用 |
|---|---|
| `ros_robot_controller/scripts/99-ttyACM0.rules` | 把 STM32 CDC 设备固定为 `/dev/rrc` |
| `peripherals/scripts/99-ttyACM0.rules` | 同上（重复） |
| `peripherals/scripts/lidar.rules` | 雷达串口 |
| `peripherals/scripts/99-usb-cam.rules` | USB 相机 |
| `peripherals/scripts/99-obsensor-libusb.rules` | 奥比中光 USB 权限（13 KB，厂商提供） |
| `peripherals/scripts/xf_mic.rules` | 讯飞麦克风 |

`create_udev_rules.sh` 负责把规则拷到 `/etc/udev/rules.d/`。

## 7. 复用建议

* 雷达/相机驱动：**直接复用**（都是标准 ROS 2 包）。
* `lidar.launch.py`：**修改后复用**（修 MS200 分支、去掉硬编码机型判断）。
* IMU：直接复用整条链，但要知道"姿态是算出来的"，静止漂移需实测。
* 输入设备：不复用（会绕开课题组仲裁器），只保留 `/joy` 原始数据。
