---
title: "为 Android Studio 配置 Flutter"
date: 2019-11-20T09:17:07+08:00
lastmod: 2019-11-20T09:17:07+08:00
tags: ["Android Studio", "Flutter"]
author: "dianbanjiu"
---

Android SDK 在国内是有官方镜像源的，可以直接下载就可以，如果在配置了代理之后再下载的话，反而很容易拖慢下载速度，甚至导致下载失败。  

Android Studio 现在可能都会直接默认附带了 Flutter 的插件，如果没有的话，先在 `Plugins` 中安装 Flutter 插件，然后在 SDK manager 中下载 sdk，可以下载几个最新版本之前的 SDK，因为 Android 的各个版本是向后兼容的。下载好 SDK 之后在系统变量当中添加 `ANDROID_HOME=/path/to/sdk`。  

最后可以再运行 `flutter doctor` 查看是否还有哪些问题出现。  

如果在运行程序的时候，发生 `Unable to locate a development device; please run 'flutter doctor' for information about installing additional components.` 的错误，可以修改 `File -> Project struture` 中的 SDK 版本，确定项目的 SDK 版本小于或者等于 AVD 的 SDK 版本。  
