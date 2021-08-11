AJAX入门

---

# 全局刷新和局部刷新

B/S 结构项目中， 浏览器(Browse)负责把用户的请求和参数通过网络发送给服务器 (Server),服务端使用 Servlet(多种服务端技术的一种)接收请求，并将处理结果返回给浏览器。浏览器在 html，jsp 上呈现数据，混合使用 css， js 帮助美化页面，或响应事件。

## 全局刷新

### 登录请求处理步骤

`index.jsp`发起登录请求 -----> `LoginServlet` -----> `result.jsp`

### 发起请求request阶段

浏览器现在内存中是 index 页面的内容和数据

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210719074238.png)

### 服务器应答结果阶段

sevlet 返回后把数据全部覆盖掉原来 index 页面内容，`result.jsp`覆盖了全部的浏览器内存数据。 整个浏览器数据全部被刷新。重新在浏览器窗口显示数据，样式，标签等.

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210719074343.png)

### 全局刷新原理

1. 必须由浏览器亲自向服务端发送请求协议包
2. 这个行为导致服务端直接将【响应包】发送到浏览器内存中
3. 这个行为导致浏览器内存中原有内容被覆盖掉
4. 这个行为导致浏览器在展示数据时候，只有响应数据可以展示

## 局部刷新

浏览器在展示数据时，此时在窗口既可以看到本次的响应数据， 同时又可以看到浏览器内存中原有数据

### 局部刷新原理

1. 不能由浏览器发送请求给服务端
2. 浏览器委托浏览器内存中一个脚本对象代替浏览器发送请求.
3. 这个行为导致导致服务端直接将【响应包】发送脚本对象内存中
4. 这个行为导致脚本对象内容被覆盖掉，但是此时浏览器内存中绝大部分内容没有收到任何影响
5. 这个行为导致浏览器在展示数据时候,同时展示原有数据和响应数据

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210719074559.png)

**AJAX 是实现局部刷新的一种技术**

## 异步请求对象

### 介绍

在局部刷新，需要创建一个对象，代替浏览器发起请求的行为，这个对象存在内存中。 代替浏览器发起请求并接收响应数据。这个对象叫做异步请求对象

全局刷新是同步行为， 局部刷新是异步行为[浏览器数据没有全部更新]

这个异步对象用于在后台与服务器交换数据。`XMLHttpRequest`就是我们说的异步对象

### XMLHttpRequest

`XMLHttpRequest`对象能够：

1. 在不重新加载页面的情况下更新网页
2. 在页面已加载后向服务器请求数据
3. 在页面已加载后从服务器接收数据

所有现代浏览器 (IE7+、Firefox、Chrome、Safari 以及 Opera) 都内建了`XMLHttpRequest`对象。通过一行简单的`JavaScript`代码，我们就可以创建`XMLHttpRequest`对象

```javascript
var xmlHttp = new XMLHttpRequest();
```

AJAX 中的核心对象就是 `XMLHttpRequest`

# AJAX

## 什么是AJAX？

AJAX(Asynchronous JavaScript and XML)——异步的 JavaScript和XML

**AJAX 是一种在无需重新加载整个网页的情况下，能够更新部分页面内容的新方法**

AJAX 不是新的编程语言，而是使用现有技术混合使用的一种新方法。ajax 中使用的技术有 JavaScript, html , dom , xml ,css 等。主要是 JavaScript , XML.

`JavaScript`负责：使用脚本对象`XMLHttpRequest`发送请求， 接收响应数据

`XML`负责：发送和接收的数据格式（现在已使用`Json`代替`XML`）

AJAX 不单需要前端的技术，同时需要后端(服务器)的配合。服务器需要提供数据，数据 是 AJAX 请求的响应结果

## AJAX异步实现步骤

### `XMLHttpRequest`对象介绍

#### 创建对象方式

`var xmlHttp = new XMLHttpRequest();`

#### `onreadystatechange`事件

当请求被发送到服务器时，我们需要执行一些基于响应的任务。每当`readyState`改变时，就会触发`onreadystatechange`事件。此事件可以指定一个处理函数 `function`。

通过判断`XMLHttpReqeust`对象的状态，获取服务端返回的数据。

```javascript
var xmlHttp = new XMLHttpRequest();

xmlHttp.onreadystatechange = function() {
  if(xmlHttp.readyState == 4 && xmlHttp.status == 200) {
    //处理服务器返回的数据
  }
}
```

#### 三个重要属性

1. `onreadystatechange`属性：一个 js 函数名 或 直接定义函数，每当`readyState`属性改变时，就会调用该函数
2. `readyState`属性：存有`XMLHttpRequest`的状态，从 0 到 4 发生变化。
   + `0`：请求未初始化，创建异步请求对象——`var xmlHttp = new XMLHttpRequest()`
   + `1`：初始化异步请求对象——`xmlHttp.open(请求方式，请求地址，true)`
   + `2`：异步对象发送请求——`xmlHttp.send()`
   + `3`：异步对象接收应答数据从服务端返回数据——`XMLHttpRequest`内部处理
   + `4`：异步请求对象已经将数据解析完毕——此时才可以读取数据
3. `status `属性：
   + `200`：页面状态正常
   + `404`：未找到页面

#### 初始化请求参数

1. 方法：`open(method, url, async)`：初始化异步请求对象
2. 参数说明
   + `method`：请求的类型——GET 或 POST
   + `url`：服务器的 servlet 地址
   + `async`：选择为异步还是同步——true(异步)或 false(同步)
3. 举例：`xmlHttp.open("get","http:192.168.1.20:8080/myweb/query",true)`

**XMLHttpRequest 对象 open( method , url, true ) 第三个参数 true 表示异步请求 异步请求 特点:** 

+ 某一个时刻，浏览器可以委托多个异步请求对象发送请求，无需等待请求处理完成。 
+ 浏览器委托异步请求对象工作期间，浏览器处于活跃状态。可以继续向下执行其他命令。
+ 当响应就绪后再对响应结果进行处理

**XMLHttpRequest 对象 open( method , url, false ) 第三个参数 false 表示同步请求 同步请求，特点:** 

+ 某一个时刻，浏览器只能委托一个异步请求对象发送请求，必须等待请求处理完成。 
+ 浏览器委托异步请求对象工作期间，浏览器处于等待状态。不能执行其他命令。 
+ 不推荐使用。

#### 发送请求

`xmlHttp.send()`

#### 接收服务器响应的数据

如需获得来自服务器的响应，请使用`XMLHttpRequest`对象的`responseText`或`responseXML`属性

+ `responseText`：获得字符串形式的响应数据
+ `responseXML`：获得 XML 形式的响应数据

## 案例演示

### 全局刷新案例演示

#### 案例需求

计算某个用户的 BMI：用户在 jsp页面中输入自己的身高、体重，servlet 中计算 BMI，并显示 BMI 的计算结果和建议

> BMI 指数(即身体质量指数，英文为 BodyMassIndex，简称 BMI)，是用体重公斤数除以身高 米数平方得出的数字，是目前国际上常用的衡量人体胖瘦程度以及是否健康的一个标准

成人的 BMI 数值：

1. 过轻：低于 18.5
2. 正常：18.5-23.9
3. 过重：24-27
4. 肥胖：28-32
5. 非常肥胖：高于32

#### 案例实现一

**使用HttpsServletRequest请求转发到新页面**

1. 新建一个带有`maven-archetype-webapp`的maven项目，工程目录结果如图所示：

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210719084852.png)

2. 配置`pom.xml`文件，导入Jar包依赖

   ```xml
       <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
       <dependency>
         <groupId>javax.servlet</groupId>
         <artifactId>javax.servlet-api</artifactId>
         <version>4.0.1</version>
         <scope>provided</scope>
       </dependency>
       <!-- https://mvnrepository.com/artifact/javax.servlet.jsp/javax.servlet.jsp-api -->
       <dependency>
         <groupId>javax.servlet.jsp</groupId>
         <artifactId>javax.servlet.jsp-api</artifactId>
         <version>2.3.3</version>
         <scope>provided</scope>
       </dependency>
   ```

3. 编写`index.jsp`页面

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>BMI测量</title>
   </head>
   <body>
   <form action="bmi" method="get">
       姓名：<input type="text" name="name"><br>
       身高（米）：<input type="text" name="height"><br>
       体重（公斤）：<input type="text" name="weight"><br>
       <input type="submit" value="开始计算">
   </form>
   </body>
   </html>
   ```

4. 编写`BMIServlet`类

   ```java
   package xyz.rtx3090.servlet;
   
   import javax.servlet.ServletException;
   import javax.servlet.http.HttpServlet;
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   import java.io.IOException;
   
   public class BMIServlet extends HttpServlet {
   
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           //接收参数
           String name = req.getParameter("name");
           String height = req.getParameter("height");
           String weight = req.getParameter("weight");
   
           //计算bmi数值
           float h = Float.parseFloat(height);
           float w = Float.parseFloat(weight);
           float bmi = w / (h * h);
           System.out.printf("%s的bmi为%f",name, bmi);
   
           //判断体重情况
           String msg = "";
           if (bmi < 18.5) {
               msg = "过瘦";
           } else if (bmi >= 18.5 && bmi < 23.9) {
               msg = "正常";
           } else if (bmi >= 23.9 && bmi < 27) {
               msg = "过重";
           } else if (bmi >= 27 && bmi < 32) {
               msg = "肥胖";
           } else {
               msg = "非常肥胖";
           }
   
           //发送请求数据
           req.setAttribute("msg", name + "您的BMI值为：" + bmi + "；属于"+msg+"水平");
           req.getRequestDispatcher("/WEB-INF/jsp/result.jsp").forward(req,resp);
       }
   
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doGet(req,resp);
       }
   }
   ```

5. 编写`/WEB-INF/jsp/result.jsp`页面

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>result</title>
   </head>
   <body>
   ${msg}
   </body>
   </html>
   ```

6. 编写`web.xml`文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
   
     <servlet>
       <servlet-name>bmi</servlet-name>
       <servlet-class>xyz.rtx3090.servlet.BMIServlet</servlet-class>
     </servlet>
     <servlet-mapping>
       <servlet-name>bmi</servlet-name>
       <url-pattern>/bmi</url-pattern>
     </servlet-mapping>
   </web-app>
   ```

7. 配置并启动Tomcat服务器，测试成功！

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210719085218.png)

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210719085231.png)

#### 案例实现二

**使用HttpServletResponse响应直接进行输出**

1. 新建一个带有`maven-archetype-webapp`的maven项目，工程目录结果如图所示：

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210719102419.png)

2. 配置`pom.xml`文件，导入Jar包依赖

   ```xml
       <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
       <dependency>
         <groupId>javax.servlet</groupId>
         <artifactId>javax.servlet-api</artifactId>
         <version>4.0.1</version>
         <scope>provided</scope>
       </dependency>
       <!-- https://mvnrepository.com/artifact/javax.servlet.jsp/javax.servlet.jsp-api -->
       <dependency>
         <groupId>javax.servlet.jsp</groupId>
         <artifactId>javax.servlet.jsp-api</artifactId>
         <version>2.3.3</version>
         <scope>provided</scope>
       </dependency>
   ```

3. 编写`index.jsp`页面

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>BMI测量</title>
   </head>
   <body>
   <form action="bmi" method="get">
       姓名：<input type="text" name="name"><br>
       身高（米）：<input type="text" name="height"><br>
       体重（公斤）：<input type="text" name="weight"><br>
       <input type="submit" value="开始计算">
   </form>
   </body>
   </html>
   ```

4. 编写`BMIServlet`类

   ```java
   package xyz.rtx3090.servlet;
   
   import javax.servlet.ServletException;
   import javax.servlet.http.HttpServlet;
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   import java.io.IOException;
   import java.io.PrintStream;
   import java.io.PrintWriter;
   
   public class BMIServlet extends HttpServlet {
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           //接收参数
           String name = req.getParameter("name");
           String height = req.getParameter("height");
           String weight = req.getParameter("weight");
   
           //计算bmi数值
           float h = Float.parseFloat(height);
           float w = Float.parseFloat(weight);
           float bmi = w / (h * h);
           System.out.printf("%s的bmi为%f",name, bmi);
   
           //判断体重情况
           String msg = "";
           if (bmi < 18.5) {
               msg = "过瘦";
           } else if (bmi >= 18.5 && bmi < 23.9) {
               msg = "正常";
           } else if (bmi >= 23.9 && bmi < 27) {
               msg = "过重";
           } else if (bmi >= 27 && bmi < 32) {
               msg = "肥胖";
           } else {
               msg = "非常肥胖";
           }
   
           //直接响应输出数据
           resp.setContentType("text/html;charset=utf-8");
           PrintWriter pw = resp.getWriter();
           pw.println(name + "您的BMI值为：" + bmi + "；属于"+msg+"水平");
           pw.flush();
           pw.close();
       }
   
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doGet(req,resp);
       }
   }
   ```

5. 编写`web.xml`文件

   ```jsp
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
   
     <servlet>
       <servlet-name>bmi</servlet-name>
       <servlet-class>xyz.rtx3090.servlet.BMIServlet</servlet-class>
     </servlet>
     <servlet-mapping>
       <servlet-name>bmi</servlet-name>
       <url-pattern>/bmi</url-pattern>
     </servlet-mapping>
   </web-app>
   ```

6. 配置并启动Tomcat服务器，测试成功！

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210719102655.png)

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210719102709.png)

### 局部刷新案例演示

#### 案例需求一

与“全局刷新”的基本需求一致，也是计算BMI值。但是这次我们不能全部的更新页面信息，而是局部的更新页面信息，来显示BMI值的计算结果。

#### 案例实现一

1. 新建一个带有`maven-archetype-webapp`的maven项目，工程目录结果如图所示：

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210719120721.png)

2. 配置`pom.xml`文件，导入Jar包依赖

   ```xml
   				<!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
           <dependency>
               <groupId>javax.servlet</groupId>
               <artifactId>javax.servlet-api</artifactId>
               <version>4.0.1</version>
               <scope>provided</scope>
           </dependency>
           <!-- https://mvnrepository.com/artifact/javax.servlet.jsp/javax.servlet.jsp-api -->
           <dependency>
               <groupId>javax.servlet.jsp</groupId>
               <artifactId>javax.servlet.jsp-api</artifactId>
               <version>2.3.3</version>
               <scope>provided</scope>
           </dependency>
   ```

3. 编写`index.jsp`页面

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>测量BMI值</title>
       <script type="text/javascript">
           function doAjax() {
               //创建异步对象
               var xmlHttp = new XMLHttpRequest();
               //绑定事件
               xmlHttp.onreadystatechange = function() {
                   if (xmlHttp.readyState == 4 && xmlHttp.status == 200) {
                       var data = xmlHttp.responseText;
                       document.getElementById("dataDiv").innerText = data;
                   }
               }
               //初始化参数
               let name = document.getElementById("name").value;
               let height = document.getElementById("height").value;
               let weight = document.getElementById("weight").value;
               var param = "name="+name+"&height="+height+"&weight="+weight;
               xmlHttp.open("get","bmi?"+param,true);
               //发送ajax异步请求
               xmlHttp.send();
           }
       </script>
   </head>
   <body>
   <div style="margin-left: 500px">
       <%--没有form--%>
       姓名：<input type="text" id="name"><br>
       身高：<input type="text" id="height"><br>
       体重：<input type="text" id="weight"><br>
       <input type="button" value="计算BMI值" onclick="doAjax()"><br><br>
       <div id="dataDiv">
           等待数据更新...
       </div>
   </div>
   </body>
   </html>
   ```

4. 编写`BMIServlet`类

   ```java
   package xyz.rtx3090.servlet;
   
   import javax.servlet.ServletException;
   import javax.servlet.http.HttpServlet;
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   import java.io.IOException;
   import java.io.PrintWriter;
   
   public class BMIServlet extends HttpServlet {
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           //接收参数
           String name = req.getParameter("name");
           String height = req.getParameter("height");
           String weight = req.getParameter("weight");
   
           //计算bmi数值
           float h = Float.parseFloat(height);
           float w = Float.parseFloat(weight);
           float bmi = w / (h * h);
           System.out.printf("%s的bmi为%f",name, bmi);
   
           //判断体重情况
           String msg = "";
           if (bmi < 18.5) {
               msg = "过瘦";
           } else if (bmi >= 18.5 && bmi < 23.9) {
               msg = "正常";
           } else if (bmi >= 23.9 && bmi < 27) {
               msg = "过重";
           } else if (bmi >= 27 && bmi < 32) {
               msg = "肥胖";
           } else {
               msg = "非常肥胖";
           }
   
           //直接响应输出数据
           resp.setContentType("text/html;charset=utf-8");
           PrintWriter pw = resp.getWriter();
           pw.println(name + "您的BMI值为：" + bmi + "；属于"+msg+"水平");
           pw.flush();
           pw.close();
       }
   
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doGet(req,resp);
       }
   }
   ```

5. 编写`web.xml`文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
   
     <servlet>
       <servlet-name>bmi</servlet-name>
       <servlet-class>xyz.rtx3090.servlet.BMIServlet</servlet-class>
     </servlet>
     <servlet-mapping>
       <servlet-name>bmi</servlet-name>
       <url-pattern>/bmi</url-pattern>
     </servlet-mapping>
   </web-app>
   ```

6. 配置并启动Tomcat服务器，测试成功！

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210719125635.png)

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210719125654.png)

#### 案例需求二（结合数据库）

用户在文本框架输入省份的编号 id，在其他文本框显示省份名称（结合数据库、AJAX）

#### 案例实现二

1. 准备数据库资源

   ```mysql
   drop database if exists AJAX;
   
   create database AJAX;
   
   use AJAX;
   
   create table `province`
   (
       `id`   int(20) auto_increment,
       `name` varchar(20) not null,
       primary key (`id`)
   ) ENGINE = InnoDB
     default charset = utf8;
   
   insert into province (name)
   values ('北京'),
          ('上海'),
          ('广州'),
          ('深圳');
   
   select * from province;
   ```

2. 新建一个带有`maven-archetype-webapp`模版的maven项目，工程目录结果如图所示：

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210727124628.png)
   
3. 配置`pom.xml`文件，导入Jar包依赖

   ```xml
   <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
       <dependency>
         <groupId>mysql</groupId>
         <artifactId>mysql-connector-java</artifactId>
         <version>8.0.21</version>
       </dependency>
       <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
       <dependency>
         <groupId>javax.servlet</groupId>
         <artifactId>javax.servlet-api</artifactId>
         <version>4.0.1</version>
         <scope>provided</scope>
       </dependency>
       <!-- https://mvnrepository.com/artifact/javax.servlet.jsp/javax.servlet.jsp-api -->
       <dependency>
         <groupId>javax.servlet.jsp</groupId>
         <artifactId>javax.servlet.jsp-api</artifactId>
         <version>2.3.3</version>
         <scope>provided</scope>
       </dependency>
   ```

4. 编写`index.jsp`页面

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>省份选择</title>
       <script type="text/javascript">
           function doAjax() {
               //创建异步对象
               let xmlHttpRequest = new XMLHttpRequest;
               //绑定事件
               xmlHttpRequest.onreadystatechange = function () {
                   if (xmlHttpRequest.readyState == 4 && xmlHttpRequest.status == 200) {
                       let data = xmlHttpRequest.responseText;
                       document.getElementById("provinceName").value = data;
                   }
               }
               //初始化请求参数
               let provinceId = document.getElementById("provinceId").value;
               xmlHttpRequest.open("get", "province?provinceId=" + provinceId, true);
               //发送Ajax请求
               xmlHttpRequest.send();
           }
       </script>
   </head>
   <body>
   
       <table border="1">
           <tr>
               <td>省份编号：</td>
               <td>
                   <input type="text" id="provinceId">
                   <input type="submit" value="提交" onclick="doAjax()">
               </td>
           </tr>
           <tr>
               <td>省份名称：</td>
               <td>
                   <input type="text" id="provinceName">
               </td>
           </tr>
       </table>
   
   </body>
   </html>
   ```

5. 编写`ProvinceDao`类，用于进行数据库操作

   ```java
   package xyz.rtx3090.dao;
   
   import java.sql.*;
   
   public class ProvinceDao {
   
       /**
        * 根据传入的省份编号来获取相应的省份名称
        * @param provinceId
        * @return
        */
       public String selectProvinceName(Integer provinceId) {
           String provinceName = "";
           Connection conn = null;
           PreparedStatement pst = null;
           ResultSet rs = null;
           String url = "jdbc:mysql://localhost:3306/AJAX";
           String username = "root";
           String password = "intmain()";
           String sql = "";
   
           try {
               Class.forName("com.mysql.jdbc.Driver");
               conn = DriverManager.getConnection(url,username,password);
               sql = "select name from province where id = ?";
               pst = conn.prepareStatement(sql);
               pst.setInt(1,provinceId);
   
               rs = pst.executeQuery();
               if (rs.next()) {
                   provinceName = rs.getString("name");
               }
           } catch (ClassNotFoundException | SQLException e) {
               e.printStackTrace();
           } finally {
               if (rs != null) {
                   try {
                       rs.close();;
                   } catch (Exception e) {
                       e.printStackTrace();
                   }
               }
               if (pst != null) {
                   try {
                       pst.close();
                   } catch (Exception e) {
                       e.printStackTrace();
                   }
               }
               if (conn != null) {
                   try {
                       conn.close();
                   } catch (Exception e) {
                       e.printStackTrace();
                   }
               }
           }
   
           return provinceName;
       }
   }
   ```

6. 编写`ProvinceController`类，进行servlet操作

   ```java
   package xyz.rtx3090.controller;
   
   import xyz.rtx3090.dao.ProvinceDao;
   
   import javax.servlet.ServletException;
   import javax.servlet.http.HttpServlet;
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   import java.io.IOException;
   import java.io.PrintWriter;
   
   public class ProvinceController extends HttpServlet {
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           //获取请求参数
           String provinceId = req.getParameter("provinceId");
   
           //业务判断
           String provinceName = "";
           if (provinceId != null) {
               ProvinceDao dao = new ProvinceDao();
               provinceName = dao.selectProvinceName(Integer.parseInt(provinceId));
           }
   
           //进行响应请求
           resp.setContentType("text/html;charset=utf-8");
           PrintWriter writer = resp.getWriter();
           writer.println(provinceName);
           writer.flush();
           writer.close();
       }
   
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doGet(req,resp);
       }
   }
   ```

7. 编写`web.xml`文件，配置servlet（注意修改头文件）

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
   
     <servlet>
       <servlet-name>province</servlet-name>
       <servlet-class>xyz.rtx3090.controller.ProvinceController</servlet-class>
     </servlet>
     <servlet-mapping>
       <servlet-name>province</servlet-name>
       <url-pattern>/province</url-pattern>
     </servlet-mapping>
   </web-app>
   ```

8. 配置并启动tomcat服务器

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210727125315.png)

# AJAX完整实现演示（结合数据库和json）

## 案例需求

在前端页面中输入`省份编号`，可以查询其`省份名称`、`省份简称`、`省会城市`信息。

> 要使用mysql和json完成此案例

## 案例实现

1. 准备数据库资源

   ```mysql
   drop database if exists AJAX;
   
   create database AJAX;
   
   use AJAX;
   
   drop table if exists province;
   
   create table `province`
   (
       `id`      int auto_increment,
       `name`    varchar(20) not null,
       `abbName` varchar(5)  not null,
       `city`    varchar(20) not null,
       primary key (`id`)
   ) ENGINE = InnoDB
     default charset = utf8;
   
   insert into province (name, abbName, city)
   VALUES ('河南','豫','郑州'),
          ('湖南','湘','长沙'),
          ('陕西','陕','西安'),
          ('黑龙江','黑','哈尔滨');
   
   select * from province;
   ```

2. 新建一个带有`maven-archetype-webapp`模版的maven项目，工程目录结果如图所示：

   ![image-20210727185242931](/Users/bernardo/Library/Application Support/typora-user-images/image-20210727185242931.png)

3. 配置`pom.xml`文件，导入Jar包依赖

   ```xml
       <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
       <dependency>
         <groupId>javax.servlet</groupId>
         <artifactId>javax.servlet-api</artifactId>
         <version>4.0.1</version>
         <scope>provided</scope>
       </dependency>
       <!-- https://mvnrepository.com/artifact/javax.servlet.jsp/javax.servlet.jsp-api -->
       <dependency>
         <groupId>javax.servlet.jsp</groupId>
         <artifactId>javax.servlet.jsp-api</artifactId>
         <version>2.3.3</version>
         <scope>provided</scope>
       </dependency>
       <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
       <dependency>
         <groupId>mysql</groupId>
         <artifactId>mysql-connector-java</artifactId>
         <version>8.0.21</version>
       </dependency>
       <dependency>
         <groupId>com.fasterxml.jackson.core</groupId>
         <artifactId>jackson-databind</artifactId>
         <version>2.8.3</version>
       </dependency>
     </dependencies>
   ```

4. 编写实体类`Province`

   ```java
   package xyz.rtx3090.pojo;
   
   public class Province {
       private Integer id;
       private String name;
       private String abbName;
       private String city;
   
       //constructor
       //setter and getter
   		//toString
   }
   ```

5. 编写Dao类`ProvinceDao`，进行数据库的操作

   ```java
   package xyz.rtx3090.dao;
   
   import xyz.rtx3090.pojo.Province;
   
   import java.sql.*;
   
   public class ProvinceDao {
       /**
        * 根据省份编号获取省份对象
        * @param provinceId 省份编号
        * @return 省份对象
        */
       public Province selectProvince(Integer provinceId) {
           Province province = new Province();
           Connection conn = null;
           PreparedStatement pst = null;
           ResultSet rs = null;
           String url = "jdbc:mysql://localhost:3306/AJAX";
           String username = "root";
           String password = "intmain()";
           String sql = "";
   
           try {
               Class.forName("com.mysql.cj.jdbc.Driver");
               conn = DriverManager.getConnection(url,username,password);
               sql = "select id, name, abbName, city from province where id = ?";
               pst = conn.prepareStatement(sql);
               pst.setInt(1,provinceId);
               rs = pst.executeQuery();
               if (rs.next()) {
                   province.setId(rs.getInt(1));
                   province.setName(rs.getString("name"));
                   province.setAbbName(rs.getString("abbName"));
                   province.setCity(rs.getString("city"));
               }
           } catch (ClassNotFoundException | SQLException e) {
               e.printStackTrace();
           } finally {
               if (rs != null) {
                   try {
                       rs.close();
                   } catch (Exception e) {
                       e.printStackTrace();
                   }
               }
               if (pst != null) {
                   try {
                       pst.close();
                   } catch (Exception e) {
                       e.printStackTrace();
                   }
               }
               if (conn != null) {
                   try {
                       conn.close();
                   } catch (Exception e) {
                       e.printStackTrace();
                   }
               }
           }
           return province;
       }
   }
   ```

6. 编写jsp页面`index.jsp`

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>ajax使用json实现</title>
       <script type="text/javascript">
           function search() {
               //创建异步对象
               let xmlHttpRequest = new XMLHttpRequest();
               //绑定事件
               xmlHttpRequest.onreadystatechange = function () {
                   if (xmlHttpRequest.readyState == 4 && xmlHttpRequest.status == 200) {
                       let data = xmlHttpRequest.responseText;
                       //将json格式字符串转为 json对象
                       let jsonData = eval("(" + data + ")");
                       //处理结果函数
                       callback(jsonData);
                   }
               }
               //初始化请求参数
               let provinceId = document.getElementById("provinceId").value;
               xmlHttpRequest.open("get","search?provinceId=" + provinceId,true);
               //发送ajax请求
               xmlHttpRequest.send();
           }
           function callback(json) {
               document.getElementById("provinceName").value = json.name;
               document.getElementById("provinceAddName").value = json.abbName;
               document.getElementById("provinceCity").value = json.city;
           }
       </script>
   </head>
   <body>
   
   <table border="1">
       <tr>
           <td>省份编号：</td>
           <td>
               <input type="text" id="provinceId">
               <input type="submit" value="提交" onclick="search()">
           </td>
       </tr>
       <tr>
           <td>省份名称：</td>
           <td>
               <input type="text" id="provinceName">
           </td>
       </tr>
       <tr>
           <td>省份简写：</td>
           <td>
               <input type="text" id="provinceAddName">
           </td>
       </tr>
       <tr>
           <td>省会城市：</td>
           <td>
               <input type="text" id="provinceCity">
           </td>
       </tr>
   </table>
   </body>
   </html>
   ```

7. 编写Controller类`ProvinceController`，进行服务器业务操作

   ```java
   package xyz.rtx3090.controller;
   
   import com.fasterxml.jackson.databind.ObjectMapper;
   import xyz.rtx3090.dao.ProvinceDao;
   import xyz.rtx3090.pojo.Province;
   
   import javax.servlet.ServletException;
   import javax.servlet.http.HttpServlet;
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   import java.io.IOException;
   import java.io.PrintWriter;
   
   public class ProvinceController extends HttpServlet {
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           //获取请求参数
           String provinceId = req.getParameter("provinceId");
   
           //创建空的json对象
           String json = "{}";
   
           //如果参数不为null或者空，则进行数据库的查询
           if (provinceId != null && !("".equals(provinceId))) {
               ProvinceDao provinceDao = new ProvinceDao();
               Province province = provinceDao.selectProvince(Integer.parseInt(provinceId));
               System.out.println("province对象："+province);
               //如果查询结果对象不为null，则将其转化为json字符串
               if (province != null) {
                   ObjectMapper objectMapper = new ObjectMapper();
                   json = objectMapper.writeValueAsString(province);
               }
           }
   
           //响应输出json字符串数据
           resp.setContentType("application/json;charset=utf-8");
           PrintWriter writer = resp.getWriter();
           writer.println(json);
           writer.flush();
           writer.close();
       }
   
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doGet(req,resp);
       }
   }
   ```

8. 配置`web.xml`文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <servlet>
           <servlet-name>search</servlet-name>
           <servlet-class>xyz.rtx3090.controller.ProvinceController</servlet-class>
       </servlet>
       <servlet-mapping>
           <servlet-name>search</servlet-name>
           <url-pattern>/search</url-pattern>
       </servlet-mapping>
   </web-app>
   ```

9. 配置并启动Tomcat服务器

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210727190318.png)

# AJAX完整实现演示二（结合数据库和json和jQuery）

## 案例需求

在前端页面中输入`省份编号`，可以查询其`省份名称`、`省份简称`、`省会城市`信息。

> 要使用mysql、json和jQuery完成此案例

## 案例实现

在`AJAX完整实现演示一`的基础上，只需要对`index.jps`页面进行修改即可

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>ajax使用json实现</title>
    <%--引入在线JQuery文件--%>
    <script src="https://code.jquery.com/jquery-3.1.1.min.js"></script>
    <script type="text/javascript">
        /*通过JQuery完成AJAX的请求发送*/
        $(function () {
            $("#submit").click(
                function () {
                    $.ajax({
                        url:"search",
                        data:{
                            provinceId:$("#provinceId").val()
                        },
                        type:"get",
                        dataType:"json",
                        success:function(resp) {
                            callback(resp);
                        }
                    })
                }
            );
            function callback(json) {
                document.getElementById("provinceName").value = json.name;
                document.getElementById("provinceAddName").value = json.abbName;
                document.getElementById("provinceCity").value = json.city;
            };

            $("#test").click(
                function () {
                    let data = $("#provinceName").val();
                    alert("data: " + data);
                }
            );
        });
    </script>
</head>
<body>

<table border="1">
    <tr>
        <td>省份编号：</td>
        <td>
            <input type="text" id="provinceId">
            <input type="submit" value="提交" id="submit">
        </td>
    </tr>
    <tr>
        <td>省份名称：</td>
        <td>
            <input type="text" id="provinceName">
            <input type="submit" id="test">
        </td>
    </tr>
    <tr>
        <td>省份简写：</td>
        <td>
            <input type="text" id="provinceAddName">
        </td>
    </tr>
    <tr>
        <td>省会城市：</td>
        <td>
            <input type="text" id="provinceCity">
        </td>
    </tr>
</table>
</body>
</html>
```

## 补充

```javascript
jQuery.ajax(...)
      部分参数：
            url：请求地址
            type：请求方式，GET、POST（1.9.0之后用method）
        headers：请求头
            data：要发送的数据
    contentType：即将发送信息至服务器的内容编码类型(默认: "application/x-www-form-urlencoded; charset=UTF-8")
          async：是否异步
        timeout：设置请求超时时间（毫秒）
      beforeSend：发送请求前执行的函数(全局)
        complete：完成之后执行的回调函数(全局)
        success：成功之后执行的回调函数(全局)
          error：失败之后执行的回调函数(全局)
        accepts：通过请求头发送给服务器，告诉服务器当前客户端可接受的数据类型
        dataType：将服务器端返回的数据转换成指定类型
          "xml": 将服务器端返回的内容转换成xml格式
          "text": 将服务器端返回的内容转换成普通文本格式
          "html": 将服务器端返回的内容转换成普通文本格式，在插入DOM中时，如果包含JavaScript标签，则会尝试去执行。
        "script": 尝试将返回值当作JavaScript去执行，然后再将服务器端返回的内容转换成普通文本格式
          "json": 将服务器端返回的内容转换成相应的JavaScript对象
        "jsonp": JSONP 格式使用 JSONP 形式调用函数时，如 "myurl?callback=?" jQuery 将自动替换 ? 为正确的函数名，以执行回调函数
```

