---
title: "把图片存储在hugo项目下"
date: 2022-12-04T02:50:07+08:00
lastmod: 2022-12-04T02:50:07+08:00
tags: ["hugo","github"]
author: "dianbanjiu"
comment: true
headPic: ""
---

现在博客里面使用的图片都是放在图床上面的，但是担心哪天平台访问不了或者平台倒闭了，迁移图片的时候会成为一件麻烦事，所以就想把图片放在博客的目录里面，这样的话迁移起来成本就会低很多，最多只需要把访问的域名前缀更换一下就可以了。下面就试一下把图片放在博客的 github 仓库里面能否生效吧  

操作方式很简单：  
1. 图片放在 static 目录下面
2. 通过博客地址加图片名的方式引用到博客里面

比如我在 static 目录下新增了一张名为 doge.png 的图片，如果我需要在本地进行效果预览的话，图片的引入地址就是 `![](http://localhost:1313/doge.png)`，预览之后没有问题，需要推送到 github,那预览地址就需要修改为 `![](https://dianbanjiu.github.io/doge.png)`，如果已经为 github pages 配置了自己的域名，也可以直接使用自己的域名 `![](https://www.dianbanjiu.com/doge.png)`  

效果就会像下面这样  
![](https://www.dianbanjiu.com/doge.png)