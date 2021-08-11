---
title: springMVC入门
date: 2021-06-02 15:30:23
tags:
	- SSM
	- springMVC
categories:
	- SSM
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/wallhaven-4gpz9l.jpg
---

# 回顾MVC

## 什么是MVC

- MVC是模型(Model)、视图(View)、控制器(Controller)的简写，是一种软件设计规范。
- 是将业务逻辑、数据、显示分离的方法来组织代码。
- MVC主要作用是**降低了视图与业务逻辑间的双向偶合**。
- MVC不是一种设计模式，**MVC是一种架构模式**。当然不同的MVC存在差异。

**Model（模型）：**数据模型，提供要展示的数据，因此包含数据和行为，可以认为是领域模型或JavaBean组件（包含数据和行为），不过现在一般都分离开来：Value Object（数据Dao） 和 服务层（行为Service）。也就是模型提供了模型数据查询和模型数据的状态更新等功能，包括数据和业务。

**View（视图）：**负责进行模型的展示，一般就是我们见到的用户界面，客户想看到的东西。

**Controller（控制器）：**接收用户请求，委托给模型进行处理（状态改变），处理完毕后把返回的模型数据返回给视图，由视图负责展示。也就是说控制器做了个调度员的工作。

**最典型的MVC就是JSP + servlet + javabean的模式。**

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210602153318.png)

## Model1时代

- 在web早期的开发中，通常采用的都是Model1。
- Model1中，主要分为两层，视图层和模型层。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210602153444.png)

**Model1优点：**架构简单，比较适合小型项目开发；

**Model1缺点：**JSP职责不单一，职责过重，不便于维护；

## Model2时代

Model2把一个项目分成三部分，包括**视图、控制、模型。**

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210602153527.png)

1. 用户发请求
2. Servlet接收请求数据，并调用对应的业务逻辑方法
3. 业务处理完毕，返回更新后的数据给servlet
4. servlet转向到JSP，由JSP来渲染页面
5. 响应给前端更新后的页面

### 职责分析

#### Controller：控制器

1. 取得表单数据
2. 调用业务逻辑
3. 转向指定的页面

#### Model：模型

1. 业务逻辑
2. 保存数据的状态

#### View：视图

1. 就一个 显示页面

> Model2这样不仅提高的代码的复用率与项目的扩展性，且大大降低了项目的维护成本。Model 1模式的实现比较简单，适用于快速开发小规模项目，Model1中JSP页面身兼View和Controller两种角色，将控制逻辑和表现逻辑混杂在一起，从而导致代码的重用性非常低，增加了应用的扩展性和维护的难度。Model2消除了Model1的缺点。

## MVC框架要做哪些事情

1. 将url映射到java类或java类的方法 .
2. 封装用户提交的数据 .
3. 处理请求--调用相关的业务处理--封装响应数据 .
4. 将响应的数据进行渲染 . jsp / html 等表示层数据 .

> 常见的服务器端MVC框架有：Struts、Spring MVC、ASP.NET MVC、Zend Framework、JSF；常见前端MVC框架：vue、angularjs、react、backbone；由MVC演化出了另外一些模式如：MVP、MVVM 等等....

# 回顾Servlet

1. 新建一个Maven工程`springMVC`当做父项目，然后新建一个maven子项目`springMVC01-servlet`，勾选模版`maven-archetype-webapp`

2. 编写`pom.xml`配置文件

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
     <modelVersion>4.0.0</modelVersion>
   
     <groupId>xyz.rtx3090</groupId>
     <artifactId>SpringMVC-Review01</artifactId>
     <version>1.0-SNAPSHOT</version>
     <packaging>war</packaging>
   
     <properties>
       <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
       <maven.compiler.source>1.8</maven.compiler.source>
       <maven.compiler.target>1.8</maven.compiler.target>
     </properties>
   
     <dependencies>
       <dependency>
         <groupId>org.junit.jupiter</groupId>
         <artifactId>junit-jupiter-api</artifactId>
         <version>5.7.0</version>
         <scope>test</scope>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-webmvc</artifactId>
         <version>5.1.9.RELEASE</version>
       </dependency>
       <dependency>
         <groupId>javax.servlet</groupId>
         <artifactId>servlet-api</artifactId>
         <version>2.5</version>
       </dependency>
       <dependency>
         <groupId>javax.servlet.jsp</groupId>
         <artifactId>jsp-api</artifactId>
         <version>2.2</version>
       </dependency>
       <dependency>
         <groupId>javax.servlet</groupId>
         <artifactId>jstl</artifactId>
         <version>1.2</version>
       </dependency>
     </dependencies>
     
   </project>
   ~~~

3. 替换`SpringMVC-Review/SpringMVC-Review01/src/main/webapp/WEB-INF/web.xml`为如下内容

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
   
   
   </web-app>
   ```

4. 在`SpringMVC-Review/SpringMVC-Review01/src/main/webapp/WEB-INF`目录下新建`jsp`目录，然后在`jsp`目录下新建`hello.jsp`文件

   ```jsp
   <%--
     Created by IntelliJ IDEA.
     User: bernardo
     Date: 2021/6/30
     Time: 10:33
     To change this template use File | Settings | File Templates.
   --%>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>helloSpringMVC</title>
   </head>
   <body>
   <%--从Java程序中取数据--%>
   ${msg}
   </body>
   </html>
   ```

5. 完善项目目录结构，如图所示

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210630103957.png)

6. 编写一个Servlet类`HelloSpringMVC`，用来处理用户的请求

   ~~~java
   package xyz.rtx3090.servlet;
   
   import javax.servlet.ServletException;
   import javax.servlet.http.HttpServlet;
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   import java.io.IOException;
   
   public class HelloSpringMVC extends HttpServlet {
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           //获取方法名
           String method = req.getParameter("method");
           //根据不同的方法名设置不同的属性值
           if(method.equals("add")) {
               req.getSession().setAttribute("msg","执行了add方法");
           } else if (method.equals("delete")) {
               req.getSession().setAttribute("msg","执行了delete方法");
           } else {
               req.getSession().setAttribute("msg","执行了其他未知方法");
           }
   
           //业务逻辑代码（暂时我们不需要写）
   
           //视图跳转
           req.getRequestDispatcher("WEB-INF/jsp/hello.jsp").forward(req, resp);
       }
   
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doGet(req,resp);
       }
   }
   ~~~

7. 在web.xml配置文件中注册我们前面写的HelloServet类

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
   
       <servlet>
           <servlet-name>HelloServlet</servlet-name>
           <servlet-class>xyz.rtx3090.servlet.HelloSpringMVC</servlet-class>
       </servlet>
       <servlet-mapping>
           <servlet-name>HelloServlet</servlet-name>
           <url-pattern>/helloServlet</url-pattern>
       </servlet-mapping>
   </web-app>
   ~~~

8. 配置Idea中的Tomcat服务器，并在浏览器输入下面地址，启动测试

   +  `http://localhost:8080/tomcat配置的工程路径/helloServlet?method=add`

     ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210602160352.png)

   + `http://localhost:8080/tomcat配置的工程路径/helloServlet?method=delete`

     ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210602160324.png)

   

# SpringMVC概述

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210602160728.png)

Spring MVC是Spring Framework的一部分，是基于Java实现MVC的轻量级Web框架。

查看官方文档：https://docs.spring.io/spring/docs/5.2.0.RELEASE/spring-framework-reference/web.html#spring-web

**我们为什么要学习SpringMVC呢?**

 Spring MVC的特点：

1. 轻量级，简单易学
2. 高效 , 基于请求响应的MVC框架
3. 与Spring兼容性好，无缝结合
4. 约定优于配置
5. 功能强大：RESTful、数据验证、格式化、本地化、主题等
6. 简洁灵活

Spring的web框架围绕**DispatcherServlet** [ 调度Servlet ] 设计。

DispatcherServlet的作用是将请求分发到不同的处理器。从Spring 2.5开始，使用Java 5或者以上版本的用户可以采用基于注解形式进行开发，十分简洁；

正因为SpringMVC好 , 简单 , 便捷 , 易学 , 天生和Spring无缝集成(使用SpringIoC和Aop) , 使用约定优于配置 . 能够进行简单的junit测试 . 支持Restful风格 .异常处理 , 本地化 , 国际化 , 数据验证 , 类型转换 , 拦截器 等等......所以我们要学习 .

**最重要的一点还是用的人多 , 使用的公司多 .** 

# SpringMVC中心控制器

Spring的web框架围绕DispatcherServlet设计。DispatcherServlet的作用是将请求分发到不同的处理器。从Spring 2.5开始，使用Java 5或者以上版本的用户可以采用基于注解的controller声明方式。

Spring MVC框架像许多其他MVC框架一样, **以请求为驱动** , **围绕一个中心Servlet分派请求及提供其他功能**，**DispatcherServlet是一个实际的Servlet (它继承自HttpServlet 基类)**。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210602161004.png)

SpringMVC的原理如下图所示：

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210602161025.png)

当发起请求时被前置的控制器拦截到请求，根据请求参数生成代理请求，找到请求对应的实际控制器，控制器处理请求，创建数据模型，访问数据库，将模型响应给中心控制器，控制器使用模型与视图渲染视图结果，将结果返回给中心控制器，再将结果返回给请求者。

# SpringMVC执行原理

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210602161139.png)

图为SpringMVC的一个较完整的流程图，实线表示SpringMVC框架提供的技术，不需要开发者实现，虚线表示需要开发者实现。

**简要分析执行流程**

1. DispatcherServlet表示前置控制器，是整个SpringMVC的控制中心。用户发出请求，DispatcherServlet接收请求并拦截请求。

   我们假设请求的url为 : http://localhost:8080/SpringMVC/hello

   

    **如上url拆分成三部分：**

   http://localhost:8080服务器域名

   SpringMVC部署在服务器上的web站点

   hello表示控制器

   通过分析，如上url表示为：请求位于服务器localhost:8080上的SpringMVC站点的hello控制器。

2. HandlerMapping为处理器映射。DispatcherServlet调用HandlerMapping,HandlerMapping根据请求url查找Handler。

3. HandlerExecution表示具体的Handler,其主要作用是根据url查找控制器，如上url被查找控制器为：hello。

4. HandlerExecution将解析后的信息传递给DispatcherServlet,如解析控制器映射等。

5. HandlerAdapter表示处理器适配器，其按照特定的规则去执行Handler。

6. Handler让具体的Controller执行。

7. Controller将具体的执行信息返回给HandlerAdapter,如ModelAndView。

8. HandlerAdapter将视图逻辑名或模型传递给DispatcherServlet。

9. DispatcherServlet调用视图解析器(ViewResolver)来解析HandlerAdapter传递的逻辑视图名。

10. 视图解析器将解析的逻辑视图名传给DispatcherServlet。

11. DispatcherServlet根据视图解析器解析的视图结果，调用具体的视图。

12. 最终视图呈现给用户。

在这里先听一遍原理，不理解没有关系，我们马上来写一个对应的代码实现大家就明白了，如果不明白，那就写10遍，没有笨人，只有懒人！

# 第一个springMVC程序（配置版）

1. 首先创建一个带webapp模版的Maven项目

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210604103405.png)

2. 手动创建文件夹，恢复至标准的maven项目结构

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210604103512.png)

3. 在`WEB-INF`创建`jsp`目录，然后在其下创建`first.jsp`文件

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210604104623.png)

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>first</title>
   </head>
   <body>
   
   </body>
   </html>
   ```

4. 编写pom.xml文件，解决配置项目需要的Jar包依赖问题

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
     <modelVersion>4.0.0</modelVersion>
   
     <groupId>xyz.rtx3090</groupId>
     <artifactId>SpringMVC-Review02</artifactId>
     <version>1.0-SNAPSHOT</version>
     <packaging>war</packaging>
     
     <properties>
       <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
       <maven.compiler.source>1.8</maven.compiler.source>
       <maven.compiler.target>1.8</maven.compiler.target>
     </properties>
   
     <dependencies>
       <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>4.11</version>
         <scope>test</scope>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-webmvc</artifactId>
         <version>5.1.9.RELEASE</version>
       </dependency>
       <dependency>
         <groupId>javax.servlet</groupId>
         <artifactId>servlet-api</artifactId>
         <version>2.5</version>
       </dependency>
       <dependency>
         <groupId>javax.servlet.jsp</groupId>
         <artifactId>jsp-api</artifactId>
         <version>2.2</version>
       </dependency>
       <dependency>
         <groupId>javax.servlet</groupId>
         <artifactId>jstl</artifactId>
         <version>1.2</version>
       </dependency>
     </dependencies>
   
   </project>
   ```
   
5. 在`main->resource`目录下创建并编写SpringMVC配置文件，文件名与下面web.xml配置的`关联springMVC文件中的classpath:`相对应`springMVC-servlet.xml`

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <!--处理映射器-->
       <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
   
       <!--处理器适配器-->
       <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
   
       <!--视图解析器-->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
           <!--前缀-->
           <property name="prefix" value="/WEB-INF/jsp/"/>
           <!--后缀-->
           <property name="suffix" value=".jsp"/>
       </bean>
   
   </beans>
   ```

6. 配置web.xml文件（**注意删除原来的约束头，采用这个约束4.0版的**）

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
   
       <!--注册DispatcherServlet-->
       <servlet>
           <servlet-name>springMVC</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <!--关联springMVC配置文件-->
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:springMVC-servlet.xml</param-value>
           </init-param>
           <!--设置启动级别-->
           <load-on-startup>1</load-on-startup>
       </servlet>
       <!--/ 匹配所有的请求；（不包括.jsp）-->
       <!--/* 匹配所有的请求；（包括.jsp）-->
       <servlet-mapping>
           <servlet-name>springMVC</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   </web-app>
   ```

7. 创建编写实现Controller接口的FirstController类

   ```java
   package xyz.rtx3090.controller;
   import ...
   
   public class FirstController implements Controller {
       @Override
       public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
           ModelAndView modelAndView = new ModelAndView();
   
           //封装对象
           modelAndView.addObject("msg","Hello,world");
           //封装视图
           modelAndView.setViewName("first");//用于进行拼接，组成文件名
   
           return modelAndView;
       }
   }
   ```

8. 将FirstController类配置到SpringMVC配置文件`springMVC-servlet.xml`中

   ```xml
       <!--将Controller类注册到springIOC容器中-->
       <bean id="/first" class="xyz.rtx3090.controller.FirstController"/>
   ```

9. 在`first.jsp`中，取出FirstController类中存入的数据对象

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>first</title>
   </head>
   <body>
   ${msg}
   </body>
   </html>
   ```

10. **一定要重新配置Tomcat**，为其新的项目重新添加一个组件页面

11. 在浏览器地址栏中追加输入`/first`，启动测试（注意配置tomcat选择组件时，不要选择带exploded的，不然启动不了）

    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210604105635.png)

# 第一个springMVC程序（注解版）

1. 首先创建一个带webapp模版的Maven项目

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210604103405.png)

2. 手动创建文件夹，恢复至标准的maven项目结构

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210604103512.png)

3. 在`WEB-INF`创建`jsp`目录，然后在其下创建`first.jsp`文件

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210604104623.png)

4. 编写pom.xml文件，解决配置项目需要的Jar包依赖问题

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
     <modelVersion>4.0.0</modelVersion>
   
     <groupId>xyz.rtx3090</groupId>
     <artifactId>SpringMVC-Review03</artifactId>
     <version>1.0-SNAPSHOT</version>
     <packaging>war</packaging>
   
     <properties>
       <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
       <maven.compiler.source>1.8</maven.compiler.source>
       <maven.compiler.target>1.8</maven.compiler.target>
     </properties>
   
     <dependencies>
       <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>4.11</version>
         <scope>test</scope>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-webmvc</artifactId>
         <version>5.1.9.RELEASE</version>
       </dependency>
       <dependency>
         <groupId>javax.servlet</groupId>
         <artifactId>servlet-api</artifactId>
         <version>2.5</version>
       </dependency>
       <dependency>
         <groupId>javax.servlet.jsp</groupId>
         <artifactId>jsp-api</artifactId>
         <version>2.2</version>
       </dependency>
       <dependency>
         <groupId>javax.servlet</groupId>
         <artifactId>jstl</artifactId>
         <version>1.2</version>
       </dependency>
     </dependencies>
   
   </project>
   ```
   
5. 创建并编写FirstController类

   ```java
   package xyz.rtx3090.Controller;
   import ...
   
   @Controller
   @RequestMapping("FirstController") //这个也可以不写
   public class HelloController {
   
       //映射访问路径, 真实访问地址 : 项目名/FirstController/hello
       @RequestMapping("one")
       public String index(Model model) {
           //向模型中添加属性msg与值，可以在JSP页面中取出并渲染
           model.addAttribute("msg","Hello, Controller");
           //返回视图位置 //web-inf/jsp/hello.jsp
           return "hello";//用于进行拼接，组成文件名
       }
   
       //映射访问路径, 真实访问地址 : 项目名/FirstController/hello
       @RequestMapping("two")
       public String tomcat(Model model) {
           //向模型中添加属性msg与值，可以在JSP页面中取出并渲染
           model.addAttribute("msg","严老板都是个垃圾，是个垃圾～");
           //返回视图位置 //web-inf/jsp/hello.jsp
           return "hello";//与上一个映射的方法返回的值一致，指向同一个视图
       }
   }
   ```

   > - @Controller是为了让Spring IOC容器初始化时自动扫描到；
   > - @RequestMapping是为了映射请求路径，这里因为类与方法上都有映射所以访问时应该是/HelloController/hello；
   > - 方法中声明Model类型的参数是为了把Action中的数据带到视图中；
   > - 方法返回的结果是视图的名称hello，加上配置文件中的前后缀变成WEB-INF/jsp/**first**.jsp。

6. 创建并编写springMVC配置文件`springMVC-servlet`（文件名称与下面web.xml中关联springmvc配置文件的classpath一致）

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
   
   
       <!-- 自动扫描包，让指定包下的注解生效,由IOC容器统一管理 -->
       <context:component-scan base-package="xyz.rtx3090.Controller"/>
       <!-- 让Spring MVC不处理静态资源 -->
       <mvc:default-servlet-handler/>
       <!--
      支持mvc注解驱动
          在spring中一般采用@RequestMapping注解来完成映射关系
          要想使@RequestMapping注解生效
          必须向上下文中注册DefaultAnnotationHandlerMapping
          和一个AnnotationMethodHandlerAdapter实例
          这两个实例分别在类级别和方法级别处理。
          而annotation-driven配置帮助我们自动完成上述两个实例的注入。
       -->
       <mvc:annotation-driven/>
   
       <!-- 视图解析器 -->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
           <!-- 前缀 -->
           <property name="prefix" value="/WEB-INF/jsp/"/>
           <!-- 后缀 -->
           <property name="suffix" value=".jsp"/>
       </bean>
   </beans>
   ```

   > 1. 在视图解析器中我们把所有的视图都存放在/WEB-INF/目录下，这样可以保证视图安全，因为这个目录下的文件，客户端不能直接访问。
   >
   > 2. - 让IOC的注解生效
   >    - 静态资源过滤 ：HTML . JS . CSS . 图片 ， 视频 .....
   >    - MVC的注解驱动
   >    - 配置视图解析器

7. 配置web.xml文件（**注意删除原来的约束头，采用这个约束4.0版的**）

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
   
     <!--注册DispatcherServlet-->
     <servlet>
       <servlet-name>springMVC</servlet-name>
       <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
       <!--通过初始化参数指定SpringMVC配置文件的位置，进行关联-->
       <init-param>
         <param-name>contextConfigLocation</param-name>
         <param-value>classpath:springMVC-servlet.xml</param-value>
       </init-param>
       <!-- 启动顺序，数字越小，启动越早 -->
       <load-on-startup>1</load-on-startup>
     </servlet>
     <!--所有请求都会被springmvc拦截 -->
     <servlet-mapping>
       <servlet-name>springMVC</servlet-name>
       <url-pattern>/</url-pattern>
     </servlet-mapping>
   
   </web-app>
   ```

   > + **/ 和 /\* 的区别：**< url-pattern > / </ url-pattern > 不会匹配到.jsp， 只针对我们编写的请求；即：.jsp 不会进入spring的 DispatcherServlet类 。< url-pattern > /* </ url-pattern > 会匹配 *.jsp，会出现返回 jsp视图 时再次进入spring的DispatcherServlet 类，导致找不到对应的controller所以报404错。
   >
   > 1. + 注意web.xml版本问题，要最新版！
   >
   > 2. - 注册DispatcherServlet
   >    - 关联SpringMVC的配置文件
   >    - 启动级别为1
   >    - 映射路径为 / 【不要用/*，会404】

8. 在`first.jsp`中，取出FirstController类中存入的数据对象

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>first</title>
   </head>
   <body>
   ${msg}
   </body>
   </html>
   ```

9. 配置Tomcat，在浏览器地址栏中追加输入

   + `/FirstController/one`
   + `/FirstController/two`

   我们可以看到，虽然这个两个地址是被我们设置的指向同一个视图，但是显示的页面内容却不同！

   > 我们很多网站的页面复用，都是大概这个复用的。

## 小结

实现步骤其实非常的简单：

1. 新建一个web项目
2. 导入相关jar包
3. 编写web.xml , 注册DispatcherServlet
4. 编写springmvc配置文件
5. 接下来就是去创建对应的控制类 , controller
6. 最后完善前端视图和controller之间的对应
7. 测试运行调试.

使用springMVC必须配置的三大件：

**处理器映射器、处理器适配器、视图解析器**

通常，我们只需要**手动配置视图解析器**，而**处理器映射器**和**处理器适配器**只需要开启**注解驱动**即可，而省去了大段的xml配置

# Controller控制器

## Controller概述

- 控制器复杂提供访问应用程序的行为，通常通过接口定义或注解定义两种方法实现。
- 控制器负责解析用户的请求并将其转换为一个模型。
- 在Spring MVC中一个控制器类可以包含多个方法
- 在Spring MVC中，对于Controller的配置方式有很多种

```java
//实现该接口的类获得控制器功能
public interface Controller {
   //处理请求且返回一个模型与视图对象
   ModelAndView handleRequest(HttpServletRequest var1, HttpServletResponse var2) throws Exception;
}
```

通过源码我们可以看到Controller是一个接口，在org.springframework.web.servlet.mvc包下，接口中只有一个方法；

## Controller实现（配置版）

同上`第一个springMVC程序（配置版）`

## Controller实现（注解版）

同上`第一个springMVC程序（注解版）`

# @RequestMapping

## 概述

- @RequestMapping注解用于映射url到控制器类或一个特定的处理程序方法。可用于类或方法上。用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径。
- 为了测试结论更加准确，我们可以加上一个项目名测试 myweb

## 只注解在方法上面

```java
@Controller
public class TestController {
   @RequestMapping("/h1")
   public String test(){
       return "test";
  }
}
//访问路径：http://localhost:8080 / 项目名 / h1
```

## 同时注解类与方法

```java
@Controller
@RequestMapping("/admin")
public class TestController {
   @RequestMapping("/h1")
   public String test(){
       return "test";
  }
}
//访问路径：http://localhost:8080 / 项目名/ admin /h1  , 需要先指定类的路径再指定方法的路径；
```

# RestFul风格

## 概述

Restful就是一个资源定位及资源操作的风格。不是标准也不是协议，只是一种风格。基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存等机制。

## 功能

资源：互联网所有的事物都可以被抽象为资源

资源操作：使用POST、DELETE、PUT、GET，使用不同方法对资源进行操作。

分别对应 添加、 删除、修改、查询。

### 传统方式操作资源

过不同的参数来实现不同的效果！方法单一，post 和 get

​	http://127.0.0.1/item/queryItem.action?id=1 查询,GET

​	http://127.0.0.1/item/saveItem.action 新增,POST

​	http://127.0.0.1/item/updateItem.action 更新,POST

​	http://127.0.0.1/item/deleteItem.action?id=1 删除,GET或POST

### 使用RESTful操作资源

可以通过不同的请求方式来实现不同的效果！如下：请求地址一样，但是功能可以不同！

​	http://127.0.0.1/item/1 查询,GET

​	http://127.0.0.1/item 新增,POST

​	http://127.0.0.1/item 更新,PUT

​	http://127.0.0.1/item/1 删除,DELETE

## 代码测试

1.  拷贝`第一个springMVC程序（注解版）`项目代码

2. 在其中的Controller类中增加下面方法

   ```java
       @RequestMapping("three/{x}/{y}")
       public String restFul(@PathVariable int x, @PathVariable int y, Model model) {
           int result = x + y;
           //Spring MVC会自动实例化一个Model对象用于向视图中传值
           model.addAttribute("msg","结果：" + result);
           //返回视图位置
           return "hello";
       }
   ```

3. 配置tomcat，在浏览器中输入`localhost:8080/工程名/类映射/three/1/2`，查看页面显示结果

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210604162759.png)

4. 我们来修改下对应的参数类型，再次测试

   ```java
       @RequestMapping("four/{x}/{y}")
       public String restFul02(@PathVariable int x, @PathVariable String y, Model model) {
           //Spring MVC会自动实例化一个Model对象用于向视图中传值
           model.addAttribute("msg","结果："+ x + y);
           //返回视图位置
           return "hello";
       }
   ```

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210604163836.png)

## 使用method属性指定请求类型

**用于约束请求的类型，可以收窄请求范围。指定请求谓词的类型如GET, POST, HEAD, OPTIONS, PUT, PATCH, DELETE, TRACE等**

1. 我们来增加一个方法进行测试

   ```java
   @RequestMapping(value = "five", method = {RequestMethod.POST})
   public String restFul03(Model model) {
       model.addAttribute("msg","我是诺手狗！");
       return "hello";
   }
   ```

2. 我们使用浏览器地址栏进行访问默认是Get请求，会报错405：

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210604164610.png)

3. 如果将POST修改为GET则正常了

   ```java
       @RequestMapping(value = "five", method = {RequestMethod.GET})
       public String restFul03(Model model) {
           model.addAttribute("msg","我是诺手狗！");
           return "hello";
       }
   ```

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210604164959.png)

## 小结

Spring MVC 的 @RequestMapping 注解能够处理 HTTP 请求的方法, 比如 GET, PUT, POST, DELETE 以及 PATCH。

**所有的地址栏请求默认都会是 HTTP GET 类型的。**

方法级别的注解变体有如下几个：组合注解

```
@GetMapping
@PostMapping
@PutMapping
@DeleteMapping
@PatchMapping
```

@GetMapping 是一个组合注解，平时使用的会比较多！

它所扮演的是 @RequestMapping(method =RequestMethod.GET) 的一个快捷方式。

## 思考

**使用路径变量有什么好处？**

- 使路径变得更加简洁；
- 获得参数更加方便，框架会自动进行类型转换。
- 通过路径变量的类型可以约束访问参数，如果类型不一样，则访问不到对应的请求方法，如这里访问是的路径是/commit/1/a，则路径与方法不匹配，而不会是参数转换失败。

#  结果跳转方式

## ModelAndView

设置ModelAndView对象 , 根据view的名称 , 和视图解析器跳到指定的页面 .

**页面 : {视图解析器前缀} + viewName +{视图解析器后缀}**

```xml
<!-- 视图解析器 -->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
     id="internalResourceViewResolver">
   <!-- 前缀 -->
   <property name="prefix" value="/WEB-INF/jsp/" />
   <!-- 后缀 -->
   <property name="suffix" value=".jsp" />
</bean>
```

**对应的controller类**

```java
public class ControllerTest1 implements Controller {

   public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
       //返回一个模型视图对象
       ModelAndView mv = new ModelAndView();
       mv.addObject("msg","ControllerTest1");
       mv.setViewName("test");
       return mv;
  }
}
```

# ServletAPI

通过设置ServletAPI , 不需要视图解析器 .

1、通过HttpServletResponse进行输出

2、通过HttpServletResponse实现重定向

3、通过HttpServletResponse实现转发