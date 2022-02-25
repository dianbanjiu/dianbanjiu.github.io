---
title: "使用 Gitlab Ci 构建 Docker 镜像"
date: 2022-01-04T20:31:56+08:00
lastmod: 2022-01-04T20:31:56+08:00
tags: ["docker","gitlab","gitlab-runner"]
author: "dianbanjiu"
comment: true
headPic: ""
---

注意：下面示例的 gitlab-runner 是直接装在物理机器上的，并且对应的镜像仓库使用的是 http 协议

1、首先在安装 gitlab runner 的机器上安装 docker

2、创建 `/etc/docker/daemon.json`，内容如下  
如果你的 gitlab 是使用的 HTTPS 协议，可以跳过这一步  

```json
{
	"insecure-registries": ["registry_addr", "0.0.0.0"]
}
```

3、重启 docker

```shell
systemctl restart docker.service
```

4、为项目注册 gitlab-runner，其中执行者选择 `docker`

5、修改项目对应 gitlab-runner 的配置文件，默认情况为 `/etc/gitlab-runner/config.toml`，在项目对应 runner 的 `volumes` 后新增 `"/var/run/docker.sock:/var/run/docker.sock"`的映射

```toml
[[runners]]
  url = "https://gitlab.com/"
  token = TOKEN
  executor = "docker"
  [runners.docker]
    tls_verify = false
    image = "docker:20.10.12-dind"
    privileged = false
    disable_cache = false
    volumes = ["/cache", "/var/run/docker.sock:/var/run/docker.sock"]
  [runners.cache]
    [runners.cache.s3]
    [runners.cache.gcs]
```

6、更新 `.gitlab-ci.yml`，大体内容如下

```yaml
stages: 
  - docker_build

job_docker_build:
  stage: docker_build
  image: docker:20.10.12-dind
  script:
    - docker login -u $REGISTRY_USER -p $REGISTER_PASSWORD $REGISTRY_ADDR
    - docker build -t $REGISTRY_ADDR/test/hello:latest .
    - docker push $REGISTRY_ADDR/test/hello:latest
```

上面的 `$REGISTRY_USER`、 `$REGISTER_PASSWORD`、 `$REGISTRY_ADDR` 是定义在对应仓库的环境变量，分别代表镜像仓库的用户名、密码以及镜像仓库的地址。其中地址是不包含协议部分的，一般为： `my.registry.com`

待 gitlab-runner 的流水线执行完成之后，你应该就可以在镜像仓库看到刚刚构建好的镜像了