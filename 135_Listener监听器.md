---
title: Listener监听器
date: 2021-02-28 15:57:31
tags:
	- Java
	- JavaWeb
	- Listener
	- 监听器
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/wallhaven-lqpv1l.jpg
---

# Listener监听器概述

+ Listener监听器是JavaWeb三大组件之一（三大组件为：Servlet程序、Filter过滤器、Listener监听器）
+ Listener监听器是JavaEE的一个规范，也就是接口
+ Listener监听器是用来监听某种事物的变化，然后通过回调函数，反馈给客户端程序去做一些相应的处理

# 接口ServletContextListener

## 概述

现在很多关于Listener监听器的接口，我们都不再使用了，可能也就仅剩ServletContextListener接口还在喘息。

ServletContextListener可以监听ServletContext对象的创建和销毁。

ServletContext 对象在 web 工程启动的时候创建，在 web 工程停止的时候销毁。

监听到创建和销毁之后都会分别调用 ServletContextListener 监听器的方法反馈。

## 两个方法

+ `contextInitialized()`：在ServletContext对象创建之后被调用，做初始化
+ `contextDestroyed()`：在ServletContext对象销毁之后调用

## 实现

如何使用 ServletContextListener 监听器监听 ServletContext 对象。

1. 编写一个类去实现 ServletContextListener
2. 实现其两个回调方法
3. 在web.xml中配置监听器

监听器实现类

```java
package xyz.rtx3090.servlet;

import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;

public class MyServletContextListenerImpl implements ServletContextListener {
    public void contextInitialized(ServletContextEvent sce) {
        //servletContext对象创建之初做一些相应处理
        System.out.println("ServletContext对象被创建了");
    }

    public void contextDestroyed(ServletContextEvent sce) {
        //servletContext对象销毁之后做一些相应处理
        System.out.println("ServletContext对象被销毁了");
    }
}
```

web.xml配置文件

```xml
    <!--配置监听器-->
    <listener>
        <listener-class>xyz.rtx3090.servlet.MyServletContextListenerImpl</listener-class>
    </listener>
```

