---
title: "创建Windows服务"
date: 2025-12-08T17:38:50+08:00
lastmod: 2025-12-08T17:38:50+08:00
tags: ["Windows","Syncthing"]
author: "dianbanjiu"
comment: true
headPic: ""
---

## 安装配置
最近在折腾内网的文件同步，选用了 Syncthing 的方案，Syncthing 本身的基础安装配置比较简单

我是通过 scoop 管理一些没有 GUI 的程序或者工具的，Syncthing 已经位于 scoop 的主库中，可以通过下面的命令直接安装  
```poershell
scoop install syncthing
```

顺便我还安装了一个提供 Syncthing 在 Windows 任务栏图标的工具，也可以通过 scoop 安装  
```powershell
scoop install syncthingtray
```

安装完成之后，直接在终端执行 `syncthing.exe` 启动服务器模式，接着打开 http://127.0.0.1:8384 进行一些简单的设置即可，8384 是 Syncthing 的默认端口，也可以在配置文件中进行指定（scoop安装的配置文件位于 `scoop\apps\syncthing\current\config\config.xml` 中，直接搜索 8384 修改为自己需要的端口）  


## 创建 Windows 服务
因为 Syncthing 本身是一个后台服务，我本来是想创建一个任务计划程序的，这种方式虽然可以在电脑启动之后成功启动 Syncthing，但是会启动一个终端窗口，如果关闭这个终端窗口 Syncthing 也会停止运行，虽然可以通过 CMD 或者 PWSH 脚本再包装一层，不过我还是想再找找有没有什么更加稍微优雅一点的方案  

Syncthing 关于 [Windows 服务](https://docs.syncthing.net/users/autostart.html#windows) 部分推荐的是 nssm，研究了一下 nssm 虽然可以使用，但是已经很久没有更新了，官网稳定版下载地址是 2014 年的版本，预览版是 2017 年的版本，所以找了一下还在稳定更新的替代品，最后找到了 [WinSW](https://github.com/winsw/winsw) 这个工具

下面是关于 WinSW 的使用方式  

**1. 从 [GitHub Releases](https://github.com/winsw/winsw) 下载需要的可执行文件**  
**2. 在任意目录创建需要的 Windows 服务配置，比如 Syncthing.xml。** _(有需要可以参考文尾我的服务配置)_  
**3. 然后把之前下载好的 WinSW.exe 复制到当前目录，并将其重命名为 Syncthing.exe。**（因为 WinSW 在自行的时候不需要指定 xml 配置文件，而是直接通过文件名匹配同名的 xml 配置文件）  
4. 接着就可以通过下面的命令创建并启动 Windows 服务了
```powershell
# 安装服务
./Syncthing.exe install

# 启动服务
./Syncthing.exe start
```

Windows 服务是开机自启的，所以下次电脑启动的时候 Syncthing 服务也会一起运行  

## 安卓端同步

因为官方的 syncthing 程序已经停止维护了，所以安卓端目前推荐的软件是一个三方的软件 syncthing-fork，可以在 [Google play](https://play.google.com/store/apps/details?id=com.github.catfriend1.syncthingandroid) 下载到它  

安装好之后，在设备页面，点击右上角的添加设备按钮扫描并添加 Windows 的设备标识二维码，然后需要在两端的 WebGUI 里面分别同意添加设备，最后就可以设置需要进行同步的文件夹了  

![](/img/Syncthing-fork-add-device-icon.jpg)

## 相关资料
- [Syncthing 官网地址](https://syncthing.net/#)
- [Syncthing 官方文档](https://docs.syncthing.net/#)
- [WinSW Github地址](https://github.com/winsw/winsw)
- [我的Windows服务配置](https://github.com/dianbanjiu/WindowsServices)
