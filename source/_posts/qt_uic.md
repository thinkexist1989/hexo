---
title: Qt的ui编译机制浅析
date: 2019-3-17 23:15:00
categories:
 - Software
tags: 
 - tutorials
 - qt
mathjax: false
---

# Qt的ui编译机制

利用Qt来设计GUI界面有两种方法，一种是直接在cpp文件中编写界面，另一种就是利用ui文件来编写界面，在cpp中编写界面没有在ui文件中利用拖拽的形式来编写界面来更直观。但是Qt是如何将ui文件转换成C++代码却很令我困惑，因此我特意了解了一下Qt的ui编译机制，加深对Qt的理解。

# 利用uic来编译.ui文件

Qt的.ui文件通过Qt Designer设计好以后，利用uic程序将.ui文件中的xml语法转换为c++的类文件，假设ui文件名为`mainwindow.ui`，在命令行中输入如下命令:
```
uic mainwindow.ui -o ui_mainwindow.h
```

> 注意：若存在多个Qt版本，则可能需要指定相应uic执行路径

通过uic会生成`mainwindow.ui`文件对应的头文件`ui_mainwindow.h`，这个`ui_mainwindow.h`文件的内容大致如下：
```
/********************************************************************************
** Form generated from reading UI file 'mainwindow.ui'
**
** Created by: Qt User Interface Compiler version 5.9.5
**
** WARNING! All changes made in this file will be lost when recompiling UI file!
********************************************************************************/

#ifndef UI_MAINWINDOW_H
#define UI_MAINWINDOW_H

#include <QtCore/QVariant>
#include <QtWidgets/QAction>
#include <QtWidgets/QApplication>
#include <QtWidgets/QButtonGroup>
#include <QtWidgets/QHeaderView>
#include <QtWidgets/QMainWindow>
#include <QtWidgets/QMenuBar>
#include <QtWidgets/QStatusBar>
#include <QtWidgets/QToolBar>
#include <QtWidgets/QWidget>

QT_BEGIN_NAMESPACE

class Ui_MainWindow
{
public:
    QMenuBar *menuBar;
    QToolBar *mainToolBar;
    QWidget *centralWidget;
    QStatusBar *statusBar;

    void setupUi(QMainWindow *MainWindow)
    {
        if (MainWindow->objectName().isEmpty())
            MainWindow->setObjectName(QStringLiteral("MainWindow"));
        MainWindow->resize(400, 300);
        menuBar = new QMenuBar(MainWindow);
        menuBar->setObjectName(QStringLiteral("menuBar"));
        MainWindow->setMenuBar(menuBar);
        mainToolBar = new QToolBar(MainWindow);
        mainToolBar->setObjectName(QStringLiteral("mainToolBar"));
        MainWindow->addToolBar(mainToolBar);
        centralWidget = new QWidget(MainWindow);
        centralWidget->setObjectName(QStringLiteral("centralWidget"));
        MainWindow->setCentralWidget(centralWidget);
        statusBar = new QStatusBar(MainWindow);
        statusBar->setObjectName(QStringLiteral("statusBar"));
        MainWindow->setStatusBar(statusBar);

        retranslateUi(MainWindow);

        QMetaObject::connectSlotsByName(MainWindow);
    } // setupUi

    void retranslateUi(QMainWindow *MainWindow)
    {
        MainWindow->setWindowTitle(QApplication::translate("MainWindow", "MainWindow", Q_NULLPTR));
    } // retranslateUi

};

namespace Ui {
    class MainWindow: public Ui_MainWindow {};
} // namespace Ui

QT_END_NAMESPACE

#endif // UI_MAINWINDOW_H

```

这个文件就是将.ui文件转换为C++可以看得懂的代码，之后参与Qt源代码的编译，可以看到这个文件中定义了一个`Ui_MainWindow`类，里面包含了在.ui文件中创建的各种窗口部件的实现，文件最后还定义了一个命名空间：
```
namespace Ui {
    class MainWindow: public Ui_MainWindow {};
}
```

这个命名空间Ui中包含了一个新类`MainWindow`，而这个新类继承自`Ui_MainWindow`类，之后我们便可以在别的文件中利用`Ui::MainWindow`调用这个ui类，或者直接调用`Ui_MainWindow`也可以。

# Ui调用
在Qt项目中，在`mainwindow.h`中添加Ui命名空间的声明：
```
namespace Ui {
class MainWindow;
}
```
之后在继承`QMainWindow`类的对象声明中添加成员变量：
```
Ui::MainWindow *ui;
```

在`mainwindow.cpp`的类构造函数中，初始化ui成员变量：
```
ui = new Ui::MainWindow;
ui->setupUi(this);
```
在类的析构函数中释放ui指针指向的内存空间：
```
delete ui;
```
这样便实现了ui的调用。