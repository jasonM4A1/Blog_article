---
title: JSP阶段三之JSTL标签库
date: 2021-03-01 14:54:04
tags:
	- Java
	- JavaWeb
	- JSP
	- JSTL
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/wallhaven-w82l37.jpg
---

# JSTL标签库概述

JSTL 标签库 全称是指 `JSP Standard Tag Library` ，即JSP标准标签库。是一个不断完善的开放源代码的 JSP 标 签库。

EL 表达式主要是为了替换 jsp 中的表达式脚本，而JSTL标签库则是为了替换代码脚本。这样使得整个 jsp 页面 变得更佳简洁。

# JSTL五种标签库组成

JSTL 由五个不同功能的标签库组成

|      功能范围      |                  URI                   | 前缀 |
| :----------------: | :------------------------------------: | :--: |
| 核心标签库（重点） |   http://java.sun.com/jsp/jstl/core    |  c   |
|       格式化       |    http://java.sun.com/jsp/jstl/fmt    | fmt  |
|        函数        | http://java.sun.com/jsp/jstl/functions |  fn  |
|  数据库（已弃用）  |    http://java.sun.com/jsp/jstl/sql    | sql  |
|   XML（已弃用）    |    http://java.sun.com/jsp/jstl/xml    |  x   |

# JSTL标签库使用步骤

1. 首先导入jstl标签库的两个jar包
   + `taglibs-standard-impl-1.2.1.jar`
   + `taglibs-standard-spec-1.2.1.jar`
2. 然后使用`taglib`指令引入标签库
   + 核心标签库——`<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>`
   + 格式化——`<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>`
   + 函数——`<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>`
   + 数据库——`<%@ taglib prefix="sql" uri="http://java.sun.com/jsp/jstl/sql" %>`
   + XML——`<%@ taglib prefix="x" uri="http://java.sun.com/jsp/jstl/xml" %>`

> 引入标签库时，用到哪个引入哪个即可，不用全部引入

# JSTL——croe核心库

## <c:set />

### 格式

`<c:set scope="域范围" var="属性名" value="属性值" />`

其中scope可以填入四种值：

+ page——表示PageContext域（默认值）
+ request——表示Request域
+ session——表示Session域
+ application——表示ServletContext域

### 作用

set 标签可以往域中保存数据

### 演示

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>ten</title>
</head>
<body>

<%--保存数据--%>
<c:set scope="session" var="one" value="001" />

<%--取出数据--%>
${sessionScope.one}

</body>
</html>
```

> <c:set />现在已经很少被使用了，做为了解即可

## <c:if />

### 格式

`<c:if test="表达式"> 表达式为true时，输出的内容 </c:if>`

> test中的表达式使用EL表达式来书写

### 作用

if 标签用来做 if 判断

### 演示

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>eleven</title>
</head>
<body>

<c:if test="${1 != 2}">
    <h2>1不等于2</h2>
</c:if>

<c:if test="${1 == 2}">
    <h2>1等于2</h2>
</c:if>

</body>
</html>
```

## <c:choose />

### 格式

```jsp
<c:choose>
    <c:when test="EL表达式">
        EL表达式为true时，输出的内容
    </c:when>
    <c:when test="EL表达式">
        EL表达式为true时，输出的内容
    </c:when>
    <c:otherwise>
        上述EL表达式都不为true时，输出的内容
    </c:otherwise>
</c:choose>
```

> 标签里不能使用 html 注释，要使用 jsp 注释
>
> when 标签的父标签一定要是 choose 标签

### 作用

多路判断，与 switch ... case .... default 非常接近

### 演示

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>twelve</title>
</head>
<body>

<%
    session.setAttribute("weight","140");
%>

<c:choose>
    <c:when test="${sessionScope.weight >= 160}">
        <h2>体重大于等于160</h2>
    </c:when>
    <c:when test="${sessionScope.weight >= 140}">
        <h2>体重大于等于140</h2>
    </c:when>
    <c:otherwise>
        <h2>其他体重</h2>
    </c:otherwise>
</c:choose>

</body>
</html>
```

## <c:forEach />

### 格式

`<c:forEach begin="开始索引" end="结束索引" var="遍历的变量" >`

```jsp
<c:forEach begin="开始索引" end="结束索引" var="遍历的变量">
    输出的内容
</c:forEach>
```

其他常见属性值：

+ `iterms`：遍历的数据源
+ `varStatus`：当前遍历到的数据的状态
+ `step`：遍历的步长值

> 上面的只是一般格式，在遍历不同的数据时，需添加相应的属性

### 作用

像Java中for循环一样，遍历数据

### 演示

#### 遍历1到10

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>thirteen</title>
</head>
<body>

<c:forEach begin="1" end="10" var="i">
    ${i}<br/>
</c:forEach>

</body>
</html>
```

#### 遍历 Object 数组

```jsp
<%@ page import="java.util.List" %>
<%@ page import="java.util.ArrayList" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>fourteen</title>
</head>
<body>

<%
    List<Integer> arr = new ArrayList<>();
    arr.add(1);
    arr.add(2);
    arr.add(3);
    session.setAttribute("arr",arr);
%>

<c:forEach items="${sessionScope.arr}" var="item">
    ${item}<br/>
</c:forEach>

</body>
</html>
```

#### 遍历Map集合

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page import="java.util.Map" %>
<%@ page import="java.util.HashMap" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>fifteen</title>
</head>
<body>

<%
    Map<String, Integer> map = new HashMap<>();
    map.put("one",1);
    map.put("two",2);
    map.put("three",3);
    request.setAttribute("map",map);
%>

<c:forEach items="${map}" var="item">
    ${item.key} = ${item.value}<br/>
</c:forEach>

</body>
</html>
```

#### 遍历List集合（list中存放着Student类）

首先我们准备一个Student类

```java
package xyz.rtx3090.servlet;

public class Student {
    private Integer id;
    private String username;
    private String password;
    private Integer age;
    private String phoneNumber;
    
    //有参、无参构造函数
    
    //setter and getter
    
    //hashCode and equals
    
    //toString
}
```

下面开始遍历存放着Student类的list集合

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page import="xyz.rtx3090.servlet.Student" %>
<%@ page import="java.util.ArrayList" %>
<%@ page import="java.util.List" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>one</title>
</head>
<body>

<%
    List<Student> studentList = new ArrayList<Student>();

    for (int i = 0; i < 10; i++) {
        studentList.add(new Student(i,"username" + i,"password" + i, 18 + i, "18712345610" + i));
    }

    request.setAttribute("studentList",studentList);
%>

<c:forEach begin="2" end="8" step="2" varStatus="status" items="${requestScope.studentList}" var="student">
    ${student.id}<br/>
    ${student.username}<br/>
    ${student.password}<br/>
    ${student.age}<br/>
    ${student.phoneNumber}<br/>
    ${student.step}<br/>
</c:forEach>

</body>
</html>
```

