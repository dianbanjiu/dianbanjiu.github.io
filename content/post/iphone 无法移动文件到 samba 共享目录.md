---
title: "IPhone 无法移动文件到 samba 共享目录"
date: 2021-10-15T00:43:52+08:00
lastmod: 2021-10-15T00:43:52+08:00
tags: ["iphone","samba"]
author: "dianbanjiu"
comment: true
headPic: ""
---

为了在局域网设备之间进行文件共享，最近一直在折腾 samba，目前已经基本上算是搞好了。但是在这个过程中也遇到了一个比较蛋疼的事情，就是通过 PC  和 Android 都可以正常向共享目录写入文件，在 IPhone 端虽然可以正常查看共享目录中的文件，但是无法将 IPhone 上的文件移动/复制到共享目录中。每次通过 IPhone 向共享目录中写入文件都会出现下面的报错：  
![Imgur](https://i.imgur.com/D4PxRwz.png)


经过我在网上的一番冲浪之后找到了如下的解决方案：  

在 samba 配置文件的 [global] 中添加 `vfs objects = fruit streams_xattr`（当然也可以仅针对某些特定的共享目录单独配置），然后重启 samba 服务，注意是重启 restart，不是重新加载配置文件 reload。仅重新加载配置文件这个规则还是无法生效的。  


samba 中的 VFS（虚拟文件系统）可以通过一些扩展模块增强 samba 的功能，而上面这行配置启用了 VFS 的两个模块，其中对解决本次问题起主要作用的是 `fruit` 模块（可以的，果子🤣），这个模块主要是增强 samba 与 Apple 系统的兼容性。  

此外，如果从 IPhone 一次性向共享文件写入大量文件的话，可能会出现有些文件写失败的情况，这个问题暂时还没有找到解决方案，不过因为一次性写入大量文件需求也不是很多，暂时并不影响体验。


https://wiki.samba.org/index.php/Virtual_File_System_Modules