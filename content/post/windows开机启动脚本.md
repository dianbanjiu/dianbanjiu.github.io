---
title: "Windows开机启动脚本"
date: 2024-07-07T13:32:04+08:00
lastmod: 2024-07-07T13:32:04+08:00
tags: ["windows"]
author: "dianbanjiu"
comment: true
headPic: ""
---

目前手上有一台主力设备是 Windows，每次开机都需要手动启动一些服务，虽然之前也有写过一个脚本用来一键启动，但是还是有点麻烦，所以研究了一下如何在开机时自启动

我的脚本是通过 PowerShell 写的，所以先将自己的脚本封装到一个 .ps1 结尾的文件中。接着**将文件的打开方式设置为 PowerShell**  
注意：如果你之前有设置过 ps1 文件为其他的打开方式，比如记事本，那在下面的操作完成，电脑重启之后，默认就会通过记事本打开脚本，而不是直接运行。

具体操作，在文件上右键，选择打开方式，找到并选择 pwsh.exe，比如我是通过 scoop 安装的 PowerShell，对应的可执行文件的位置是在 `$HOME\scoop\shims\pwsh.exe`

将脚本放在 `$HOME\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup` 下即可

这个自启操作也适用于其他非脚本的应用程序，不过操作更简单，直接创建一个应用的快捷方式，然后将快捷方式放到 `$HOME\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup` 下即可

下面是一个关于脚本的示例

```pwsh
$ErrorActionPreference='SilentlyContinue'
Get-Process -Name hello | Stop-Process
Start-Process -WindowStyle Hidden -FilePath hello "-c $HOME/hello/conf.yaml"
```


- `ErrorActionPreference` 变量是设置脚本如何响应非终止错误，SilentlyContine 代表不显示错误并在不会中断的情况下继续执行，其他可选参数可自行参考pwsh文档
- `Get-Process -Name hello | Stop-Process` 尝试获取名字为 hello 的进程，并通过管道传递给 Stop-Process 进程结束它  
- `Start-Process` 用来启动一个新的进程
  - `WindowStyle Hidden` 隐藏窗口执行，即作为一个后台进程启动。默认情况下会以一个终端窗口启动执行脚本，执行完成之后也不会自动关闭窗口
  - `FilePath` 可以指定可执行文件的路径，如果此命令在环境变量中，可以省略前面的路径信息，直接指定命令名称即可
  - 如果命令本身还有一些其他的启动参数，可以跟在最后，如果是带空格的参数，可以用双引号括起来，否则脚本会将其识别为 Start-Process 的参数
