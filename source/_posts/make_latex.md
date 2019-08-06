---
title: 使用make编译LaTeX文档
date: 2018-12-20 16:37:00
categories:
 - Software
tags: 
 - make
 - makefile
 - latex
mathjax: false
---

由于一直使用Ubuntu作为主力系统，很多代码编写的工作都用VIM来完成，VIM这个牛逼闪闪的编辑器的确装逼范十足，让人爱不释手，导致我恨不得将所有的编辑器都抛弃掉，全部采用VIM来编辑（除了WORD之外还真没什么不可以）。VIM下来编辑LaTeX文档自然不在话下，利用vim-latex插件可以轻松实现LaTeX文档的编写与编译。但是这个插件也有一定的缺陷——不能利用Magic Comments自动识别编译引擎来编译LaTeX文档，默认情况下`\ll`快捷键只能支持一种编译引擎！这就让经常需要在pdflatex和xelatex之间切换的我很尴尬，每次编写完文档，都需要切到终端去利用不用的命令行来实现LaTeX文档的编译工作。为了充分展现我懒惰的一面，打算采用make来编译LaTeX文档，这样在编写好Makefile之后，我只需要在终端输入`make`就可以实现文档的全自动编译，简直不要太美妙！

# 准备工作

在编写Makefile之前，请确认你的系统环境是否满足以下条件：
- TeX环境，可以是TeX Live或者MiKTeX；
- GNU make工具，在Linux下一般默认已安装（或利用apt install make安装），若在Windows下则需要安装minGW。

make命令在执行时，需要一个Makefile文件，以告诉make命令需要如何去编译和链接程序，接下来我就会编写一个针对于xelatex的Makefile文件。

# Makefile规则

Makefile文件的编写规则大致如下

```
target ... : prerequisites ...
	command
	...
	...
```

**target**：可以是一个目标文件，也可以是一个执行文件，还可以是一个标签（伪目标）

**prerequisites**：生成该target所依赖的文件或target

**command**：该target要执行的命令（任意的shell命令）

这是一个文件的依赖关系，也就是说，target这一个或多个的目标文件依赖于prerequisites中的文件， 其生成规则定义在command中。说白一点就是说:prerequisites中如果有一个以上的文件比target文件要新的话，command所定义的命令就会执行。

这就是Makefile的规则，也是Makefile最核心的内容。

# Makefile编写

```

LATEX=xelatex #定义变量，指定编译引擎
LATEXOPT=--shell-escape #定义变量 指定编译参数
NONSTOP=--interaction=nonstopmode #定义变量 指定编译模式

LATEXMK=latexmk #采用latexmk来编译latex文档
LATEXMKOPT=-pdf #latexmk参数 输出pdf
CONTINUOUS=-pvc #若加入-pvc选项则可以持续检测文件改动并实时显示

MAIN=main #根文件名称
SOURCES=$(MAIN).tex Makefile 
FIGURES := $(shell find figures/* movies/* -type f)

all:    $(MAIN).pdf

#$(MAIN).pdf: $(MAIN).tex $(SOURCES) $(FIGURES)
#	$(LATEXMK) $(LATEXMKOPT) $(CONTINUOUS) \
#            -pdflatex="$(LATEX) $(LATEXOPT) $(NONSTOP) %O %S" $(MAIN)

$(MAIN).pdf: $(MAIN).tex $(SOURCES) $(FIGURES)
	$(LATEXMK) $(LATEXMKOPT)  \
            -pdflatex="$(LATEX) $(LATEXOPT) $(NONSTOP) %O %S" $(MAIN)


force:
	rm $(MAIN).pdf
	$(LATEXMK) $(LATEXMKOPT) $(CONTINUOUS) \
            -pdflatex="$(LATEX) $(LATEXOPT) %O %S" $(MAIN)

clean:
	$(LATEXMK) -C $(MAIN)
	rm -f $(MAIN).pdfsync
	rm -rf *~ *.tmp
	rm -f *.bbl *.blg *.aux *.end *.fls *.log *.out *.fdb_latexmk

once:
	$(LATEXMK) $(LATEXMKOPT) -pdflatex="$(LATEX) $(LATEXOPT) %O %S" $(MAIN)

debug:
	$(LATEX) $(LATEXOPT) $(MAIN)

.PHONY: clean force once all
```

# make编译 

把上面的内容放入Makefile文件，放在LaTeX文件夹下，根据需要修改文件头部定义的变量，可以更换编译引擎，编译参数和编译模式。之后在终端下输入`make`即可完成编译。

若想要删除目录下生成的中间文件，则输入`make clean`即可。
