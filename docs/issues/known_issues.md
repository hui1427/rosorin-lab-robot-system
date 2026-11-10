# 已知问题清单（修订版）

> 严重度：**P0** 影响安全或不可运行 / **P1** 功能缺陷或维护陷阱 / **P2** 整洁性与文档问题
> 证据等级：A 直接源码 / B 多源交叉 / C 推断 / D 未核实
> 本次修订：急停结论降级为分层表述；DDS 结论改为"待验证"；Gazebo 结论修正；新增版本不一致与命令笔误两项。

## 汇总表

| # | 级别 | 问题 | 证据位置 | 等级 |
|---|---|---|---|---|
| 1 | P0 | 三个 cmd_vel 入口无仲裁，最后一条生效 | `odom_publisher_node.py:136-138` | A |
| 2 | P0 | 无指令超时 | 全仓库无相关逻辑 | A（缺失） |
| 3 | P1 | Jetson 层无软件急停；硬件急停未知 | 检索无 estop 节点/话题 | A / D |
| 4 | P0 | 大模型 Demo 用 `eval()` 执行模型输出 | `llm_control_move.py:169` | A |
| 5 | P0 | 机械臂正逆解闭源 | `kinematics/kinematics/*.so` | A |
| 6 | P0 | 关键外部资源不在仓库（speech、ActionGroups 等） | 见 §外部依赖 | A |
| 7 | P1 | 里程计只积分纵向速度 | `odom_publisher_node.py:266-272` | A |
| 8 | P1 | `need_compile=True` 时舵机 launch 崩溃 | `servo_controller.launch.py` 未 import | A |
| 9 | P1 | 雷达 MS200 分支加载 LD19 驱动 | `lidar.launch.py:28-30` | A |
| 10 | P1 | `/dev/rrc` udev 规则语法错误 | `peripherals/scripts/99-ttyACM0.rules` 末行 | A |
| 11 | P1 | 电池单位未定 | `sdk.py:179` 注释矛盾；`0x2904` 暗示 mV | B |
| 12 | P1 | 标定服务参数名带 `~` | `odom_publisher_node.py:153-156` | C |
| 13 | P1 | 开机自动初始化机械臂 | `bringup.launch.py:52-59` | A |
| 14 | P1 | 启动无就绪判定（硬等 50s / 10s） | `startup_check.py:54`、`navigation.launch.py:60` | A |
| 15 | P1 | `/app/cmd_vel` 纯旋转会触发 PWM 舵机 | `odom_publisher_node.py:200-209` | A |
| 16 | **P0** | **本地快照与 GitHub 不是同一版本** | `REVISION_LOG.md` §7 | A |
| 17 | P1 | `command:370` 缺 `.py`，按原文执行会失败 | `command:370` | A |
| 18 | P2 | 重复文件（aurora copy、rrt_explorer copy、两份 udev） | 源码树 | A |
| 19 | P2 | 包依赖声明缺失（只有 test_depend） | `peripherals/package.xml` 等 | A |
| 20 | P2 | launch 统计口径不一（85/138/152） | `README.md` §8 | A |
| 21 | P2 | DDS 实际配置未知（RMW 被注释） | `.typerc:38,41` | D |
| 22 | P2 | systemd 单元路径缺前导 `/` | `start_app_node.service` | A |
| 23 | P2 | 源码包内无 Gazebo（另有课程资源包） | `simulation.md` | B |
| 24 | P2 | 大模型子模块指向内网 | `.gitmodules` | A |
| 25 | P2 | `driver/sdk` 命名误导 | 包内是通用工具 | A |

## P0 详解

### 1. 三个入口无仲裁

`/controller/cmd_vel` → `cmd_vel_callback`（L216）、`/app/cmd_vel` → `acker_cmd_vel_callback`（L200）、
`/cmd_vel` → `app_cmd_vel_callback`（L185），三个回调都会覆盖 `self.linear_x/linear_y/angular_z`，
没有优先级、没有互斥、没有"当前控制者"状态。

### 2. 无指令超时

`cmd_vel_callback` 收到消息就 publish `MotorsState`，不检查距上一条的时间。
上游崩溃后是否停车，取决于 STM32 是否有内部超时（未知）。

### 4. `eval()` 执行模型输出

`llm_control_move.py:169` 对 LLM 返回文本做切片后 `eval`，再逐条发 Twist。
应改为 JSON 解析 + schema 校验 + 限幅。

### 5. 运动学闭源

`forward_kinematics.so` / `inverse_kinematics.so` 无源码，无法审计与移植。
服务接口（`kinematics_msgs`）可复用，实现需自研或接受黑盒。

### 6. 外部依赖清单

| 依赖 | 位置 | 缺失后果 |
|---|---|---|
| `speech` pip 包 | `from speech import speech` | 语音/大模型节点无法启动 |
| `ActionGroups` | `/home/ubuntu/software/arm_pc/ActionGroups` | `init_pose` 动作组失败 |
| `servo_config.yaml` | `/home/ubuntu/software/Servo_upper_computer/` | 舵机偏差不生效（仅日志） |
| `lab_tool` / `servo_tool` | `~/software/...` | 上位机工具不可用 |
| 6 个 third_party 工作空间 | `~/third_party/*` | 雷达/相机/SLAM 依赖缺失 |

### 16. 版本不一致（新增）

本地 `src_ros2-main.zip` 与 GitHub `orin-nx@6c2a621` 抽样比对 78 个文件：
**58 相同、12 不同、8 在仓库中不存在**。已确认差异：

| 文件 | 本地 | GitHub |
|---|---|---|
| `.typerc` | VERSION 2026-06-01，DOMAIN 113，ASR English | 2026-02-02，DOMAIN 56，ASR Chinese |
| `command` | 604 行 | 310 行 |
| `ros_robot_controller_sdk.py` | 本地版 | 内容不同 |
| `agent_process.py` | 本地版 | 内容不同 |
| `function_calling/llm_control.py` | 本地版 | 内容不同 |

**所有统计与行号以本地快照为准**；GitHub 链接仅作参考。

## 其余问题（简表）

| # | 说明 |
|---|---|
| 3 | 检索 `estop/emergency/急停` 无 ROS 节点/话题/服务；`~/button` 未映射为急停；物理急停是否存在无资料 |
| 7 | `delta_x/delta_y` 只用 `linear_x`，横移不产生位移估计 |
| 8 | `servo_controller.launch.py` 的 `need_compile=='True'` 分支调用未导入的函数 |
| 9 | `MS200` 实际加载 `ldlidar_LD19.launch.py` |
| 10 | 末行 `ENV{ID_MM_PORT_IGNORE}="1"1"`，多出 `1"` |
| 11 | 注释写"电压，单位 mAh"自相矛盾；`set_battery_level(0x2904=10500)` 暗示 mV |
| 12 | `get_parameter('~linear_correction_factor')` 与声明名不一致（未实机验证） |
| 13 | `bringup` 默认拉起 `init_pose`，上电机械臂会动 |
| 14 | `startup_check` 硬睡 50 s；导航/SLAM `TimerAction(period=10.0)` |
| 15 | 麦轮机型收到 `/app/cmd_vel` 纯旋转也会发 PWM 舵机 1 号 |
| 17 | `command:370` 写 `robot_base_control.launch`，实际文件是 `.launch.py` |
| 18 | `aurora930.launch copy.py`、`rrt_explorer.launch copy.py`、两份 `99-ttyACM0.rules` |
| 19 | 多个 `package.xml` 只有 `test_depend`，`rosdep` 与构建顺序失效 |
| 20 | `*.launch.py` 152；各包 `launch/` 83；含示例 137 |
| 21 | RMW 行被注释，实际 DDS 需实机确认 |
| 22 | `source home/ubuntu/.zshrc` 少 `/`，依赖 cwd=`/` |
| 23 | 源码包只有 URDF/RViz；Gazebo 在 `simulations.zip`（未验证） |
| 24 | `http://192.168.11.206:3000/...`，外网不可达 |
| 25 | `sdk` 包是 `common/pid/led/fps` 工具集，STM32 SDK 在 `ros_robot_controller` 包内 |
