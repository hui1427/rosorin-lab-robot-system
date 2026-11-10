# ROS 源码使用说明

本目录依据本次提供的 998 个文件及既有文件清单重建，按原有 ROS 功能包组织。由于附件不再包含原始目录，同名文件依据原文件清单和附件编号的顺序推断，尚未与原始压缩包逐文件哈希核对。

## 尚未上传的模型

以下两个路径指向同一份约 145 MB 的 FastSAM 模型，超过 GitHub 普通文件大小限制，当前尚未上传：

- `src/large_models_examples/large_models_examples/resources/models/FastSAM-x.pt`
- `src/test_stand/test_stand/resources/models/FastSAM-x.pt`

相关功能在补齐模型前不能视为可运行。其他附件已上传；`src/large_models/large_models/large_models/config.py` 中的非空密钥已清空，原始附件未修改。此代码尚未在目标 ROS 环境编译、仿真或实机测试。
