---
title: "Windows配置nginx"
date: 2024-06-13T00:45:35+08:00
lastmod: 2024-06-13T00:45:35+08:00
tags: ["windows", "nginx", "开发"]
author: "dianbanjiu"
comment: true
headPic: ""
---

1、安装 nginx  
我是通过 scoop 安装的 nginx  
```shell
scoop update
scoop install nginx
```

2、测试 nginx  
```shell
nginx -p "$env:NGINX_HOME"
```

命令执行成功之后，在浏览器输入 http://127.0.0.1:80 就可以看到 nginx 的输出信息了

3、以后台进程的方式启动 nginx  
通过上面的命令启动 nginx 后可以发现，nginx 进程会占用当前的终端，可以通过 pwsh 的命令将 nginx 作为后台进程启动  
```shell
start -WindowStyle Hidden nginx -ArgumentList "-p $env:NGINX_HOME"
```

因为是通过 scoop 安装的，对应的配置文件也是位于 scoop 的目录下，所以启动 nginx 的时候也需要通过`-p`指定配置对应的目录  

nginx 的配置目录默认位于 `$HOME\scoop\persist\nginx`，如果需要增加新的 server 配置，也需要配置在这个目录下