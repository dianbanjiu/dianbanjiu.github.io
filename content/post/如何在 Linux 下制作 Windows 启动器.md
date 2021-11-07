---
title: "如何在 Linux 下制作 Windows 启动器"
date: 2021-11-06T12:24:27+08:00
lastmod: 2021-11-06T12:24:27+08:00
tags: ["linux","windows"]
author: "dianbanjiu"
comment: true
headPic: ""
---

在 Linux 下制作 Windows 启动盘，有三种常用的方式：dd、WoeUSB、Ventoy。  

## WoeUSB

WoeUSB 可以使用 `pip3 install WoeUsb-ng` 进行安装。  

WoeUSB 我以前用的时候还可以，但是这次也不知道为什么，虽然可以正常写入镜像，但是做的启动盘始终不能进入 Windows 引导界面，换了好几个版本的 Windows10 镜像都是一样的。

## Ventoy
Ventoy 的话，用起来比较简单，首先去 [Ventoy 官方下载页面](https://www.ventoy.net/en/download.html) 下载压缩包，解压后在终端执行 `sudo bash /path/to/VentoyWeb.sh` 启动服务，接着访问启动信息中展示的本地端口，像我这里就是 24680 端口。在页面上选择你用来做启动盘的设备，然后点击安装即可。
![](https://i.imgur.com/cH5zfZ3.png)  
![](https://i.imgur.com/F0b0m9S.png)
上面之所以需要使用 sudo 来运行，一方面是因为 Ventoy 需要获取你的 PC 上现在都有哪些存储设备，另一方面是因为安装的过程中需要对设备进行格式化。  

安装好 Ventoy 之后，你只需要把各种 ISO 镜像文件扔到安装 Ventoy 的 U 盘中即可。  

如果你现在看一下这个 U 盘的分区结构你可以发现 U 盘上会有两个分区，一大一小，后面很小的那个分区放的就是 Ventoy 的引导文件，前面这个分区是用来放你的镜像文件的，理论上你把前面这个分区用来存放其他文件也是完全没问题的。

这次电脑成功进入了 Ventoy 的引导界面，但是在我选择了 ISO 文件之后，Ventoy 提示我缺少一些东西 Windows 引导失败（我裂开了）我又重新下载了最新的 Windows10 镜像，还是会出现同样的问题，可能是需要安装一些其他的 Ventoy 插件或者是 Ventoy 对于 AMD 的支持不太好？，但是我没有继续折腾了。  

## dd
这一 Part 的标题虽然是 dd，但是你也可以直接使用复制粘贴来完成，这个方案相比上面两个稍微麻烦一点，但是并不复杂，而且这也是我唯一测试成功的方案😭

下面涉及到对磁盘的分区操作，熟悉终端工具的可以直接用 fdisk 跟 mkfs 完成，习惯 GUI 的可以使用 gparted 完成。

下面是使用 dd 创建 Windows 启动盘的步骤：  
1. 备份好 U 盘中的数据
2. 删除 U 盘中所有的分区
3. 创建 GPT 分区表
4. 在 U 盘上创建两个分区，其中第二个分区只需要留 300MB 的空间即可，剩下的可以全部给第一个分区（**注意：大的分区在前面，里面会存放 Windows 镜像中的文件，小的分区在后面，里面会放引导文件，两个分区的顺序一定不可以错：前大后小**）  
   1. 第一个分区格式化为 NTFS 格式
   2. 第二个分区格式化为 Fat32 格式
5. 直接复制或者使用 dd 将 Windows 镜像中的文件写入到 U 盘的第一个分区
6. 下载由 Rufus 提供的 [uefi-ntfs.img](https://github.com/pbatard/rufus/blob/master/res/uefi/uefi-ntfs.img) UEFI 引导镜像
7. 复制 uefi-utfs.img 中的所有文件到 U 盘的第二个分区中

待所有文件都写入完成之后，在 BIOS 中将 U 盘移动到启动顺序的第一个，然后启动电脑，应该就可以看到 Windows 的引导界面了。