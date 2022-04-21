---
title: "VSCode 自动补全 Python 的函数括号"
date: 2022-04-21T11:19:41+08:00
lastmod: 2022-04-21T11:19:41+08:00
tags: ["vscode","python"]
author: "dianbanjiu"
comment: true
headPic: ""
---

目前 VSCode 对 Python 的支持已经相当好了，比如各种提示、补全、跳转、调试都是完全没有任何问题的

在 VSCode 中写 Python 的话，我一般会安装两个插件

- Python：提供对 Python 开发的支持
- Pylance：提供对 Python 一些智能补全方面的增强

不过在 VSCode 下写 Python 有一个比较蛋疼的点，就是补全函数的时候仅会补全函数名，而不会自动补全后面的括号

```python
# 输入
pr
# 期待补全为
print()
# 实际补全效果
print
```

不过好在 Pylance 插件其实是支持通过配置项来开启该功能的。在设置中搜索 `python.analysis.completefunctionparens` 并启用即可