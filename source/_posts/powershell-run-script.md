---
title: Windows Powershell运行自定义脚本
mathjax: false
date: 2020-02-05 21:35:53
categories:
 - 技术探索
tags:
 - Windows
 - Powershell
---

## 问题描述

Windows Powershell为了防止恶意脚本的执行，设计了一个执行策略(Execution Policy)，若发现自定义的脚本无法运行，则需要更改执行策略。

执行策略包括6种类型：
 - Restricted 受限制的，只能执行单个命令，不能执行脚本
 - AllSigned 允许执行有数字签名的脚本
 - RemoteSigned 本地脚本可以运行，网络下载的需要数字签名
 - Unrestricted 无限制，但从网络下载的会有安全提示
 - Bypass 不设任何限制，且没有安全提示
 - Undefined 未设置策略，使用继承或默认策略

## 解决方法

利用管理员身份打开Powershell终端，输入以下命令将策略更改为`RemoteSigned`：

```
$ Set-ExecutionPolicy RemoteSigned
```

