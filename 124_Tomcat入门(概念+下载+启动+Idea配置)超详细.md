---
title: Tomcat入门(概念+下载+启动+Idea配置)超详细
date: 2021-02-03 03:50:40
tags:
	- JavaWeb
	- Tomcat
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203035146.jpg
---

# Tomcat概述

Tomcat是由Apache组织提供的一种 Web 服务器，提供对 jsp 和 Servlet 的支持。它是一种轻量级的 javaWeb 容器（服务 器），也是当前应用最广的 JavaWeb 服务器（免费）。

当然除了Tomcat还其他Web服务器，下面我就罗列一下常见的几个Web服务器：

1. `Jdoss`：一个遵从 JavaEE 规范的、开放源代码的、纯 Java 的 EJB 服务器，它支持所有的 JavaEE 规范（免费）。
2. `GloassFish`：Oracle 公司开发的一款 JavaWeb 服务器，是一款强健的商业服务器，达到产品级质量（应用很少）。
3. `Resin`：CAUCHO 公司的产品，是一个非常流行的服务器，对 servlet 和 JSP 提供了良好的支持， 性能也比较优良，resin 自身采用 JAVA 语言开发（收费，应用比较多）。
4. `WebLogic`：Oracle 公司的产品，是目前应用最广泛的 Web 服务器，支持 JavaEE 规范， 而且不断的完善以适应新的开发要求，适合大型项目（收费，用的不多，适合大公司）。

# 下载Tomcat

进入[Tomcat官网](http://tomcat.apache.org/)，选择Tomcat8，根据自己系统情况下载Tomcat版本。

> 企业中使用Tomcat7、8最多，当然你想要最新的也可以。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203040111.png)

解压下载好的Tomcat文件（路径随意），自此Tomcat的下载就算完成了，下面来简单的讲解一下Tomcat的各个目录的作用。

# Tomcat目录作用

打开你下载解压好的Tomcat文件夹，目录结构如下：

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203035951.png)

+ `bin`： 存放Tomcat 的可执行程序
+ `conf`：存放Tomcat的各种配置文件，如`xml`、`porperties`等。
+ `lib`：一些Tomcat需要的jar包
+ `logs`：Tomcat的日志文件
+ `temp`：用于存放Tomcat运行时产生的临时数据
+ `webapps`：里面可以存放需要部署的web工程文件
+ `work`：Tomcat运行工作时的目录，里面主要是一些Servlet的源代码等等。

你可以自己挨个打开这些文件，看看里面的目录信息。

# 启动Tomcat

启动手动Tomcat其实有两种方式。

## 方式一

进入Tomcat根目录下的bin目录，双击`startup.bat`文件，即可启动Tomcat服务器。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203042403.png)

### 验证Tomcat是否启动成功

在浏览器地址栏中输入`http:localhost:8080`，如果出现下面的界面代表Tomcat启动成功。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203051307.png)

## 方式二

打开cmd，使用命令cd到Tomcat根目录下的bin目录中，然后执行命令`catalina run`，这样Tomcat服务器也被启动了。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203042347.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203042445.png)

### 验证Tomcat是否启动成功

在浏览器地址栏中输入`http:localhost:8080`，如果出现下面的界面代表Tomcat启动成功。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203043527.png)

## 两种方法的差别

第一种方法，如果你的Java环境变量配置有问题，或者其他原因导致Tomcat启动失败。那个cmd窗口会一闪而过。你就无法直接看到失败原因的（当然你也可以去log文件去查看日志文件，但这样就麻烦了）。

但是第二种方法，在你Tomcat启动失败后，你可以直接看到cmd给你的提示，让你能第一时间知道是哪里出了问题。

# 启动Tomcat失败的原因

## cmd窗口一闪而过

这是因为没有正确配置JAVA_HOME环境变量，将jdk的根目录路径，配置到JAVA_HOME中即可（实在不会，可以百度）。

## 启动报错

可能是Tomcat默认的端口号8080被占用，我们可以在cmd中使用命令`netstat -ano`查看是哪一个进程占用了8080端口，然后将它关闭。

当然，我们也可以去修改Tomcat的端口号，避免发生端口冲突。下面我们说说怎么修改Tomcat端口号。

# 如何修改Tomcat端口号

进入Tomcat下的conf目录，打开使用文本编辑器`server.xml`文件。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203044612.png)

大概定位到69行，或者搜索`Connector`，找到如图所示的语句，将`port=8080`，改为`port=8081`（当然你也可以改为别的数字，但最好别与系统进程其他端口冲突了）。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203044731.png)

重新启动Tomcat，发现端口冲突问题已经解决。

## Connector讲解

- `port`：端口号
- `protocol`：HTTP协议版本（1.0或1.1）
- `connectionTimeout`：连接超时时限
- `redirectPort`：重定向端口

# 如何修改主机配置

还是进入Tomcat下的conf目录，打开使用文本编辑器`server.xml`文件。

大概定位到152行，或直接搜索`<Host`：

~~~xml
<Host name="localhost"  appBase="webapps"
      unpackWARs="true" autoDeploy="true">
~~~

- `name`：主机名
- `appBase`：网站文件存放位置
- `unpackWARS`：是否解压压缩包
- `autoDeploy`：是否自动部署

# 关闭Tomcat

## 方式一（推荐）

双击Tomcat下bin目录中的`shutdown.bat`文件（Linux系统是`shutdown.sh`），这样Tomcat就关闭了。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203043520.png)

## 方式二

直接关闭Tomcat启动后的cmd窗口，这样Tomcat也会被关闭。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203044541.png)

# 删除Tomcat

直接删除Tomcat目录即可。

# 手动部署web项目到Tomcat中

> 关于web项目：
>
> 你可能说自己没有什么web项目工程，其实你只需创建一个文件夹，然后里面创建一个HTML文件，文件中随便写点内容即可。

## 方式一

将你要部署的web项目文件夹，copy到Tomcat下的webapps目录中即可。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203045430.png)

在浏览器输入`你的web工程文件名/你要访问的资源名`来访问你部署好的web工程。

例如我的web工程名为MyWeb，要访问的资源名为index.html。那么我在浏览器中输入`http://localhost:8080/MyWeb/index.html`，即可访问成功！（别忘了启动Tomcat）

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203045828.png)



## 方式二

将你的web文件随意存在这一个位置，例如：`D:\MyWeb`。

进入Tomcat下的`conf/Catalina/localhost`文件夹，并创建`pro.xml`（名字可以随意，只要是xml文件）。

xml文件内容如下：

~~~xml
<!-- Context 工程上下文
path 浏览器上访问路径
docBase web工程目录本地目录
-->
<Context path="/pro" docBase="D:\MyWeb" />
~~~

启动Tomcat，在浏览器输入`http://localhost:8080/pro/index.html`，访问成功，证明web工程部署成功。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203050525.png)

> 注：其实我们在输入项目名后，不需要再输入index.html，因为浏览器后自动访问这个文件。

# 在Idea中配置、启动Tomcat

Idea2020.2不同与之前的版本，不需要创建JavaWeb工程，而是直接创建一个Java工程，Tomcat在后面配置。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203105508.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203105546.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203105608.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203105651.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203105720.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203105758.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203110016.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203110112.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203110210.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203110239.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203110312.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203110428.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203110632.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203111218.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203110749.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203111125.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203111030.png)

自此，我们就已经完成了在Idea初步配置和启动Tomcat了。

# Idea配置Tomcat项目的目录讲解

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203112038.png)

--------------------

自此关于Tomcat的入门讲解已经完成，撤！