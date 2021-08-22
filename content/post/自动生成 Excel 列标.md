---
title: "自动生成 Excel 列标"
date: 2021-08-22T14:55:33+08:00
lastmod: 2021-08-22T14:55:33+08:00
tags: ["golang","动态规划"]
author: "dianbanjiu"
comment: true
headPic: "https://imgur.com/6aEufje.png"
---

最近开发中涉及到将结果导出到 Excel 文件的功能，我看了一下现有的这些操作 Excel 的开源项目，无论是 Golang 还是 Python，基本上都不支持一次性插入一行数据，只能通过指定 cell 位置来一个个插入，而一个 cell 的位置基本包含这三项（sheet 名，列标，行号），sheet 名好说，毕竟是我自己生成的名字；行号也比较方便拿到；但是列标就有一丢丢蛋疼了，都是由字母拼成的，既然没有现成的实现，就只能自己实时计算了。 

![excel 界面示意图](https://imgur.com/6aEufje.png)  

观察了一下 Excel 的列标发现，Excel 的列标完全由 26 个英文字母按顺序全排列组合而成，大体规律如下
- 1 个字母：A,B,C...Z
- 2 个字母：AA,AB...BA,BB...CA,CB...ZY,ZZ
- 3 个字母：AAA,AAB...BAA,BAB...CAA,CAB...ZZY,ZZZ
- n 个字母：....


如果想要计算给定列数 n 对应的列标，可以通过让 n 不断与 26 进行取余操作来实现（**类似计算一个数值对应二进制数的方式，不过此处将 0 和 1 替换为了 26 个英文字母**）。比如：  
- **23**：23%26=0...23，f[n] = letters[23] = "W"
- **56**：56%26=2...4，f[n] = letters[2]+letters[4] = "BD"
- **1998**：1998%26=76...22，76%26=2...24，f[n] = result[76] + letters[22] = letters[2]+letters[24]+letters[22] = "BXV"
- ...

通过上面的归纳，可以看出，这其实是一个动态规划的问题，每一个 n 对应的列标值为 **n/26 + n%26**，前 26 个列标均为单个字母，它们不需要与其他字母进行组合。所以根据这个逻辑可以得到下面的公式：
- n<=26：result[n] = letters[n]
- n>26：result[n] = result[n/26]+result[n%26]

动态规划的问题一般来说只要能归纳出上的计算公式，那对应的代码逻辑其实就比较简单了，上代码：  

```go

func GetExcelColList(n int) []string {
    // Excel 的列标是由 26 个英文字母按照顺序组合而成的
    // 此处通过一个数组先保存基础的 26 个列标
	var baseList = []string{"A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K", "L", "M", "N", "O", "P", "Q", "R", "S", "T", "U", "V", "W", "X", "Y", "Z"}

    // 为了方便后面生成，此处将列数调整为 26 的整数倍
	count := n / 26
	if n%26 != 0 {
		count += 1
	}
    // 根据上面计算出的列数提前创建好一个数组用来存储列标
	var result = make([]string, count*26)

    // 无论列数的大小，默认填充前 26 个列标
    // 因为前 26 个列标均未单个字母，不是组合而成的元素，所以此处单独生成
	copy(result, baseList)

    // 从第 n = 26 开始，列标生成规则为：result[n] = result[n/26 -1] + result[n%26]
    // 因为数组的下标是从 0 开始的，所以上面需要 -1
	for i := 26; i < n; i++ {
		result[i] = result[i/26 - 1] + result[i%26]
	}

    // 根据列数返回所需的数据
	return result[:n]
}

```

