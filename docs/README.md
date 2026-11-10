# ROSOrin Lab｜原厂源码分析资料库 V1.2

> **资料纠错稿，尚未通过完整技术验收。** 本资料主要基于历史静态分析；完整本地源码快照未在本轮逐文件复核，未进行编译、仿真或实机测试。源码差异、结论和运行时行为均需按各文档所列证据边界进一步核实。不要将这里的内容视为已验证的运行手册。

分析基线为本地交付的 `src_ros2-main.zip` 历史索引；厂商 GitHub 的 `orin-nx` 固定提交 `6c2a621fe628cc4928c48fc11d8d982594d471eb` 是对照版本，两者并非同一快照。见[资料包说明与证据边界](provenance/package_README.md)和[来源清单](provenance/evidence/SOURCE_MANIFEST.md)。

## 导航

- 源码索引：[完整目录](source-index/complete_source_tree.md) · [文件功能映射](source-index/file_function_mapping.csv) · [GitHub 链接表](source-index/github_links.csv)
- [命令索引](command/command_index.md)：181 条可执行行，另有 15 条非命令记录；命令未上机运行。
- 架构：[系统总览](architecture/system_overview.md) · [软件架构](architecture/software_architecture.md) · [模块关系](architecture/module_relationship.md) · [启动流程](architecture/startup_flow.md)
- 模块：[系统](modules/system.md) · [运动](modules/motion.md) · [硬件](modules/hardware.md) · [传感器](modules/sensors.md) · [感知](modules/perception.md) · [导航](modules/navigation.md) · [机械臂](modules/manipulation.md) · [AI](modules/ai.md) · [安全](modules/safety.md) · [仿真](modules/simulation.md)
- 硬件：[源码映射](hardware/hardware_source_mapping.md) · [STM32 分析](hardware/stm32_analysis.md)
- [源码复用指南](reuse/source_reuse_guide.md) · [已知问题](issues/known_issues.md)
- 修订与来源：[V1.2 修订记录](provenance/V1.2_REVISION_LOG.md) · [早期修订记录](provenance/REVISION_LOG.md) · [交接说明](provenance/WORK_HANDOFF.md) · [原始 command 记录](provenance/evidence/command_local_604_lines.txt)

原资料包各文件按内容原样保存，目录位置依交接说明调整；文中出现的原资料包相对路径用于说明原始结构，请以本导航为准。对代码安全、固件和实机行为的判断应保留原文限定，不应推断尚未检查的实现或默认状态。

## 待核实的路径

[启动流程图](architecture/startup_flow.md)中的 `source home/ubuntu/.zshrc` 缺少路径开头的 `/`，是否为笔误及实际启动配置待查原始服务文件；不要直接将其作为已验证的执行命令。
