---
title: 机器人操作系统ROS学习笔记：功能包package文件结构
date: 2019-8-6 13:14:00
categories:
 - 技术探索
tags: 
 - ROS
mathjax: false
---

# ROS功能包package

在ROS下，所有的文件按照功能包（package）的形式进行组织，是ROS软件中的基本单元。其中包括ROS节点、库、配置文件等。

# package的典型文件结构

ROS package的典型结构包含一下文件及文件夹：

 - config（文件夹）：放置功能包中的配置文件，由用户创建，文件名可以不同。
 - include（文件夹）：放置功能包中需要用到的头文件。
 - scripts（文件夹）：放置可以直接运行的Python脚本。
 - src（文件夹）：放置需要编译的C++代码。
 - launch（文件夹）：放置功能包中的所有启动文件。
 - msg（文件夹）：放置功能包自定义的消息类型。
 - srv（文件夹）：放置功能包自定义的服务类型。
 - action（文件夹）：放置功能包自定义的动作指令。
 - CMakeLists.txt（文件）：编译器编译功能包的规则（cmake的规则文件）。
 - package.xml（文件）：功能包清单，这个文件可以显示包信息，并注明依赖项，之后可以利用rosdep来安装依赖。

# 元功能包meta package

元功能包是一种特殊的功能包，只包含一个package.xml元功能包清单文件，其主要功能是将多个功能包整合成一个逻辑上的独立功能包，类似于功能包集的概念。
