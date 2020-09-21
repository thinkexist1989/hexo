---
title: Ubuntu与Windows双系统时间设置问题 
date: 2019-8-1 15:38:00
categories:
 - 技术探索
tags: 
 - Linux
 - Windows
mathjax: false
---

## 问题缘由

现在很多人都在电脑里安装了Ubuntu和Windows双系统，在安装完系统之后会发现，系统的显示时间经常会出问题，要么比正常时间快8个小时，要么比正常时间慢8个小时。即使利用网络同步时间之后，当切换过系统之后还是会出现时间差的问题。

这种现象是由于Windows与Ubuntu默认对时间的管理方式不同造成的。我们知道电脑的BIOS里记录着一串时间数据，操作系统就是根据这个数据得到当前时间的。Ubuntu系统使用的是UTC时间（世界协调时），系统会把BIOS里记录的时间看成GMT+0时间，即世界标准时。在Ubuntu系统中有一个设置，是配置当前所在时区的，在中国就会配置为东八区（GMT+8），所以Ubuntu会把BIOS中得到的时间加上8个小时显示出来，随后在系统关机的时候，将当前显示的时间减去8个小时后存入BIOS中。

而在Windows中，会将BIOS中的时间看做本地时间，直接显示出来，因此，当从Ubuntu切换到Windows时，会出现时间差8个小时的情况。

## 解决方法

最简单的方法就是将Ubuntu下的UTC时间关闭，采用和Window一样的时间管理方式。只需要在Ubuntu终端下运行一行代码：

```bash
$sudo timedatectrl set-local-utc 1
```

这样便可以使Windows和Ubuntu下的时间同步了。
