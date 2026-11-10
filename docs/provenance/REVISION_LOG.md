# 修订记录（v1.0 → v1.1）

> 本次只做**定点核验与修正**，不重写已有有效内容，不重新遍历仓库生成报告。
> 所有修正均先重读源码，再改文档；每条给出"原结论 → 证据 → 新结论 → 改动文件"。

## 0. 修订方法

1. 重读 `odom_publisher_node.py` 全文（298 行）、`.typerc`、`command`、SDK 与 AI 关键文件；
2. 用脚本对 `command` 604 行逐行分类，重建命令→launch→节点→源码映射（含嵌套 include）；
3. 扫描**整个百度网盘交付目录**（不只是 ROS 2 压缩包）核对 STM32 资料；
4. 用 GitHub raw 接口逐文件比对本地快照与 `orin-nx@6c2a621`（78 个文件）；
5. 全仓库检索 DDS 相关配置，确认生效范围；
6. 对安全相关接口（急停/超时/限流/舵机保护）逐项检索并分层。

## 1. 底盘调用关系（问题 1）

| 项 | 内容 |
|---|---|
| 原结论 | `motion.md` §1 与两张架构图把 `/controller/cmd_vel`、`/app/cmd_vel`、`/cmd_vel` 都画成进入 `cmd_vel_callback`；`system_overview.md` 文字写"/app/cmd_vel 是阿克曼入口"，与图不一致 |
| 复核证据 | `odom_publisher_node.py:136` 订阅 `controller/cmd_vel` → `cmd_vel_callback`（L216）；`:137` 订阅 `app/cmd_vel` → `acker_cmd_vel_callback`（L200）；`:138` 订阅 `cmd_vel` → `app_cmd_vel_callback`（L185） |
| 新结论 | 三个入口对应**三个不同回调**；`/cmd_vel` 先限幅再转 `cmd_vel_callback`；`/app/cmd_vel` 先做阿克曼/舵机换算；`/app/cmd_vel` 纯旋转时**还会发 PWM 舵机 1 号** |
| 改动文件 | `04_module_analysis/motion.md`（§1 重写、新增 §3 回调细节）、`03_architecture/system_overview.md` §4.1、`03_architecture/software_architecture.md` §3.1、`03_architecture/module_relationship.md` §3 |
| 结果 | 文字、表格、三张图口径一致；已标注"函数名与话题名相反"的命名陷阱 |

## 2. AI 功能分类（问题 2）

| 项 | 内容 |
|---|---|
| 原结论 | `system_overview.md` §4.2 写"所有 AI 能力的落点都是直接发 cmd_vel" |
| 复核证据 | 扫描 `large_models`、`large_models_examples`、`openclaw_controller`、`xf_mic_asr_offline` 的发布者/客户端：核心包 **0 个 cmd_vel 发布者**；示例包 25 个带机器人接口；openclaw 3 个 Twist、4 个 Servo；语音包 3 个（move/arm/navigation） |
| 新结论 | 按接口分 7 类：直接底盘速度 / Nav2 / 机械臂 / 视觉跟踪 / function calling / 任务调度 / 语音；并给出"文本→工具调用→ROS 调用→硬件执行"四层证据链 |
| 改动文件 | `04_module_analysis/ai.md`（重写）、`03_architecture/system_overview.md` §4.2 |
| 结果 | 明确 `large_models` 只产出文本/工具调用；`eval()` 仅存在于简单 Demo；不再笼统归为"直接发 cmd_vel" |

## 3. command 统计与映射（问题 3）

| 项 | 内容 |
|---|---|
| 原结论 | 604 行、196 条有效记录、79 条 `ros2 launch` |
| 复核证据 | 逐行分类脚本 + 重新解析 launch（含嵌套 include、`EXE_SRC` 入口点映射） |
| 新结论 | 604 行 = 186 注释 + 155 空行 + 50 分隔线 + 17 樱花 + 8 终端回显 + 7 自然语言 + **181 可执行行**；`ros2 launch` **80** 条（79 条解析到文件，1 条 `robot_base_control.launch` 缺 `.py`）；75 条能解析出节点，67 条能落到本地源码；8 种 include 形式无法继续追踪 |
| 改动文件 | `02_command_mapping/command_index.md`（重生成）、`07_issues/known_issues.md` #17 |
| 结果 | 统计口径写进文档开头；新增"追踪质量"表与"无法继续追踪"清单；明确"找到 launch ≠ 找到源码" |

## 4. STM32 资料（问题 4）

| 项 | 内容 |
|---|---|
| 原结论 | "厂家只交付了 HEX 固件，没有交付可编译的 STM32 工程"（仅依据 ROS 2 压缩包） |
| 复核证据 | 扫描 `ROSOrin Pro智能小车(Orin NX)\` 全目录：`.c/.cpp/.h/.hpp` **0 个**、Keil/CubeMX 工程 **0 个**、HEX **1 个**；`4 硬件资料/` 下 STM32 资料是 0.4 KB 的网盘链接（提取码 `vy7s`），未下载 |
| 新结论 | 本地已下载目录**确实没有** STM32 工程；但厂家资料入口存在且未获取，**不能断定厂家不提供**；结论改为"待下载核实"，HEX 分析结论保留 |
| 改动文件 | `05_hardware_mapping/stm32_analysis.md`（重写）、`README.md` §7、`07_issues/known_issues.md` #6 |
| 结果 | 区分"本地缺失 / 网盘未下载 / 固件可证结构"三种状态，不再越界 |

## 5. DDS 结论（问题 5）

| 项 | 内容 |
|---|---|
| 原结论 | `.typerc` 中 `RMW_IMPLEMENTATION` 被注释 → "实际为 rmw_fastrtps"（把默认推断当事实） |
| 复核证据 | 全仓库检索 `RMW_IMPLEMENTATION|CYCLONEDDS|rmw_`：只有 `.typerc:38`（注释）与 `:41`（`CYCLONEDDS_URI`）两处，`.robotrc` 无；镜像级配置不可见 |
| 新结论 | 仓库**没有启用** CycloneDDS；实际 RMW 为 **D 级待验证**；`CYCLONEDDS_URI` 仅在选 CycloneDDS 时才被读取 |
| 改动文件 | `04_module_analysis/system.md`（新增 §2.1）、`03_architecture/system_overview.md` §6、`README.md` §4、`07_issues/known_issues.md` #21 |
| 结果 | 全部文档统一为"待验证"，给出上机确认命令 |

## 6. 安全结论（问题 6）

| 项 | 内容 |
|---|---|
| 原结论 | "原厂没有安全层。没有急停……"（把 ROS 层缺失扩大为整机结论） |
| 复核证据 | ROS 层检索无 estop/超时/仲裁；SDK 有 `bus_servo_set_{angle,vin,temp}_limit`、`set_battery_level`，但业务层只透传、`set_battery_level` 无调用；硬件资料无急停/电源文档 |
| 新结论 | 分三层：Jetson 层无软件急停/超时/仲裁；STM32 层有保护**配置接口**但默认状态未知；硬件层有无物理急停**无法判断** |
| 改动文件 | `04_module_analysis/safety.md`（重写）、`03_architecture/system_overview.md` §5、`07_issues/known_issues.md` #3 |
| 结果 | 建议新增的安全模块单列，不再混入"原厂功能" |

## 7. GitHub 链接与版本（问题 7）

### 7.1 仓库状态

* 仓库 `Hiwonder/ROSOrin-Pro` 可访问；`main` 分支 **404**（不存在），可访问分支为 `orin-nx`；
* commit `6c2a621fe628cc4928c48fc11d8d982594d471eb` 存在：2026-04-15，message `inital commit`，
  父提交 `2c2912e`；该 commit 本身只删除了两个 `.gitignore`。

### 7.2 版本比对（78 个文件，raw 逐字节）

| 结果 | 数量 | 说明 |
|---|---|---|
| 一致 | 58 | 含 `mecanum.py`、`odom_publisher_node.py`、`bringup.launch.py`、`lidar.launch.py`、`navigation.launch.py` |
| 不同 | 12 | `.typerc`、`command`、`ros_robot_controller_sdk.py`、`agent_process.py`、`function_calling/llm_control.py` 等 |
| 仓库中不存在 | 8 | `.gitignore`、`.gitmodules` 及 5 个 launch（`smart_scene_navigation`、`test_stand` 等） |

差异明细：

| 文件 | 本地 | GitHub |
|---|---|---|
| `.typerc` | VERSION `2026-06-01`；`ROS_DOMAIN_ID=113`；`ASR_LANGUAGE=English` | `2026-02-02`；`56`；`Chinese` |
| `command` | 604 行 | 310 行 |
| SDK / agent_process / llm_control | 本地版 | 内容不同 |

### 7.3 采用版本

**本资料包以本地交付包为准**（机器人出厂镜像配套的是这一版）。
GitHub 仅用于提供可点击链接；`github_links.csv` 的 `内容比对` 列标明每个文件是否与本地一致。

### 7.4 改动文件

* 新增 `01_source_index/github_links.csv`（78 条，固定到 commit，含 `same/differs/unreachable` 与备注）；
* `README.md` 新增 §4 版本基线、§5 GitHub 链接说明；
* `07_issues/known_issues.md` 新增 #16（版本不一致）。

## 8. 额外质量检查（十项）

| # | 检查项 | 结果 |
|---|---|---|
| 1 | 998 文件 / 25 包 | 准确；998 = 993 个 `src/` 文件 + 5 个根文件 |
| 2 | A/B/C/D 统计 | 24 / 363 / 281 / 330 = 998，一致 |
| 3 | A 级 24 的构成 | 13 Python + 5 launch + 5 根配置 + 1 systemd；**不是 24 个 Python**，README 已写明 |
| 4 | 998 条是否重复 | CSV 路径 unique = 998，无重复 |
| 5 | CSV 路径是否真实 | 998 条全部存在（脚本校验 missing=0） |
| 6 | Mermaid 与源码一致 | 已修正底盘图；其余 7 张按源码连接绘制，未逐张实机验证 |
| 7 | 模块文档矛盾 | 已统一 DDS / 急停 / Gazebo / 版本四处；其余抽查一致 |
| 8 | 第三方误标 | 讯飞头文件、`pid.py`、模型文件均标"第三方"；`driver/sdk` 已注明非 STM32 SDK |
| 9 | 闭源库标记 | 两个运动学 `.so` 标"直接复用（不可修改）"，未写成可改源码 |
| 10 | 待核实是否写成确认 | DDS、物理急停、电池单位、Gazebo 均标 C/D |

## 9. 验收表

| 问题 | 是否修正 | 源码证据 | 修改文件 | 剩余问题 |
|---|---|---|---|---|
| 1. 底盘调用关系 | ✅ | `odom_publisher_node.py:136-138,185,200,216` | motion.md、system_overview.md、software_architecture.md、module_relationship.md | `/app/cmd_vel` 是否真被使用需实机确认 |
| 2. AI 功能分类 | ✅ | `agent_process.py:336-390`；`function_calling/llm_control.py:461-521`；各 Demo 发布者扫描 | ai.md、system_overview.md | 工具集中"预定义地点"表未逐一列出 |
| 3. command 统计 | ✅ | `command` 604 行逐行分类；80 条 launch | command_index.md、known_issues.md | 8 种动态 include 无法静态解析 |
| 4. STM32 资料 | ✅（修正结论范围） | 全目录扩展名扫描；`4 硬件资料` 链接 | stm32_analysis.md、README.md、known_issues.md | 网盘资料未下载，源码是否存在待核 |
| 5. DDS 结论 | ✅ | `.typerc:38,41`；全仓库无其他 RMW 配置 | system.md、system_overview.md、README.md、known_issues.md | 实际 RMW 需实机 `echo` |
| 6. 安全结论 | ✅（分层） | ROS 层检索；SDK 保护接口调用点 | safety.md、system_overview.md、known_issues.md | 物理急停、STM32 看门狗未知 |
| 7. GitHub 链接/版本 | ✅ | 78 文件 raw 比对；commit 元数据 | github_links.csv、README.md、known_issues.md | 998 个文件未逐一建链 |

## 10. 剩余工作（交给后续）

1. 按 `vy7s` / `pfvg` 提取码下载 STM32 与扩展板资料，补 `stm32_analysis.md`；
2. 实机确认 `RMW_IMPLEMENTATION`、`/dev/rrc`、电池单位、物理急停；
3. 对 `command` 中 8 种动态 include 手工追一次（nav2/slam/语音包）；
4. 若以 GitHub 版本为交付基线，需按 GitHub 重算 604→310 行等统计；
5. Gazebo 课程包解压验证，补 `simulation.md` 的实机/仿真对照。
