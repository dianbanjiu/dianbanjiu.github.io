---
title: "Linux 安装后"
date: 2022-04-06
tags: ["Linux", "实用工具"]
author: "dianbanjiu"
---
之前因为好奇心，在自己的笔记本上重装了 N 多次系统，虽然基本上都是局限在 Linux 的各种发行版之间，不过最近两三年基本上算是稳定下来，只尝试过 Manjaro 和 Arch Linux，最近两年的话就只有在使用 Arch Linux 了。下面记录一下我在重装系统之后会安装的一些软件以及基本都会做的一些简单配置，来帮助我更好的使用这台电脑

## 配置源

为了保证后续软件安装的速度，在连接网络后的第一件事就是先配置国内的软件源

1、编辑 `/etc/pacman.d/mirrorlist`，搜索 `China` 字段，将清华、中科大的镜像源移动到文件的开头

```bash
Server = https://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
```

2、archlinuxcn 软件源提供了很多非官方源的常用软件包，在 `/etc/pacman.conf` 的最后添加如下的内容以启用 archlinuxcn 源

```
[archlinuxcn]
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

添加完成之后需要安装 `archlinuxcn-keyring` 来导入一些 archlinuxcn 对应的密钥

```
$ sudo pacman -Syyu archlinuxcn-keyring
```

## 软件安装

| 软件 | 备注 |
| --- | --- |
| chromium | 浏览器 |
| tmux | 终端复用工具 |
| git | 版本管理工具 |
| visual-studio-code-bin | 文本编辑器 |
| tree | 树形目录查看 |
| goland gland-jre | go IDE，继承自 IDEA |
| spotify | 流音乐媒体 |
| keepassxc | 密码管理工具 |
| jdk11-openjdk | jdk |
| java11-openjfx | goland 的 markdown 预览依赖此程序 |
| docker | 容器 |
| obs-studio | 录屏 |
| flameshot | 截图工具 |
| yay | aur 包安管理 |
| go | go 语言开发环境 |
| fcitx fcitx-qt5 | fcitx 输入法框架 |
| telegram-desktop | IM 工具 |
| hugo | hugo 博客命令行工具 |
| nodejs npm | nodejs 开发环境 |
| scrcpy | 连接 Android 与 PC，可以在 PC 上直接操作手机 |
| plasma-browser-integration | plasma 桌面的浏览器集成插件 |
| translate-shell | 终端翻译工具 |
| postman-bin | api 测试工具 |
| kgpg | kde 的 gnupg 图形界面 |
| ark | KDE 官方的压缩文件查看器 |
| unzip unrar p7zip | 几种常用的压缩格式 |
| kate | KDE 官方的文本编辑器 |
| gnome-keyring | 钥匙串管理，vscode 连接 github 需要使用到 |
| peek | linux 下一个非常简单的 gif 录制工具 |
| numix-circle-icon-theme-git | 图标包 |

一行命令安装上面所有应用。

```
$ sudo pacman -S git zsh vim tmux visual-studio-code-bin tree goland goland-jre spotify keepassxc jdk11-openjdk java11-openjfx docker obs-studio flameshot yay go fcitx fcitx-qt5 telegram-desktop hugo nodejs npm scrcpy plasma-browser-integration translate-shell postman-bin kgpg ark unarchiver unzip unrar p7zip kate gnome-keyring peek numix-circle-icon-theme-git
```

## 环境配置

### 磁盘自动挂载

如果想要将一些其他的磁盘在系统开机时自动挂载，可以先通过 `blkid` 命令找到磁盘对应的 UUID，接着在 `/etc/fstab` 的末尾按照下面的格式新增一行

```bash
UUID=disk_uuid /path/to/mount disk_format defaults 0 0
```

- `disk_uuid` 就是上面通过 `blkid` 命令找到的磁盘的 uuid
- `/path/to/mount` 是你想要将磁盘挂载到的目录
- `disk_format` 是磁盘的文件系统格式，比如 NTFS、EXT4、FAT32 等等
- 后面的 `defaults 0 0` 如非，必要保持不变即可，其及具体含义可以参照 [Arch Linux Wiki 关于 fstab 的介绍](https://wiki.archlinux.org/title/fstab)

### zsh 配置

oh-my-zsh 提供了一套开箱即用的 zsh 配置，并且有很多额外的主题和插件可供选用，可以通过下面的命令来安装 oh-my-zsh 以及 zsh-autosuggestions、zsh-syntax-highlighting 两个插件

```
$ sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
$ git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
$ git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

如果提示 `443 拒绝连接` 的话，可以直接访问 [ohmyzsh github 仓库](https://github.com/ohmyzsh/ohmyzsh/blob/master/tools/install.sh)，然后将该文件的内容复制到 install.sh 文件中，使用 `bash install.sh` 命令来安装 oh-my-zsh

安装完成之后编辑 `~/.zshrc`，修改 plugins 字段。

```
plugins=(git zsh-autosuggestions zsh-syntax-highlighting extract)
```

`extract` 插件可以简化命令行解压时复杂的参数，只需 `x 压缩包`，即可将压缩包解压到当前目录下。

一般情况下 `zsh-autosuggestions`会将一些待补全的内容以较浅的颜色进行展示，但是异常情况下可能直接以普通文本的形式展示出来，这可能是因为终端的颜色编码配置不正确，需要在 .zshrc 中添加 `export TERM=xterm-256color`

### 配置 docker

使用下面的命令将当前用户添加到 docker 组中，之后就可以通过当前用户身份直接使用 docker。

```
$ sudo usermod -aG docker $USERNAME
```

编辑 `/etc/docker/daemon.json`，添加国内镜像源

```json
{  "registry-mirrors": ["https://reg-mirror.qiniu.com/"]}
```

阿里也提供了 docker 镜像源服务，但是需要使用个人帐号获取对应的链接，[点此获取](https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors)

### go 开发配置

安装 go 依赖自动导入工具

```
$ go get -u golang.org/x/tools/cmd/goimports
```

在 .zshrc 添加以下内容切换 goproxy，加速依赖获取

```
# export GO111MODULE=on #新版本的 GO 默认开启此功能，可不添加此行
export GOPROXY=https://goproxy.cn
```

也可以通过 go 提供的命令行工具来完成 GOPROXY 环境变量的设置

```bash
$ go env -w GOPROXY=https://goproxy.cn
```

### 输入法配置

/etc/profile 开头添加以下内容，可以避免 fcitx 的一些问题

```
export XMODIFIERS="@im=fcitx"
export GTK_IM_MODULE="fcitx"
export QT_IM_MODULE="fcitx"
```

### deepin-tim 配置

安装 deepin 版 tim 之后需要在设置的开机自启动里添加 `/usr/lib/gsd-xsettings` 的自启动脚本。

```
调整 tim 的 DPI

$ cd /opt/deepinwine/tools  # TIM 的安装目录
$ ./SetDpi.sh 126 Deepin-TIM    # 调整 TIM 的 DPI
```

### 蓝牙配置

编辑 `/etc/bluetooth/main.conf`，找到 `AutoEnable` 字段，取消前面的注释，并将对应的值修改为 `true` 就可以在电脑启动的同时开启蓝牙，这样在登录界面就可以直接连接到你的蓝牙外设了

### Jetbrains 配置

Jetbrains 系列的软件在 Linux 下默认的字体显示非常辣眼睛，需要同时安装对应的 `-jre` 支持才行。如 Goland 就需要安装 `goland-jre`

如果有在 goland 中预览 markdown 文件的需要，还需要安装 `java11-openjfx`

### 界面美化

KDE 当前的默认桌面其实已经挺不错的了，此处仅推荐一套图标包 [numix-circle-icon-theme](https://github.com/numixproject/numix-icon-theme-circle)