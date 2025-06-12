---
title: "Windows硬件监控工具——TrafficMonitor"
date: 2025-06-12T21:33:04+08:00
lastmod: 2025-06-12T21:33:04+08:00
tags: ["Window"]
author: "dianbanjiu"
comment: true
headPic: ""
---

平时在 Windows 下看电脑资源占用情况的时候，经常需要打开任务管理器切来切去，感觉有点麻烦。最近在 V2EX 和 Reddit 的 desktop 社区找灵感的时候，发现了 [TrafficMonitor](https://github.com/zhongyang219/TrafficMonitor) 这个工具  

- 它可以在悬浮窗和任务栏展示当前系统资源的占用情况  
- 内置了网卡、CPU、显卡、内存和硬盘的监控
- 悬浮窗支持换肤
- 支持通过插件扩展更多功能

因为平时不喜欢使用悬浮窗，所以现在仅启用了任务栏模式，展示了上传下载、CPU、内存使用率，显卡使用率和显卡温度。显示效果是这样子的  
![实际效果](/img/trafficmonitor-on-taskbar.png)  

### 关于安装 
可以选择直接从项目的 [Github release](https://github.com/zhongyang219/TrafficMonitor/releases/latest) 进行下载  
也可以选择通过包管理器，比如 scoop 进行安装（scoop install trafficmonitor）  

### 关于如何启用
打开软件之后，默认是以主窗口，也就是悬浮窗的方式进行展示的，如果需要启用任务栏模式，需要在右下角的任务图标中，在 TrafficMonitor 的图标上点击右键，并选中 “**显示任务栏窗口**”  

### 关于如何启用显卡监控
默认情况下工具只会显示网速、CPU、内存，如果需要添加显卡的支持，需要在工具的 **设置（或者叫选项）->常规设置->硬件监控**中启用更多的监控选项  
![启用更多硬件设置](/img/trafficmonitor-options.png)