---
title: "Vscode中一些好用的小tips"
date: 2023-11-23T17:07:40+08:00
lastmod: 2023-11-23T17:07:40+08:00
tags: ["vscode"]
author: "dianbanjiu"
comment: true
headPic: ""
---
vscode 目前是我的主力编辑器，日常的一些文字记录/开发都是通过他来完成的。此文会记录一些能够帮助我更舒服使用 vscode 的内容，可能会包含插件、设置、快捷方式等各方面，后续会根据使用情况逐渐进行补全

**1、补全注释中的方法名**  
go 的注释风格是以方法名开头的，比如  
```go
// Hello 打印欢迎信息
func Hello(name string) {
    fmt.Println("hello", name)
}
```
但是每次手动拼写每个函数名也还是挺麻烦的，最近发现可以先在函数上面打出 `//`，然后按下 `ctrl+space`，补全提示的第一个就是当前的方法名  

![vscode补全方法名](/img/vscode补全方法名.gif)  

`ctrl+space` 是 vscode 默认的 `触发建议` 的快捷键

**2、补全结构体的tag**  
写 go 的结构体时，经常需要给结构体加上 tag，目前 vscode-go 是按照固定格式直接一次性给你生成好，但是有时候我想要的并不是模板中设定的，这时候推荐安装 `maxnatchanon.go-struct-tag-autogen` 扩展，你可以先提前设定好你可能用到的 tag 以及样式，比如我的设置  
```json
"editor.quickSuggestions": {
    "other": "on",
    "comments": "off",
    "strings": "on"
},
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
},
```
在写完结构体的一个字段之后，如果我想生成名为 form 的 tag，在字段的最后输入 `f 就能看到一个推荐的补全列表，在其中选择需要的即可

![vscode补全结构体tag](/img/vscode补全结构体tag.gif)
