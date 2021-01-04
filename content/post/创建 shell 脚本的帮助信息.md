---
title: "创建 Shell 脚本的帮助信息"
date: 2020-07-22T23:28:50+08:00
lastmod: 2020-07-22T23:28:50+08:00
tags: ["linux", "shell"]
author: "dianbanjiu"
toc: false
---

原文链接 [英]：[https://samizdat.dev/help-message-for-shell-scripts/](https://samizdat.dev/help-message-for-shell-scripts/)  

有很多种方式可以获取一个 shell 脚本的帮助信息（当然得是在你确实在脚本中写了帮助信息的情况下），你可以使用 cat 或者 echo 来获取这些帮助信息。但是这种方式显得太过暴力，我们其实有更加优雅的方式。  

假设你所有的脚本信息都写在文件的开头。  

```shell
#!/bin/bash
### 
### my-script -- do something well
### 
### Usage:
###   my-script <input> <output>
### 
### Options:
###   <input> Input file to read
###   <output>  Output file to write, use - for stdout
###   -h  show help message
```

接着我们需要写一个方法来获取这些帮助信息：  
```shell
help(){
    sed -rn 's/^### ?//;T;p' "$0"
}
```

其中，`$0` 代表你所执行的这个脚本文件的文件名。  
这个方法中主要使用的命令就是 sed，sed 是一个流编辑器，会基于你给定的一系列规则来处理给定的数据。  

- `-n` —— 表示不会打印模式空间里的内容。  
- `-r` —— 表示可以在脚本中使用 扩展的正则表达式。  
- `s` —— 代表替代模式。  
- `/` —— 代表规则的开始/结束。  
- `^### ?` —— 按照正则匹配以 ### 开头的包含一个或者零个空格的字符串。  
- `//` —— 这两个 “/” 中的内容会替换掉前面匹配到的内容。  
所以 `sed -rn 's/^### ?//` 就表示会以 「空字符串」 替换掉所有 「行首的三个#以及其后可能有的一个空格」。  
- `T` —— 表示如果 sed 编辑器没有成功完成替换，则跳转到 sed 命令的最后。  
- `p` —— 代表打印替换的结果。  

最后用空参数或者 `-h` 来调用这个方法：  
```shell
if [[ $# == 0 ]] || [[ $1 == "-h" ]]; then
  help
  exit 1
fi
```

如果你习惯在一个脚本的开头做注释的话，可以试试这种方法。