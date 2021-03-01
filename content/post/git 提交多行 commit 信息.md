---
title: "Git 提交多行 Commit 信息"
date: 2020-07-19T22:46:28+08:00
lastmod: 2020-07-19T22:46:28+08:00
tags: ["git"]
author: "dianbanjiu"
---

git 提交信息 commit 在项目中追踪代码改动原因非常有用，特别是在团队合作中，一个简洁有效的 commit 可以帮助进行 merge 的人快速了解本次代码改动的必要性。为了保证提交信息的可读性，多行 commit 几乎是不可避免的。下面就来说一下如何实现多行 commit。  

我平时的习惯是使用命令行进行 git 的各种操作，命令行进行 commit 的时候使用的命令是 `git commit -m messgae`，这里面的 message 就是我们要为本次提交添加的 commit，至于实现多行 commit 其实很简单，就是给提交的 commit 内容加上引号。  
```shell
$ git status
位于分支 master

尚无提交

要提交的变更：
  （使用 "git rm --cached <文件>..." 以取消暂存）
        新文件：   readme
$ git add .
$ git commit -m "这是项目的第一提交，                    
dquote> 进行了项目的初始化，
dquote> 并添加了 readme 文件。"
[master（根提交） f647981] 这是项目的第一提交， 进行了项目的初始化， 并添加了 readme 文件。
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 readme
$ git log
commit f6479814f196ab818abcc9ed9b33c8aeffc43b5d (HEAD -> master)
Author: dianbanjiu <dianbanjiu@gmail.com>
Date:   Sun Jul 19 22:59:07 2020 +0800

    这是项目的第一提交，
    进行了项目的初始化，
    并添加了 readme 文件。
```

可以看到，在对 commit 信息的开始添加了第一个引号之后，输入一些信息键入回车，git 会自动转到下一行，并且以 `dquote>` 表示 git 在继续等你输入 commit 信息，直到检测到第二个引号为止。  

如果的 commit 信息中也包含了英文的半角引号，可以通过 `\` 来进行转义。  