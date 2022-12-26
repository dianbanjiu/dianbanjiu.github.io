---
title: "从零开始配置 Go 开发环境"
date: 2020-04-22T22:50:05+08:00
lastmod: 2020-04-22T22:50:05+08:00
tags: ["go","goland","vscode"]
author: "dianbanjiu"
---

# 安装 go
关于详细的安装教程，可以参考 [官方安装文档](https://go.dev/doc/install)

官网提供了各个操作系统下的安装包以及源码，你也可以尝试通过包管理器进行安装，更加简单而且方便后面的升级  

```bash
sudo pacman -S go #Arch Linux

sudo zypper in go #OpenSuse

brew install go #MacOS

scoop install go #Windows
```

通过包管理器安装的 go，只需要定期运行对应的更新命令，就可以将本地的 go 更新到当前比较新的版本（这个主要取决于包管理对于包的更新策略）  

当然你也可以使用官方的 [多版本管理方式进行安装](/post/go-多版本安装)  

## 测试
安装完成之后，可以使用下面的命令看是否安装成功了  
```bash
$ go version
go version go1.14.2 linux/amd64
```
我当前 go 的版本是 1.14.2 linxu/amd64 架构的  

# gomodule
gomodule 现在是 go 默认的包管理工具，用来解决 go 项目的依赖问题  

## 配置
下面来配置一下 gomodule 的环境，在终端执行下面的两条命令  

```bash
go env -w GO111MODULE=on # 如果你使用 1.13 之后的版本，可以忽略这一条命令
go env -w GOPROXY=https://goproxy.cn # 你可以使用其他的代理，比如 https://goproxy.io 等
```

goproxy 是 go 的模块代理，以前一些 `golang.org/` 下的包下载可能会比较慢，有了 goproxy 之后就不用再担心这个了。`goproxy.cn` 使用的是七牛的 CDN 进行分发的，实测速度很可以，当然还有一些其他的代理服务，你可以自行查找自己喜欢的  

## 简单使用
```bash
$ mkdir ~/GoProject
$ cd ~/GoProject
$ go mod init hello   # 初始化 go 模块
```

上面就创建了一个名为 `demo` 的模块，模块名可以根据自己的需要修改   
执行完上面的命令之后，会在项目根目录下生成一个 `go.mod` 文件，看一下里面的内容  
```bash
$ cat go.mod
module demo

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

# 编辑器/IDE

推荐的编辑器是 VSCode，多平台、插件丰富。推荐的 IDE 是 Goland，功能齐全  

## VSCode
1、安装 VSCode  
2、扩展市场搜索并安装 `golang.go`  
3、按 `Ctrl+Shift+p`，输入 `go: install/update tools`，全选依赖项，点击确定安装开发 go 用到的一些依赖。在开始之前请先配置好 go 的 `GOPROXY` 变量  
4、（可选）安装`formulahendry.code-runner` 插件用于直接运行一些代码片段  
5、（可选）安装 `maxnatchanon.go-struct-tag-autogen` 用于自动补全结构体多种风格的 tag。具体的配置可以参考 https://github.com/maxnatchanon/vscode-go-struct-tag-autogen，下面是我的配置  
```JSON
"goStructTagAutogen.tagSuggestion": {
    "json": {
      "cases": ["camel", "snake", "uppersnake", "pascal", "none"],
      "options": ["-", "omitempty"]
    },
    "form": {
      "cases": ["camel", "snake", "uppersnake", "pascal", "none"],
      "options": ["-", "omitempty"]
    },
    "bson": {
      "cases": ["snake"],
      "options": ["-", "omitempty"]
    },
    "binding": {
      "options": ["required"]
    }
  }
```
注意，如果自动补全不生效，你需要在你 VSCode 的配置文件中新增下面的内容  
```JSON
"editor.quickSuggestions": {
    "strings": "on"
}
```
6、（可选）安装 `doggy8088.quicktype-refresh`，可以转换 JSON 数据为对应的结构体  
7、（可选）安装 `mishkinf.goto-next-previous-member`，使用 Ctrl+Arrow Up/Down 在同一个文件相邻的方法间进行跳转  
8、（可选）在 VSCode 的配置文件中新增下面的内容，启用由 gopls 支持的语法高亮，具体说明可以参见 https://code.visualstudio.com/api/language-extensions/semantic-highlight-guide  
```JSON
"gopls": {
  "ui.semanticTokens": true
}
```

接着就可以使用 VSCode 开始你愉快的 go 开发了  
## Goland
Goland 是 Jetbrains 开发的 Go IDE，它的配置在我的 [Goalnd 基础配置](https://www.dianbanjiu.com/post/goalnd-%E5%9F%BA%E7%A1%80%E9%85%8D%E7%BD%AE/) 有介绍，需要的话可以翻看一下，这里就不赘述了  

Enjoy Yourself.  