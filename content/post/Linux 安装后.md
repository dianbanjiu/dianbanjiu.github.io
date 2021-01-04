---
title: "Linux 安装后"
date: 2020-11-07T13:31:23+08:00
lastmod: 2020-01-02T13:31:23+08:00
tags: ["Linux", "实用工具"]
author: "dianbanjiu"
toc: false
---

之前因为各种奇奇怪怪的想法重装了 n 次电脑，不过因为一直都是在 Linux 的圈内，所以就记录一下在基本的安装完成之后，我都还干了些什么。

以下操作均基于 Manjaro Linux 20.2 Nibia 进行。

## 配置源

在连接网络之后，第一件事就是先配置软件源，要不之后安装软件的过程会极慢。

1. 更改主源

```shell
$ sudo pacman-mirrors -i -c China -m rank
```

选择清华、中科大、上交大镜像源，保存。

2. 添加 archlinuxcn 源

```shell
$ sudo nano /etc/pacman.conf

文件最后添加以下内容

[archlinuxcn]
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

然后需要安装 `archlinuxcn-keyring`。

```shell
$ sudo pacman -Syyu archlinuxcn-keyring
```

## 软件安装

| 软件                                                                                | 备注                              |
| ----------------------------------------------------------------------------------- | --------------------------------- |
| firefox、chromium                                                                   | 浏览器                            |
| nutstore                                                                            | 坚果云，同步云盘                  |
| tmux                                                                                | 终端复用工具                      |
| visual-studio-code-bin                                                              | 文本编辑器                        |
| gimp                                                                                | 图片编辑工具                      |
| tree                                                                                | 树形目录查看                      |
| goland                                                                              | go IDE，继承自 IDEA               |
| spotify                                                                             | 流音乐媒体                        |
| keepassxc                                                                           | 密码管理工具                      |
| jdk11-openjdk                                                                       | jdk                               |
| java11-openjfx                                                                      | goland 的 markdown 预览依赖此程序 |
| docker                                                                              | 容器                              |
| obs-studio                                                                          | 录屏                              |
| flameshot                                                                           | 截屏                              |
| yay                                                                                 | aur 包安管理                      |
| go                                                                                  | go 语言                           |
| fcitx5 fcitx5-chinese-addons fcitx5-gtk fcitx5-material-color fcitx5-qt fcitx5-rime | fcitx5 输入法框架                 |
| telegram-desktop                                                                    | IM 工具                           |
| hugo                                                                                | hugo 博客命令行工具               |
| nodejs npm                                                                          |
| scrcpy                                                                              | 手机投屏                          |
| virtualbox virtualbox-ext-oracle virtualbox-ext-vnc virtualbox-guest-iso            | 虚拟机                            |
| plasma-browser-integration                                                          | plasma 桌面的浏览器集成插件       |
| translate-shell                                                                     | 终端翻译工具                      |
| postman                                                                             | api 测试工具                      |
| kgpg                                                                                | kde 的 gnupg 图形界面             |

```shell
$ sudo pacman -S nutstore vim tmux visual-studio-code-bin gimp tree goland spotify keepassxc jdk11-openjdk java11-openjfx docker obs-studio flameshot yay go fcitx5 fcitx5-chinese-addons fcitx5-gtk fcitx5-material-color fcitx5-qt fcitx5-rime unoconv telegram-desktop hugo nodejs npm golangci-lint scrcpy virtualbox virtualbox-ext-oracle virtualbox-ext-vnc virtualbox-guest-iso plasma-browser-integration translate-shell
```

## 环境配置

如果你有一些非系统盘，需要在开机时自动挂载，可以通过 `blkid` 命令查看对应的 UUID，然后将其添加到 fstab 中。

### zsh 配置

使用下面的命令获取 oh-my-zsh。

```shell
$ sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
$ git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
$ git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

如果提示 `443` 拒绝连接的话，可以直接访问 [ohmyzsh github 仓库](https://github.com/ohmyzsh/ohmyzsh/blob/master/tools/install.sh)，然后将该文件的内容复制到 install.sh 文件中，使用 `bash install.sh` 命令来安装 oh-my-zsh。

安装完成之后编辑 `~/.zshrc`，修改 plugins 字段。

```shell
plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
```

### 配置 vbox 和 docker

使用下面的命令将 vbox 和 docker 添加到当前用户组下，之后就可以通过普通用户身份直接使用。

```
$ sudo usermod -aG vboxusers,docker username
```

编辑 `/etc/docker/daemon.json`，添加国内镜像源

```json
{
  "registry-mirrors": ["https://reg-mirror.qiniu.com/"]
}
```

阿里也提供了 docker 镜像源服务，但是需要使用个人帐号获取对应的链接，[点此访问](https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors)。

配置完成之后需要重启一次系统，让用户添加操作生效。

### go 开发配置

安装 go 依赖自动导入工具

```shell
$ go get -u golang.org/x/tools/cmd/goimports
```

在 .zshrc 添加以下内容切换 goproxy,加速依赖下载。

```shell
export GO111MODULE=on
export GOPROXY=https://goproxy.cn
```

### 输入法配置

/etc/profile 开头添加以下内容，可以避免 fcitx5 的一些问题

```shell
export XMODIFIERS="@im=fcitx5"
export GTK_IM_MODULE="fcitx5"
export QT_IM_MODULE="fcitx5"
```

### deepin-tim 配置

安装 deepin 版 tim 之后需要在设置的开机自启动里添加 `/usr/lib/gsd-xsettings` 的自启动脚本。

```shell
调整 tim 的 DPI

$ cd /opt/deepinwine/tools  # TIM 的安装目录
$ ./SetDpi.sh 126 Deepin-TIM    # 调整 TIM 的 DPI
```

### 蓝牙配置

在电脑启动后开启蓝牙，可以在电脑启动之后直接连接到蓝牙。

编辑 `/etc/bluetooth/main.conf`，找到 `AutoEnable` 字段，取消前面的注释，并将对应的值修改为 `true`。


### Jetbrains 配置
Jetbrains 系列的软件在 Linux 下默认的字体显示非常辣眼睛，需要同时安装对应的 `-jre` 支持才行。如 Goland 就需要安装 `goland-jre`。  