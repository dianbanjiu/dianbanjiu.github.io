---
title: "Gitlab Runner 运行报错（1）"
date: 2021-06-28T21:42:16+08:00
lastmod: 2021-06-28T21:42:16+08:00
tags: ["gitlab","docker"]
author: "dianbanjiu"
comment: false
headPic: "https://cdn.pixabay.com/photo/2021/06/22/16/39/arch-6356637_960_720.jpg"
---

> 今天在《也谈钱》的公众号看到一个很有意思的观点：
>
> 你所有的工作都包含两部分：你可以带走的和带不走的，如果你只注意自己带不走的部分，那你就是在为老板工作，注定会很累，如果你将注意力放在你可以提炼带走的部分，那你就是在为自己工作

## 问题描述及解决方案
今天在给新项目添加了 Runner 之后，出现了打包失败的情况，下面是基本的情况以及解决方案。  

项目情况：项目使用 `gitlab-runner register` 添加的 Runner，执行者为 Docker，使用的是自定义的 node 镜像，镜像名称为 `node:v12`。  

在将代码提交到 gitlab 之后，自动触发 CI，但是 CI 提示下面的报错：  
```
Running with gitlab-runner 13.0.0 (c127439c)
   on xxx ddd T8Hqqyzz
Preparing the "docker" executor
00:07
 Using Docker executor with image node:v12 ...
 Pulling docker image node:v12 ...
 ERROR: Job failed: Error response from daemon: manifest for node:v12 not found: manifest unknown: manifest unknown (docker.go:198:3s)
```

可以看到，上面的报错是出现在拉取 Docker 镜像的过程中，提示 Docker 找不到名为 `node:v12` 的镜像。但是我在运行 Runner 的机器上已经构建好了 node:v12 的镜像，为什么还是会拉取失败呢？

其实是因为 Gitlab Runner 的 Docker 执行者默认是直接从 Docker Hub 拉取你指定的镜像，即使你本地已经有了需要的镜像，所以当它在 Docker Hub 找不到 `node:v12` 的镜像时，就会出现上面的报错。  

要解决这个问题也很简单，就是在 gitlab runner 的配置文件中添加 `pull_policy = "if-not-present"` 的参数，添加方式如下：  
```toml
[[runners]]
  name = "runner test"
  url = "http://www.gitlab.com/"
  token = "T9hppyxxbwN_PZNen1_o"
  executor = "docker"
  [runners.custom_build_dir]
  [runners.cache]
    [runners.cache.s3]
    [runners.cache.gcs]
  [runners.docker]
    tls_verify = false
    image = "node:v12"
    privileged = false
    disable_entrypoint_overwrite = false
    oom_kill_disable = false
    disable_cache = false
    volumes = ["/cache"]
    # 在下面设置
    pull_policy = "if-not-present" 
    shm_size = 0
```

## 关于 `pull_policy` 参数
`pull_policy` 参数是用来指定当你使用 Docker/Docker machine 作为执行者时镜像的拉取策略。该参数共有三个可选项，你可以设置零个或者多个选项作为该参数的值。  
- never：这个选项意味着仅使用本地的 Docker 镜像，任何你需要的镜像都必须先手动拉取到本地，否则在运行 CI 的时候就会出现下面的报错
```
Pulling docker image node:latest ...
ERROR: Build failed: Error: image local_image:latest not found
```

- if-not-present：这个选项会先在本地检查是否有指定的镜像，没有的话会在 Docker Hub 拉取。如果你想避免从 Docker Hub 拉取镜像的时间，又有使用自定义镜像的需求，可以使用该选项。  
- always：`pull_policy` 的默认值，总是从 Docker Hub 拉取镜像。  

**参数的设置方式：**  
- 不设置：在不添加 `pull_policy` 字段的情况下，`pull_policy` 默认为 always。  
- 单值设置： `pull_policy = "if-not-present"`。
- 多值设置： `pull_policy = ["always", "if-not-present"]`（暂未遇到过具体的使用场景）。