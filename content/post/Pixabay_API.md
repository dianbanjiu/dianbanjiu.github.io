---
title: "[译] Pixabay API"
date: 2021-02-19T14:40:22+08:00
lastmod: 2021-02-19T14:40:22+08:00
tags: ["Pixabay", "翻译"]
author: "dianbanjiu"
---
[原文链接](https://pixabay.com/api/docs/)


欢迎来到 Pixabay API 文档，我们提供 RESTful 风格的接口，你可以通过这些接口搜索遵循 [Pixabay 协议](https://pixabay.com/service/license/) 的免费图像和视频。  

当你使用了这些接口，需要在所有搜索结果中向用户展示图片和视频来自何处，其中必须包含 Pixabay 的链接，同时你也可以使用我们的 [logo](https://pixabay.com/service/about/#goodies) 来达到同样的效果。  

调用这些 API 将会返回 JSON 格式的对象，哈希键值都是区分大小写并且使用 UTF-8 编码。哈希键是无序的，每次调用返回的顺序都可能会不同。我们每次从结果中移除或者添加任何新的哈希键的时候都会尽可能的提醒用户。  

## 访问频率限制

默认情况下，你每小时可以发起 5000 次请求。请求与 API 密钥相关联，与 IP 地址无关。在响应头部会说明所有与当前访问频率限制有关的信息。  
名称 | 描述  
---|---|
X-RateLimit-Limit|在 30min 内可以发出的最大请求数|
X-RateLimit-Remaining|在当前访问频率窗口中剩余的请求数|
X-RateLimit-Reset|当前访问频率窗口的重置剩余时间（以秒为单位）|

为了保证 Pixabay API 的访问速度，所有的请求都会被缓存 24小时。这些接口本身是为用户的人为请求所设计的，禁止大量自动化请求。系统不允许一次性进行大量下载，如果你有这方面的需要，我们可以随时增加这个限制 —— 假设你已经实现了这个接口。  

## 热链接
返回图片的 URL 一般用于暂时的图片展示。禁止对图像的永久热链接（应该在你的应用中使用 Pixabay URL）。如果你想要使用这些图片，请先下载它们到你自己的服务器上，视频则可以直接嵌入到你的应用中，当然，我们建议视频也最好存储在你自己的服务器上。

## 错误处理
如果请求发生了错误，响应中将会带有对应的错误状态码，响应体中会带有关于错误的描述信息。例如：一旦请求次数超过了请求频率的限制，你将会得到一个 429（请求次数过多）的错误代码以及 "API rate limit exceeded" 的返回信息。  

## 图像搜索
```http
https://pixabay.com/api/ GET
```

### 参数
参数|类型|描述
---|---|---|
key(必需)|str|请[登录](https://pixabay.com/accounts/login/?next=/api/docs/)\|[注册](https://pixabay.com/accounts/login/?next=/api/docs/)后查看你的 API 密钥|
q|str|已经编码的请求项，如果忽略该字段将会返回所有图像，该字段最长为 100 个字符。例如："yellow+flower"|
lang|str|要搜索语言的语言代码，可接收的值包括：cs、da、de、en、es、fr、id、it、hu、nl、no、pl、pt、ro、sk、fi、sv、tr、vi、th、bg、ru、el、ja、ko、zh，默认值：en|
id|str|通过图片 id 检索唯一图片|
image_type|str|通过图片类型过滤搜索结果。可接收的值：all、photo、illustration、vector。默认值：all|
orientation|str|图像高大于宽还是宽大于高，可接收的值：all、horizontal、vertical，默认值：all|
category|str|通过类别过滤结果。可接收的值：backgrounds、 fashion、 nature、 science、 education、 feelings、 health、 people、 religion、 places、 animals、 industry、 computer、 food、 sports、 transportation、 travel、 buildings、 business、 music|
min_width|int|图像的最小宽度。默认值：0|
min_height|int|图像的最小高度。默认值：0|
colors|str|通过颜色属性过滤图片。可以通过半角逗号分割的颜色列表来选择多种颜色属性。可接收的值：grayscale、 transparent、 red、 orange、 yellow、 green、 turquoise、 blue、 lilac、 pink、 white、 gray、 black、 brown|
editors_choice|bool|从[编辑推荐](https://pixabay.com/editors_choice/)筛选图片。可接收的值：true、false。默认值：false|
safesearch|bool|返回适合所有年龄段的图片。可接收的值：true、false。默认值：false|
order|str|结果的排序标准。可接收的值：popular、latest。默认值：popular|
page|int|返回结果是分页的，使用此参数选择页号。默认值：1|
per_page|int|设定每页的结果数。可接收的值：3-200。默认值：20|
callback|str|JSONP 回调函数名|
pretty|bool|缩进 JSON 输出，此选项不应该使用在你的产品中。可接收的值：true、false。默认值：false|

### 实例
检索 "yellow flowers" 的图片。搜索项 `q` 需要进行 URL 编码，{ KEY } 需要替换为你的 API 密钥。  
`https://pixabay.com/api/?key={ KEY }&q=yellow+flowers&image_type=photo`  

该请求的响应结果：  
```json
{
"total": 4692,
"totalHits": 500,
"hits": [
    {
        "id": 195893,
        "pageURL": "https://pixabay.com/en/blossom-bloom-flower-195893/",
        "type": "photo",
        "tags": "blossom, bloom, flower",
        "previewURL": "https://cdn.pixabay.com/photo/2013/10/15/09/12/flower-195893_150.jpg"
        "previewWidth": 150,
        "previewHeight": 84,
        "webformatURL": "https://pixabay.com/get/35bbf209e13e39d2_640.jpg",
        "webformatWidth": 640,
        "webformatHeight": 360,
        "largeImageURL": "https://pixabay.com/get/ed6a99fd0a76647_1280.jpg",
        "fullHDURL": "https://pixabay.com/get/ed6a9369fd0a76647_1920.jpg",
        "imageURL": "https://pixabay.com/get/ed6a9364a9fd0a76647.jpg",
        "imageWidth": 4000,
        "imageHeight": 2250,
        "imageSize": 4731420,
        "views": 7671,
        "downloads": 6439,
        "favorites": 1,
        "likes": 5,
        "comments": 2,
        "user_id": 48777,
        "user": "Josch13",
        "userImageURL": "https://cdn.pixabay.com/user/2013/11/05/02-10-23-764_250x250.jpg",
    },
    {
        "id": 73424,
        ...
    },
    ...
]
}
```

响应项| 描述
---|---|
total|此次请求命中的图片数量|
totalHits|可以通过 API 获取的图片数量。默认情况下，每次请求 API 返回的图片数量的最大值为 500|
id|图片的唯一标识符|
pageURL|Pixabay 的源页面，提供 imageWidth x imageHeight 和文件大小为 imageSize 的原始文件下载|
previewURL|低分辨率图片，最大宽度或最大高度为 150 px|
webformatURL|中等尺寸的图片，宽或高为 640 px。此链接 24 小时内有效。**使用其他尺寸的 webformatURL 替换 '_640' 可以获取其他尺寸的图片：** 使用 '_180' 或 '_340' 可以获取 180 或者 340px 分辨率的图片，使用 '_960' 则可以获取 960x720px 的高分辨率图片|
largeImageURL|大尺寸图片，宽或高为 1280px|
views|图片被查看的次数|
downloads|图片被下载的次数|
faverites|图片被收藏的次数|
likes|图片被喜欢的数量|
comments|图片的总评论数|
user_id, user|贡献者的 ID 和用户名。个人资料链接：`https://pixabay.com/users/{ USERNAME }-{ ID }/`|
userImageURL|个人资料头像链接（250x250 px）|

**下面的这些响应项只有当你的账户[获得完全 API 访问权限](https://pixabay.com/api/docs/)之后才会出现**。这些地址可以让你以全分辨率和矢量格式（如果可用）访问原始图片。  

响应项|描述
---|---|
fullHDURL|全高清图片，最大宽/高为 1920 px|
imageURL|原始图像地址|
vectorURL|矢量资源地址，如果存在则显示，否则忽略|

## 视频搜索

```http
https://pixabay.com/api/videos/ GET
```

### 请求参数
参数|类型|说明
---|---|---
key(必须)|str|请[登录](https://pixabay.com/accounts/login/?next=/api/docs/)\|[注册](https://pixabay.com/accounts/login/?next=/api/docs/)后查看你的 API 密钥|
q|str|已经编码的请求项，如果忽略该字段将会返回所有视频，该字段最长为 100 个字符。例如："yellow+flower"|
lang|str|要搜索语言的语言代码，可接收的值包括：cs、da、de、en、es、fr、id、it、hu、nl、no、pl、pt、ro、sk、fi、sv、tr、vi、th、bg、ru、el、ja、ko、zh，默认值：en|
id|str|通过 id 检索单独的视频|
video_type|str|通过视频类型过滤结果。可接收的值：all、film、animation。默认值：all|
category|str|通过类别过滤结果。可接收的值：backgrounds、 fashion、nature、science、education、feelings、health、people、religion、places、animals、industry、computer、food、sports、transportation、travel、buildings、business、music|
min_width|int|视频的最小宽度。默认值：0|
min_height|int|视频的最小高度。默认值：0|
editors_choice|bool|从[编辑推荐](https://pixabay.com/editors_choice/)选择视频。可接收的值：true、false。默认值：false|
safesearch|bool|返回适合所有年龄段的视频。可接收的值：true、false。默认值：false|
order|str|结果的排序标准。可接收的值：popular、latest。默认值：popular|
page|int|返回结果是分页的，使用此参数选择页号。默认值：1|
per_page|int|设定每页的结果数。可接收的值：3-200。默认值：20|
callback|str|JSONP 回调函数名|
pretty|bool|缩进 JSON 输出，此选项不应该使用在你的产品中。可接收的值：true、false。默认值：false|

### 实例
检索 "yellow flowers" 的视频。搜索项 `q` 需要进行 URL 编码，{ KEY } 需要替换为你的 API 密钥。  
`https://pixabay.com/api/videos/?key={ KEY }&q=yellow+flowers`  

该请求的响应结果：  
```json
{
"total": 42,
"totalHits": 42,
"hits": [
    {
        "id": 125,
        "pageURL": "https://pixabay.com/videos/id-125/",
        "type": "film",
        "tags": "flowers, yellow, blossom",
        "duration": 12,
        "picture_id": "529927645",
        "videos": {
            "large": {
                "url": "https://player.vimeo.com/external/135736646.hd.mp4?s=ed02d71c92dd0df7d1110045e6eb65a6&profile_id=119",
                "width": 1920,
                "height": 1080,
                "size": 6615235
            },
            "medium": {
                "url": "https://player.vimeo.com/external/135736646.hd.mp4?s=ed02d71c92dd0df7d1110045e6eb65a6&profile_id=174",
                "width": 1280,
                "height": 720,
                "size": 3562083
            },
            "small": {
                "url": "https://player.vimeo.com/external/135736646.sd.mp4?s=db2924c48ef91f17fc05da74603d5f89&profile_id=165",
                "width": 950,
                "height": 540,
                "size": 2030736
            },
            "tiny": {
                "url": "https://player.vimeo.com/external/135736646.sd.mp4?s=db2924c48ef91f17fc05da74603d5f89&profile_id=164",
                "width": 640,
                "height": 360,
                "size": 1030736
            }
        },
        "views": 169,
        "downloads": 66,
        "favorites": 7,
        "likes": 3,
        "comments": 2,
        "user_id": 1281706,
        "user": "CoverrFreeFootage",
        "userImageURL": "https://cdn.pixabay.com/user/2015/10/16/09-28-45-303_250x250.png"
    },
    {
        "id": 473,
        ...
    },
    ...
]
}
```

响应项| 描述
---|---|
total|此次请求命中的视频数量|
totalHits|可以通过 API 获取的视频数量。默认情况下，API 会限制每次请求返回的视频数量的最大值为 500|
id|视频的唯一标识符|
pageURL|Pixabay 的源页面|
picture_id|这个值可以用于检索视频多种尺寸的静态预览图：`https://i.vimeocdn.com/video/{ PICTURE_ID }_{ SIZE }.jpg`。可接收的尺寸：100x75, 200x150, 295x166, 640x360, 960x540, 1920x1080。例如： `https://i.vimeocdn.com/video/529927645_295x166.jpg`|
videos|一系列不同尺寸的视频流："large" 的分辨率通常为 1920x1080px，如果没有 large 尺寸的视频，large 对象的 URL 将会标为空，size 设置为 0。"medium" 的分辨率通常为 1280x720px，这个分辨率适用于所有视频。"small" 的分辨率通常为 960x540px，老视频还有 640x360px 的分辨率，这个分辨率适用于所有视频。"tiny" 的分辨率通常为 640x360px，老视频还有 480x270px 的分辨率，此分辨率适用于所有视频。在任何视频流的 URL 的 GET 请求后添加 download=1 都可将视频下载至自己的服务器|
views|视频被查看的次数|
downloads|视频被下载的次数|
faverites|视频被收藏的次数|
likes|视频被喜欢的数量|
comments|视频的总评论数|
user_id, user|贡献者的 ID 和用户名。个人资料链接：`https://pixabay.com/users/{ USERNAME }-{ ID }/`|
userImageURL|个人资料头像链接（250x250 px）|

## JavaScript 实例

```js
var API_KEY = 'YOUR_API_KEY';
var URL = "https://pixabay.com/api/?key="+API_KEY+"&q="+encodeURIComponent('red roses');
$.getJSON(URL, function(data){
if (parseInt(data.totalHits) > 0)
    $.each(data.hits, function(i, hit){ console.log(hit.pageURL); });
else
    console.log('No hits');
});
```

## 支持
获取 [API 完全请求权限](https://pixabay.com/api/docs/#) 以检索高质量图片。  
如果你有关于 API 的任何问题请[联系我们](https://pixabay.com/service/contact/)
