---
title: "Vim 执行多条命令"
date: 2020-09-20T22:37:02+08:00
lastmod: 2020-09-20T22:37:02+08:00
tags: ["vim"]
author: "dianbanjiu"
---

最近又开始折腾自己的 vim 配置了，目前在 github 有 [一份存档][1]。为了方便还顺便也给这个 .vimrc 添加了一个安装的脚本。在写安装脚本的过程中遇到了一点小小的问题。  

我的 vim 是使用 vim-plug 来管理插件的，所以在写好配置文件后还需要进 vim 执行安装的指令，但是我这么懒，肯定是能让脚本自己跑的就交给脚本啦。  

在使用 `vim --help` 查看帮助手册之后发现 vim 有一个 `-c` 参数，支持在打开 vim 后的文件后执行对应的命令。具体到我的这个例子中的使用方法如下：  
```shell
$ vim -c PlugInstall # 进入 vim 中执行 PlugInstall 命令
$ vim -c "PlugInstall | p | p" # 同 Linux 的管道，在安装完插件之后，退出插件安装窗口，然后再退出 vim 主窗口
```





----
[1]: https://github.com/dianbanjiu/dotvimrc
