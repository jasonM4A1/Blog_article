---
title: AJAX入门
date: 2021-03-06 14:57:37
tags:
	- Java
	- JavaWeb
	- AJAX
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/wallhaven-j3339m.jpg
---

# AJAX概述

AJAX即“Asynchronous JavaScript and XML”（异步的JavaScript与XML技术），指的是一套综合了多项技术的浏览器端网页开发技术。是一种用来创建交互式网页应用的网页开发技术。之所以说AJAX是一项综合技术，是因为AJAX包含了以下技术：

+ 基于HTML和CSS进行表示
+ 使用 DOM进行动态显示及交互
+ 使用 XML 和 JSON 进行数据交换及相关操作
+ 使用 XMLHttpRequest 进行异步数据查询、检索
+ 使用 JavaScript 将所有的东西绑定在一起

# 为什么要使用AJAX？

### 传统Web应用存在的问题

+ 传统的Web应用提交表单时会向网页服务器发送一个请求。服务器接收并处理传来的表单，然后送回一个新的网页。但这个做法浪费了许多带宽，因为在前后两个页面中的大部分HTML码往往是相同的。
+ 由于每次应用的沟通都需要向服务器发送请求，应用的回应时间依赖于服务器的回应时间。这导致了用户界面的回应比本机应用慢得多。即同步请求，浏览器需要等待服务器处理请求，导致了浏览器端的阻塞。

### AJAX的出现解决的问题

+ AJAX应用可以仅向服务器发送并取回必须的数据，并在客户端采用JavaScript处理来自服务器的回应。因为在服务器和浏览器之间交换的数据大量减少（大约只有原来的5%）,服务器回应更快了。局部刷新。
+ AJAX采用异步模式，通过XMLHttpRequest对象，发送给服务器请求以后，不用等待服务器的处理结果，用户可以继续进行操作。非阻塞的方式提升了用户体验。

总的来说就好像，浏览器大哥有一书包的好东西，但是他只想给服务器大哥其中一个好东西。传统Web应用处理方法是，浏览器大哥直接把书包给服务器大哥，他等服务器大哥自己找完以后，才能拿回自己的书包，继续干别的事情。AJAX的处理方法是，浏览器大哥找了个叫XMLHttpRequest的小弟来做这件事。小弟先从书包里找出来那个东西，然后再给服务器大哥。服务器拿到东西以后处理了一下再把东西还给XMLHttpRequest小弟。这期间浏览器大哥爱干嘛干嘛。

# AJAX的优缺点

## 优点

+ 能在不更新整个页面的前提下维护数据。这使得Web应用程序更为迅捷地回应用户动作，并避免了在网络上发送那些没有改变的信息。
+ 通过异步模式，不阻塞用户，从而提升了用户体验
+ AJAX不需要任何浏览器插件，但需要用户允许JavaScript在浏览器上执行
+ AJAX引擎在客户端运行，承担了一部分本来由服务器承担的工作，从而减少了大用户量下的服务器负载

## 缺点

+ 破坏浏览器的后退与加入收藏书签功能。在用AJAX动态更新页面的情况下，用户无法回到前一个页面状态，这是因为浏览器仅能记下历史记录中的静态页面
+ AJAX如果使用GET方法，会暴露了与服务器交互的细节
+ 对搜索引擎的支持比较弱，通过AJAX动态更新的页面可能无法被搜索引擎搜到

# 一个简单的AJAX请求示例

```javascript
// 在这里使用javaScript语言发起Ajax请求，访问服务器AjaxServlet中javaScriptAjax
function ajaxRequest() {
  //1、我们首先要创建XMLHttpRequest
  let xmlHttpRequest = new XMLHttpRequest();

  //2、调用open方法设置请求参数
  xmlHttpRequest.open("GET","http://localhost:8080/Ajax/AjaxServlet?action=ajaxJavaScript",true);

  //3、在send方法前绑定onreadystatechange事件，处理请求完成后的操作。


  //4、调用send方法发送请求
  xmlHttpRequest.send();
}
```

# 