---
title: "Arch Linux 安装微信"
date: 2022-04-09T12:57:54+08:00
lastmod: 2022-04-09T12:57:54+08:00
tags: ["linux","日常"]
author: "dianbanjiu"
comment: true
headPic: ""
---

AUR 中提供了多个微信的安装包，具体的可以参考 [微信的 Wiki 页](https://wiki.archlinux.org/title/WeChat_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))，我使用的是 [com.qq.weixin.deepin](https://aur.archlinux.org/packages/com.qq.weixin.deepin)，版本是 3.2.1.154

安装比较简单，直接通过 AUR 的包管理器安装即可

```bash
$ yay com.qq.weixin.deepin
```

为了让微信用起来更舒服，在安装完成之后还有一些需要调整的地方

**1、方块字**

当你打开微信之后，有可能会发现微信输入框的中文字全都是方块字，这一般是因为你的系统缺少了一些字体导致的，此处推荐安装微软雅黑（msyh）或者宋体（simsun）

如果从包管理器中没有找到这两个字体包，可以直接把 Windows `C:\Windows\Fonts` 下面的所有字体 copy 到 Arch Linux 的 `/usr/share/fonts/` 下，然后执行 `fc-cache -vf` 刷新字体缓存

**2、高分屏**

微信默认的 DPI 是 96dpi，在 1080p 下刚刚好，但是在一些高分屏上就会显得有点小，作者在 AUR 的评论中是通过执行 `WINEPREFIX=~/.deepinwine/Deepin-WeChat deepin-wine6-stable winecfg` 命令，然后在 `显示` 标签中调整 DPI，我尝试了一下，程序重启之后 DPI 又会恢复到默认的 96dpi

我现在是通过在 `/etc/environment` 中新增一行 `DEEPIN_WINE_SCALE=1.25` 解决的，因为我的显示器是 2k 的，所以此处设置的缩放是 1.25（**注意：需要重启一次系统才能使配置文件的修改生效**）