---
title: "搭建memos"
date: 2024-02-22T00:36:16+08:00
lastmod: 2024-02-22T00:36:16+08:00
tags: ["memos", "linux"]
author: "dianbanjiu"
comment: true
headPic: ""
---

最近看到了一个比较有意思的服务 Memos，支持自己部署，所以我也来搞了一个玩玩，下面是大概的流程

## 1、安装 docker

我服务器使用的是 Centos8，如果你使用的是其他发行版可以参考 [Docker 官方的说明文档](https://docs.docker.com/engine/install/)

首先卸载已经安装的旧 docker

```shell
sudo yum remove docker \
    docker-client \
    docker-client-latest \
    docker-common \
    docker-latest \
    docker-latest-logrotate \
    docker-logrotate \
    docker-engine
```

安装 yum-utils 包

```shell
sudo yum install -y yum-utils
```

添加 docker 软件源

```shell
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

安装最新版的 docker

```shell
sudo yum install docker-ce
```

启动 docker，并将它设置为开机自启

```shell
sudo systemctl start docker
sudo systemctl enable docker
```

确认 docker 启动成功

```shell
sudo docker info
```

## 2、安装 Memos

官网提供了 docker 和 docker-compose 的部署方式，因为只有一个服务，所以我选择直接使用 docker 部署

[官方文档链接](https://www.usememos.com/docs/install/self-hosting)

先创建一个目录用来存放 Memos 的数据文件

```shell
mkdir /opt/memos
```

安装 Memos

```shell
docker run -d \
  --init \
  --name memos \
  --publish 5230:5230 \
  --volume /opt/memos/:/var/opt/memos \
  neosmemo/memos:stable
```

上面命令执行完之后 Memos 就跑起来了，可以使用 http://ip:5230 来测试一下

## 3、添加域名解析

添加一条 A 记录，如 memos.dianbanjiu.com 解析到自己服务器的公网 IP 上

1. 在 cloudflare 选择自己的站点
2. 在侧边栏选择 DNS->记录
3. 选择添加记录按钮，添加一条 A 记录（类型选择 A，名称输入 memos，IPV4 地址填自己的服务器公网 IP）

后续我会把服务改为 https 访问，而我的域名是托管在 cloudflare 上的，所以可以直接用 cloudflare 生成一份 15 年有效期的证书

1. 在 cloudflare 选择自己的站点
2. 在侧边栏选择 SSL/TLS->源服务器
3. 点击创建证书按钮，根据提示创建即可。推荐生成 pem 格式的私钥

将证书保存为 dianbanjiu.com.pem，将私钥保存为 dianbanjiu.com.key

将生成的两个文件上传至服务器的 /etc/nginx/cert 下

## 4、安装 nginx

官方软件源里的 nginx 版本可能比较老，可以选择安装 nginx 软件源，以安装最新的 nginx

```shell
rpm -ivh http://nginx.org/packages/centos/8/x86_64/RPMS/nginx-1.24.0-1.el8.ngx.x86_64.rpm
```

上面的命令会直接安装好 1.24.0 版本的 nginx，这也是当前最新版本

启动 nginx 并设置开机自启

```shell
sudo systemctl start nginx
sudo systemctl enable nginx
```

在浏览器直接访问 http://ip 应该就可以看到 nginx 的输出了

新增 Memos 的 nginx 配置，在 /etc/nginx/conf.d 中新增一个 memos.dianbanjiu.com.conf 文件。文件内容如下

```nginx
server {
    listen       443 ssl http2;
    server_name  memos.dianbanjiu.com;

    ssl_certificate /etc/nginx/cert/dianbanjiu.com.pem;
    ssl_certificate_key /etc/nginx/cert/dianbanjiu.com.key;
    ssl_prefer_server_ciphers on;

    access_log  /var/log/nginx/memos.access.log  main;

    location / {
        proxy_pass http://127.0.0.1:5230;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

}

server {
        listen 80;
        server_name memos.dianbanjiu.com;
        rewrite ^/(.*)$ https://memos.dianbanjiu.com/$1 permanent;
}

```

重新加载 nginx 配置

```shell
nginx -t #检查配置文件是否有误
nginx -s reload
```

直接通过浏览器访问 https://memos.dianbanjiu.com 确认服务是否安装成功

如果你是在国内服务器上搭建的 Memos，服务在短暂能访问之后就出现再也无法访问的情况，有可能是因为域名未备案导致被服务商直接拦截了，这种情况下要么去备案并自己修改页面在最下面加上备案信息，要么就只能通过 ip 访问。
