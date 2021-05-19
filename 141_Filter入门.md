---
title: Filter入门
date: 2021-03-04 21:05:20
tags:
	- Java
	- JavaWeb
	- Filter
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/wallhaven-48zyyk.png
---

# Filter概述

Filter称之为过滤器，是servlet中最实用的技术，web开发人员通过Filter技术，对web服务器所管理的资源（jsp,servlet,静态图片或者静态html文件）进行拦截，从而实现一些特殊的功能。简单说就是过滤从服务端向服务器发送请求的。

+ Filter过滤器它是JavaWeb的三大组件之一，三大组件分别是：Servlet程序、Listener监听器、Filter过滤器。
+ Filter过滤器它是JavaEE规范，也就是一个接口。
+ Filter过滤器作用是：拦截请求，过滤响应。

# 一个简单的Filter程序

使用FIlter过滤器进行请求过滤的步骤：

+ 编写一个类去实现Filter接口
+ 实现过滤方法doFilter()
+ 在web.xml中配置Filter拦截路径

首先创建一个继承Filter接口的Java类

```java
package xyz.rtx3090.filter;

import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpSession;
import java.io.IOException;

public class AdminFilter implements Filter {
    public void init(FilterConfig filterConfig) throws ServletException {

    }
  
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("Filter的doFilter方法");

        HttpServletRequest request = (HttpServletRequest) servletRequest;
        HttpSession session = request.getSession();
        Object user = session.getAttribute("user");

        if (user == null) {
            request.getRequestDispatcher("/login.jsp").forward(servletRequest, servletResponse);
            return;
        } else {
            //让程序继续往下访问用户的目标资源
            filterChain.doFilter(servletRequest, servletResponse);
        }
    }

    public void destroy() {
        System.out.println("Filter的destroy方法");
    }
}
```

在web.xml文件中配置filter相关路径

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!--filter标签用于配置一个Filter过滤器-->
    <filter>
        <!--给filter起别名-->
        <filter-name>AdminFilter</filter-name>
        <!--配置filter的全类名-->
        <filter-class>xyz.rtx3090.filter.AdminFilter</filter-class>      
    </filter>
    <!--配置filter过滤器的拦截路径-->
    <filter-mapping>
        <!--对应上面的别名-->
        <filter-name>AdminFilter</filter-name>
        <!--配置需要拦截的web目录-->
        <!--/映射到web下-->
        <url-pattern>/admin/*</url-pattern>
    </filter-mapping>
</web-app>
```

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210304215038.png)

# 生命周期

Filter共有四个方法：`构造器方法`、`init初始化方法`、`doFilter过滤方法`、`destroy销毁方法`。

其中`构造器方法`、`init初始化方法`在web工程启动的时候就回被执行。

而`doFilter过滤方法`，在每次进行过滤拦截的时候，都会被执行一次。

最后在web工程停止运行的时候，`destroy销毁方法`才会被执行。

# FilterConfig类

FilterConfig类见名知义，它是 Filter 过滤器的配置文件类。

Tomcat 每次创建 Filter 的时候，也会同时创建一个 FilterConfig 类，这里包含了 Filter 配置文件的配置信息。

FilterConfig类的作用是获取filter过滤器的配置内容

+ 获取Filter的名称，即标签filter-name的内容。
+ 获取在Filter中配置的init-param初始化参数。
+ 获取ServletContext对象

首先我们需要在web.xml文件中配置初始化参数

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!--filter标签用于配置一个Filter过滤器-->
    <filter>
        <!--给filter起别名-->
        <filter-name>AdminFilter</filter-name>
        <!--配置filter的全类名-->
        <filter-class>xyz.rtx3090.filter.AdminFilter</filter-class>
        <!--配置初始化参数(可以配置多个)-->
        <init-param>
            <param-name>username</param-name>
            <param-value>root</param-value>
        </init-param>
        <init-param>
            <param-name>url</param-name>
            <param-value>jdbc:mysql://localhost:3306/book</param-value>
        </init-param>
    </filter>
    <!--配置filter过滤器的拦截路径-->
    <filter-mapping>
        <!--对应上面的别名-->
        <filter-name>AdminFilter</filter-name>
        <!--配置需要拦截的web目录-->
        <!--/映射到web下-->
        <url-pattern>/admin/*</url-pattern>
    </filter-mapping>
</web-app>
```

然后写一个继承Filter接口的Java类，来演示FilterConfig的三种作用

```java
package xyz.rtx3090.filter;

import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpSession;
import java.io.IOException;

public class AdminFilter implements Filter {
    public AdminFilter() {
        System.out.println("Filter的构造方法");
    }

    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("Filter的初始化方法");

        //获取filter-name的内容
        String filterName = filterConfig.getFilterName();
        System.out.println("filterName的内容：" + filterName);

        //Filter中的init-param初始化参数的内容
        String username = filterConfig.getInitParameter("username");
        String url = filterConfig.getInitParameter("url");
        System.out.println("username: " + username);
        System.out.println("url: " + url);

        //获取ServletContext对象
        ServletContext servletContext = filterConfig.getServletContext();
        System.out.println("servletContext对象：" + servletContext);
    }

    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("Filter的doFilter方法");
    }

    public void destroy() {
        System.out.println("Filter的destroy方法");
    }
}
```

# FilterChain过滤器链

FilterChain直译为过滤器链，即是多个过滤器连接在一起工作

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210305062932.png)

我们首先简述一下FilterChain.doFilter()方法的执行规则

+ 执行下一个Filter过滤器（如果有下一个Filter过滤器存在）
+ 执行目标资源（所有的Filter过滤器都已经执行完毕的情况下）
+ 多个Filter过滤器的执行顺序，是由他们在web.xml配置的前后顺序决定的

最后我们在说下多个Filter过滤器的执行特点：

+ 所有filter和目标资源默认都执行在同一个线程中
+ 多个Filter共同执行的时候，它们都适用同一个Request对象

# Filter拦截路径

## 精确匹配

`<url-pattern>/target.jsp</url-pattern>`

表示只匹配`http://ip/port/工程路径/target.jsp`这个文件

> 其中/表示：http://ip:port/工程路径/

## 目录匹配

`<url-pattern>/admin/*</url-pattern>`

表示匹配`http://ip:port/工程路径/admin/`下的所有子目录和子文件。

## 后缀匹配

`<url-pattern>*.html</url-pattern>`

表示匹配所有`http://ip:port/工程路径/`下的.html结尾的文件。

> 最后需要注意的一点：Filter 过滤器它只关心请求的地址是否匹配，不关心请求的资源是否存在！！！



