# 源码复用指南

> 复用等级：**A 直接复用** / **B 改造后复用** / **C 仅参考** / **D 不要用**
> 判定依据：代码质量、耦合程度、是否绕开安全层、依赖是否可获取。

## 1. 按包

| 包 | 等级 | 理由 | 课题组落点 |
|---|---|---|---|
| `driver/ros_robot_controller` | **A** | 协议完整、注释清楚、与固件配套 | 直接作为底层驱动 |
| `driver/controller` | **B** | 运动学公式可用，odom 与多入口有问题 | 保留运动学，重写 odom 与入口 |
| `driver/servo_controller` | **A** | 舵机管理完整，限幅齐全 | 直接复用 |
| `driver/kinematics` | **B/D** | 接口清晰但核心是闭源 .so | 保留服务契约，换自研 IK |
| `driver/sdk` | **B** | 是通用工具（PID/图像），不是机器人 SDK | 按需取用 |
| `peripherals` | **A/B** | 驱动齐全；launch 有分支 bug | 复用驱动，重写 launch |
| `navigation` | **B** | 标准 Nav2，参数可作起点 | 改就绪判断 + 输出接仲裁器 |
| `slam` | **A/B** | 标准 SLAM 封装 | 直接用，替换 10s 硬等 |
| `app` | **C** | 直接抢底盘、参数写死 | 仅参考算法 |
| `bringup` | **C** | 厂商整套启动，含大量应用层 | 不直接用，参考依赖顺序 |
| `large_models` | **C** | 依赖外部 `speech` 包与服务 | 参考 Tools 解耦思路 |
| `large_models_examples` | **C/D** | `eval` 解析、无安全 | 只做演示参考 |
| `openclaw_controller` | **C** | 依赖外部 CLI 与场景配置 | 参考技能划分 |
| `simulations/rosorin_description` | **A** | 模型与 mesh 可用 | 仿真与可视化的基础 |
| `example` | **C** | 教学示例，风格不统一 | 查算法片段 |
| `multi` / `test_stand` / `calibration` | **B/C** | 特定场景工具 | 标定流程可参考 |

## 2. 按文件（高价值清单）

### 建议直接复用

| 文件 | 作用 |
|---|---|
| `driver/ros_robot_controller/ros_robot_controller/ros_robot_controller_sdk.py` | 串口协议 |
| `driver/ros_robot_controller/ros_robot_controller/ros_robot_controller_node.py` | 硬件桥 |
| `driver/servo_controller/servo_controller/controller_manager.py` | 舵机/关节管理 |
| `driver/servo_controller/servo_controller/servo_controller.py` | 位置限幅与下发 |
| `driver/controller/controller/mecanum.py`（改参数来源） | 麦轮公式 |
| `driver/controller/controller/ackermann.py`（改参数来源） | 阿克曼公式 |
| `peripherals/launch/include/*.launch.py` | 各雷达/相机驱动启动 |
| `simulations/rosorin_description/{urdf,meshes}` | 模型 |

### 建议改造

| 文件 | 改造点 |
|---|---|
| `driver/controller/controller/odom_publisher_node.py` | 去掉多入口；补 `linear_y` 积分；加指令超时；参数名去 `~` |
| `peripherals/launch/lidar.launch.py` | 修 MS200 分支；配置化滤波 |
| `navigation/launch/*.launch.py` | 10s 硬等改成就绪检查；输出接 `/lab/cmd_vel/nav` |
| `bringup/launch/bringup.launch.py` | 拆成"驱动子集"，去掉 app/rosbridge |

### 不要复用

| 文件 | 原因 |
|---|---|
| `large_models_examples/.../llm_control_move.py` | `eval()` 解析模型输出 + 直接发 cmd_vel |
| `app/app/object_tracking.py`、`line_following.py` | 直接抢底盘，限速写死 |
| `bringup/scripts/start_app_node.service` | 路径缺 `/`、无就绪判断 |
| `driver/kinematics/kinematics/*.so` | 闭源，不可维护（可保留二进制但要有替代方案） |

## 3. 复用时的四个硬约束

1. **不改原厂源码**：所有修改在课题组工作空间内重写或 wrap，保证可回退。
2. **单写者**：任何复用模块都不得直接发布 `/controller/cmd_vel`。
3. **依赖可获取**：`speech`、`ActionGroups`、`servo_config.yaml`、`~/software/*` 这些外部依赖，
   要么随代码入库，要么在文档里明确"缺失时降级"。
4. **证据分级**：复用前先确认本文档标注的证据等级，C/D 级结论必须实机验证。

## 4. 迁移顺序建议

```
1. 驱动层（ros_robot_controller + servo_controller）—— 低风险，先跑通
2. 运动学（mecanum/ackermann）+ 自研 odom —— 中风险，需标定
3. 传感器（雷达/相机）—— 低风险
4. 仲裁与安全（自研）—— 必须先于导航
5. 导航（Nav2 改造）—— 高风险，依赖 2/3/4
6. 机械臂（IK 替换）—— 高风险，依赖标定
7. Agent（自研）—— 最后
```
