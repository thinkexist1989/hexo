---
title: Ubuntu右上角状态指示栏顺序配置
date: 2020-1-16 10:29:00
categories:
 - 技术探索
tags: 
 - Linux
 - 小技巧
mathjax: false
---

Ubuntu的右上角状态指示栏(indicator)顺序通常按照先加载靠右，后加载靠左的顺序放置图标，但有时配置顺序不理想，需要手动调整。

# 设置方法

在Ubuntu中存在一个表单，这个表单里固定了一部分indicator的顺序，这个文件保存在如下位置：

```
/usr/share/indicator-application/ordering-override.keyfile
```

若是针对个人用户的配置，可以将ordering-override.keyfile复制到用户目录下：

```
cp /usr/share/indicator-application/ordering-override.keyfile ~/.local/share/indicators/application/
```

随后利用`vim ~/.local/share/indicators/application/ordering-override.keyfile`打开`.keyfile`文件，可以看到，文件里的内容如下所示：

```
[Ordering Index Overrides]
nm-applet=1
My_Weather_Indicator=2
lang_indicator=3
bluetooth_manager=4
transmission=6
```

数字越大则越靠近左边，而数字越小则越靠近右边。若想要indicator放置在最左边，则可以使数值为-1：

```
indicator_sysmonitor=-1
```
