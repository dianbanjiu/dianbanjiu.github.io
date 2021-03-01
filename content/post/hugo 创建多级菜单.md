---
title: "Hugo 创建多级菜单"
date: 2019-12-21T15:05:02+08:00
lastmod: 2019-12-21T15:05:02+08:00
tags: ["hugo"]
author: "dianbanjiu"
---

hugo 的多级多级菜单相较于单级菜单，仅仅是多了一个 `parent` 参数。  

下面是一个单级菜单的样式配置：  
*注：我使用的是 toml 格式的配置文件，其他类型的根据自己需求修改就可以了。*  

```toml
[[menu.main]]
  name = "主页"
  weight = 10
  url = "/"
[[menu.main]]
  name = "标签"
  weight = 20
  url = "/tags/"
```

上面这样可以创建一个最简单的单级菜单，显示效果就是我博客上现在显示的样子。  


下面是创建多级菜单的一个实例：  

```toml
[[menu.main]]
  name = "多级菜单"
  weight = 30
[[menu.main]]
  parent = "多级菜单"
  name = "菜单1"
  weight = 1
[[menu.main]]
  parent = "多级菜单"
  name = "菜单2"
  weight = 2
```

这样就可以创建一个两层菜单了，显示效果如下图所示。如果需要更多级，只需要在下面按照上面的规则继续添加即可。其中的 `weight` 参数是可选的，它可以调整各个菜单在菜单栏中的相对顺序，数值越小的菜单越靠前。  

![多级菜单](https://i.imgur.com/I0y54MM.png)  

需要注意的是，次级菜单的名称中必须有一些标识序号的选项，比如数字或者加减号。否则可能只会显示配置文件当中创建的最后一个次级菜单。  

![有数字标识1](https://i.imgur.com/I0y54MM.png)  

![有数字标识2](https://i.imgur.com/bIUCMSr.png)  

![有加减号](https://i.imgur.com/x6hWT8q.png)  

![无标识](https://i.imgur.com/05hBRKE.png)  

