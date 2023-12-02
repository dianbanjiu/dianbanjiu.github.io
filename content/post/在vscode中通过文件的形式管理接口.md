---
title: "在vscode中通过文件的形式管理接口"
date: 2023-12-02T19:06:33+08:00
lastmod: 2023-12-02T19:06:33+08:00
tags: ["vscode"]
author: "dianbanjiu"
comment: true
headPic: ""
---

我之前一直使用 postman 来管理接口，postman 功能强大，容易上手，但是随着版本的更新，postman 逐渐发展为了一个在线服务，虽然能够让不同机器间切换时也能有一个统一的体验，但是因为网络原因，特别是在公司的网络策略面前，纯在线服务经常会出现接口无法从服务器上拉取下来的情况。所以我转而开始寻找一些对本地服务有更好支持的接口管理工具，在尝试过多个工具之后，最终选择了 httpyac，目前还在初期的磨合阶段，不过 httpyac 已经基本能满足我一个开发人员对于接口管理&测试方面的所有基础需求了  

httpyac 支持 vscode 插件(扩展商店搜索 `anweber.vscode-httpyac` 安装即可)，通过 .http/.rest 的形式来管理接口文件，这也就意味着你可以把接口文件随项目一起进行版本管理，或者将所有的接口汇总在一个仓库统一管理，也可以选择自己喜欢&可用的云服务进行托管，这样就不会出现 postman 那种拉不到接口的情况出现了  

下面是 httpyac 的一些简单的使用示例，也是我在日常使用中最主要的几个点  

## 发起请求  
```http
### 获取用户信息
GET https://www.dianbanjiu.com/user/info?name=John HTTP/1.1

### 新增用户
POST https://www.dianbanjiu.com/user HTTP/1.1
Content-Type: application/json

{
    "name": "John",
    "age": 30,
    "phone": "12345678901"
}
```
你可以通过在每行的开头通过 `#` 或者 `//` 来添加单行注释，也支持通过 `/* */` 来添加多行注释  

## 定义变量  
httpyac 支持多种定义变量的形式  
1. 在接口文件内定义变量
```http
@host=https://www.dianbanjiu.com
@user=John
@pass=123456
```
2. 在项目根目录下通过 .env.xx 定义环境文件，xx 为环境名
```
// .env.dev
host=https://www.dianbanjiu.com
user=John
pass=123456
```
3. 在 .vscode/settings.json 中定义环境变量，其中 `$share` 是所有环境的共享变量
```json
{
    "httpyac.environmentVariables": {
        "$shared": {
            "foo": "bar"
        },
        "dev": {
            "host": "https://www.dianbanjiu.com",
            "user": "user",
            "pass": "pass"
        },
        "prd": {
            "host": "https://www.dianbanjiu.com",
            "user": "user",
            "pass": "pass"
        }
    }
}
```

定义好变量之后，在接口文件中通过双大括号即可引用  
```http
GET {{host}}/user/info?name={{name}}
```

我一般是第1、3结合使用。这里有一个语法糖，如果你定义了 host 变量，在写接口的时候可以忽略前置的域名，直接写路由，如果你在环境变量中域名使用的是其他名字，也可以在接口文件中重新赋值 host 变量来使用这个语法糖  
```http
# 环境文件中定义了 dian-api=https://www.dianbanjiu.com
@host={{dian-api}}

GET /user/info?name={{name}}
```

## 脚本命令
httpyac 支持在接口文件中自定义脚本，可以实现类似 postman 的脚本功能，这也是我之所以选择 httpyac 的最主要原因  

比如很多接口都需要进行签名，你可以在脚本中使用 nodejs 的 crypto 包来完成
```http
{{
  //pre request script
  const crypto = require('crypto');
  const date = new Date();
  const signatureBase64 = crypto.createHmac('sha256', 'secret')
  .update(`${request.method}\u2028${request.url}\u2028${date.getTime()}`).digest("base64");
  exports.authDate = date.toUTCString();
  exports.authentcation = `Basic ${signatureBase64}`;
}}

GET https://www.dianbanjiu.com/user/info?name={{name}} HTTP/1.1
Date: {{authDate}}
Authentication: {{authentcation}}
```

如果你的接口需要统一设置请求头也可以通过脚本实现，`{{+` 开头的脚本会在所有请求之前执行，请求的具体执行流程可以参照下图  
```http
{{+
  //pre request script
  const crypto = require('crypto');
  const date = new Date();
  const signatureBase64 = crypto.createHmac('sha256', 'secret')
  .update(`${request.method}\u2028${request.url}\u2028${date.getTime()}`).digest("base64");
  request.headers.authDate = date.toUTCString();
  request.headers.authentcation = `Basic ${signatureBase64}`;
}}

GET https://www.dianbanjiu.com/user/info?name={{name}} HTTP/1.1
```
![httpyac接口执行流程](https://httpyac.github.io/assets/scripting.8a515e08.svg)  

一些需要把整个请求体都作为加密字段的接口，也可以通过脚本实现  
```http
{{
  const crypto = require('crypto');
  const date = new Date();
  const signatureBase64 = crypto.createHmac('sha256', 'secret')
  .update(request.body).digest("base64");
  exports.authDate = date.toUTCString();
  exports.authentcation = `Basic ${signatureBase64}`;
}}

GET https://www.dianbanjiu.com/user/info?name={{name}}&sign={{authentcation}}&time={{authDate}} HTTP/1.1
```

目前还有一个不大不小的痛点，就是 httpyac 尚不支持导入 curl 或者 postman 导出的文件来转换为 .http/.rest 文件，需要自己一个个接口再从 postman 换过来也是一个不小的工作量  

至此我在 postman 中会用的 99% 的功能 httpyac 都提供了，此外 httpyac 也提供了断言、hook、debug 等一些其他的高级功能，但是我都未使用过，这里就不多说明了，具体的说明可以参照 [httpyac官网](https://httpyac.github.io/) 了解。还有一些语法糖官网其实也没有说明的很清楚，可能需要去 [httpyac代码仓库的issue](https://github.com/AnWeber/vscode-httpyac) 自行搜索了