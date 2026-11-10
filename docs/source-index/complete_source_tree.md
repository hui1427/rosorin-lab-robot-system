# 原厂源码完整目录树

> 根路径 `src_ros2/`，共 998 个文件（含隐藏配置文件）。
> 每行末尾为该文件的证据等级与功能摘要；`A` 为逐行精读，`B` 为解析其结构，`C` 为静态扫描，`D` 为二进制/资源。

## ./

- `.gitignore` — [A] Git 忽略规则：install/build/log、地图、音频、生成的 msg
- `.gitmodules` — [A] 唯一子模块 large_models 指向厂家内网 Git
- `.robotrc` — [A] Shell 环境：source .typerc + 7 个工作空间叠加 + CUDA/PATH
- `.typerc` — [A] 环境变量单点配置：机型/雷达/相机/麦克风/DDS/domain/need_compile
- `command` — [A] 厂家命令手册（604 行）：整理后的可执行命令集合

## src/app/

- `package.xml` — [B] XML
- `setup.cfg` — [D] CFG
- `setup.py` — [C] Python 模块（未精读，功能待核实）

## src/app/app/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）
- `common.py` — [C] 类 ColorPicker/Heart
- `lidar_controller.py` — [C] 类 LidarController；话题节点
- `line_following.py` — [C] 类 LineFollower/LineFollowingNode；话题节点
- `obj_loader.py` — [C] 类 OBJ
- `object_tracking.py` — [C] 类 ObjectTracker/OjbectTrackingNode；话题节点

## src/app/app/model/

- `hand_landmark.py` — [C] 类 HandLandmark
- `hand_landmark_sparse_Nx3x224x224.onnx` — [D] AI 推理模型
- `palm_detection.py` — [C] 类 PalmDetection
- `palm_detection_full_inf_post_192x192.onnx` — [D] AI 推理模型
- `utils.py` — [C] Python 模块（未精读，功能待核实）

## src/app/app/msc/14568787b9f27199e9ec950aa0af565e/

- `cfg.ldata` — [D] 授权/数据文件

## src/app/launch/

- `lidar_node.launch.py` — [B] 启动脚本：拉起 1 个节点
- `line_following_node.launch.py` — [B] 启动脚本：拉起 1 个节点
- `object_tracking_node.launch.py` — [B] 启动脚本：拉起 1 个节点
- `self_driving_node.launch.py` — [B] 启动脚本：拉起 2 个节点
- `start_app.launch.py` — [B] 启动脚本（仅 include 或参数）

## src/app/models/

- `bicycle.obj` — [D] 机器人外观/贴图资源
- `chair.obj` — [D] 机器人外观/贴图资源
- `cow.obj` — [D] 机器人外观/贴图资源
- `fox.obj` — [D] 机器人外观/贴图资源
- `model.jpg` — [D] 机器人外观/贴图资源
- `pirate-ship-fat.obj` — [D] 机器人外观/贴图资源
- `rat.obj` — [D] 机器人外观/贴图资源
- `wolf.obj` — [D] 机器人外观/贴图资源

## src/app/resource/

- `app` — [D] 无扩展名

## src/app/test/

- `test_copyright.py` — [C] Python 模块（未精读，功能待核实）
- `test_flake8.py` — [C] Python 模块（未精读，功能待核实）
- `test_pep257.py` — [C] Python 模块（未精读，功能待核实）

## src/bringup/

- `package.xml` — [B] XML
- `setup.cfg` — [D] CFG
- `setup.py` — [C] Python 模块（未精读，功能待核实）

## src/bringup/bringup/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）
- `startup_check.py` — [A] 开机自检：等待 50s，蜂鸣器提示，OLED 显示 SSID 与 IP，检查麦克风

## src/bringup/launch/

- `bringup.launch.py` — [A] 原厂总启动：自检 + 底盘 + 相机 + 雷达 + rosbridge + app + 手柄 + 初始位姿

## src/bringup/launch/msc/14568787b9f27199e9ec950aa0af565e/

- `cfg.ldata` — [D] 授权/数据文件

## src/bringup/resource/

- `bringup` — [D] 无扩展名

## src/bringup/scripts/

- `expand_rootfs.service` — [D] systemd单元
- `navigation.desktop` — [D] 桌面入口
- `navigation.sh` — [D] Shell 脚本
- `slam.desktop` — [D] 桌面入口
- `slam.sh` — [D] Shell 脚本
- `start_app_node.service` — [A] systemd 单元：开机执行 zsh 并启动 bringup（路径少了前导 /）
- `start_node.sh` — [D] Shell 脚本

## src/bringup/test/

- `test_copyright.py` — [C] Python 模块（未精读，功能待核实）
- `test_flake8.py` — [C] Python 模块（未精读，功能待核实）
- `test_pep257.py` — [C] Python 模块（未精读，功能待核实）

## src/calibration/

- `package.xml` — [B] XML
- `setup.cfg` — [D] CFG
- `setup.py` — [C] Python 模块（未精读，功能待核实）

## src/calibration/calibration/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）
- `calibrate_angular.py` — [C] 类 CalibrateAngular；话题节点
- `calibrate_linear.py` — [C] 类 CalibrateLinear；话题节点
- `calibration.py` — [C] 类 CalibrationNode；话题节点

## src/calibration/config/

- `bk.yaml` — [B] 参数配置：SM,bias
- `calibration.yaml` — [B] 参数配置：depth,kinematics,pixel
- `imu_calib.yaml` — [B] 参数配置：SM,bias
- `transform.yaml` — [B] 参数配置：corners,extristric,plane,white_area_pose_cam

## src/calibration/launch/

- `angular_calib.launch.py` — [B] 启动脚本：拉起 2 个节点
- `calibration_node.launch.py` — [B] 启动脚本：拉起 1 个节点
- `linear_calib.launch.py` — [B] 启动脚本：拉起 2 个节点

## src/calibration/resource/

- `calibration` — [D] 无扩展名

## src/calibration/test/

- `test_copyright.py` — [C] Python 模块（未精读，功能待核实）
- `test_flake8.py` — [C] Python 模块（未精读，功能待核实）
- `test_pep257.py` — [C] Python 模块（未精读，功能待核实）

## src/driver/controller/

- `package.xml` — [B] XML
- `setup.cfg` — [D] CFG
- `setup.py` — [C] Python 模块（未精读，功能待核实）

## src/driver/controller/config/

- `calibrate_params.yaml` — [B] 参数配置
- `ekf.yaml` — [B] 参数配置
- `ekf_rf2o.yaml` — [B] 参数配置
- `init_pose.yaml` — [B] 参数配置
- `robot_1_ekf.yaml` — [B] 参数配置
- `robot_2_ekf.yaml` — [B] 参数配置
- `robot_3_ekf.yaml` — [B] 参数配置

## src/driver/controller/controller/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）
- `ackermann.py` — [A] 阿克曼运动学：把 (v, w) 换算成左右轮 rps + 转向舵机脉宽（限 ±34°）
- `init_pose.py` — [C] 类 InitPose；话题节点
- `mecanum.py` — [A] 麦轮运动学：把 (vx, vy, wz) 换算成 4 个电机的 rps，生成 MotorsState
- `odom_publisher_node.py` — [A] 底盘控制核心节点：订阅 3 路 cmd_vel → 运动学 → MotorsState；同时用速度积分发布 odom_raw

## src/driver/controller/launch/

- `controller.launch.py` — [B] 启动脚本：拉起 1 个节点
- `init_pose.launch.py` — [B] 启动脚本：拉起 1 个节点
- `odom_publisher.launch.py` — [B] 启动脚本：拉起 1 个节点
- `rf2o_laser_odometry.launch.py` — [B] 启动脚本：拉起 1 个节点

## src/driver/controller/resource/

- `controller` — [D] 无扩展名

## src/driver/controller/test/

- `test_copyright.py` — [C] Python 模块（未精读，功能待核实）
- `test_flake8.py` — [C] Python 模块（未精读，功能待核实）
- `test_pep257.py` — [C] Python 模块（未精读，功能待核实）

## src/driver/kinematics/

- `package.xml` — [B] XML
- `setup.cfg` — [D] CFG
- `setup.py` — [C] Python 模块（未精读，功能待核实）

## src/driver/kinematics/kinematics/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）
- `forward_kinematics.so` — [D] 闭源动态库（正/逆运动学实现，无法阅读源码）
- `inverse_kinematics.so` — [D] 闭源动态库（正/逆运动学实现，无法阅读源码）
- `kinematics_control.py` — [A] 机械臂运动学客户端封装（构造 SetRobotPose/SetJointValue 请求）
- `kinematics_demo.py` — [C] Python 模块（未精读，功能待核实）
- `search_kinematics_solutions_node.py` — [A] 机械臂正逆解服务节点：提供 set_pose_target/get_current_pose/set_joint_value_target 等服务
- `transform.py` — [C] Python 模块（未精读，功能待核实）

## src/driver/kinematics/launch/

- `kinematics_node.launch.py` — [B] 启动脚本：拉起 1 个节点

## src/driver/kinematics/resource/

- `kinematics` — [D] 无扩展名

## src/driver/kinematics/test/

- `test_copyright.py` — [C] Python 模块（未精读，功能待核实）
- `test_flake8.py` — [C] Python 模块（未精读，功能待核实）
- `test_pep257.py` — [C] Python 模块（未精读，功能待核实）

## src/driver/kinematics_msgs/

- `CMakeLists.txt` — [D] 文本
- `package.xml` — [B] XML

## src/driver/kinematics_msgs/msg/

- `JointRange.msg` — [B] MSG 接口定义（2 个字段）
- `JointsRange.msg` — [B] MSG 接口定义（5 个字段）
- `Link.msg` — [B] MSG 接口定义（5 个字段）

## src/driver/kinematics_msgs/srv/

- `GetJointRange.srv` — [B] SRV 接口定义（2 个字段）
- `GetLink.srv` — [B] SRV 接口定义（2 个字段）
- `GetRobotPose.srv` — [B] SRV 接口定义（3 个字段）
- `SetJointRange.srv` — [B] SRV 接口定义（3 个字段）
- `SetJointValue.srv` — [B] SRV 接口定义（4 个字段）
- `SetLink.srv` — [B] SRV 接口定义（3 个字段）
- `SetRobotPose.srv` — [B] SRV 接口定义（9 个字段）

## src/driver/ros_robot_controller/

- `package.xml` — [B] XML
- `setup.cfg` — [D] CFG
- `setup.py` — [C] Python 模块（未精读，功能待核实）

## src/driver/ros_robot_controller/launch/

- `ros_robot_controller.launch.py` — [B] 启动脚本：拉起 1 个节点

## src/driver/ros_robot_controller/resource/

- `ros_robot_controller` — [D] 无扩展名

## src/driver/ros_robot_controller/ros_robot_controller/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）
- `ros_robot_controller_node.py` — [A] Jetson↔STM32 桥：串口收 IMU/按键/手柄/电池，下发电机/舵机/LED/蜂鸣器/OLED
- `ros_robot_controller_sdk.py` — [A] STM32 串口协议实现：AA55 帧、CRC8、电机/舵机/IMU/电池读写、异步接收线程

## src/driver/ros_robot_controller/scripts/

- `99-ttyACM0.rules` — [B] udev 规则：固定设备名/权限
- `create_udev_rules.sh` — [D] Shell 脚本

## src/driver/ros_robot_controller/test/

- `test_copyright.py` — [C] Python 模块（未精读，功能待核实）
- `test_flake8.py` — [C] Python 模块（未精读，功能待核实）
- `test_pep257.py` — [C] Python 模块（未精读，功能待核实）

## src/driver/ros_robot_controller_msgs/

- `CMakeLists.txt` — [D] 文本
- `package.xml` — [B] XML

## src/driver/ros_robot_controller_msgs/msg/

- `BusServoState.msg` — [B] MSG 接口定义（12 个字段）
- `ButtonState.msg` — [B] MSG 接口定义（2 个字段）
- `BuzzerState.msg` — [B] MSG 接口定义（4 个字段）
- `GetBusServoCmd.msg` — [B] MSG 接口定义（10 个字段）
- `GetPWMServoCmd.msg` — [B] MSG 接口定义（3 个字段）
- `LedState.msg` — [B] MSG 接口定义（4 个字段）
- `MotorState.msg` — [B] MSG 接口定义（2 个字段）
- `MotorsState.msg` — [B] MSG 接口定义（1 个字段）
- `OLEDState.msg` — [B] MSG 接口定义（2 个字段）
- `PWMServoState.msg` — [B] MSG 接口定义（3 个字段）
- `RGBState.msg` — [B] MSG 接口定义（4 个字段）
- `RGBStates.msg` — [B] MSG 接口定义（1 个字段）
- `Sbus.msg` — [B] MSG 接口定义（2 个字段）
- `ServoPosition.msg` — [B] MSG 接口定义（2 个字段）
- `ServosPosition.msg` — [B] MSG 接口定义（2 个字段）
- `SetBusServoState.msg` — [B] MSG 接口定义（2 个字段）
- `SetPWMServoState.msg` — [B] MSG 接口定义（2 个字段）

## src/driver/ros_robot_controller_msgs/srv/

- `GetBusServoState.srv` — [B] SRV 接口定义（3 个字段）
- `GetPWMServoState.srv` — [B] SRV 接口定义（3 个字段）

## src/driver/sdk/

- `package.xml` — [B] XML
- `setup.cfg` — [D] CFG
- `setup.py` — [C] Python 模块（未精读，功能待核实）

## src/driver/sdk/resource/

- `sdk` — [D] 无扩展名

## src/driver/sdk/sdk/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）
- `common.py` — [C] 类 Colors
- `fps.py` — [C] 类 FPS
- `led.py` — [C] Python 模块（未精读，功能待核实）
- `pid.py` — [C] Ivmech PID Controller is simple implementation of a Proportional-Integral-Derivative (PID) Controller in the Python Prog

## src/driver/sdk/test/

- `test_copyright.py` — [C] Python 模块（未精读，功能待核实）
- `test_flake8.py` — [C] Python 模块（未精读，功能待核实）
- `test_pep257.py` — [C] Python 模块（未精读，功能待核实）

## src/driver/servo_controller/

- `package.xml` — [B] XML
- `setup.cfg` — [D] CFG
- `setup.py` — [C] Python 模块（未精读，功能待核实）

## src/driver/servo_controller/config/

- `servo_controller.yaml` — [B] 参数配置

## src/driver/servo_controller/launch/

- `servo_controller.launch.py` — [B] 启动脚本：拉起 1 个节点

## src/driver/servo_controller/resource/

- `servo_controller` — [D] 无扩展名

## src/driver/servo_controller/servo_controller/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）
- `action_group_controller.py` — [C] 类 ActionGroupController
- `bus_servo_control.py` — [C] 话题节点
- `controller_manager.py` — [A] 舵机管理节点(节点名 controller_manager)：订阅 /servo_controller 与 /joint_controller，发布 joint_states/servo_states
- `joint_position_controller.py` — [C] 类 JointPositionController
- `joint_trajectory_action_controller.py` — [C] 类 Segment/JointTrajectoryActionController
- `servo_controller.py` — [A] 舵机管理器：位置限幅 [0,1000]、换算时长、发布到 /ros_robot_controller/bus_servo/set_position

## src/driver/servo_controller/test/

- `test_copyright.py` — [C] Python 模块（未精读，功能待核实）
- `test_flake8.py` — [C] Python 模块（未精读，功能待核实）
- `test_pep257.py` — [C] Python 模块（未精读，功能待核实）

## src/driver/servo_controller_msgs/

- `CMakeLists.txt` — [D] 文本
- `package.xml` — [B] XML

## src/driver/servo_controller_msgs/msg/

- `ServoPosition.msg` — [B] MSG 接口定义（2 个字段）
- `ServoState.msg` — [B] MSG 接口定义（5 个字段）
- `ServoStateList.msg` — [B] MSG 接口定义（2 个字段）
- `ServosPosition.msg` — [B] MSG 接口定义（3 个字段）

## src/example/

- `package.xml` — [B] XML
- `setup.cfg` — [D] CFG
- `setup.py` — [C] Python 模块（未精读，功能待核实）

## src/example/config/

- `automatic_pick_roi.yaml` — [B] 参数配置
- `bridge_plane_distance.yaml` — [B] 参数配置
- `color_sorting_roi.yaml` — [B] 参数配置
- `garbage_classification_roi.yaml` — [B] 参数配置
- `line_follow_clean_roi.yaml` — [B] 参数配置
- `object_classification_plane_distance.yaml` — [B] 参数配置
- `plane_distance.yaml` — [B] 参数配置
- `roi.yaml` — [B] 参数配置

## src/example/example/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）

## src/example/example/ar_detect/

- `ar_detect.launch.py` — [B] 启动脚本：拉起 1 个节点
- `ar_detect.py` — [C] 类 ARNode；话题节点

## src/example/example/ar_detect/ar_models/

- `bicycle.obj` — [D] 机器人外观/贴图资源
- `chair.obj` — [D] 机器人外观/贴图资源
- `cow.obj` — [D] 机器人外观/贴图资源
- `fox.obj` — [D] 机器人外观/贴图资源
- `model.jpg` — [D] 机器人外观/贴图资源
- `pirate-ship-fat.obj` — [D] 机器人外观/贴图资源
- `rat.obj` — [D] 机器人外观/贴图资源
- `wolf.obj` — [D] 机器人外观/贴图资源

## src/example/example/body_control/

- `body_and_rgb_control.launch.py` — [B] 启动脚本：拉起 1 个节点
- `body_control.launch.py` — [B] 启动脚本：拉起 1 个节点
- `body_track.launch.py` — [B] 启动脚本：拉起 2 个节点
- `fall_down_detect.launch.py` — [B] 启动脚本：拉起 1 个节点

## src/example/example/body_control/include/

- `body_and_rgb_control.py` — [C] 类 BodyControlNode；话题节点
- `body_control.py` — [C] 类 BodyControlNode；话题节点
- `body_track.py` — [C] 类 BodyControlNode；话题节点
- `fall_down_detect.py` — [C] 类 FallDownDetectNode；话题节点

## src/example/example/color_detect/

- `color_detect_demo.py` — [C] 话题节点
- `color_detect_node.launch.py` — [B] 启动脚本：拉起 1 个节点
- `color_detect_node.py` — [C] 类 ColorDetectNode；话题节点

## src/example/example/color_detect/msc/14568787b9f27199e9ec950aa0af565e/

- `cfg.ldata` — [D] 授权/数据文件
- `kaitalk.dat` — [D] 数据

## src/example/example/color_sorting/

- `color_sorting_node.launch.py` — [B] 启动脚本：拉起 1 个节点
- `color_sorting_node.py` — [C] 类 ColorSortingNode；话题节点

## src/example/example/color_track/

- `color_track_node.launch.py` — [B] 启动脚本：拉起 1 个节点
- `color_track_node.py` — [C] 类 ColorTrackNode；话题节点

## src/example/example/garbage_classification/

- `garbage_classification.launch.py` — [B] 启动脚本：拉起 2 个节点
- `garbage_classification.py` — [C] 类 GarbageClassificationNode；话题节点

## src/example/example/hand_gesture_control/

- `hand_gesture_control_node.launch.py` — [B] 启动脚本：拉起 2 个节点
- `hand_gesture_control_node.py` — [C] 类 HandGestureControlNode；话题节点

## src/example/example/hand_track/

- `hand_detect_node.py` — [C] 类 HandDetectNode；话题节点
- `hand_track_node.launch.py` — [B] 启动脚本：拉起 2 个节点
- `hand_track_node.py` — [C] 类 HandTrackNode；话题节点

## src/example/example/hand_trajectory/

- `hand_trajectory_node.launch.py` — [B] 启动脚本：拉起 2 个节点
- `hand_trajectory_node.py` — [C] 类 State/HandTrajectoryNode；话题节点

## src/example/example/line_follow_clean/

- `line_follow_clean_node.launch.py` — [B] 启动脚本：拉起 1 个节点
- `line_follow_clean_node.py` — [C] 类 LineFollowCleanNode；话题节点

## src/example/example/mediapipe_example/

- `face_detect.py` — [C] 类 FaceDetectionNode；话题节点
- `face_mesh.py` — [C] 类 FaceMeshNode；话题节点
- `fps.py` — [C] 类 FPS
- `hand.py` — [C] 类 HandNode；话题节点
- `hand_gesture.py` — [C] 类 State/HandGestureNode；话题节点
- `holistic.py` — [C] 类 HolisticNode；话题节点
- `mediapipe_visual.py` — [C] Python 模块（未精读，功能待核实）
- `objectron.py` — [C] 类 ObjectronNode；话题节点
- `pose.py` — [C] 类 PoseNode；话题节点
- `self_segmentation.py` — [C] 类 SegmentationNode；话题节点

## src/example/example/mediapipe_example/include/

- `arm_init.launch.py` — [B] 启动脚本：拉起 1 个节点
- `arm_init.py` — [C] 类 ServoControlNode；话题节点

## src/example/example/mediapipe_example/model/

- `classifier.tflite` — [D] TFLite模型
- `deeplabv3.tflite` — [D] TFLite模型
- `detector.tflite` — [D] TFLite模型
- `face_landmarker_v2_with_blendshapes.task` — [D] MediaPipe任务模型
- `face_stylizer.task` — [D] MediaPipe任务模型
- `gesture_recognizer.task` — [D] MediaPipe任务模型
- `hand_landmarker.task` — [D] MediaPipe任务模型
- `pose_landmarker.task` — [D] MediaPipe任务模型

## src/example/example/navigation_transport/

- `automatic_pick.launch.py` — [B] 启动脚本：拉起 1 个节点
- `automatic_pick.py` — [C] 类 AutomaticPickNode；话题节点
- `navigation_transport.launch.py` — [B] 启动脚本：拉起 1 个节点
- `navigation_transport.py` — [C] 类 NavigationTransport；话题节点

## src/example/example/qrcode/

- `myQRcode.jpg` — [D] 机器人外观/贴图资源
- `qrcode_creater.py` — [C] Python 模块（未精读，功能待核实）
- `qrcode_detected.png` — [D] 机器人外观/贴图资源
- `qrcode_detecter.py` — [C] 类 QRCodeDetectNode；话题节点

## src/example/example/rgbd_function/

- `cross_bridge.launch.py` — [B] 启动脚本：拉起 1 个节点
- `cross_bridge_node.py` — [C] 类 CrossBridgeNode；话题节点
- `object_classification.launch.py` — [B] 启动脚本：拉起 1 个节点
- `object_classification.py` — [C] 类 ObjectClassificationNode；话题节点
- `prevent_falling.launch.py` — [B] 启动脚本：拉起 1 个节点
- `prevent_falling_node.py` — [C] 类 PreventFallingNode；话题节点
- `track_and_grab.launch.py` — [B] 启动脚本：拉起 1 个节点
- `track_and_grab.py` — [C] 类 ColorTracker/TrackAndGrabNode；话题节点
- `track_object.launch.py` — [B] 启动脚本：拉起 1 个节点
- `track_object_node.py` — [C] 类 TrackObjectNode；话题节点
- `vc_object_classification.launch.py` — [B] 启动脚本：拉起 1 个节点
- `vc_object_classification.py` — [C] 类 VoiceControlGrabNode；话题节点
- `vc_track_and_grab.launch.py` — [B] 启动脚本：拉起 1 个节点
- `vc_track_and_grab.py` — [C] 类 VoiceControlGrabNode；话题节点

## src/example/example/rgbd_function/config/

- `calibration.yaml` — [B] 参数配置：depth,kinematics,pixel
- `transform.yaml` — [B] 参数配置：corners,extristric,plane,white_area_pose_cam

## src/example/example/rgbd_function/utils/

- `calculate_grasp_yaw_by_depth.py` — [C] Python 模块（未精读，功能待核实）
- `calibration.py` — [C] 类 CalibrationNode；话题节点
- `camera_offset_node.py` — [C] 读取 transform.yaml → 计算相机 roll/pitch/yaw → 提供 Trigger 服务返回
- `common.py` — [C] 类 ColorPicker/Heart
- `pick_and_place.py` — [C] Python 模块（未精读，功能待核实）
- `plane_fit_node.py` — [C] Plane fitting node for Orbbec / Realsense‑style深度相机。
- `plane_object_detection_gui.py` — [C] ROS2 Shape Recognition Upper Computer Interface
- `position_change_detect.py` — [C] Python 模块（未精读，功能待核实）
- `search_plane.py` — [C] 类 SearchPlane
- `utils.py` — [C] Python 模块（未精读，功能待核实）

## src/example/example/self_driving/

- `lane_detect.py` — [C] 类 LaneDetector；话题节点
- `self_driving.launch.py` — [B] 启动脚本：拉起 2 个节点
- `self_driving.py` — [C] 类 SelfDrivingNode；话题节点

## src/example/example/self_driving/include/

- `end_orin.wav` — [D] 语音提示/唤醒词音频
- `go_out.wav` — [D] 语音提示/唤醒词音频
- `red_light.wav` — [D] 语音提示/唤醒词音频
- `stop_four.wav` — [D] 语音提示/唤醒词音频
- `stop_one.wav` — [D] 语音提示/唤醒词音频
- `stop_three.wav` — [D] 语音提示/唤醒词音频
- `stop_two.wav` — [D] 语音提示/唤醒词音频
- `turn_left_orin.wav` — [D] 语音提示/唤醒词音频
- `turn_right_orin.wav` — [D] 语音提示/唤醒词音频

## src/example/example/self_driving/include/english/

- `end_orin_en.wav` — [D] 语音提示/唤醒词音频
- `green_light_en.wav` — [D] 语音提示/唤醒词音频
- `red_light_en.wav` — [D] 语音提示/唤醒词音频
- `stop_four_en.wav` — [D] 语音提示/唤醒词音频
- `stop_one_en.wav` — [D] 语音提示/唤醒词音频
- `stop_three_en.wav` — [D] 语音提示/唤醒词音频
- `stop_two_en.wav` — [D] 语音提示/唤醒词音频
- `turn_left_orin_en.wav` — [D] 语音提示/唤醒词音频
- `turn_right_orin_en.wav` — [D] 语音提示/唤醒词音频

## src/example/example/yolo_detect/

- `utils.py` — [C] 类 Colors
- `yolo_detect.launch.py` — [B] 启动脚本：拉起 1 个节点
- `yolo_detect_demo.launch.py` — [B] 启动脚本：拉起 1 个节点
- `yolo_detect_demo.py` — [C] 类 Colors/yoloNode；话题节点
- `yolo_node.py` — [C] 类 YoloNode；话题节点

## src/example/example/yolo_detect/models/26/

- `best_garbage_26.engine` — [D] TensorRT 序列化引擎
- `best_traffic_26.engine` — [D] TensorRT 序列化引擎
- `yolo26n.engine` — [D] TensorRT 序列化引擎

## src/example/example/yolo_detect/models/v11/

- `best_garbage_11.engine` — [D] TensorRT 序列化引擎
- `best_traffic_11.engine` — [D] TensorRT 序列化引擎
- `yolov11n.engine` — [D] TensorRT 序列化引擎

## src/example/resource/

- `example` — [D] 无扩展名
- `flag.dae` — [D] 机器人外观/贴图资源

## src/example/test/

- `test_copyright.py` — [C] Python 模块（未精读，功能待核实）
- `test_flake8.py` — [C] Python 模块（未精读，功能待核实）
- `test_pep257.py` — [C] Python 模块（未精读，功能待核实）

## src/interfaces/

- `CMakeLists.txt` — [D] 文本
- `mapping_rules.yaml` — [B] 参数配置
- `package.xml` — [B] XML

## src/interfaces/msg/

- `ColorDetect.msg` — [B] MSG 接口定义（2 个字段）
- `ColorInfo.msg` — [B] MSG 接口定义（7 个字段）
- `ColorsInfo.msg` — [B] MSG 接口定义（1 个字段）
- `LineROI.msg` — [B] MSG 接口定义（3 个字段）
- `ObjectInfo.msg` — [B] MSG 接口定义（6 个字段）
- `ObjectsInfo.msg` — [B] MSG 接口定义（1 个字段）
- `PixelPosition.msg` — [B] MSG 接口定义（2 个字段）
- `Point2D.msg` — [B] MSG 接口定义（4 个字段）
- `Points.msg` — [B] MSG 接口定义（1 个字段）
- `Pose2D.msg` — [B] MSG 接口定义（5 个字段）
- `ROI.msg` — [B] MSG 接口定义（5 个字段）

## src/interfaces/srv/

- `GetBoxPosition.srv` — [B] SRV 接口定义（6 个字段）
- `GetPose.srv` — [B] SRV 接口定义（3 个字段）
- `SetBox.srv` — [B] SRV 接口定义（6 个字段）
- `SetCircleROI.srv` — [B] SRV 接口定义（3 个字段）
- `SetColorDetectParam.srv` — [B] SRV 接口定义（3 个字段）
- `SetColorRGBA.srv` — [B] SRV 接口定义（3 个字段）
- `SetFloat64.srv` — [B] SRV 接口定义（3 个字段）
- `SetFloat64List.srv` — [B] SRV 接口定义（3 个字段）
- `SetInt64.srv` — [B] SRV 接口定义（3 个字段）
- `SetLineROI.srv` — [B] SRV 接口定义（3 个字段）
- `SetPoint.srv` — [B] SRV 接口定义（3 个字段）
- `SetPose.srv` — [B] SRV 接口定义（8 个字段）
- `SetPose2D.srv` — [B] SRV 接口定义（3 个字段）
- `SetString.srv` — [B] SRV 接口定义（3 个字段）
- `SetStringBool.srv` — [B] SRV 接口定义（4 个字段）
- `SetStringList.srv` — [B] SRV 接口定义（3 个字段）

## src/large_models/

- `.gitignore` — [D] 无扩展名

## src/large_models/large_models/

- `LICENSE` — [D] 无扩展名
- `package.xml` — [B] XML
- `setup.cfg` — [D] CFG
- `setup.py` — [C] Python 模块（未精读，功能待核实）

## src/large_models/large_models/large_models/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）
- `agent_process.py` — [A] 大模型代理节点：ASR 文本 → LLM(function calling) → 发布 Tools 工具调用 → 等待结果 → 循环
- `config.py` — [C] Python 模块（未精读，功能待核实）
- `tts_node.py` — [C] 类 TTSNode；话题节点
- `vocal_detect.py` — [C] 类 VocalDetect；话题节点

## src/large_models/large_models/large_models/resources/audio/

- `dong.wav` — [D] 语音提示/唤醒词音频
- `error.wav` — [D] 语音提示/唤醒词音频
- `no_voice.wav` — [D] 语音提示/唤醒词音频
- `record_finish.wav` — [D] 语音提示/唤醒词音频
- `start_audio.wav` — [D] 语音提示/唤醒词音频
- `start_track.wav` — [D] 语音提示/唤醒词音频
- `track_fail.wav` — [D] 语音提示/唤醒词音频
- `tts_audio.wav` — [D] 语音提示/唤醒词音频
- `wakeup.wav` — [D] 语音提示/唤醒词音频

## src/large_models/large_models/large_models/resources/audio/en/

- `dong.wav` — [D] 语音提示/唤醒词音频
- `error.wav` — [D] 语音提示/唤醒词音频
- `no_voice.wav` — [D] 语音提示/唤醒词音频
- `record_finish.wav` — [D] 语音提示/唤醒词音频
- `start_audio.wav` — [D] 语音提示/唤醒词音频
- `start_track.wav` — [D] 语音提示/唤醒词音频
- `track_fail.wav` — [D] 语音提示/唤醒词音频
- `wakeup.wav` — [D] 语音提示/唤醒词音频

## src/large_models/large_models/large_models/resources/audio/offline/

- `dong.wav` — [D] 语音提示/唤醒词音频
- `error.wav` — [D] 语音提示/唤醒词音频
- `no_voice.wav` — [D] 语音提示/唤醒词音频
- `record_finish.wav` — [D] 语音提示/唤醒词音频
- `start_audio.wav` — [D] 语音提示/唤醒词音频
- `start_track.wav` — [D] 语音提示/唤醒词音频
- `track_fail.wav` — [D] 语音提示/唤醒词音频
- `wakeup.wav` — [D] 语音提示/唤醒词音频

## src/large_models/large_models/large_models/resources/audio/offline/en/

- `dong.wav` — [D] 语音提示/唤醒词音频
- `error.wav` — [D] 语音提示/唤醒词音频
- `no_voice.wav` — [D] 语音提示/唤醒词音频
- `record_finish.wav` — [D] 语音提示/唤醒词音频
- `start_audio.wav` — [D] 语音提示/唤醒词音频
- `start_track.wav` — [D] 语音提示/唤醒词音频
- `track_fail.wav` — [D] 语音提示/唤醒词音频
- `wakeup.wav` — [D] 语音提示/唤醒词音频

## src/large_models/large_models/launch/

- `agent_process.launch.py` — [B] 启动脚本：拉起 1 个节点
- `start.launch.py` — [B] 启动脚本（仅 include 或参数）
- `tts_node.launch.py` — [B] 启动脚本：拉起 1 个节点
- `vocal_detect.launch.py` — [B] 启动脚本：拉起 1 个节点

## src/large_models/large_models/resource/

- `large_models` — [D] 无扩展名

## src/large_models/large_models/test/

- `test_copyright.py` — [C] Python 模块（未精读，功能待核实）
- `test_flake8.py` — [C] Python 模块（未精读，功能待核实）
- `test_pep257.py` — [C] Python 模块（未精读，功能待核实）

## src/large_models/large_models_msgs/

- `CMakeLists.txt` — [D] 文本
- `LICENSE` — [D] 无扩展名
- `package.xml` — [B] XML

## src/large_models/large_models_msgs/msg/

- `AgentResult.msg` — [B] MSG 接口定义（2 个字段）
- `Tools.msg` — [B] MSG 接口定义（3 个字段）
- `Transport.msg` — [B] MSG 接口定义（3 个字段）

## src/large_models/large_models_msgs/srv/

- `SetBox.srv` — [B] SRV 接口定义（5 个字段）
- `SetContent.srv` — [B] SRV 接口定义（14 个字段）
- `SetInt32.srv` — [B] SRV 接口定义（3 个字段）
- `SetModel.srv` — [B] SRV 接口定义（14 个字段）
- `SetString.srv` — [B] SRV 接口定义（3 个字段）
- `SetTools.srv` — [B] SRV 接口定义（3 个字段）
- `SetTransportObject.srv` — [B] SRV 接口定义（3 个字段）

## src/large_models_examples/

- `package.xml` — [B] XML
- `setup.cfg` — [D] CFG
- `setup.py` — [C] Python 模块（未精读，功能待核实）

## src/large_models_examples/config/

- `automatic_pick_roi.yaml` — [B] 参数配置
- `automatic_transport_roi.yaml` — [B] 参数配置

## src/large_models_examples/large_models_examples/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）
- `llm_color_track.launch.py` — [B] 启动脚本：拉起 1 个节点
- `llm_color_track.py` — [C] 类 LLMColorTrack；话题节点
- `llm_control_move.launch.py` — [B] 启动脚本：拉起 2 个节点
- `llm_control_move.py` — [A] 大模型动作示例：用 eval 解析 LLM 返回的 JSON action 列表，直接发 /controller/cmd_vel
- `llm_control_move_offline.py` — [C] 类 LLMControlMove；话题节点
- `llm_visual_patrol.launch.py` — [B] 启动脚本：拉起 1 个节点
- `llm_visual_patrol.py` — [C] 类 LLMColorTrack；话题节点
- `navigation_controller.py` — [C] 类 NavigationController；话题节点
- `track_anything.py` — [C] 类 ObjectTracker
- `tracker.py` — [C] 类 StopWatch/Config/BackBoneProcessor
- `vllm_navigation.launch.py` — [B] 启动脚本：拉起 2 个节点
- `vllm_navigation.py` — [C] 类 VLLMNavigation；话题节点
- `vllm_track.launch.py` — [B] 启动脚本：拉起 2 个节点
- `vllm_track.py` — [C] 类 VLLMTrack；话题节点
- `vllm_with_camera.launch.py` — [B] 启动脚本：拉起 1 个节点
- `vllm_with_camera.py` — [C] 类 VLLMWithCamera；话题节点

## src/large_models_examples/large_models_examples/color_sorting/

- `common.py` — [C] 类 ColorPicker/Heart
- `llm_object_sorting.launch.py` — [B] 启动脚本：拉起 1 个节点
- `llm_object_sorting.py` — [C] 类 LLMColorSorting；话题节点
- `object_sorting.py` — [C] 类 ObjectSortingNode；话题节点
- `object_sorting_node.launch.py` — [B] 启动脚本：拉起 1 个节点

## src/large_models_examples/large_models_examples/color_sorting/config/

- `calibration.yaml` — [B] 参数配置：kinematics,pixel
- `calibration_color.yaml` — [B] 参数配置：kinematics,pixel
- `calibration_transport.yaml` — [B] 参数配置：kinematics,pixel
- `calibration_waste.yaml` — [B] 参数配置：kinematics,pixel
- `config.yaml` — [B] 参数配置
- `lab_config.yaml` — [B] 参数配置
- `positions.yaml` — [B] 参数配置：color_sortting,tag_sortting,tag_stackup,waste_classification
- `transform.yaml` — [B] 参数配置：corners,extristric,plane,white_area_pose_cam

## src/large_models_examples/large_models_examples/color_sorting/hed_model/

- `deploy.prototxt` — [D] Caffe配置
- `hed_pretrained_bsds.caffemodel` — [D] Caffe模型

## src/large_models_examples/large_models_examples/color_sorting/utils/

- `calculate_grasp_yaw.py` — [C] Gripper rotation angle calculation module for robotic arm during object grasping.
- `calculate_grasp_yaw_by_depth.py` — [C] Python 模块（未精读，功能待核实）
- `calibration.py` — [C] 类 CalibrationNode；话题节点
- `common.py` — [C] 类 Colors
- `distortion_inverse_map.py` — [C] Python 模块（未精读，功能待核实）
- `image_process.py` — [C] 类 CropLayer/Colors/GetObjectSurface
- `pick_and_place.py` — [C] Python 模块（未精读，功能待核实）
- `position_change_detect.py` — [C] Python 模块（未精读，功能待核实）
- `search_plane.py` — [C] 类 SearchPlane
- `utils.py` — [C] Python 模块（未精读，功能待核实）

## src/large_models_examples/large_models_examples/function_calling/

- `__init__.py` — [C] Function-calling examples.
- `llm_agent_progress.launch.py` — [B] 启动脚本：拉起 3 个节点
- `llm_control.py` — [A] 工具调用执行器：定义 10+ 个机器人工具（取深度/识别/导航/机械臂），把工具调用转成真实动作
- `llm_control_progress.launch.py` — [B] 启动脚本：拉起 3 个节点

## src/large_models_examples/large_models_examples/function_calling/road_network_llm/

- `__init__.py` — [C] Road-network function-calling tools.
- `road_network_tool.launch.py` — [B] 启动脚本（仅 include 或参数）
- `road_network_tool.py` — [C] 类 LogColors/LLMControlMove；话题节点

## src/large_models_examples/large_models_examples/function_calling/smart_logistics_llm/

- `__init__.py` — [C] Smart logistics function-calling tools.
- `smart_logistics_llm.launch.py` — [B] 启动脚本（仅 include 或参数）
- `smart_logistics_tool.py` — [C] 类 LogColors/SmartLogisticsTool；话题节点

## src/large_models_examples/large_models_examples/navigation_transport/

- `automatic_pick.launch.py` — [B] 启动脚本：拉起 1 个节点
- `automatic_pick.py` — [C] 类 AutomaticPickNode；话题节点
- `navigation_transport.launch.py` — [B] 启动脚本：拉起 1 个节点
- `vllm_navigation_transport.launch.py` — [B] 启动脚本：拉起 1 个节点
- `vllm_navigation_transport.py` — [C] 类 VLLMNavigation；话题节点

## src/large_models_examples/large_models_examples/object_transport/

- `object_transport.py` — [C] 类 ObjectTransport；话题节点
- `object_transport_node.launch.py` — [B] 启动脚本：拉起 1 个节点
- `object_transport_test.py` — [C] Python 模块（未精读，功能待核实）
- `vllm_object_transport.launch.py` — [B] 启动脚本：拉起 2 个节点
- `vllm_object_transport.py` — [C] 类 VLLMObjectTransport；话题节点

## src/large_models_examples/large_models_examples/resources/models/

- `FastSAM-x.pt` — [D] PyTorch模型
- `nanotrack_backbone_exam.engine` — [D] TensorRT 序列化引擎
- `nanotrack_backbone_sim.onnx` — [D] AI 推理模型
- `nanotrack_backbone_temp.engine` — [D] TensorRT 序列化引擎
- `nanotrack_head.engine` — [D] TensorRT 序列化引擎
- `nanotrack_head_sim.onnx` — [D] AI 推理模型

## src/large_models_examples/large_models_examples/road_network/

- `nav2_execution_node.py` — [C] 类 NavigationController；话题节点
- `road_network.launch.py` — [B] 启动脚本：拉起 3 个节点
- `road_network_builder.py` — [C] 类 FileComboBox/MapCanvas/WaypointEditor
- `road_network_navigator.py` — [C] 类 RoadNetworkNavigator；话题节点

## src/large_models_examples/large_models_examples/road_network/config/

- `nav2_controller_rpp.yaml` — [B] 参数配置：controller_server
- `nav2_params.yaml` — [B] 参数配置：amcl,amcl_map_client,amcl_rclcpp_node,bt_navigator
- `navigation_road_network.rviz` — [B] RViz配置
- `road_network.yaml` — [B] 参数配置：waypoints
- `road_network_community.yaml` — [B] 参数配置：waypoints
- `road_network_factory.yaml` — [B] 参数配置：waypoints
- `road_network_logistics.yaml` — [B] 参数配置：waypoints
- `self_driving_network.yaml` — [B] 参数配置：waypoints

## src/large_models_examples/large_models_examples/road_network/include/

- `bringup.launch.py` — [B] 启动脚本：拉起 1 个节点
- `localization.launch.py` — [B] 启动脚本（仅 include 或参数）
- `navigation.launch.py` — [B] 启动脚本（仅 include 或参数）
- `navigation_base.launch.py` — [B] 启动脚本（仅 include 或参数）

## src/large_models_examples/large_models_examples/transport_dietitianl/

- `apritag_control.py` — [C] 类 AprilTagHighAngleAligner；话题节点
- `automatic_transport.launch.py` — [B] 启动脚本：拉起 1 个节点
- `automatic_transport.py` — [C] 类 AutomaticTransportNode；话题节点
- `transport_dietitianl.launch.py` — [B] 启动脚本：拉起 1 个节点
- `vllm_transport_dietitianl.launch.py` — [B] 启动脚本：拉起 1 个节点
- `vllm_transport_dietitianl.py` — [C] 类 VLLMTransportDietitianl；话题节点

## src/large_models_examples/large_models_examples/vllm_track_arm/

- `arm_track_anything.py` — [C] 类 ObjectTracker
- `vllm_track_arm.launch.py` — [B] 启动脚本：拉起 2 个节点
- `vllm_track_arm.py` — [C] 类 Center/VLLMArmTrack；话题节点

## src/large_models_examples/large_models_examples/waste_classification/

- `llm_waste_classification.launch.py` — [B] 启动脚本：拉起 1 个节点
- `llm_waste_classification.py` — [C] 类 LLMColorSorting；话题节点
- `waste_classification.launch.py` — [B] 启动脚本：拉起 2 个节点
- `waste_classification.py` — [C] 类 WasteClassificationNode；话题节点

## src/large_models_examples/resource/

- `large_models_examples` — [D] 无扩展名

## src/large_models_examples/test/

- `test_copyright.py` — [C] Python 模块（未精读，功能待核实）
- `test_flake8.py` — [C] Python 模块（未精读，功能待核实）
- `test_pep257.py` — [C] Python 模块（未精读，功能待核实）

## src/multi/

- `package.xml` — [B] XML
- `setup.cfg` — [D] CFG
- `setup.py` — [C] Python 模块（未精读，功能待核实）

## src/multi/launch/

- `amcl.launch.py` — [B] 启动脚本：拉起 2 个节点
- `carto_nav.launch.py` — [B] 启动脚本：拉起 1 个节点
- `joy_control.launch.py` — [B] 启动脚本（仅 include 或参数）
- `joystick_control_multi.py` — [C] 类 ButtonState/JoystickController；话题节点
- `map.launch.py` — [B] 启动脚本：拉起 2 个节点
- `multi_controller.launch.py` — [B] 启动脚本（仅 include 或参数）
- `navigation_launch.py` — [C] Python 模块（未精读，功能待核实）
- `robot1_amcl.launch.py` — [B] 启动脚本：拉起 2 个节点
- `robot1_nav.launch.py` — [B] 启动脚本：拉起 1 个节点
- `robot2_amcl.launch.py` — [B] 启动脚本：拉起 2 个节点
- `robot2_nav.launch.py` — [B] 启动脚本：拉起 1 个节点
- `robot3_amcl.launch.py` — [B] 启动脚本：拉起 2 个节点
- `robot3_nav.launch.py` — [B] 启动脚本：拉起 1 个节点

## src/multi/multi/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）
- `costmap_publish.py` — [C] 类 VirtualWallNode；话题节点
- `formation_update.py` — [C] 类 FormationUpdate
- `get_follower_point.py` — [C] 类 TfListenerNode；话题节点
- `pub_follower_goal.py` — [C] 类 NavigationClient；话题节点
- `set_odom.py` — [C] 类 SetOdomNode
- `slave_tf_listener.py` — [C] 类 SlaveTFListener
- `tf_follower.py` — [C] 类 TFFollower
- `tf_listen.py` — [C] 类 SlaveTFListener
- `tf_publish.py` — [C] Python 模块（未精读，功能待核实）

## src/multi/param/

- `robot1_amcl_param.yaml` — [B] 参数配置：robot1_amcl,bt_navigator,bt_navigator_navigate_through_poses_rclcpp_node,bt_navigator_navigate_to_pose_rclcpp_node
- `robot1_nav_param.yaml` — [B] 参数配置：bt_navigator,bt_navigator_rclcpp_node,controller_server
- `robot2_amcl_param.yaml` — [B] 参数配置：robot2_amcl,bt_navigator,bt_navigator_navigate_through_poses_rclcpp_node,bt_navigator_navigate_to_pose_rclcpp_node
- `robot2_nav_param.yaml` — [B] 参数配置：bt_navigator,bt_navigator_rclcpp_node,controller_server
- `robot3_amcl_param.yaml` — [B] 参数配置：robot3_amcl,bt_navigator,bt_navigator_navigate_through_poses_rclcpp_node,bt_navigator_navigate_to_pose_rclcpp_node
- `robot3_nav_param.yaml` — [B] 参数配置：bt_navigator,bt_navigator_rclcpp_node,controller_server

## src/multi/resource/

- `multi` — [D] 无扩展名

## src/multi/rviz/

- `imu_view.rviz` — [B] RViz配置
- `lidar_view.rviz` — [B] RViz配置
- `navigation.rviz` — [B] RViz配置

## src/multi/test/

- `test_copyright.py` — [C] Python 模块（未精读，功能待核实）
- `test_flake8.py` — [C] Python 模块（未精读，功能待核实）
- `test_pep257.py` — [C] Python 模块（未精读，功能待核实）

## src/navigation/

- `package.xml` — [B] XML
- `setup.cfg` — [D] CFG
- `setup.py` — [C] Python 模块（未精读，功能待核实）

## src/navigation/config/

- `nav2_controller_dwb.yaml` — [B] 参数配置：controller_server,controller_server_rclcpp_node
- `nav2_controller_teb.yaml` — [B] 参数配置：controller_server
- `nav2_params.yaml` — [B] 参数配置：amcl,amcl_map_client,amcl_rclcpp_node,bt_navigator
- `navigate_to_pose_minimal.xml` — [B] XML

## src/navigation/launch/

- `navigation.launch.py` — [A] 2D 导航启动：先起底层 robot.launch，10s 后起 Nav2 bringup，加载地图
- `rtabmap_navigation.launch.py` — [B] 启动脚本（仅 include 或参数）
- `rtabmapviz.launch.py` — [B] 启动脚本：拉起 1 个节点
- `rviz_navigation.launch.py` — [B] 启动脚本（仅 include 或参数）
- `rviz_rtabmap_navigation.launch.py` — [B] 启动脚本（仅 include 或参数）

## src/navigation/launch/include/

- `bringup.launch.py` — [B] 启动脚本：拉起 1 个节点
- `localization.launch.py` — [B] 启动脚本（仅 include 或参数）
- `navigation_base.launch.py` — [A] Nav2 节点组：controller/planner/bt_navigator/velocity_smoother 等，cmd_vel 经 smoother 输出
- `rtabmap.launch.py` — [B] 启动脚本：拉起 2 个节点

## src/navigation/navigation/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）

## src/navigation/resource/

- `navigation` — [D] 无扩展名

## src/navigation/rviz/

- `navigation.rviz` — [B] RViz配置
- `navigation_controller.rviz` — [B] RViz配置
- `navigation_desktop.rviz` — [B] RViz配置
- `navigation_transport.rviz` — [B] RViz配置
- `rtabmap.rviz` — [B] RViz配置
- `rtabmap01.rviz` — [B] RViz配置

## src/navigation/test/

- `test_copyright.py` — [C] Python 模块（未精读，功能待核实）
- `test_flake8.py` — [C] Python 模块（未精读，功能待核实）
- `test_pep257.py` — [C] Python 模块（未精读，功能待核实）

## src/openclaw_controller/

- `package.xml` — [B] XML
- `setup.cfg` — [D] CFG
- `setup.py` — [C] Python 模块（未精读，功能待核实）

## src/openclaw_controller/config/

- `apriltag_control_roi.yaml` — [B] 参数配置
- `navigation_position.yaml` — [B] 参数配置：navigation_positions
- `smart_community.yaml` — [B] 参数配置
- `smart_factory.yaml` — [B] 参数配置
- `smart_logistics.yaml` — [B] 参数配置

## src/openclaw_controller/launch/

- `apriltag_control_node.launch.py` — [B] 启动脚本：拉起 1 个节点
- `claw_object_track.launch.py` — [B] 启动脚本：拉起 1 个节点
- `claw_track_and_grab.launch.py` — [B] 启动脚本：拉起 1 个节点
- `navigation_manager.launch.py` — [B] 启动脚本：拉起 2 个节点
- `robot_base_control.launch.py` — [B] 启动脚本：拉起 2 个节点
- `smart_scene_navigation.launch.py` — [B] 启动脚本：拉起 3 个节点
- `voice_control.launch.py` — [B] 启动脚本：拉起 1 个节点

## src/openclaw_controller/launch/include/

- `claw_tts.launch.py` — [B] 启动脚本：拉起 1 个节点
- `device_base.launch.py` — [B] 启动脚本（仅 include 或参数）

## src/openclaw_controller/openclaw_controller/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）
- `claw_tts.py` — [C] 类 LogColor/ZH_Name/EN_Name；话题节点
- `claw_voice.py` — [C] 类 LogColor/VoiceOpenClaw；话题节点
- `config.py` — [C] Python 模块（未精读，功能待核实）

## src/openclaw_controller/openclaw_controller/apriltag_control_node/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）
- `apriltag_control_node.py` — [C] 类 ApriltagControl；话题节点

## src/openclaw_controller/openclaw_controller/claw_object_track/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）
- `claw_object_track.py` — [C] 类 ObjectTrackDetect；话题节点

## src/openclaw_controller/openclaw_controller/claw_track_and_grab/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）
- `claw_track_and_grab.py` — [C] 类 ObjectPositionTrack/ColorTracker/TrackAndGrabNode；话题节点

## src/openclaw_controller/openclaw_controller/navigation_manager/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）
- `claw_navigation_manager.py` — [C] 类 ClawNavigationManager

## src/openclaw_controller/openclaw_controller/robot_base_control/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）
- `claw_arm_group_control.py` — [C] 类 ArmController；话题节点
- `claw_move_control.py` — [C] 类 ChassisController；话题节点

## src/openclaw_controller/openclaw_controller/smart_logistics/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）
- `smart_logistics.launch.py` — [B] 启动脚本：拉起 1 个节点
- `smart_logistics.py` — [C] 类 SmartLogistics；话题节点
- `smart_logistics_vision.py` — [C] 类 SmartLogisticsVisionMixin

## src/openclaw_controller/openclaw_controller/smart_logistics/config/

- `smart_logistics.yaml` — [B] 参数配置

## src/openclaw_controller/resource/

- `openclaw_controller` — [D] 无扩展名

## src/openclaw_controller/scripts/

- `request_demo.py` — [C] Python 模块（未精读，功能待核实）

## src/openclaw_controller/test/

- `test_copyright.py` — [C] Python 模块（未精读，功能待核实）
- `test_flake8.py` — [C] Python 模块（未精读，功能待核实）
- `test_pep257.py` — [C] Python 模块（未精读，功能待核实）

## src/peripherals/

- `package.xml` — [B] XML
- `setup.cfg` — [D] CFG
- `setup.py` — [C] Python 模块（未精读，功能待核实）

## src/peripherals/config/

- `camera_info.yaml` — [B] 参数配置：camera_matrix,distortion_coefficients,rectification_matrix,projection_matrix
- `lidar_filters_config_a1.yaml` — [B] 参数配置
- `lidar_filters_config_default.yaml` — [B] 参数配置
- `lidar_filters_config_g4.yaml` — [B] 参数配置
- `lidar_filters_config_ld14p.yaml` — [B] 参数配置
- `lidar_filters_config_ld19.yaml` — [B] 参数配置
- `lidar_filters_config_ms200.yaml` — [B] 参数配置
- `lidar_filters_config_sclidar.yaml` — [B] 参数配置
- `usb_cam_param.yaml` — [B] 参数配置

## src/peripherals/launch/

- `depth_camera.launch.py` — [B] 启动脚本：拉起 1 个节点
- `imu_filter.launch.py` — [B] 启动脚本：拉起 2 个节点
- `imu_view.launch.py` — [B] 启动脚本：拉起 2 个节点
- `joystick_control.launch.py` — [B] 启动脚本：拉起 1 个节点
- `lidar.launch.py` — [A] 雷达启动：按 LIDAR_TYPE 选择驱动；Pro 机型再串 laser_filters（scan_raw→scan）
- `lidar_view.launch.py` — [B] 启动脚本：拉起 1 个节点
- `teleop_key_control.launch.py` — [B] 启动脚本：拉起 1 个节点
- `usb_cam.launch.py` — [B] 启动脚本：拉起 1 个节点

## src/peripherals/launch/include/

- `ascamera.launch.py` — [B] 启动脚本：拉起 3 个节点
- `aurora930.launch copy.py` — [C] Python 模块（未精读，功能待核实）
- `aurora930.launch.py` — [B] 启动脚本：拉起 2 个节点
- `hp60c.launch.py` — [B] 启动脚本：拉起 1 个节点
- `ldlidar_LD19.launch.py` — [B] 启动脚本：拉起 2 个节点
- `ms200_scan.launch.py` — [B] 启动脚本：拉起 1 个节点
- `ms200_scan_old.launch.py` — [B] 启动脚本：拉起 2 个节点
- `sclidar.launch.py` — [B] 启动脚本：拉起 2 个节点
- `sllidar_a1.launch.py` — [B] 启动脚本：拉起 1 个节点
- `stl06n.launch.py` — [B] 启动脚本：拉起 2 个节点
- `ydlidar_g4.launch.py` — [B] 启动脚本：拉起 1 个节点

## src/peripherals/peripherals/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）
- `demo.py` — [C] 类 JoyListener；话题节点
- `joystick_control.py` — [C] 类 ButtonState/JoystickController；话题节点
- `teleop_key_control.py` — [C] 类 TeleopControl；话题节点
- `tf_broadcaster_imu.py` — [C] 类 IMUTransformBroadcaster

## src/peripherals/peripherals/msc/14568787b9f27199e9ec950aa0af565e/

- `cfg.ldata` — [D] 授权/数据文件

## src/peripherals/resource/

- `peripherals` — [D] 无扩展名

## src/peripherals/rviz/

- `imu_view.rviz` — [B] RViz配置
- `lidar_view.rviz` — [B] RViz配置

## src/peripherals/scripts/

- `99-obsensor-libusb.rules` — [B] udev 规则：固定设备名/权限
- `99-ttyACM0.rules` — [B] udev 规则：固定设备名/权限
- `99-usb-cam.rules` — [B] udev 规则：固定设备名/权限
- `create_udev_rules.sh` — [D] Shell 脚本
- `lidar.rules` — [B] udev 规则：固定设备名/权限
- `xf_mic.rules` — [B] udev 规则：固定设备名/权限

## src/peripherals/test/

- `test_copyright.py` — [C] Python 模块（未精读，功能待核实）
- `test_flake8.py` — [C] Python 模块（未精读，功能待核实）
- `test_pep257.py` — [C] Python 模块（未精读，功能待核实）

## src/simulations/rosorin_description/

- `package.xml` — [B] XML
- `setup.cfg` — [D] CFG
- `setup.py` — [C] Python 模块（未精读，功能待核实）

## src/simulations/rosorin_description/launch/

- `display.launch.py` — [B] 启动脚本：拉起 2 个节点
- `robot_description.launch.py` — [B] 启动脚本：拉起 3 个节点
- `rviz.launch.py` — [B] 启动脚本：拉起 2 个节点

## src/simulations/rosorin_description/meshes/

- `base_link_ack.stl` — [D] 机器人外观/贴图资源
- `base_link_diff.stl` — [D] 机器人外观/贴图资源
- `base_link_mec.stl` — [D] 机器人外观/贴图资源

## src/simulations/rosorin_description/meshes/pro/arm/

- `camera_connect_link.stl` — [D] 机器人外观/贴图资源
- `camera_link.stl` — [D] 机器人外观/贴图资源
- `gripper_link.stl` — [D] 机器人外观/贴图资源
- `link1.stl` — [D] 机器人外观/贴图资源
- `link2.stl` — [D] 机器人外观/贴图资源
- `link3.stl` — [D] 机器人外观/贴图资源
- `link4.stl` — [D] 机器人外观/贴图资源
- `link5.stl` — [D] 机器人外观/贴图资源

## src/simulations/rosorin_description/meshes/pro/common/

- `lidar_link.stl` — [D] 机器人外观/贴图资源

## src/simulations/rosorin_description/meshes/pro/gripper/

- `l_in_link.stl` — [D] 机器人外观/贴图资源
- `l_link.stl` — [D] 机器人外观/贴图资源
- `l_out_link.stl` — [D] 机器人外观/贴图资源
- `r_in_link.stl` — [D] 机器人外观/贴图资源
- `r_link.stl` — [D] 机器人外观/贴图资源
- `r_out_link.stl` — [D] 机器人外观/贴图资源

## src/simulations/rosorin_description/meshes/pro/mecanum/

- `base_link.stl` — [D] 机器人外观/贴图资源

## src/simulations/rosorin_description/resource/

- `rosorin_description` — [D] 无扩展名

## src/simulations/rosorin_description/rosorin_description/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）

## src/simulations/rosorin_description/rviz/

- `view.rviz` — [B] RViz配置

## src/simulations/rosorin_description/test/

- `test_copyright.py` — [C] Python 模块（未精读，功能待核实）
- `test_flake8.py` — [C] Python 模块（未精读，功能待核实）
- `test_pep257.py` — [C] Python 模块（未精读，功能待核实）

## src/simulations/rosorin_description/urdf/

- `acker.xacro` — [B] 机器人模型：links=3,joints=2,robot=car
- `ascamera.xacro` — [B] 机器人模型：links=0,joints=0,robot=camera
- `dabai.xacro` — [B] 机器人模型：links=0,joints=0,robot=camera
- `differential.xacro` — [B] 机器人模型：links=3,joints=2,robot=car
- `materials.xacro` — [B] 机器人模型：links=0,joints=0,robot=material
- `mecanum.xacro` — [B] 机器人模型：links=3,joints=2,robot=car
- `rosorin.gazebo.xacro` — [B] 机器人模型：links=0,joints=0,robot=rosorin
- `rosorin.xacro` — [B] 机器人模型：links=0,joints=0,robot=car
- `rosorin_old.xacro` — [B] 机器人模型：links=3,joints=2,robot=car

## src/simulations/rosorin_description/urdf/pro/

- `arm.urdf.xacro` — [B] 机器人模型：links=1,joints=1,robot=arm
- `car_mecanum.urdf.xacro` — [B] 机器人模型：links=2,joints=1,robot=car
- `common.urdf.xacro` — [B] 机器人模型：links=0,joints=0,robot=common
- `connect.urdf.xacro` — [B] 机器人模型：links=0,joints=0,robot=arm
- `depth_camera.urdf.xacro` — [B] 机器人模型：links=1,joints=0,robot=depth_camera
- `gripper.urdf.xacro` — [B] 机器人模型：links=0,joints=0,robot=gripper
- `imu.urdf.xacro` — [B] 机器人模型：links=1,joints=0,robot=imu
- `inertial_matrix.xacro` — [B] 机器人模型：links=0,joints=0,robot=inertial_matrix
- `lidar.urdf.xacro` — [B] 机器人模型：links=1,joints=1,robot=lidar
- `lidar_ld19.urdf.xacro` — [B] 机器人模型：links=1,joints=1,robot=lidar
- `lidar_ms200.urdf.xacro` — [B] 机器人模型：links=1,joints=1,robot=lidar
- `rosorinpro.xacro` — [B] 机器人模型：links=0,joints=0,robot=rosorinpro

## src/slam/

- `package.xml` — [B] XML
- `setup.cfg` — [D] CFG
- `setup.py` — [C] Python 模块（未精读，功能待核实）

## src/slam/config/

- `cartographer_2d.lua` — [D] Lua
- `gmapping.yaml` — [B] 参数配置：slam_gmapping
- `slam.yaml` — [B] 参数配置

## src/slam/launch/

- `rtabmap_slam.launch.py` — [B] 启动脚本（仅 include 或参数）
- `rviz_rtabmap.launch.py` — [B] 启动脚本（仅 include 或参数）
- `rviz_slam.launch.py` — [B] 启动脚本（仅 include 或参数）
- `slam.launch.py` — [A] SLAM 总启动：按 slam_method 选 slam_toolbox / cartographer / gmapping / rrt_explorer

## src/slam/launch/include/

- `cartographer.launch.py` — [B] 启动脚本：拉起 2 个节点
- `gmapping.launch.py` — [B] 启动脚本：拉起 1 个节点
- `robot.launch.py` — [B] 启动脚本（仅 include 或参数）
- `rrt_explorer.launch copy.py` — [C] Python 模块（未精读，功能待核实）
- `rrt_explorer.launch.py` — [B] 启动脚本：拉起 8 个节点
- `rtabmap.launch.py` — [B] 启动脚本：拉起 2 个节点
- `slam_base.launch.py` — [B] 启动脚本：拉起 1 个节点

## src/slam/launch/msc/14568787b9f27199e9ec950aa0af565e/

- `cfg.ldata` — [D] 授权/数据文件

## src/slam/resource/

- `slam` — [D] 无扩展名

## src/slam/rviz/

- `rtabmap.rviz` — [B] RViz配置
- `slam.rviz` — [B] RViz配置
- `slam_desktop.rviz` — [B] RViz配置

## src/slam/slam/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）
- `map_save.py` — [C] 类 MapSaveNode

## src/slam/test/

- `test_copyright.py` — [C] Python 模块（未精读，功能待核实）
- `test_flake8.py` — [C] Python 模块（未精读，功能待核实）
- `test_pep257.py` — [C] Python 模块（未精读，功能待核实）

## src/test_stand/

- `package.xml` — [B] XML
- `setup.cfg` — [D] CFG
- `setup.py` — [C] Python 模块（未精读，功能待核实）

## src/test_stand/launch/

- `llm_color_sorting.launch.py` — [B] 启动脚本：拉起 1 个节点
- `llm_control_servo.launch.py` — [B] 启动脚本：拉起 1 个节点
- `object_sorting_node.launch.py` — [B] 启动脚本：拉起 1 个节点
- `object_transport_node.launch.py` — [B] 启动脚本：拉起 1 个节点
- `vllm_object_transport.launch.py` — [B] 启动脚本：拉起 1 个节点
- `vllm_track_arm.launch.py` — [B] 启动脚本：拉起 1 个节点
- `vllm_with_camera.launch.py` — [B] 启动脚本：拉起 1 个节点

## src/test_stand/resource/

- `test_stand` — [D] 无扩展名

## src/test_stand/test_stand/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）
- `tracker.py` — [C] 类 StopWatch/Config/BackBoneProcessor
- `vllm_with_camera.py` — [C] 类 VLLMWithCamera；话题节点

## src/test_stand/test_stand/arm_control/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）
- `llm_control_servo.py` — [C] 类 LLMControlServo；话题节点
- `llm_control_servo_offline.py` — [C] 类 LLMControlServo；话题节点

## src/test_stand/test_stand/color_sorting/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）
- `common.py` — [C] 类 ColorPicker/Heart
- `llm_object_sorting.py` — [C] 类 LLMColorSorting；话题节点
- `object_sorting.py` — [C] 类 ObjectSortingNode；话题节点

## src/test_stand/test_stand/color_sorting/config/

- `calibration.yaml` — [B] 参数配置：kinematics,pixel
- `calibration_color.yaml` — [B] 参数配置：kinematics,pixel
- `calibration_transport.yaml` — [B] 参数配置：depth,kinematics,pixel
- `calibration_waste.yaml` — [B] 参数配置：kinematics,pixel
- `config.yaml` — [B] 参数配置
- `lab_config.yaml` — [B] 参数配置
- `positions.yaml` — [B] 参数配置：color_sortting,tag_sortting,tag_stackup,waste_classification
- `transform.yaml` — [B] 参数配置：corners,extristric,plane,white_area_pose_cam

## src/test_stand/test_stand/color_sorting/hed_model/

- `deploy.prototxt` — [D] Caffe配置
- `hed_pretrained_bsds.caffemodel` — [D] Caffe模型

## src/test_stand/test_stand/color_sorting/utils/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）
- `calculate_grasp_yaw.py` — [C] Gripper rotation angle calculation module for robotic arm during object grasping.
- `calculate_grasp_yaw_by_depth.py` — [C] Python 模块（未精读，功能待核实）
- `calibration.py` — [C] 类 CalibrationNode；话题节点
- `common.py` — [C] 类 Colors
- `distortion_inverse_map.py` — [C] Python 模块（未精读，功能待核实）
- `image_process.py` — [C] 类 CropLayer/Colors/GetObjectSurface
- `pick_and_place.py` — [C] Python 模块（未精读，功能待核实）
- `position_change_detect.py` — [C] Python 模块（未精读，功能待核实）
- `search_plane.py` — [C] 类 SearchPlane
- `utils.py` — [C] Python 模块（未精读，功能待核实）

## src/test_stand/test_stand/object_transport/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）
- `object_transport.py` — [C] 类 ObjectTransport；话题节点
- `vllm_object_transport.py` — [C] 类 VLLMObjectTransport；话题节点

## src/test_stand/test_stand/resources/models/

- `FastSAM-x.pt` — [D] PyTorch模型
- `nanotrack_backbone_exam.engine` — [D] TensorRT 序列化引擎
- `nanotrack_backbone_sim.onnx` — [D] AI 推理模型
- `nanotrack_backbone_temp.engine` — [D] TensorRT 序列化引擎
- `nanotrack_head.engine` — [D] TensorRT 序列化引擎
- `nanotrack_head_sim.onnx` — [D] AI 推理模型

## src/test_stand/test_stand/vllm_track_arm/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）
- `arm_track_anything.py` — [C] 类 ObjectTracker
- `vllm_track_arm.py` — [C] 类 Center/VLLMArmTrack；话题节点

## src/xf_mic_asr_offline/

- `CMakeLists.txt` — [D] 文本
- `package.xml` — [B] XML
- `xf_mic.rules` — [B] udev 规则：固定设备名/权限

## src/xf_mic_asr_offline/config/

- `call.bnf` — [D] 文法文件

## src/xf_mic_asr_offline/config/msc/msc/14568787b9f27199e9ec950aa0af565e/

- `cfg.ldata` — [D] 授权/数据文件
- `kaitalk.dat` — [D] 数据

## src/xf_mic_asr_offline/config/msc/res/asr/

- `common.jet` — [D] JetRacer资源

## src/xf_mic_asr_offline/feedback_voice/

- `10.0V.wav` — [D] 语音提示/唤醒词音频
- `10.5V.wav` — [D] 语音提示/唤醒词音频
- `11V.wav` — [D] 语音提示/唤醒词音频
- `12V.wav` — [D] 语音提示/唤醒词音频
- `awake.wav` — [D] 语音提示/唤醒词音频
- `back.wav` — [D] 语音提示/唤醒词音频
- `blue.wav` — [D] 语音提示/唤醒词音频
- `close_fail.wav` — [D] 语音提示/唤醒词音频
- `close_success.wav` — [D] 语音提示/唤醒词音频
- `come.wav` — [D] 语音提示/唤醒词音频
- `crawl_succeeded.wav` — [D] 语音提示/唤醒词音频
- `drift.wav` — [D] 语音提示/唤醒词音频
- `end_orin.wav` — [D] 语音提示/唤醒词音频
- `fail.wav` — [D] 语音提示/唤醒词音频
- `find_target.wav` — [D] 语音提示/唤醒词音频
- `finish_mapping.wav` — [D] 语音提示/唤醒词音频
- `food_waste.wav` — [D] 语音提示/唤醒词音频
- `go.wav` — [D] 语音提示/唤醒词音频
- `go_a.wav` — [D] 语音提示/唤醒词音频
- `go_b.wav` — [D] 语音提示/唤醒词音频
- `go_c.wav` — [D] 语音提示/唤醒词音频
- `go_origin.wav` — [D] 语音提示/唤醒词音频
- `go_out.wav` — [D] 语音提示/唤醒词音频
- `green.wav` — [D] 语音提示/唤醒词音频
- `green_light.wav` — [D] 语音提示/唤醒词音频
- `hazardous_waste.wav` — [D] 语音提示/唤醒词音频
- `mission_completed.wav` — [D] 语音提示/唤醒词音频
- `ok.wav` — [D] 语音提示/唤醒词音频
- `open_fail.wav` — [D] 语音提示/唤醒词音频
- `open_success.wav` — [D] 语音提示/唤醒词音频
- `perform_tasks.wav` — [D] 语音提示/唤醒词音频
- `reach_destination.wav` — [D] 语音提示/唤醒词音频
- `recyclable_waste.wav` — [D] 语音提示/唤醒词音频
- `red.wav` — [D] 语音提示/唤醒词音频
- `red_light.wav` — [D] 语音提示/唤醒词音频
- `residual_waste.wav` — [D] 语音提示/唤醒词音频
- `running.wav` — [D] 语音提示/唤醒词音频
- `sleep.wav` — [D] 语音提示/唤醒词音频
- `start_mapping.wav` — [D] 语音提示/唤醒词音频
- `start_navigating.wav` — [D] 语音提示/唤醒词音频
- `start_track_blue.wav` — [D] 语音提示/唤醒词音频
- `start_track_green.wav` — [D] 语音提示/唤醒词音频
- `start_track_red.wav` — [D] 语音提示/唤醒词音频
- `stop.wav` — [D] 语音提示/唤醒词音频
- `stop_fail.wav` — [D] 语音提示/唤醒词音频
- `stop_four.wav` — [D] 语音提示/唤醒词音频
- `stop_one.wav` — [D] 语音提示/唤醒词音频
- `stop_three.wav` — [D] 语音提示/唤醒词音频
- `stop_track.wav` — [D] 语音提示/唤醒词音频
- `stop_two.wav` — [D] 语音提示/唤醒词音频
- `track_fail.wav` — [D] 语音提示/唤醒词音频
- `turn_left.wav` — [D] 语音提示/唤醒词音频
- `turn_left_orin.wav` — [D] 语音提示/唤醒词音频
- `turn_right.wav` — [D] 语音提示/唤醒词音频
- `turn_right_orin.wav` — [D] 语音提示/唤醒词音频
- `unknow.wav` — [D] 语音提示/唤醒词音频
- `warnning.wav` — [D] 语音提示/唤醒词音频

## src/xf_mic_asr_offline/feedback_voice/english/

- `10.0V.wav` — [D] 语音提示/唤醒词音频
- `10.5V.wav` — [D] 语音提示/唤醒词音频
- `11V.wav` — [D] 语音提示/唤醒词音频
- `12V.wav` — [D] 语音提示/唤醒词音频
- `awake.wav` — [D] 语音提示/唤醒词音频
- `back.wav` — [D] 语音提示/唤醒词音频
- `blue.wav` — [D] 语音提示/唤醒词音频
- `close_fail.wav` — [D] 语音提示/唤醒词音频
- `close_success.wav` — [D] 语音提示/唤醒词音频
- `come.wav` — [D] 语音提示/唤醒词音频
- `crwal_successed.wav` — [D] 语音提示/唤醒词音频
- `drift.wav` — [D] 语音提示/唤醒词音频
- `end_orin_en.wav` — [D] 语音提示/唤醒词音频
- `fail.wav` — [D] 语音提示/唤醒词音频
- `find_target.wav` — [D] 语音提示/唤醒词音频
- `finish_mapping.wav` — [D] 语音提示/唤醒词音频
- `food_waste.wav` — [D] 语音提示/唤醒词音频
- `go.wav` — [D] 语音提示/唤醒词音频
- `go_a.wav` — [D] 语音提示/唤醒词音频
- `go_b.wav` — [D] 语音提示/唤醒词音频
- `go_c.wav` — [D] 语音提示/唤醒词音频
- `go_origin.wav` — [D] 语音提示/唤醒词音频
- `go_out_en.wav` — [D] 语音提示/唤醒词音频
- `green.wav` — [D] 语音提示/唤醒词音频
- `green_light_en.wav` — [D] 语音提示/唤醒词音频
- `hazardous_waste.wav` — [D] 语音提示/唤醒词音频
- `mission_completed.wav` — [D] 语音提示/唤醒词音频
- `ok.wav` — [D] 语音提示/唤醒词音频
- `open_fail.wav` — [D] 语音提示/唤醒词音频
- `open_success.wav` — [D] 语音提示/唤醒词音频
- `perform_tasks.wav` — [D] 语音提示/唤醒词音频
- `reach_destination.wav` — [D] 语音提示/唤醒词音频
- `recyclable_waste.wav` — [D] 语音提示/唤醒词音频
- `red.wav` — [D] 语音提示/唤醒词音频
- `red_light_en.wav` — [D] 语音提示/唤醒词音频
- `residual_waste.wav` — [D] 语音提示/唤醒词音频
- `running.wav` — [D] 语音提示/唤醒词音频
- `start_mapping.wav` — [D] 语音提示/唤醒词音频
- `start_navigating.wav` — [D] 语音提示/唤醒词音频
- `start_track_blue.wav` — [D] 语音提示/唤醒词音频
- `start_track_green.wav` — [D] 语音提示/唤醒词音频
- `start_track_red.wav` — [D] 语音提示/唤醒词音频
- `stop.wav` — [D] 语音提示/唤醒词音频
- `stop_fail.wav` — [D] 语音提示/唤醒词音频
- `stop_four_en.wav` — [D] 语音提示/唤醒词音频
- `stop_one_en.wav` — [D] 语音提示/唤醒词音频
- `stop_track.wav` — [D] 语音提示/唤醒词音频
- `stop_two_en.wav` — [D] 语音提示/唤醒词音频
- `track_fail.wav` — [D] 语音提示/唤醒词音频
- `turn_left.wav` — [D] 语音提示/唤醒词音频
- `turn_left_orin_en.wav` — [D] 语音提示/唤醒词音频
- `turn_right.wav` — [D] 语音提示/唤醒词音频
- `turn_right_orin_en.wav` — [D] 语音提示/唤醒词音频
- `warnning.wav` — [D] 语音提示/唤醒词音频

## src/xf_mic_asr_offline/include/

- `asr_offline_record_sample.h` — [D] C头文件
- `formats.h` — [D] C头文件
- `linuxrec.h` — [D] C头文件
- `msp_cmn.h` — [D] C头文件
- `msp_errors.h` — [D] C头文件
- `msp_types.h` — [D] C头文件
- `play_path.h` — [D] C头文件
- `qise.h` — [D] C头文件
- `qisr.h` — [D] C头文件
- `qtts.h` — [D] C头文件
- `queue_internal.h` — [D] C头文件
- `queue_simple.h` — [D] C头文件
- `record_param.h` — [D] C头文件
- `speech_recognizer.h` — [D] C头文件
- `user_interface.h` — [D] C头文件
- `voice_control.h` — [D] C头文件

## src/xf_mic_asr_offline/launch/

- `mic_init.launch.py` — [B] 启动脚本：拉起 4 个节点
- `startup_test.launch.py` — [B] 启动脚本：拉起 1 个节点
- `voice_control_arm.launch.py` — [B] 启动脚本：拉起 1 个节点
- `voice_control_color_detect.launch.py` — [B] 启动脚本：拉起 1 个节点
- `voice_control_color_sorting.launch.py` — [B] 启动脚本：拉起 1 个节点
- `voice_control_color_track.launch.py` — [B] 启动脚本：拉起 1 个节点
- `voice_control_garbage_classification.launch.py` — [B] 启动脚本：拉起 1 个节点
- `voice_control_move.launch.py` — [B] 启动脚本：拉起 1 个节点
- `voice_control_navigation.launch.py` — [B] 启动脚本：拉起 1 个节点
- `voice_control_navigation_transport.launch.py` — [B] 启动脚本：拉起 1 个节点

## src/xf_mic_asr_offline/launch/msc/14568787b9f27199e9ec950aa0af565e/

- `cfg.ldata` — [D] 授权/数据文件
- `kaitalk.dat` — [D] 数据

## src/xf_mic_asr_offline/lib/arm64/

- `libmsc.so` — [D] 闭源动态库（正/逆运动学实现，无法阅读源码）
- `liboffline_record_lib.so` — [D] 闭源动态库（正/逆运动学实现，无法阅读源码）

## src/xf_mic_asr_offline/lib/x64/

- `libaiui.so` — [D] 闭源动态库（正/逆运动学实现，无法阅读源码）
- `libmsc.so` — [D] 闭源动态库（正/逆运动学实现，无法阅读源码）
- `liboffline_record_lib.so` — [D] 闭源动态库（正/逆运动学实现，无法阅读源码）

## src/xf_mic_asr_offline/scripts/

- `asr_node.py` — [C] 类 ASRNode；话题节点
- `awake_node.py` — [C] 类 CircleMic/AwakeNode；话题节点
- `voice_control_arm.py` — [C] 类 VoiceControlColorDetectNode；话题节点
- `voice_control_color_detect.py` — [C] 类 VoiceControlColorDetectNode；话题节点
- `voice_control_color_sorting.py` — [C] 类 VoiceControlColorSortingNode；话题节点
- `voice_control_color_track.py` — [C] 类 VoiceControlColorTrackNode；话题节点
- `voice_control_garbage_classification.py` — [C] 类 VoiceControlGarbageClassificationNode；话题节点
- `voice_control_move.py` — [C] 类 VoiceControMovelNode；话题节点
- `voice_control_navigation.py` — [C] 类 VoiceControlNavNode；话题节点
- `voice_control_navigation_transport.py` — [C] 类 VoiceControlNavigationTransportNode；话题节点
- `wonder_echo_pro_node.py` — [C] 类 ASRNode；话题节点

## src/xf_mic_asr_offline/scripts/msc/14568787b9f27199e9ec950aa0af565e/

- `cfg.ldata` — [D] 授权/数据文件
- `kaitalk.dat` — [D] 数据

## src/xf_mic_asr_offline/src/

- `voice_control.cpp` — [D] C++

## src/xf_mic_asr_offline/tmp/

- `all.pcm` — [D] 语音提示/唤醒词音频
- `config.txt` — [D] 文本
- `in.pcm` — [D] 语音提示/唤醒词音频
- `system.tar` — [D] 归档

## src/xf_mic_asr_offline/xf_mic_asr_offline/

- `__init__.py` — [C] Python 模块（未精读，功能待核实）
- `voice_play.py` — [C] Python 模块（未精读，功能待核实）

## src/xf_mic_asr_offline/xf_mic_asr_offline/msc/14568787b9f27199e9ec950aa0af565e/

- `cfg.ldata` — [D] 授权/数据文件

## src/xf_mic_asr_offline_msgs/

- `CMakeLists.txt` — [D] 文本
- `package.xml` — [B] XML

## src/xf_mic_asr_offline_msgs/srv/

- `GetOfflineResult.srv` — [B] SRV 接口定义（6 个字段）
- `SetString.srv` — [B] SRV 接口定义（3 个字段）
