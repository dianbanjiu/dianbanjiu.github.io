---
title: "Git 子模块管理"
date: 2021-01-04T23:10:33+08:00
lastmod: 2021-01-04T23:10:33+08:00
tags: ["git", "submodule"]
author: "dianbanjiu"
toc: true
---

## 子模块的创建
子模块的创建很简单，通过下面的命令就可以将 `xxx` 仓库注册到当前目录的 `xxx` 路径下。  
```shell
$ git submodule add https://github.com/xxx/xxx.git ./xxx
```

## 子模块的更新
如果需要在 clone 项目时将子模块也 clone 下来，可以使用 `--recurse-submodules` 参数。  
```shell
$ git clone --recurse-submodules https://github.com/xxx/xxx.git
```

如果 clone 项目的时候忘记使用 `--recurse-submodules` 参数，也可以在项目 clone 完成之后依次执行下面的命令来获取子模块。  
```shell
$ git submodule --init
$ git submodule update --recursive
```

## 子模块的删除
相比于子模块的创建和更新，子模块的删除相对比较麻烦一些。首先需要从注册路径删除对应子模块
```shell
$ rm -rf ./xxx
$ rm -rf .git/modules/xxx
```

接着需要从 .gitmodules 中删除对应注册项    
然后从 .git/config 中删除对应注册项    
最后使用下面的命令删除缓存  
```shell
$ git rm --cached xxx
```