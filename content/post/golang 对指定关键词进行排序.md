---
title: "Golang 对指定关键词进行排序"
date: 2019-12-20T16:14:03+08:00
lastmod: 2019-12-20T16:14:03+08:00
tags: ["go", "sort"]
author: "dianbanjiu"
toc: false
---

之前在处理以结构体作为切片类型的问题里，如果可以对切片进行排序，则可以使问题简化许多。因为结构体有很多字段，既有字符字段又有数值字段，可以考虑通过实现 golang sort.Sort 的接口对结构体切片进行排序。  

假设结构体切片如下：  

```go
type Node struct {
	Data   string
	Weight int
	Length float64
}

type NodeSlice []Node

func main() {
	node := make(NodeSlice, 0)
	a := Node{
		Data:   "A",
		Weight: 12,
		Length: 90,
	}
	b := Node{
		Data:   "B",
		Weight: 17,
		Length: 26.2,
	}
	c := Node{
		Data:   "C",
		Weight: 5,
		Length: 1.2,
	}
	d := Node{
		Data:   "D",
		Weight: 8,
		Length: 22,
	}
	node = append(node, a, b, c, d)

}
```

现在以 Weight 作为关键词来对 node 进行排序。  

首先需要导入 sort 包，并且需要以 NodeSlice 类型作为接收者 (reciver) 实现 `Len()`，`Swap(i, j int)`，`Less(i,j int)` 三个方法。  

```go
func (ns NodeSlice) Len() int {
	return len(ns)
}
func (ns NodeSlice) Swap(i, j int) {
	ns[i], ns[j] = ns[j], ns[i]
}
func (ns NodeSlice) Less(i, j int) bool {
	return ns[i].Weight < ns[j].Weight
}
```

然后你就可以调用 sort.Sort 来对该类型的切片进行排序了，默认是从小到大：  

```go
fmt.Println(node)
sort.Sort(node)
fmt.Println(node)
```

打印结果如下：  

> [{A 12 90} {B 17 26.2} {C 5 1.2} {D 8 22}]  
> [{C 5 1.2} {D 8 22} {A 12 90} {B 17 26.2}]


如果需要从大到小进行打印，只需要将 `sort.Sort(node)` 替换为 `sort.Sort(sort.Reverse(node))`。  

如果需要以其他参数作为关键词进行排序，只需要调整实现的 `Less` 方法的关键词即可。  
