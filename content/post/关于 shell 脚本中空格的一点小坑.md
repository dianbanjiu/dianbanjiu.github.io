---
title: "关于 Shell 脚本中空格的一点小坑"
date: 2020-07-27T00:23:54+08:00
lastmod: 2020-07-27T00:23:54+08:00
tags: ["shell", "Linux"]
author: "dianbanjiu"
toc: false
---

最近为了结合 gilab runner 自动化打包，写了不少的 shell 脚本，虽然都是比较简单的内容，但还是踩了不少的坑。最近的一个就是有关字符串的问题。  

在很多场景下我们可能都需要给 shell 脚本指定一些参数，或者在 shell 脚本执行过程中输入一些内容，这些内容大多是字符串的形式，比如下面这个创建文件的脚本：  
```shell
$ cat touchFile.sh
#!/bin/bash
echo $1
touch $1
$ bash touchFile.sh "hello world"
```

你觉得上面的代码会输出什么以及会创建哪些文件呢？输出 `hello world`，创建 `hello\ world`？  

正确答案是输出 `hello world`，并创建 `hello` 和 `world` 两个文件。  

因为 shell 在从命令行获取到由双引号包裹的内容后，一般会将其识别为一个字符串，即便引号中有空格。但是在获取到含有引号的字符串之后，shell 就会将这个双引号给自动去掉了，所以 `echo hell world` 可以得到预期的结果，但到了 `touch hello world` 就相当于创建了 hello 和 world 两个文件，这显然不符合我们的预期，那么该如何解决这个问题呢？  

首先我们需要知道，Linux 很多命令在获取参数的时候通常都会以空格为分隔符，但是如果空格是在两个双引号中间的话，shell 就会忽略这个空格，而把这个空格作为引号字符串的一部分。所以我们只需对上面的代码稍作修改即可。  
```shell
$ cat touchFile.sh
#!/bin/bash
echo $1
touch "$1"
```

这样在执行脚本就可以成功创建带有空格的文件名了。当然你也可以将其应用到 shell 各种需要获取带空格字符串的地方，而不仅仅是创建文件。  