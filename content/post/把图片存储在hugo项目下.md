---
title: "把图片存储在hugo项目下"
date: 2022-12-04T02:50:07+08:00
lastmod: 2022-12-04T02:50:07+08:00
tags: ["hugo","github"]
author: "dianbanjiu"
comment: true
headPic: ""
---

之前博客的图片一直是放在各种图床上面的，一开始是 imgur，虽然是免费的，但是在国内访问是一个很大的问题，所以后来就迁移到了 sm.ms，迁移完成之后在国内国外网络都可以比较顺畅的访问了  

不过在迁移的过程中发现一个问题，图片在上传到这些图床之后，他们都会使用自己的规则重新生成一个对外的名字，虽然我理解他们这么做的原因，但是这会极大的增加我以后迁移图床的难度。所以我就开始考虑把所有的图片都放在能够自定义文件名的地方，这样不管是在博客中引用还是以后迁移起来都很简单，只需要使用正则统一修改图片的前缀地址即可。目前采用的是直接把所有图片都放到博客的目录下面，跟博客一同编译  

我现在的博客使用的是 hugo，hugo 在 build 站点的时候会把根目录下 static 目录里面的内容直接扔在生成的静态站点的根目录下面，所以可以通过这种方式来引用图片  
1. 在 static 目录下面创建 img 目录（static 不存在的话手动创建）  
2. 把图片放在 static/img 下面  
3. 在引用图片的时候使用 baseUrl/img/picturename.extra 进行引用  

比如我放了一张名为 doge.png 的图片到 static/img 下面，我的 baseUrl 是 https://dianbanjiu.github.io，那我就可以通过 `![](https://dianbanjiu.github.io/img/doge.png)` 来引用这张图片  

如果配置了自己的域名，也可以使用自己的域名进行访问，比如我给自己的博客配置了 https://www.dianbanjiu.com 的域名，那我就可以通过 `![](https://www.dianbanjiu.com/img/doge.png)` 在博客中引用这张图片  

当图片尚未上传到 github 之前，在本地预览图片时，可以把图片地址修改为本地的地址来访问，比如我在本地预览 hugo 时起的服务地址是 http://localhost:1313，那在预览时把图片的地址改为 `![](http://localhost:1313/img/doge.png)` 即可。但是要记得在把文章上传到 github 之前把地址改回自己的外网域名  

![](https://www.dianbanjiu.com/img/doge.png)  

我现在的域名是配置了 Cloudflare 加速的，把图片也放在博客目录下之后，相当于也顺便给图片也开了加速，可以说是一举两得了😂