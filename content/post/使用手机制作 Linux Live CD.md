---
title: "使用手机制作 Linux Live CD"
date: 2020-01-02T12:44:45+08:00
lastmod: 2020-01-02T12:44:45+08:00
tags: ["Linux","Gparted"]
author: "dianbanjiu"
---

在 2020 年的第一天，因为想试试黑苹果，所以就从网上下载了安装镜像，在自己的笔记本上尝试了一下，不过因为各种原因最后安装失败了，惟一的系统也在尝试安装黑苹果的时候给格掉了。身边又只有这一台电脑，没办法，本来准备去网吧写个 Manjaro 的启动盘的，可惜木马病毒太多，在网吧杀了两个小时的毒，也没杀完，最后就放弃了。  

![Imgur](https://i.imgur.com/BXi7QoK.jpg)

在从网吧回寝室的路上，我想到了曾经在 distrowatch.org 上好像看到过 gparted，就是那个 Linux 下挺著名的分区工具，也有一个 Linux 发行版，而且我隐约记得它似乎是可以直接解压到 U 盘使用的。想到这我就迫不及待得尝试了一下，果然可以！！！  

### 预先准备  

- 两个 U 盘（一个用来装 gparted，另一个用来制作 manjaro 启动盘）。  
- 一个 otg（可以连接 U 盘到手机上的工具）。  
- 手机一部

### Game Start  

首先去 [gparted 下载页面](https://gparted.org/download.php) 下载 **Stable directory (.iso/.zip)(for i686, i686-pae and amd64 architectures)**。  

![Imgur](https://i.imgur.com/O0ld1kR.png)

下载下来的是一个 zip 压缩包，在手机上可以直接解压出来，然后利用 otg 把手机跟 U 盘连接起来，在复制文件之前，请先确保 U 盘的格式为 fat 格式。然后将解压之后的文件复制到 U 盘当中，gparted 本身并不是很大，解压后大概 357M，但是因为之后还要下载 manjaro 镜像文件，所以这两个 U 盘的容量都建议至少 4G 以上。  

至此，Gparted Live CD 已经制作完成，插到电脑上，进入电脑的引导界面，选择 Gparted 所在的 U 盘启动。  

建议你在复制完 Live CD 所需的文件之后，同时下载好需要的镜像文件，比如我选择的 manjaro，[manjaro 下载地址](https://mirrors.tuna.tsinghua.edu.cn/osdn/storage/g/m/ma/)，这是清华的镜像源，国内下载速度很快，包括官方版本和社区版本以及之前的历史版本。在手机上下载好之后也复制到 Live CD 当中。  


引导加载完成之后后你可以看到下面这个画面，选择第一项回车进入。  

![Imgur](https://i.imgur.com/d27drNa.png)

之后的键盘模式选择 `Don't touch keymap`：  

![imgur](https://i.imgur.com/O55JJzQ.png)

后面的两项直接回车，第一个回车会直接选择默认的 US English 为默认的语言，第二个回车是以图形界面加载桌面。  

![Imgur](https://i.imgur.com/LtegLuc.png)

加载完成即可进入桌面了：  

![Imgur](https://i.imgur.com/rTqcked.png)

插上你的另外一个 U 盘，使用 `fdisk` 命令确定新插入 U 盘的设备名，然后就可以使用 dd 命令将镜像写入 U 盘了。  

![Imgur](https://i.imgur.com/hOsO0gV.png)  

最后关机拔掉 gparted Live CD 的 U 盘，开机重新进入新的引导盘。之后的就可以根据新的安装要求安装新系统了。  