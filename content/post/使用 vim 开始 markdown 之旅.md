---
title: "使用 Vim 开始 Markdown 之旅"
date: 2020-05-03T23:14:39+08:00
lastmod: 2020-05-03T23:14:39+08:00
tags: ["vim","markdown"]
author: "dianbanjiu"
---

![img](https://i.imgur.com/GUHdWtf.jpg)  
<center style="color:#C0C0C0;font-size:13px; text-decoration:underline">爬山时遇到的小猫咪</center>


最近没事干，就想着给自己的 Vim 配置一下 markdown 环境，因为有现成的 markdown 预览插件，而且 vim 总体来说加载速度也快一些（说的就是比你 vscode 快🤪），说干就干。  

我使用的系统是 Manjaro Linux，所以下面的介绍也是基于 Linux 的，其他系统的配置过程可能会略有不同。  

> 开始之前你需要先简单了解 vim 的几个编辑模式。  

> 在我们刚打开一个 vim 窗口的时候，可以看到 vim 的左下角会有一个 Normal 字样，这个模式下你无法在 vim 窗口中输入内容，只能使用一些定义好的指令，比如按下 `dd` 可以删除光标所在的一整行。或者当你按下 `:` 的时候将会开启命令模式，常见的保存并退出就是在这个模式下输入 `:wq`。  

> 在 Normal 模式下按下 `i` 就会进入 Insert 模式，这个模式下你可以在 vim 窗口中输入内容。输入完毕之后可以通过按下 \<Esc\> 键来返回 Normal 模式。  

> 在 Normal 模式下按下 `v` 将会进入 Visual 模式，这个模式下你可以对文本进行复制等操作，同样可以通过按下 \<Esc\> 来返回 Normal 模式。  

## 安装
首先去 vim 官网 [下载 vim](https://www.vim.org/download.php)。官网提供了当下大部分平台的安装程序，像是 Linux、MacOS、Windows。

Linux 用户可以使用自带的包管理器直接从源里安装。不过有些发行版的 vim 版本可能稍微老一些，如果你想用最新版的，可以选择自己手动构建，在 vim 的 github 项目里有相关教程，可以自行查阅。(手动构建的时候可以添加一个将 vim 的 buffer 内容复制到系统剪贴板的功能，而从源里安装的 vim 一般都是缺失这项功能的)  

下载好 Vim 之后就可以来配置 markdown 的环境了，首先你需要在主目录下新建一个 .vimrc 文件，这个文件就是当前用户的 vim 配置文件。  

markdown 编辑器最重要的一点就是得有预览支持，而 vim 本身并不支持这个功能，不过可以通过安装插件来添加预览功能。  

在安装插件之前你可能还需要一个 vim 的插件管理器，我使用的是 [vim-plug](https://github.com/junegunn/vim-plug)，你可以通过下面的命令来安装 vim-plug：  
```shell
$ curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

不过由于种种原因，从 github 下载文件可能会很慢，甚至无法下载。别担心，只要你还能访问 github，那就可以很容易解决这个问题。  

1. 首先在主目录下新建一个 .vim 目录
2. 在 .vim 目录下再新建一个 autoload 目录
3. 在 autoload 目录下新建一个 plug.vim 的文件
4. 打开 [这个链接](https://github.com/junegunn/vim-plug/blob/master/plug.vim)
5. 复制这里个链接里的内容到 plug.vim 中

这样 vim-plug 就安装好了。😁  

上面几步用到的命令：  
```shell
$ cd # 进入家目录
$ mkdir -p ~/.vim/autoload
$ touch ~/.vim/autoload/plug.vim
```

## 配置
打开你的 .vimrc 文件，现在里面还是空的，在里面先添加下面几行。  
```
call plug#begin('~/.vim/plugged')

Plug 'iamcco/markdown-preview.nvim', { 'do': 'cd app & yarn install'  }

call plug#end()
```

- 第一行和最后一行的 plug#start 和 plug#end 是为给 vim-plug 指明插件位置的，`~/.vim/plugged` 是插件之后的安装位置。  
- Plug 'iamcco/markdown-preview.nvim' 就是 markdown 预览插件本体了。在它后面可以看到还有一串 ` { 'do': 'cd app & yarn install'  }`，这条命令将会在 markdown-preview 源码下载好之后自动执行，用以构建它所需要的环境。如果你的系统上没有安装 yarn，那你可以将 `{ 'do': 'cd app & yarn install'  }`替换为`{ 'do': { -> mkdp#util#install() } }`。需要注意的是，在 Linux 下 `{ 'do': { -> mkdp#util#install() } }` 有一定的几率安装之后预览无法使用。  

至此 vim 的 markdown 环境基本配置完毕，打开一个 markdown 文件，然后输入 `:MarkdownPreview`，将会使用你的默认浏览器打开一个窗口，如下所示：  

![vim-markdown-preview](https://i.imgur.com/4kFLvwL.png)  

在预览完成之后你可以通过 `:MarkdownPreviewStop` 来关闭预览，如果你选择直接退出当前的 vim，那预览界面也会同时关闭。  

## 进阶操作
安装完预览插件之后，虽然已经可用了，但是如果能为 markdown 配置一些快捷操作，那肯定可以让你的文档编写效率更上一层楼。下面就来介绍一些常用的操作。  

最常用的操作那当然非预览莫属了，每次都要输入一长串的命令显然不太舒服，我们可以通过在 .vimrc 里面添加下面的一行代码来将预览绑定到 `,m` 组合键上。  
```
let g:mkdp_brower = 'chromium'
autocmd Filetype markdown noremap ,m :MarkdownPreview<CR>
autocmd Filetype markdown noremap ,ms :MarkdownPreviewStop<CR>
```

- 上面的第一句是将预览的默认浏览器设置为 chromium (可选，如果不指定则使用系统的默认浏览器打开)  
- 后两句开始的 `autocmd Filetype markdown` 声明只有在文件类型为 markdown 时这两个快捷键才会生效  
- 第二句是将 `:MarkdownPreview` 命令绑定到 `,m` 组合键上  
- 第三句是将 `:MarkdownPreviewStop` 绑定到 `,ms` 组合键上  

除此之外，你还可以将一些常用的样式，像是加粗、斜体、链接、图片等等根据自己的需要定义对应的快捷键。  
```
autocmd Filetype markdown noremap ,b i****<Esc>hi
```

在 Normal 模式下，按下 `,b`，将会依此执行 
1. `i` 进入 Insert 模式  
2. 插入四个 `*`  
3. 执行 `<Esc>` 指令返回 Normal 模式  
4. 向左移动一位将光标移到星号中间  
5. 执行 `i` 指令进入 Insert 模式

按照上面的格式你可以配置其他样式的快捷键。  

最后还需要提醒的是，这些组合键你不必同时按下，但是两个键按下的间隔不要超过 2 秒，否则只会执行你最后输入的那个键位对应的操作。  
