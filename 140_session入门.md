---
title: Session入门
date: 2021-03-04 01:11:18
tags:
	- Java
	- JavaWeb
	- Session
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/wallhaven-rddgwm.jpg
---

# Session概述

Session一般译为会话，是解决Http协议的无状态问题的方案，可以将一次会话中的数据存储在服务器端的内存中，保证在下一次的会话中可以使用。

在客户端浏览器第一次向服务器端发送请求时，服务器端会为这个客户端创建独有的Session，并具有唯一的Session ID，存储在服务器端的内存中。在客户端第二次访问服务器端时，会携带Session ID在请求中，服务器端会根据Session ID查找对应的Session信息，进行进一步地操作。

在JavaEE中提供了javax.servlet.http.HttpSession接口，通过该接口可以将共享的数据内容存储在HttpSession对象中，从而解决Http协议的无状态问题。

session机制是一种服务器端的机制，服务器使用一种类似于散列表的结构(也可能就是使用散列表)来保存息。

但程序需要为某个客户端的请求创建一个session的时候，服务器首先检查这个客户端的请求里是否包含了一个session标识－称为session id，如果已经包含一个session id则说明以前已经为此客户创建过session，服务器就按照session id把这个session检索出来使用(如果检索不到，可能会新建一个，这种情况可能出现在服务端已经删除了该用户对应的session对象，但用户人为地在请求的URL后面附加上一个JSESSION的参数)。如果客户请求不包含session id，则为此客户创建一个session并且同时生成一个与此session相关联的session id，这个session id将在本次响应中返回给客户端保存。

# Session与Cookie对比

## 特点对比

cookie是将数据保存在浏览器端，是一门浏览器端的技术。由于数据保存在浏览器端，所以可以被任意的查看，安全性较低，但是可以长时间存储数据。cookie善于存储安全性要求较低，但是存储时间较长的数据。

session是将数据保存在服务器端，是一门服务器端的技术，数据保存在服务器端相对安全，但是服务器无法保留大量session对象，所以不能够长时间存储数据。服务器善于存储安全性要求较高，但是存储时间较短的数据。

## cookie机制和session机制的区别

具体来说cookie机制采用的是在客户端保持状态的方案，而session机制采用的是在服务器端保持状态的方案。同时我们也看到，由于才服务器端保持状态的方案在客户端也需要保存一个标识，所以session机制可能需要借助于cookie机制来达到保存标识的目的，但实际上还有其他选择，比如说重写 URL和隐藏表单域。

# Session的创建和获取

其实创建和获取Session的API是一样的，都是`request.getSession()`。

+ 第一次调用，既是创建Session对象
+ 之后的每次调用，都是获取创建好的Session对象

那么我们怎么知道一次Session是不是新创建的呢？我们可以使用`isNew()`方法来判断。

+ `true`：表示这个Session对象是新创建的
+ `false`：表示这个Session对象之前创建好了

另外每个Session会话都会想我们一样，都有一个id号，这个id号是唯一的。我们可以通过调用`getId()`方法来，获取Session会话的id值。

```java
package xyz.rtx3090.servlet;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;

public class SessionServlet extends BaseServlet {

    protected void createOrGetSession(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        //创建和获取Session会话对象
        HttpSession session = req.getSession();
        //判断当前Session对象，是否为新创建的
        boolean aNew = session.isNew();
        //获取Session会话的唯一标示id
        String id = session.getId();

        resp.getWriter().write("这是Session是否为新创建：" + aNew + "<br/>");
        resp.getWriter().write("这个Session的唯一标示id为：" + id);
    }
}
```

# Session域数据的存取

前面我们说过，Session四大域对象之一，Session也可以存取键值对数据。

```java
package xyz.rtx3090.servlet;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;

public class SessionServlet extends BaseServlet {
    protected void setAttribute(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        //获取Session对象，并设置域数据
        req.getSession().setAttribute("one",1);

        resp.getWriter().write("已经设置好域对象数据!");
    }

    protected void getAttribute(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        //获取Session对象，并取得域数据
        Integer one = (Integer) req.getSession().getAttribute("one");

        resp.getWriter().write("获取到的名为one的域数据值为：" + one);
    }
}
```

# Session生命周期

Session的默认超时时长为30分钟，是在Tomcat服务器的web.xml配置文件中配置的。如果要修改你这个web工程的Session默认超时时间，我们可以在web目录下的web.xml中配置如下内容（是web目录下的web.xml，不是Tomcat服务器的web.xml）：

```xml
<session-config>
	<session-timeout>20</session-timeout>
</session-config>
```

如果你只想单独设置一个Session的超时时间，那么就需要用到`setMaxInactiveInterval()`方法了，参数为要设置的超时时间，单位为秒。

需要注意的是，当参数为负数时，表示这个Session永不超时（极少使用）。

这时你可能又会问，我想要Session对象立即超时，该怎么做呢？这时，我们就要用到`invalidate()`方法了，这个方法可以使调用的Session对象立即超时。

最后如果你想要查看Session现在的超时时间为多少，你可以使用`getMaxInactiveInterval()`方法。

```java
package xyz.rtx3090.servlet;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;

public class SessionServlet extends BaseServlet {
    protected void defaultLife(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        //获取Session默认生命周期
        int maxInactiveInterval = req.getSession().getMaxInactiveInterval();

        resp.getWriter().write("Session的默认生命周期为："+maxInactiveInterval);
    }

    protected void threeSecondsLife(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        //首先获取Session对象
        HttpSession session = req.getSession();

        //单独设置Session超时时间
        session.setMaxInactiveInterval(3);

        resp.getWriter().write("单独设置Session对象超时时间为：" + 3 + "秒");
    }

    protected void deleteNow(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        //首先获取Session对象
        HttpSession session = req.getSession();

        //设置Session立即超时
        session.invalidate();

        resp.getWriter().write("设置Session对象立即超时!");
    }
}
```

# 浏览器和 Session 之间关联的技术内幕

Session 技术，底层其实是基于 Cookie 技术来实现的。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210304203430.png)