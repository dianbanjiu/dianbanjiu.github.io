---
title: "Github Action 自动修改文章的更新日期"
date: 2022-06-08T00:25:49+08:00
lastmod: 2022-06-08T00:25:49+08:00
tags: ["hugo","github"]
author: "dianbanjiu"
comment: true
headPic: ""
---

我博客写的多是一些偏向于技术或者工具使用方面的文章，随着技术或者工具的更迭，我也需要时不时地更新一下历史文章。在没有使用 action 之前我一直是手动修改文章更新时间，这种方式虽然可行，但是多少有点繁琐。在使用 action 自动部署博客之后，我就想能不能通过 action 自动修改文章的更新时间  

结合官方文档，目前确定了整体的流程，大致需要下面三步  

## 1、修改 archetypes/default.md

**archetypes/default.md** 是博客文章的默认模板，当你使用 `hugo new post` 创建文章的时候，hugo 会把这个模板内容填充到你新建的文章中。你需要在其中新增下面一行，当你创建新文章的时候，默认会把当前日期填充为文章的更新日期  
```markdown  
lastmod: {{ .Date }}
```

当你设置了第二步之后，这里实际展示的数据会根据第二步的设置进行替换  
## 2、修改 config.toml/yaml/json

修改博客的配置文件，在其中新增下面两行  
```toml  
[frontmatter]
  lastmod = [":git", "lastmod", ":fileModTime"]
```  
- `:git`: 以文件的提交时间作为更新时间
- `lastmod`: 以文章里面的 lastmod 字段作为更新时间
- `:fileModTime`: 以文件的修改时间作为更新时间

你可以根据自己的需要设置/组合这些参数，这几个参数的生效顺序就是根据他们在数组中的顺序决定的  

如果你像我一样使用文章 git 提交的时间作为文章更新时间的话，还需要在博客的配置文件中新增下面一行  
```toml  
enableGitInfo = true
```

关于这部分更多的详细配置，你可以参考延伸阅读[^1]  
## 3、修改 action 配置文件（可选）

如果你想以文章的 git 提交时间作为文章的更新时间的话，你还需要修改一下 action 的配置文件。下面是我的 action 的配置文件    

```yaml
name: Build and Publish Blog

on: 
  push:
    branches:
      - blog

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@master
        with:
          fetch-depth: 0
      - name: Disable quotePath
        run: git config --global core.quotePath false
      - name: setup hugo
        uses: peaceiris/actions-hugo@v2
        with:
            hugo-version: 'latest'
      - name: build hugo
        run: hugo --gc --minify --cleanDestinationDir
      - name: deploy hugo
        uses: peaceiris/actions-gh-pages@v2
        env:
          ACTIONS_DEPLOY_KEY: ${{ secrets.ACTIONS_DEPLOY_KEY }}
          PUBLISH_BRANCH: master
          PUBLISH_DIR: ./public
```

注意上面的  `Checkout` 和 `Disable quotePath` 阶段

其中 `Checkout` 阶段需要有下面两行，在这里  **fetch-depth** 的默认值是 1，也就是说它默认只会拉取分支最近的一次 commit，这可能会导致一些文章的 `GitInfo` 变量无法获取到。设置为 0 代表拉取所有分支的所有提交  
```yaml
with:
    fetch-depth: 0
```

默认情况下，当文件名中包含中文的时候，git 会使用引号把文件名括起来，但是这会导致 action 中无法读取 `:GitInfo` 变量，所以这里需要有 `Disable quotePath` 阶段


通过上面这些操作之后，你可以尝试修改一篇历史文章，并提交到 GitHub 上，待 action 成功运行完成之后，你应该就可以看到效果了  

[^1]:  hugo 日期配置官方文档: https://gohugo.io/getting-started/configuration/#configure-dates