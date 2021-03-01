---
title: "Golang 切片"
date: 2020-03-15T11:59:22+08:00
lastmod: 2020-03-15T11:59:22+08:00
tags: ["golang"]
author: "dianbanjiu"
---

最近在使用 golang 创建子集的时候，遇到了一些问题，下面是代码：  

```golang
func Subsets(nums []int) [][]int {
    var sets = make([][]int, 0)
    var t = make([]int, 0)
    sets = append(sets, t)
    for i := 0; i < len(nums); i++ {
        for _, v := range sets {
            t = append(v, nums[i])
            sets = append(sets, t)      }
    }
    return sets
}
```

当测试切片的长度不大于 4，比如 []int{1,2,3,4} 的时候，程序输出正常，结果也是正确的。  
不过当切片长度大于 4 的时候，比如 []int{1,2,3,4,5}，程序输出的二维切片的长度虽然正确，但是期中有一些数据就会有问题。在 debug 的时候发现，当计算完 sets[21]，开始计算 sets[22] 的时候，sets[15] 会从 []int{1,2,3,4}，变成 []int{1,2,3,5}。  

在查询之后发现，因为 sets 的元素每次引用的都是同样的切片，所以可能会导致这个问题。最好的解决办法就是为 sets 的每个元素创建新的切片，下面是修改后的代码。  

```golang
func Subsets(nums []int) [][]int {
    var sets = make([][]int, 0)
    var t = make([]int, 0)
    sets = append(sets, t)
    for i := 0; i < len(nums); i++ {
        for _, v := range sets {
            t = append([]int{}, v...)   // 将 v 中的元素逐个复制到 t 中
            t = append(t, nums[i])  // 将新的元素再添加到 t 中
            sets = append(sets, t)      }
    }
    return sets
}
```