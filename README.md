# ROSOrin Lab｜机器人系统开发资料

ROS 2 Based Autonomous Mobile Manipulation Robot System

> **V1.2 是待技术验收的分析底稿。** 完整本地源码尚未在本轮逐文件复核，未进行编译、仿真或实机验证。涉及固件的接口描述不等同于固件源码或内部实现。

## 中文目录总览

```text
rosorin-lab-robot-system/
├── README.md                    项目首页与中文导航
└── docs/                        V1.2 分析文档
    ├── source-index/            源码目录、文件功能映射与参考链接
    ├── command/                 命令索引
    ├── architecture/            系统架构、启动流程和模块关系图
    ├── modules/                 按系统功能分类的分析
    ├── hardware/                硬件和 STM32 映射
    ├── reuse/                   源码复用指南
    ├── issues/                  已知问题及待核实事项
    └── provenance/              来源、证据与修订记录
```

## 功能模块导航

| 模块 | 分析文档 |
| --- | --- |
| 系统与启动 | [系统](docs/modules/system.md)、[启动流程](docs/architecture/startup_flow.md) |
| 底盘与运动控制 | [运动](docs/modules/motion.md) |
| 控制板与硬件 | [硬件](docs/modules/hardware.md)、[STM32 分析](docs/hardware/stm32_analysis.md) |
| 传感器与感知 | [传感器](docs/modules/sensors.md)、[感知](docs/modules/perception.md) |
| 建图与导航 | [导航](docs/modules/navigation.md) |
| 机械臂与舵机 | [机械臂](docs/modules/manipulation.md) |
| AI 与语音应用 | [AI](docs/modules/ai.md) |
| 安全、仿真 | [安全](docs/modules/safety.md)、[仿真](docs/modules/simulation.md) |

## 索引与证据

- [文档总览和使用边界](docs/README.md)
- [完整源码目录](docs/source-index/complete_source_tree.md) · [文件功能映射](docs/source-index/file_function_mapping.csv) · [参考链接表](docs/source-index/github_links.csv)
- [命令索引](docs/command/command_index.md) · [系统总览](docs/architecture/system_overview.md) · [模块关系图](docs/architecture/module_relationship.md)
- [来源清单](docs/provenance/evidence/SOURCE_MANIFEST.md) · [V1.2 修订记录](docs/provenance/V1.2_REVISION_LOG.md) · [已知问题](docs/issues/known_issues.md)

## 整理原则

- main 保存分析文档；原厂资料库及程序目录保留在私有的 [V1 分支](https://github.com/hui1427/rosorin-lab-robot-system/tree/feature/official-source-library-v1)，未搬动或修改原厂源码。
- 分析采用本地源码历史索引；厂商 GitHub 固定提交仅作对照，不能视作同一版本。各结论以文档中的证据和待核实说明为准。
- 本资料中的命令和路径不代表已在本机或机器人上验证；启动流程中的路径疑点见[文档总览](docs/README.md)。
