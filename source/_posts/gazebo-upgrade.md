---
title: 机器人操作系统ROS学习笔记：Gazebo的更新与配置
mathjax: false
date: 2020-02-02 22:04:25
categories:
 - 技术探索
tags:
 - ROS
---

由于Gazebo随ROS一起安装的版本通常不是当前ROS版本下最新的，因此可以通过手动升级的方式来将Gazebo更新为当前ROS版本下对应的最新版本。

## 查看当前Gazebo版本

输入以下命令查看当前Gazebo版本：

```bash
$gazebo -v
```

## 升级方法

执行以下命令升级Gazebo：

```bash
$sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'
$wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -
$sudo apt-get update
$sudo apt-get install gazebo7
```

以Ubuntu 16.04 + ROS Kinetic为例，默认情况下Gazebo的版本为7.0.0，在更新完之后Gazebo的版本便成为7.16.0。
