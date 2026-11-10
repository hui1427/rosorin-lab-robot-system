# Work整合交接说明｜ROSOrin Lab 文档V1.2

## 任务边界

这是一份**带未验收标记的文档修订底稿**。请在GitHub上只做**文档整合**，不要移动/修改原厂源代码、构建/运行ROS包或实机控制。保留原分支`feature/official-source-library-v1`不覆盖，使用独立文档分支并提交可审查的PR；不直接修改main。

## 参考与版本

- 厂商对照：`Hiwonder/ROSOrin-Pro`、`orin-nx`、`6c2a621fe628cc4928c48fc11d8d982594d471eb`。
- 主分析基线：本地交付`src_ros2-main.zip`，据DeepSeek历史索引为998文件/25包。本轮未获得完整源文件复核，只直接读取了本地604行`command`。
- `00_evidence/SOURCE_MANIFEST.md`列明本轮可访问文件和哈希。不要根据老版本GitHub行号来宣称本地版本已验证。

## 推荐文档落点

- `01_source_index/*` → `docs/source-index/`
- `02_command_mapping/command_index.md` → `docs/command/command_index.md`
- `03_architecture/*` → `docs/architecture/`
- `04_module_analysis/*` → `docs/modules/`
- `05_hardware_mapping/*` → `docs/hardware/`
- `06_reuse_guide/*` → `docs/reuse/`
- `07_issues/*` → `docs/issues/`
- `V1.2_REVISION_LOG.md`、`00_evidence/SOURCE_MANIFEST.md` → `docs/provenance/`

## 检查重点

1. 根README形成系统、motion、hardware、sensors、perception、navigation、manipulation、ai等导航链接；逻辑目录只做索引，不复制或搬动原厂代码。
2. 保留原始厂商树/版本来源，清理已有中英文双份副本前先核对分支差异和文件归属，不要未经授权删除已有内容。
3. `command`：181条可执行行；196只是旧索引记录（181命令+7自然语言+8终端回显）；`--once`不是话题名。
4. Motion：三个cmd_vel入口及回调；阿克曼`R=L/tan(delta)`，可能还有PWM舵机输出。
5. AI：Nav2/机械臂/直接Twist/工具调用分开；语音部分可直接向底盘与舵机接口发送控制命令。
6. 安全与STM32：SDK接口≠固件实际实现；RAR分卷/嵌套压缩未完全检查，不能推断厂家不提供工程。
7. 整合前核对所有Markdown链接、Mermaid语法及计数单位。
8. 检查GitHub旧仓库可见性和任何潜在敏感内容，勿将未确认可以公开再分发的源码进一步公开扩散。

## 完成标准

- GitHub新分支真实提交，提供分支、commit SHA、PR链接和diff摘要；不得声称已编译/仿真/实机验证。
- 所有旧文档的矛盾/已修正/待核实状态都有明确说明；不要将工作底稿发布成“技术验收通过”。
- 任何无法访问的本地源码只标记待核实，不用不同版本远程源码悄悄替代。
