---
title: Shell编程：who指令
mathjax: false
date: 2020-01-19 23:37:36
categories:
 - 技术探索
tags:
 - Linux
 - Shell
---

`who`命令用于显示系统中有哪些使用者正在上面，显示的资料包含了使用者 ID、使用的终端机、从哪边连上来的、上线时间、呆滞时间、CPU 使用量、动作等等。

经常使用的参数如下：

| 参数 | 说明 |
| :--- | :--- |
| -H, --heading | 显示各栏的列标题 |
| -b, --boot | 最后一次系统启动时间 |
| -d, --dead | 死进程  |
| -l, --login | 系统登陆进程  |
| -p, --process | 激活进程  |
| -r, --runlevel |  当前运行级别 |
| -t, --time | 显示最后一次系统时钟改动  |
| -T, -w, --mesg | 加入用户信息状态 + - ?  |
| -u, --user | 列出登陆用户  |
| -a 或 --all | 显示全部信息 -b -d -l -p -r -t -T -u |

## 获取本人信息

```bash
$ who am i
think    pts/18     2020-01-19 23:35 (:0)
```
