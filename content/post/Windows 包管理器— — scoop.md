---
title: "Windows 包管理器 —— Scoop"
date: 2022-04-21T00:08:16+08:00
lastmod: 2022-04-21T00:08:16+08:00
tags: ["windows","scoop"]
author: "dianbanjiu"
comment: true
headPic: ""
---

我很喜欢 Linux 软件的安装方式，如果一个软件存在于软件源中，只需一行命令就可以安装好这个软件，而且大多数时候也不用担心下到什么流氓程序。所以当我有了一台 Windows 的电脑之后，我也尝试去寻找 Windows 下是否也有类似的工具，最终，我找到了 Scoop

## 安装

官网地址： [https://scoop.sh/](https://scoop.sh/)

首先确保系统已经安装好下面两个程序：

1. PowerShell 的版本 >= 5
2. .NET Framework 的版本 >=4.5

通过管理员身份启动一个终端窗口，依次执行下面的命令

```powershell
Set-ExecutionPolicy RemoteSigned -scope CurrentUser

iwr -useb get.scoop.sh | iex

```

## 常用操作

- 查看帮助信息

```powershell
scoop help
```

- 查看某个子命令的帮助信息，如 update

```powershell
scoop help update
```

- 查询软件包，如 aria2

```powershell
scoop search aria2
```

- 查看某一个软件包的主页，如 aria2

```powershell
scoop home aria2
```

- 安装软件包，如 aria2

```powershell
scoop install aria2
```

- 更新某个软件包，如 aria2

```powershell
scoop update aria2
```

- 更新软件源

```powershell
scoop update
```

- 更新所有已安装的软件包

```powershell
scoop update -a
```

- 展示软件包状态以及可更新的软件包

```powershell
scoop status
```

- 清除历史包缓存

```powershell
scoop cache rm *
```

- 移除历史旧版本

```powershell
scoop cleanup *
```

- 启用 extras 软件源

主软件源中包含的软件比较有限，可以通过下面的命令来启用 extras 软件源。main 和 extras 软件源都是由 scoop 官方维护的，由其他用户维护的软件源可以通过类似的方式进行添加，只需要把 extras 替换为对应软件源的链接即可

```powershell
scoop bucket add extras
```

- 移除某个软件源，如 extras

```powershell
scoop bucket rm extras
```

## 进阶操作

- 设置代理

```powershell
scoop config proxy 127.0.0.1:8889
```

- 通过 aria2c 启用多线程下载

aria2 安装之后，Scoop 默认就会启用 aria2 的多线程下载

```powershell
scoop install aria2 
```

启用 aria2 之后，scoop 默认会打印一些警告信息，虽然很有道理，但是我选择关闭提示

![https://i.imgur.com/1pJJ51V.png](https://i.imgur.com/1pJJ51V.png)

```powershell
scoop config aria2-warning-enabled false
```

- 关闭 aria2 多线程下载

```powershell
scoop config aria2-enabled false
```

## 常见问题

### 在更新/下载程序包的时候出现类似 `Invoke-WebRequest : 无法连接到远程服务器` 的报错

![https://i.imgur.com/psX044t.png](https://i.imgur.com/psX044t.png)

这很有可能是 scoop 对一些系统目录没有访问权限，而这些程序包的安装又需要对一些系统目录进行操作，这个时候可以 `管理员身份` 运行终端，然后重新执行安装/更新命令  

### scoop 安装程序失败之后，在 status 命令里面会一直输出错误信息  
```powershell
PS > scoop status
Scoop is up to date.

Name     Installed Version Latest Version Missing Dependencies Info
----     ----------------- -------------- -------------------- ----
snipaste                                                       Install failedManifest removed
```  

可以通过 uninstall 这个软件包来移除这个提示  
```powershell
PS > scoop uninstall snipaste
ERROR 'snipaste' isn't installed correctly.
Removing older version (1.16.2).
'snipaste' was uninstalled.

PS > scoop status
Scoop is up to date.
Everything is ok!
```