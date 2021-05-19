---
title: JSP阶段二之EL表达式
date: 2021-02-28 18:21:09
tags:
	- Java
	- EL
	- JSP
	- JavaWeb
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/wallhaven-e7o9yo.jpg
---

# EL表达式概述

EL表达式全称为：Expression Language，即表达式语言。

EL表达式作用为：主要是代替 jsp 页面中的表达式脚本在 jsp 页面中进行数据的输出，相比原生的jsp可读性更高，更简洁。

# 一个简单的EL表达式

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>eleven</title>
</head>
<body>

<%
    request.setAttribute("key","value");
%>

<b>使用原生JSP中的表达式脚本输出key值到页面：</b>
<%=
   request.getAttribute("key")
%><br/>

<b>使用EL表达式输出key值到页面：</b>
${key}

</body>
</html>
```

> EL 表达式在输出 null 值的时候，输出的是空串。jsp 表达式脚本输出 null 值的时候，输出的是值为 null 的字符串。

# EL表达式与域对象

由于EL表达式是在jsp页面中输出数据，其主要输出的数据是来自域对象中的，所以我们要探究一下EL表达式会按照何种顺序拿取域对象中的数据。

好吧，直接抛结论：**EL表达式会按照四个域对象的从小到大拿取数据**！

即先从范围最小的pageContext中拿取数据。数据不存在，再去范围更大的request中拿取数据。数据不存在，再去范围更大的session中拿取数据。数据不存在，最后去范围最大的servletContext中拿取数据。（前提当时条件满足，范围特定的域对象要求）

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>twelve</title>
</head>
<body>

<%
    //往四个域中都保存了相同的 key 的数据
    pageContext.setAttribute("key","pageContext");
    request.setAttribute("key","request");
    session.setAttribute("key","session");
    application.setAttribute("key","servletContext");
%>

<%--从满足条件的最小域对象中取数据--%>
${key}

</body>
</html>
```

# EL表达式输出各种数据

首先我们创建一个Person类，以便下面的EL表达式代码测试。

```java
package xyz.rtx3090.servlet;

import javax.servlet.http.HttpServlet;
import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.Objects;

public class Person {
    private String name;
    private String[] phones;
    private List<String> list;
    private Map<String,Object> map;

		//有参、无参构造函数
  
  	//setter and getter
  
    //toString
    
    //equals and hashCode
}
```

接下来就是我们进行EL表达式输出各种数据的测试代码

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>thirteen</title>
</head>
<body>

<%
    Person person = new Person();

    person.setName("Jason");

    person.setPhones(new String[]{"iphone", "mi", "huawei"});

    List<String> strings = new ArrayList<>();
    strings.add("beijing");
    strings.add("shanghai");
    strings.add("zhengzhou");
    person.setList(strings);

    Map<String, Object> hashMap = new HashMap<>();
    hashMap.put("key01","value01");
    hashMap.put("key02","value02");
    hashMap.put("key03","value03");
    person.setMap(hashMap);

    //将person对象存放在pageContext中，方便调用
    pageContext.setAttribute("p",person);
%>

输出Person：${p}<br/>
输出Person的name属性：${p.name}<br/>
输出Person的phones属性：${p.phones}<br/>
输出Person的list集合：${p.list}<br/>
输出Person list集合的个别元素：${p.list[1]}
输出Person的map集合：${p.map}<br/>
输出Person map集合的个别元素：${p.map[1]}

</body>
</html>
```

# EL表达式的运算符

## 关系运算符

| 关系元算符 |   说明   |          范例          | 结果  |
| :--------: | :------: | :--------------------: | :---: |
|  == 或 eq  |   等于   | ${5 == 5}或 ${5 eq 5}  | true  |
|  != 或 ne  |  不等于  | ${5 != 5} 或 ${5 ne 5} | false |
|  < 或 lt   |   小于   | ${4 < 5} 或 ${4 lt 5}  | true  |
|  > 或 gt   |   大于   | ${4 > 5} 或 ${4 gt 5}  | false |
|  <= 或 le  | 小于等于 | ${9 <= 9} 或 ${9 le 9} | true  |
|  >= 或 ge  | 大于等于 | ${8 >= 8} 或 ${8 ge 8} | true  |



## 逻辑运算符

| 逻辑运算符 |   说明   |                   范例                   | 结果  |
| :--------: | :------: | :--------------------------------------: | :---: |
| && 或 and  |  与运算  | ${1 < 2 $$ 2 < 1} 或 ${1 < 2 and 2 > 1}  | false |
| \|\| 或 or |  或运算  | ${1 < 2 \|\| 2 < 1} 或 ${1 < 2 or 2 < 1} | true  |
|  ! 或 not  | 取反运算 |         ${! true} 或 ${not true}         | false |



## 算数运算符

| 算数运算符 | 说明 |          范例          | 结果 |
| :--------: | :--: | :--------------------: | :--: |
|     +      | 加法 |        ${1 + 2}        |  3   |
|     -      | 减法 |        ${1 - 2}        |  -1  |
|     *      | 乘法 |        ${1 * 2}        |  2   |
|  / 或 div  | 除法 | ${1 / 2} 或 ${1 div 2} |  0   |
|  % 或 mod  | 取模 | ${1 % 2} 或 ${1 mod 2} |  1   |



## 三元运算符

|       三元运算符       |                    说明                    |             范例              |  结果  |
| :--------------------: | :----------------------------------------: | :---------------------------: | :----: |
| 表达式 ? 结果1 : 结果2 | 如果表达式为true，返回结果1，否则返回结果2 | ${1 == 2 ? "等于" : "不等于"} | 不等于 |



## 点运算符和中括号运算符

+ `.`点运算，可以输出 Bean 对象中某个属性的值
+ `[]`中括号运算，可以输出有序集合中某个元素的值。并且还可以输出 map 集合中 key 里含有特殊字符的 key 的值

```jsp
<html>
<head>
    <title>one</title>
</head>
<body>
<%
    Map<String, Object> map = new HashMap<>();
    map.put("a","one");
    map.put("a.a","two");

    pageContext.setAttribute("m",map);
%>

${m.a}<br/>

<%--含有特殊番号的key，必须使用[]运算符来取出其值--%>
${m["a.a"]}

</body>
</html>
```



## Empty运算符

## 格式

`${empty 数据}`

## 作用

empty 运算可以判断一个数据是否为空，如果为空，则输出 true,不为空输出 false

常见的为空的情况：

+ 值为null
+ 值为空字符串
+ 值为Object数组且长度为0
+ 值为list集合且长度为0
+ 值为map集合且长度为0

## 演示

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>two</title>
</head>
<body>
<%
    //1. 值为null时，为空
    request.setAttribute("one",null);
    //2. 值为空字符串时，为空
    request.setAttribute("two","");
    //3. 值为Object数组且长度为0时，为空
    request.setAttribute("three",new Object[]{});
    //4. 值为list集合且长度为0时，为空
    request.setAttribute("four",new ArrayList<String>());
    //5. 值为map集合且长度为0时，为空
    request.setAttribute("five",new HashMap<String, Integer>());
%>
${empty one}<br/>
${empty two}<br/>
${empty three}<br/>
${empty four}<br/>
${empty five}<br/>
</body>
</html>
```

# EL表达式的11个隐含对象

## 概述

EL 个达式中 11 个隐含对象，是 EL 表达式中已经定义好的，我们可以直接使用

|       变量       |         类型          |                         作用                         |
| :--------------: | :-------------------: | :--------------------------------------------------: |
|   pageContext    |    PageContextImpl    |             用来获取jsp中的九大内置对象              |
|    pageScope     |  Map<String, Object>  |            用来获取pageContext域中的数据             |
|   requestScope   |  Map<String, Object>  |              用来获取Request域中的数据               |
|   sessionScope   |  Map<String, Object>  |              用来获取Session域中的数据               |
| applicationScope |  Map<String, Object>  |           用来获取ServletContext域中的数据           |
|      param       |  Map<String, String>  |                 用来获取请求参数的值                 |
|   paramValues    | Map<String, String[]> |               用来获取请求参数的多个值               |
|      header      |  Map<String, String>  |                  用来获取请求头的值                  |
|   headerValues   | Map<String, String[]> |                用来获取请求头的多个值                |
|      cookie      |  Map<String, Cookie>  |              用来获取当前请求Cookie的值              |
|    initParam     |  Map<String, String>  | 用来获取web.xml配置文件中<context-param>标签中的数据 |

下面我会逐个介绍和演示各个对象的用法。

## pageContext对象

我们可以间接的通过pageContext对象获取以下几种数据：

+ 通信协议
+ 服务器ip地址
+ 服务器端口
+ 工程路径
+ 请求方法
+ 客户端ip地址
+ 会话id编号

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>five</title>
</head>
<body>
通信协议：${pageContext.request.scheme}<br/>
服务器ip地址：${pageContext.request.serverName}<br/>
服务器端口号：${pageContext.request.serverPort}<br/>
工程路径：${pageContext.request.contextPath}<br/>
请求方法：${pageContext.request.method}<br/>
客户端ip地址：${pageContext.request.remoteHost}<br/>
会话id编号：${pageContext.session.id}<br/>
</body>
</html>
```



## EL获取四个特定域中的属性

+ pageScope——pageContext域
+ requestScope——Request域
+ sessionScope——Session域
+ applicationScope——ServletContext域

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>three</title>
</head>
<body>
<%
    //分别向四大域中存储数据
    pageContext.setAttribute("key",1);
    request.setAttribute("key",2);
    session.setAttribute("key",3);
    application.setAttribute("key",4);
%>

<%--取出四大域中的数据--%>
${pageScope.key}<br/>
${requestScope.key}<br/>
${sessionScope.key}<br/>
${applicationScope.key}<br/>
</body>
</html>
```

## param对象和paramValues

下面我们演示一下param和paramValues对象使用

```jsp
<%--
  Created by IntelliJ IDEA.
  User: bernardo
  Date: 2021/2/28
  Time: 23:54
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>six</title>
</head>
<body>
输出请求参数username的值：${param.username}<br/>
输出请求参数password的值：${param.password}<br/>
输出请求参数hobby的第一个值：${paramValues.hobby[0]}<br/>
输出请求参数hobby的第二个值：${paramValues.hobby[1]}<br/>
输出请求参数hobby的第三个值：${paramValues.hobby[2]}<br/>
</body>
</html>
```

> 浏览器的请求地址手动输入：http://localhost:8080/JSP/six.jsp?username=Jason&password&31415926&hobby=game&hobby=reading&hobby=run

## header对象和headerValues对象

下面我们演示一下header和headerValues对象使用

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>seven</title>
</head>
<body>
输出请求头【User-Agent】的所有值：${header['User-Agent']}<br/>
输出请求头【Connection】的值：${header.Connection}<br/>
输出请求头【Accept-Encoding】的第一个值：${headerValues['Accept-Encoding'][0]}<br/>
</body>
</html>
```

## cookie对象

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>eight</title>
</head>
<body>
获取Cookie的名称：${cookie.JSESSIONID.name}<br/>
获取Cookie的值：${cookie.JSEESSIONID.value}<br/>
</body>
</html>
```



## initParam对象

为了演示initParam对象的使用，我们需要在web.xml文件的跟标签中添加如下内容

```xml
    <context-param>
        <param-name>username</param-name>
        <param-value>root</param-value>
    </context-param>
    <context-param>
        <param-name>password</param-name>
        <param-value>admin@password</param-value>
    </context-param>
```

下面我们开始演示initParam对象的使用

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>nine</title>
</head>
<body>
输出&lt;Context-param&gt;username 的值：${ initParam.username } <br>
输出&lt;Context-param&gt;password 的值：${ initParam.password } <br>
</body>
</html>
```