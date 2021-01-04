---
title: "Goalnd 基础配置"
date: 2019-10-30T10:22:53+08:00
lastmod: 2019-10-30T10:22:53+08:00
tags: ["goland", "go", "go module"]
author: "dianbanjiu"
toc: false
---

首先先设置一下 goproxy 和 go module。在系统的环境变量当中添加以下两句（zsh 的配置环境在 ~/.zshrc，bash 的在 ~/.bashrc）:  
```shell
export GO111MODULE=on
export GOPROXY=https://goproxy.cn
export PATH=$PATH:$HOME:/go/bin
```

在 goland - settings - go - go modules，中，选中 `Enable go Module`，并在 Proxy 中填入代理站点，如 `https://goproxy.io` 或者 `https://goproxy.cn`。  

![](https://i.imgur.com/H2hB8nA.png)  

之前我在 goland 中开启了 go module 之后就再也不能自动补全包中的设置了，究其原因还是因为我虽然开启了 go module，但是每次创建新项目的时候都是手动创建文件夹，然后再手动创建 go 文件，所以我只是开启了 go module，但是并未实际使用。  

所以在之后使用 goland 创建新项目的时候，最好直接选择 vgo 来创建，在导入一个外部包之后，在这个包上使用 `Alt+Enter` 组合键，然后选中 `Sync packages of <Project name>`，goland 就会自动将依赖添加至 go.mod 文件当中，这时你再使用外部包中的相关函数的时候，就可以触发自动补全了。  

除此之外，goland 还为我们提供了一些其他的工具，需要手动开启。  

在 Settings - Tools - File Watchers 中，可以依次添加 `go fmt`，`goimports` 还有 `golangci-lint`，全部使用默认配置即可。  

gofmt 一般在安装 go 的时候就已经预装了，goimports 在 golang.org/x/tools/cmd/goimports 当中，如果系统上之前没有装，现在就需要手动安装。  
```shell
$ go get -u golang.org/x/tools/cmd/goimports
$ cd ~/go/pkg/mod/golang.org/x/tools@2019xxxx/cmd/goimports
$ go install
```

如果你添加了 archlinuxcn 的源，那你可以直接通过 pacman 来安装 golangci-lint，或者你也可以通过 aur 来安装，当然，你也可以从 golangci-lint 源码安装：  
```shell
$ sudo pacman -S golangci-lint # Arch Linux 用户
$ yay golangci-lint # Arch Linux Aur
$ curl -sfL https://raw.githubusercontent.com/golangci/golangci-lint/master/install.sh| sh -s -- -b $(go env GOPATH)/bin v1.21.0  # 源码安装，将安装至 go/bin 下
```

最后是 `golint`，在 Settings - Tools - File Watchers 中，复制 `go fmt` 的内容，将 Name 修改为 `golint`，Program 改为 `golint`，在 Arguments 前添加 `-set_exit_status`：  

![](https://i.imgur.com/VCnfW2A.png)  

假如你的系统上之前没有安装 `golint`，可以通过下面的方法来安装。  
```shell
$ go get -u golang.org/x/tools/lint/golint
```

一般情况下，go 会自动安装 golint 到 go/bin 下。如果没有的话可以进入 golint 包，手动安装。  

---

**参考文章：** https://goframe.org/prepare/gomodule