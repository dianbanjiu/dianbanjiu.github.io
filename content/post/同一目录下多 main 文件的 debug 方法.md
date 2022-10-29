---
title: "同一目录下多 main 文件的 Debug 方法"
date: 2022-10-29T14:30:45+08:00
lastmod: 2022-10-29T14:30:45+08:00
tags: ["go", "vscode"]
author: "dianbanjiu"
comment: true
headPic: ""
---

现在有一个项目包含如下文件

- go.mod
- main.go
- demo.go

main.go 文件内容如下

```go
package main

func main() {
        n := Node{
                Id: 10,
                Data: "Hello foo",
        }

        demo(n)
}
```

demo.go 文件内容如下

```go
package main

import "fmt"

type Node struct {
        Data string `json:"data"`
        Id   int    `json:"id"`
}

func demo(n Node) {
        fmt.Println(n)
}
```

直接在项目目录下执行 `go run main.go` 或者使用 VSCode、Goland 等进行编译可能会出现异常

```text
./main.go:4:7: undefined: Node
./main.go:9:2: undefined: demo
```

在终端运行/编译的时候将 `go run main.go` 修改为 `go run .`（`.` 是 main 包所在的路径，需要根据项目情况进行修改）程序即可正常运行

在 VSCode 中可以通过如下配置来避免此问题  
1、打开 VSCode 的`运行和调试窗口`（快捷键 `Ctrl+Shift+D`）  
2、点击 `创建 launch.json 文件`  
3、选择 `Go：Launch Package`  
4、上一步会创建并打开 .vscode/launch.json 文件，文件内容如下

```JSON
{
    // 使用 IntelliSense 了解相关属性。
    // 悬停以查看现有属性的描述。
    // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Launch Package",
            "type": "go",
            "request": "launch",
            "mode": "auto",
            "program": "${fileDirname}"
        }
    ]
}
```

修改其中 `program` 字段的值为 `"${workspaceFolder}"`，这时再按下 F5 就会发现程序正常运行了

`workspaceFolder` 表示当前工作区的根目录，单项目一般指项目的根目录。如果 main 包在其他目录下，将 program 的值改为对应的路径即可。比如我的 main 包在项目的 `cmd/single/` 下，program 的值就需要改为 `"${workspaceFolder}/cmd/single"`

简而言之就是运行/编译项目的时候需要指定 main 包所在的目录，而非指定 main 方法所属的文件。在编译 go 程序的时候单纯指定 main 方法所在的文件，同目录下其他 main 包里的内容是不会被检索和编译的
