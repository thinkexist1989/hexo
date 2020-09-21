---
title: Ubuntu16.04下Terminator支持256色
mathjax: false
date: 2020-02-07 19:56:31
categories:
 - 技术探索
tags:
 - Terminator
---


## 问题描述

在Ubuntu 16.04下利用`apt install terminator`安装Terminator后，其版本为0.98。而0.98版本的Terminator默认不支持256色，导致终端下无法显示很多颜色，看上去很难受。

## 解决方法

1. 打开Terminator，在`Preferences`中选择`Profiles`；
2. 在`Command`选项卡中，将`Run a custom command instead of my shell`选中；
3. 在`Custom command` 中输入`TERM=xterm-256color bash -l`

随后重启Terminator，便可以看到已经支持256色，终端里显示出了更多的颜色，来区分不同的项目了。
