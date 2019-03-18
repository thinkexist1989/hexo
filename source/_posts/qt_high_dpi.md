---
title: Qt高分屏支持
date: 2019-3-17 22:47:00
categories:
 - Software
tags: 
 - tutorials
 - qt
mathjax: false
---

随着智能手机等电子设备的发展，越来越多的电子设备开始使用上了高分屏，高分屏的确让屏幕的观感更好，但是若没有相应的支持，由于DPI过高，会导致程序字体按钮等看上去特别小，影响使用。而Qt从5.6版本开始对高分屏有了相应的支持，只需要一行代码即可完成对高分屏的支持。

# 代码使用

在程序的`main`函数中`QApplication`对象初始化之前加入如下一行代码即可：
```
QApplication::setAttribute(Qt::AA_EnableHighDpiScaling);
```
这个文件看上去大致如下：
```
#include "mainwindow.h" //头文件
#include <QApplication>

int main(int argc, char *argv[])
{
    QApplication::setAttribute(Qt::AA_EnableHighDpiScaling); //添加高分屏支持
    QApplication a(argc, argv);
    MainWindow w;
    w.show();

    return a.exec();
}
```
# 说明
添加完上述代码后，若是高分屏的电脑则会将Qt的窗口相应的放大，若不是高分屏的电脑则分辨率不变，这样便实现了对高分屏的支持。