---
title: "在 Github Pages 上部署 Hugo"
date: 2019-08-22T13:15:09+08:00
lastmod: 2019-08-22T13:15:09+08:00
tags: ["blog","hugo"]
author: "dianbanjiu"
toc: false
---

其实在 github pages（以下简称 pages）上部署 hugo 的过程很简单，如何保证这些文件的迁移才是有点麻烦的事，不过最严重的问题其实在于你是否有足够的精力写下去。  

### 需要准备的一些东西

- github 帐号
- hugo
- git

### 开始

git 跟 hugo 这两个软件包应该在很多 Linux 发行版中都可以找到，可以直接使用系统对应的包管理进行安装即可。  
安装完之后可以使用下面的命令进行确认  
```shell
$ hugo version
```

然后使用下面的命令创建站点
```shell
$ hugo new site mysite 
Congratulations! Your new Hugo site is created in /home/xxx/demo/mysite.

Just a few more steps and you're ready to go:

1. Download a theme into the same-named folder.
   Choose a theme from https://themes.gohugo.io/ or
   create your own with the "hugo new theme <THEMENAME>" command.
2. Perhaps you want to add some content. You can add single files
   with "hugo new <SECTIONNAME>/<FILENAME>.<FORMAT>".
3. Start the built-in live server via "hugo server".

Visit https://gohugo.io/ for quickstart guide and full documentation.

```

输入命令后基本上立刻就可以看到输出的 “congratulation ...”，这说明创建成功了。  

刚创建好的站点一般包含下面几个目录
```shell
mysite
├── archetypes
│   └── default.md
├── config.toml
├── content
├── data
├── layouts
├── static
└── themes
```

**注意：** 如果没有指明操作路径，默认就是在 mysite 下。  

接下来去 [hugo 主题站](https://themes.gohugo.io/) 挑一个自己喜欢的主题，然后将其下载到 themes 目录中。下面以 hugo-theme-jane 为例。
```shell
$ cd mysite
$ git clone https://github.com/xianmin/hugo-theme-jane themes/jane
```

在 jane 目录下有一些已经比较完善的配置文件，像是 jane/exampleSite 下的 config.toml 以及 jane/archetypes/default.md，前者是用来配置站点的环境的，后者则是编写的文章的模板文件，一般这些文件里都会有比较详细的说明，根据它的说明进行修改就可以了。  

修改完成之后，就可以创建一篇文章来测试一下  
```shell
$ hugo new post/helloWorld.md
```

这条命令将会在 mysite/content/post 下创建 helloWorld.md 这个文件，编辑这个文件，并向其中天际一些内容。使用下面的命令来进行预览  
```shell
$ hugo server -D
```

打开浏览器，输入 http://localhost:1313 进行预览。  

如果预览之后觉得满意，就可以使用下面的命令将站点部署至 pages（请将下面的 Name 修改为你的 github 用户名），在开始之前，先在 github 上创建一个名为 Name.github.io 的 repo
```shell
$ hugo
$ cd public
$ git init
$ git add .
$ git remote add origin https://github.com/Name/Name.github.io.git
$ git commit -m "create blog site"
$ git push origin master
```

在浏览器输入 Name.github.io 就可以查看部署好的站点。

### 备份

这样 hugo 站点基本就算是部署完成了，不过这些文件都是 hugo 编译好的一些静态文件，为了避免更换系统之后无法更新博客，你需要将 mysite 站点下除了 public 之外的目录都进行一个备份（之所以不备份 public 目录，是因为在执行 `hugo` 命令之后都会重新生成 public 目录）。  

可以在 github 创建一个新的 repo 来存放这些文件。假设这个 repo 名为 Blog
```shell
$ git init
$ git submodule add https://github.com/xianmin/hugo-theme-jane.git themes/jane
$ echo "public/" >> .gitignore
$ git add .
$ git commit -m "backup blog"
$ git push origin master
```

上面的第二步使用 submodule 参数是因为 themes/jane 目录本身也是一个使用 git 进行版本控制的目录。而 public 虽然也是一个使用 git 进行版本控制的目录，但是因为它已经被添加到 .gitignore 文件当中，所以是不会被 git 索引的。  

如果你换了系统之类的，先将 Blog 这个 repo 克隆到本地，然后再将主题克隆一遍就可以像之前那样继续你的写作了。  
```shell
$ git clone https://github.com/Name/Blog.git && cd Blog
$ git clone https://github.com/xianmin/hugo-theme-jane.git themes/jane
```

除此之外，你还可以尝试使用 travis ci 对 Blog 目录进行持续集成，每次只需要更新 Blog 这个 repo，Name.github.io 就会自动更新，不过我之前一直没有配置成功，所以就只能自己手动执行了。
