---
title: Ubuntu下Qt Creator无法输入中文问题
date: 2020-1-17 14:20:00
categories:
 - 技术探索
tags: 
 - Qt
 - 输入法
 - 小技巧
mathjax: false
---

在Ubuntu 16.04系统下安装Qt5.12.2之后，发现在Qt Creator下无法使用Sogou Linux输入法实现中文输入。这是由于Sogou输入法使用的是fcitx框架，而Qt Creator下缺少fcitx输入法的插件造成的。

## 解决步骤

### 1. 确认安装fcitx-frontend-qt5

```bash
sudo apt install fcitx-frontend-qt5
```

### 2. 确认该路径下存在文件

由于Sogou Linux输入法采用的是fcitx框架，因此，需要确认`libfcitxplatforminputcontextplugin.so`是否存在

```bash
cd /usr/lib/x86_64-linux-gnu/qt5/plugins/platforminputcontexts
ls | grep libfcitx
```

### 3. 将此lib文件复制到Qt与Qt Creator的对应路径下

```bash
sudo cp libfcitxplatforminputcontextplugin.so /opt/Qt5.12.2/Tools/QtCreator/lib/Qt/plugins/platforminputcontexts/
sudo cp libfcitxplatforminputcontextplugin.so /opt/Qt5.12.2/5.12.2/gcc_64/plugins/platforminputcontexts/
```

随后重启Qt Creator，便可以输入中文了。
