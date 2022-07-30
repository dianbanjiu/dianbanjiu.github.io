---
title: "从零开始配置 Go 开发环境"
date: 2020-04-22T22:50:05+08:00
lastmod: 2020-04-22T22:50:05+08:00
tags: ["go","goland","vscode"]
author: "dianbanjiu"
---

## 安装 go
关于详细的安装教程，可以参考 [官方安装文档](https://go.dev/doc/install)

上面的官方文档中是以安装包/源码包的方式安装的，常规开发是没什么问题，如果你想尽可能保证本地的版本为最新的版本，可以尝试通过各个系统的包管理器进行安装  

```bash
sudo pacman -S go #Arch Linux

sudo zypper in go #OpenSuse

brew install go #MacOS

scooop install go #Windows
```

通过包管理器安装的 go，只需要定期运行对应的更新命令，就可以将本地的 go 更新到当前比较新的版本（这个主要取决于包管理对于包的更新策略）  

当然你也可以使用官方的 [多版本管理方式进行安装](/post/go-多版本安装)
### 测试
安装完成之后，可以使用下面的命令看是否安装成功了  
```bash
$ go version
go version go1.14.2 linux/amd64
```
我当前 go 的版本是 1.14.2 linxu/amd64 架构的  

## gomodule
gomodule 现在是 go 默认的包管理工具，用来解决 go 项目的依赖问题  

### 配置
下面来配置一下 gomodule 的环境，在终端执行下面的两条命令  

```bash
go env -w GO111MODULE=on # 如果你使用 1.13 之后的版本，可以忽略这一条命令
go env -w GOPROXY=https://goproxy.cn # 你可以使用其他的代理，比如 https://goproxy.io 等
```

goproxy 则是 go 的模块代理，以前一些类似 `golang.org/` 下的包可能很难下载，有了 goproxy 之后就不用再担心这个了。`goproxy.cn` 使用的是七牛的 CDN 进行分发的，实测速度很可以，当然还有一些其他的代理服务，你可以自行查找自己喜欢的  

### 简单使用
```bash
$ cd ~/GoProject    # 进入项目根目录
$ go mod init dian/banjiu   # 初始化 go 模块
```

上面就创建了一个名为 `dian/banjiu` 的模块，模块名可以根据自己的需要修改   
执行完上面的命令之后，会在项目根目录下生成一个 `go.mod` 文件，看一下里面的内容  
```bash
$ cat go.mod
module dian/banjiu

go 1.14
```

一开始里面只有一个模块名和你当前的 go 版本  

安装模块的时候跟之前一样，在项目根目录下执行：  
```bash
$ go get -u https://github.com/gin-gonic/gin
```

再看一下里面的内容：  
```bash
$ cat go.mod
module leetcode

go 1.14

require (
	github.com/gin-gonic/gin v1.6.2 // indirect
	github.com/golang/protobuf v1.4.0 // indirect
	github.com/modern-go/concurrent v0.0.0-20180306012644-bacd9c7ef1dd // indirect
	github.com/modern-go/reflect2 v1.0.1 // indirect
	golang.org/x/sys v0.0.0-20200420163511-1957bb5e6d1f // indirect
)
```

可以看到，gin 框架及其依赖都已经被添加进来了  

项目构建完成之后，可以使用 `go mod tidy` 将项目中从未用到过的模块移除  

## 编辑器/IDE

推荐的编辑器是 VSCode，多平台、插件丰富。推荐的 IDE 是 Goland，功能齐全  

### VSCode
安装好 VSCode 之后，扩展市场安装 `go` 插件，直接在 `扩展` 里搜索 go，第一个就是了  

接着在 VSCode 中按 `Ctrl+Shift+p`，输入 `go: install/update tools`，全选所有依赖项，点击确定安装一些使用 VSCode 开发 go 时的必须依赖。如果你在国内，在点击 Install 之前，记得一定先配置好 `GOPROXY` 变量  

打开 `文件-首选项-设置`，首先建议修改 `Files.Auto Save` 为 `afterDelay`，也就是在文件修改以后多久会自动保存，这个时间是根据下一项的 `Files.Auto Save Delay` 指定的，我设置的是 `1000`，也就是 1秒。注意，启用定时保存方式的时候，自动格式化的功能是不会生效的  

如果你使用过 Goland，你应该会发现每次把窗口从 Goland 切换到其他应用的时候，他都会自动格式化你之前修改的文件。在 VSCode 中也可以实现类似的效果，只需要将 
1. `Files.Auto Save` 修改为 `onWindowsChange` 或者 `onFocusChange`
2. 启用 `Format on Save`
3. 将 `Format on Save Mode` 修改为 `file`


接着可以修改 go 插件相关的几项内容：  
1. `go.Docs Tool` 为 `gogetdoc`，获取 包/函数/方法/结构/... 的文档  
2. 修改 `go.Format Tool` 为 `goimports`，go 源码的格式化工具  
3. 启用 `go.Use Language Server`，如果 VSCode 的 go 补全延迟很高的时候，可能就是没有启用这项导致的  
4. 启用 `go.Autocomplete Unimported Packages`，在文件执行保存操作的时候，如果文件里有使用但未导入的包就会自动导入，第三方包需要先使用 `go get` 安装到当前项目才可以  

最后就可以使用 VSCode 开始你愉快的 go 开发了  

### Goland
Goland 是 Jetbrains 开发的 Go IDE，它的配置在我的 [Goalnd 基础配置](https://www.dianbanjiu.com/post/goalnd-%E5%9F%BA%E7%A1%80%E9%85%8D%E7%BD%AE/) 有介绍，需要的话可以翻看一下，这里就不赘述了  

Enjoy Yourself.  