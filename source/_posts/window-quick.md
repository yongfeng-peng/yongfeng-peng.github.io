---
title: window下git bash 命令行实用技巧
date: 2019-05-04 11:43:59
tags:
---

#### 命令窗口，使用快捷键快速跳转
1. 新建一个文件目录，mkdir ~/repos
2. 切换目录 cd ~/repos
3. 克隆 git clone https://github.com/rupa/z.git
4. 新建bashrc文件，如果**已经存在这个文件，可忽略这步**，touch ~/.bashrc
5. 打开bashrc文件，start ~/.bashrc
6. 在文件里写入
```
. ~/repos/z/z.sh
alias j='z'
```
7. 重启 Git Bash
8. 验证你的配置，使用 j XXX 就可以快速到达之前去过的目录了
9. 使用 j 可以查看所有去过的目录，使用z也可以调整目录，调整目录名都可以不用写文件名全称，如：d/document 使用快速调整 z doc 或 j doc
    **注意，~/repos/z/z.sh 不能删除，删除了就不能用了**
