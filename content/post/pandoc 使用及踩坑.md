---
title: "Pandoc 使用及踩坑"
date: 2019-08-31T18:52:53+08:00
lastmod: 2019-08-31T18:52:53+08:00
tags: ["pandoc"]
author: "dianbanjiu"
---

我在转换文档方面的需求很简单，只要能把各种不同类型的文档转换成 PDF 文件就可以了。我的电脑系统是 Arch Linux，没有装 LibreOffice 或者 OpenOffice，因为这两个在格式方面的问题太多了，不如 PDF 来的痛快（关键是 Okular + PDF 真的很舒服），所以我大多数时候都是把 word 或者 markdown 之类的文件转换成 PDF 文件来看。鉴于我的这些需求，我想到了 [pandoc](https://pandoc.org/)。  

## 安装

pandoc 在各个 Linux 发行版的仓库中都有，可以直接使用自己系统上的包管理进行安装即可。   
除了 pandoc 之外，还需要安装 `texlive` 的相关组件，用于把文档转换为 PDF 格式。基于 Arch Linux 的系统下叫做 `texlive-core`，基于Debian 的系统下叫做 `texlive-base`。

Arch Linux
```shell
$ sudo pacman -S  pandoc texlive-core
```

Debian/Ubuntu
```shell
$ sudo apt install pandoc texlive-base
```

## 快速上手  

在 pandoc 的官方 Github 库的 [README](https://github.com/jgm/pandoc/blob/master/README.md) 列出了 pandoc 支持读取和转换成的各种类型。  

pandoc 的使用很简单  
```shell
$ pandoc test.md -o test.pdf 
```
> 注意：如果文件转换成功，是不会在终端输出任何内容的  


这样就可以把 `markdown` 转换成 `pdf` 类型了，其中的 `-o` 选项是指定转换后的文件名。  

pandoc 还支持许多其他的选项，如果你有兴趣的话，可以使用 `man pandoc ` 命令查看，或者可以参阅 pandoc 的在线 [User Guide](https://pandoc.org/MANUAL.html)，也可以下载 [PDF 版](https://pandoc.org/MANUAL.pdf)。

不过上面说到的那个例子只有在你的源文件是纯英文的时候出问题的情况才会比较少，如果文件是非英文的，那就还需要进行一些额外的配置。    

首先使用 `fc-list :lang=zh` 命令查看一下你自己系统上都安装了哪些中文字体：  
```shell
$ fc-list :lang=zh  
...  
/usr/share/fonts/noto-cjk/NotoSerifCJK-Regular.ttc: Noto Serif CJK SC:style=Regular
/usr/share/fonts/noto-cjk/NotoSansCJK-DemiLight.ttc: Noto Sans CJK JP,Noto Sans CJK JP DemiLight:style=DemiLight,Regular
/usr/share/fonts/noto-cjk/NotoSansCJK-Black.ttc: Noto Sans CJK JP,Noto Sans CJK JP Black:style=Black,Regular
/usr/share/fonts/noto-cjk/NotoSerifCJK-Light.ttc: Noto Serif CJK KR,Noto Serif CJK KR Light:style=Light,Regular
/usr/share/fonts/noto-cjk/NotoSansCJK-Medium.ttc: Noto Sans CJK JP,Noto Sans CJK JP Medium:style=Medium,Regular
/usr/share/fonts/noto-cjk/NotoSansCJK-Bold.ttc: Noto Sans CJK TC:style=Bold
/usr/share/fonts/noto-cjk/NotoSerifCJK-ExtraLight.ttc: Noto Serif CJK SC,Noto Serif CJK SC ExtraLight:style=ExtraLight,Regular
/usr/share/fonts/noto-cjk/NotoSansCJK-Bold.ttc: Noto Sans CJK SC:style=Bold
/usr/share/fonts/noto-cjk/NotoSansCJK-Medium.ttc: Noto Sans CJK TC,Noto Sans CJK TC Medium:style=Medium,Regular
```

> 上面的是我系统上安装的一部分字体，注意每行**第一个冒号**到其后**第一个逗号**之间的内容  

在最初例子的基础上我们需要再加两个参数使 pandoc 可以正常识别并转换文档，这两个参数分别是 `--pdf-engine=xelatex` 和 `-V CJKmainfont='Noto Sans CJK SC`。其中 `--pdf-engine` 是用来指定 pandoc 在转换过程中使用的模板，后者可以使 Pandoc 正常地识别源文档中的汉语等非英文语言，`CJKmainfont` 用来指定你要使用的字体（也就是上面使用 `fc-list` 命令列出的，每一行第一个**冒号**到其后第一个**逗号**之间的内容），比如我选择的是 'Noto Sans CJK SC'，这里的字体名称区分大小写，如果字体间有空白的话（就像 Noto Sans CJK SC)，一定要用引号把它们扩起来，单引号双引号都可以。

好了，修改后的命令如下：  
```shell
$ pandoc test.md -o test.pdf --pdf-engine=xelatex -V CJKmainfont='Noto Sans CJK SC'
```

这下就可以应付绝大多数需要将其他类型的文档转换成 PDF 格式的需求了。  

## 一些坑  

最近在使用 pandoc 命令将 markdown 文件转换成 pdf 文件时，发生了一些之前没遇到的错误。  

```shell
$ pandoc test.md -o test.pdf --pdf-engine=xelatex -V CJKmainfont='Noto Sans Mono CJK SC'
Error producing PDF.
! LaTeX Error: File `environ.sty' not found.

Type X to quit or <RETURN> to proceed,
or enter new name. (Default extension: sty)

Enter file name: 
! Emergency stop.
<read *> 
         
l.42 \file_if_exist:nT
```

上面的错误提示缺少了 environ.sty 这个文件，后来在 `texlive-latexextra` 这个包中发现了 `environ.sty` 这个文件。  

texlive-latexextra 包位于官方源中，可以直接使用 `pacman` 命令安装。  

除此之外，这个包中还有很多额外的 .sty 文件。具体的文件列表可以在 [此处](https://www.archlinux.org/packages/extra/any/texlive-latexextra/files/) 查看
