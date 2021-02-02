---
title: JavaWeb-Request对象
date: 2021-02-02 01:42:34
tags:
	- JavaWeb
	- Java
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202014611.jpg
---

# 概述

1. request和response对象是由服务器创建的。我们来使用它们
2. request对象是来获取请求消息，response对象是来设置响应消息

# 继承结构

```
ServletRequest		--	接口
		|	继承
HttpServletRequest	-- 接口
		|	实现
org.apache.catalina.connector.RequestFacade 类(tomcat)
```

# 功能

## 获取请求行数据

### 方法

- 获取请求方式：`String getMethod()`
- 获取虚拟目录：`String getContextPath()`
- 获取Servlet路径：`String getServletPath()`
- 获取get方式请求参数：`String getQueryString()`
- 获取请求URI：`String getRequestURI()`或`StringBuffer getRequestURL()`
- 获取协议及版本：`String getProtocol()`
- 获取客户机的IP地址：`String getRemoteAddr()`

### 方法演示

```
package top.liboshuai.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/demo")
public class Demo extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取请求方式
        String method = req.getMethod();
        System.out.println("获取请求方式：" + method);

        //获取虚拟目录
        String contextPath = req.getContextPath();
        System.out.println("获取虚拟目录：" + contextPath);

        //获取Servlet路径
        String servletPath = req.getServletPath();
        System.out.println("获取Servlet路径：" + servletPath);

        //获取get方式请求参数
        String queryString = req.getQueryString();
        System.out.println("获取get方式请求参数：" + queryString);

        //获取请求uri
        String uri = req.getRequestURI();
        System.out.println("获取请求uri：" + uri);

        //获取请求url
        StringBuffer url = req.getRequestURL();
        System.out.println("获取请求url：" + url);

        //获取协议及版本
        String protocol = req.getProtocol();
        System.out.println("获取协议及版本：" + protocol);

        //获取客户机得ip地址
        String remoteAddr = req.getRemoteAddr();
        System.out.println("获取客户机得ip地址：" + remoteAddr);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("post...");
    }
}
```

### 演示结果

```
获取请求方式：GET
获取虚拟目录：/web
获取Servlet路径：/demo
获取get方式请求参数：null
获取请求uri：/web/demo
获取请求url：http://localhost:8080/web/demo
获取协议及版本：HTTP/1.1
获取客户机得ip地址：0:0:0:0:0:0:0:1
```

## 获取请求头数据

### 方法

- 获取所有的请求头名称：`Enumeration<String> getHeaderNames()`
- 通过请求头的名称获取请求头的值：`String getHeader(String name)`

### 方法演示

```
package top.liboshuai.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Enumeration;

@WebServlet("/demo")
public class Demo extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取所有的请求头名称的集合
        Enumeration<String> headerNames = req.getHeaderNames();

        while (headerNames.hasMoreElements()) {
            //获取请求头名称
            String headerName = headerNames.nextElement();
            //获取请求头值
            String header = req.getHeader(headerName);
            System.out.println(headerName + ": " + header);
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("post...");
    }
}
```

### 方法结果

```
host: localhost:8080
user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:83.0) Gecko/20100101 Firefox/83.0
accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
accept-language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
accept-encoding: gzip, deflate
connection: keep-alive
cookie: JSESSIONID=6D0AA46142613BD9C31F185F1D6EB9E1
upgrade-insecure-requests: 1
```

## 获取请求体数据

只有POST请求方式，才有请求体，在请求体中封装了POST请求的请求参数

1. 获取流对象
   - 获取字符输入流，只能操作字符数据：`BufferedReader getReader()`
   - 获取字节输入流，可以操作所有类型数据在文件上传知识点后讲解：`ServletInputStream getInputStream()`
2. 再从流对象中拿数据

## 其他功能

### 获取请求参数通用方式

不论get还是post请求方式都可以使用下列方法来获取请求参数。

- `String getParameter(String name)`：根据参数名称获取参数值
- `String[] getParameterValues(String name)`：根据参数名称获取参数值的数组
- `Enumeration<String> getParameterNames()`：获取所有请求的参数名称
- `Map<String,String[]> getParameterMap()`：获取所有参数的map集合