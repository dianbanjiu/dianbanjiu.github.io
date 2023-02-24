---
title: "在非Apple的系统上安装苹果字体"
date: 2023-02-24T23:43:48+08:00
lastmod: 2023-02-24T23:43:48+08:00
tags: ["apple", "font"]
author: "dianbanjiu"
comment: true
headPic: ""
---

这里主要说的是苹果的 SF 字体，在 Linux 上或者 Windows 上的安装流程大同小异  

首先需要准备解压工具，我这里用到的是 [7-Zip](https://www.7-zip.org/)，Windows 和 Linux 都可以使用自己的包管理器从源里面安装，当然也可以点击前面的链接去官网下载安装包进行安装，具体的安装步骤这里就不赘述了，下面默认你已经安装好了 7-Zip  

_为了方便起见，我下面统一使用了命令行的方式进行解压，如果你用的是 7-Zip 的图形界面进行下面的解压操作也是完全没有任何问题的，只是解压出来的文件位置可能会有一点区别_  

接着需要在[苹果开发者](https://developer.apple.com/fonts/)平台下载字体，下载无需登录开发者账号，此处我以 SF Pro 为例   

点击 SF Pro 下面的 `Download SF Pro`，下载完成之后你会得到一个名为 `SF-Pro.dmg` 的文件，dmg 是苹果的一种打包方式，既然能打包就能解包，7-Zip 刚好就可以做到这件事情  

打开终端（Terminal/CMD），使用下面的命令解压这个文件  
```shell
# Windows
7z e .\SF-Pro.dmg

# Linux
7z e ./SF-Pro.dmg
```

解压完成之后你应该会得到下面这几个文件  
- .HFS+ Private Directory Data_
- [HFS+ Private Data]
- SFProFonts
- .background.png
- .DS_Store
- SF Pro Fonts.pkg

接着继续解压 `SF Pro Fonts.pkg` 这个文件  
```shell
# Windows
7z e '.\SF Pro Fonts.pkg'

# Linux
7z e './SF Pro Fonts.pkg'
```

解压完成之后，在当前目录会多出一个叫做 `Payload~` 的文件，继续解压这个文件  
```shell
# Windows
7z e .\Payload~

# Linux 
7z e ./Payload~
```

解压完成之后你会在当前目录下看到一堆 .otf 或者 .ttf 结尾的字体文件  

Windows 用户可以用文件管理器打开这些字体所在的目录，选中你想安装的字体，点击右键，在右键菜单中选择安装即可。或者也可以打开 设置->个性化->字体，把想要安装的字体拖到 `拖放以安装` 的框中即可  

Linux 用户  
- 如果是给自己当前用户安装，可以创建 `~/.local/share/fonts/` 目录
- 如果是给系统上所有用户安装，需要创建 `/usr/local/share/fonts` 目录

在创建好的 fonts 目录下新增 otf 和 ttf 两个文件夹，分别把想要安装的 ttf 字体和 otf 字体复制到对应的目录中，最后使用 `fc-cache` 命令刷新一下字体即可  
