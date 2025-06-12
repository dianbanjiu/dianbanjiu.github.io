---
title: "Linux 客户端代理配置"
date: 2019-08-30T18:34:00+08:00
lastmod: 2020-09-08T23:13:00+08:00
tags: ["Linux","shadowsocks","privoxy"]
author: "dianbanjiu"
---

## 购买服务  

最开始是自建的 shadowsocks 服务，使用的搬瓦工洛杉矶的服务器，但是速度不是很理想，而且还经常被封。后来迁移数据的时候，发现搬瓦工也有单独的 shadowsocks 服务出售，相对于自建来说成本更低，相较来说算是个不错的选择。  

费用如下图所示  
![Imgur](https://imgur.com/iJGqs6F.png)  
这个是最便宜的那个套餐，每月 100G 的流量，因为我平时多的话也不过 50 左右，所以就选了这个，如果你嫌少，他们还有每月 500/1000G 的套餐，世界加钱可及。  

速度的话，YouTube 1080p 基本上可以全程无卡顿，至于 2k、4k 的话，也可以，不过会有偶尔的卡顿。  

如果你也想购买他们的服务，可以 [点击此处直达](https://justmysocks.net/members/aff.php?aff=4151)。  

**2022 年 7 月 15 号更新**  
弃用了 Qv2ray 和 shadowsocks-libev，统一使用 [clash](https://github.com/Dreamacro/clash) 配置代理  

clash 是一个用 go 写的多平台代理工具，我这里使用的是无图形界面的版本，你可以直接从包管理器里面安装，或者从项目的 release 下载  
```shell
# Arch Linux
sudo pacman -S clash

# windows
scoop install clash
```  

安装完成之后需要先运行一次 clash 程序下载 `Country.mmdb` 并且初始化 `config.yaml`，这两个文件一般位于用户主目录下面的 `.config/clash` 下面

接着只需要修改 config.yaml 文件，并且添加自己的服务配置信息即可  

一些参数的基础配置可以参考 [此处](https://github.com/dianbanjiu/just/tree/main/clash)  

---


**2020 年 9 月 7 日更新**  
现在 justmysocks 默认的五个连接中的三个已经从 shadowsocks 修改为了 vmess 协议，虽然 shadowsocks-libev 可以安装 v2ray 插件来支持 vmess，但是总的来说配置还是不够直观，而 shadowsocks-qt 也并没有提供插件的计划，所以现在我已经改为使用 `qv2ray` 来作为新的代理工具了。  

## Qv2ray

需要安装的软件：  
- Qv2ray
- v2ray

安装完成之后，打开 Qv2ray，点击左上角的 `首选项`，在 `内核设置` 处确保 v2ray 核心选择正确。  
![](/resources/_gen/images/qv2ray首选项.png)  

在 `入站设置` 中勾选设置 `系统代理`，并启用 `socks设置` 与 `http设置`（Linux 在此处只需设置 系统代理，MacOS 三项都需开启）。  

在 `连接设置` 中，启用 `绕过中国大陆`。  

最后点击左下角的 `新建` 按钮，选择 `二维码`，在你的 justmysocks 界面截屏扫描对应的帐号二维码即可添加 socks 或者 v2ray 链接。  

Linux 下启用 `系统代理` 后，浏览器无需设置任何代理工具就可以直接访问 google，而在 MacOS 下则仍需配置浏览器代理，所以 Linux 在第二步中只需设置 系统代理，而 MacOS 三项都需开启才能正常使用。

---

## shadowsocks 配置  

首先使用系统的包管理软件安装 shadowsocks-libev 这个软件包。  
```shell
$ sudo pacman -S shadowsocks-libev # Arch Linux
$ sudo apt install shadowsocks-libev # Debian/Ubuntu
```  

在 /etc/shadowsocks 下创建 config.json 文件。在其中写入以下内容，将其中的字段根据情况修改，主要是服务器的地址(server)、密码(password)、端口号(server_port)、加密方法(method)。  
```json
{
    "server":"my_server_ip",
    "server_port":8388,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
```  

shadowsocks-libev 添加 obfs 插件，在配置文件中还需要添加两行：  

```json
    "plugin":"obfs-local",
    "plugin_opts":"obfs=tls"
```

配置好文件后，使用下面的命令启动 shadowsocks 服务：  
```shell
$ sudo systemctl start shadowsocks-libev@config.service
```

如果想要开机自启动的话：  
```shell
$ sudo systemctl enable shadowsocks-libev@config.service
```

当然，你也可以选择使用带有前端界面的 `shadowsocks-qt5` 程序，它在大多数的发行版中都可以找到。  
 
因为并不是所有的程序都支持 socks 服务，但是大多数的程序都支持 http 代理，下面就通过 privoxy 将 socks 转发为 http 代理。  

## privoxy 配置

首先下载 `privoxy` 这个软件包。  

编辑 privoxy 的配置文件 `/etc/privoxy/config`，在其中添加下面一行，转发 socks5：  
```shell
forward-socks5 / localhost:1080 .
```

**注意在最后有一个点**  

使用下面的命令启动 privoxy  
```shell
$ sudo systemctl start privoxy.service
```  

建议将该服务设置为开机自启动。  
```shell
$ sudo systemctl enable privoxy.service
```

至此为止，shadowsocks 就在 Linux 客户端设置完毕，也成功将 socks5 转发为 http，可以通过 chromium 来验证一下是否可用。  
```shell
$ http_proxy=http://127.0.0.1:8118 chromium
```

如果可以正常使用 google 就说明配置成功了。  