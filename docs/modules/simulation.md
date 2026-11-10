# simulation · 仿真与模型

## 1. 交付内容

| 类型 | 数量 | 位置 |
|---|---|---|
| Xacro | 21 | `simulations/rosorin_description/urdf/` |
| URDF | 0（全部由 xacro 生成） | — |
| Launch | 3 | `display.launch.py`、`robot_description.launch.py`、`rviz.launch.py` |
| Mesh | 19 个 STL | `meshes/`（含 pro/arm、pro/gripper、pro/mecanum） |
| RViz 配置 | 1 | `rviz/view.rviz` |

`rosorin.xacro` 组合了底盘（mecanum/acker/differential 三选一）、机械臂、相机、材质与 Gazebo 插件。

## 2. 能做什么

```bash
ros2 launch rosorin_description display.launch.py
```

展开：`robot_state_publisher` + `joint_state_publisher` + `joint_state_publisher_gui` + `rviz2`。
用途：**看模型、拖滑块看关节**。

## 3. 源码包内没有 Gazebo（但厂家另有课程资源包）

| 期望 | ROS 2 源码包内 | 教程资源包 `simulations.zip` |
|---|---|---|
| Gazebo launch | ❌ | ✅ 8 个（`worlds.launch.py`、`spawn_model.launch.py`、`robot_gazebo.launch.py` 等） |
| world 文件 | ❌ | ✅ `worlds/empty.sdf`、`worlds/robocup_home.sdf` |
| 仿真传感器插件 | 仅 xacro 标签 | ✅ `urdf/*.gazebo.xacro`（lidar/camera/imu） |
| 仿真底盘/机械臂 | ❌ | 未解压验证（`wheel_base.urdf.xacro`、`rosorinpro.gazebo.xacro` 存在） |

**来源**：`1 教程资料/8 Gazebo仿真课程/资源文件/3 功能包/simulations.zip`（139 个条目，含 `robot_gazebo` 包），
**不在 `src_ros2-main.zip` 内**，本次未解压运行，装法与可用性未验证（D 级）。

`rosorin_description` 自己的 `rosorin.gazebo.xacro` 有 12 处 `gazebo` 标签，
但源码包内**没有任何 launch 把它加载进 Gazebo**。

## 3.1 结论修正

上一版写"完全没有 Gazebo 仿真"，**不准确**。准确表述是：

* `src_ros2-main.zip` 只有 URDF/RViz 模型展示；
* 厂家另发了一个 `robot_gazebo` 课程资源包，含 launch/world/插件，需单独索取与验证；
* 两者是否版本配套，无法从文件名判断。

## 4. 关节状态的两条来源

`robot_description.launch.py`：

```python
joint_state_publisher(source_list=['controller_manager/joint_states'], rate=20)
joint_state_publisher_gui(remappings=[('/joint_states', 'joint_controller')])
```

* 实机：`controller_manager` 发布 `/controller_manager/joint_states`，被转成 TF；
* 调试：GUI 滑块直接发 `/joint_controller`，走 `controller_manager` 驱动真实舵机。

> 这也解释了为什么 `controller_manager` 的节点名必须是 `controller_manager`——
> 上游配置按这个名字找话题。

## 5. 与实机的差异

| 项 | 实机 | 模型 |
|---|---|---|
| 底盘尺寸 | 代码常量 0.17706/0.17165/0.08 | xacro 中的 mesh 与 joint 原点 |
| 轮子运动 | 无关节（麦轮直驱） | 模型里也没有轮子关节 |
| 机械臂 | 舵机脉宽 0–1000 | URDF 角度（rad） |
| 传感器 | 真实驱动 | 无 |
| `use_sim_time` | false | `robot_description.launch.py` 默认 **true** |

## 6. 复用建议

* 模型文件（xacro/mesh）可直接用于课题组 RViz 可视化；
* Gazebo 集成优先看教程资源包的 `robot_gazebo`，再决定是否自建；
* 不要把 `display.launch.py` 当作"仿真入口"——它只是模型查看器。

## 7. 结论

* `src_ros2-main.zip`：**URDF/RViz 模型展示**，不是可运行仿真；
* 教程资源包：**存在 `robot_gazebo` 仿真包**，但未验证；
* 若 08 窗口要做仿真回归，应从 `simulations.zip` 起步，并核实其与本地快照的版本匹配。
