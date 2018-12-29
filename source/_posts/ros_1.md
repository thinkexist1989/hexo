---
title: 机器人操作系统ROS学习笔记（一）：安装与配置
date: 2018-8-30 11:42:00
categories:
 - Robot Operating System
tags: 
 - summary
 - ros
 - tutorials
mathjax: false
---

![](http://oxuze83b3.bkt.clouddn.com/image/ros/ros_org.png)

# 什么是ROS？

ROS (Robot Operating System, 机器人操作系统)在其[官网ros.org](ros.org)上是这么介绍的：

> ROS 提供一系列程序库和工具以帮助软件开发者创建机器人应用软件。它提供了硬件抽象、设备驱动、函数库、可视化工具、消息传递和软件包管理等诸多功能。ROS遵循BSD开源许可协议。

听上去还是没有感觉到ROS的厉害之处，在我刚开始接触ROS的时候，以为ROS只是一些函数库，类似于各种应用程序开放的API一样，我只要调用相应的函数，就执行相应的功能。但是在后来的使用过程中，我才意识到ROS并不仅仅是一堆可供调用的函数而已。实际上ROS是一个机器人系统的开发框架，之所以可以称自己为“操作系统”，就是因为在这个框架之上，ROS为机器人的开发提供了一个平台，类似于一个操作系统一样，他帮你抽象好了硬件，设置好消息传递方式，让你可以更加方便快捷的实现你的想法，而不用从机器人硬件的底层开始编写程序，提高机器人系统的研发效率。

# ROS能做什么？

作为一种标准化机器人软件框架，利用ROS中大量的示例代码和开源程序可以轻松地完成机器人编程和控制。ROS的设计目标之一便是提高代码的复用率，ROS在运行时松散耦合，各个子功能之间相互独立，利用发布与订阅消息进行通讯，因此极易扩展，并且ROS提供了大量的监视和调试工具，方便机器人控制程序的开发。

我认为ROS也有一定的缺点，即实时性差，以我对ROS的了解，在一台普通电脑上，ROS的控制频率很难超过20Hz。当然ROS设计的目的是快速的进行机器人系统的开发，提高控制频率也并不是ROS关注的方向。

# 安装ROS

在ROS的官方网站[ros.org](http://ros.org)有详细的安装介绍，这里不再赘述，只简单介绍以下安装步骤。

## ROS版本与操作系统环境

ROS在Ubuntu下有着完美的支持，因此对于像我这样的新手果断选择在Ubuntu下安装ROS。不同的Ubuntu版本对应着不同版本的ROS，必须按照官网上ROS版本所支持的Ubuntu版本来进行对应安装。由于我目前的版本为Ubuntu 18.04 LTS，因此我安装的是ROS Melodic Morenia，如果你使用的是Ubuntu 16.04 LTS,则需要对应安装ROS Kinetic Kame，这两个版本的ROS都是长期支持版，推荐使用。

## 添加sources.list

```
$sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

## 设置keys

```
$sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116
```

## 更新库

```
$sudo apt-get update
```

## 安装ROS完整版

```
$sudo apt-get install ros-melodic-desktop-full
```

## 安装rosdep

```
$sudo rosdep init
$rosdep update
```

## 配置环境

```
$echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
$source ~/.bashrc
```

## 添加用于构建package的依赖

```
$sudo apt-get install python-rosinstall python-rosinstall-generator python-wstool build-essential
```

到这里，ROS就已经安装完成，可运行如下指令来验证ROS环境是否已经配置完成：

```
$printenv | grep ROS
```
如果显示如下内容则说明已经完成。

```
ROS_ETC_DIR=/opt/ros/melodic/etc/ros
ROS_ROOT=/opt/ros/melodic/share/ros
ROS_MASTER_URI=http://localhost:11311
ROS_VERSION=1
ROS_PYTHON_VERSION=2
ROS_PACKAGE_PATH=/opt/ros/melodic/share
ROSLISP_PACKAGE_DIRECTORIES=
ROS_DISTRO=melodic

```
