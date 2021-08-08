---
title: "关于 Alertmanager 中 group_interval 与 repeat_interval 上的一些坑"
date: 2021-07-14T23:56:01+08:00
lastmod: 2021-07-14T23:56:01+08:00
tags: ["prometheus","alertmanager"]
author: "dianbanjiu"
comment: true
headPic: "https://github.com/prometheus/alertmanager/raw/main/doc/arch.svg"
---

Alertmanager 中有三个关于告警时间的参数：  
1. group_wait：alertmanager 在接收到一条新的告警（第一次出现的告警）时，将这条告警发送给 receiver 之前需要等待的时间  
2. group_interval：对于一条已经出现过的告警，alertmanager 检查会每隔 group_interval 时间检查一次告警  
3. repeat_interval： 对于一条已经出现过的告警，每隔 repeat_interval 会重新发送给 receiver

这三个参数的基础含义比较简单，但是在最近的使用中，我发现当我按照如下格式设置这三个参数时：  
```yaml
group_wait: 15m
group_interval: 5m
repeat_interval: 15m
```

我将会每隔 20m 收到一次告警，如果我将 repeat_interval 设置为 20m，那我将会每 25m 收到一次告警...   
这种现象好像跟这三个参数在 alertmanager 文档中的说明不太一样啊。在搜寻无果之后，我在 alertmanager 的 github 仓库上提了一个 issue，官方给出了一个比较合理的解释。

[点此查看此 issue](https://github.com/prometheus/alertmanager/issues/2647)  


我根据 Issue 的回答简单总结了一下这三个参数的含义：  
> Alertmanager 在收到一条新的告警之后，会等待 group_wait 时间，对这条新的告警做一些分组、更新、静默的操作。当第一条告警经过 group_wait 时间之后，Alertmanager 会每隔 group_interval 时间检查一次这条告警，判断是否需要对这条告警进行一些操作，当 Alertmanager 经过 n 次 group_interval 的检查后，n*group_interval 恰好大于 repeat_interval 的时候，Alertmanager 才会将这条告警再次发送给对应的 receiver。  

结合我上面的例子，第一条告警在发送给 alertmanager 15m 后会第一次发送给 receiver，接着 Alertmanager 会每隔 5m 检查一次这条告警，在第 4 次检查的时候，到上次告警发出的时间刚好经过了 5*4=20>15，所以第二次告警会在第一条告警发出后的 20m 后再次发出，此后每条重复告警都会每隔 20m 发送一次。

这也就意味着在上面的例子中，当你的 15m<repeat_interval<20m 时，任意两条重复告警的间隔都是 20m。  

此外还有一个问题：在上面的例子中，如果 alertmanager 是每隔 5m 检查一次告警，那为什么不是恰好在 15m 的时候发出这条告警？关于这个问题，在 Issue 中尚未给出比较明确的答复。  

根据目前的情况，这个问题有两个暂时解决方案：  
1. 调整 repeat_interval 的值：同样结合上面的例子，如果你想每隔 20m 收到一次重复告警，那就把 repeat_interval 设置为 [15m,20m) 之间的任意一个时间
2. 调整 group_interval 的值：既然 alertmanager 会每隔 group_interval 检查一次已有的告警，那就尽可能将它的时间调的低一些，比如将其设置为 1m，这样你最多在你设置的 repeat_interval+1m 左右就可以收到重复告警