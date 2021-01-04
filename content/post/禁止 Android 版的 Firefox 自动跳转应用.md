---
title: "禁止 Android 版的 Firefox 自动跳转应用"
date: 2019-11-23T21:48:35+08:00
lastmod: 2019-11-23T21:48:35+08:00
tags: ["Android", "Firefox"]
author: "dianbanjiu"
toc: false
---

Android 版的 Firefox 在打开像是知乎回答，bilibili 的视频，小米商城等的时候，会自动跳转至手机上对应的 app 里面去，可是如果你的手机上如果没有安装这些软件的话，Firefox 就会跳转至以 app 名为协议名的一个网址，类似 `zhihu://...`，而这个网址一般是无法识别的，这时候就会出现下面的情况。  

![](https://i.loli.net/2019/11/23/oVcpgzalmKjAH34.jpg)  

要解决这个问题，可以按照下面的步骤：  

1. 在 Firefox 的地址栏输入 `about:config`，  
2. 搜索 `network.protocol-handler.external-default` 字段，  
3. 将该项对应的值由 `true` 改为 `false` 即可。  


---  
参考链接：  
https://www.mobibrw.com/2018/12856  
