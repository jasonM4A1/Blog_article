---
title: HTTP协议入门
date: 2021-02-02 01:44:33
tags:
	- JavaWeb
	- HTTP
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202014528.jpg
---

# 概述

HTTP全写为Hyper Text Transfer Protocol（超文本传输协议）。

定义了客户端和服务器端通信时发送数据的格式。

特点：

1. 基于TCP/IP的高级协议
2. 默认端口号:80
3. 基于请求/响应模型的:一次请求对应一次响应
4. 无状态的，即每次请求之间相互独立，不能交互数据

# 请求消息数据格式

## 请求行

### 格式

```
求方式    请求url       请求协议/版本
GET     /login.html	   HTTP/1.1
```

### 请求方式

HTTP协议有7中请求方式，常用的有2种

#### GET

1. 请求参数在请求行中，在url后。
2. 请求的url长度有限制的
3. 不太安全

#### POST

1. 请求参数在请求体中
2. 请求的url长度没有限制的
3. 相对安全

## 请求头

客户端浏览器告诉服务器一些信息

常见的请求头：

1. User-Agent

   浏览器告诉服务器，我访问你使用的浏览器版本信息。* 可以在服务器端获取该头的信息，解决浏览器的兼容性问题

2. Referer：http://localhost/login.html

   告诉服务器，我(当前请求)从哪里来？

## 请求空行

空行，就是用于分割POST请求的请求头，和请求体的。

## 请求体

封装POST请求消息的请求参数的