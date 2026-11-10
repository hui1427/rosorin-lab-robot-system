# command 逐行命令索引（V1.2 静态修订）

> **基线：** 本次实际提供的本地 `command` 文件，共604行、22,675字节。命令原文与原始行号直接从该文件生成；本地完整ROS 2源码树不在本轮可直接访问的附件中，下面“节点/源码”列沿用V1.1静态映射并标记**尚待本地源码复核**。未执行任何命令。

> **SHA-256（本地command）：** `dac2199039afc27bb0385a073a0ceb86f4a08a9efee23c3e45f6f8449286d3db`。GitHub `orin-nx@6c2a621` 的command为另一个版本（此前报告为310行），不能代替本地604行。

## 一、可验证的分类口径

| 记录类型 | 数量 | 说明 |
|---|---:|---|
| 历史索引总记录 | 196 | 包含非命令文本，不应称“196条有效命令” |
| 可执行命令行 | **181** | 依据原始文件逐行核对 |
| 自然语言任务示例 | 7 | 不是shell命令 |
| 终端回显/服务响应 | 8 | 不是shell命令 |

**181条可执行行的命令类型：**

| 类型 | 行数 |
|---|---:|
| ros2 launch | 80 |
| ros2 service call | 60 |
| ros2 topic pub | 14 |
| ros2 run | 1 |
| cd（含脚本调用） | 14 |
| 构建/系统脚本 | 3 |
| python3 | 4 |
| openclaw CLI | 4 |
| 其他工具 | 1 |

`181−14−3=164`仅是扣除14条`cd`行、3条构建/系统脚本后的算术值，**不得称为164条驱动机器人行为的命令**：剩余仍有OpenClaw配置、RViz、话题查看等工具命令，且部分`cd`行本身包含`&& python3`执行。

## 二、按功能分类（仅计可执行行）

| 分类 | 可执行行 |
|---|---:|
| 系统启动与管理 | 4 |
| 标定与校准 | 8 |
| 底盘应用（雷达/巡线/追踪） | 13 |
| 视觉与MediaPipe示例 | 20 |
| SLAM建图（2D） | 6 |
| 自主导航（2D） | 2 |
| SLAM/导航（3D） | 5 |
| 仿真与模型显示 | 1 |
| 视觉应用（2D/3D） | 5 |
| 语音交互 | 5 |
| 上位机工具 | 3 |
| 大模型应用（large_models_examples） | 31 |
| OpenClaw 应用 | 21 |
| 智慧场景（社区/工厂） | 20 |
| 垃圾分类 | 4 |
| 测试架（test_stand） | 22 |
| 智慧物流 | 11 |
| **合计** | **181** |

## 三、Launch追踪状态（沿用V1.1静态报告；未能独立复核源码树）

| 指标 | 统计 | 计数单位/证据边界 |
|---|---:|---|
| `ros2 launch`命令 | **80** | 本地command中的命令行数；已从原文复算 |
| 找到文件 | 79 | V1.1声称匹配的命令行数；本轮没有完整本地源码树，未独立逐项证实 |
| 可解析到启动节点 | 75 | V1.1声称的**命令行数**，不是唯一节点数；需重跑解析器确认 |
| 可关联本地源码 | 67 | V1.1声称的**命令行数**，不是唯一源码文件数；需重跑确认 |

`ros2 launch openclaw_controller robot_base_control.launch` 与 `robot_base_control.launch.py` **并非相同文件名**。本地V1.1记录前者无法直接匹配，而后者存在；这一差异不等于已经实机验证命令执行成败。

## 四、按原始行号逐条映射（完整命令不截断）

**说明：** “原始命令”和“话题/服务端点”由本轮提供的原始`command`复核；“Launch/节点/核心源码”是DeepSeek V1.1的静态映射历史记录，并非本轮独立源码验证。对于有省略号的旧内容，不把省略片段当成路径。

### 系统启动与管理

| 行号 | 完整原始命令 | 类型 | 直接端点/文件 | V1.1节点/源码线索（待复核） |
|---:|---|---|---|---|
| 1 | `colcon build --event-handlers  console_direct+  --cmake-args  -DCMAKE_BUILD_TYPE=Release --symlink-install` | 构建/系统脚本 | `—` | —; — |
| 4 | `~/.stop_ros.sh` | 构建/系统脚本 | `—` | —; — |
| 7 | `cd ~/ros2_ws && ~/.build.sh` | cd（含脚本调用） | `—` | —; — |
| 10 | `colcon build --event-handlers  console_direct+  --cmake-args  -DCMAKE_BUILD_TYPE=Release --symlink-install --packages-select xxx` | 构建/系统脚本 | `—` | —; — |

### 标定与校准

| 行号 | 完整原始命令 | 类型 | 直接端点/文件 | V1.1节点/源码线索（待复核） |
|---:|---|---|---|---|
| 16 | `ros2 launch calibration linear_calib.launch.py` | ros2 launch | `calibration / linear_calib.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 19 | `ros2 launch calibration angular_calib.launch.py` | ros2 launch | `calibration / angular_calib.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 22 | `ros2 launch ros_robot_controller ros_robot_controller.launch.py` | ros2 launch | `ros_robot_controller / ros_robot_controller.launch.py` | ros_robot_controller:ros_robot_controller; src/driver/ros_robot_controller/ros_robot_controller/ros_robot_controller_node.py |
| 23 | `ros2 run imu_calib do_calib --ros-args -r imu:=/ros_robot_controller/imu_raw --param output_file:=/home/ubuntu/ros2_ws/src/calibration/config/imu_calib.yaml` | ros2 run | `imu_calib / do_calib` | —; — |
| 26 | `ros2 launch peripherals imu_view.launch.py` | ros2 launch | `peripherals / imu_view.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 29 | `ros2 launch peripherals depth_camera.launch.py` | ros2 launch | `peripherals / depth_camera.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 30 | `rviz2` | 其他工具 | `—` | —; — |
| 34 | `ros2 launch peripherals lidar_view.launch.py` | ros2 launch | `peripherals / lidar_view.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |

### 底盘应用（雷达/巡线/追踪）

| 行号 | 完整原始命令 | 类型 | 直接端点/文件 | V1.1节点/源码线索（待复核） |
|---:|---|---|---|---|
| 41 | `ros2 launch app lidar_node.launch.py debug:=true` | ros2 launch | `app / lidar_node.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 43 | `ros2 service call /lidar_app/enter std_srvs/srv/Trigger {}` | ros2 service call | `/lidar_app/enter` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 44 | `ros2 service call /lidar_app/set_running interfaces/srv/SetInt64 "{data: 1}"` | ros2 service call | `/lidar_app/set_running` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 47 | `ros2 service call /lidar_app/enter std_srvs/srv/Trigger {}` | ros2 service call | `/lidar_app/enter` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 48 | `ros2 service call /lidar_app/set_running interfaces/srv/SetInt64 "{data: 2}"` | ros2 service call | `/lidar_app/set_running` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 51 | `ros2 service call /lidar_app/enter std_srvs/srv/Trigger {}` | ros2 service call | `/lidar_app/enter` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 52 | `ros2 service call /lidar_app/set_running interfaces/srv/SetInt64 "{data: 3}"` | ros2 service call | `/lidar_app/set_running` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 55 | `ros2 launch app line_following_node.launch.py debug:=true` | ros2 launch | `app / line_following_node.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 56 | `ros2 service call /line_following/enter std_srvs/srv/Trigger {}` | ros2 service call | `/line_following/enter` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 58 | `ros2 service call /line_following/set_running std_srvs/srv/SetBool "{data: True}"` | ros2 service call | `/line_following/set_running` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 61 | `ros2 launch app object_tracking_node.launch.py debug:=true` | ros2 launch | `app / object_tracking_node.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 62 | `ros2 service call /object_tracking/enter std_srvs/srv/Trigger {}` | ros2 service call | `/object_tracking/enter` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 64 | `ros2 service call /object_tracking/set_running std_srvs/srv/SetBool "{data: True}"` | ros2 service call | `/object_tracking/set_running` | 仅运行时发布/调用；接收方节点需按本地源码核对 |

### 视觉与MediaPipe示例

| 行号 | 完整原始命令 | 类型 | 直接端点/文件 | V1.1节点/源码线索（待复核） |
|---:|---|---|---|---|
| 70 | `ros2 launch peripherals depth_camera.launch.py` | ros2 launch | `peripherals / depth_camera.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 73 | `cd ~/ros2_ws/src/example/example/qrcode && python3 qrcode_creater.py` | cd（含脚本调用） | `—` | —; — |
| 75 | `cd ~/ros2_ws/src/example/example/qrcode && python3 qrcode_detecter.py` | cd（含脚本调用） | `—` | —; — |
| 78 | `cd ~/ros2_ws/src/example/example/mediapipe_example && python3 face_detect.py` | cd（含脚本调用） | `—` | —; — |
| 81 | `cd ~/ros2_ws/src/example/example/mediapipe_example && python3 face_mesh.py` | cd（含脚本调用） | `—` | —; — |
| 84 | `cd ~/ros2_ws/src/example/example/mediapipe_example && python3 hand.py` | cd（含脚本调用） | `—` | —; — |
| 87 | `cd ~/ros2_ws/src/example/example/mediapipe_example && python3 pose.py` | cd（含脚本调用） | `—` | —; — |
| 90 | `cd ~/ros2_ws/src/example/example/mediapipe_example && python3 self_segmentation.py` | cd（含脚本调用） | `—` | —; — |
| 93 | `cd ~/ros2_ws/src/example/example/mediapipe_example && python3 holistic.py` | cd（含脚本调用） | `—` | —; — |
| 96 | `cd ~/ros2_ws/src/example/example/mediapipe_example && python3 objectron.py` | cd（含脚本调用） | `—` | —; — |
| 99 | `cd ~/ros2_ws/src/example/example/mediapipe_example && python3 hand_gesture.py` | cd（含脚本调用） | `—` | —; — |
| 102 | `cd ~/ros2_ws/src/example/example/color_detect && python3 color_detect_demo.py` | cd（含脚本调用） | `—` | —; — |
| 108 | `ros2 launch example body_control.launch.py` | ros2 launch | `example / body_control.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 111 | `ros2 launch example body_and_rgb_control.launch.py` | ros2 launch | `example / body_and_rgb_control.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 114 | `ros2 launch example fall_down_detect.launch.py` | ros2 launch | `example / fall_down_detect.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 117 | `ros2 launch example body_track.launch.py` | ros2 launch | `example / body_track.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 120 | `ros2 launch example ar_detect.launch.py` | ros2 launch | `example / ar_detect.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 121 | `ros2 service call /ar_detect/set_model interfaces/srv/SetString "{data: 'bicycle'}"` | ros2 service call | `/ar_detect/set_model` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 131 | `ros2 launch example self_driving.launch.py debug_mode:=true` | ros2 launch | `example / self_driving.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 133 | `ros2 launch example self_driving.launch.py` | ros2 launch | `example / self_driving.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |

### SLAM建图（2D）

| 行号 | 完整原始命令 | 类型 | 直接端点/文件 | V1.1节点/源码线索（待复核） |
|---:|---|---|---|---|
| 139 | `ros2 launch slam slam.launch.py` | ros2 launch | `slam / slam.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 142 | `ros2 launch slam rviz_slam.launch.py` | ros2 launch | `slam / rviz_slam.launch.py` | —; `src/slam/launch/rviz_slam.launch.py` |
| 145 | `ros2 launch peripherals teleop_key_control.launch.py` | ros2 launch | `peripherals / teleop_key_control.launch.py` | peripherals:teleop_key_control; src/peripherals/peripherals/teleop_key_control.py |
| 149 | `cd ~/ros2_ws/src/slam/maps && ros2 run nav2_map_server map_saver_cli -f "map_01" --ros-args -p map_subscribe_transient_local:=true` | cd（含脚本调用） | `—` | —; — |
| 155 | `ros2 launch slam slam.launch.py slam_method:=cartographer` | ros2 launch | `slam / slam.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 157 | `ros2 launch slam slam.launch.py slam_method:=gmapping` | ros2 launch | `slam / slam.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |

### 自主导航（2D）

| 行号 | 完整原始命令 | 类型 | 直接端点/文件 | V1.1节点/源码线索（待复核） |
|---:|---|---|---|---|
| 166 | `ros2 launch navigation rviz_navigation.launch.py` | ros2 launch | `navigation / rviz_navigation.launch.py` | —; `src/navigation/launch/rviz_navigation.launch.py` |
| 168 | `ros2 launch navigation navigation.launch.py map:=地图名称` | ros2 launch | `navigation / navigation.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |

### SLAM/导航（3D）

| 行号 | 完整原始命令 | 类型 | 直接端点/文件 | V1.1节点/源码线索（待复核） |
|---:|---|---|---|---|
| 174 | `ros2 launch slam rtabmap_slam.launch.py` | ros2 launch | `slam / rtabmap_slam.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 177 | `ros2 launch slam rviz_rtabmap.launch.py` | ros2 launch | `slam / rviz_rtabmap.launch.py` | —; `src/slam/launch/rviz_rtabmap.launch.py` |
| 180 | `ros2 launch peripherals teleop_key_control.launch.py` | ros2 launch | `peripherals / teleop_key_control.launch.py` | peripherals:teleop_key_control; src/peripherals/peripherals/teleop_key_control.py |
| 186 | `ros2 launch navigation rtabmap_navigation.launch.py` | ros2 launch | `navigation / rtabmap_navigation.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 189 | `ros2 launch navigation rviz_rtabmap_navigation.launch.py` | ros2 launch | `navigation / rviz_rtabmap_navigation.launch.py` | —; `src/navigation/launch/rviz_rtabmap_navigation.launch.py` |

### 仿真与模型显示

| 行号 | 完整原始命令 | 类型 | 直接端点/文件 | V1.1节点/源码线索（待复核） |
|---:|---|---|---|---|
| 195 | `ros2 launch rosorin_description display.launch.py` | ros2 launch | `rosorin_description / display.launch.py` | joint_state_publisher:joint_state_publisher, robot_state_publisher:robot_state_publisher; `src/simulations/rosorin_description/launch/display.launch.p |

### 视觉应用（2D/3D）

| 行号 | 完整原始命令 | 类型 | 直接端点/文件 | V1.1节点/源码线索（待复核） |
|---:|---|---|---|---|
| 201 | `ros2 launch example hand_trajectory_node.launch.py debug:=true` | ros2 launch | `example / hand_trajectory_node.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 202 | `ros2 service call /hand_trajectory/start std_srvs/srv/Trigger {}` | ros2 service call | `/hand_trajectory/start` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 205 | `ros2 launch example hand_gesture_control_node.launch.py` | ros2 launch | `example / hand_gesture_control_node.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 212 | `ros2 launch example prevent_falling.launch.py debug:=true` | ros2 launch | `example / prevent_falling.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 214 | `ros2 launch example cross_bridge.launch.py debug:=true` | ros2 launch | `example / cross_bridge.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |

### 语音交互

| 行号 | 完整原始命令 | 类型 | 直接端点/文件 | V1.1节点/源码线索（待复核） |
|---:|---|---|---|---|
| 220 | `ros2 launch xf_mic_asr_offline mic_init.launch.py` | ros2 launch | `xf_mic_asr_offline / mic_init.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 223 | `ros2 launch xf_mic_asr_offline mic_init.launch.py enable_setting:=true` | ros2 launch | `xf_mic_asr_offline / mic_init.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 226 | `ros2 launch xf_mic_asr_offline voice_control_move.launch.py` | ros2 launch | `xf_mic_asr_offline / voice_control_move.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 229 | `ros2 launch xf_mic_asr_offline voice_control_color_detect.launch.py` | ros2 launch | `xf_mic_asr_offline / voice_control_color_detect.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 232 | `ros2 launch xf_mic_asr_offline voice_control_navigation.launch.py map:=map_01` | ros2 launch | `xf_mic_asr_offline / voice_control_navigation.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |

### 上位机工具

| 行号 | 完整原始命令 | 类型 | 直接端点/文件 | V1.1节点/源码线索（待复核） |
|---:|---|---|---|---|
| 238 | `ros2 launch peripherals depth_camera.launch.py` | ros2 launch | `peripherals / depth_camera.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 241 | `python3 ~/software/lab_tool/main.py` | python3 | `—` | —; — |
| 244 | `python3 ~/software/servo_tool/main.py` | python3 | `—` | —; — |

### 大模型应用（large_models_examples）

| 行号 | 完整原始命令 | 类型 | 直接端点/文件 | V1.1节点/源码线索（待复核） |
|---:|---|---|---|---|
| 251 | `ros2 launch large_models_examples llm_control_move.launch.py` | ros2 launch | `large_models_examples / llm_control_move.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 257 | `ros2 launch large_models_examples llm_color_track.launch.py` | ros2 launch | `large_models_examples / llm_color_track.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 261 | `ros2 launch large_models_examples llm_visual_patrol.launch.py` | ros2 launch | `large_models_examples / llm_visual_patrol.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 266 | `ros2 launch large_models_examples vllm_with_camera.launch.py` | ros2 launch | `large_models_examples / vllm_with_camera.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 270 | `ros2 launch large_models_examples vllm_track.launch.py` | ros2 launch | `large_models_examples / vllm_track.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 275 | `ros2 launch large_models_examples vllm_navigation.launch.py map:=map_01` | ros2 launch | `large_models_examples / vllm_navigation.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 283 | `ros2 launch large_models_examples llm_control_progress.launch.py` | ros2 launch | `large_models_examples / llm_control_progress.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 287 | `ros2 launch large_models_examples llm_control_progress.launch.py function:=navigation` | ros2 launch | `large_models_examples / llm_control_progress.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 292 | `ros2 launch large_models_examples vllm_track_arm.launch.py` | ros2 launch | `large_models_examples / vllm_track_arm.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 293 | `ros2 topic pub --once /vocal_detect/asr_result std_msgs/msg/String '{data: "追踪前面的风扇"}'` | ros2 topic pub | `/vocal_detect/asr_result` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 298 | `ros2 launch large_models_examples llm_control_progress.launch.py` | ros2 launch | `large_models_examples / llm_control_progress.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 299 | `ros2 topic pub --once /vocal_detect/asr_result std_msgs/msg/String '{data: "沿着黑线走，遇到障碍物就停下，接着向左转，最后给我描述一下你看到了什么"}'` | ros2 topic pub | `/vocal_detect/asr_result` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 305 | `ros2 launch large_models_examples llm_control_progress.launch.py` | ros2 launch | `large_models_examples / llm_control_progress.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 306 | `ros2 service call /llm_control_move/transport_mode large_models_msgs/srv/SetString "{data: 'pick'}"` | ros2 service call | `/llm_control_move/transport_mode` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 307 | `ros2 service call /llm_control_move/transport_mode large_models_msgs/srv/SetString "{data: 'place'}"` | ros2 service call | `/llm_control_move/transport_mode` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 309 | `ros2 launch large_models_examples llm_control_progress.launch.py function:=navigation` | ros2 launch | `large_models_examples / llm_control_progress.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 310 | `ros2 service call /llm_control_move/transport_mode large_models_msgs/srv/SetString "{data: 'pick'}"` | ros2 service call | `/llm_control_move/transport_mode` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 314 | `ros2 launch large_models_examples llm_object_sorting.launch.py` | ros2 launch | `large_models_examples / llm_object_sorting.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 315 | `ros2 topic pub --once /vocal_detect/asr_result std_msgs/msg/String '{data: "把蓝色的东西拿走"}'` | ros2 topic pub | `/vocal_detect/asr_result` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 320 | `ros2 launch large_models_examples waste_classification.launch.py start:=true` | ros2 launch | `large_models_examples / waste_classification.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 322 | `ros2 launch large_models_examples llm_waste_classification.launch.py` | ros2 launch | `large_models_examples / llm_waste_classification.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 323 | `ros2 topic pub --once /vocal_detect/asr_result std_msgs/msg/String '{data: "分拣其他垃圾"}'` | ros2 topic pub | `/vocal_detect/asr_result` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 328 | `ros2 launch large_models_examples object_transport_node.launch.py start:=true` | ros2 launch | `large_models_examples / object_transport_node.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 330 | `ros2 launch large_models_examples vllm_object_transport.launch.py` | ros2 launch | `large_models_examples / vllm_object_transport.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 331 | `ros2 topic pub --once /vocal_detect/asr_result std_msgs/msg/String '{data: "将红色方块夹起来、放到蓝色的盒子里"}'` | ros2 topic pub | `/vocal_detect/asr_result` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 336 | `cd /home/ubuntu/ros2_ws/src/large_models_examples/large_models_examples/road_network` | cd（含脚本调用） | `—` | —; — |
| 337 | `python3 road_network_builder.py` | python3 | `—` | —; — |
| 339 | `ros2 launch large_models_examples road_network.launch.py` | ros2 launch | `large_models_examples / road_network.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 341 | `ros2 topic pub /request_waypoint std_msgs/msg/Int32 "data: 1" --once` | ros2 topic pub | `/request_waypoint` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 347 | `ros2 launch large_models_examples road_network_tool.launch.py` | ros2 launch | `large_models_examples / road_network_tool.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 348 | `ros2 topic pub --once /vocal_detect/asr_result std_msgs/msg/String '{data: "去公园看看有什么"}'` | ros2 topic pub | `/vocal_detect/asr_result` | 仅运行时发布/调用；接收方节点需按本地源码核对 |

### OpenClaw 应用

| 行号 | 完整原始命令 | 类型 | 直接端点/文件 | V1.1节点/源码线索（待复核） |
|---:|---|---|---|---|
| 357 | `openclaw config` | openclaw CLI | `—` | —; — |
| 359 | `openclaw skills list --eligible` | openclaw CLI | `—` | —; — |
| 362 | `openclaw gateway run` | openclaw CLI | `—` | —; — |
| 365 | `openclaw tui` | openclaw CLI | `—` | —; — |
| 370 | `ros2 launch openclaw_controller robot_base_control.launch` | ros2 launch | `openclaw_controller / robot_base_control.launch` | —; — |
| 374 | `ros2 topic pub --once /claw_move_control/chassis_command std_msgs/msg/String "{data: 'forward 1, stop 0.5, backward 2'}"` | ros2 topic pub | `/claw_move_control/chassis_command` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 385 | `ros2 topic pub --once /claw_arm_group_control/arm_group_control std_msgs/msg/String "{data: 'voice_pick'}"` | ros2 topic pub | `/claw_arm_group_control/arm_group_control` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 395 | `ros2 launch openclaw_controller robot_base_control.launch.py` | ros2 launch | `openclaw_controller / robot_base_control.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 397 | `ros2 launch openclaw_controller robot_base_control.launch.py` | ros2 launch | `openclaw_controller / robot_base_control.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 399 | `ros2 service call /claw_track_and_grab/start std_srvs/srv/Trigger "{}"` | ros2 service call | `/claw_track_and_grab/start` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 401 | `ros2 service call /claw_track_and_grab/set_color interfaces/srv/SetString "{data: 'red'}"` | ros2 service call | `/claw_track_and_grab/set_color` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 403 | `ros2 service call /claw_track_and_grab/start_pick std_srvs/srv/Trigger "{}"` | ros2 service call | `/claw_track_and_grab/start_pick` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 405 | `ros2 service call /claw_track_and_grab/place std_srvs/srv/Trigger "{}"` | ros2 service call | `/claw_track_and_grab/place` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 407 | `ros2 service call /claw_track_and_grab/init_pose std_srvs/srv/Trigger "{}"` | ros2 service call | `/claw_track_and_grab/init_pose` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 409 | `ros2 service call /claw_track_and_grab/pick_status std_srvs/srv/Trigger "{}"` | ros2 service call | `/claw_track_and_grab/pick_status` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 416 | `ros2 launch openclaw_controller robot_base_control.launch.py  track_method:="obj_track"  object_track_debug:=true` | ros2 launch | `openclaw_controller / robot_base_control.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 418 | `ros2 service call /claw_track_and_grab/start std_srvs/srv/Trigger "{}"` | ros2 service call | `/claw_track_and_grab/start` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 422 | `ros2 service call /claw_track_and_grab/start_pick std_srvs/srv/Trigger "{}"` | ros2 service call | `/claw_track_and_grab/start_pick` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 424 | `ros2 service call /claw_track_and_grab/place std_srvs/srv/Trigger "{}"` | ros2 service call | `/claw_track_and_grab/place` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 426 | `ros2 service call /claw_track_and_grab/init_pose std_srvs/srv/Trigger "{}"` | ros2 service call | `/claw_track_and_grab/init_pose` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 428 | `ros2 service call /claw_track_and_grab/pick_status std_srvs/srv/Trigger "{}"` | ros2 service call | `/claw_track_and_grab/pick_status` | 仅运行时发布/调用；接收方节点需按本地源码核对 |

### 智慧场景（社区/工厂）

| 行号 | 完整原始命令 | 类型 | 直接端点/文件 | V1.1节点/源码线索（待复核） |
|---:|---|---|---|---|
| 432 | `ros2 launch openclaw_controller navigation_manager.launch.py` | ros2 launch | `openclaw_controller / navigation_manager.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 435 | `ros2 service call /claw_navigation_manager/list_positions std_srvs/srv/Trigger {}` | ros2 service call | `/claw_navigation_manager/list_positions` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 437 | `ros2 service call /claw_navigation_manager/set_pose interfaces/srv/SetString "{data: '动物园'}"` | ros2 service call | `/claw_navigation_manager/set_pose` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 444 | `ros2 launch openclaw_controller smart_scene_navigation.launch.py  navigation_mode:="smart_factory"` | ros2 launch | `openclaw_controller / smart_scene_navigation.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 449 | `ros2 launch openclaw_controller smart_scene_navigation.launch.py  navigation_mode:="smart_factory"  road_network_map:="road_network_factory"` | ros2 launch | `openclaw_controller / smart_scene_navigation.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 456 | `ros2 launch openclaw_controller smart_scene_navigation.launch.py  navigation_mode:="smart_community"` | ros2 launch | `openclaw_controller / smart_scene_navigation.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 460 | `ros2 service call /apriltag_control_node/set_debug_mode interfaces/srv/SetString "{data: pick_exp}"` | ros2 service call | `/apriltag_control_node/set_debug_mode` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 462 | `ros2 service call /apriltag_control_node/debug_pick_exp std_srvs/srv/Trigger "{}"` | ros2 service call | `/apriltag_control_node/debug_pick_exp` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 464 | `ros2 service call /apriltag_control_node/place_exp std_srvs/srv/Trigger "{}"` | ros2 service call | `/apriltag_control_node/place_exp` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 466 | `ros2 service call /apriltag_control_node/init_pose std_srvs/srv/Trigger "{}"` | ros2 service call | `/apriltag_control_node/init_pose` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 470 | `ros2 service call /apriltag_control_node/set_debug_mode interfaces/srv/SetString "{data: pick_basket}"` | ros2 service call | `/apriltag_control_node/set_debug_mode` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 472 | `ros2 service call /apriltag_control_node/debug_pick_basket std_srvs/srv/Trigger "{}"` | ros2 service call | `/apriltag_control_node/debug_pick_basket` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 474 | `ros2 service call /apriltag_control_node/debug_place_basket std_srvs/srv/Trigger "{}"` | ros2 service call | `/apriltag_control_node/debug_place_basket` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 476 | `ros2 service call /apriltag_control_node/init_pose std_srvs/srv/Trigger "{}"` | ros2 service call | `/apriltag_control_node/init_pose` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 481 | `ros2 service call /apriltag_control_node/pick_basket std_srvs/srv/Trigger "{}"` | ros2 service call | `/apriltag_control_node/pick_basket` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 483 | `ros2 service call /apriltag_control_node/place_basket std_srvs/srv/Trigger "{}"` | ros2 service call | `/apriltag_control_node/place_basket` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 486 | `ros2 service call /apriltag_control_node/pick_exp std_srvs/srv/Trigger "{}"` | ros2 service call | `/apriltag_control_node/pick_exp` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 488 | `ros2 service call /apriltag_control_node/place_exp std_srvs/srv/Trigger "{}"` | ros2 service call | `/apriltag_control_node/place_exp` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 491 | `ros2 service call /apriltag_control_node/clear_target std_srvs/srv/Trigger "{}"` | ros2 service call | `/apriltag_control_node/clear_target` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 494 | `ros2 launch openclaw_controller smart_scene_navigation.launch.py  navigation_mode:="smart_community" road_network_map:="road_network_community"` | ros2 launch | `openclaw_controller / smart_scene_navigation.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |

### 垃圾分类

| 行号 | 完整原始命令 | 类型 | 直接端点/文件 | V1.1节点/源码线索（待复核） |
|---:|---|---|---|---|
| 500 | `ros2 launch large_models_examples waste_classification.launch.py` | ros2 launch | `large_models_examples / waste_classification.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 502 | `ros2 service call /waste_classification/enter std_srvs/srv/Trigger {}` | ros2 service call | `/waste_classification/enter` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 506 | `ros2 service call /waste_classification/set_target interfaces/srv/SetStringList "{data: [residual_waste]}"` | ros2 service call | `/waste_classification/set_target` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 515 | `ros2 service call /waste_classification/enable_transport std_srvs/srv/SetBool "{data: True}"` | ros2 service call | `/waste_classification/enable_transport` | 仅运行时发布/调用；接收方节点需按本地源码核对 |

### 测试架（test_stand）

| 行号 | 完整原始命令 | 类型 | 直接端点/文件 | V1.1节点/源码线索（待复核） |
|---:|---|---|---|---|
| 527 | `ros2 launch test_stand llm_control_servo.launch.py` | ros2 launch | `test_stand / llm_control_servo.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 528 | `ros2 topic pub --once /vocal_detect/asr_result std_msgs/msg/String '{data: "1号舵机在1秒内转到45度"}'` | ros2 topic pub | `/vocal_detect/asr_result` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 529 | `ros2 service call /llm_control_servo/command large_models_msgs/srv/SetString "{data: '1号舵机在1秒内转到45度'}"` | ros2 service call | `/llm_control_servo/command` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 532 | `ros2 launch test_stand vllm_track_arm.launch.py` | ros2 launch | `test_stand / vllm_track_arm.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 533 | `ros2 topic pub --once /vocal_detect/asr_result std_msgs/msg/String '{data: "追踪前面红色的木块"}'` | ros2 topic pub | `/vocal_detect/asr_result` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 534 | `ros2 service call /vllm_track_arm/command large_models_msgs/srv/SetString "{data: '追踪前面红色的木块'}"` | ros2 service call | `/vllm_track_arm/command` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 537 | `ros2 launch test_stand vllm_with_camera.launch.py` | ros2 launch | `test_stand / vllm_with_camera.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 538 | `ros2 topic pub --once /vocal_detect/asr_result std_msgs/msg/String '{data: "描述下你看到了什么"}'` | ros2 topic pub | `/vocal_detect/asr_result` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 539 | `ros2 service call /vllm_with_camera/command large_models_msgs/srv/SetString '{data: "描述下你看到了什么"}'` | ros2 service call | `/vllm_with_camera/command` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 542 | `ros2 launch test_stand llm_color_sorting.launch.py` | ros2 launch | `test_stand / llm_color_sorting.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 544 | `ros2 service call /object_sorting/detect_box_position std_srvs/srv/Trigger {}` | ros2 service call | `/object_sorting/detect_box_position` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 546 | `ros2 service call /object_sorting/test_sort std_srvs/srv/Trigger {}` | ros2 service call | `/object_sorting/test_sort` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 547 | `ros2 topic pub --once /vocal_detect/asr_result std_msgs/msg/String '{data: "把红色木块放到红色盒子里面"}'` | ros2 topic pub | `/vocal_detect/asr_result` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 548 | `ros2 service call /llm_color_sorting/command large_models_msgs/srv/SetString '{data: "把红色木块放到红色盒子里面"}'` | ros2 service call | `/llm_color_sorting/command` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 552 | `ros2 launch test_stand vllm_object_transport.launch.py` | ros2 launch | `test_stand / vllm_object_transport.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 553 | `ros2 topic pub --once /vocal_detect/asr_result std_msgs/msg/String '{data: '把红色木块放到绿色木块上面'}'` | ros2 topic pub | `/vocal_detect/asr_result` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 554 | `ros2 service call /vllm_object_transport/command large_models_msgs/srv/SetString "{data: '把红色木块放到绿色木块上面'}"` | ros2 service call | `/vllm_object_transport/command` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 559 | `ros2 launch test_stand llm_control_servo.launch.py` | ros2 launch | `test_stand / llm_control_servo.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 563 | `ros2 launch test_stand vllm_track_arm.launch.py` | ros2 launch | `test_stand / vllm_track_arm.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 567 | `ros2 launch test_stand vllm_with_camera.launch.py` | ros2 launch | `test_stand / vllm_with_camera.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 571 | `ros2 launch test_stand llm_color_sorting.launch.py` | ros2 launch | `test_stand / llm_color_sorting.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 575 | `ros2 launch test_stand vllm_object_transport.launch.py` | ros2 launch | `test_stand / vllm_object_transport.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |

### 智慧物流

| 行号 | 完整原始命令 | 类型 | 直接端点/文件 | V1.1节点/源码线索（待复核） |
|---:|---|---|---|---|
| 583 | `ros2 service call /smart_logistics/calibrate_pick std_srvs/srv/Trigger` | ros2 service call | `/smart_logistics/calibrate_pick` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 584 | `ros2 service call /smart_logistics/calibrate_place std_srvs/srv/Trigger` | ros2 service call | `/smart_logistics/calibrate_place` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 586 | `ros2 service call /smart_logistics/set_color interfaces/srv/SetString "{data: 'red'}"` | ros2 service call | `/smart_logistics/set_color` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 588 | `ros2 service call /smart_logistics/pick std_srvs/srv/Trigger` | ros2 service call | `/smart_logistics/pick` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 589 | `ros2 service call /smart_logistics/place std_srvs/srv/Trigger` | ros2 service call | `/smart_logistics/place` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 591 | `ros2 service call /smart_logistics/init_pose std_srvs/srv/Trigger` | ros2 service call | `/smart_logistics/init_pose` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 593 | `ros2 service call /smart_logistics/get_arm_state std_srvs/srv/Trigger {}` | ros2 service call | `/smart_logistics/get_arm_state` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 595 | `ros2 service call /smart_logistics/count_color std_srvs/srv/Trigger {}` | ros2 service call | `/smart_logistics/count_color` | 仅运行时发布/调用；接收方节点需按本地源码核对 |
| 599 | `python3 /home/ubuntu/ros2_ws/src/large_models_examples/large_models_examples/road_network/road_network_builder.py road_network_logistics` | python3 | `—` | —; — |
| 601 | `ros2 launch openclaw_controller smart_scene_navigation.launch.py navigation_mode:=smart_logistics  road_network_map:="road_network_logistics"` | ros2 launch | `openclaw_controller / smart_scene_navigation.launch.py` | V1.1旧映射有截断，详情须重跑解析器 |
| 603 | `ros2 service call /claw_navigation_manager/set_pose interfaces/srv/SetString "{data: 'Point_A'}"` | ros2 service call | `/claw_navigation_manager/set_pose` | 仅运行时发布/调用；接收方节点需按本地源码核对 |

## 五、非可执行记录（不纳入181）

| 行号 | 类型 | 原始内容 |
|---:|---|---|
| 376 | 自然语言任务示例 | 前进1秒, 后退两秒 |
| 380 | 自然语言任务示例 | 看看前面有什么,然后告诉我 |
| 387 | 自然语言任务示例 | 拔个萝卜 |
| 388 | 自然语言任务示例 | 拿给我 |
| 412 | 自然语言任务示例 | 夹取一个红色的方块,然后放置到旁边的盘子里 |
| 440 | 自然语言任务示例 | 带我去动物园看看,那里面有哪些东西，总结好给我 |
| 446 | 自然语言任务示例 | 给我去看看生产线的货物是否充足,如果不充足给我从原料仓进行补货 |
| 507 | 终端回显/服务响应 | waiting for service to become available... |
| 508 | 终端回显/服务响应 | requester: making request: interfaces.srv.SetStringList_Request(data=['residual_waste']) |
| 510 | 终端回显/服务响应 | response: |
| 511 | 终端回显/服务响应 | interfaces.srv.SetStringList_Response(success=True, message='set target') |
| 516 | 终端回显/服务响应 | waiting for service to become available... |
| 517 | 终端回显/服务响应 | requester: making request: std_srvs.srv.SetBool_Request(data=True) |
| 519 | 终端回显/服务响应 | response: |
| 520 | 终端回显/服务响应 | std_srvs.srv.SetBool_Response(success=True, message='start') |

## 六、待Work/源码验证的事项

- 需要可访问的本地源码快照与其SHA-256，逐条核对V1.1的Launch、节点及setup.py映射；不能用不一致的GitHub快照替代本地源码。
- 对动态`IncludeLaunchDescription`手工核对，明确条件分支与实际生效配置。
- 脚本是否存在必须按所选基线逐条检查；本轮仅能确认命令原文中的路径，不得写“本地脚本缺失”。
- 此文档可作为索引和编辑底稿，但不表示运行或实机验证。
