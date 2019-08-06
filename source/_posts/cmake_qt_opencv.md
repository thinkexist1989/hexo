---
title: 利用CMAKE构建Qt和OpenCV项目 
date: 2019-3-5 12:18:00
categories:
 - Software
tags: 
 - cmake
 - Qt
 - opencv
mathjax: false
---

# 预备知识

## Qt
Qt是一套完整的跨平台软件开发框架，在开源世界无人不知无人不晓。

官网地址：[https://qt.io](https://qt.io/)

## OpenCV
开源计算机视觉库，如何编译OpenCV可以参考我的另一篇博客[《利用CMAKE编译OpenCV源码》](http://yluo.name/2019/02/12/cmake_build_opencv/)

## CMAKE
关于什么是CMAKE可以参考我的另一篇博客[《利用CMAKE构建和管理软件项目》](http://yluo.name/2019/01/02/cmake/)。

# 目录结构

- 项目目录：qt_cmake
  - 源文件目录： src
    - 文件： 
    - main.cpp
    - mainwindow.cpp
    - mainwindow.h
    - mainwindow.ui
  - 构建文件目录：build


# 文件内容

新建一个文件夹名为`qt_cmake`，在文件夹下新建两个文件夹，一个为`src`，另一个为`build`。其中`src`用来放置工程源代码文件，`build`用来存放构建生成的项目文件。

在`src`文件夹下新建`main.cpp`文件，内容如下:
```
#include "mainwindow.h"
#include <QApplication>

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    MainWindow w;
    w.show();

    return a.exec();
}
```

利用Qt Creator新建一个Qt设计师界面类，会自动生成3个文件：`mainwindow.h`，`mainwindow.cpp`，`mainwindow.ui`。

随后打开`mainwindow.cpp`文件，在其中填入相关的OpenCV代码做测试：
```
#include "mainwindow.h"
#include "ui_mainwindow.h"

#include <opencv2/opencv.hpp>

MainWindow::MainWindow(QWidget *parent) :
    QMainWindow(parent),
    ui(new Ui::MainWindow)
{
    ui->setupUi(this);

    cv::Mat srcImg = cv::imread("/path/to/your/image.jpg");
    cv::imshow("origin image",srcImg);

    cv::waitKey(0);
}

MainWindow::~MainWindow()
{
    delete ui;
}
```

在`src`文件夹下建立CMakeLists.txt文件，在里面建立内容如下：

```
cmake_minimum_required(VERSION 3.1)

project(qt_cmake)

# Find includes in corresponding build directories
set(CMAKE_INCLUDE_CURRENT_DIR ON)
# Instruct cmake to run moc automatically when needed
set(CMAKE_AUTOMOC ON)
# Create code from a list of Qt designer ui files
set(CMAKE_AUTOUIC ON)

# set OpenCV directory
#set(OpenCV_DIR /usr/share/OpenCV)

# set Qt directory
#set(CMAKE_PREFIX_PATH /home/think/Qt5.11.1/5.11.1/gcc_64/lib/cmake)

# Find the QtWidgets library
find_package(Qt5 REQUIRED Widgets Core)
#find_package(Qt5Widgets CONFIG REQUIRED)
# Finde OpenCV library
find_package(OpenCV REQUIRED)


message(STATUS "OpenCV library status:")
message(STATUS "    version: ${OpenCV_VERSION}")
message(STATUS "    libraries: ${OpenCV_LIBS}")
message(STATUS "    include path: ${OpenCV_INCLUDE_DIRS}")

include_directories(${OpenCV_INCLUDE_DIRS})

set(SOURCES
	main.cpp
	mainwindow.cpp)

set(FORMS
	mainwindow.ui)

add_executable(main ${SOURCES} ${FORMS})
#link_directories(${OpenCV_LIBRARY_DIRS})
target_link_libraries(main Qt5::Widgets ${OpenCV_LIBS})
```

简单介绍一下CMakeLists.txt文件中代码含义

1. cmake_minimum_required(VERSION 3.1)

    接下来是设置cmake要求的最低版本号：为3.1。CMAKE在3.1版本以上才支持Qt。

2. project(qt_cmake [CXX] [C] [Java])
   
    定义工程名称，并可指定工程支持的语言，支持的语言列表是可以忽略的，这个指令隐式的定义了两个cmake变量:qt_cmake_BINARY_DIR以及qt_cmake_SOURCE_DIR。前者指构建路径，后者指工程路径，即CMakeLists.txt所在的路径。

    同时cmake系统也帮助我们预定义了PROJECT_BINARY_DIR和PROJECT_SOURCE_DIR变量，他们的值分别跟qt_cmake_BINARY_DIR与qt_cmake_SOURCE_DIR一致。

    为了统一起见，建议以后直接使用PROJECT_BINARY_DIR，PROJECT_SOURCE_DIR，即使修改了工程名称，也不会影响这两个变量。如果使用了qt_cmake_SOURCE_DIR，修改工程名称后，需要同时修改这些变量。

3. set(OpenCV_DIR /usr/share/OpenCV)

    设置OpenCV_DIR变量，若只安装了一个版本的OpenCV则不用设置这个变量，若存在多个版本的OpenCV则需要利用OpenCV_DIR变量来指定想要的版本。

4. find_package(OpenCV REQUIRED)

    find_package这个指令以被用来在系统中自动查找配置构建工程所需的程序库。在linux和unix类系统下这个命令尤其有用。CMake自带的模块文件里有大半是对各种常见开源库的find_package支持，支持库的种类非常多。

    当它找到OpenCV程序库之后，就会帮助我们预定义几个变量，OpenCV_FOUND、OpenCV_INCLUDE_DIRS、OpenCV_LIBRARY_DIRS、OpenCV_LIBRARIES，它们分别指是否找到OpenCV，OpenCV的头文件目录，OpenCV的库文件目录，OpenCV的所有库文件列表。

5. include_directories(${OpenCV_INCLUDE_DIRS})

    OpenCV相关包含路径

6. add_executable(main ${SOURCES} ${FORMS})

    添加可执行文件main

7. target_link_libraries(main Qt5::Widgets ${OpenCV_LIBS})

    添加动态链接库

# 构建

- 进入build目录
- 执行`cmake ../src`
- 执行`make`
- 运行程序`./main`