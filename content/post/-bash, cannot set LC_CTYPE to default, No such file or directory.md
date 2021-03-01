---
title: " Bash, Cannot Set LC_CTYPE to Default, No Such File or Directory"
date: 2020-07-30T22:38:35+08:00
lastmod: 2020-07-30T22:38:35+08:00
tags: ["Linux", "Macos"]
author: "dianbanjiu"
---

最近使用 MacOS 的终端通过 ssh 连接 Linux 服务器的过程中，出现了一点问题，每次连接上之后，都会出现 `-bash, cannot change locale, No such file or directory`，整段错误代码没记住，但大概就是这么个意思。  

最开始我以为这是服务器那边的问题，所以就没怎么在意，毕竟登录之后一个 `Ctrl-l` 就啥都没了。直到前几天因为 activemq 获取中文信息乱码的时候我才注意到这个问题的重要性。  

我在服务器上搭建了一个 activemq 的中间件，然后通过本地向 activemq 发送了一些信息，如果信息中包含了中文，那在 activemq 的 Web console 中中文就会显示乱码。  

可是按理说我服务器的字符编码是 `UTF-8`（重点，后面要考），avtivemq 的 webapps 用的编码也是 `utf-8`，向 activemq 发送信息的程序是用 golang 写的，golang 默认的编码格式也是 `utf-8`，编码格式都是统一的，为什么会出现乱码呢。  

在尝试过修改代码发送的信息为各种编码格式，以及修改 activemq 的 webapps 编码格式仍旧无果之后，我就怀着死马当活马医的心态修改了 activemq 运行时的字符编码环境为 `zh_CN.UTF-8`，然后奇迹发生了，中文乱码消失了，我又试了一下 `en_US.UTF-8`，也可以正常显示中文。  

```shell
$ env LC_CTYPE=zh_CN.UFT-8 activemq start
```

后来在网上搜原因的时候无意中看到了一篇文章是教别人如何修改 MacOS 的终端在连接到服务器时不传递自己的 `locale`，[[原文链接]](https://www.cyberciti.biz/faq/os-x-terminal-bash-warning-setlocale-lc_ctype-cannot-change-locale/) ，我就试着改了一下，修改完之后我再登录到服务器，这次没有了之前的 `-bash, cannot change locale, No such file or directory` 的错误，查看服务器的字符编码 `en_US.UTF-8`。  

**握了个大草**  

突然就想明白了为什么在 MacOS 下使用终端登录服务器会报 `-bash, cannot change locale, No such file or directory` 的那个错误了。  

在 Linux 下的 UTF-8 字符集有很多，比如 zh_CN.UTF-8、en_US.UTF-8，但唯独没有独立的叫做 UTF-8 的字符集，因为在 Linux 中，UTF-8 是作为一种字符编码的规范，而 zh_CN.UTF-8 则是按照 UTF-8 规范实现的字符集。  

那问题来了，为什么我之前在终端中查看的字符编码会是 UTF-8 而非 zh_CN.UTF-8 之类呢？结合我之前看到的那篇文章，其实是因为 MacOS 的终端在连接到服务器的时候会把自己的字符编码环境给带到服务器去，MacOS 的字符集是苹果自己实现的一套符合 UTF-8 规范叫做 UTF-8 的字符集，但是 Linux 上并没有这套字符集，虽然 MacOS 终端带到服务器上的编码环境仅在它这个终端会话中有效，但是在这个会话中所有运行的程序都会去调用这个不存在的字符集，所以才出现了 `-bash, cannot change locale, No such file or directory` 的问题以及 activemq 中文乱码的现象。  

> 修改方式：在 MacOS 的 终端(Terminal)->偏好设置->高级 中，取消勾选在启动时设置 locale 环境变量。  
> 另外还可以通过在 `/etc/ssh/ssh_config` 中注释掉 `SendEnv LANG LC_*`，也可以达到同样的效果。  
> 如果使用第一种方式可能会导致 MacOS 的终端中文无法显示，所以还是更推荐第二种方式。  

最后简单说一下 Linux 字符编码相关的内容：  

Linux 的字符编码环境是通过多个 LC_* 环境变量来定义的，比如 LC_CTYPE(Linux 的字符显示方式)，LC_TIME(时间和日期的格式)...  

既然是变量，那都是可以随意修改的，即使是系统中不存在的那在设置的当时也不会报错，除非在某些显式调用时，比如使用 `locale` 命令查看系统的各种字符编码的格式时，如果你定义了错误的字符格式环境，那 `locale` 就会提示类似 `Cannot set  LC_CTYPE to default, No such file or directory` 的错误。不过其他一些隐式的调用，比如 activemq 执行时就不会自动检查这些变量的正确与否。  

所以希望大家不要忽视遇到的任何一个 warning 以及 error，指不定就是以后某个的 bug 突破口。

参考链接：  
[1]: [Arch Wiki -- locale](https://wiki.archlinux.org/index.php/Locale)