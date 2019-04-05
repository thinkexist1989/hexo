---
title: 分屏终端Terminator上手
date: 2019-4-6 00:18:00
categories:
 - Software
tags: 
 - tutorials
 - practise
 - linux
mathjax: false
---

之前使用Ubuntu时一直使用系统自带的终端，最苦恼的便是在同时使用多个终端的时候互相重叠，很难控制，有时候甚至找不到之前开的终端跑到哪里了。后来[佳神](https://github.com/ljjhome)给推荐了一款老外经常使用的终端Terminator，上手了几天的确感觉很方便，尤其是分屏功能，可以在一个终端上像Vim一样分成多个终端，在使用ROS这种需要同时运行多个终端的软件时尤为便利。

# 特点
Terminator可以在同一个窗口上分割多个子窗口，每个小窗口运行独立的命令程序。一个父窗口管理多个子窗口，清晰明了知道每个子窗口的运行情况。可以快速自由切换子窗口，并且对子窗口进行最大化和全屏状态。除此之外还有自定义窗口标题、激活标签等等。

# 安装
Ubuntu软件源可以直接安装
```
sudo apt install terminator
```
若想要安装最新版，则需要手动添加ppa
```
sudo add-apt-repository ppa:gnome-terminator
sudo apt update
sudo apt install terminator
```

安装后，按终端的快捷键`Ctrl+Alt+T`便可呼出Terminator终端

# 设置
设置看个人喜好，配色方案什么的可以在设置菜单终端首选项里自由发挥，对于我来说，我一般配置两个：
- 背景配置成80%透明，这样在抄写各种东西的时候很方便，可以透过终端看到终端后面的东西。
- 在配置文件里添加对256色的支持，这个在18.04下是迷人支持的，但是在14.04下是不支持的，需要手动开启一下，否则像Vim的插件airline之类的颜色是无法显示的。

# 快捷键
- 水平分屏`Ctrl+Shift+O`
- 垂直分屏`Ctrl+Shift+E`
- 搜索`Ctrl+Shift+F` 
- 复制`Ctrl+Shift+C`
- 粘贴`Ctrl+Shift+V`
- 关闭当前终端`Ctrl+Shift+W`
- 退出当前窗口`Ctrl+Shift+Q`
- 切换显示当前窗口`Ctrl+Shift+X`
- 全屏状态`F11`
- Clear屏幕`Ctrl+Shift+G`
- 移动分隔条`Ctrl+Shift+方向键`
- 隐藏/显示滚动条`Ctrl+Shift+S`

# 添加右键菜单
若想把Terminator添加到右键菜单方便使用，有2种方案：
- `nautilus-actions`工具，貌似在18.04上不好使
- `fma-config-tool`工具
