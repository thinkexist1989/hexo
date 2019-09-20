---
title: Git 未知索引格式错误
date: 2019-9-29 11:00:00
categories:
 - Software
tags: 
 - Git
mathjax: false
---

# 错误说明

运行`git status`时，提示

```
fatal: Unknown index entry format xxxxx
```

# 解决方法

进入仓库文件夹，输入以下指令：

```
rm -f .git/index
git reset
```

大功告成，喜大普奔。
