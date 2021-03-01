---
title: "在Hugo博客中添加一个新的页面"
date: 2020-06-11T23:18:16+08:00
lastmod: 2020-06-11T23:18:16+08:00
tags: ["hugo"]
author: "dianbanjiu"
---

![](https://pixabay.com/get/57e8d2454c57a514eade867dce21357d083edbec5259784a702e7a.jpg)  
<center style="color:#C0C0C0;font-size:13px; text-decoration:underline">该图片由<a href="https://pixabay.com/zh/users/qimono-1962238/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=1876659">Arek Socha</a>在<a href="https://pixabay.com/zh/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=1876659">Pixabay</a>上发布</center>


这里说的页面不是指 hugo 利用 markdown 文件生成的页面，而是我们自己手动创建的 html 页面。  

这个过程其实并不复杂，但是你需要有一些前端开发的经验，起码的 HTML、CSS、JavaScript 基础你需要了解一些，其次不太推荐使用类似 Vue 这样的 JS 框架，因为 hugo 的页面使用了大量的 golang 模板语法，就是你在主题文件中看到的 `{{}}` 语法，而 Vue 中也有到这个语法，这可能会在无意中给你的开发造成障碍，而且后期再看或者修改这些代码的时候也很有可能会混乱。  

屁话说得有点多了，下面正式开始。

### TODO
首先在根目录下的 `static` 下创建一个目录，目录名是你的页面名。比如我想通过 https://www.dianbanjiu.com/just-talk 来访问这个页面，那目录名就需要命名为 `just-talk`，下面我就以这个名字为例。  

在 just-talk 下创建 index.html 文件，并复制 public 目录下 404.html 里面的内容到 just-talk/index.html 中，接着你就可以根据自己的需求修改 just-talk/index.html 文件。  

创建 just-talk 还有另外一种途径，首先在 content 目录下创建 just-talk.md 文件，文件表头部分的填充内容与其他的文章类似，然后使用 `hugo` 命令生成博客内容，这时候打开 public 目录，你可以看到出现了一个 just-talk 目录，将这个目录移动到 static 目录下，并将之前的 just-talk.md 文件删掉，之后修改 just-talk/index.html 里面的内容为你需要的即可。  

通过 `hugo` 命令可以将 static 下的内容原封不动的复制到 public 目录，所以在你修改完 just-talk/index.html 之后，就可以直接使用 `hugo` 生成页面。  

如果你需要将该页面添加到导航栏，可以参照配置文件中其他导航项的结构，将其添加进去即可，比如我的 toml 设置方式：  
```toml
[[menu.main]]
  name = "一言"
  weight = 20
  url = "just-talk"
  identifier = "just-talk"
```

你可以使用 `hugo server -D` 预览一下效果。  

### Little Tips 
1. 当你使用第二种方式创建完 just-talk 目录之后一定要删除 just-talk.md 文件，否则你即便修改了 just-talk/index.html 文件，修改结果也不会同步到你的博客中，因为 hugo 首先获取的是 just-talk.md 中的内容，只有在这个文件不存在时，才会使用 static 中的内容。  
2. public 目录中的内容只会修改，而不会删除。比如你之前创建了一篇名为 hello.md 的文章，使用 `hugo` 命令生成博客内容后，即便你手动删除了 hello.md，并再次使用 `hugo` 指令生成博客内容，你会发现你博客中依然会存在 hello 这么一篇文章。如果你想彻底删除它，除了删除 hello.md 外，还需要手动将 public 中对应的文件删除掉才行。所以建议可以先整个删除 public 目录，然后再使用 `hugo` 命令生成博客内容。

### Something others  
[golang template](https://golang.org/pkg/html/template/#hdr-Contexts)  
[Vue 模板语法](https://cn.vuejs.org/v2/guide/syntax.html)  
[前端教程](https://www.runoob.com/)  
