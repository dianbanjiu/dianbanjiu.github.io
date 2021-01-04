---
title: "为 Blogger 添加数字分页"
date: 2019-09-03T19:23:16+08:00
lastmod: 2019-09-03T19:23:16+08:00
tags: ["Blogger"]
author: "dianbanjiu"
toc: false
---

Blogger 的默认主题一般都不包含数字分页的功能，如果我们浏览到后面几页的时候突然想看前面的某一页，就只能一步一步的倒退回去。  

不过好在 Blogger 可以通过编辑页面的原始代码，来让主题的效果达到自己的理想状态。下面就来看一下吧。  

文章原文是英文，我选择了其中重要的部分翻译转述过来，**[点击这里查看原文](https://helplogger.blogspot.com/2014/04/how-to-add-numbered-page-navigation-widget-for-blogger.html)**

## 添加 CSS

登录你的 Blogger 页面，点击左侧 Tab 栏中的 **主题背景**，选择 **修改 HTML**。  

在源代码中搜索下面的代码：  
```css
]]></b:skin>
```

导航到相应的位置后，在该行代码的上面根据你的喜好插入下面任意一种分页导航的主题代码。  

#### 灰色
![Imgur](https://i.imgur.com/ImwrlYQ.png)

```css
#blog-pager{clear:both;margin:30px auto;text-align:center; padding: 7px;}
.blog-pager {background: none;}
.displaypageNum a,.showpage a,.pagecurrent{padding: 3px 7px;margin-right:5px;background:#E9E9E9;color: #888;border:1px solid #E9E9E9;}
.displaypageNum a:hover,.showpage a:hover,.pagecurrent{background:#CECECE;text-decoration:none;color: #000;}
 .showpageOf{display:none!important}
#blog-pager .showpage, #blog-pager .pagecurrent{font-weight:bold;color: #888;}
 #blog-pager .pages{border:none;}
```

#### 黑橙

![Imgur](https://i.imgur.com/EnJhawM.png)

```css
#blog-pager{clear:both;margin:30px auto;text-align:center; padding: 7px;}
.blog-pager {background: none;}
.displaypageNum a,.showpage a,.pagecurrent{padding: 5px 10px;margin-right:5px; color: #F4F4F4; background-color:#404042;-webkit-box-shadow: 0px 5px 3px -1px rgba(50, 50, 50, 0.53);-moz-box-shadow:0px 5px 3px -1px rgba(50, 50, 50, 0.53);box-shadow: 0px 5px 3px -1px rgba(50, 50, 50, 0.53);}
.displaypageNum a:hover,.showpage a:hover, .pagecurrent{background:#EC8D04;text-decoration:none;color: #fff;}
#blog-pager .showpage, #blog-pager, .pagecurrent{font-weight:bold;color: #000;}
 .showpageOf{display:none!important}
#blog-pager .pages{border:none;-webkit-box-shadow: 0px 5px 3px -1px rgba(50, 50, 50, 0.53);-moz-box-shadow:0px 5px 3px -1px rgba(50, 50, 50, 0.53);box-shadow: 0px 5px 3px -1px rgba(50, 50, 50, 0.53);}
```
#### 黑蓝

![Imgur](https://i.imgur.com/SDSXAAu.png)

```css
#blog-pager{clear:both;margin:30px auto; padding: 7px; text-align:center;font-size: 11px;background-image: -webkit-gradient(linear,left bottom,left top,color-stop(0, #000000),color-stop(1, #292929));background-image: -o-linear-gradient(top, #000000 0%, #292929 100%);background-image: -moz-linear-gradient(top, #000000 0%, #292929 100%);background-image: -webkit-linear-gradient(top, #000000 0%, #292929 100%);background-image: -ms-linear-gradient(top, #000000 0%, #292929 100%);background-image: linear-gradient(to top, #000000 0%, #292929 100%); padding: 6px;-webkit-border-radius: 3px;-moz-border-radius: 3px;border-radius: 3px;}
.blog-pager {background: none;}
.displaypageNum a,.showpage a,.pagecurrent{padding: 3px 10px;margin-right:5px; color: #fff;}
.displaypageNum a:hover,.showpage a:hover,.pagecurrent{background-image: -webkit-gradient(linear,left bottom,left top,color-stop(0, #59A2CF),color-stop(1, #D9EAFF));background-image: -o-linear-gradient(top, #59A2CF 0%, #D9EAFF 100%);background-image: -moz-linear-gradient(top, #59A2CF 0%, #D9EAFF 100%);background-image: -webkit-linear-gradient(top, #59A2CF 0%, #D9EAFF 100%);background-image: -ms-linear-gradient(top, #59A2CF 0%, #D9EAFF 100%);background-image: linear-gradient(to top, #59A2CF 0%, #D9EAFF 100%);text-decoration: none;color: #000;-webkit-border-radius: 3px;-moz-border-radius: 3px;border-radius: 3px;}
.showpageOf{display:none!important}.blog-pager-older-link, .home-link, .blog-pager-newer-link {background: transparent;}
a.blog-pager-older-link, a.home-link, a.blog-pager-newer-link {color: #fff;}
#blog-pager .pages{border:none;background: none;}
```

#### 灰蓝

![Imgur](https://i.imgur.com/QS3kQWQ.png)

```css
#blog-pager{clear:both;margin:30px auto;text-align:center; padding: 7px;}
.blog-pager {background: none;}
.displaypageNum a,.showpage a,.pagecurrent{font-size: 14px;padding: 5px 12px;margin-right:5px; color: #666; background-color:#eee;}
.displaypageNum a:hover,.showpage a:hover, .pagecurrent{background:#359BED;text-decoration:none;color: #fff;}
#blog-pager .pagecurrent{font-weight:bold;color: #fff;background:#359BED;}
 .showpageOf{display:none!important}
#blog-pager .pages{border:none;}
```

#### 绿橙粉

![Imgur](https://i.imgur.com/MzJG15W.png)

```css
#blog-pager{clear:both;margin:30px auto;text-align:center; padding: 7px; }
.blog-pager {background: none;}
.displaypageNum a,.showpage a,.pagecurrent{font-size: 13px;padding: 5px 12px;margin-right:5px; color: #3E5801; background-color:#E0EDC1;}
.displaypageNum a:hover,.showpage a:hover, .pagecurrent{background:#FEF6DF;text-decoration:none;color: #E16800;}
#blog-pager .pagecurrent{font-weight:bold;color: #D25E71;background:#FFDEDF;}
 .showpageOf{display:none!important}
#blog-pager .pages{border:none;}
```

#### 橙红

![Imgur](https://i.imgur.com/iTbCvXL.png)

```css
#blog-pager{clear:both;margin:30px auto;text-align:center; padding: 7px; }
.blog-pager {background: none;}
.displaypageNum a,.showpage a,.pagecurrent{font-size: 13px;padding: 5px 12px;margin-right:5px; color: #AD0B00; background-color:#FAB001;}
.displaypageNum a:hover,.showpage a:hover, .pagecurrent{background:#DB4920;text-decoration:none;color: #fff;}
#blog-pager .pagecurrent{font-weight:bold;color: #fff;background:#DB4920;}
 .showpageOf{display:none!important}
#blog-pager .pages{border:none;}
```

#### 灰红

![Imgur](https://i.imgur.com/3AdJk71.png)

```css
#blog-pager{clear:both;margin:30px auto;text-align:center; padding: 7px; }
.blog-pager {background: none;}
.displaypageNum a,.showpage a,.pagecurrent{font-size: 12px;padding: 5px 12px;margin-right:5px; color: #222; background-color:#eee; border: 1px solid #EEEEEE;}
.displaypageNum a:hover,.showpage a:hover, .pagecurrent{background:#E5E5E5;text-decoration:none;color: #222;}
#blog-pager .pagecurrent{font-weight:bold;color: #fff;background:#DB4920;}
 .showpageOf{display:none!important}
#blog-pager .pages{border:none;}
```

如果你想隐藏掉分页导航的 **First** 和 **Last** 的话，可以在上面的代码末添加下面这行代码：   
```css
.firstpage, .lastpage {display: none;}
```

## 添加 script

在代码中搜索 `</body>` 标签，在这个标签上面插入下面的代码：  
```html
<b:if cond='data:blog.pageType != &quot;item&quot;'>
<b:if cond='data:blog.pageType != &quot;static_page&quot;'>
<script type='text/javascript'>
  /*<![CDATA[*/
    var perPage=3;
    var numPages=3;
    var firstText ='First';
    var lastText ='Last';
    var prevText ='« Previous';
    var nextText ='Next »';
    var urlactivepage=location.href;
    var home_page="/";

if(typeof firstText=="undefined")firstText="First";if(typeof lastText=="undefined")lastText="Last";var noPage;var currentPage;var currentPageNo;var postLabel;pagecurrentg();function looppagecurrentg(pageInfo){var html='';pageNumber=parseInt(numPages / 2);if(pageNumber==numPages-pageNumber){numPages=pageNumber*2+1}
pageStart=currentPageNo-pageNumber;if(pageStart<1)pageStart=1;lastPageNo=parseInt(pageInfo / perPage)+1;if(lastPageNo-1==pageInfo / perPage)lastPageNo=lastPageNo-1;pageEnd=pageStart+numPages-1;if(pageEnd>lastPageNo)pageEnd=lastPageNo;html+="<span class='showpageOf'>Page "+currentPageNo+' of '+lastPageNo+"</span>";var prevNumber=parseInt(currentPageNo)-1;if(currentPageNo>1){if(currentPage=="page"){html+='<span class="showpage firstpage"><a href="'+home_page+'">'+firstText+'</a></span>'}else{html+='<span class="displaypageNum firstpage"><a href="/search/label/'+postLabel+'?&max-results='+perPage+'">'+firstText+'</a></span>'}}
if(currentPageNo>2){if(currentPageNo==3){if(currentPage=="page"){html+='<span class="showpage"><a href="'+home_page+'">'+prevText+'</a></span>'}else{html+='<span class="displaypageNum"><a href="/search/label/'+postLabel+'?&max-results='+perPage+'">'+prevText+'</a></span>'}}else{if(currentPage=="page"){html+='<span class="displaypageNum"><a href="#" onclick="redirectpage('+prevNumber+');return false">'+prevText+'</a></span>'}else{html+='<span class="displaypageNum"><a href="#" onclick="redirectlabel('+prevNumber+');return false">'+prevText+'</a></span>'}}}
if(pageStart>1){if(currentPage=="page"){html+='<span class="displaypageNum"><a href="'+home_page+'">1</a></span>'}else{html+='<span class="displaypageNum"><a href="/search/label/'+postLabel+'?&max-results='+perPage+'">1</a></span>'}}
if(pageStart>2){html+=' ... '}
for(var jj=pageStart;jj<=pageEnd;jj++){if(currentPageNo==jj){html+='<span class="pagecurrent">'+jj+'</span>'}else if(jj==1){if(currentPage=="page"){html+='<span class="displaypageNum"><a href="'+home_page+'">1</a></span>'}else{html+='<span class="displaypageNum"><a href="/search/label/'+postLabel+'?&max-results='+perPage+'">1</a></span>'}}else{if(currentPage=="page"){html+='<span class="displaypageNum"><a href="#" onclick="redirectpage('+jj+');return false">'+jj+'</a></span>'}else{html+='<span class="displaypageNum"><a href="#" onclick="redirectlabel('+jj+');return false">'+jj+'</a></span>'}}}
if(pageEnd<lastPageNo-1){html+='...'}
if(pageEnd<lastPageNo){if(currentPage=="page"){html+='<span class="displaypageNum"><a href="#" onclick="redirectpage('+lastPageNo+');return false">'+lastPageNo+'</a></span>'}else{html+='<span class="displaypageNum"><a href="#" onclick="redirectlabel('+lastPageNo+');return false">'+lastPageNo+'</a></span>'}}
var nextnumber=parseInt(currentPageNo)+1;if(currentPageNo<(lastPageNo-1)){if(currentPage=="page"){html+='<span class="displaypageNum"><a href="#" onclick="redirectpage('+nextnumber+');return false">'+nextText+'</a></span>'}else{html+='<span class="displaypageNum"><a href="#" onclick="redirectlabel('+nextnumber+');return false">'+nextText+'</a></span>'}}
if(currentPageNo<lastPageNo){if(currentPage=="page"){html+='<span class="displaypageNum lastpage"><a href="#" onclick="redirectpage('+lastPageNo+');return false">'+lastText+'</a></span>'}else{html+='<span class="displaypageNum lastpage"><a href="#" onclick="redirectlabel('+lastPageNo+');return false">'+lastText+'</a></span>'}}
var pageArea=document.getElementsByName("pageArea");var blogPager=document.getElementById("blog-pager");for(var p=0;p<pageArea.length;p++){pageArea[p].innerHTML=html}
if(pageArea&&pageArea.length>0){html=''}
if(blogPager){blogPager.innerHTML=html}}
function totalcountdata(root){var feed=root.feed;var totaldata=parseInt(feed.openSearch$totalResults.$t,10);looppagecurrentg(totaldata)}
function pagecurrentg(){var thisUrl=urlactivepage;if(thisUrl.indexOf("/search/label/")!=-1){if(thisUrl.indexOf("?updated-max")!=-1){postLabel=thisUrl.substring(thisUrl.indexOf("/search/label/")+14,thisUrl.indexOf("?updated-max"))}else{postLabel=thisUrl.substring(thisUrl.indexOf("/search/label/")+14,thisUrl.indexOf("?&max"))}}
if(thisUrl.indexOf("?q=")==-1&&thisUrl.indexOf(".html")==-1){if(thisUrl.indexOf("/search/label/")==-1){currentPage="page";if(urlactivepage.indexOf("#PageNo=")!=-1){currentPageNo=urlactivepage.substring(urlactivepage.indexOf("#PageNo=")+8,urlactivepage.length)}else{currentPageNo=1}
document.write("<script src=\""+home_page+"feeds/posts/summary?max-results=1&alt=json-in-script&callback=totalcountdata\"><\/script>")}else{currentPage="label";if(thisUrl.indexOf("&max-results=")==-1){perPage=20}
if(urlactivepage.indexOf("#PageNo=")!=-1){currentPageNo=urlactivepage.substring(urlactivepage.indexOf("#PageNo=")+8,urlactivepage.length)}else{currentPageNo=1}
document.write('<script src="'+home_page+'feeds/posts/summary/-/'+postLabel+'?alt=json-in-script&callback=totalcountdata&max-results=1" ><\/script>')}}}
function redirectpage(numberpage){jsonstart=(numberpage-1)*perPage;noPage=numberpage;var nameBody=document.getElementsByTagName('head')[0];var newInclude=document.createElement('script');newInclude.type='text/javascript';newInclude.setAttribute("src",home_page+"feeds/posts/summary?start-index="+jsonstart+"&max-results=1&alt=json-in-script&callback=finddatepost");nameBody.appendChild(newInclude)}
function redirectlabel(numberpage){jsonstart=(numberpage-1)*perPage;noPage=numberpage;var nameBody=document.getElementsByTagName('head')[0];var newInclude=document.createElement('script');newInclude.type='text/javascript';newInclude.setAttribute("src",home_page+"feeds/posts/summary/-/"+postLabel+"?start-index="+jsonstart+"&max-results=1&alt=json-in-script&callback=finddatepost");nameBody.appendChild(newInclude)}
function finddatepost(root){post=root.feed.entry[0];var timestamp1=post.published.$t.substring(0,19)+post.published.$t.substring(23,29);var timestamp=encodeURIComponent(timestamp1);if(currentPage=="page"){var pAddress="/search?updated-max="+timestamp+"&max-results="+perPage+"#PageNo="+noPage}else{var pAddress="/search/label/"+postLabel+"?updated-max="+timestamp+"&max-results="+perPage+"#PageNo="+noPage}
location.href=pAddress}

  /*]]>*/
</script>
</b:if>
</b:if>
```

可以通过调整上面代码中的下面这些参数来调整底部数字导航栏的显示形式：  
```html
perPage: 7,
numPages: 6,
var firstText ='First';
var lastText ='Last';
var prevText ='« Previous';
var nextText ='Next »';
}
```
- `perPage` 每页显示的文章数量，建议与在 Blogger 设置中的每页显示文章数目一致。
- `numPages` 显示的数字导航数。
- 可以把 `First`、`Last`、`« Previous`、`Next »` 更换为自己想要的符号页面导航符号。
