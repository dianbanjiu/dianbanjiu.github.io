---
title: "从零开始配置 Golang 开发环境"
date: 2020-04-22T22:50:05+08:00
lastmod: 2020-04-22T22:50:05+08:00
tags: ["golang"]
author: "dianbanjiu"
---

## 安装 go

安装 go 的运行环境是第一步。  

Windows/Linux/MacOS 用户都可以从 [golang.org](https://golang.org) 下载安装包安装。  

### Windows
Windows 用户双击下载好的安装包，然后就可以开始熟悉的下一步了。  

### Linux
Linux 用户下载好之后需要执行下面的命令将包解压到 /usr/local 目录，方便所有用户的访问：  
```bash
$ tar -C /usr/local -xzf go.xxx.linux-amd64.tar.gz #请根据对应的版本修改命令，切勿盲目复制执行任何命令
```

然后你可以把 `/usr/local/go/bin` 添加到你的 `PATH` 当中：  
```bash
$ export PATH=$PATH:/usr/local/go/bin
```

如果你不想修改你的 PATH，那你可以把 `go/bin` 下的可执行文件链接到 `/usr/local/bin` 下面，因为默认的 PATH 路径一般都会包含这个路径：  
```bash
$ ln -s /usr/local/go/bin/* /usr/local/bin
# 注意最后的点
# 如果提示没有权限，可以使用 sudo 来进行，不过在使用 sudo 前请确保你知道自己在干什么
```

如果你只想为当前用户配置 golang 环境，那你也可以直接编辑你的 `~/.bashrc` 或者 `~/.zshrc` 在里面添加这么一句：  

```
PATH=$PATH:/usr/local/go/bin
```

当然最简单的方式还是使用系统的包管理器直接安装，包名一般为 `go` 或者 `golang`。对于 ArchLinux 这样的滚动发行版，源里对应的 golang 版本基本跟 golang 官网的版本保持一致，而 Debian 或者 Ubuntu 的版本可能会比较老一些。  

### MacOS
没用过 Mac，大致流程类似 Linux 下的操作

### 测试
安装配置好之后，可以使用下面的命令看是否安装成功了。  
```bash
$ go version
go version go1.14.2 linux/amd64
```
我当前 go 的版本是 1.14.2 linxu/amd64 架构的。  

## gomodule
gomodule 现在是 golang 默认的包管理工具，用来解决 go 项目的依赖问题。  

### 配置
下面来配置一下 gomodule 的环境。  

在你的 `.bashrc` 或者 `.zshrc` 里面添加下面两行：  
```bash
export GO111MODULE=on
export GOPROXY=https://goproxy.cn
```

gomodule 是从 go1.11 正式提供的，使用前需要先启用。  
goproxy 则是 go 的模块代理，以前一些类似 `golang.org/` 下的包可能很难下载，有了 goproxy 之后就不用再担心这个了。`goproxy.cn` 使用的是七牛的 CDN 进行分发的，实测速度很可以，当然还有一些其他的代理服务，你可以自行查找自己喜欢的。  

### 简单使用
```bash
$ cd ~/GoProject    # 进入项目根目录
$ go mod init dian/banjiu   # 初始化 go 模块
```

上面就创建了一个名为 `dian/banjiu` 的模块，模块名可以根据自己的需要修改。  
执行完上面的命令之后，会在项目根目录下生成一个 `go.mod` 文件，看一下里面的内容。  
```bash
$ cat go.mod
module dian/banjiu

go 1.13
```

初始情况里面只有一个模块名和你当前的 go 版本。  

安装模块的时候跟之前一样，项目根目录下执行：  
```bash
$ go get -u https://github.com/gin-gonic/gin
```

再看一下里面的内容：  
```bash
$ cat go.mod
module leetcode

go 1.13

require (
	github.com/gin-gonic/gin v1.6.2 // indirect
	github.com/golang/protobuf v1.4.0 // indirect
	github.com/modern-go/concurrent v0.0.0-20180306012644-bacd9c7ef1dd // indirect
	github.com/modern-go/reflect2 v1.0.1 // indirect
	golang.org/x/sys v0.0.0-20200420163511-1957bb5e6d1f // indirect
)
```

可以看到，gin 框架及其依赖都已经被添加进来了。

项目构建完成之后，可以使用 `go mod tidy` 将项目中从未用到过的模块移除。  

## 编辑器/IDE

推荐的编辑器是 VSCode，多平台、插件丰富。推荐的 IDE 是 Goland，功能齐全。  

### VSCode
安装好 VSCode 之后，你还需要安装 `go` 插件，直接 `扩展` 里搜索 go，第一个就是了。  

用 VSCode 打开一个 go 的项目，然后点开一个 go 文件，这时候右下角应该就会提醒你下载 go 的一些其他的包，用来支持 VSCode 中 go 的各种特性。在下载过程中，如果有某些包下载不下来，你可以复制这些包的链接，然后在终端手动下载，因为已经配置过 goproxy，所以手动下载一般都不会出现啥幺蛾子。然后就可以重启一下 VSCode 了。  

打开 `文件-首选项-设置`，首先建议修改 `Files.Auto Save` 为 `afterDelay`，也就是在文件修改以后多久会自动保存，这个时间是根据下一项的 `Files.Auto Save Delay` 指定的，我设置的是 `1000`，也就是 1秒。  
然后修改下面几项：  
1. `go.Docs Tool` 为 `gogetdoc`，获取 包/函数/方法/结构/... 的文档。  
2. 修改 `go.Format Tool` 为 `goimports`，go 源码的格式化工具。  
3. 启用 `go.Use Language Server`，如果 VSCode 的 golang 补全延迟很高的时候，可能就是没有启用这项导致的。  
4. 启用 `go.Autocomplete Unimported Packages`，在文件执行保存操作的时候，如果文件里有使用但未导入的包就会自动导入，第三方包需要先使用 `go get` 安装到当前项目才可以。  

然后就可以使用 VSCode 开始你愉快的 golang 开发了。  

### Goland
Goland 是 Jetbrains 开发的 Go IDE，它的配置在我的 [Goalnd 基础配置](https://www.dianbanjiu.com/post/goalnd-%E5%9F%BA%E7%A1%80%E9%85%8D%E7%BD%AE/) 有介绍，需要的话可以翻看一下，这里就不赘述了。  

Enjoy Yourself.  