---
title: "Arch Linux更新后没有声音"
date: 2023-10-29T12:41:20+08:00
lastmod: 2023-10-29T12:41:20+08:00
tags: ["Linux"]
author: "dianbanjiu"
comment: true
headPic: ""
---

在昨晚 `pacman -Syu` 之后，今天早上突然发现电脑无法播放声音了  

我目前使用的音频方案是 pulseaudio+pulseaudio-bluetooth，修改各种配置无果之后，尝试换了一个新的方案 pipewire-pulse+wireplumber  

首先把电脑声音调整到一个稍小一点的音量，然后 `pacman -S pipewire-pulse wireplumber`    
安装过程中会提示需要移除 pulseaudio+pulseaudio-bluetooth，安装完成之后，声音立刻就开始播放了