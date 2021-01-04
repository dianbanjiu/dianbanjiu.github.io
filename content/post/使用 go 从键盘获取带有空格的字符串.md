---
title: "使用 Go 从键盘获取带有空格的字符串"
date: 2019-11-17T21:09:05+08:00
lastmod: 2019-11-17T21:09:05+08:00
tags: ["go", "bufio"]
author: "dianbanjiu"
toc: false
---

最近在使用 golang 从键盘获取输入的时候，发现无论使用 `fmt.Scan()`，`fmt.Scanf()` 还是 `fmt.Scanln()` 获取一个字符串的时候，只要字符串里含有空格，就只能获取到空格以前的数据。  

可以使用 bufio 包来解决这个问题：  

```go
input := bufio.NewReader(os.Stdin)
str, err := input.ReadString('\n')
```

这样的话可以从键盘获取一些字符串，直到遇见换行符。也可以将 `\n` 换为需要的终止符来定制自己的需求。  
