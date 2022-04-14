---
title: "Linux 访问 Windows 共享目录"
date: 2022-04-14T16:36:33+08:00
lastmod: 2022-04-14T16:36:33+08:00
tags: ["linux","windows","samba"]
author: "dianbanjiu"
comment: true
headPic: ""
---

因为工作需要，需要在电脑上安装一个 VPN 程序，但是官方并没有提供 Linux 版本，通过 wine 虽然能安装成功，但是界面总是显示不出来。实在不想折腾了，就直接装了一个 Windows 的虚拟机，有一些需要编辑的文件就通过共享的方式在宿主机上进行编辑

Linux 上 samba 的安装以及 Windows 文件共享的设置倒是一帆风顺，没出现什么问题，不过在 Linux 下获取 Windows 共享目录的时候却出现了异常

```bash
# 正常打印出了 users 目录下的内容
$ smbclient -c "dir" //ip/users -U user%passwd
.          DR     0    Wed Apr 13 17:52:49 2022
..         DR     0    Wed Apr 13 17:52:49 2022
Default    DHR    0    Wed Apr 13 17:52:49 2022

# 继续打印用户目录下的内容时出现异常
$ smbclient -c "dir" //ip/users/test -U user%passwd
tree connect failed: NT_STATUS_BAD_NETWORK_NAME
```

经过一番冲浪之后大概确定原因是 Windows 使用的 samba 版本是 v1，而 Linux 下的 samba 版本是 v4，v4 并不直接向下兼容，`smbclient` 又不支持指定 samba 版本，不指定 samba 版本就无法直接打印出共享目录中的文件，死循环

不过通过 mount 使用 cifs 格式挂载共享目录的时候是可以指定 samba 版本的，修改命令，通过下面的方式可以将共享目录挂载到本地

```bash
sudo mount -t cifs -o username=windows_user,password=windows_user_password,vers=1.0,gid=$GID,uid=$UID //ip/共享路径 /home/test/samba_mount_path
```

- 这里注意把 windows_user、windows_user_password、ip、共享路径修改为 Windows 对应的信息，/home/test/samba_mount_path 是在 Linux 下的挂载点
- vers=1.0 指定的是 samba 的协议版本

因为一般情况下都是由 root 用户来进行 mount 操作的，挂载到本地的目录所属用户及用户组也是 root，对其中的文件进行操作时需要添加 sudo 前缀命令，为了让普通用户能直接操作，此处需要指定 gid=$GID,uid=$UID 两个参数，分别代表当前用户所属用户组的 ID 和当前用户的 ID