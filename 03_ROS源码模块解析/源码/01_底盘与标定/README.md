# 底盘与标定

里程计节点接收多个速度指令入口，并发布里程计及执行器指令；控制算法按底盘类型转换速度。标定包包含角速度和直线运动标定。

[里程计节点](driver/controller/controller/odom_publisher_node.py) · [阿克曼模型](driver/controller/controller/ackermann.py) · [麦克纳姆模型](driver/controller/controller/mecanum.py) · [标定包](calibration/)

本说明基于静态源码阅读；未进行编译、仿真或实机验证。
