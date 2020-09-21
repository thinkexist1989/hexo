---
title: thinkpad-p52-ubuntu-install.md
mathjax: false
date: 2020-02-02 23:45:09
categories:
 - 技术探索
tags:
 - Linux
---

最近弄了一台Thinkpad P52移动工作站，想在上面安装Windows 10和Ubuntu 16.04作为双系统，但是安装Ubuntu时会出现一个提示："The system is in low graphic mode"，之后便卡住了，无法安装。后来一顿尝试才发现，这是由于Thinkpad的核显与独显冲突造成的，需要在主板BIOS里禁用核显。

## 解决方法

开机按`Enter`进入BIOS配置，随后按`F1`进入BIOS Setup，在Config菜单下选择Display菜单打开，第二项Graphic Device选项中，选择`Discrete Graphics`，随后保存退出，便实现了核显的禁用。
