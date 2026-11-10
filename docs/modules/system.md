# system · 系统启动与环境管理

## 1. 文件清单

| 文件 | 作用 | 证据 |
|---|---|---|
| `.robotrc` | Shell 环境入口：加载 `.typerc`、CUDA、7 个工作空间 | A |
| `.typerc` | 单点配置：机型/雷达/相机/麦克风/domain/need_compile | A |
| `.gitmodules` | 唯一子模块 `large_models` → 内网 Git | A |
| `command` | 604 行命令手册 | A |
| `src/bringup/launch/bringup.launch.py` | 总启动 | A |
| `src/bringup/bringup/startup_check.py` | 开机自检（50s 延迟） | A |
| `src/bringup/scripts/start_app_node.service` | systemd 单元 | A |
| `src/bringup/scripts/{slam,navigation}.sh/.desktop` | 桌面快捷入口 | B |

## 2. 环境变量（`.typerc`）

| 变量 | 值 | 谁在用 |
|---|---|---|
| `MACHINE_TYPE` | `ROSOrin_Mecanum_Pro` | `odom_publisher_node.py:94`、`lidar.launch.py:14`、`controller_manager.py:22` |
| `LIDAR_TYPE` | `SCLIDAR` | `lidar.launch.py:13` |
| `DEPTH_CAMERA_TYPE` | `aurora` | `depth_camera.launch.py:12` |
| `MIC_TYPE` / `ASR_LANGUAGE` / `ASR_MODE` | `WonderEchoPro` / `English` / `online` | 语音与大模型节点 |
| `ROS_DOMAIN_ID` | `113`（**本地快照**；GitHub 版为 `56`） | ROS 2 |
| `need_compile` | `False` | 所有 launch 的路径分支 |
| `CYCLONEDDS_URI` | `file:///etc/cyclonedds/config.xml` | 仅在 RMW 选 CycloneDDS 时被读取 |
| `RMW_IMPLEMENTATION` | **该行被注释** | 实际值**未知**，需实机 `echo` |

## 2.1 DDS 配置：只能写到"配置意图"，不能写"运行状态"

`.typerc` 里与 DDS 有关的只有两行：

| 行号 | 内容 | 状态 |
|---|---|---|
| 38 | `#export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp` | **注释，不生效** |
| 41 | `export CYCLONEDDS_URI=file:///etc/cyclonedds/config.xml` | 已导出，但只有 RMW 为 CycloneDDS 时才被读取 |

全仓库检索 `RMW_IMPLEMENTATION|CYCLONEDDS|rmw_`，**只有这两处**（`.robotrc` 中无）。

因此可以确定的只有：

1. 已检查的配置未明确设置为 CycloneDDS（不能代表运行时实际RMW）；
2. 若镜像层（`/etc/environment`、systemd 环境、`~/.zshrc` 之外的脚本）设置了 `RMW_IMPLEMENTATION`，本仓库看不出来；
3. 所以"机器人实际用哪个 DDS" **属于待验证（D）**，不能推断为 FastRTPS，也不能推断为 CycloneDDS。

上机确认命令：

```bash
echo $RMW_IMPLEMENTATION
ros2 doctor --report | grep -i rmw
ls -l /etc/cyclonedds/config.xml
```

## 3. 工作空间叠加

`.robotrc` 顺序（后者优先）：

```
/opt/ros/humble
~/ros2_ws/install
~/third_party/third_party_ws/install
~/third_party/orbbec_ws/install
~/third_party/rtabmap_ws/install
~/third_party/gmapping_ws/install
~/third_party/aurora_ws/install
```

| 工作空间 | 提供什么 | 是否在源码包内 |
|---|---|---|
| `ros2_ws` | 25 个功能包 | ✅ |
| `third_party_ws` | 通用第三方（laser_filters、robot_localization 等） | ❌ 需镜像自带 |
| `orbbec_ws` | 奥比中光相机 | ❌ |
| `rtabmap_ws` / `gmapping_ws` | RTAB-Map / GMapping | ❌ |
| `aurora_ws` | Aurora 深度相机驱动 | ❌ |

**结论**：源码包只覆盖 `ros2_ws`；其余 6 个工作空间全部依赖出厂镜像。换机/重装必须另行准备。

## 4. `need_compile` 的真实含义

```python
compiled = os.environ['need_compile']
if compiled == 'True':
    path = get_package_share_directory('controller')
else:
    path = '/home/ubuntu/ros2_ws/src/driver/controller'
```

* 出厂默认 `False` → **直接从源码树加载 launch**，`install/` 可能与实际行为不一致。
* 改成 `True` 会踩两个坑：
  1. `servo_controller.launch.py` 在该分支调用 `get_package_share_directory()` 但**没有 import** → `NameError`；
  2. `odom_publisher.launch.py` 等文件在 `False` 分支里才写死参数路径，`True` 分支未必被验证过。

> 二次开发建议：保持 `need_compile=False`，并明确"运行的是源码树"。

## 5. `command` 的定位

604 行命令手册，既有分类共196条记录，其中181条可执行行、7条自然语言示例、8条终端回显（本轮基于原始command复核）。它不是脚本，是**人工复制的命令集合**：

* 79 条 `ros2 launch` 全部能对应到真实 launch 文件（见 `02_command_mapping/command_index.md`）；
* 部分命令依赖外部资源（`~/software/lab_tool/main.py`、`Servo_upper_computer/servo_config.yaml`、`ActionGroups`），这些**不在源码包内**；
* 命令之间有隐含前置条件（先 `calibration` 标定、先 `mic_init` 初始化），文件里没有说明。

## 6. 与原厂交付物的一致性风险

| 风险 | 说明 |
|---|---|
| 快照不一致 | 本地包名 `src_ros2-main.zip`，与 `orin-nx@6c2a621` 未必同一 commit |
| 子模块 | `large_models` 指向 `192.168.11.206:3000`，外网不可达；本地包内含 64 个文件但版本不可追溯 |
| 外部目录 | `/home/ubuntu/software/`（上位机、ActionGroups、servo_config.yaml）不在仓库 |
| pip 包 | `from speech import speech` 中的 `speech` 不在源码树 |

## 7. 复现建议

1. 先在一台机器上 `git rev-parse HEAD` 固定基线；
2. 导出 `pip3 freeze`、`dpkg -l | grep ros-humble`、`ls /home/ubuntu/software`；
3. 把 `third_party_ws` 等 6 个工作空间的 `install/` 清单存档（不必存二进制）；
4. 再谈"独立部署"。

### V1.2运行状态边界

`.typerc`中RMW设置是否注释仅可证明该文件内容；不能从缺少`RMW_IMPLEMENTATION`直接推出镜像中运行的是哪一种DDS/RMW。需在Jetson上检查环境、运行节点与实际RMW，未执行。
