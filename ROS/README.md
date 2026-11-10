# ROS 代码

- [按功能模块浏览实际代码](源码/)
- [按原功能包层级浏览](src/)

源码目录以完整 ROS 功能包为单位分类，包内文件与 src 目录相同。分类目录中的 COLCON_IGNORE 防止构建工具重复发现同名功能包；构建时请使用原功能包目录，并核对环境与硬编码路径。

本目录依据本次提供的 998 个文件及历史清单重建。附件原目录层级已丢失，同名文件按清单和附件编号顺序推断，尚未与原始压缩包逐文件哈希核对。一个配置文件中的非空密钥已清空，原附件未修改。

以下两个位置的同一份约 145 MB FastSAM 模型尚未上传，相关功能需补齐模型：

- src/large_models_examples/large_models_examples/resources/models/FastSAM-x.pt
- src/test_stand/test_stand/resources/models/FastSAM-x.pt

分类副本中相应模型也缺失。代码尚未编译、仿真或实机验证。
