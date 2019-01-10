---
title: 利用CMAKE构建和管理软件项目
date: 2019-1-2 10:02:00
categories:
 - Software
tags: 
 - tutorials
 - cmake
 - Makefile
mathjax: false
---

# CMAKE是什么

CMake是一个跨平台的安装（编译）工具，可以用简单的语句来描述所有平台的安装(编译过程)。他能够输出各种各样的makefile或者project文件，能测试编译器所支持的C++特性,类似UNIX下的automake。只是 CMake 的组态档取名为 CMakeLists.txt。Cmake 并不直接建构出最终的软件，而是产生标准的建构档（如 Unix 的 Makefile 或 Windows Visual C++ 的 projects/workspaces），然后再依一般的建构方式使用。这使得熟悉某个集成开发环境（IDE）的开发者可以用标准的方式建构他的软件，这种可以使用各平台的原生建构系统的能力是 CMake 和 SCons 等其他类似系统的区别之处。

对于一个大型软件，其编译、维护是一个复杂而耗时的过程。它涉及到大量的文件、目录，这些文件可能是在不同的时间、由不同的人、在不同的地方分别写的，其中一些是程序，有些是数据，有些是文档，有些是衍生文件。甚至参与开发的人员也不一定清楚所有文件的细节，包括如何处理它们。此外，构成软件的文件数目可能达到成百上千，甚至成千上万个，开发过程中当修改了少量几个文件后，往往只需要重新编译、生成少数几个文件。有效地描述这些文件之间的依赖关系以及处理命令，当个别文件改动后仅执行必要的处理，而不必重复整个编译过程，可以大大提高软件开发的效率。

# 为什么用CMAKE

如果你之前有过维护软件包的构建和安装的经验，你就会对CMake有兴趣。当前很多项目都可以在Linux下用Makefile和在Windows下用Visual Studio进行编译；这要求开发者在对应的系统下保持构建工具的更新，并且不同系统的构建行为保持一致；如果再引入XCode，这需要更多的构建工具，这样会是一个问题。如果在此基础上引入可选组件，比如如果系统上有libjpeg，项目就支援JPEG，这会造成更大的麻烦。CMake提供了一个简单的，易于理解的文件格式来解决上述问题。如果一个项目有多个开发者参与，或者这个项目有多个目标平台；那么不可避免的需要在多台PC上进行构建，不同的PC在开发环境上会有差异。

- 自动进行项目构建所需的program、library、header file的查找能力；
- 在source tree以外进行构建的能力；
- 为Qt moc，SWIG等自动产生复杂的自定义命令的能力；
- 在configuration阶段进行可选组件定制的能力；
- 自动从简单文件文件产生workspace和project的能力；
- 配置生成静态库/动态库的能力；
- 自动产生文件依赖，支持并行编译；

# CMAKE基本语法

build的过程由每个目录下的名为`CMakeFileLists.txt`的文件组成的一系列文件列表所控制；
`CMakeFileLists.txt`文件由CMake语句进行项目描述，CMake语句的语法为：

```
command(args...)
```

- `command`是命令的名字，CMake是不区分大小写的；
- `args`是一系列由空格分隔的参数，如果参数中有空格，参数需要用双引号引起来；

- 变量被引用的格式是`${VAR}`;
- 多个参数可以使用`set`来使之构成一个list。

```
set(Foo a b c)
```

这样设置的结果是`Foo`的值是 a b c;

CMake可以直接访问系统环境变量和Windows注册表；
访问系统环境变量的语法：

```
$Env{ARG}
```

访问Windows注册表：

```
[HKEY_CURRENT_USER\\Software\\path1\\path2;key]
```

# 如何运行CMAKE

举一个简单的例子，在文件夹下建立一个C语言源文件`main.c`，内容如下：

```
#include <stdio.h>

void main()
{
  float sum;
  int i;

  for(int i=0;i<100;i++)
          sum += 0.1;

  printf("sum:%f\n",sum);
}
```
在文件夹下建立`CMakeLists.txt`文件，内容如下：

```
cmake_minimum_required(VERSION 2.8)
add_executable(Main main.c)
```

在终端运行：

```
$cmake .
```

则可以看到在文件夹下生成了Makefile文件，紧接着运行：
```
$make
```

在文件夹下看到生成了可执行文件`Main`，于是在终端运行：
```
$./Main
```
可以看到终端运行了程序并输出：
```
$sum:10.000002
```
# 后记

强烈推荐一本CMAKE圣经《Mastering CMAKE》，CMAKE的强大之处只有一点点的学习体会才能感受到，我也会在今后的学习中慢慢领悟CMAKE的精髓，用CMAKE来管理构建我自己的项目。