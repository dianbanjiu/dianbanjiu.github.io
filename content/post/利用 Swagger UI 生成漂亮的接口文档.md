---
title: "利用 Swagger UI 生成漂亮的接口文档"
date: 2021-08-08T13:56:16+08:00
lastmod: 2021-08-08T13:56:16+08:00
tags: ["swagger","docker"]
author: "dianbanjiu"
comment: true
headPic: ""
---

最近在跟前端进行对接的时候，规定了后端每个接口都必须要写接口文档，并且必须生成对应的 .json/.yaml 文件，这样他们就可以通过接口文件直接生成对应的 ts 文件，能够准确的进行接口参数的对接。这一部分现在我已经通过 Gitlab CI + 文件服务器的方式实现了。

但是作为后端，有时候想看看现在有什么接口，接口里有什么参数，之前只能通过看代码，现在有了接口文件，那当然是要搞个花里胡哨的东西来展示一下了。既然要看 Swagger 文档，最简单且有效的工具当然就是 Swagger 官方提供的 Swagger UI 了。

下面就来看看怎么来整一个 Swagger UI 玩玩。

## **安装**

我们下面使用 Docker 来安装 Swagger UI。

```bash
$ docker pull swaggerapi/swaggerui
```

## **启动**

```bash
$ docker run -d -p 80:8080 -v ~/swagger:/usr/share/nginx/html/swagger --name swagger-ui swaggerapi/swagger-ui

```

- `d` 会让该容器以守护进程的形式在后台运行
- `p` 映射容器的 8080 端口到本机的 80 端口
- `v` 挂载本机的 `~/swagger` 目录到容器的 `/usr/share/nginx/html/swagger`
- `-name` 设定该容器的名字，方便以后通过名字对容器进行一些操作

启动完成之后，你就可以将已有的 swagger 文档（.yaml、.json 文件）放到 `~/swagger` 目录下（假设该目录下已经有了一个名为 swagger.json 的文件），打开浏览器，访问 `http://ip` 页面，在搜索框输入 `swagger/swagger.json` 就可以查看你的接口文档了。

![https://i.imgur.com/rR7fnLm.png](https://i.imgur.com/rR7fnLm.png)

## **说明**

下面是 Swagger UI 容器中 nginx 的配置文件，在容器中的 `/etc/nginx/nginx.conf`

```
worker_processes      1;

events {
  worker_connections  1024;
}

http {
  include             mime.types;
  default_type        application/octet-stream;

  sendfile on;

  keepalive_timeout   65;

  gzip on;
  gzip_static on;
  gzip_disable "msie6";

  gzip_vary on;
  gzip_types text/plain text/css application/javascript;

  map $request_method $access_control_max_age {
    OPTIONS 1728000; # 20 days
  }
  server_tokens off; # Hide Nginx version

  server {
    listen            8080;
    server_name       localhost;
    index             index.html index.htm;

    location / {
      absolute_redirect off;
      alias            /usr/share/nginx/html/;
      expires 1d;

      location ~* \.(?:json|yml|yaml)$ {
        #SWAGGER_ROOT
        expires -1;

        include cors.conf;
      }

      include cors.conf;
    }
  }
}

```

Swagger UI 的容器中启动了一个 nginx 服务，通过 nginx 来指定你需要访问的接口文档，默认情况下，该容器中 nginx 监听的是 8080 端口。

可以看到，当你访问 http://ip:80/ 时，它默认访问的是 `/usr/share/nginx/html/index.html` ，当你在搜索框中输入一些文档名时，它会直接在 `/usr/share/nginx/html/` 下找，找到了就会展示出来，找不到就会直接报错。所以上面才需要将本机存放接口文档的目录挂载到 `/usr/share/nginx/html/` 下。

当然，无论是端口还是文档的存放位置，你都是可以通过修改镜像/容器的方式来更改。