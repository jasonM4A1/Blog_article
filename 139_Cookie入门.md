---
title: Cookie入门
date: 2021-03-03 16:06:27
tags:
	- Java
	- JavaWeb
	- Cookie
categories:	
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/wallhaven-83ow2y.jpg
---

# Cookie概述

Cookie翻译成中文是小饼干的意思。在HTTP中它表示服务器送给客户端浏览器的小饼干。其实Cookie就是一个键和一个值构成的，随着服务器端的响应发送给客户端浏览器。然后客户端浏览器会把Cookie保存起来，当下一次再访问服务器时把Cookie再发送给服务器。

Cookie是由服务器创建，然后通过响应发送给客户端的一个键值对。客户端会保存Cookie，并会标注出Cookie的来源（哪个服务器的Cookie）。当客户端向服务器发出请求时会把所有这个服务器Cookie包含在请求中发送给服务器，这样服务器就可以识别客户端了！

# Cookie规范

+ Cookie大小上限为4KB
+ 一个服务器最多在客户端浏览器上保存20个Cookie
+ 一个浏览器最多保存300个Cookie

> 上面的数据只是HTTP的Cookie规范，但是很多浏览器不遵守，例如每个Cookie的大小为8KB，最多可保存500个Cookie等！但也不会出现把你硬盘占满的可能！
>
> 注意，不同浏览器之间是不共享Cookie的。也就是说在你使用谷歌访问服务器时，服务器会把Cookie发给谷歌，然后由谷歌保存起来，当你在使用FireFox访问服务器时，不可能把谷歌保存的Cookie发送给服务器

# 如何创建Cookie

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210303163027.png)

简单的说，就是先创建一个Cookie对象，然后通知客户端添加Cookie对象

```java
package xyz.rtx3090.servlet;

import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class CookieServlet extends BaseServlet {
    protected void createCookie(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        //创建Cookie对象
        Cookie cookie = new Cookie("key01", "value01");
        //通知客户端添加Cookie对象
        resp.addCookie(cookie);

        //创建Cookie完成，在页面打印语句
        resp.getWriter().write("创建Cookie成功！");
    }
}
```

# 服务器如何获取Cookie

服务器获取客户端的 Cookie 只需要一行代码：`req.getCookies()`，就能得到一个Cookie数组。

由于我们会经常的去获取Cookie，所以最好的方法是写一个CookieUtil类，方便以后多次获取Cookie。

```java
package xyz.rtx3090.utils;

import javax.servlet.http.Cookie;

public class CookieUtil {
    public static Cookie findCookie(String name, Cookie[] cookies) {
        if (name == null || cookies == null || cookies.length == 0) {
            return null;
        }
        for (Cookie cookie: cookies
             ) {
            if (name.equals(cookie.getName())) {
                return cookie;
            }
        }
        return null;
    }
}
```

然后再创建一个类调用写好的工具类，即可

```java
package xyz.rtx3090.servlet;

import xyz.rtx3090.utils.CookieUtil;

import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class CookieServlet extends BaseServlet {
    protected void getCookie(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        //服务器获取Cookie数组
        Cookie[] cookies = req.getCookies();

        //遍历Cookie数组到页面上
        for (Cookie cookie : cookies
        ) {
            resp.getWriter().write("Cookie[" + cookie.getName() + " = " + cookie.getValue() + "]<br/>");
        }

        //调用写好的CookieUtil类中的findCookie()方法，来根据名称获取Cookie对象
        Cookie cookie01 = CookieUtil.findCookie("key01", cookies);

        //将查找到的Cookie打印到页面上
        resp.getWriter().write("Cookie[key01 = " + cookie01.getValue() + "]");
    }
}
```

# 如何修改Cookie的值

## 方式一

创建与要修改Cookie对象相同名字的新Cookie对象，并在创建时赋予新值，最后通过客户端添加创建好的Cookie即可

```java
package xyz.rtx3090.servlet;

import xyz.rtx3090.utils.CookieUtil;

import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class CookieServlet extends BaseServlet {
    protected void updateCookie(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        //方式一
        //创建与要修改Cookie对象相同名字的新Cookie对象，并在创建时赋予新值
        Cookie cookie = new Cookie("key01", "newValue01");
        //通知客户端添加Cookie
        resp.addCookie(cookie);

        //Cookie值更新完毕，打印在页面上
        Cookie[] cookies = req.getCookies();
        Cookie cookie02 = CookieUtil.findCookie("key01", cookies);
        resp.getWriter().write("Cookie[key01 = " + cookie02.getValue() + "]");
    }
}
```

## 方式二

先获取（查找）到需要修改的Cookie对象，然后调用setValue()方法赋予Cookie新值，最后通知客户端添加修改后的Cookie对象

```java
package xyz.rtx3090.servlet;

import xyz.rtx3090.utils.CookieUtil;

import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class CookieServlet extends BaseServlet {
    protected void updateCookie(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        //方式二
        //先查找需要修改的Cookie对象，然后调用setValue()方法修改Cookie的值，最后通知客户端添加修改后的Cookie对象
        Cookie[] cookies1 = req.getCookies();
        Cookie key02 = CookieUtil.findCookie("key02", cookies1);
        key02.setValue("newValue02");

        //Cookie值更新完毕，打印在页面上
        Cookie key03 = CookieUtil.findCookie("key02", cookies1);
        resp.getWriter().write("Cookie[key02 = " + key03.getValue() + "]<br/>");
    }
}
```

# Cookie生命控制

Cookie 的生命控制指的是如何管理 Cookie 什么时候被销毁（删除）。

我们一般通过调用`setMaxAge()`传入参数来控制Cookie的生命周期。

+ 参数为正数：表示Cookie会在指定的秒数后过期
+ 参数为负数：表示Cookie会在浏览器关闭时过期
+ 参数为零   ：表示Cookie会立即过期

```java
package xyz.rtx3090.servlet;

import xyz.rtx3090.utils.CookieUtil;

import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class CookieServlet extends BaseServlet {
    protected void defaultLife(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        Cookie cookie = new Cookie("defaultLife", "defaultLife");
        //设置cookie声明值为-1，即浏览器关闭时消失（默认值）
        cookie.setMaxAge(-1);
        resp.addCookie(cookie);

        resp.getWriter().write("设置Cookie生命值为-1");
    }

    protected void nowLife(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        Cookie cookie = CookieUtil.findCookie("key01", req.getCookies());
        if (cookie != null) {
            //0表示立即删除Cookie
            cookie.setMaxAge(0);;
            resp.addCookie(cookie);

            resp.getWriter().write("名为key01的Cookie会被立即删除了");
        }
    }

    protected void tenSecondsLife(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        Cookie cookie = CookieUtil.findCookie("key02", req.getCookies());
        if (cookie != null) {
            //10表示10秒后被删除Cookie
            cookie.setMaxAge(10);
            resp.addCookie(cookie);

            resp.getWriter().write("名为key02的Cookie会在10秒被删除");
        }
    }
}
```

# Cookie有效路径Path的设置

Cookie的path属性用于过滤哪些Cookie可以发送给服务器，哪些Cookie不发给服务器。

path属性是通过请求的地址来进行有效的过滤。

我们假设：`CookieA的path属性为：/cookie`，而`CookieB的path属性为：/cookie/jason`

现在我们在浏览器输入`http://localhost:8080/cookie/`，此时`CookieA`会被发送给服务器，而`CookieB`则不会被发送给服务器。

而我们又在浏览器输入`http://localhost:8080/cookie/jason`，此时`CookieA`不会被发送给服务器，而`CookieB`会给发送给服务器。

```java
package xyz.rtx3090.servlet;

import xyz.rtx3090.utils.CookieUtil;

import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class CookieServlet extends BaseServlet {
    protected void setPath(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        //创建一个新的Cookie对象
        Cookie cookie = new Cookie("path", "path");
        //为新创建的Cookie对象设置path路径（req.getContextPath方法是为了获取工程路径）
        cookie.setPath(req.getContextPath() + "/jason");
        //添加Cookie到客户端
        resp.addCookie(cookie);

        resp.getWriter().write("创建了一个Cookie对象，并为其设置了path路径");
    }
}
```

# 会话cookie和持久cookie的区别

如果不设置过期时间，则表示这个cookie生命周期为浏览器会话期间，只要关闭浏览器窗口，cookie就消失了。这种生命期为浏览会话期的cookie被称为会话cookie。会话cookie一般不保存在硬盘上而是保存在内存里。

如果设置了过期时间(setMaxAge(60*60*24))，浏览器就会把cookie保存到硬盘上，关闭后再次打开浏览器，这些cookie依然有效直到超过设定的过期时间。存储在硬盘上的cookie可以在不同的浏览器进程间共享，比如两个IE窗口。而对于保存在内存的cookie，不同的浏览器有不同的处理方式。(在IE下测试通过)

# 如何利用Cookie实现自动登录

当用户在某个网站注册后，就会收到一个惟一用户ID的cookie。客户后来重新连接时，这个用户ID会自动返回，服务器对它进行检查，确定它是否为注册用户且选择了自动登录，从而使用户务需给出明确的用户名和密码，就可以访问服务器上的资源。

# 如何根据用户的爱好定制站点

网站可以使用cookie记录用户的意愿。对于简单的设置，网站可以直接将页面的设置存储在cookie中完成定制。然而对于更复杂的定制，网站只需仅将一个惟一的标识符发送给用户，由服务器端的数据库存储每个标识符对应的页面设置

# 使用cookie属性的注意问题

属性是从服务器发送到浏览器的报头的一部分；但它们不属于由浏览器返回给服务器的报头。

因此除了名称和值之外，cookie属性只适用于从服务器输出到客户端的cookie；服务器端来自于浏览器的cookie并没有设置这些属性。因而不要期望通过request.getCookies得到的cookie中可以使用这个属性。这意味着，你不能仅仅通过设置cookie的最大时效，发出它，在随后的输入数组中查找适当的cookie,读取它的值，修改它并将它存Cookie，从而实现不断改变的cookie值。

# 案例——免输入用户名密码登录

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210304002657.png)

首先创建一个`LoginServlet`类

```java
package xyz.rtx3090.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class LoginServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //解决post中文乱码问题
        req.setCharacterEncoding("UTF-8");
        //解决响应中文乱码问题
        resp.setContentType("text/html; charset=UTF-8");

        //获取表单请求参数
        String username = req.getParameter("username");
        String password = req.getParameter("password");

        //指定唯一用户名 jason, 密码为 java（账号密码错误不给予登录）
        if ("jason".equals(username) && "java".equals(password)) {
            //登录成功

            //创建两个新的Cookie对象，用于保存用户名和密码
            Cookie cookie = new Cookie("username", username);
            Cookie cookie1 = new Cookie("password", password);

            //设置用户名Cookie生命周期为一周，密码Cookie生命周期为一天
            cookie.setMaxAge(60 * 60 * 24 * 7);
            cookie1.setMaxAge(60 * 60 * 24);

            //保存两个Cookie对象到客户端
            resp.addCookie(cookie);
            resp.addCookie(cookie1);

            resp.getWriter().write("登录成功！用户名密码Cookie已保存～");
        } else {
            //登录失败
            resp.getWriter().write("登录失败！用户名或密码错误");
        }
    }
}
```

然后创建一个`login.jsp`页面

```jsp
%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>login</title>
</head>
<body>
<form action="/cookie/loginServlet" method="post">
    <input type="text" name="username" value="${cookie.username.value}"/>
    <input type="password" name="password" value="${cookie.password.value}"/>
    <input type="submit" value="提交" />
</form>
</body>
</html>
```



