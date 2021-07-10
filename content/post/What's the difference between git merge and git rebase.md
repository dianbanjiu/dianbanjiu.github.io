---
title: "What's the Difference Between Git Merge and Git Rebase"
date: 2021-07-10T14:07:01+08:00
lastmod: 2021-07-10T14:07:01+08:00
tags: ["git"]
author: "dianbanjiu"
comment: true
headPic: ""
---

git merge 与 git rebase 是我们平常协同开发过程中最常见，但是也是最容易让人迷惑的两个合并操作。下面是我对这两种合并方式的理解以及一些使用建议。



## git merge

git merge 相对来说比较好理解，他会将待 merge 的分支上所有不存在于当前分支的提交按照时间顺序插入到当前分支上，最后再生成一个 `Merge branch 'xxx'` 的 commit。



基础语法：

```shell
# git merge other-branch
$ git checkout master
$ git merge feature
```



下面是 merge 操作的基本图例。

![git merge](https://i.imgur.com/KerogMC.png)

merge 操作不会对每个提交节点本身做任何修改，也就是说在提交前后每个节点的提交信息、sha256 等信息都是一致的。



最后生成的合并节点 M 中一般会包含待提交分支与当前分支的所有差异代码，代码合并过程中的冲突修正也是在 M 节点中。一次 merge 操作最多只需要解决一次代码冲突。

## git rebase

rebase 操作会移动当前分支的基点（创建当前分支时基于的节点）为上游分支的最新提交，然后重写当前分支基点后的所有提交。



基础语法：

```shell
# git rebase upstream branch
$ git checkout feature
$ git rebase master feature
```



下面是 rebase 的基础图例：

![git rebase](https://i.imgur.com/PC8iDcZ.png)

rebase 操作会把 feature 的基点从 C 移到 D，然后重写 C、E，虽然表面上看 C、E 的提交信息没有任何变化，但是这两个点的 sha256 值其实已经不同了，相当于是两个新的提交。



rebase 过程中如果出现冲突，则需要针对冲突节点进行修改，修改完成后使用 `git rebase --continue` 继续 rebase，整个过程中不会产生类似 `Merge branch xxx` 的额外提交。



rebase 与 merge 相比，虽然可以减少一些无用的合并提交，但是因为 rebase 会基于上游分支重写当前分支，所以一般情况下不建议在主分支上进行 rebase 操作。

## 使用建议

因为现在公司的合并检查都是通过 CI 来做的，所以你提的 merge 请求至少不能与主版本发生任何冲突，这样才能通过检查。下面是我在日常工作流中两种合并操作的使用方式。  

1. 在非主版本上进行开发时，定期（一般是每次提交后）将主分支的更新 rebase 到当前分支，有冲突及早解决
2. 在主版本上只使用 merge 操作合并其他分支的代码
