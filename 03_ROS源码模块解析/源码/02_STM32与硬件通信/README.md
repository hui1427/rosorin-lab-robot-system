# STM32与硬件通信

ROS 主控端通过控制板节点与串口 SDK 发送执行器指令并接收状态。接口定义包包含电机、舵机等消息；这些代码不是 STM32 固件内部源码。

[控制板节点](driver/ros_robot_controller/ros_robot_controller/ros_robot_controller_node.py) · [串口 SDK](driver/ros_robot_controller/ros_robot_controller/ros_robot_controller_sdk.py) · [接口定义](driver/ros_robot_controller_msgs/)

本说明基于静态源码阅读；未进行编译、仿真或实机验证。
