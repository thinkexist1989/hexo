---
title: 机器人操作系统ROS学习笔记：CMakeLists.txt文件
date: 2019-8-6 14:17:00
categories:
 - 技术探索
tags: 
 - ROS
 - cmake
mathjax: false
---

# CMakeLists.txt

ROS中使用的编译器catkin是对cmake的一种封装，因此编译规则使用的也是CMakeLists.txt文件，关于CMakeLists.txt文件的规则之前在博客中已经提及过，不再赘述。

# 编译规则

在ROS功能包中的CMakeLists文件主要包括以下配置项：

- project(PROJ_NAME): 项目名称，一般情况下使用catkin_create_pkg命令会自动生成；
- find_package(catkin REQUIRED COMPONTS rospy roscpp ...): 寻找相关的功能包；
- include_directories(include ${catkin_INCLUDE_DIRES}): 设置头文件的相对路径，通常在功能包下将相关的头文件都放置在include文件夹下；
- add_executable(node_name src/xxx.cpp): 设置需要编译的代码和生成的可执行文件；
- target_link_libraries(node_name ${catkin_LIBRARIES}): 设置链接库；
- add_dependencies(node_name ${PROJECT_NAME}_generate_messages_cpp): 设置依赖，一般情况下自定义了消息类型的话便需要添加依赖。
