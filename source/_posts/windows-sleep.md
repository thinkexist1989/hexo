---
title: Windows无法进入睡眠
mathjax: false
date: 2020-02-03 10:28:25
categories:
 - 技术探索
tags:
 - Windows
---

## 问题描述

自己家的台式机在接通电源的情况下点击开始菜单->电源->睡眠，依然无法睡眠，会始终自动唤醒。这是由于Windows开启了**离开模式**(Away Mode)，一般来说能够改变到离开模式的软件主要有：

- **迅雷**：离开模式下载；
- **百度网盘**：传输时不休眠。

## 解决方法

1. 按`WIN`+`R`打开**运行**
2. 输入`regedit`打开注册表编辑器
3. 定位到**计算机\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SessionManager\Power**，在右侧找到`AwayModeEnabled`，若其值为1时表示处于离开模式，无法正常睡眠，将其值改为0即可正常睡眠。
