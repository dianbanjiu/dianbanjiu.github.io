---
title: "Arch Linux 无法配对蓝牙耳机"
date: 2022-02-25T13:42:06+08:00
lastmod: 2022-02-25T13:42:06+08:00
tags: ["Linux"]
author: "dianbanjiu"
comment: true
headPic: ""
---

最近在尝试使用这台装了 Arch Linux 的笔记本连接蓝牙耳机的时候，发现总是无法成功配对  

因为蓝牙键盘是可以正常连接的，判断蓝牙驱动应该是正常的，所以大概率是缺少某些蓝牙音频驱动。然后查看蓝牙的日志发现如下报错：  

```shell
$ systemctl status bluetooth
...src/service.c:btd_service_connect() a2dp-sink profile connect failed for B8:D5:0B:D0:06:B0: Protocol not available
```

虽然在 Arch Linux Wiki 的 [Bluetooth_headset](https://wiki.archlinux.org/title/Bluetooth_headset_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)) 章节提示说已经默认支持 A2DP profile，但是看上面的日志应该是没有安装成功。  

安装 `pulseaudio-bluetooth` 之后再尝试配对蓝牙耳机，发现可以正常连接并且正常当作电脑扬声器了。