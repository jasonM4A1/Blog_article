---
title: Http协议入门
date: 2021-02-10 09:20:50
tags:
	- JavaWeb
	- Http
	- 计算机网络
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203172229.png
---

# HTTP概述

+ HTTP协议是Hyper Text Transfer Protocol（超文本传输协议）的缩写,是用于从万维网（WWW:World Wide Web ）服务器传输超文本到本地浏览器的传送协议。
+ HTTP是一个基于TCP/IP通信协议来传递数据（HTML 文件, 图片文件, 查询结果等）。
+ HTTP是一个属于应用层的面向对象的协议，由于其简捷、快速的方式，适用于分布式超媒体信息系统。
+ HTTP协议工作于客户端-服务端架构为上。浏览器作为HTTP客户端通过URL向HTTP服务端即WEB服务器发送所有请求。Web服务器根据接收到的请求后，向客户端发送响应信息。

# HTTP特点

+ **简单快速**：客户向服务器请求服务时，只需传送请求方法和路径。请求方法常用的有GET、HEAD、POST等。每种方法规定了客户与服务器联系的类型不同。由于HTTP协议简单，使得HTTP服务器的程序规模小，因而通信速度很快。
+ **灵活**：HTTP允许传输任意类型的数据对象。正在传输的类型由Content-Type加以标记。
+ **无连接**：无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。
+ **无状态**：HTTP协议是无状态协议。无状态是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。

# HTTP版本

目前HTTP版本分为两种——HTTP1.0和HTTP1.1，常用的就是HTTP1.1

## HTTP1.0

客户端可以与web服务器连接后，只能获得一个web资源，断开连接

## HTTP1.1

客户端可以与web服务器连接后，可以获得多个web资源，不会断开连接

# HTTP中的URL

HTTP使用统一资源标识符（Uniform Resource Identifiers, URI）来传输数据和建立连接。URL是一种特殊类型的URI，包含了用于查找某个资源的足够的信息。

URL,全称是UniformResourceLocator, 中文叫统一资源定位符,是互联网上用来标识某一处资源的地址。以下面这个URL为例，介绍下普通URL的各部分组成：

```
http://www.rtx3090.xyz:8080/news/index.asp?boardID=5&ID=24618&page=1#name
```

从上面的URL可以看出，一个完整的URL包括以下几部分：

1. **协议部分**：该URL的协议部分为`http:`，这代表网页使用的是HTTP协议。在Internet中可以使用多种协议，如HTTP，FTP等等本例中使用的是HTTP协议。在"HTTP"后面的“//”为分隔符
2. **域名部分**：该URL的域名部分为`www.rtx3090.xyz`。一个URL中，也可以使用IP地址作为域名使用
3. **端口部分**：跟在域名后面的是端口，域名和端口之间使用`:`作为分隔符。端口不是一个URL必须的部分，如果省略端口部分，将采用默认端口（HTTP协议的默认端口为80）。
4. **虚拟目录部分**：从域名后的第一个`/`开始到最后一个`/`为止，是虚拟目录部分。虚拟目录也不是一个URL必须的部分。本例中的虚拟目录是`/news`
5. **文件名部分**：从域名后的最后一个`/`开始到`?`为止，是文件名部分，如果没有`?`,则是从域名后的最后一个`/`开始到`#`为止，是文件部分，如果没有`?`和`#`，那么从域名后的最后一个`/`开始到结束，都是文件名部分。本例中的文件名是`index.asp`。文件名部分也不是一个URL必须的部分，如果省略该部分，则使用默认的文件名
6. **锚部分**：从`#`开始到最后，都是锚部分。本例中的锚部分是`name`。锚部分也不是一个URL必须的部分
7. **参数部分**：从`?`开始到`#`为止之间的部分为参数部分，又称搜索部分、查询部分。本例中的参数部分为`boardID=5&ID=24618&page=1`。参数可以允许有多个参数，参数与参数之间用`&`作为分隔符。

> 此部分引用自：[蒙声伐大柴的CSDN](https://blog.csdn.net/ergouge/article/details/8185219)

## URL与URI的区别

### URI

URI，是uniform resource identifier，统一资源标识符，用来唯一的标识一个资源。

Web上可用的每种资源如HTML文档、图像、视频片段、程序等都是一个来URI来定位的。

URI一般由三部组成：

1. 访问资源的命名机制
2. 存放资源的主机名
3. 资源自身的名称，由路径表示，着重强调于资源。

### URL

URL是uniform resource locator，统一资源定位器，它是一种具体的URI，即URL可以用来标识一个资源，而且还指明了如何找到这个资源。

URL是Internet上用来描述信息资源的字符串，主要用在各种WWW客户程序和服务器程序上，特别是著名的Mosaic。

采用URL可以用一种统一的格式来描述各种信息资源，包括文件、服务器的地址和目录等。URL一般由三部组成：

1. 协议(或称为服务方式)
2. 存有该资源的主机IP地址(有时也包括端口号)
3. 主机资源的具体地址。如目录和文件名等

### 总结

URI是以一种抽象的，高层次概念定义统一资源标识，而URL和URN则是具体的资源标识的方式。URL和URN都是一种URI。笼统地说，每个 URL 都是 URI，但不一定每个 URI 都是 URL。这是因为 URI 还包括一个子类，即统一资源名称 (URN)，它命名资源但不指定如何定位资源。上面的 mailto、news 和 isbn URI 都是 URN 的示例。

在Java的URI中，一个URI实例可以代表绝对的，也可以是相对的，只要它符合URI的语法规则。而URL类则不仅符合语义，还包含了定位该资源的信息，因此它不能是相对的。
 在Java类库中，URI类不包含任何访问资源的方法，它唯一的作用就是解析。
 相反的是，URL类可以打开一个到达资源的流。

> URN，uniform resource name，统一资源命名，是通过名字来标识资源，比如[mailto:java-net@java.sun.com](https://link.jianshu.com/?t=mailto:java-net@java.sun.com)。

# HTTP请求和响应

## 概述

HTTP请求即：客户端——>服务器 发送的消息

HTTP响应即：服务端——>客户端 发送的消息

## 格式分析

### Get请求

#### GET请求格式

```undefined
GET /books/?sex=man&name=Professional HTTP/1.1
Host: www.wrox.com
User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.7.6)
Gecko/20050225 Firefox/1.0.1
Connection: Keep-Alive

```

> 注意最后一行是空行

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210209193542.png)

#### 常见的Get请求

1. form标签`method=get`
2. a标签
3. link标签引入css
4. Script标签引入js文件
5. img标签引入图片
6. iframe引入html页面
7. 在浏览器地址栏中输入地址后敲回车

### Post请求

#### POST请求格式

```dart
POST / HTTP/1.1
Host: www.wrox.com
User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.7.6)
Gecko/20050225 Firefox/1.0.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 40
Connection: Keep-Alive

name=Professional%20Ajax&publisher=Wiley
```

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210209193615.png)

#### 常见的Post请求

1. form标签`method=post`

### Get请求与Post请求的区别

#### 请求参数不同

GET请求参数是通过URL传递的，多个参数以&连接，POST请求放在request body中。

GET只接受ASCII字符，而POST没有限制。

GET请求参数长度最多1024kb，POST对请求数据没有限制

> 关于此点，在HTTP协议中没有对URL长度进行限制，这个限制是**不同的浏览器及服务器**由于有不同的规范而带来的限制。

#### 请求缓存不同

GET请求会被缓存，而POST请求不会，除非手动设置

#### 书签功能不同

GET请求支持，POST请求不支持。

#### 安全性不同

POST比GET安全，GET请求在浏览器回退时是无害的，而POST会再次请求。

#### 历史记录不同

GET请求参数会被完整保留在浏览历史记录里，而POST中的参数不会被保留。

#### 编码方式不同

GET请求只能进行url编码，而POST支持多种编码方式。

> PS：通过浏览器地址栏输入URL访问资源的方式都是`GET`请求。

### 响应消息

一般情况下，服务器接收并处理客户端发过来的请求后会返回一个HTTP的响应消息。

**HTTP响应也由四个部分组成，分别是：状态行、消息报头、空行和响应正文。**

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210210083001.png)

我们打开百度首页，F12进入控制台，得到如下页面：

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220011612.png)

我已经在图中，对各个部分做了简单的注释，部分需要详解的内容，下面我会放在下面。

## 请求方式

根据HTTP标准，HTTP请求可以使用多种请求方法（启动GET和POST最为常见）。

+ HTTP1.0定义了三种请求方法： GET, POST 和 HEAD方法。
+  HTTP1.1新增了五种请求方法：OPTIONS, PUT, DELETE, TRACE 和 CONNECT 方法。

```csharp
GET  	请求指定的页面信息，并返回实体主体。
HEAD     类似于get请求，只不过返回的响应中没有具体的内容，用于获取报头
POST     向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST请求可能会导致新的资源的建立和/或已有资源的修改。
PUT  	从客户端向服务器传送的数据取代指定的文档的内容。
DELETE   请求服务器删除指定的页面。
CONNECT  HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器。
OPTIONS  允许客户端查看服务器的性能。
TRACE    回显服务器收到的请求，主要用于测试或诊断。
```

## 状态码

状态代码有三位数字组成，第一个数字定义了响应的类别，共分五种类别:

+ 1xx：指示信息--表示请求已接收，继续处理
+ 2xx：成功--表示请求已被成功接收、理解、接受
+ 3xx：重定向--要完成请求必须进行更进一步的操作
+ 4xx：客户端错误--请求有语法错误或请求无法实现
+ 5xx：服务器端错误--服务器未能实现合法的请求

**常见状态码**

```
200 OK                        //客户端请求成功
400 Bad Request               //客户端请求有语法错误，不能被服务器所理解
401 Unauthorized              //请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用 
403 Forbidden                 //服务器收到请求，但是拒绝提供服务
404 Not Found                 //请求资源不存在，eg：输入了错误的URL
500 Internal Server Error     //服务器发生不可预期的错误
503 Server Unavailable        //服务器当前不能处理客户端的请求，一段时间后可能恢复正常
```

## MIME 类型说明

### 概述

MIME 是 HTTP 协议中数据类型。

MIME 的英文全称是"Multipurpose Internet Mail Extensions" 多功能 Internet 邮件扩充服务。MIME 类型的格式是“大类型/小 类型”，并与某一种文件的扩展名相对应。

### 常见的MIME类型

|        文件        |                    MIME类型                     |
| :----------------: | :---------------------------------------------: |
| 超文本标记语言文本 |         `.html`、`.htm`对应`text/html`          |
|      普通文件      |             `.txt`对应`text/plain`              |
|      RTF文本       |           `.rtf`对应`application/rtf`           |
|      GIF图形       |              `.gif`对应`image/gif`              |
|      JPEG图形      |         `.jpeg`、`.jpg`对应`image/jpeg`         |
|     au声音文件     |             `.au`对应`audio/basic`              |
|    MIDI音乐文件    | `.mid`、`.midi`对应`audio/midi`、`audio/x-midi` |
| RealAudio音乐文件  |     `.ra`、`.ram`对应`audio/x-pn-realaudio`     |
|      MPEG文件      |         `.mpg`、`.mpeg`对应`video/mepg`         |
|      AVI文件       |           `.avi`对应`video/x-msvideo`           |
|      GZIP文件      |          `.gz`对应`application/x-gzip`          |
|      TAR文件       |          `.tar`对应`application/x-tar`          |

# HTTP工作原理

## 工作原理概述

HTTP协议定义Web客户端如何从Web服务器请求Web页面，以及服务器如何把Web页面传送给客户端。

HTTP协议采用了请求/响应模型。客户端向服务器发送一个请求报文，请求报文包含请求的方法、URL、协议版本、请求头部和请求数据。

服务器以一个状态行作为响应，响应的内容包括协议的版本、成功或者错误代码、服务器信息、响应头部和响应数据。

## 工作步骤

### 1.  户端连接到Web服务器

一个HTTP客户端，通常是浏览器，与Web服务器的HTTP端口（默认为80）建立一个TCP套接字连接。例如，[http://www.oakcms.cn](https://link.jianshu.com/?t=http://www.oakcms.cn)。

### 2. 发送HTTP请求

通过TCP套接字，客户端向Web服务器发送一个文本的请求报文，一个请求报文由请求行、请求头部、空行和请求数据这四个部分组成。

### 3. 服务器接受请求并返回HTTP响应

Web服务器解析请求，定位请求资源。服务器将资源复本写到TCP套接字，由客户端读取。一个响应由状态行、响应头部、空行和响应数据4部分组成。

### 4. 释放连接[TCP连接](https://www.jianshu.com/p/ef892323e68f)

若connection 模式为close，则服务器主动关闭[TCP连接](https://www.jianshu.com/p/ef892323e68f)，客户端被动关闭连接，释放[TCP连接](https://www.jianshu.com/p/ef892323e68f);若connection 模式为keepalive，则该连接会保持一段时间，在该时间内可以继续接收请求;

### 5. 客户端浏览器解析HTML内容

客户端浏览器首先解析状态行，查看表明请求是否成功的状态代码。然后解析每一个响应头，响应头告知以下为若干字节的HTML文档和文档的字符集。客户端浏览器读取响应数据HTML，根据HTML的语法对其进行格式化，并在浏览器窗口中显示。

例如：在浏览器地址栏键入URL，按下回车之后会经历以下流程：

1. 浏览器向 DNS 服务器请求解析该 URL 中的域名所对应的 IP 地址;
2. 解析出 IP 地址后，根据该 IP 地址和默认端口 80，和服务器建立[TCP连接](https://www.jianshu.com/p/ef892323e68f);
3. 浏览器发出读取文件(URL 中域名后面部分对应的文件)的HTTP 请求，该请求报文作为 [TCP 三次握手](https://www.jianshu.com/p/ef892323e68f)的第三个报文的数据发送给服务器;
4. 服务器对浏览器请求作出响应，并把对应的 html 文本发送给浏览器;
5. 释放 [TCP连接](https://www.jianshu.com/p/ef892323e68f);
6. 浏览器将该 html 文本并显示内容

--------------------

> 资源参考：
>
> [关于HTTP协议，一篇就够了](https://www.jianshu.com/p/80e25cb1d81a)
>
> [【网络协议】彻底弄清POST和GET请求的区别，这次你GET了么](https://segmentfault.com/a/1190000023940344)