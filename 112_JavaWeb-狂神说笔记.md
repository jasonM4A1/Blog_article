---
title: JavaWeb-狂神说笔记(未完)
date: 2021-02-02 01:40:00
tags:
	- JavaWeb
	- Java
	- 前端
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202013902.jpg
---

# 基本概念

## 前言

JavaWeb开发，顾名思义就是使用Java相关的技术，进行一些Web相关的网站开发。

Web翻译为中文，可以被称为网页、网站等。其中这个Web分为【静态Web】和【动态Web】。

## 静态web

内容主要包括：HTML、CSS。

### 优点

简单，占用资源少，易开发和部署。

### 缺点

1. Web页面无法动态更新，所有用户看到都是同一个页面
2. 它无法和数据库交互，数据无法持久化，用户无法交互

## 动态web

目前我们日常见到的网站，几乎都是动态Web的网站。例如：淘宝，京东，B站等。

其特征为：提供给所有人看的数据始终会发生变化，每个人在不同的时间，不同的地点看到的信息各不相同。

所使用到的技术栈：**Servlet/JSP**，ASP，PHP等。

### 优点

1. Web页面可以动态更新，所有用户看到都不是同一个页面
2. 它可以与数据库交互 （数据持久化：注册，商品信息，用户信息……..）

### 缺点

需要停机维护

### 相关技术简介

#### JSP/Servlet

- sun公司主推的B/S架构
- 基于Java语言的 (所有的大公司，或者一些开源的组件，都是用Java写的)
- 可以承载三高问题带来的影响
- 语法像ASP

#### ASP

- ASP 指 Active Server Pages （动态服务器页面）；ASP 是一项微软公司的技术
- ASP 是在 IIS 中运行的程序；IIS 指 Internet Information Services （Internet 信息服务）
- ASP 文件和 HTML 文件类似；ASP 文件可包含文本、HTML、XML 和脚本
- ASP 文件中的脚本可在服务器上执行；ASP 文件的扩展名是 “.asp”
- 可以动态地编辑、改变或者添加页面的任何内容
- 对由用户从 HTML 表单提交的查询或者数据作出响应
- 访问数据或者数据库，并向浏览器返回结果
- 为不同的用户定制网页，提高这些页面的可用性

#### PHP

- PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。
- PHP 是免费的，并且使用非常广泛。同时，对于像微软 ASP 这样的竞争者来说，PHP 无疑是另一种高效率的选项。
- PHP开发速度很快，功能很强大，跨平台，代码很简单 （70% , WP）
- 无法承载大访问量的情况（局限性）

## web应用程序

Web应用组成：

- HTML，CSS，JavaScript
- JSP，Servlet
- Java程序
- Jar包
- Properties配置文件
- 等等

# Tomcat

## 概述

web服务器一种被动的操作，用来处理用户的一些请求和给用户一些响应信息；

常见的web服务器有IIS、Tomcat。其中IIS是微软公司的，在Windows中自带，应用与ASP中。

而Tomcat是Apache 软件基金会（Apache Software Foundation）的Jakarta 项目中的一个核心项目，最新的Servlet 和JSP 规范总是能在Tomcat 中得到体现，因为Tomcat 技术先进、性能稳定，而且**免费**，因而深受Java 爱好者的喜爱并得到了部分软件开发商的认可，成为目前比较流行的Web 应用服务器。

Tomcat 服务器是一个免费的开放源代码的Web 应用服务器，属于轻量级应用[服务器](https://baike.baidu.com/item/服务器)，在中小型系统和并发访问用户不是很多的场合下被普遍使用，是开发和调试JSP 程序的首选。对于一个Java初学web的人来说，它是最佳的选择

Tomcat 实际上运行JSP 页面和Servlet。Tomcat最新版本为**9.0。**

## 安装Tomcat

需要注意的是，虽说是安装Tomcat，其实只需要将下载的Tomcat压缩包解压出来既可。

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201219214351.png)

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201219214403.png)

## Tomcat目录详解

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201219214603.png)

## 启动、关闭Tomcat

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201219214638.png)

然后访问测试：http://localhost:8080/，得到如下的页面，则表面启动Tomcat成功。

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201219215039.png)

我们还可以访问`http://localhost:8080/examples`，来测试、学习Tomcat，如下：

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201219231906.png)

我们也可以部署自己的项目，在如下目录中创建自己的项目文件夹：

然后通过`http://localhost:8080/项目文件夹名`来访问自己的项目。

## Tomcat项目文件夹

最开始我们默认的Tomcat项目文件夹都是在`D:\Tomcat\apache-tomcat-8.5.61\webapps`下的，如图：

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201219233230.png)

上面的每一个文件夹都是一个项目，我们采用`http://localhost:8080`访问的是默认项目文件夹`ROOT`。当然我们也可以通过`http://localhost:8080/项目文件夹名`的方式来访问指定的项目。如：现在我来在浏览器中输入`http://localhost:8080/examples`来访问Tomcat为我们提供的【代码实例项目】。

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201219233605.png)

我们以后的网站目录结构为：

```
--webapps
	--ROOT
	--web网站目录名
		--WEB-INF
            --classes：Java程序
            --lib：web应用所依赖的Jar包
            --web.xml：网站配置文件
		--index.html：默认的首页
		--static
			--css
			--js
			-img
		--
		......
```

### 案例

#### 案例要求

通过前面的学习，我们知道了可以通过`http://localhost:8080/项目文件夹名`的方式来访问指定的项目。现在要求我们发布并访问自己的web项目。

#### 案例实现

1. 首先创建一个自己的项目文件，这里我们可以直接复制`ROOT`文件夹，然后修改文件夹名和里面的内容。

   ![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201219234419.png)

   ![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201219234707.png)

   ![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201219234642.png)

2. 在浏览器中输入`http://localhost:8080/项目文件夹名`。

   ![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201219234555.png)

## Tomcat配置

打开下面所示位置文件

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201219214727.png)

### 端口相关配置

大概定位到69行，或直接搜索`<Connector`：

```
<Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
```

- `port`：端口号
- `protocol`：HTTP协议版本（1.0或1.1）
- `connectionTimeout`：连接超时时限
- `redirectPort`：重定向端口

### 主机相关配置

大概定位到152行，或直接搜索`<Host`：

```
<Host name="localhost"  appBase="webapps"
      unpackWARs="true" autoDeploy="true">
```

- `name`：主机名
- `appBase`：网站文件存放位置
- `unpackWARS`：是否解压压缩包
- `autoDeploy`：是否自动部署

### 高难度面试题

#### 题目

请你谈谈网站是如何进行访问的?

#### 解答

1. 输入一个域名；回车
2. 检查本机的 C:\Windows\System32\drivers\etc\hosts配置文件下有没有这个域名映射；
   - 有：直接返回对应的ip地址，这个地址中，有我们需要访问的web程序，可以直接访问
   - 没有：去DNS服务器找，找到的话就返回，找不到就返回找不到；

# HTTP协议

## 概述

HTTP（超文本传输协议）是一个简单的请求-响应协议，它通常运行在TCP之上。

## 版本

### HTTP1.0

客户端可以与web服务器连接后，只能获得一个web资源，断开连接

### HTTP1.1

客户端可以与web服务器连接后，可以获得多个web资源，不会断开连接

## HTTP请求和响应

### 概述

HTTP请求即：客户端——>服务器 发送的消息

HTTP响应即：服务端——>客户端 发送的消息

我们打开百度首页，F12进入控制台，得到如下页面：

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220011612.png)

我已经在图中，对各个部分做了简单的注释，部分需要详解的内容，下面我会放在下面。

### 请求方式

请求行中显示的请求方法有好多种，如：**Get**、**Post**、Head、Delete等。

- Get请求方式：能够携带的参数比较少，大小有限制，会在浏览器的URL地址栏显示数据内容，不安全，但高效
- Post请求方式：能够携带的参数没有限制，大小没有限制，不会在浏览器的URL地址栏显示数据内容，安全，但效率较低

### 状态码

请求行中显示的状态码也有好多种，如：200、3XX、4XX、5XX等。

- 200：请求响应成功
- 3XX：请求重定向
- 4XX：如404，找不到资源
- 5XX：服务器代码错误，如502的网关错误

# Maven

## 概述

在Javaweb开发中，需要使用大量的jar包，我们手动去导入。为了能过高效、安全的导入和配置我们开发中所需的Jar包，Maven就出现了。

Maven目前对于我们来说，就是方便用来导入Jar包的。

Maven的核心思想为：**约定大于配置**。Maven会规定好你该如何去编写我们的Java代码，必须要按照这个规范来。

## 下载安装Maven

进入Maven官网：https://maven.apache.org/。下载Maven压缩包，如图所示：

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202031521.png)

对于Maven也是一样，下载完成后解压即可。

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220014658.png)

## 配置环境变量

1. 新建系统变量，变量名为：`M2_HOME`，变量值为：`D:\apache-maven-3.6.3\bin`
2. 新建系统变量，变量名为：`MAVEN_HOME`，变量值为：`D:\apache-maven-3.6.3`
3. 在系统变量Path中新建，变量值为：`%MAVEN_HOME%\bin`

测试，cmd中输入`mvn -version`，得到返回如下结果，则说明环境变量配置成功：

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220020343.png)

## 配置镜像

打开`conf`文件夹中的`setting.xml`文件，搜索`<mirrors>`，在这里面加入阿里云的镜像。

```
    <mirror>
      <id>aliyunmaven</id>
      <mirrorOf>central</mirrorOf>
      <name>aliyun maven</name>
      <url>https://maven.aliyun.com/repository/public </url>
    </mirror>
    <mirror>
      <id>aliyunmaven</id>
      <mirrorOf>central</mirrorOf>
      <name>阿里云公共仓库</name>
      <url>https://maven.aliyun.com/repository/central</url>
    </mirror>
    <mirror>
      <id>repo1</id>
      <mirrorOf>central</mirrorOf>
      <name>central repo</name>
      <url>http://repo1.maven.org/maven2/</url>
    </mirror>
    <mirror>
      <id>aliyunmaven</id>
      <mirrorOf>apache snapshots</mirrorOf>
      <name>阿里云阿帕奇仓库</name>
      <url>https://maven.aliyun.com/repository/apache-snapshots</url>
    </mirror>
```

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220025047.png)

> 需要注意的是，可以配置多个镜像，每个镜像使用标签`<mirror></mirror>`包括起来(不带s)。

## 配置本地仓库

首先在`pache-maven-3.6.3`目录下新建一个目录`maven-repo`。

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220021713.png)

然后还是打开`conf`文件夹中的`setting.xml`文件，搜索`<!-- localRepository`，在此条注释后加入刚才新建的目录地址，用来指定本地仓库地址。不然Maven默认会在`${user.home}/.m2/repository`新建本地仓库。

```
<!-- localRepository
   | The path to the local repository maven will use to store artifacts.
   |
   | Default: ${user.home}/.m2/repository
  <localRepository>/path/to/local/repo</localRepository>
  -->
  <localRepository>D:\apache-maven-3.6.3\maven-repo</localRepository>
```

## 使用Idea创建Maven项目

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220150130.png)

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220031949.png)

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220032117.png)

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220032227.png)

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220032326.png)

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220032341.png)

出现以下情况，则说明创建Maven项目成功。

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220032833.png)

## Idea中Maven项目配置

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220145509.png)

上面红色框住的部分，都是可以使用Idea对创建好的的Maven项目进行的配置选项。

## Idea普通Maven项目目录说明

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220151315.png)

## Idea标记文件夹

我们在使用模板创建一个Maven项目后，是没有Java源代码目录和resources配置文件目录的，是需要我们自己手动添加。但是普通创建的目录后，还需要标记一下，才能使用。方法有二种：

1. 直接在工程中，右键创建好的目录，如图所示：

   ![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220152225.png)

2. 在Project structure中进行标记：

   ![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220152356.png)

## 在Idea中配置Tomcat

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220153111.png)

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220153454.png)

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220153548.png)

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220153639.png)

最后OK确定，既可初步配置Tomcat完成。

## pom.xml文件详解

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220155734.png)

## Maven远程仓库的使用

首先进入https://mvnrepository.com/，找到自己需要的Jar包，然后复制配置代码，粘贴到pom.xml对应的位置。

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220162348.png)

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220162209.png)

然后Maven就会自动将这jar包所需的相关Jar包都给download下来。

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220162538.png)

这里需要注意的是，自己Maven配置是否正确。需要保证Maven本地仓库和配置文件是自己设置的（否则粘贴进入的代码，会报红）。如图：

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220163302.png)

## 使用Idea全局配置Maven设置

我们每次使用Idea创建Maven后，都要手动更改Maven的更目录文件夹、配置文件和本地仓库。我们可以全局对Maven进行设置，避免每次重复。如图：

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220163544.png)

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220163616.png)

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220163724.png)

# Servlet

## 概述

Servlet就是sun公司开发动态web的一门技术

Java Servlet 是运行在 Web 服务器或应用服务器上的程序，它是作为来自 Web 浏览器或其他 HTTP 客户端的请求和 HTTP 服务器上的数据库或应用程序之间的中间层。

## Servlet接口及相关类

servlet程序中，与之相关的接口和类有三个，下面分别介绍一下这三个类或接口。

### 继承关系

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220234011.png)

### Servlet接口

Servlet接口默认有两个实现类，这两个实现类同时也为父子类关系——GenericServlet(父)和HttpServlet(子)。

Servlet接口去除注释、注解等，源代码大致为：

```
package javax.servlet;

import java.io.IOException;

public interface Servlet {
 	// 用于servlet初始化。init()方法在servlet生命周期中，只会被调用一次。
    public void init(ServletConfig config) throws ServletException;
    
    // 获取servlet的配置信息，例如：servlet名称、路径、参数等等。可以调用多次
    public ServletConfig getServletConfig();
    
    // 处理客户端的请求。每次请求都会被调用一次。
    public void service(ServletRequest req, ServletResponse res)
	throws ServletException, IOException;
    
    //返回一个String类型的字符串，其中包括了关于Servlet的信息，例如，作者、版本和版权。可以调用多次。
    public String getServletInfo();
    
    //当servlet被卸载或者tomcat服务器停止运行时，调用destroy()销毁方法，用于释放一些资源。在servlet生命周期中，只会被调用一次。
    public void destroy();
}
```

Servlet接口方法的作用简述，我已经注解在了各个方法的上面。

### GenericServlet抽象类

GenericServlet是位于javax.servlet包下的抽象类，并且实现自Servlet接口。

[![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220233512.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220233512.png)

GenericServlet类中提供了一个ServletConfig成员对象，用于保存servlet的配置信息。在Tomcat服务器调用init()方法时，会将ServletConfig对象赋值给config成员变量。

GenericServlet类对Servlet接口中的init()、destroy()、getServletInfo()、getServletConfig()方法进行了实现，但是对service()方法没有进行实现，因为service方法需要开发人员自己编写业务逻辑代码。

除了Servlet接口中的五个方法，GenericServlet抽象类中还提供了getInitParameter()、getServletContext()、getServletName()等方法。

- `String getInitParameter(String name)`：用于获取servlet初始化参数。
- `ServletContext getServletContext()`：获取servlet的上下文对象。一个web工程中只有一个servlet上下文对象。
- `String getServletName()`：获取servlet的名称。即：在web.xml配置文件中对应servlet的标签的名称。

### HttpServlet抽象类

HttpServlet是位于javax.servlet.http包下的抽象类，继承自GenericServlet抽象类。它不仅拥有GenericServlet类中的所有方法，而且HttpServlet类对service方法进行了重载，并且还添加了对应的doGet()、doPost()等http请求方法。

我们进行Servlet业务开发，都是创建一个类然后继承`HttpServlet`类，然后重写它的`service()`方法既可。

## 部署简单的Servlet程序

1. 配置Maven，添加Servlet程序所依赖的jar包。

   ![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201221001028.png)

2. 源代码目录中创建一个类并继承`HttpServlet`类，然后实现其中的方法。

   ![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201221001634.png)

3. 配置`webapp`——>`WEB-INF`下的`web.xml`文件。为动态web项目添加映射。

   ![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201221001447.png)

4. 启动Tomcat服务器。

   ![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201221001840.png)

5. 在浏览器输入对应的映射地址，查看是否打印成功。

   ![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201221002106.png)

### 虚拟地址映射的相关问题

1. 一个`servlet`可以映射多个`servlet-mapping`，访问时随便使用一个映射地址都可以。

   ![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201221005917.png)

2. 一个`Servlet`可以指定通用映射路径

   ![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201221010326.png)

3. 还可以配置默认请求路径，即服务器启动之初开启的页面

   ![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201221011607.png)

   > 指定了固有的映射路径优先级最高，如果找不到才会走默认的处理请求；

4. 为虚拟路径指定前缀或后缀。

   ![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201221012131.png)

## Servlet原理

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201221003423.png)

## ServletContext

### 概述

web容器在启动的时候，它会为每个web程序都创建一个对应的ServletContext对象，它代表了当前的web应用

### 方法总结

- `ServletContext getServletContext()`：获取本web程序的ServletContext对象（直接使用this调用既可）。
- `void setAttribute(String name, Object object)`：向ServletContext对象中存储数据，`String name`为键，`Object object`为存储的数据。
- `Object getAttribute(String name)`：传入字符串键名，取出存储在ServletContext对象中的数据。
- `String getInitParameter(String name)`：获取`xml`配置文件中定义的参数值。
- `RequestDispatcher getRequestDispatcher(String path)`：传入要转发到的虚拟路径，来获取一个请求转发对象。
- `void forward(ServletRequest request, ServletResponse response)`：由`RequestDispatcher`对象调用，启动转发。

### 案例-共享数据

因为ServletContext对象是一个web程序中所有类共享的，所以存入ServletContext中的数据，也是所有类共享的。

- 在一个类中存入数据

  ```
  public class HelloServlet extends HttpServlet {
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          // 获取ServletContext对象
          ServletContext servletContext = this.getServletContext();
          //向ServletContext对象中存入数据
          String s = "Jason";
          servletContext.setAttribute("employee",s);
      }
  
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          doGet(req, resp);
      }
  }
  ```

- 在另一个类中取出数据

  ```
  public class getHelloServlet extends HttpServlet {
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          //获取ServletContext对象
          ServletContext servletContext = this.getServletContext();
          //取出ServletContext对象中存储的数据
          Object employee = servletContext.getAttribute("employee");
          System.out.println(employee);
      }
  
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          doGet(req, resp);
      }
  }
  ```

### 案例-获取初始化参数

我们在`web.xml`中配置的`<context-param>`的参数数据，同样可以在Java程序中获取到。

- web.xml文件中加入下列代码，增加初始化参数

  ```
  <context-param>
      <param-name>url</param-name>
      <param-value>jdbc:mysql://localhost:3306/mybatis</param-value>
  </context-param>
  ```

- 创建类并继承HttpServlet，然后书写获取初始化参数数据

  ```
  public class paramHello extends HttpServlet {
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          //获取ServletContext对象
          ServletContext servletContext = this.getServletContext();
          //通过传入键值对的名，来获取初始化参数的值
          String url = servletContext.getInitParameter("url");
          System.out.println(url);
      }
  
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          doGet(req, resp);
      }
  }
  ```

### 案例-请求转发

```
public class Dispatcher extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取ServletContext对象
        ServletContext servletContext = this.getServletContext();
        //获取RequestDispatcher对象，参数为转发的路径
        RequestDispatcher requestDispatcher = servletContext.getRequestDispatcher("/paramHello");
        //进行请求转发
        requestDispatcher.forward(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

### 案例-读取资源文件

我们在`resources`目录中创建的`properties`文件，都会被打包到网站根目录`web-servlet`下的`WEB-INF`——>`classes`目录。

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201221171149.png)

这样我们就可以通过`ServletContext`对象结合IO流，来获取网站目录中打包到的`properties`配置文件中的信息。代码操作如下：

```
public class GetProperties extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取指定配置文件的资源流
        InputStream resourceAsStream = this.getServletContext().getResourceAsStream("/WEB-INF/classes/users.properties");
        //创建Properties对象
        Properties properties = new Properties();
        //加载配置文件
        properties.load(resourceAsStream);
        //获取配置文件中的信息
        String username = properties.getProperty("username");
        String password = properties.getProperty("password");
        //打印获取到的信息到网页页面
        resp.getWriter().println(username + ": " + password);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

## HttpServletResponse

### 概述

web服务器接收到客户端的http请求，针对这个请求，分别创建一个代表请求的HttpServletRequest对象，一个代表响应的HttpServletResponse对象。即：

- 如果要获取客户端请求过来的参数：使用`HttpServletRequest`
- 如果要给客户端响应一些信息：使用`HttpServletResponse`

### 方法总结

#### 负责向浏览器发送数据的方法

```
ServletOutputStream getOutputStream() throws IOException;

PrintWriter getWriter() throws IOException;
```

#### 负责向浏览器发送响应头的方法

```
void setCharacterEncoding(String var1);

void setContentLength(int var1);

void setContentLengthLong(long var1);

void setContentType(String var1);

void setDateHeader(String var1, long var2);

void addDateHeader(String var1, long var2);

void setHeader(String var1, String var2);

void addHeader(String var1, String var2);

void setIntHeader(String var1, int var2);

void addIntHeader(String var1, int var2);
```

#### 其他常用方法

```
public void addCookie(Cookie cookie);//将指定的Cookie加入到当前的响应中

public void addHeader(String name, String value);//将指定的名字和值加入到响应的头信息中

public boolean containsHeader(String name);//返回一个布尔值，判断响应的头部是否被设置

public String encodeURL(String url);//编码指定的URL

public void sendError(int sc, String msg) throws IOException;//	使用指定状态码发送一个错误到客户端

public void sendRedirect(String location) throws IOException;//发送一个临时的响应到客户端

public void setDateHeader(String name, long date);//将给出的名字和日期设置响应的头部

public void setHeader(String name, String value);//将给出的名字和值设置响应的头部

public void setStatus(int sc);//给当前响应设置状态码
```

### 案例-下载文件

我们需要完成一个加载页面，自动下载图片的功能。

```
public class DownloadTest extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //输入要下载文件的路径
        String s = "D:\\Java\\web\\web-Response\\target\\web-Response\\WEB-INF\\classes\\wallhaven-8x8z5o.jpg";
        //获取下载文件的名字
        String fileName = s.substring(s.lastIndexOf("\\") + 1);
        //设置浏览器响应头部，使浏览器下载文件，同时保证不出现中文乱码
        resp.setHeader("Content-Disposition","attachment;filename="+ URLEncoder.encode(fileName,"UTF-8"));
        //获取下载文件的输入流
        FileInputStream in = new FileInputStream(s);;
        //创建缓冲区
        int len=0;
        byte[] bytes = new byte[1024];
        //获取OutputStream对象
        ServletOutputStream outputStream = resp.getOutputStream();
        //读写数据
        while ((len=in.read(bytes)) != -1) {
            outputStream.write(bytes,0,len);
        }
        //关闭流资源
        in.close();
        outputStream.close();
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

### 案例-用户登录

这个案例需要创建的文件，如下图：

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201221222527.png)

文件代码如下：

- `Login.java`

  ```
  public class Login extends HttpServlet {
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          //模仿数据库中的账号密码信息
          String name = "Jason";
          String pwd = "qaqqaq";
          //获取用户输入的帐号密码
          String username = req.getParameter("username");
          String password = req.getParameter("password");
          //检查账号密码信息是否相同
          if (name.equals(username) && pwd.equals(password)) {
              resp.sendRedirect("/web_Response_war_exploded/LoginSuccess.jsp");
          }else {
              resp.sendRedirect("/web_Response_war_exploded/LoginFail.jsp");
          }
      }
  
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          doGet(req, resp);
      }
  }
  ```

- `Login.jsp`

  ```
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>Login</title>
  </head>
  <body>
      <form action="${pageContext.request.contextPath}/Login" method="get">
          用户名：<input type="text" name="username"><br/>
          密码：<input type="password" name="password"><br/>
          <input type="submit" value="提交">
      </form>
  </body>
  </html>
  ```

- `LoginFail.jsp`

  ```
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>LoginFail</title>
  </head>
  <body>
      <h1>小丑啊！失败了啊！</h1>
  </body>
  </html>
  ```

- `LoginSuccess.jsp`

  ```
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>LoginSuccess</title>
  </head>
  <body>
      <h1>恭喜你，登录成功！</h1>
  </body>
  </html>
  ```

**用户登录演示：**

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201221223008.png)

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201221223121.png)

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201221223227.png)

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201221223323.png)

## HttpServletRequest

### 概述

HttpServletRequest代表客户端的请求，用户通过Http协议访问服务器，HTTP请求中的所有信息会被封装到HttpServletRequest，通过这个HttpServletRequest的方法，获得客户端的所有信息。

### 方法总结

| 方 法                           | 说 明                                                        |
| ------------------------------- | ------------------------------------------------------------ |
| getAttributeNames()             | 返回当前请求的所有属性的名字集合                             |
| getAttribute(String name)       | 返回name指定的属性值                                         |
| getCookies()                    | 返回客户端发送的Cookie                                       |
| getsession()                    | 返回和客户端相关的session，如果没有给客户端分配session，则返回null |
| getsession(boolean create)      | 返回和客户端相关的session，如果没有给客户端分配session，则创建一个session并返回 |
| getParameter(String name)       | 获取请求中的参数，该参数是由name指定的                       |
| getParameterValues(String name) | 返回请求中的参数值，该参数值是由name指定的                   |
| getCharacterEncoding()          | 返回请求的字符编码方式                                       |
| getContentLength()              | 返回请求体的有效长度                                         |
| getInputStream()                | 获取请求的输入流中的数据                                     |
| getMethod()                     | 获取发送请求的方式，如get、post                              |
| getParameterNames()             | 获取请求中所有参数的名字                                     |
| getProtocol()                   | 获取请求所使用的协议名称                                     |
| getReader()                     | 获取请求体的数据流                                           |
| getRemoteAddr()                 | 获取客户端的IP地址                                           |
| getRemoteHost()                 | 获取客户端的名字                                             |
| getServerName()                 | 返回接受请求的服务器的名字                                   |
| getServerPath()                 | 获取请求的文件的路径                                         |

### 案例-获取参数，请求转发

和上一个的用户登录案例差不多，只不过这一次我们需要获取用户多选框选择的数据，并输出到控制台上。我们需要创建的文件，如图所示：

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201221235302.png)

上图所示文件代码，如下：

- `GetParameter.java`

  ```
  public class GetParameter extends HttpServlet {
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          //定义编码为utf-8
          req.setCharacterEncoding("utf-8");
          resp.setCharacterEncoding("utf-8");
          //模拟数据库中的账号密码信息
          String name = "Jason";
          String pwd = "qaqqaq";
          //获取用户输入的账号密码
          String username = req.getParameter("username");
          String password = req.getParameter("password");
          //账号密码检查
          if (name.equals(username) && pwd.equals(password)) {
              //获取用户输入的其他数据
              String[] hobbies = req.getParameterValues("hobbies");
              System.out.println("登录成功，下面是用户输入的爱好信息：");
              //遍历数据到控制台
              for (String hobby : hobbies) {
                  System.out.print(hobby + "; ");
              }
              System.out.println();
              //使用请求转发到登录成功的页面
              req.getRequestDispatcher("/LoginSuccess.jsp").forward(req, resp);
          } else {
              //输入错误提示到控制台
              System.out.println("你可真是一个小丑啊！你！密码输错了啊！");
              //使用请求转发到登录失败的页面
              req.getRequestDispatcher("/LoginFail.jsp").forward(req, resp);
          }
      }
  
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          doGet(req, resp);
      }
  }
  ```

- `Login.jsp`

  ```
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>Login</title>
  </head>
  <body>
      <form action="${pageContext.request.contextPath}/getParameter" method="post">
          用户名：<input type="text" name="username"> <br/>
          密码：<input type="password" name="password"> <br/>
          爱好：<input type="checkbox" name="hobbies" value="game">
              <input type="checkbox" name="hobbies" value="reading">
              <input type="checkbox" name="hobbies" value="make love"> <br/>
          <input type="submit" value="提交">
      </form>
  </body>
  </html>
  ```

- `LoginFail.jsp`

  ```
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>LoginFail</title>
  </head>
  <body>
      <h1>登录失败</h1>
  </body>
  </html>
  ```

- `LoginSuccess.jsp`

  ```
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>LoginSuccess</title>
  </head>
  <body>
      <h1>登录成功！</h1>
  </body>
  </html>
  ```

**案例使用演示：**

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201221235650.png)

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201221235924.png)

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201222000021.png)

## 会话机制

### 概述

Web程序中常用的技术，用来**跟踪用户的整个会话**。常用的会话跟踪技术是Cookie与Session。**Cookie通过在客户端记录信息确定用户身份**，**Session通过在服务器端记录信息确定用户身份**。

### Cookie

#### 概述

HTTP协议本身是无状态的。什么是无状态呢，即服务器无法判断用户身份。Cookie实际上是一小段的文本信息（key-value格式）。客户端向服务器发起请求，如果服务器需要记录该用户状态，就使用response向客户端浏览器颁发一个Cookie。客户端浏览器会把Cookie保存起来。当浏览器再请求该网站时，浏览器把请求的网址连同该Cookie一同提交给服务器。服务器检查该Cookie，以此来辨认用户状态。

#### 过程

当用户第一次访问并登陆一个网站的时候，cookie的设置以及发送会经历以下4个步骤：

**客户端发送一个请求到服务器** ——> **服务器发送一个HttpResponse响应到客户端，其中包含Set-Cookie的头部** ——> **客户端保存cookie，之后向服务器发送请求时，HttpRequest请求中会包含一个Cookie的头部** ——>**服务器返回响应数据**

#### 属性值

| 属性项     | 属性项介绍                                                   |
| ---------- | ------------------------------------------------------------ |
| NAME=VALUE | 键值对，可以设置要保存的 Key/Value，注意这里的 NAME 不能和其他属性项的名字一样 |
| Expires    | 过期时间，在设置的某个时间点后该 Cookie 就会失效             |
| Domain     | 生成该 Cookie 的域名，如 domain=”[www.baidu.com](http://www.baidu.com/)“ |
| Path       | 该 Cookie 是在当前的哪个路径下生成的，如 path=/wp-admin/     |
| Secure     | 如果设置了这个属性，那么只会在 SSH 连接时才会回传该 Cookie   |

#### 常用方法

- `new Cookie(String name, String value)`：创建一个Cookie对象。
- `void setMaxAge(int expiry)`：设置Cookie有效期
- `void addCookie(Cookie cookie)`：将指定的cookie添加到响应中。
- `Cookie[] getCookies()`：获取浏览器发送的所有Cookie，保存在一个数组中。
- `String getName()`：获取cookie的名称，此名称创建后不可更改。
- `String getValue()`：获取cookie当前的值。

**方法演示**

```
public class CookieText extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //向浏览器发送Cookie
        Cookie cookie = new Cookie("lastLoginTime", System.currentTimeMillis() + "");//传入值，新建一个Cookie对象
        cookie.setMaxAge(24 * 60 * 60);
        ;//设置Cookie的有效期
        resp.addCookie(cookie);//响应给客户端一个cookie

        //获取浏览器发送的Cookie
        Cookie[] cookies = req.getCookies();//获得保存Cookie的数组
        for (Cookie c : cookies) {
            String name = c.getName();//获得cookie中的key
            String value = c.getValue();//获得cookie中的vlaue
            resp.getWriter().println(name + ": " + value);//在浏览器页面打印
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

#### 注意事项

1. 一个Cookie只能保存一个信息
2. 一个web站点可以给浏览器发送多个cookie，最多存放20个cookie
3. Cookie大小限制为4kb
4. 浏览器Cookie上限为300个

### Session

#### 概述

当访问服务器否个网页的时候，会在服务器端的内存里开辟一块内存，这块内存就叫做session，而这个内存是跟浏览器关联在一起的。这个浏览器指的是浏览器窗口，或者是浏览器的子窗口，意思就是，只允许当前这个session对应的浏览器访问，就算是在同一个机器上新启的浏览器也是无法访问的。而另外一个浏览器也需要记录session的话，就会再启一个属于自己的session。

#### 常用方法

- `HttpSession getSession()`：返回一个与当前请求相关的Session对象，如果该请求没有Session对象，则创建一个。
- `void setAttribute(String name, Object value)`：存储键值对数据到Session对象中。
- `Object getAttribute(String name)`：获取此Session对象绑定的指定名称的对象。
- `String getId()`：返回一个字符串，其中包含分配给此Session对象的唯一标识符*。标识符由Servlet容器分配*，并且取决于实现。
- `void removeAttribute(String name)`：从此Session对象中删除与指定名称绑定的对象
- `void invalidate()`：使该Seesion无效，并取消绑定的任何对象

**方法演示**

```
public class SessionTest extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //解决中文乱码问题
        req.setCharacterEncoding("UTF-8");;
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=utf-8");

        //获取Session对象
        HttpSession session = req.getSession();
        //向Session对象中存入键值对数据
        session.setAttribute("name",new Human("Jason",21,true));

        //获取Session的ID
        String sessionId = session.getId();

        //判断Session是不是新创建的
        if (session.isNew()) {
            resp.getWriter().write("session心创建成功，ID为：" + sessionId);
        } else {
            resp.getWriter().write("session以及在服务器中存在了，ID为："+ sessionId);
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

# JSP

## 概述

## 语法

### JSP表达式

```
<%--JSP表达式
    作用：用来将程序的输出，输出到客户端
    <%= 变量或者表达式%>
--%>
<%= new java.util.Date()%>
```

### JSP脚本片段

```
<%--jsp脚本片段--%>
<%
int sum = 0;
for (int i = 1; i <=100 ; i++) {
    sum+=i;
}
out.println("<h1>Sum="+sum+"</h1>");
%>
```

### JSP声明

```
<%!
    static {
    System.out.println("Loading Servlet!");
}

private int globalVar = 0;

public void kuang(){
    System.out.println("进入了方法Kuang！");
}
%>
```