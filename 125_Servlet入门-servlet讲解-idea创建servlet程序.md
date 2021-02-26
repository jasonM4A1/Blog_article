---

title: Servlet入门(servlet讲解+idea创建servlet程序)
date: 2021-02-03 14:30:02
tags:
	- JavaWeb
	- Servlet
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203022101.png
---

# Servlet概述

1. JServlet就是sun公司开发动态web的一门技术，是运行在 Web 服务器或应用服务器上的程序，作为来自 Web 浏览器或其他 HTTP 客户端的请求和 HTTP 服务器上的数据库或应用程序之间的中间层。
2. Servlet简单点说，其实就是JavaEE的一个规范。我们学习Servlet，就是学习JavaEE的Servlet接口。
3. Servlet是JavaWeb的三大组件之一，它属于动态资源。三大组件分别是：Servlet 程序、Filter 过滤器、Listener 监听器。

# Servlet继承体系

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210209172702.png)

## Servlet接口

Servlet接口默认有两个实现类，这两个实现类同时也为父子类关系——GenericServlet(父)和HttpServlet(子)。

Servlet接口去除注释、注解等，源代码大致为：

```java
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

## GenericServlet抽象类

GenericServlet是位于javax.servlet包下的抽象类，并且实现自Servlet接口。

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220233512.png)



GenericServlet类中提供了一个ServletConfig成员对象，用于保存servlet的配置信息。在Tomcat服务器调用init()方法时，会将ServletConfig对象赋值给config成员变量。

GenericServlet类对Servlet接口中的init()、destroy()、getServletInfo()、getServletConfig()方法进行了实现，但是对service()方法没有进行实现，因为service方法需要开发人员自己编写业务逻辑代码。

除了Servlet接口中的五个方法，GenericServlet抽象类中还提供了getInitParameter()、getServletContext()、getServletName()等方法。

- `String getInitParameter(String name)`：用于获取servlet初始化参数。
- `ServletContext getServletContext()`：获取servlet的上下文对象。一个web工程中只有一个servlet上下文对象。
- `String getServletName()`：获取servlet的名称。即：在web.xml配置文件中对应servlet的标签的名称。

## HttpServlet抽象类

HttpServlet是位于`javax.servlet.http`包下的抽象类，继承自GenericServlet抽象类。它不仅拥有GenericServlet类中的所有方法，而且HttpServlet类对service方法进行了重载，并且还添加了对应的doGet()、doPost()等http请求方法。

我们进行Servlet业务开发，都是创建一个类然后继承`HttpServlet`类，然后重写它的`service()`方法既可。

# 实现Servlet三种方式

1. 实现`javax.servlet.Servlet`接口
2. 继承`javax.servlet.GenericServlet`类
3. 继承`javax.servlet.http.HttpServlet`类

开发中通常我们会去继承HttpServlet类来完成我们的Servlet，既采用第三种方式。

# Servlet编程快速上手

## Servlet编程初始方式

在前面我们已经在Idea中初步配置了Tomcat服务器，现在我们要在Idea使用Tomcat创建我们的第一个Servlet程序。

1. **导入Servlet的Jar包**

   关于如何在Idea中导入Jar包，我们之前的文章已经介绍过了，不会的可以点[这里](https://www.kuangstudy.com/bbs/1356635551163219969)，去学习一下（很简单）。

   不过我是使用Maven+Tomcat，所以我直接在Maven的`pom.xml`文件中导入Servlet的Jar包远程库地址即可（关于Maven使用，点[这里](https://www.kuangstudy.com/bbs/1356634744057495554)学习）。

2. 新建Java类，实现`Servlet`接口，同时重写`Servlet`的5个方法。

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203151604.png)

3. 在`init`、`service`、`destroy`方法中各自写一个打印语句，为了测试其执行顺序

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203152048.png)

4. 配置`web/WEB-INF`目录下的`web.xml`文件

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203150824.png)b

5. 启动Tocmat服务器

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203152141.png)

6. 打开浏览器输入追加输入你在`web.xml`中配置的虚拟路径，不报错既表明访问成功，你的第一个Servlet程序就完成了！

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203153051.png)

7. 多次刷新页面，然后返回Idea关闭Tomcat服务器。查看Idea控制台输出情况，发现方法的执行顺序为`init`--->`service`--->`destroy`。

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203153956.png)

Servlet编程的最初步方式，已经演示完毕了。下面我来演示，更贴近实际开发的Servlet编程方式。

## Servlet编程进阶方式

在实际的开发中，我们不会去通过实现`Servlet`接口来进行Servlet编程，而是通过去继承`Servlet`子抽象类`HttpServlet`来进行Servlet编程的。

1. 新建Java类，并继承HttpServlet抽象类，同时重写HttpServlet的两个方法`doGet`和`doPost`。

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203154854.png)

2. 配置web.xml，为项目配置虚拟路径映射

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203155744.png)

3. 在web目录下新建html文件，写一个文本框和提交按钮。

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203155137.png)

4. 在打开的浏览器地址栏追加输入刚才html文件名，然后点击提交按钮

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203160433.png)

5. 查看浏览器地址栏发现的变化

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203160601.png)

6. 返回Idea查看控制台输出情况

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203160718.png)

7. 修改`web`目录下`table.html`文件中`<form>`标签的属性为`method="GET"`。

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203160911.png)

8. 重启Tomcat服务器，然后重复步骤4、5、6。在第6步查看Idea控制输出情况时，发现输出结果变成了`执行POST请求操作`，表明这次执行的是`doPost()`。

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203161242.png)

我们在实际开发，就是将get请求的操作写到`doGet`方法中、post请求的操作写道`doPost`中。不过，我们可以使用Idea帮我们更加便捷的完成Servlet程序的编写。

## Servlet编程高级方式

1. 在包上右键，进行如图操作

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203161659.png)

2. 进行名字的填写

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203162454.png)

3. 查看生成的Java类与我们自己手动创建的Java有什么不同

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203162922.png)

4. 配置`web.xml`文件，为项目做虚拟映射

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203163207.png)

5. 修改`web`目录下的`table.html`文件，将`form`标签中的属性`action`的值改为Servlet2的地址。

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203163507.png)

6. 启动Tomcat服务器，在浏览器中追加输入Servlet2的虚拟地址，后续步骤与"Servlet编程进阶方式"一致。不再赘述。

   

# 虚拟映射的相关问题

1. 一个`servlet`可以映射多个`servlet-mapping`，访问时随便使用一个映射地址都可以。

   ![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201221005917.png)

2. 一个`Servlet`可以指定通用映射路径

   ![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201221010326.png)

3. 还可以配置默认请求路径，即服务器启动之初开启的页面

   ![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201221011607.png)

   > 指定了固有的映射路径优先级最高，如果找不到才会走默认的处理请求；

4. 为虚拟路径指定前缀或后缀。

   ![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201221012131.png)

# 使用注解来映射虚拟地址

Servlet3.0提供了注解(annotation)，使得不再需要在web.xml文件中进行Servlet的部署描述，简化开发流程。

1. 使用注解映射项目虚拟地址，需要在添加web框架时取消创建`web.xml`。除此之外，其他创建步骤和使用Idea部署Tomcat和创建JavaWeb项目一致。

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203165530.png)

2. 我们发现idea真的没有生成`web.xml`文件，我们通过上面讲过的"Servlet编程高级方式"，来创建一个Java类。

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203170056.png)

3. 使用注解配置虚拟地址映射

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203171019.png)

4. 直接启动Tomcat，进入浏览器追加输入你在注解中配置的虚拟地址。页面不报错，既访问成功！

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203171211.png)

5. 返回Idea，控制台上也输出了执行语句，表明使用注解配置虚拟路径成功了。

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203171301.png)

# 获取请求参数

+ 请求的table.html页面

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>table</title>
  </head>
  <body>
  <!--这里的 / 表示：http://ip:端口/ (与请求转发中的不同)-->
  <form action="/crawler/Parameter" method="get">
      用户名：<input type="text" name="username"><br>
      密码： <input type="password" name="password"><br>
      <b>爱好</b><br>
      足球：<input type="checkbox" name="hobby" value="football"><br>
      网球：<input type="checkbox" name="hobby" value="tennis"><br>
      篮球：<input type="checkbox" name="hobby" value="basketball"><br>
      <input type="submit" value="提交">
  </form>
  </body>
  </html>
  ```

+ Java源代码

  ```java
  package xyz.rtx3090;
  
  import javax.servlet.ServletException;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  import java.io.IOException;
  
  public class Parameter extends HttpServlet {
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          System.out.println("我是Parameter");
  
          //获取table.html页面中的请求参数
          String username = req.getParameter("username");
          String password = req.getParameter("password");
          String[] hobbies = req.getParameterValues("hobby");
  
          System.out.println("username: " + username);
          System.out.println("password: " + password);
          for (String hobby: hobbies
               ) {
              System.out.println("hobby: " + hobby);
          }
  
      }
  
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
  
      }
  }
  ```

# HttpServletRequest类

## 概述

每次只要有请求进入 Tomcat 服务器，Tomcat 服务器就会把请求过来的 HTTP 协议信息解析好封装到 Request 对象中。 然后传递到 service 方法（doGet 和 doPost）中给我们使用。我们可以通过 HttpServletRequest 对象，获取到所有请求的信息。

## 常见方法

+ `String getMethod()`：获取请求方式
+ `String getContextPath()`：获取虚拟目录
+ `String getServletPath()`：获取Servlet路径
+ `String getQueryString()`：获取get方法请求参数
+ `String getRequestURI`：获取请求URI
+ `StringBuffer getRequestURL() `：获取请求URL
+ `String getProtocol()`：获取协议及版本
+ `String getRemoteAddr()`：获取客户机的IP地址
+ `Enumeration<String> getHeaderNames()`：获取所有请求头名称
+ `String getHeader(String name)`：通过请求头的名称获取请求头的值
+ `String getParameter(String name)`：根据参数名称获取参数值
+ `String[] getParameterValues(String name)`：根据参数名称获取参数值的数组
+ `Enumeration<String> getParameterNames()`：获取所有请求的参数名称
+ `Map<String, String[]> getParameterMap()`：获取所有参数的map集合
+ `void setAttribute(String name, Object o)`：设置域数据
+ `Object getAttribute(String name)`：获取域数据
+ `RequestDispatcher getRequestDispatcher(String path)`：获取请求转发对象

## 演示

### 表单项

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>table</title>
</head>
<body>
<!--这里的 / 表示：http://ip:端口/ (与请求转发中的不同)-->
<form action="/crawler/" method="get">
    用户名：<input type="text" name="username"><br>
    密码： <input type="password" name="password"><br>
    <b>爱好：</b><br>
    <input type="checkbox" name="hobby" value="football"><br>足球
    <input type="checkbox" name="hobby" value="tennis"><br>网球
    <input type="checkbox" name="hobby" value="basketball"><br>篮球
    <input type="submit" value="提交">
</form>
</body>
</html>
```

### Java源代码

```java
package xyz.rtx3090;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Enumeration;
import java.util.Map;
import java.util.Set;

public class Request extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取请求方式
        String method = req.getMethod();
        System.out.println("method: " + method);

        //获取虚拟目录
        String contextPath = req.getContextPath();
        System.out.println("contextPath" + contextPath);

        //获取Servlet路径
        String servletPath = req.getServletPath();
        System.out.println("servletPath" + servletPath);

        //获取get方法请求参数
        String queryString = req.getQueryString();
        System.out.println("queryString: " + queryString);

        //获取请求URI
        String requestURI = req.getRequestURI();
        System.out.println("requestURI: " + requestURI);

        //获取请求URL
        StringBuffer requestURL = req.getRequestURL();
        System.out.println("requestRUL: " + requestURL);

        //获取协议版本
        String protocol = req.getProtocol();
        System.out.println("protocol: " + protocol);

        //获取客户机的IP地址
        String remoteAddr = req.getRemoteAddr();
        System.out.println("remoteAddr: " + remoteAddr);

        //获取所有请求头名称
        Enumeration<String> headerNames = req.getHeaderNames();
        System.out.println("headerNames: " + headerNames);

        //通过请求头的名称获取请求头的值
        while (headerNames.hasMoreElements()) {
            String headerName = headerNames.nextElement();
            String header = req.getHeader(headerName);
            System.out.println("header: " + header);
        }

        //根据参数名称获取参数值
        String username = req.getParameter("username");
        String password = req.getParameter("password");

        System.out.println("username: " + username);
        System.out.println("password: " + password);

        //根据参数名称获取参数值的数组
        String[] hobbies = req.getParameterValues("hobby");
        for (String hobby : hobbies
        ) {
            System.out.println("hobby: " + hobby);
        }

        //获取所有请求的参数名称
        Enumeration<String> parameterNames = req.getParameterNames();
        while (parameterNames.hasMoreElements()) {
            String element = parameterNames.nextElement();
            System.out.println("parameterName: " + element);
        }

        //获取所有参数的map集合
        Map<String, String[]> parameterMap = req.getParameterMap();
        Set<Map.Entry<String, String[]>> entries = parameterMap.entrySet();
        for (Map.Entry<String, String[]> s : entries
        ) {
            System.out.println("entries: " + s);
        }

        //设置域数据
        req.setAttribute("height", "180");
        req.setAttribute("weight", "160");

        //取出域数据
        Object height = req.getAttribute("height");
        Object weight = req.getAttribute("weight");

        System.out.println("height: " + height);
        System.out.println("weight: " + weight);

        //获取请求转发对象
        RequestDispatcher requestDispatcher = req.getRequestDispatcher("/FormD");
        //进行转发
        requestDispatcher.forward(req, resp);

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

    }
}
```

# ServletConfig类

## ServletConfig类概述

1. ServletConfig 类是 Servlet 程序的配置信息类。

2. Servlet 程序和 ServletConfig 对象都是由 Tomcat 负责创建，而我们只负责使用。 

3. Servlet 程序默认是第一次访问的时候创建，ServletConfig 是每个 Servlet 程序创建时，就创建一个对应的 ServletConfig 对象。

## ServletConfig类作用

1. 获取 Servlet 程序的别名 servlet-name 的值
2. 获取初始化参数 init-param
3. 获取 ServletContext 对象

# ServletContext类

## ServletContext类的概述

1. ServletContext 是一个接口，它表示 Servlet 上下文对象
2. 一个 web 工程，只有一个 ServletContext 对象实例
3. ServletContext 对象是一个域对象
4. ServletContext 是在 web 工程部署启动的时候创建，在 web 工程停止的时候销毁

> **什么是域对象?** 
>
> 域对象，是可以像 Map 一样存取数据的对象。 这里的域指的是存取数据的操作范围，整个 web 工程。

|        |     存数据     |     取数据     |     删除数据      |
| :----: | :------------: | :------------: | :---------------: |
|  Map   |     put()      |     get()      |     remove()      |
| 域对象 | setAttribute() | getAttribute() | removeAttribute() |

## ServletContext类的作用

1. 获取 web.xml 中配置的上下文参数 context-param
2. 像 Map 一样存取数据
3. 获取当前的工程路径，格式: /工程路径
4. 获取工程部署后在服务器硬盘上的绝对路径

## ServletContext类的方法

- `ServletContext getServletContext()`：获取本web程序的ServletContext对象（直接使用this调用既可）。
- `void setAttribute(String name, Object object)`：向ServletContext对象中存储数据，`String name`为键，`Object object`为存储的数据。
- `Object getAttribute(String name)`：传入字符串键名，取出存储在ServletContext对象中的数据。
- `String getInitParameter(String name)`：获取`xml`配置文件中定义的参数值。
- `RequestDispatcher getRequestDispatcher(String path)`：传入要转发到的虚拟路径，来获取一个请求转发对象。
- `void forward(ServletRequest request, ServletResponse response)`：由`RequestDispatcher`对象调用，启动转发。

# ServletConfig类和ServletContext类的作用演示

**web.xml文件内容**

```xml
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
         http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1"><!--将web版本4.0修改为3.1，注意上一行约束文件也要修改-->
    <!--上下文参数(所有Servlet程序共享)-->
    <context-param>
        <!--参数名-->
        <param-name>namespace</param-name>
        <!--参数值-->
        <param-value>henan</param-value>
    </context-param>
    
    <servlet>
        <servlet-name>Servlet04</servlet-name>
        <servlet-class>xyz.rtx3090.Servlet.Servlet04</servlet-class>
        <!--初始化参数-->
        <init-param>
            <!--参数名-->
            <param-name>username</param-name>
            <!--参数值-->
            <param-value>bernardo</param-value>
        </init-param>
        <!--初始化参数可以配置多个-->
        <init-param>
            <param-name>url</param-name>
            <param-value>jdbc:mysql://localhost:3306/test</param-value>
        </init-param>
    </servlet>
    
    <servlet-mapping>
        <servlet-name>Servlet04</servlet-name>
        <url-pattern>/Servlet04</url-pattern>
    </servlet-mapping>
</web-app>
```

**Java源代码**

```java
package xyz.rtx3090.Servlet;

import javax.servlet.ServletConfig;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class Servlet04 extends HttpServlet {
    public Servlet04() {
        System.out.println("我是构造函数!");//我是构造函数!
    }

    @Override
    public void init(ServletConfig config) throws ServletException {
        //继承HttpServlet的类重写init方法时，必须写下面的这行代码
        super.init(config);

        //1. 获取web.xml文件中servlet-name标签的内容
        String servletName = config.getServletName();
        System.out.println("web.xml文件中servlet-name标签的内容: " + servletName);
        //web.xml文件中servlet-name标签的内容: Servlet04

        //2. 获取web.xml文件中的init-param子标签param-value标签的内容
        String username = config.getInitParameter("username");
        System.out.println("init-param子标签param-value标签的内容: " + username);
        //init-param子标签param-value标签的内容: bernardo

        //3. 获取ServletContext对象
        ServletContext servletContext = config.getServletContext();

        //4. 获取context-param子标签param-value的值
        String namespace = servletContext.getInitParameter("namespace");
        System.out.println("context-param子标签param-value的值: " + namespace);
        //context-param子标签param-value的值: henan

        //5. 设置ServletContext的attribute值
        servletContext.setAttribute("country", "China");

        //6. 获取ServletContext的attribute值
        Object country = servletContext.getAttribute("country");
        System.out.println("ServletContext的attribute值: " + country);
        //ServletContext的attribute值: China

        //7. 获取当前工程路径
        String contextPath = servletContext.getContextPath();
        System.out.println("获取当前工程路径: " + contextPath);
        //获取当前工程路径: /Servlet

        //8. 获取工程部署后在服务器硬盘上的绝对路径
        String realPath = servletContext.getRealPath("/");
        System.out.println("工程部署后在服务器硬盘上的绝对路径: " + realPath);
        //工程部署后在服务器硬盘上的绝对路径: D:\JavaProject\Servlet\out\artifacts\Servlet_war_exploded\

        //9. 获取web项目中css文件夹里的图片路径
        String realPath1 = servletContext.getRealPath("/css/wallhaven-5wwvd7.jpg");
        System.out.println("web项目中css文件夹里的图片路径: " + realPath1);
        //web项目中css文件夹里的图片路径: D:\JavaProject\Servlet\out\artifacts\Servlet_war_exploded\css\wallhaven-5wwvd7.jpg
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("doGet方法...");//doGet方法...
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("doPost方法...");
    }
}
```

# 解决Post请求的中文乱码问题

```java
package xyz.rtx3090;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class doGetChinese extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取table.html页面中的请求参数
        String username = req.getParameter("username");
        String password = req.getParameter("password");

        //get请求不会乱码
        System.out.println("username: " + username);;
        System.out.println("password: " + password);;
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //设置请求字符集为UTF-8
        req.setCharacterEncoding("UTF-8");

        //获取table.html页面中的请求参数
        String username = req.getParameter("username");
        String password = req.getParameter("password");

        //如果不设置请求字符集，Post请求会乱码
        System.out.println("username: " + username);;
        System.out.println("password: " + password);;
    }
}
```



# 解决响应的中文乱码问题

## 方法一

```java
package xyz.rtx3090;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

public class Chinese01 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //首先设置服务器字符集为UTF-8
        resp.setCharacterEncoding("UTF-8");

        //在设置响应头，指定浏览器使用UTF-8解析
        resp.setHeader("Content-Type", "text/html; charset=UTF-8");

        PrintWriter writer = resp.getWriter();
        writer.write("我会中文乱码");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

    }
}
```

## 方法二

```java
package xyz.rtx3090;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

public class Chinese02 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //同时设置服务器和浏览器使用UTF-8字符集 (注意：一定要在获取流对象之前调用)
        resp.setContentType("text/html; charset=UTF-8");

        PrintWriter writer = resp.getWriter();
        writer.write("我也不会乱码哦～");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

    }
}
```

# / 符号表示不同的路径名

+ `/`如果被服务器解析，将会被解析为`http://ip:port/工程路径/`
  1. `<url-pattern>/servlet1</url-pattern>`
  2. `servletContext.getRealPath("/")`
  3. `request.getRequestDispatcher("/")`
+ `/`如果被浏览器解析，将会被解析为`http://ip:port/`
  1. `form`表单中的`action="/"`
  2. `response.sendRediect("/")`

# 两个输出流的说明

## 字节流

`getOutputStream()`：常用于下载（传递二进制数据）

```java
package xyz.rtx3090;

import javax.servlet.ServletException;
import javax.servlet.ServletOutputStream;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

public class Stream extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //同时设置服务器和浏览器使用UTF-8字符集（必须首行调用）
        resp.setContentType("text/html; charset=UTF-8");

        //OutputStream
        ServletOutputStream outputStream = resp.getOutputStream();
        byte[] one = "Output传递二进制文件，常用于下载文件".getBytes();
        outputStream.write(one);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

    }
}
```

## 字符流

`getWriter()`：常用于回传字符串

```java
package xyz.rtx3090;

import javax.servlet.ServletException;
import javax.servlet.ServletOutputStream;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

public class Stream extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //同时设置服务器和浏览器使用UTF-8字符集（必须首行调用）
        resp.setContentType("text/html; charset=UTF-8");

        //Writer
        PrintWriter writer = resp.getWriter();
        writer.write("Writer传递字符，常用于回传字符");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

    }
}
```

> 两个输出流只能同时使用一个，否则会报错

# base标签的作用

<base> 标签为页面上的所有链接规定默认地址或默认目标。

通常情况下，浏览器会从当前文档的 URL 中提取相应的元素来填写相对 URL 中的空白。

使用 <base> 标签可以改变这一点。浏览器随后将不再使用当前文档的 URL，而使用指定的基本 URL 来解析所有的相对 URL。这其中包括 <a>、<img>、<link>、<form> 标签中的 URL。

## 案例演示

+ `form.html页面`

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>one</title>
  </head>
  <body>
      <b>这是form页面</b> <br>
      <a href="a/b/c.html">跳转</a>
      <a href="http://localhost:8080/crawler/Base">转发</a>
  </body>
  </html>
  ```

  

+ `/a/b/c.html`页面

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>two</title>
    <!--如果不设置这个base标签，转发后在跳转就会失败-->
      <base href="http://localhost:8080/crawler/a/b/c.html">
  </head>
  <body>
  <b>这是C页面</b><br>
  <a href="../../form.html">跳转</a>
  </body>
  </html>
  ```

+ Java源代码

  ```java
  package xyz.rtx3090;
  
  import javax.servlet.ServletException;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  import java.io.IOException;
  
  public class Base extends HttpServlet {
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          System.out.println("进行转发到/a/b/c.html");
          req.getRequestDispatcher("/a/b/c.html").forward(req, resp);
      }
  
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
  
      }
  }
  ```

# 请求转发

## 概述

请求转发是指，服务器收到请求后，从一次资源跳转到另一个资源的操作叫请求转发。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210226134604.png)

## 特点

1. 浏览器地址栏不会发生变化
2. 转发前与转发后同为一次请求
3. 他们共享Request域中的数据
4. 可以转发到WEB-INF目录中
5. 不能访问工程以外的资源

## 代码演示

+ Servlet程序1

  ```java
  package xyz.rtx3090;
  
  import javax.servlet.RequestDispatcher;
  import javax.servlet.ServletException;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  import java.io.IOException;
  
  public class Servlet01 extends HttpServlet {
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          System.out.println("准备进行请求转发！我是Servlet01");
  
          // 传入虚拟映射的路径，来获取转发对象（ / 表示：http://ip:端口/项目地址/）
          RequestDispatcher requestDispatcher = req.getRequestDispatcher("/Servlet02");
  
          //开始请求转发
          requestDispatcher.forward(req, resp);
      }
  
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
  
      }
  }
  ```

+ Servlet程序2

  ```java
  package xyz.rtx3090;
  
  import javax.servlet.ServletException;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  import java.io.IOException;
  
  public class Servlet02 extends HttpServlet {
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          System.out.println("请求转发成功！我是Servlet02");
      }
  
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          System.out.println("请求转发成功！我是Servlet02");
      }
  }
  ```

# 重定向

## 概述

请求重定向，是指客户端给服务器发请求，然后服务器告诉客户端说。我给你一些地址。你去新地址访问。叫请求 重定向（因为之前的地址可能已经被废弃）

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210226184406.png)

## 特点

1. 浏览器地址栏会发生变化
2. 重定向前和重定向后，都为一次请求，共两次
3. 不共享Request域中数据
4. 不能访问WEB-INF下的资源
5. 可以访问工程外的资源

## 代码实现

### 方式一

```java
package xyz.rtx3090;

import javax.jws.WebService;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet(name="Redirect01", urlPatterns = "/Redirect01")
public class Redirect01 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //设置响应码为302
        resp.setStatus(302);

        //设置响应头Location为重定向的地址
        resp.setHeader("Location", "http://localhost:8080/crawler/Redirect02");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

    }
}
```

### 方式二

```java
package xyz.rtx3090;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet(name = "Redirect03", urlPatterns = "/Redirect03")
public class Redirect03 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //直接传入地址，进行重定向（这里的 / 表示：http://ip:port/）
        resp.sendRedirect("/crawler/Redirect04");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

    }
}
```

