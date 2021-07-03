---
title: "😶‍🌫️在 Supervisor 中调用环境变量"
date: 2021-07-02T22:29:31+08:00
lastmod: 2021-07-02T22:29:31+08:00
tags: ["supervisor"]
author: "dianbanjiu"
comment: false
headPic: ""
---
> 以前总觉得写东西就得上纲上线，一篇不整个几千字都不敢发出来。现在想想，我又不是奔着出书的目的去的，我只是纯粹的想记录一些东西，又何必限制于篇幅呢  

supervisor 调用的环境变量分为两种，一种是在 supervisor 配置文件中使用系统已经定义好的环境变量，另一种为 supervisor 子程序运行过程中使用的环境变量。  

## supervisor 配置文件中调用系统环境变量
此类环境变量一般用来定义一些预先无法确定的变量，比如可以自定义安装位置的程序，可以将其主目录定义在系统环境变量中，如果程序的安装位置发生了变化，只需要修改一次环境变量中对应的值即可，而无需再对 supervisor 的配置文件进行修改。  

在 supervisor 的配置文件中调用系统的环境变量时，需要以 `%(ENV_xxxx)s` 的格式进行调用。假如我的系统环境变量中有这样一个环境变量 `export MY_HOME=/opt/home`，那我在 supervisor 中就要以 `%(ENV_MY_HOME)s`进行调用。  

## 定义子程序运行的环境变量
此类环境变量一般用来定义子程序所依赖的一些库文件。  

子程序的环境变量需要在自己的 `[program:x]` 下新增 `environment` 字段。如 `environment=LD_LIBRARY=/opt/home/lib`。

下面是一个带有上面两种环境变量定义的简单例子：  
```shell
$ cat supervisor.conf
[inet_http_server]
port=0.0.0.0:9111

[program:hello]
depends_on=hello
command=%(ENV_MY_HOME)s/hello
directory=%(ENV_MY_HOME)s/hello
autostart=true
autorestart=true
stopwaitsecs=3
stdout_logfile=%(ENV_MY_HOME)s/logs/hello.log
stderr_logfile=%(ENV_MY_HOME)s/logs/hello.log
stdout_logfile_maxbytes=10485760
stdout_logfile_backups=2
stderr_logfile_maxbytes=10485760
stderr_logfile_backups=2
environment=LD_LIBRARY=%(ENV_MY_HOME)s/lib

$ echo $MY_HOME
/opt/home
```