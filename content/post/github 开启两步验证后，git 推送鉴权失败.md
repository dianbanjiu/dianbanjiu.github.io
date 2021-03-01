---
title: "Github 开启两步验证后，git 推送鉴权失败"
date: 2019-10-03T11:05:02+08:00
lastmod: 2019-10-03T11:05:02+08:00
tags: ["Linux", "git", "github", "两步验证"]
author: "dianbanjiu"
---

最近开启了 Github 的两步验证，然后在桌面上使用 git 向 Github 推送文件的时候总是出现`鉴权失败`的问题，在排除了密码错误的原因之后，觉得应该就是两步验证的锅了。  

下面是解决办法：  

1. 打开 Github 的 「[Settings](https://github.com/settings/profile)」
2. 进入 「[Developer settings](https://github.com/settings/apps)」
3. 打开 「[Personal access tokens](https://github.com/settings/tokens)」
4. 点击 「Generate new token」，生成一个新的 token

**生成 token 之后千万记得把 token 复制并保存好，因为 github 为了安全起见，这个 token 只会出现一次，也就是说如果你以后忘记了这个 token，只能重新创建一个。**  

push 之前记得先要配置好用户名跟密码：  

```shell
$ git config --global user.name "username"
$ git config --global user.email "email@example.com"
```

之后再 push 的时候将密码替换为上面生成的 `access token` 就可以了。  

---

你还可以创建一个 `.git-credentials` 文件 (文件名可以任意，这里只是为了表义)，其中的内容（依规则替换掉其中的用户名和密码/access token）：

```
https://{username}:{password}@github.com
```

在终端输入下面的命令：  

```shell
$ git config --global credential.helper store --file ~/.git-credentials
```

`credential` 是用来配置 git 的凭证存储的参数。   
`--file` 后面指定的是凭证存放的位置。  

这种方式可以避免每次 push 的时候都需要输入密码，但是因为密码是`明文`存储在本地，可能不是很安全。  

---

不过貌似这个文件并不是强制的，我在使用 access token push 完之后，直接执行：  

```shell
$ git config --global credential.helper store
```

在之后的推送中也不再需要输入用户名和密码了。  