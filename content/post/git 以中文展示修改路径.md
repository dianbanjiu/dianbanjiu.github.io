---
title: "Git 以中文展示修改路径"
date: 2022-03-20T14:38:24+08:00
lastmod: 2022-03-20T14:38:24+08:00
tags: ["linux","git"]
author: "dianbanjiu"
comment: true
headPic: ""
---

默认情况下，使用 `git status` 来查看更改文件时，如果路径中包含有空格或者一些中文字符时，对应的文件路径会用引号括起来，并且其中的中文字符会以反斜杠 `\` 进行转义，就像下面这样：   
```shell
$ git status
On branch blog
Your branch is up to date with 'origin/blog'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        "content/post/git \344\273\245\344\270\255\346\226\207\345\261\225\347\244\272\344\277\256\346\224\271\350\267\257\345\276\204.md"

nothing added to commit but untracked files present (use "git add" to track)
```

虽然这种配置在你的文件名中含有换行回车之类字符的时候可能比较有用（这种文件数量应该很少吧😑），但是在修改文件比较多的情况下就很难知道对应的是哪些文件，想要正常打印出这些中文字符的话可以通过下面的配置项来解决  
```shell
$ git config core.quotePath false
```

上面的设置是仅对单个项目设置，如果需要针对全局所有的项目关闭这个特性，可以添加 `--global` 参数  
```shell
$ git config --global core.quotePath false
```

参数的一些具体说明可以参考此处： https://git-scm.com/docs/git-config#Documentation/git-config.txt-corequotePath