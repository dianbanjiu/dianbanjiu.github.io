---
title: "使用 Rclone 挂载 OneDrive"
date: 2022-03-16T11:47:59+08:00
lastmod: 2022-03-16T11:47:59+08:00
tags: ["linux","rclone","onedrive","日常"]
author: "dianbanjiu"
comment: true
headPic: ""
---
## 安装

因为 OneDrive 没有提供官方的 Linux 客户端，所以为了方便起见，我选择直接通过 rclone 将 OneDrive 挂载到本地，这样就可以在本地直接对 OneDrive 的文件进行管理

首先需要安装 `rclone` 程序本体，我使用的是 Arch Linux，可以直接通过 pacman 进行安装

```bash
$ sudo pacman -S rclone
```

## 配置

接着就可以使用 `rclone`开始配置自己的 OneDrive 了，下面是我实际配置过程的输出

开始之前，先说几个需要注意的地方：

1. 类似下面的这种选择，可以选择输入前面的序号，如 1，也可以选择输入后面的文字，如 fichier

```
1 / 1Fichier
   \ "fichier"
```

2. 在选择 `region` 地区的时候，一定确定好自己要挂载的 OneDrive 是哪个版本的，如果你使用的是世纪互联版本的 OneDrive 那就选择 `cn`，否则一般选择 `global` 就可以， `us` 是美国政府版（应该没人真的是这个吧）
3. 在 `Use auto config?` 阶段，如果你选择了 `Y` ，rclone 会打开一个浏览器页面，需要你登录并授权 OneDrive 的访问权限。有些教程可能会让你使用 `rclone authorize` 来单独获取 OneDrive 的 token 及授权，不过两者其实区别不大，唯一的区别就是自动配置模式下可以自动帮你获取到 token，而后者需要你使用另一个命令获取 token，在配置 OneDrive 的时候手动粘贴进去

```bash
$ rclone config          
Current remotes:

Name                 Type
====                 ====

e) Edit existing remote
n) New remote
d) Delete remote
r) Rename remote
c) Copy remote
s) Set configuration password
q) Quit config
e/n/d/r/c/s/q> n
name> testDrive
Option Storage.
Type of storage to configure.
Enter a string value. Press Enter for the default ("").
Choose a number from below, or type in your own value.
 1 / 1Fichier
   \ "fichier"
 2 / Alias for an existing remote
   \ "alias"
 3 / Amazon Drive
   \ "amazon cloud drive"
 4 / Amazon S3 Compliant Storage Providers including AWS, Alibaba, Ceph, Digital Ocean, Dreamhost, IBM COS, Minio, SeaweedFS, and Tencent COS
   \ "s3"
 5 / Backblaze B2
   \ "b2"
 6 / Better checksums for other remotes
   \ "hasher"
 7 / Box
   \ "box"
 8 / Cache a remote
   \ "cache"
 9 / Citrix Sharefile
   \ "sharefile"
10 / Compress a remote
   \ "compress"
11 / Dropbox
   \ "dropbox"
12 / Encrypt/Decrypt a remote
   \ "crypt"
13 / Enterprise File Fabric
   \ "filefabric"
14 / FTP Connection
   \ "ftp"
15 / Google Cloud Storage (this is not Google Drive)
   \ "google cloud storage"
16 / Google Drive
   \ "drive"
17 / Google Photos
   \ "google photos"
18 / Hadoop distributed file system
   \ "hdfs"
19 / Hubic
   \ "hubic"
20 / In memory object storage system.
   \ "memory"
21 / Jottacloud
   \ "jottacloud"
22 / Koofr
   \ "koofr"
23 / Local Disk
   \ "local"
24 / Mail.ru Cloud
   \ "mailru"
25 / Mega
   \ "mega"
26 / Microsoft Azure Blob Storage
   \ "azureblob"
27 / Microsoft OneDrive
   \ "onedrive"
28 / OpenDrive
   \ "opendrive"
29 / OpenStack Swift (Rackspace Cloud Files, Memset Memstore, OVH)
   \ "swift"
30 / Pcloud
   \ "pcloud"
31 / Put.io
   \ "putio"
32 / QingCloud Object Storage
   \ "qingstor"
33 / SSH/SFTP Connection
   \ "sftp"
34 / Sia Decentralized Cloud
   \ "sia"
35 / Sugarsync
   \ "sugarsync"
36 / Tardigrade Decentralized Cloud Storage
   \ "tardigrade"
37 / Transparently chunk/split large files
   \ "chunker"
38 / Union merges the contents of several upstream fs
   \ "union"
39 / Uptobox
   \ "uptobox"
40 / Webdav
   \ "webdav"
41 / Yandex Disk
   \ "yandex"
42 / Zoho
   \ "zoho"
43 / http Connection
   \ "http"
44 / premiumize.me
   \ "premiumizeme"
45 / seafile
   \ "seafile"
Storage> onedrive
Option client_id.
OAuth Client Id.
Leave blank normally.
Enter a string value. Press Enter for the default ("").
client_id> {直接回车就可以}
Option client_secret.
OAuth Client Secret.
Leave blank normally.
Enter a string value. Press Enter for the default ("").
client_secret> {直接回车就可以}
Option region.
Choose national cloud region for OneDrive.
Enter a string value. Press Enter for the default ("global").
Choose a number from below, or type in your own value.
 1 / Microsoft Cloud Global
   \ "global"
 2 / Microsoft Cloud for US Government
   \ "us"
 3 / Microsoft Cloud Germany
   \ "de"
 4 / Azure and Office 365 operated by 21Vianet in China
   \ "cn"
region> global
Edit advanced config?
y) Yes
n) No (default)
y/n> n
Use auto config?
 * Say Y if not sure
 * Say N if you are working on a remote or headless machine

y) Yes (default)
n) No
y/n> y
2022/03/16 10:04:50 NOTICE: If your browser doesn't open automatically go to the following link: http://127.0.0.1:53682/auth?state=qkwpEntUvXPewzviZQXNOHA
2022/03/16 10:04:50 NOTICE: Log in and authorize rclone for access
2022/03/16 10:04:50 NOTICE: Waiting for code...
2022/03/16 10:06:03 NOTICE: Got code
Option config_type.
Type of connection
Enter a string value. Press Enter for the default ("onedrive").
Choose a number from below, or type in an existing value.
 1 / OneDrive Personal or Business
   \ "onedrive"
 2 / Root Sharepoint site
   \ "sharepoint"
   / Sharepoint site name or URL
 3 | E.g. mysite or https://contoso.sharepoint.com/sites/mysite
   \ "url"
 4 / Search for a Sharepoint site
   \ "search"
 5 / Type in driveID (advanced)
   \ "driveid"
 6 / Type in SiteID (advanced)
   \ "siteid"
   / Sharepoint server-relative path (advanced)
 7 | E.g. /teams/hr
   \ "path"
config_type> 1
Drive OK?

Found drive "root" of type "personal"
URL: https://onedrive.live.com/?cid=j2sy7l2us75j92

y) Yes (default)
n) No
y/n> y
--------------------
[testDrive]
type = onedrive
token = {这里就不展示了}
drive_id = js7y3isjy29hj725
drive_type = personal
--------------------
y) Yes this is OK (default)
e) Edit this remote
d) Delete this remote
y/e/d> y
Current remotes:

Name                 Type
====                 ====
testDrive          onedrive

e) Edit existing remote
n) New remote
d) Delete remote
r) Rename remote
c) Copy remote
s) Set configuration password
q) Quit config
e/n/d/r/c/s/q> q
```

## 测试&&挂载&&卸载
### 测试
接着你可以使用下面的命令列出自己 OneDrive 中的所有一级目录，测试一下是否配置成功

```bash
$ rclone lsd testDrive:
```

### 挂载
如果你只想在需要的时候手动挂载 OneDrive，可以使用下面的命令

```bash
$ rclone mount testDrive:/ /path/to/mount --allow-non-empty --vfs-cache-mode full --daemon
```

- `testDrive` 是上面在配置 OneDrive 时指定的 name
- `testDrive:` 后面跟着的是你想要挂载到本地的 OneDrive 中的目录或者文件，这里的 `/` 代表 OneDrive 中的所有内容
- `/path/to/mount` 是指定你将 OneDrive 挂载到本地的位置。此目录需要提前创建并赋予足够的读写权限
- `--allow-non-empty` 允许将 OneDrive 挂载到一个非空目录中，该参数**可不指定**
- `--vfs-cache-mode full` 缓存模式，我这里指定的是 `full`，默认是 `off` 。这里一共支持 4 种模式，我这里就简单说一下这两种的区别
    - `full` 模式下所有对远程文件的读写都会缓存到磁盘中
    - `off` 模式下所有对远程文件的读写都不会缓存到磁盘中
- rclone 的挂载命令是一个前台服务，如果你关闭了当前的会话，那挂载进程也会退出。`--daemon` 参数可以将 rclone 的挂载命令作为一个后台服务，这样即使你关闭了当前会话，挂载进程也不会退出

### 卸载
使用 rclone 挂载的云盘的卸载和普通磁盘的卸载方式一样，可以直接使用 `umount` 来完成  
```shell
umount testDrive
```

## 开机自启动

当然，如果想要避免每次开机都要手动挂载一次，也可以选择创建开机自启脚本

在 `~/.config/autostart` 目录下创建一个以 `.desktop` 结尾的文件，该文件的内容如下

```bash
[Desktop Entry]
Type=Application
NoDisplay=true
Terminal=false
Name=dianDrive
Exec=/usr/bin/rclone mount testDrive:/ /path/to/mount --allow-non-empty --vfs-cache-mode full --daemon
Icon=onedrive
Comment=Auto mount OneDrive to local.
Categories=Network;
```