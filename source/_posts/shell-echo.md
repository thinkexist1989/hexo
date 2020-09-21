---
title: Shell编程：echo指令
mathjax: false
date: 2020-01-19 23:57:54
categories:
 - 技术探索
tags:
 - Linux
 - Shell
---

`echo`命令用于在终端打印一行文本，例如，

```bash
$ echo this is a test
this is a test
```

经常使用的参数如下：

| 参数 | 说明 |
| :--- | :--- |
| -n | 不输出结尾换行符 |
| -e | 启用反斜杠转义的解释 |
| -E | 不启用反斜杠转义的解释（默认）|

若采用`-e`， 则下列转义字符将被解释：

| 参数 | 说明 |
| :--- | :--- |
| \\ | 不输出结尾换行符 |
| \b | 退格 backspace |
| \c | 不再输出后面内容 |
| \e | 退出 escape |
| \f | 退格 backspace |
| \n | 换行 |
| \r | 回车 |
| \t | 水平tab |
| \v | 垂直tab |
| \0NNN | 八进制NNN |
| \xHH | 16进制HH |
