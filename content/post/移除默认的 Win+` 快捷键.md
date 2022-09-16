---
title: "移除默认的 Win+` 快捷键"
date: 2022-09-17T02:00:26+08:00
lastmod: 2022-09-17T02:00:26+08:00
tags: ["powershell", "windows"]
author: "dianbanjiu"
comment: true
headPic: ""
---

在 Windows 上安装 `终端` 之后，**Win+`** 快捷键会以聚焦模式打开终端（下拉式终端的样子）。这个快捷键可以通过下面的方式来移除

1. 打开终端的设置界面，点击左下角的 **打开 JSON 文件**  
![terminal_setting_interface.png](https://s2.loli.net/2022/09/17/b2GDLoEchN4wknK.png)  
2. 在 `actions` 数组中新增下面的内容  
![terminal_setting_file.png](https://s2.loli.net/2022/09/17/k3luvNfP5pa2rjx.png)  

```json
{
  "command": null,
  "keys": "win+`"
}
```
最后重启 `终端` 即可