---
title: JSP阶段一之JSP基础
date: 2021-02-27 21:36:05
tags:
	- JavaWeb
	- JSP
	- Java
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/wallhaven-456e78.jpg
---

# JSP概述



jsp全称`java server pages`，即 java服务器页面。

jsp其主要作用为代替Servlet程序回传html页面数据。

> 如果不使用jsp，手动回传html页面数据一件非常繁琐的事情，开发成本和维护成本都很高。

# 一个简单的JSP页面

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>JSP代替回传页面</title>
</head>
<body>
  
<h1>这是JSP页面数据！</h1>
<%
    int i = 12 / 1;
%>
  
</body>
</html>
```

浏览器访问jps文件与访问普通的html文件一样，都是`http://ip:port/工程名/a.jsp`。

# JSP的本质

首先抛出答案：**jsp 页面本质上是一个 Servlet 程序**！

当我们第一次访问 jsp 页面的时候。Tomcat 服务器会帮我们把 jsp 页面翻译成为一个 java 源文件。并且对它进行编译成 为.class 字节码程序。我们打开 java 源文件不难发现其里面的内容是：

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210227215116.png)

我们跟踪原代码发现，HttpJspBase 类。它直接地继承了 HttpServlet 类。也就是说。jsp 翻译出来的 java 类，它间接了继 承了 HttpServlet 类。也就是说，翻译出来的是一个 Servlet 程序。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210227215145.png)

大家也可以去观察翻译出来的 Servlet 程序的源代码，不难发现。其底层实现，也是通过输出流。把 html 页面数据回传 给客户端。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210227215244.png)

# JSP三种语法

## JSP头部的page指令

jsp 的 page 指令可以修改 jsp 页面中一些重要的属性，或者行为。

+ `language`属性：指定jsp 翻译后是什么语言文件（暂时只支持 java）。
+ `contentType`属性：指定jsp 返回的数据类型是什么（即为 response.setContentType("")参数值）
+ `pageEncoding`属性：指定当前 jsp 页面文件本身的字符集
+ `import`属性：用于导包、导类
+ `autoFlush`属性：设置当 out 输出流缓冲区满了之后，是否自动刷新冲级区。默认值是 true
+ `buffer`属性：设置 out 缓冲区的大小。默认是 8kb
+ `errorPage`属性：设置当 jsp 页面运行时出错，自动跳转去的错误页面路径
+ `isErrorPage`属性：设置当前 jsp 页面是否是错误信息页面。默认是 false。如果是 true 可以 获取异常信息
+ `session`属性：设置访问当前 jsp 页面，是否会创建 HttpSession 对象。默认是 true
+ `extends`属性：设置 jsp 翻译出来的 java 类默认继承谁

### 代码实例

```jsp
<%@ page import="java.util.Map" %>
<%@ page contentType="text/html;charset=UTF-8"
         pageEncoding="UTF-8"
         autoFlush="true"
         buffer="8kb"
         errorPage="/404NOTFOUND.jsp"
         isErrorPage="false"
         session="true"
         extends="apple.applescript.AppleScriptEngine"
         language="java" %>
<html>
<head>
    <title>JSP代替回传页面</title>
</head>
<body>
  
<h1>这是JSP页面数据！</h1>
  
</body>
</html>
```

# JSP脚本

## 声明脚本（极少使用）

### 格式

`<%! 声明Java代码 %>`

### 作用

用来指定jsp 翻译出来的 java 类定义属性和方法甚至是静态代码块、内部类等。

+ 声明类属性
+ 声明static静态代码块
+ 声明类方法
+ 声明内部类

### 演示

```jsp
<%@ page import="java.util.Map" %>
<%@ page import="java.util.HashMap" %>
<html>
<head>
    <title>one</title>
</head>
<body>
<%--1. 声明类属性--%>
<%!
    private Integer id;
    private String name;
    private static Map<String, Object> map;
%>

<%--2. 声明static静态代码块--%>
<%!
    static {
        map = new HashMap<>();
        map.put("one","value01");
        map.put("two","value02");
        map.put("three","value03");
    }
%>

<%--3. 声明类方法--%>
<%!
    public int abc(int a, int b) {
        return a + b;
    }
%>

<%--4. 声明内部类--%>
<%!
    public static class A {
        private Integer id = 12;
        public void HelloWorld() {
            System.out.println("Hello World!");
        }
    }
%>
</body>
</html>
```

### jsp代码 到照翻译后的 Java代码

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210228103616.png)

## 表达式脚本（常用）

### 格式

`<%=表达式%>`

### 作用

往JSP页面上输出数据。

### 特点

+ 所有的表达式脚本都会被翻译到`_jspService()`方法中
+ 表达式脚本都会被翻译到`out.print()`方法中从而输出到页面上
+ 由于表达式脚本翻译的内容都在`_jspService()`方法中，所以`_jspService()`方法中的对象都可以直接使用
+ 表达式脚本中的表达式不能以分号结束

### 演示

```jsp
<%@ page import="java.util.Map" %>
<%@ page import="java.util.HashMap" %>
<html>
<head>
    <title>two</title>
</head>
<body>
<%--提前声明一个map集合对象--%>
<%
    Map<String, Integer> map = new HashMap<>();
    map.put("one",1);
    map.put("two",2);
    map.put("three",3);
%>

<%--输出整型--%>
<%=12%><br>

<%--输出浮点型--%>
<%=3.1415726%>

<%--输出字符串--%>
<%="我是眼缘！"%>

<%--输出对象--%>
<%=map%>

<%=request.getParameter("username")%>
</body>
</html>
```

### Jsp代码 到照翻译后的 Java代码

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210228103721.png)

## 代码脚本

### 格式

`<% Java语句 %>`

### 作用

用来在Jsp页面中，编写Java代码来实现我们所需的功能。

### 特点

+ 代码脚本翻译之后都在`_jspService()`方法中
+ 代码脚本由于翻译到`_jspService()`方法中，所以在`_jspService()`方法中的现有对象都可以直接使用
+ 多个代码脚本可以组合成为一个完整的Java语句
+ 代码脚本还可以和表达式脚本一起组合使用

### 演示

```jsp
<%@ page import="java.io.PrintWriter" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>three</title>
</head>
<body>
<%
    // 解决中文乱码问题
    response.setContentType("text/html; charset=UTF-8");
    //获取Writer输出流
    PrintWriter writer = response.getWriter();
%>

<%--1. if语句--%>
<%
    int i = 10;
    if (i == 10) {
        writer.write("i原来等于10啊！\n");
    } else {
        writer.write("i原来不等于10啊！\n");
    }
%>

<%--2. for循环语句--%>
<%
    for (int x = 0; x < 10; x ++) {
%>
<%
        writer.write("你还好吗\n");
    }
%>

<%--3. 翻译后 java 文件中_jspService 方法内的代码都可以写--%>
<%
    String username = request.getParameter("username");
    writer.write("用户名的请求参数为：" + username + "\n");
%>
</body>
</html>
```

### Jsp代码 到照翻译后的 Java代码

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210228110939.png)

# JSP注释

## HTML注释

`<!--这是HTML注释-->`

html 注释会被翻译到 java 源代码中。在_jspService 方法里，以 out.writer 输出到客户端。

## Java注释

`//单行注释`

`/* 多行注释 */`

java 注释会被翻译到 java 源代码中。

## JSP注释

`<%--这是JSP注释--%>`

JSP注释可以注掉JSP页面中所有代码

# JSP九大内置对象

jsp 中的内置对象，是指 Tomcat 在翻译 jsp 页面成为 Servlet 源代码后，内部提供的九大对象，叫内置对象。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210228111440.png)

1. `request`：请求对象
2. `response`：响应对象
3. `pageContext`：JSP的上下文对象
4. `session`：回话对象
5. `application`：ServletContext对象
6. `config`：ServletConfig对象
7. `out`：JSP输出流对象
8. `page`：指向当前JSP的对象
9. `exception`：异常对象

# JSP四大域对象

## 概述

1. `pageContext`
   + 对应Java中的`PageContextImpl`类
   + 仅在当前JSP页面范围内有效
2. `request`
   + 对应Java中的`HttpServletRequest`类
   + 仅在一次请求内有效
3. `session`
   + 对应Java中的`HttpSession`类
   + 一个会话范围内有效（打开浏览器访问服务器，直到关闭浏览器）
4. `application`
   + 对应Java中的`ServletContext`类
   + 整个web工程范围内都有效（只要web工程不停止，数据都在）

## 作用

四大域对象可以像 Map 一样存取数据的对象。

## 范围

`pageContext < request < session < application`

## 优先级

`application < session < request < pageContext`

## 演示

```jsp
<%@ page import="java.io.PrintWriter" %>
<html>
<head>
    <title>four</title>
</head>
<body>
<%
    //分别往四个域中保存数据
    pageContext.setAttribute("one",1);
    request.setAttribute("two",2);
    session.setAttribute("three",3);
    application.setAttribute("four",4);
%>

<%
    //取出四个域中保存的数据
    Object one = pageContext.getAttribute("one");
    Object two = request.getAttribute("two");
    Object three = session.getAttribute("three");
    Object four = application.getAttribute("four");
%>
</body>
</html>
```

# JSP输出流

## out输出流和writer输出流

out输出流和writer输出流虽然都会将数据输出到页面上，但是输出的顺序却不同。

+ writer输出流的内容会被最优先输出到页面上。
+ 而out输出流的内容只有在所有代码结束后，才会被通过`out.flush()`的方法刷入页面上

## write()方法和print()方法

解决了out输出流和writer输出流的问题，我们又遇到了`write()`方法和`print()`方法的问题。

+ `writer()`只适合输出字符串
+ `print()`输出任意数据都没有问题（在输出字符串时，会自动转成write()方法）

## 结论

**我们在 jsp 页面中统一使用 out.print() 来进行输出，避免页面混乱和乱码问题。**

# JSP常用标签

## JSP静态包含标签

### 格式

`<%@ include file="引用的JSP页面路径" %>`

> 地址中第一个斜杠 / 表示为 http://ip:port/工程路径/， 映射到代码的 web 目录

### 作用

在一个JSP页面中，引用另一个JSP页面。

### 特点

+ 静态包含不会翻译被包含的 jsp 页面
+ 静态包含其实是把被包含的 jsp 页面的代码拷贝到包含的位置执行输出

### 演示

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>five</title>
</head>
<body>
  
<h2>这是本体页面</h2>
<%@ include file="six.jsp"%>
  
</body>
</html>
```

## JSP动态包含标签

### 格式

`<jsp:include page="引用的jsp页面路径"> </jsp:include>`

### 作用

动态包含也可以像静态包含一样。把被包含的内容执行输出到包含位置

### 特点

+ 动态包含会把包含的 jsp 页面也翻译成为 java 代码

+ 动态包含底层代码使用如下代码去调用被包含的 jsp 页面执行输出

  `JspRuntimeLibrary.include(request, response, "引用的jsp文件路径", out, false);`

+ 动态包含，还可以传递参数

### 演示

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>seven</title>
</head>
<body>
  
<h2>这是本体页面seven jsp</h2>
<jsp:include page="eight.jsp">
    <jsp:param name="username" value="Jason"/>
    <jsp:param name="password" value="jason_password"/>
</jsp:include>
  
</body>
</html>
```

## JSP转发标签

### 格式

`<jsp:forward page="请求转发的路径"> </jsp:forward>`

### 作用

传入指定路径，实现请求转发的功能

### 演示

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>nine</title>
</head>
<body>
  
<h2>这是本体页面nine</h2>
<jsp:forward page="ten.jsp"></jsp:forward>
  
</body>
</html>
```



> 其实最后本有两个小练习的，但是JSP写的东西太乱了，可读性太差了，不能忍。大伙也就别练了，反正也几乎用不到。