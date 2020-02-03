---
title: Ubuntu蓝牙不可用
mathjax: false
date: 2020-02-03 10:43:10
categories:
 - 技术探索
tags:
 - Linux
 - Bluetooth
---

## 问题描述

之前在Ubuntu下不小心删除了`/var/lib/bluetooth/`下的文件夹，导致蓝牙界面一直显示disabled。

## 解决方法

在终端里输入以下命令：

```
rfkill unblock bluetooth
```

随后重启系统并打开蓝牙，蓝牙便显示enabled了。
