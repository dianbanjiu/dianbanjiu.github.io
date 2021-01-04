---
title: "压缩/解压缩"
date: 2019-10-06T08:26:21+08:00
lastmod: 2019-10-06T08:26:21+08:00
tags: ["Linux", "p7zip", "unarchiver"]
author: "dianbanjiu"
toc: false
---

在 Linux 下，除了最常用的 tar 归档命令之外，还有两个很好用的压缩/解压缩的工具，一个是 `unarchiver`，另一个是 `p7zip`。  

## Unarchiver

unarchiver 实际上是一个解压工具，其中包含了 `unar` 和 `lsar` 两个命令行程序。  

在 Arch Linux 下可以直接通过 pacman 命令安装 unarchiver 这个程序包，在其他的 Linux 发行版，如 Ubuntu 下，可以通过 apt 直接安装 unar 程序包。  

其中 `lsar` 是用来列出压缩包中的文件，`unar` 则负责把文件从压缩包中提取出来，两个命令的用法都很简单：  

```shell
$ lsar <archive name>
$ unar <archive name>
```

因为 zip 压缩包并不会包含有关系统编码的信息，所以在 Linux（系统编码一般为 utf-8）下解压一些非 utf-8（如 gbk 之类）的 zip 压缩包的时候经常会出现乱码的情况。而 unarchiver 的强大之处就在于**即使该压缩包的编码格式不是 utf-8,在解压后也不会出现任何乱码的情况**。   


## p7zip

p7zip 其实就是 7zip 在 Linux 下的命令行版本。  

在 Linux 下其实有很多好用的压缩工具，但是随之而来的一个问题就是：工具多了，涉及的命令也多了。选择 p7zip 的原因还是它支持更多的压缩格式。  

p7zip 中一般会包含三个命令，分别是 `7z`、`7za`、`7zr`，这三个命令的主要区别就是处理的范围：  

- 7z 支持 p7zip 范围内所有的压缩格式；
- 7za 支持的格式略少于 7z；
- 7zr 则只支持 7z 类型的压缩格式，且无法处理加密文件。

所以下面就只介绍 7z 相关的几个常用命令。  

创建压缩文件  
```shell
$ 7z a <archive name> <file name>   
```

创建压缩包的同时加密，-p 选项用来指定密码。切勿在 -p 选项后直接跟随密码，上面这种格式回车之后会提示你设置密码的  
```shell
$ 7z a <archive name> <file name> -p    
```

更新压缩文件  
```shell
$ 7z u <archive name> <file name>   
```

列出压缩文件中包含的文件  
```shell
$ 7z l <archive name>   
```

直接将压缩包中的内容提取到当前目录  
```shell
$ 7z e <archive name>   
```

全路径提取（不建议使用，可能会覆盖掉系统原有的一些文件）  
```shell
$ 7z x <archive name>   
```

提取文件到一个新的目录当中  
```shell
$ 7z x -o<floader name> <archive name>  
```
检查压缩包的完整性  
```shell
$ 7z t <archive name>  
```
