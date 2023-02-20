---
title: "给我的Blog加上搜索功能"
date: 2023-02-20T23:25:11+08:00
lastmod: 2023-02-20T23:25:11+08:00
tags: ["hugo", "pagefind"]
author: "dianbanjiu"
comment: true
headPic: ""
---

之前一直想给博客加上搜索功能，但是看了好几个方案都是不再维护的状态，后来在《阮一峰的网络日志》中看到了 [pagefind](https://pagefind.app/) 这个工具，配置比较简单，而且使用上也算比较方便。因为我是计划新增一个页面专门用来给搜索页面用，下面的介绍也是以新增搜索页面的方式来介绍  

1、在 content 目录下新增一个 search 目录，并在 search 目录中新建一个 index.md 的文件，index.md 的内容如下  
```markdown
---
title: "🥰Find something~🥰"
layout: search
---
```
title 可以根据自己的需要随便修改  

2、在自己主题目录的 layouts/_default 下新增一个 search.html 的页面，页面的主体内容可以复制 layouts/_default/single.html，只需要将其中的 {{.Content}} 替换为下面的内容：  
```html
window.addEventListener('DOMContentLoaded', (event) => {
    new PagefindUI({
        element: "#search",
    });
});
```

我的 search.html 整个文件全文如下：  
```html
{{ define "content" -}}
<article class="post bg-white">
    <!-- post-header -->
    <header class="post-header">
        <h1 class="post-title">{{ .Title }}</h1>
        {{ partial "post/i18nlist.html" . }}
    </header>

    <!-- Content -->
    <div class="post-content">
        <link href="/pagefind/pagefind-ui.css" rel="stylesheet">
        <script src="/pagefind/pagefind-ui.js" type="text/javascript"></script>
        <div id="search"></div>
        <script>
            window.addEventListener('DOMContentLoaded', (event) => {
                new PagefindUI({
                    element: "#search",
                });
            });
        </script>

    </div>

</article>

{{- end }}
``` 

pagefind 默认会把创建好的索引等文件放在 _pagefind 目录下，但是在 GitHub page 上托管的时候，下划线开头的文件可能无法被获取到，导致搜索功能无法使用，我们下面要将 pagefind 的输出修改到其他位置  

3、在博客的根目录下创建一个 pagefind.yml 的文件，文件内容如下：  
```yaml
source: public
glob: post/*/*.{html}
bundle_dir: pagefind
```

- source 是指定我们通过 hugo 命令生成的博客静态文件的位置，一般都是在 public 目录  
- glob 是指定 pagefind 都需要索引哪些位置的哪些文件的。因为我的博客是放在 content/post 目录中的，生成到 public 之后对应的目录是在 post 中，在搜索的时候我也只想搜索这部分内容，所以需要单独指定一下。此处是可选的  
- bundle_dir 指定了 pagefind 生成的索引等文件在 public 目录的存放位置，我此处将其存放到 public/pagefind 中，如果你想将其修改到 public 下的其他目录中，需要同时修改上面 search.html 文件中 pagefind 的 js 和 css 文件的引入位置

4、在博客的配置文件中新增一个搜索路由，以我的 config.toml 文件为例，在文件中新增下面的内容：  
```toml
[[menu.main]]
  name = "搜索"
  weight = 50
  identifier = "search"
  url = "search"
```

5、运行下面的命令查看效果  
```shell
$ rm -rf ./public/* 
$ hugo
$ npx -y pagefind --serve
```

等索引完成之后，打开浏览器访问 http://localhost:1414/search 就可以看到效果了  
![](/img/pagefind_search_show.png)  

6、为了保证之后的可用性，我实际使用的是 pagefind 提供的二进制文件，这些预构建的文件在 GitHub 的 [release 页面](https://github.com/CloudCannon/pagefind/releases) 可以下载到，推荐下载带 _extended 的文件，因为它支持对中文的索引。二进制文件和 npx 的使用方式类似
```shell
$ pagefind_extended --serve
```

7、如果你像我一样使用的是 GitHub Action 自动构建博客的，并且也是用的是二进制包的方式进行索引，还需要在 action 的脚本中 deploy 之前加一个 step  
```yaml
- name: create search index
  run: tar -xf pagefind_extended.tar.gz && chmod u+x pagefind_extended && ./pagefind_extended && rm -f pagefind_extended
```
推荐在上传文件到 GitHub 的时候上传之前下载的压缩包，并且需要是 linux 系统 x86_64 架构的压缩包，这样可以减少一些不必要的上传带宽（压缩后的文件大概 44M，解压出来的有 222M）。为了方便起见，我这里把压缩包重新命名为 pagefind_extended.tar.gz  

等构建完成之后清理一下浏览器缓存，重新访问一下博客应该就可以看到搜索按钮和搜索页面了  

最后有一个点需要注意一下，因为 pagefind 是通过分词的方式来进行搜索的，所以搜索功能只会在命中分词的时候生效，比如我现在有一个词 `碳酸危机`，我搜索 `碳酸` 或者 `危机` 都可以搜到对应的内容，但是直接搜索 `碳酸危机` 会得不到任何搜索结果，这就是分词导致的  

不过这种分词的优势是可以大大减少索引文件的大小，如果采用 hugo 默认的生成 json 文件的方式，其中包含了你所有文章的所有内容，随着文章的增多生成的索引 json 会膨胀的比较快，而 pagefind 这种分词索引的方式只会在有新的分词的时候索引文件的大小才会有些微增加，生成的索引文件膨胀的就会慢很多