SpringMVC入门详细版

---

# SpringMVC概述

1. SpringMVC 也叫 Spring web mvc，是 Spring 框架的一部分，是在 Spring3.0 后发布的。
2. 基于 MVC 架构，功能分工明确。解耦合，
3. 容易理解，上手快，使用简单。就可以开发一个注解的 SpringMVC 项目，SpringMVC 也是轻量级的，jar 很小。不依赖的特定的接口和类
4. 作为 Spring 框架一部分，能够使用 Spring 的 IoC 和 Aop 。方便整合 Strtus,MyBatis,Hiberate,JPA 等其他框架。
5. SpringMVC 强化注解的使用，在控制器，Service，Dao 都可以使用注解，方便灵活。使用@Controller 创建处理器对象,@Service 创建业务对象，@Autowired 或者@Resource在控制器类中注入 Service, Service 类中注入 Dao

#  第一个注解的SpringMVC程序

## 介绍

所谓 SpringMVC 的注解式开发是指，在代码中通过对类与方法的注解，便可完成处理器在 springmvc 容器的注册。注解式开发是重点。

## 需求

用户提交一个请求，服务端处理器在接收到这个请求后，给出一条欢迎信息，在响应页面中显示该信息。

## 实现

1. 创建一个带`maven-archetype-webapp`模版的maven项目，工程结构目录如图所示：

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210717160033.png)

   

2. 配置`pom.xml`文件，导入所依赖Jar包

   ```xml
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
               <groupId>javax.servlet</groupId>
               <artifactId>javax.servlet-api</artifactId>
               <version>3.1.0</version>
               <scope>provided</scope>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-webmvc</artifactId>
               <version>5.2.5.RELEASE</version>
           </dependency>
           <dependency>
               <groupId>javax.servlet.jsp</groupId>
               <artifactId>jsp-api</artifactId>
               <version>2.2</version>
           </dependency>
       </dependencies>
   ```

3. 编写servlet配置文件`web.xml`

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
               <param-value>classpath:applicationContext.xml</param-value>
           </init-param>
           <!--设置启动级别-->
           <load-on-startup>1</load-on-startup>
       </servlet>
       <servlet-mapping>
           <servlet-name>springMVC</servlet-name>
           <url-pattern>*.do</url-pattern>
       </servlet-mapping>
   </web-app>
   ```

4. 编写springMVC配置文件`applictionContext.xml`

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
   
       <!--注解扫描-->
       <context:component-scan base-package="xyz.rtx3090.controller"/>
   
   
       <!--注册视图解析器
           帮助我们处理视图的路径和扩展名，生成视图对象-->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
           <!--前缀：表示视图所在的路径-->
           <property name="prefix" value="/WEB-INF/jsp/"/>
           <!--后缀：表示视图文件的扩展名-->
           <property name="suffix" value=".jsp"/>
       </bean>
   </beans>
   ```

5. 编写Controller类`MyController`

   ```java
   package xyz.rtx3090.controller;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.servlet.ModelAndView;
   
   @Controller
   public class MyController {
       //映射访问路径：http://localhost:8080/项目名/hello.do
       @RequestMapping(value = "hello.do")
       public ModelAndView doSome() {
           System.out.println("处理some.do请求");
           //调用service处理请求，把处理结果放入到返回值ModelAndView
           ModelAndView mv = new ModelAndView();
           mv.addObject("msg","使用注解的SpringMVC应用");
           mv.addObject("fun","doSome");
           mv.setViewName("hello");
           return mv;
       }
   }
   ```

6. 配置Tomcat服务器，启动服务器，在浏览器地址栏输入`http://localhost:8080/项目名/hello.do`进行测试（测试成功，结果如下）

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210717160913.png)

> form表单的 action属性地址不能有/

## 分析

### 全限定性类名

该中央调度器为一个`Servlet`，名称为`DispatcherServlet`。中央调度器的全限定性类名在导入的 Jar 文件`spring-webmvc-5.2.5.RELEASE.jar`的第一个包 `org.springframework.web.servlet`下可找到。

### load-on-startup标签

在`<servlet/>`中添加`<load-on-startup/>`的作用是，标记是否在 Web 服务器(这里是 Tomcat) 启动时会创建这个 Servlet 实例，即是否在 Web 服务器启动时调用执行该 Servlet 的 init()方 法，而不是在真正访问时才创建(它的值必须是一个整数)。

1. 当值大于等于0时，表示容器在启动时就加载并初始化这个servlet，数值越小，该Servlet的优先级就越高，其被创建的也就越早;
2. 当值小于0或者没有指定时，则表示该Servlet在真正被使用时才会去创建。
3. 当值相同时，容器会自己选择创建顺序

### url-pattern标签

对于`<url-pattern/>`，可以写为`/`，建议写为`*.do`的形式，表示拦截以`.do`结尾的请求

### 配置文件位置与名称

注册完毕后，可直接在服务器上发布运行。此时，访问浏览器页面，控制台均会抛出`FileNotFoundException`异常。即默认要从项目根下的 WEB-INF 目录下找名称为 Servlet 名称`-servlet.xml`的配置文件。这里的“Servlet 名称”指的是注册中央调度器`<servlet-name/>`标签中指定的 Servlet 的 name 值。本例配置文件名为`springmvc-servlet.xml`。

而一般情况下，配置文件是放在类路径下，即 `resources` 目录下。所以在注册中央调度器时，还需要使用`init-param`标签为中央调度器设置查找 SpringMVC 配置文件路径，及文件名。

```xml
    <!--注册DispatcherServlet-->
    <servlet>
        <servlet-name>springMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--关联springMVC配置文件-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:applicationContext.xml</param-value>
        </init-param>
        <!--设置启动级别-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springMVC</servlet-name>
        <url-pattern>*.do</url-pattern>
    </servlet-mapping>
```

打开 `DispatcherServlet` 的源码，其继承自`FrameworkServlet`，而该类中有一个属性 `contextConfigLocation`，用于设置 SpringMVC 配置文件的路径及文件名。该初始化参数的属性 就来自于这里。

### Controller处理器

在类上与方法上添加相应注解即可。

`@Controller`: 表示当前类为处理器

`@RequestMapping`: 表示当前方法为处理器方法。

该方法要对 value 属性所指定的 URI进行处理与响应。被注解的方法的方法名可以随意。

若有多个请求路径均可匹配该处理器方法的执行，则`@RequestMapping` 的 value 属性中 可以写上一个数组。

ModelAndView 类中的 `addObject()`方法用于向其 Model 中添加数据。Model 的底层为一 个 HashMap。

Model 中的数据存储在 request 作用域中，SringMVC 默认采用转发的方式跳转到视图， 本次请求结束，模型中的数据被销毁。

### 使用SpringMVC框架web请求处理顺序

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210717164147.png)

# SpringMVC执行流程

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210717164337.png)

1. 浏览器提交请求到中央调度器
2. 中央调度器直接将请求转给处理器映射器
3. 处理器映射器会根据请求，找到处理该请求的处理器，并将其封装为处理器执行链后 返回给中央调度器
4. 中央调度器根据处理器执行链中的处理器，找到能够执行该处理器的处理器适配器
5. 处理器适配器调用执行处理器
6. 处理器将处理结果及要跳转的视图封装到一个对象`ModelAndView`中，并将其返回给处理器适配器
7. 处理器适配器直接将结果返回给中央调度器
8. 中央调度器调用视图解析器，将`ModelAndView`中的视图名称封装为视图对象
9. 视图解析器将封装了的视图对象返回给中央调度器
10. 中央调度器调用视图对象，让其自己进行渲染，即进行数据填充，形成响应对象
11. 中央调度器响应浏览器

# SpringMVC注解式开发

## `@RequestMapping`定义请求规则

### 指定模块名称

#### 介绍

通过`@RequestMapping`注解可以定义处理器对于请求的映射规则。该注解可以注解在方法上，也可以注解在类上，但意义是不同的。value 属性值常以“/”开始。

`@RequestMapping`的 value 属性用于定义所匹配请求的 URI。但对于注解在方法上与类上，其 value 属性所指定的 URI，意义是不同的

一个`@Controller`所注解的类中，可以定义多个处理器方法。当然，不同的处理器方法 所匹配的 URI 是不同的。这些不同的 URI 被指定在注解于方法之上的`@RequestMapping` 的 value 属性中。但若这些请求具有相同的 URI 部分，则这些相同的 URI，可以被抽取到注解在 类之上的`@RequestMapping`的 value 属性中。此时的这个 URI 表示模块的名称。URI 的请求 是相对于 Web 的根目录

换个角度说，要访问处理器的指定方法，必须要在方法指定 URI 之前加上处理器类前定义的模块名称

#### 实现

1. 修改`第一个注解的SpringMVC程序`中的`MyController`类

   ```java
   package xyz.rtx3090.controller;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.servlet.ModelAndView;
   
   @Controller
   @RequestMapping("test")
   public class MyController {
       //映射访问路径：http://localhost:8080/项目名/test/hello.do
       @RequestMapping(value = "hello.do")
       public ModelAndView doSome() {
           System.out.println("处理some.do请求");
           //调用service处理请求，把处理结果放入到返回值ModelAndView
           ModelAndView mv = new ModelAndView();
           mv.addObject("msg","使用注解的SpringMVC应用");
           mv.addObject("fun","doSome");
           mv.setViewName("hello");
           return mv;
       }
       
       //映射访问路径：http://localhost:8080/项目名/test/world.do
       @RequestMapping(value = "world.do")
       public ModelAndView doWorld() {
           System.out.println("处理world.do请求");
           ModelAndView mv = new ModelAndView();
           mv.addObject("msg","使用注解的SpringMVC应用");
           mv.addObject("fun","doWorld");
           mv.setViewName("hello");
           return mv;
       }
   }
   ```

2. 配置并启动tomcat服务器，在地址栏中分别输入`http://localhost:8080/项目名/test/hello.do`和`http://localhost:8080/项目名/test/world.do`

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210717170355.png)

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210717170418.png)

### 对请求提交方式的定义

#### 介绍

对于`@RequestMapping`，其有一个属性`method`，用于对被注解方法所处理请求的提交方式进行限制，即只有满足该`method`属性指定的提交方式的请求，才会执行该被注解方法。

`Method`属性的取值为`RequestMethod`枚举常量。常用的为`RequestMethod.GET`与`RequestMethod.POST`，分别表示提交方式的匹配规则为`GET`与`POST`提交

客户端浏览器常用的请求方式，及其提交方式有以下几种：

|    请求方式     |          提交方式           |
| :-------------: | :-------------------------: |
|    表单请求     | 默认`GET`，可以指定为`POST` |
|    AJAX请求     | 默认`GET`，可以指定为`POST` |
|   地址栏请求    |          `GET`请求          |
|   超链接请求    |          `GET`请求          |
| src资源路径请求 |          `GET`请求          |

也就是说，只要指定了处理器方法匹配的请求提交方式为`POST`，则相当于指定了请求发送的方式: 要么使用表单请求，要么使用`AJAJ`请求。其它请求方式被禁用

当然，若不指定`method`属性，则无论是 GET 还是 POST 提交方式，均可匹配。即对于请求的提交方式无要求

#### 实现

1. 在上个`指定模块名称`的基础上进行修改代码`MyController `

   ```java
   package xyz.rtx3090.controller;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RequestMethod;
   import org.springframework.web.servlet.ModelAndView;
   
   @Controller
   @RequestMapping("test")
   public class MyController {
       //映射访问路径：http://localhost:8080/项目名/test/hello.do
       @RequestMapping(value = "hello.do",method = RequestMethod.GET)
       public ModelAndView doSome() {
           System.out.println("处理some.do请求");
           //调用service处理请求，把处理结果放入到返回值ModelAndView
           ModelAndView mv = new ModelAndView();
           mv.addObject("msg","使用注解的SpringMVC应用");
           mv.addObject("fun","doSome");
           mv.setViewName("hello");
           return mv;
       }
   
       //映射访问路径：http://localhost:8080/项目名/test/world.do
       @RequestMapping(value = "world.do", method = RequestMethod.POST)
       public ModelAndView doWorld() {
           System.out.println("处理world.do请求");
           ModelAndView mv = new ModelAndView();
           mv.addObject("msg","使用注解的SpringMVC应用");
           mv.addObject("fun","doWorld");
           mv.setViewName("hello");
           return mv;
       }
   }
   ```

2. 修改`index.jsp`文件

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <body>
   <h2>Hello World!</h2>
   
   <a href="/SpringMVC_Review09_war/test/hello.do">以get方式跳转到some页面</a><br>
   <a href="/SpringMVC_Review09_war/test/world.do">以get方式跳转到world页面</a>
   
   <form action="/SpringMVC_Review09_war/test/hello.do" method="post">
       <input type="submit" value="以post方式跳转到some页面">
   </form>
   
   <form action="/SpringMVC_Review09_war/test/world.do" method="post">
       <input type="submit" value="以post方式跳转到world页面">
   </form>
   
   </body>
   </html>
   ```

3. 配置并启动tomcat服务器，测试结果如图所示：

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210717182137.png)

## 处理器方法的参数

### 概述

处理器方法可以包含以下四类参数，这些参数会在系统调用时由系统自动赋值，即程序员可在方法内直接使用。

1. `HttpServletRequest`
2. `HttpServletResponse`
3. `HttpSession`
4. 请求中所携带的请求参数

### 逐个接收参数

只要保证**请求参数名**与该**请求处理方法的参数名**相同即可

1. 在上个`@RequestMapping`代码的基础上进行，修改`index.jsp`页面

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <body>
   
   <form action="/SpringMVC_Review09_war/test/register.do" method="post">
       姓名：<input type="text" name="name"/><br>
       年龄：<input type="text" name="age"/><br>
       <input type="submit" value="注册">
   </form>
   
   </body>
   </html>
   ```

2. 修改处理类`MyController`

   ```java
   package xyz.rtx3090.controller;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.servlet.ModelAndView;
   
   @Controller
   @RequestMapping("test")
   public class MyController {
       @RequestMapping(value = "register.do")
       public ModelAndView register(String name, Integer age) {
           System.out.println("age: " + age);
           System.out.println("name: " + name);
           ModelAndView mv = new ModelAndView();
           mv.addObject("name",name);
           mv.addObject("age",age);
           mv.setViewName("hello");
           return mv;
       }
   }
   ```

3. 修改`/jsp/hello.jsp`页面

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>HelloWord</title>
   </head>
   <body>
   
   <h2>${name}</h2>
   <h2>${age}</h2>
   
   </body>
   </html>
   ```

### 请求参数中文乱码问题

#### 介绍

对于前面所接收的请求参数，若含有中文，则会出现中文乱码问题。Spring 对于请求参数中的中文乱码问题，给出了专门的字符集过滤器:`spring-web-5.2.5.RELEASE.jar` 的`org.springframework.web.filter`包下的`CharacterEncodingFilter`类。

#### 代码实现

在 web.xml 中注册字符集过滤器，即可解决 Spring 的请求参数的中文乱码问题。不过最好将该过滤器注册在其它过滤器之前，因为过滤器的执行是按照其注册顺序进行的。

1. 代码直接在`逐个接收参数`的基础上进行，修改`web.xml`文件，添加上下列内容

   ```xml
       <!--注册过滤器
           解决post请求乱码的问题-->
       <filter>
           <filter-name>characterEncodingFilter</filter-name>
           <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
           <!--指定字符集-->
           <init-param>
               <param-name>encoding</param-name>
               <param-value>utf-8</param-value>
           </init-param>
           <!--强制request使用字符集encoding-->
           <init-param>
               <param-name>forceRequestEncoding</param-name>
               <param-value>true</param-value>
           </init-param>
           <!--强制response使用字符集encoding-->
           <init-param>
               <param-name>forceResponseEncoding</param-name>
               <param-value>true</param-value>
           </init-param>
       </filter>
       <filter-mapping>
           <filter-name>characterEncodingFilter</filter-name>
           <url-pattern>/*</url-pattern>
       </filter-mapping>
   ```

### 校正请求参数名`@RequestParam`

#### 介绍

所谓校正请求参数名，是指若请求 URL 所携带的参数名称与处理方法中指定的参数名不相同时，则需在处理方法参数前，添加一个注解`@RequestParam(`“请求参数名”)，指定请求 URL 所携带参数的名称。该注解是对处理器方法参数进行修饰的。

+ `value`：属性指定请求参数的名称
+ `required`：规定请求是否必须包含此参数，默认为true，即必须包含此参数

#### 代码实现

1. 还是在上个代码的基础上进行，修改`index.jsp`页面中的`name`属性

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <body>
   
   <form action="/SpringMVC_Review09_war/test/register.do" method="post">
       姓名：<input type="text" name="aName"/><br>
       年龄：<input type="text" name="aAge"/><br>
       <input type="submit" value="注册">
   </form>
   
   </body>
   </html>
   ```

2. 修改处理器类`MyController`，在方法形参上加上注解

   ```java
   package xyz.rtx3090.controller;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RequestParam;
   import org.springframework.web.servlet.ModelAndView;
   
   @Controller
   @RequestMapping("test")
   public class MyController {
       @RequestMapping(value = "register.do")
       public ModelAndView register(@RequestParam(value = "aName") String name, @RequestParam(value = "aAge") Integer age) {
           System.out.println("age: " + age);
           System.out.println("name: " + name);
           ModelAndView mv = new ModelAndView();
           mv.addObject("name",name);
           mv.addObject("age",age);
           mv.setViewName("hello");
           return mv;
       }
   }
   ```

3. 配置并启动tomcat服务器，参数被成功取了出来

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210717234101.png)

### 对象接收参数

#### 介绍

将处理器方法的参数定义为一个对象，只要保证请求参数名与这个对象的属性同名即可。

#### 代码实现

1. 同样是在上一个代码的基础上进行，定义一个参数类`Person`

   ```java
   package xyz.rtx3090.parameter;
   
   public class Person {
       private String name;
       private int age;
   
       //constructor
   
       //setter and getter
   }
   ```

2. 修改处理器类`MyController`

   ```java
   package xyz.rtx3090.controller;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.servlet.ModelAndView;
   import xyz.rtx3090.parameter.Person;
   
   @Controller
   @RequestMapping("test")
   public class MyController {
       @RequestMapping(value = "register.do")
       public ModelAndView register(Person person) {
           System.out.println("age: " + person.getAge());
           System.out.println("name: " + person.getName());
           ModelAndView mv = new ModelAndView();
           mv.addObject("name", person.getName());
           mv.addObject("age", person.getAge());
           mv.setViewName("hello");
           return mv;
       }
   }
   ```

3. 修改`index.jsp`页面的name属性恢复至正确的

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <body>
   
   <form action="/SpringMVC_Review09_war/test/register.do" method="post">
       姓名：<input type="text" name="name"/><br>
       年龄：<input type="text" name="age"/><br>
       <input type="submit" value="注册">
   </form>
   
   </body>
   </html>
   ```

4. 修改`/jsp/hello.jsp`页面

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>HelloWord</title>
   </head>
   <body>
   
   <h2>${name}</h2>
   <h2>${age}</h2>
   <h2>${Person}</h2>
   
   </body>
   </html>
   ```

5. 配置并启动tomcat服务器，测试成功，成功的取出参数

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210717235653.png)

## 处理器方法的返回值

### 介绍

使用@Controller 注解的处理器的处理器方法，其返回值常用的有四种类型

1. `ModelAndView`
2. `String`
3. `void`
4. 返回自定义类型对象

根据不同的情况，使用不同的返回值。

### 返回 ModelAndView

若处理器方法处理完后，需要跳转到其它资源，且又要在跳转的资源间传递数据，此时处理器方法返回 ModelAndView 比较好。当然，若要返回 ModelAndView，则处理器方法中需要定义 ModelAndView 对象

在使用时，若该处理器方法只是进行跳转而不传递数据，或只是传递数据而并不向任何 资源跳转(如对页面的 Ajax 异步响应)，此时若返回 ModelAndView，则将总是有一部分多 余:要么 Model 多余，要么 View 多余。即此时返回 ModelAndView 将不合适

### 返回 String

#### 介绍

1. 处理器方法返回的字符串为**指定逻辑视图名**，通过视图解析器解析可以将其转换为物理视图地址
2. 处理器方法返回的字符串为**单纯的字符串数据**，这就需要我们再处理器方法上加入`@ResponseBody`注解

#### 代码实现一：返回的字符串为指定逻辑视图名

1. 还是在上个代码的基础上进行，修改处理器`MyController`

   ```java
   package xyz.rtx3090.controller;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   import xyz.rtx3090.parameter.Person;
   
   @Controller
   @RequestMapping("test")
   public class MyController {
       @RequestMapping(value = "register.do")
       public String register(Person person) {
           //只进行页面跳转，不传递数据
           return "hello";
       }
   }
   ```

2. 配置并启动tomcat服务器，页面跳转成功，但没有取出参数

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210718000702.png)

#### 代码实现二：返回的字符串为单纯的字符串数据

1. 还是在上个代码的基础上进行，修改处理器`MyController`

   ```java
   package xyz.rtx3090.controller;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.ResponseBody;
   import xyz.rtx3090.pojo.Student;
   
   @Controller
   @RequestMapping(value = "/test")
   public class MyController {
     	//注意produces用来解决中文乱码问题
       @RequestMapping(value = "/perfect.do", produces = "text/plain;charset=utf-8")
       @ResponseBody
       public String perfect() {
           return "String对象";
       }
   }
   ```

### 返回void

#### 介绍

对于处理器方法返回 void 的应用场景，AJAX 响应.

若处理器对请求处理后，无需跳转到其它任何资源，此时可以让处理器方法返回 void。

例如，对于 AJAX 的异步请求的响应。

#### 代码实现

1. 新建带`maven-archetype-webapp`模版的maven项目，工程目录结构如图所示：

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210728171308.png)

   

   

2. 配置`pom.xml`文件，导入依赖Jar包（注意各个Jar包的版本，很容易出现不兼容的情况，最好抄我的）

   ```xml
   		<dependency>
         <groupId>javax.servlet</groupId>
         <artifactId>javax.servlet-api</artifactId>
         <version>3.1.0</version>
         <scope>provided</scope>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-webmvc</artifactId>
         <version>5.2.5.RELEASE</version>
       </dependency>
       <dependency>
         <groupId>javax.servlet.jsp</groupId>
         <artifactId>jsp-api</artifactId>
         <version>2.2</version>
       </dependency>
       <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
       <dependency>
         <groupId>com.fasterxml.jackson.core</groupId>
         <artifactId>jackson-databind</artifactId>
         <version>2.12.3</version>
       </dependency>
   ```

3. 编写前端页面`index.jsp`，使用JQuery发送AJAX请求（注意引入JQuery）

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
     	<%--引入JQuery文件--%>
       <script src="https://code.jquery.com/jquery-3.6.0.js" integrity="sha256-H+K7U5CnXl1h5ywQfKtSj8PCmoN9aaq30gDh27Xc0jk=" crossorigin="anonymous"></script>
       <script type="text/javascript">
           $(function(){
               $("#submit").click(function(){
                   $.ajax({
                       url:"hello.do",
                       data:{
                           name:$("#name").val(),
                           age:$("#age").val()
                       },
                       type:"post",
                       dataType:"json",
                       success:function(resp) {
                           alert("resp: " + resp.name + ","+resp.age);
                       }
                   })
               })
           })
       </script>
   </head>
   <body>
   <h2>index.jsp</h2>
   <table>
       <tr>
           <td>名字：</td>
           <td>
               <input type="text" id="name">
           </td>
       </tr>
       <tr>
           <td>年龄：</td>
           <td>
               <input type="text" id="age">
           </td>
       </tr>
       <tr>
           <td>
               <input type="submit" value="提交" id="submit">
           </td>
       </tr>
   </table>
   </body>
   </html>
   ```

4. 编写实体类`Student`，用于存入前端页面发送来的数据

   ```java
   package xyz.rtx3090.pojo;
   
   public class Student {
       private String name;
       private Integer age;
   
       //constructor
       //setter and getter
   		//toString
   }
   ```

5. 编写controller类`FirstController`，处理前端页面发送的数据

   ```java
   package xyz.rtx3090.controller;
   
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   import xyz.rtx3090.pojo.Student;
   
   import javax.servlet.http.HttpServletResponse;
   import java.io.IOException;
   import java.io.PrintWriter;
   
   @Controller
   public class FirstController {
       /**
        * 处理器返回值为void，表示不传递数据，也没有跳转视图
        * 但是我们可以收到的传递数据，使用HttpServletResponse来输出数据到浏览器
        *
        * @param name 接受到到前端页面发送过来的参数name
        * @param age  接受到到前端页面发送过来的参数age
        */
       @RequestMapping(value = "hello.do")
       public void hello(String name, Integer age, HttpServletResponse resp) {
           System.out.println("执行了hello方法");
           //把接受到的参数存入一个自定义对象中
           Student student = new Student();
           if (name != null && !("".equals(name)) && age != null && age >= 0) {
               System.out.println("执行了参数存入对象的方法");
               student.setName(name);
               student.setAge(age);
           }
   
           //使用jackson工具库，将student对象转化为json字符串
           String jsonString = "{}";
           if (student != null) {
               ObjectMapper objectMapper = new ObjectMapper();
               try {
                   jsonString = objectMapper.writeValueAsString(student);
               } catch (JsonProcessingException e) {
                   e.printStackTrace();
               }
           }
   
           //使用HttpServletResponse输出数据到浏览器
           try {
               PrintWriter writer = resp.getWriter();
               writer.println(jsonString);
               writer.flush();
               writer.close();
           } catch (IOException e) {
               e.printStackTrace();
           }
       }
   }
   ```

6. 编写springmvc配置文件`springmvc-servlet.xml`

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
   
       <!--配置注解扫描-->
       <context:component-scan base-package="xyz.rtx3090.controller"/>
   
       <!--因为不需要跳转视图，所以就不需要配置视图解析器-->
   </beans>
   ```

7. 编写servlet配置文件`web.xml`

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
   
     <!--注册配置DispatcherServlet中央调度器-->
     <servlet>
       <servlet-name>dispatcherServlet</servlet-name>
       <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
       <!--关联springmvc配置文件-->
       <init-param>
         <param-name>contextConfigLocation</param-name>
         <param-value>classpath:springmvc-servlet.xml</param-value>
       </init-param>
       <!--设置启动级别-->
       <load-on-startup>1</load-on-startup>
     </servlet>
     <servlet-mapping>
       <servlet-name>dispatcherServlet</servlet-name>
       <url-pattern>*.do</url-pattern>
     </servlet-mapping>
   
     <!--注册配置乱码过滤器-->
     <filter>
       <filter-name>characterEncodingFilter</filter-name>
       <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
       <!--指定字符集-->
       <init-param>
         <param-name>encoding</param-name>
         <param-value>utf-8</param-value>
       </init-param>
       <!--强制HttpRequest使用字符集encoding-->
       <init-param>
         <param-name>forceRequestEncoding</param-name>
         <param-value>true</param-value>
       </init-param>
       <!--强制HttpResponse使用字符集encoding-->
       <init-param>
         <param-name>forceResponseEncoding</param-name>
         <param-value>true</param-value>
       </init-param>
     </filter>
     <filter-mapping>
       <filter-name>characterEncodingFilter</filter-name>
       <url-pattern>/*</url-pattern>
     </filter-mapping>
   </web-app>
   ```

8. 配置并启动Tomcat

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210728172448.png)

### 返回对象Object

#### 介绍

如果controller类方法返回的是一个对象（这个 Object 可以是 Integer，String，自定义对象， Map，List 等），框架会自动将其转化为Json对象。

我们只需要在springmvc配置文件中加入`<mvc:annotation-driven/>`注解驱动，并在controller类方法上加入`@ResponseBody`注解，即可。

#### 代码实现

1. 新建带`maven-archetype-webapp`模版的maven项目，工程目录结构如图所示：

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210729132331.png)

2. 配置`pom.xml`文件，导入依赖Jar包（注意各个Jar包的版本，很容易出现不兼容的情况，最好抄我的）

   ```xml
   <dependency>
         <groupId>javax.servlet</groupId>
         <artifactId>javax.servlet-api</artifactId>
         <version>3.1.0</version>
         <scope>provided</scope>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-webmvc</artifactId>
         <version>5.2.5.RELEASE</version>
       </dependency>
       <dependency>
         <groupId>javax.servlet.jsp</groupId>
         <artifactId>jsp-api</artifactId>
         <version>2.2</version>
       </dependency>
       <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
       <dependency>
         <groupId>com.fasterxml.jackson.core</groupId>
         <artifactId>jackson-databind</artifactId>
         <version>2.12.3</version>
       </dependency>
   ```

3. 编写前端页面`index.jsp`，使用JQuery发送AJAX请求（注意引入JQuery）

   ```jsp
   <%--
     Created by IntelliJ IDEA.
     User: bernardo
     Date: 2021/7/29
     Time: 11:42
     To change this template use File | Settings | File Templates.
   --%>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>返回对象</title>
       <%--在线引入JQuery文件--%>
       <script src='https://code.jquery.com/jquery-3.2.1.min.js'></script>
       <script type="text/javascript">
           $(function () {
               $("#submit").click(function () {
                   $.ajax({
                       url: "test/hello.do",
                       data: {
                           name: $("#name").val(),
                           age: $("#age").val()
                       },
                       /*因为返回的数据是Json对象，所以不需要标注数据类型*/
                       success: function (data) {
                           alert("数据：" + data.name + ", " + data.age);
                       }
                   });
               });
           });
       </script>
   </head>
   <body>
   <table>
       <tr>
           <td>名字：</td>
           <td>
               <input type="text" id="name">
           </td>
       </tr>
       <tr>
           <td>年龄：</td>
           <td>
               <input type="text" id="age">
           </td>
       </tr>
       <tr>
           <td>
               <input type="submit" value="提交" id="submit">
           </td>
       </tr>
   </table>
   </body>
   </html>
   ```

4. 编写实体类`Student`，用于存入前端页面发送来的数据

   ```java
   package xyz.rtx3090.pojo;
   
   public class Student {
       private String name;
       private Integer age;
   
       //constructor
       //setter and getter
   		//
   }
   ```

5. 编写controller类`FirstController`，处理前端页面发送的数据

   ```java
   package xyz.rtx3090.controller;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.ResponseBody;
   import xyz.rtx3090.pojo.Student;
   
   @Controller
   @RequestMapping(value = "/test")
   public class MyController {
       @RequestMapping(value = "/hello.do")
       @ResponseBody
       public Student hello(String name, Integer age) {
           System.out.println("执行了hello方法");
           //创建Student对象
           Student student = new Student();
           student.setName(name);
           student.setAge(age);
           //自动转换为Json对象，并返回
           System.out.println("student: " + student);
           return student;
       }
   }
   ```

6. 编写springmvc配置文件`springmvc-servlet.xml`

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context
           https://www.springframework.org/schema/context/spring-context.xsd
           http://www.springframework.org/schema/mvc
           https://www.springframework.org/schema/mvc/spring-mvc.xsd">
   
       <!--配置注解扫描器-->
       <context:component-scan base-package="xyz.rtx3090.controller"/>
   
       <!--因为返回的不是视图对象，所以不需要配置视图解析器-->
   
       <!--注册mvc的注解驱动，
           将Object数据转化为JSON对象
               注意导入xml头文件时，要选择mvc的-->
       <mvc:annotation-driven/>
   </beans>
   ```

7. 编写servlet配置文件`web.xml`

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
   
       <!--注册配置DispatcherServlet中央调度器-->
       <servlet>
           <servlet-name>dispatcherServlet</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <!--关联springmvc配置文件-->
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:springmvc-servlet.xml</param-value>
           </init-param>
           <!--设置启动级别-->
           <load-on-startup>1</load-on-startup>
       </servlet>
       <servlet-mapping>
           <servlet-name>dispatcherServlet</servlet-name>
           <url-pattern>*.do</url-pattern>
       </servlet-mapping>
   
       <!--注册配置乱码过滤器-->
       <filter>
           <filter-name>characterEncoding</filter-name>
           <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
           <!--指定字符集-->
           <init-param>
               <param-name>encoding</param-name>
               <param-value>utf-8</param-value>
           </init-param>
           <!--强制HttpRequest使用字符集encoding-->
           <init-param>
               <param-name>forceRequestEncoding</param-name>
               <param-value>true</param-value>
           </init-param>
           <!--强制HttpResponse使用字符集encoding-->
           <init-param>
               <param-name>forceResponseEncoding</param-name>
               <param-value>true</param-value>
           </init-param>
       </filter>
       <filter-mapping>
           <filter-name>characterEncoding</filter-name>
           <url-pattern>/*</url-pattern>
       </filter-mapping>
   </web-app>
   ```

8. 配置并启动Tomcat

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210729133528.png)

### 返回List集合

#### 介绍

如果Controller类的返回结果为List集合，则框架会自动将其转化为Json对象数组

我们只需要在springmvc配置文件中加入`<mvc:annotation-driven/>`注解驱动，并在controller类方法上加入`@ResponseBody`注解，即可。

#### 代码实现

在上个项目`返回对象Object`的代码基础上进行修改

1. 修改controller类

   ```java
   package xyz.rtx3090.controller;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.ResponseBody;
   import xyz.rtx3090.pojo.Student;
   
   import java.util.ArrayList;
   import java.util.List;
   
   @Controller
   @RequestMapping(value = "/test")
   public class MyController {
       @RequestMapping(value = "/jason.do")
       @ResponseBody
       public List<Student> jason() {
           //创建List对象
           List<Student> students = new ArrayList<>();
           students.add(new Student("王多余",22));
           students.add(new Student("卧龙凤雏",24));
           //将自动转化为Json数组，并返回
           return students;
       }
   }
   ```

2. 修改前端`index.jsp`页面

   ```jsp
   <%--
     Created by IntelliJ IDEA.
     User: bernardo
     Date: 2021/7/29
     Time: 11:42
     To change this template use File | Settings | File Templates.
   --%>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <%--使用EL表达式获取项目根目录--%>
   <%
       String base = request.getContextPath()+"/";
       String url = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+base;
   %>
   <html>
   <head>
       <title>返回对象</title>
       <%--使用base标签规定项目根目录--%>
       <base href="<%=url%>"/>
       <%--在线引入JQuery文件--%>
       <script src='https://code.jquery.com/jquery-3.2.1.min.js'></script>
       <script type="text/javascript">
           $(function () {
               $("button").click(function () {
                   $.ajax({
                       url: "test/jason.do",
                       success: function (data) {
                           $.each(data,function (i,n) {
                               alert("数据：" + n.name + ", " + n.age);
                           })
                       }
                   });
               });
           });
       </script>
   </head>
   <body>
   <button>发起AJAX请求</button>
   </body>
   </html>
   ```

3. 配置并启动Tomcat服务器

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210729145414.png)

# 解决路径问题

## `<servlet-mapping/>`中`<url-pattern/>`的地址配置问题

### 地址路径为`*.do`

在没有特殊要求的情况下，SpringMVC 的中央调度器 DispatcherServlet 的`<url-pattern/>`常使用后辍匹配方式，如写为*.do 或者 *.action, *.mvc 等。

### 地址路径为`/`

当地址为`/`时，我们就可以让所有的资源都被我们的controller处理，但就无法访问静态资源了。

解决静态资源无法访问的问题，有两种解决方法：

#### 方式一

**在springmvc配置文件中加入`<mvc:default-servlet-handler/>`标签**

```xml
<mvc:default-servlet-handler/>
```

> 声 明 了 <mvc:default-servlet-handler /> 后 ， springmvc 框 架 会 在 容 器 中 创 建 DefaultServletHttpRequestHandler 处理器对象。它会像一个检查员，对进入 DispatcherServlet 的 URL 进行筛查，如果发现是静态资源的请求，就将该请求转由 Web 应用服务器默认的 Servlet 处理。一般的服务器都有默认的 Servlet。

#### 方式二

在springmvc配置文件中加入`<mvc:resources/>`标签和`mvc:annotation-driver/>`标签

```xml
<mvc:resources location="/images/" mapping="/images/**"/>

<mvc:annotation-driver/>
```

+ `location`：表示静态资源所在目录（目录不要使用/WEB-INF/及其子目录）
+ `mapping`：表示匹配资源目录文件的规则，`**`表示匹配其子文件或其子目录文件

## 前端页面中，我们如何书写请求地址

1. 方法一：采用EL表达式获取项目根目录

   ```jsp
   <a href="${pageContext.request.contextPath}/test/hello.do">测试</a>
   ```

   缺点：每个请求地址都需要重复EL表达式

2. 方法二：采用`<base/>`标签

   ```jsp
       <%--使用base标签规定项目根目录--%>
       <base href="http://localhost:8080//SpringMVC_Review20_war/"/>
   
   		<%--请求地址中就可以直接写入相对地址--%>
   		<a href="test/hello.do"/>
   ```

   缺点：项目根地方发生变化，就需要手动修改base标签

3. **方法三**：采用`<base/>`标签加jsp脚步

   ```jsp
         <%--使用jsp脚步获取项目根目录--%>
         <%
             String base = request.getContextPath()+"/";
             String url = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+base;
         %>
   
         <%--使用base标签规定项目根目录--%>
         <base href="<%=url%>"/>
   
   		  <%--发起测试请求--%>
   			<a href="test/bernardo.do">测试base标签</a>
   ```


# 整合SSM框架

1. 准备数据库

   ```mysql
   drop database if exists SSM;
   
   create database SSM;
   
   use SSM;
   
   create table `Student`
   (
       `id`   int(11) not null auto_increment,
       `name` varchar(255),
       `age`  int(11),
       primary key (`id`)
   ) ENGINE = InnoDB
     DEFAULT CHARSET = utf8;
   
   insert into Student (name, age)
   VALUES ('Jason', 22),
          ('Bernardo', 24);
   
   select *
   from Student;
   ```

2. 创建新的带`maven-archetype-webapp`模板的maven项目，工程目录结构如图所示：

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210808182403.png)

3. 配置`pom.xml`文件，导入Jar包依赖

   ```xml
       <dependencies>
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.11</version>
               <scope>test</scope>
           </dependency>
   
           <dependency>
               <groupId>javax.servlet</groupId>
               <artifactId>javax.servlet-api</artifactId>
               <version>3.1.0</version>
               <scope>provided</scope>
           </dependency>
           <!-- jsp依赖 -->
           <dependency>
               <groupId>javax.servlet.jsp</groupId>
               <artifactId>jsp-api</artifactId>
               <version>2.2.1-b03</version>
               <scope>provided</scope>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-webmvc</artifactId>
               <version>5.2.5.RELEASE</version>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-tx</artifactId>
               <version>5.2.5.RELEASE</version>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-jdbc</artifactId>
               <version>5.2.5.RELEASE</version>
           </dependency>
           <!--aspectj 依赖-->
           <dependency>
               <groupId>
                   org.springframework
               </groupId>
               <artifactId>
                   spring-aspects
               </artifactId>
               <version>
                   5.2.5.RELEASE
               </version>
           </dependency>
           <dependency>
               <groupId>com.fasterxml.jackson.core</groupId>
               <artifactId>jackson-core</artifactId>
               <version>2.9.0</version>
           </dependency>
           <dependency>
               <groupId>com.fasterxml.jackson.core</groupId>
               <artifactId>jackson-databind</artifactId>
               <version>2.9.0</version>
           </dependency>
           <dependency>
               <groupId>org.mybatis</groupId>
               <artifactId>mybatis-spring</artifactId>
               <version>1.3.1</version>
           </dependency>
           <dependency>
               <groupId>org.mybatis</groupId>
               <artifactId>mybatis</artifactId>
               <version>3.5.1</version>
           </dependency>
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
               <version>5.1.9</version>
           </dependency>
           <dependency>
               <groupId>com.alibaba</groupId>
               <artifactId>druid</artifactId>
               <version>1.1.12</version>
           </dependency>
       </dependencies>
   
       <build>
           <plugins>
               <!--规定项目JDK版本-->
               <plugin>
                   <groupId>org.apache.maven.plugins</groupId>
                   <artifactId>maven-compiler-plugin</artifactId>
                   <version>3.8.0</version>
                   <configuration>
                       <source>1.8</source>
                       <target>1.8</target>
                   </configuration>
               </plugin>
           </plugins>
           <resources>
               <!--解决Maven静态资源过滤问题-->
               <resource>
                   <directory>src/main/java</directory>
                   <includes>
                       <include>**/*.properties</include>
                       <include>**/*.xml</include>
                   </includes>
                   <filtering>true</filtering>
               </resource>
               <resource>
                   <directory>src/main/resources</directory>
                   <includes>
                       <include>**/*.properties</include>
                       <include>**/*.xml</include>
                   </includes>
                   <filtering>true</filtering>
               </resource>
           </resources>
       </build>
   ```

4. 创建`controller`、`dao`、`pojo`、`service`四个包

5. 编写`pojo`包下的`Student`实体类

   ```java
   package xyz.rtx3090.pojo;
   
   public class Student {
       private Integer id;
       private String name;
       private Integer age;
   
       //setter and getter
   
       public Integer getId() {
           return id;
       }
   
       public void setId(Integer id) {
           this.id = id;
       }
   
       public String getName() {
           return name;
       }
   
       public void setName(String name) {
           this.name = name;
       }
   
       public Integer getAge() {
           return age;
       }
   
       public void setAge(Integer age) {
           this.age = age;
       }
   }
   ```

6. 编写`dao`包下的`StudentDao`接口

   ```java
   package xyz.rtx3090.dao;
   
   import xyz.rtx3090.pojo.Student;
   
   import java.util.List;
   
   public interface StudentDao {
       //插入学生信息
       int insertStudent(Student student);
   
       //查询全部学生信息
       List<Student> selectAllStudents();
   }
   ```

7. 编写`service`包下的`StudentService`接口

   ```java
   package xyz.rtx3090.service;
   
   import xyz.rtx3090.pojo.Student;
   
   import java.util.List;
   
   public interface StudentService {
       //添加学生信息
       int addStudent(Student student);
   
       //查询全部学生信息
       List<Student> findAllStudents();
   }
   ```

8. 编写`service/impl`包下的`StudentServiceImpl`类

   ```java
   package xyz.rtx3090.service.impl;
   
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.stereotype.Service;
   import xyz.rtx3090.dao.StudentDao;
   import xyz.rtx3090.pojo.Student;
   import xyz.rtx3090.service.StudentService;
   
   import java.util.List;
   
   @Service(value = "studentService")//通过注解创建service对象
   public class StudentServiceImpl implements StudentService {
       @Autowired//自动装配
       private StudentDao studentDao;
   
       @Override
       public int addStudent(Student student) {
           return studentDao.insertStudent(student);
       }
   
       @Override
       public List<Student> findAllStudents() {
           return studentDao.selectAllStudents();
       }
   }
   ```

9. 编写`controller`包下的`StudentController`类

   ```java
   package xyz.rtx3090.controller;
   
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.ResponseBody;
   import org.springframework.web.servlet.ModelAndView;
   import xyz.rtx3090.pojo.Student;
   import xyz.rtx3090.service.StudentService;
   
   import java.util.List;
   
   @Controller
   @RequestMapping("/student")
   public class StudentController {
       @Autowired
       private StudentService studentService;
   
       /**
        * 添加学习信息
        * @param student 需要添加的学生对象
        * @return 将要跳转的视图和传递的数据
        */
       @RequestMapping(value = "/addStudent.do")
       public ModelAndView addStudent(Student student) {
           ModelAndView modelAndView = new ModelAndView();
           //调用service处理业务，并将结果存入到ModelAndView中
           int result = studentService.addStudent(student);
           if (result == 1) {
               modelAndView.addObject("msg","注册成功");
               modelAndView.setViewName("success");
           } else {
               modelAndView.addObject("msg","注册失败");
               modelAndView.setViewName("fail");
           }
           return modelAndView;
       }
   
       /**
        * 查询全部学生信息（使用AJAX）
        * @return 查询到的所以学生信息集合
        */
       @RequestMapping(value = "/queryAllStudents.do")
       @ResponseBody
       public List<Student> queryAllStudent() {
           List<Student> students = studentService.findAllStudents();
           System.out.println("执行了AJAX请求");
           return students;
       }
   }
   ```

10. 创建`resources/xyz/rtx3090/dao`包，并创建编写mapper文件`StudentDao.xml`

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="xyz.rtx3090.dao.StudentDao">
       <!--插入学生信息-->
       <insert id="insertStudent" parameterType="student">
           insert into SSM.Student(name, age) VALUES (#{name},#{age});
       </insert>
   
       <!--查询全部学生信息-->
       <select id="selectAllStudents" resultType="student">
           select id, name , age from SSM.Student order by id asc;
       </select>
   </mapper>
   ```

11. 创建`resources/configure`包，并创建编写数据库配置文件`jdbc.properties`

    ```properties
    driver=com.mysql.jdbc.Driver
    url=jdbc:mysql://localhost:3306/mybatis?useSSL=false&useUnicode=true&characterEncoding=UTF-8
    username=root
    password=intmain()
    ```

12. 创建编写`resources/configure`包下的mybatis核心配置文件`mybatisConfig.xml`

    ```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE configuration
            PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
            "http://mybatis.org/dtd/mybatis-3-config.dtd">
    <configuration>
        <!--配置别名-->
        <typeAliases>
            <package name="xyz.rtx3090.pojo"/>
        </typeAliases>
    
        <!--指定sql映射文件-->
        <mappers>
            <package name="xyz.rtx3090.dao"/>
        </mappers>
    </configuration>
    ```

13. 创建编写`resources/configure`包下的spring配置文件`applicationContext.xml`

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    
        <!--注册组件扫描器-->
        <context:component-scan base-package="xyz.rtx3090.service"/>
    
        <!--引入数据库属性配置文件-->
        <context:property-placeholder location="classpath:configure/jdbc.properties"/>
    
        <!--配置阿里的Druid数据库链接池-->
        <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
              init-method="init" destroy-method="close">
            <property name="url" value="${url}"/>
            <property name="username" value="${username}"/>
            <property name="password" value="${password}"/>
        </bean>
    
        <!--注册SqlSessionFactoryBean-->
        <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
            <!--关联数据库链接池-->
            <property name="dataSource" ref="dataSource"/>
            <!--关联mybatis核心配置文件-->
            <property name="configLocation" value="classpath:configure/mybatisConfig.xml"/>
        </bean>
    
        <!--动态代理对象-->
        <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
            <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
            <property name="basePackage" value="xyz.rtx3090.dao"/>
        </bean>
    </beans>
    ```

14. 创建编写`resources/configure`包下的springmvc配置文件`springmvc-servlet.xml`

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xmlns:mvc="http://www.springframework.org/schema/mvc"
           xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
    
        <!--注册组件扫描器-->
        <context:component-scan base-package="xyz.rtx3090.controller"/>
    
        <!--指定视图解析器-->
        <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
            <property name="prefix" value="/WEB-INF/view/"/>
            <property name="suffix" value=".jsp"/>
        </bean>
    
        <!--注册注解驱动-->
        <mvc:annotation-driven/>
    </beans>
    ```

15. 编写`web.xml`配置文件，注意修改文件头

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
             version="4.0">
    
        <!--注册spring的监听器-->
        <context-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:configure/applicationContext.xml</param-value>
        </context-param>
        <listener>
            <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
        </listener>
    
        <!--注册springmvc的中央调度器-->
        <servlet>
            <servlet-name>dispatcherServlet</servlet-name>
            <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
            <!--关联springmvc配置文件-->
            <init-param>
                <param-name>contextConfigLocation</param-name>
                <param-value>classpath:configure/springmvc-servlet.xml</param-value>
            </init-param>
            <!--设置启动级别-->
            <load-on-startup>1</load-on-startup>
        </servlet>
        <servlet-mapping>
            <servlet-name>dispatcherServlet</servlet-name>
            <url-pattern>*.do</url-pattern>
        </servlet-mapping>
    
        <!--注册字符集过滤器-->
        <filter>
            <filter-name>characterEncodingFilter</filter-name>
            <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
            <!--指定字符集-->
            <init-param>
                <param-name>encoding</param-name>
                <param-value>utf-8</param-value>
            </init-param>
            <!--强制Request使用encoding字符集-->
            <init-param>
                <param-name>forceRequestEncoding</param-name>
                <param-value>true</param-value>
            </init-param>
            <!--强制Request使用encoding字符集-->
            <init-param>
                <param-name>forceResponseEncoding</param-name>
                <param-value>true</param-value>
            </init-param>
        </filter>
        <filter-mapping>
            <filter-name>characterEncodingFilter</filter-name>
            <url-pattern>/*</url-pattern>
        </filter-mapping>
    </web-app>
    ```

16. 创建`webapp/images`目录，并导入一个图片

17. 编写`index.jsp`文件，作为前端主页

    ```jsp
    <%--
      Created by IntelliJ IDEA.
      User: bernardo
      Date: 2021/7/29
      Time: 20:18
      To change this template use File | Settings | File Templates.
    --%>
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <%
        String base = request.getContextPath()+"/";
        String url = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+base;
    %>
    <html>
    <head>
        <title>Student System</title>
        <%--指定项目根目录--%>
        <base href="<%=url%>"/>
    </head>
    <body>
    <div align="center">
        <h3>SSM整合开发——实现Student表的操作</h3>
        <img src="images/bluePerson.jpeg" width="300" height="300"/>
        <table cellpadding="0" cellspacing="0">
            <tr>
                <td>
                    <a href="addStudent.jsp">注册学生</a>
                </td>
            </tr>
            <tr>
                <td>&nbsp;</td>
            </tr>
            <tr>
                <td>
                    <a href="queryAllStudents.jsp">查询学生</a>
                </td>
            </tr>
        </table>
    </div>
    </body>
    </html>
    ```

18. 编写`addStudent.jsp`文件，作为注册页面

    ```jsp
    <%--
      Created by IntelliJ IDEA.
      User: bernardo
      Date: 2021/7/29
      Time: 21:22
      To change this template use File | Settings | File Templates.
    --%>
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <%
        String base = request.getContextPath()+"/";
        String url = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+base;
    %>
    <html>
    <head>
        <title>添加学生</title>
        <base href="<%=url%>"/>
    </head>
    <body>
    <div align="center">
        <h3>学生注册页面</h3>
        <form action="student/addStudent.do" method="post">
            <table>
                <tr>
                    <td>姓名：</td>
                    <td>
                        <input type="text" name="name">
                    </td>
                </tr>
                <tr>
                    <td>年龄：</td>
                    <td>
                        <input type="text" name="age">
                    </td>
                </tr>
                <tr>
                    <td>&nbsp;</td>
                    <td>
                        <input type="submit" value="注册">
                    </td>
                </tr>
            </table>
            <a href="index.jsp">返回首页</a>
        </form>
    </div>
    </body>
    </html>
    ```
    
19. 编写`queryAllStudent.jsp`文件，作为学生信息展示页面

    ```jsp
    <%--
      Created by IntelliJ IDEA.
      User: bernardo
      Date: 2021/7/29
      Time: 21:22
      To change this template use File | Settings | File Templates.
    --%>
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <%
        String base = request.getContextPath()+"/";
        String url = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+base;
    %>
    <html>
    <head>
        <title>查询实现所有用户</title>
        <script src="js/jquery-3.6.0.min.js"></script>
        <script type="text/javascript" src="js/queryAllStudents.js"></script>
        <base href="<%=url%>"/>
    </head>
    <body>
    <div align="center">
        <h3>查询学生数据</h3>
        <table>
            <thead>
            <tr>
                <td>id</td>
                <td>姓名</td>
                <td>年龄</td>
            </tr>
            </thead>
            <tbody id="studentBody">
    
            </tbody>
        </table>
        <a href="index.jsp">返回首页</a>
    </div>
    </body>
    </html>
    ```

20. 创建`webapp/WEB-INF/view/`目录，并创建编写`fail.jsp`文件，作为注册失败的页面

    ```jsp
    <%--
      Created by IntelliJ IDEA.
      User: bernardo
      Date: 2021/7/29
      Time: 20:11
      To change this template use File | Settings | File Templates.
    --%>
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <%
        String base = request.getContextPath()+"/";
        String url = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+base;
    %>
    <html>
    <head>
        <title>fail</title>
        <base href="<%=url%>"/>
    </head>
    <body>
    <h2>创建用户【${msg}】失败！</h2><br>
    <a href="index.jsp">返回首页</a>
    </body>
    </html>
    ```

21. 创建`webapp/WEB-INF/view/`目录，并创建编写`success.jsp`文件，作为注册成功的页面

    ```jsp
    <%--
      Created by IntelliJ IDEA.
      User: bernardo
      Date: 2021/7/29
      Time: 20:10
      To change this template use File | Settings | File Templates.
    --%>
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <%
        String base = request.getContextPath()+"/";
        String url = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+base;
    %>
    <html>
    <head>
        <title>success</title>
        <base href="<%=url%>"/>
    </head>
    <body>
    <h2>创建用户【${msg}】成功！</h2><br>
    <a href="index.jsp">返回首页</a>
    </body>
    </html>
    ```

22. 创建 `webapp/js/`目录，下载并导入`jquery-3.6.0.min.js`文件

23. 在`webapp/js/`目录下，创建并编写`queryAllStudent.js`文件

    ```js
    $(function() {
        studentInfo();
    })
    
    function studentInfo() {
        $.ajax({
            url:"student/queryAllStudents.do",
            type:"post",
            dataType:"json",
            success:function(resp) {
                $("#studentBody").html("");
                $.each(resp,function (i,n){
                    $("#studentBody").append("<tr>")
                        .append("<td>" + n.id + "</td>")
                        .append("<td>" + n.name + "</td>")
                        .append("<td>" + n.age + "</td>")
                        .append("</tr>")
                })
            }
        })
    }
    ```

24. 配置并启动tomcat服务器

# SpringMVC核心技术

## 请求重定向和转发

### 概述

当处理器对请求处理完毕后，向其它资源进行跳转时，有两种跳转方式:  请求转发与重定向。而根据所要跳转的资源类型，又可分为两类:跳转到页面与跳转到其它处理器。

注意，对于请求转发的页面，可以是 WEB-INF 中页面;而重定向的页面，是不能为 WEB-INF 中页的。因为重定向相当于用户再次发出一次请求，而用户是不能直接访问 WEB-INF 中资 源的。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210809101601.png)

SpringMVC 框架把原来 Servlet 中的请求转发和重定向操作进行了封装。现在可以使用简单的方式实现转发和重定向。

+ `forward`：表示转发，实现`request.getRequestDispatcher("xx.jsp").forward()`
+ `redirect`：表示重定向，实现`response.sendRedirect("xx.jsp")`

### 请求转发

处理器方法返回 ModelAndView 时，需在 setViewName()指定的视图前添加 forward:，且此时的视图不再与视图解析器一同工作，这样可以在配置了解析器时指定不同位置的视图。 视图页面必须写出相对于项目根的路径。forward 操作不需要视图解析器。

处理器方法返回 String,在视图路径前面加入 forward: 视图完整路径。

```java
    /**
     * 处理器方法返回ModelAndView：实现转发到其他视图页面
     * 语法：setViewName("forward:视图的完整路径")
     * 特点：不和视图解析器一同工作，即忽略视图解析器
     * @param age 年龄
     * @param name 名字
     * @return 将要传递的数据和跳转的视图
     */
    @RequestMapping(value = "/forward.do")
    public ModelAndView forward(Integer age, String name) {
        System.out.println("执行了forward方法：" + "name = " + name + ",age = " + age);
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("myName",name);
        modelAndView.addObject("myAge",age);
        //转发可以到WEB-INF目录下
        modelAndView.setViewName("forward:/WEB-INF/jsp/forwardResult.jsp");
        return modelAndView;
    }
```

### 请求重定向

在处理器方法返回的视图字符串的前面添加 redirect:，则可实现重定向跳转

```java
    /**
     * 处理器方法返回ModelAndView，实现重定向到视图页面
     * 语法：setViewName("redirect:视图完整路径")
     * 特点：不和视图解析器一同工作，即忽略视图解析器
     * @param age 年龄
     * @param name 名字
     * @return 将要传递的数据和跳转的视图
     */
    @RequestMapping(value = "/redirect.do")
    public ModelAndView redirect(Integer age, String name) {
        System.out.println("执行了redirect方法：" + "name = " + name + ",age = " + age);
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("myName", name);
        modelAndView.addObject("myAge",age);
        //重定向不可以到WEB-INF目录下
        modelAndView.setViewName("redirect:redirectResult.jsp");
        return modelAndView;
    }
```

## 异常处理

### 概述

SpringMVC框架处理异常的常用方式：使用`@ExceptionHandler`注解处理异常。

使用注解`@ExceptionHandler`可以将一个方法指定为异常处理方法。该注解只有一个可选属性`value`，为一个`Class<?>`数组，用于指定该注解的方法所要处理的异常类，即所要匹配的异常。

而被注解的方法，其返回值可以是`ModelAndView`、` String`或`void`，方法名随意，方法参数可以是`Exception`及其子类对象、`HttpServletRequest`、`httpsServletResponse`等。系统会自动为这些方法参数赋值。

对于异常处理注解的用法，也可以直接将异常处理方法注解于`Controller`之中。

不过，一般不这样使用。而是将异常处理方法专门定义在一个类中，作为全局的异常处理类。

需要使用注解`@ControllerAdvice`，字面理解就是【控制器增强】，是给控制器对象增强功能的。使用`@ControllerAdvice`修饰的类中可以使用`@ExceptionHandler`。

当使用`@RequestMapping`注解修饰的方法抛出异常时，会执行`@ControllerAdvice`修饰的类中异常处理方法。

`@ControllerAdvice`是使用`@Component`注解修饰的，可以`<context:component-scan>`扫描到`@ControllerAdvice`所在的类路径（包名），创建对象。

### 代码实现

> 代码是在`整合SSM框架`的代码基础上进行修改的

1. **自定义异常类**

   定义三个异常类：`MyUserException`、 `NameException`、`AgeException`。其中`MyUserException`是另外两个异常的父类。

   ```java
   package xyz.rtx3090.handler;
   
   public class MyUserException extends Exception{
       public MyUserException() {
           super();
       }
   
       public MyUserException(String message) {
           super(message);
       }
   }
   ```

   ```java
   package xyz.rtx3090.handler;
   
   public class NameException extends MyUserException{
       public NameException() {
           super();
       }
   
       public NameException(String message) {
           super(message);
       }
   }
   ```

   ```java
   package xyz.rtx3090.handler;
   
   public class AgeException extends MyUserException{
       public AgeException() {
           super();
       }
   
       public AgeException(String message) {
           super(message);
       }
   }
   ```

2. 定义全局异常处理类

   ```java
   package xyz.rtx3090.controller;
   
   import org.springframework.web.bind.annotation.ControllerAdvice;
   import org.springframework.web.bind.annotation.ExceptionHandler;
   import org.springframework.web.servlet.ModelAndView;
   import xyz.rtx3090.handler.AgeException;
   import xyz.rtx3090.handler.NameException;
   
   @ControllerAdvice
   public class GlobalExceptionResolver {
       /**
        * 处理NameException类型的异常
        */
       @ExceptionHandler(value = NameException.class)
       public ModelAndView nameException(Exception ex) {
           ModelAndView modelAndView = new ModelAndView();
           modelAndView.addObject("tips","@ControllerAdvice使用注解处理NameException");
           modelAndView.addObject("ex",ex);
           modelAndView.setViewName("nameException");
           return modelAndView;
       }
   
       /**
        * 处理AgeException类型的异常
        */
       @ExceptionHandler(value = AgeException.class)
       public ModelAndView ageException(Exception ex) {
           ModelAndView modelAndView = new ModelAndView();
           modelAndView.addObject("tips","@ControllerAdvice使用注解处理AgeException");
           modelAndView.addObject("ex",ex);
           modelAndView.setViewName("ageException");
           return modelAndView;
       }
   
       /**
        * 处理其他默认类型的异常
        */
       @ExceptionHandler
       public ModelAndView defaultException(Exception ex){
           ModelAndView modelAndView = new ModelAndView();
           modelAndView.addObject("tips","@ControllerAdvice使用注解处理defaultException");
           modelAndView.addObject("ex",ex);
           modelAndView.setViewName("defaultException");
           return modelAndView;
       }
   }
   ```
   
3. 修改`Controller`抛出异常

   ```java
   package xyz.rtx3090.controller;
   
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.ResponseBody;
   import org.springframework.web.servlet.ModelAndView;
   import xyz.rtx3090.handler.AgeException;
   import xyz.rtx3090.handler.NameException;
   import xyz.rtx3090.pojo.Student;
   import xyz.rtx3090.service.StudentService;
   
   import java.util.List;
   
   @Controller
   @RequestMapping(value = "/student")
   public class StudentController {
       @Autowired
       private StudentService studentService;
   
       @RequestMapping(value = "/insertStudent.do")
       public ModelAndView insertStudent(Student student) throws NameException, AgeException {
           ModelAndView modelAndView = new ModelAndView();
           if ("admin".equals(student.getName())){
               //抛出NameException异常
               throw new NameException("注册用户名不能为[admin]");
           } else if (student.getAge() < 0 || student.getAge() > 120){
               //抛出AgeException异常
               throw new AgeException("注册用户年龄不合法");
           } else {
               int result = studentService.insertStudent(student);
               if (result == 1) {
                   modelAndView.addObject("msg","注册学生用户成功!");
                   modelAndView.setViewName("success");
               } else {
                   modelAndView.addObject("msg","注册的学生用户失败");
                   modelAndView.setViewName("fail");
               }
           }
           return modelAndView;
       }
   
       @RequestMapping(value = "/selectAllStudents.do")
       @ResponseBody
       public List<Student> selectAllStudents() {
           return studentService.selectAllStudents();
       }
   }
   ```

4. 定义异常响应页面

   `nameException.jsp`页面

   ```jsp
   <%--
     Created by IntelliJ IDEA.
     User: bernardo
     Date: 2021/8/12
     Time: 16:02
     To change this template use File | Settings | File Templates.
   --%>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
     <head>
       <title>名字异常页面</title>
     </head>
     <body>
       <h2>nameException</h2>
       ${ex.message}
     </body>
   </html>
   
   ```

   `ageException.jsp`页面

   ```jsp
   <%--
     Created by IntelliJ IDEA.
     User: bernardo
     Date: 2021/8/12
     Time: 16:02
     To change this template use File | Settings | File Templates.
   --%>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>年龄页面异常页面</title>
   </head>
   <body>
       <h2>ageException</h2>
       ${ex.message}
   </body>
   </html>
   ```

   `defaultException.jsp`页面

   ```jsp
   <%--
     Created by IntelliJ IDEA.
     User: bernardo
     Date: 2021/8/12
     Time: 16:30
     To change this template use File | Settings | File Templates.
   --%>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>默认异常页面</title>
   </head>
   <body>
       <h2>defaultException</h2>
       #{ex.message}
   </body>
   </html>
   ```

5. 修改Spring配置文件，增强`@ControllerAdvice`注解所在包名的注册组件扫描器

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context
           https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
   
       <!--注册组件扫描器,指定@Controller注解所在的包-->
       <context:component-scan base-package="xyz.rtx3090.controller"/>
   
       <!--注册组件扫描器，指定@ControllerAdvice注解所在的包名-->
       <context:component-scan base-package="xyz.rtx3090.handler"/>
   
       <!--指定视图解析器-->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
           <property name="prefix" value="/WEB-INF/result/"/>
           <property name="suffix" value=".jsp"/>
       </bean>
   
       <!--注册注解驱动-->
       <mvc:annotation-driven/>
   </beans>
   ```


## 拦截器

### 概述

SpringMVC中的`Interceptor`拦截器是非常重要和相当有用，它的主要作用是拦截指定的用户请求，并进行相应的预处理和后处理。其拦截的时间点在“处理器映射器根据用户提交的请求映射出了所要执行的处理器类，并且也找到了要执行该处理器类的处理器适配器，在处理器适配器执行处理器之前”。当然，在处理器映射器映射出所要执行的处理器类时，已经将拦截器与处理器组合为了一个处理器执行链，并返回给了中央调度器。

### 第一个拦截器程序

1. 修改`Controller`类，在最后新增拦截器方法

   ```java
   package xyz.rtx3090.controller;
   
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.ResponseBody;
   import org.springframework.web.servlet.ModelAndView;
   import xyz.rtx3090.handler.AgeException;
   import xyz.rtx3090.handler.NameException;
   import xyz.rtx3090.pojo.Student;
   import xyz.rtx3090.service.StudentService;
   
   import javax.servlet.http.HttpSession;
   import java.util.List;
   
   @Controller
   @RequestMapping(value = "/student")
   public class StudentController {
       @Autowired
       private StudentService studentService;
   
       @RequestMapping(value = "/insertStudent.do")
       public ModelAndView insertStudent(Student student) throws NameException, AgeException {
           ModelAndView modelAndView = new ModelAndView();
           if ("admin".equals(student.getName())){
               //抛出NameException异常
               throw new NameException("注册用户名不能为[admin]");
           } else if (student.getAge() < 0 || student.getAge() > 120){
               //抛出AgeException异常
               throw new AgeException("注册用户年龄不合法");
           } else {
               int result = studentService.insertStudent(student);
               if (result == 1) {
                   modelAndView.addObject("msg","注册学生用户成功!");
                   modelAndView.setViewName("success");
               } else {
                   modelAndView.addObject("msg","注册的学生用户失败");
                   modelAndView.setViewName("fail");
               }
           }
           return modelAndView;
       }
   
       @RequestMapping(value = "/selectAllStudents.do")
       @ResponseBody
       public List<Student> selectAllStudents() {
           return studentService.selectAllStudents();
       }
   
     	//拦截器代码
       @RequestMapping(value = "/interceptor.do")
       public ModelAndView interceptor(String name, Integer age, HttpSession session) {
           System.out.println("执行Controller中的方法");
           ModelAndView modelAndView = new ModelAndView();
           modelAndView.addObject("name",name);
           modelAndView.addObject("age",age);
           modelAndView.setViewName("interceptorResult");
   
           session.setAttribute("attr","session中数据");
   
           return modelAndView;
       }
   }
   ```

2. 创建`interceptors.MyInterceptor`类，执行拦截操作

   ```java
   package xyz.rtx3090.interceptors;
   
   import org.springframework.web.servlet.HandlerInterceptor;
   import org.springframework.web.servlet.ModelAndView;
   
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   import javax.servlet.http.HttpSession;
   
   public class MyInterceptor implements HandlerInterceptor {
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           System.out.println("执行MyInterceptor类的preHandler()方法");
           return true;
       }
   
       @Override
       public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
           System.out.println("执行MyInterceptor类的postHandle()方法");
       }
   
       @Override
       public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
           System.out.println("执行MyInterceptor类的afterCompletion()方法");
           HttpSession session = request.getSession();
           Object attr = session.getAttribute("attr");
           System.out.println("attr删除之前: " + session.getAttribute("attr"));
           session.removeAttribute("attr");
           System.out.println("attr删除之后: " + session.getAttribute("attr"));
       }
   }
   ```

3. 新建`interceptor.jsp`页面

   ```jsp
   <%--
     Created by IntelliJ IDEA.
     User: bernardo
     Date: 2021/8/13
     Time: 19:13
     To change this template use File | Settings | File Templates.
   --%>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <%
       String base = request.getContextPath()+"/";
       String url = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+base;
   %>
   <html>
   <head>
       <title>拦截器测试页面</title>
       <base href="<%=url%>"/>
   </head>
   <body>
       <form action="student/interceptor.do" method="post">
           姓名：<input type="text" name="name"><br>
           年龄：<input type="text" name="age"><br>
           <input type="submit" value="提交">
       </form>
   </body>
   </html>
   ```

4. 新建`WEB-INF/result/interceptorResult.jsp`页面

   ```jsp
   <%--
     Created by IntelliJ IDEA.
     User: bernardo
     Date: 2021/8/13
     Time: 18:54
     To change this template use File | Settings | File Templates.
   --%>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>拦截器测试结果页面</title>
   </head>
   <body>
   <h2>拦截器测试结果页面</h2>
   </body>
   </html>
   ```

5. 配置并启动tomcat服务器

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210813192443.png)

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210813192555.png)

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210813192607.png)

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210813192638.png)

### 拦截器方法详解

自定义拦截器，需要实现`HandlerInterceptor`接口，而该接口中含有三个方法。

1. `public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)`

   该方法在处理器方法执行之前执行，其返回值为`boolean`，若为`true`，则紧接着会执行处理器方法，且会将`afterCompletion()`方法放入到一个专门的方法栈中等待执行。

2. `public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView)`

   该方法在处理器方法执行之后执行，处理器方法若最终未被执行，则该方法不会执行。由于该方法是在处理器方法执行完后执行，且该方法参数重包含`ModelAndView`，所以该方法可以修改处理器方法的处理结果数据，且可以修改跳转方向。

3. `public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex)`

   当`preHandle()`方法返回`true`时，会将该方法放到专门的方法栈中，等到对请求进行响应所有功能完成之后才执行该方法。即该方法是在中央调度器渲染（数据填充）了响应页面之后执行的，此时对`ModelAndView`再操作也对响应无济于事。

   `afterCompletion`最后执行的方法，清除资源，例如在`Controller`方法中加入数据。

### 拦截器执行顺序

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210813195445.png)

### 多个拦截器的执行

在`第一个拦截器程序`的代码基础上进行修改

1. 在包`interceptors`新增一个拦截器`MyInterceptor2`类

   ```java
   package xyz.rtx3090.interceptors;
   
   import org.springframework.web.servlet.HandlerInterceptor;
   import org.springframework.web.servlet.ModelAndView;
   
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   
   public class MyInterceptor2 implements HandlerInterceptor {
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           System.out.println("执行MyInterceptor2类的preHandler()方法");
           return true;
       }
   
       @Override
       public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
           System.out.println("执行MyInterceptor2类的postHandle()方法");
       }
   
       @Override
       public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
           System.out.println("执行MyInterceptor2类的afterCompletion()方法");
       }
   }
   ```

2. 修改`springmvc-servlet.xml`配置文件，新增一个拦截器的注册

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context
           https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
   
       <!--注册组件扫描器,指定@Controller注解所在的包-->
       <context:component-scan base-package="xyz.rtx3090.controller"/>
   
       <!--注册组件扫描器，指定@ControllerAdvice注解所在的包名-->
       <context:component-scan base-package="xyz.rtx3090.handler"/>
   
       <!--指定视图解析器-->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
           <property name="prefix" value="/WEB-INF/result/"/>
           <property name="suffix" value=".jsp"/>
       </bean>
   
       <!--注册拦截器-->
       <mvc:interceptors>
           <!--声明第一个拦截器-->
           <mvc:interceptor>
               <!--指定所需拦截的请求路径，/**表示拦截所有请求-->
               <mvc:mapping path="/**"/>
               <!--声明拦截器对象-->
               <bean class="xyz.rtx3090.interceptors.MyInterceptor"/>
           </mvc:interceptor>
           <!--声明第二个拦截器-->
           <mvc:interceptor>
               <mvc:mapping path="/**"/>
               <bean class="xyz.rtx3090.interceptors.MyInterceptor2"/>
           </mvc:interceptor>
       </mvc:interceptors>
   
       <!--注册注解驱动-->
       <mvc:annotation-driven/>
   </beans>
   ```

3. 配置并启动tomcat服务器，观察控制台的执行结果

   ```
   执行MyInterceptor类的preHandler()方法
   执行MyInterceptor2类的preHandler()方法
   执行Controller中的方法
   执行MyInterceptor2类的postHandle()方法
   执行MyInterceptor类的postHandle()方法
   执行MyInterceptor2类的afterCompletion()方法
   执行MyInterceptor类的afterCompletion()方法
   attr删除之前: session中数据
   attr删除之后: null
   ```

当有多个拦截器时，形成拦截器链。拦截器链的执行顺序，与其注册顺序一致。需要再次强调一点的是，当某一个拦截器的`preHandle()`方法返回`true`并被执行到时，会向一个专门的方法栈中放入拦截器的`afterCompletion()`方法。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210814012358.png)

从图中可以看出，只要一个`preHandle()`方法返回`false`，则上部的执行链将被断开，其后续的处理器方法与`postHandle()`方法将无法执行。但无论执行链执行情况怎样，只要方法栈中有方法，即执行链中只要有`preHandle()`方法返回`true`，就会执行方法栈中的`afterCompletion()`方法，最终都会给响应。

换一种表现方式，也可以这样理解：

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210814012638.png)

### 案例——权限拦截器举例

只有经过登录的用户可访问处理器，否则将返回“无权访问”提示。

本例的登录，由一个JSP页面完成。即在该页面里将用户信息放入`session`中，也就是说只要访问过该页面，就说明登录了。没访问过，则为未登录用户。

1. 修改`index.jsp`页面

   ```jsp
   <%--
     Created by IntelliJ IDEA.
     User: bernardo
     Date: 2021/7/29
     Time: 20:18
     To change this template use File | Settings | File Templates.
   --%>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <%
       String base = request.getContextPath()+"/";
       String url = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+base;
   %>
   <html>
   <head>
       <title>Student System</title>
       <%--指定项目根目录--%>
       <base href="<%=url%>"/>
   </head>
   <body>
       <h2>Index page</h2>
   </body>
   </html>
   ```

2. 新建`login.jsp`页面

   ```jsp
   <%--
     Created by IntelliJ IDEA.
     User: bernardo
     Date: 2021/8/15
     Time: 16:39
     To change this template use File | Settings | File Templates.
   --%>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>登录页面</title>
   </head>
   <body>
   <%
       session.setAttribute("user","beijing");
   %>
   <h2>登录成功！</h2>
   </body>
   </html>
   ```

3. 新建`logout.jsp`页面

   ```jsp
   <%--
     Created by IntelliJ IDEA.
     User: bernardo
     Date: 2021/8/15
     Time: 16:40
     To change this template use File | Settings | File Templates.
   --%>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>退出登录页面</title>
   </head>
   <body>
   <%
       session.removeAttribute("user");
   %>
   <h2>已退出系统！</h2>
   </body>
   </html>
   ```

4. 新建`/WEB-INF/result/welcome.jsp`页面

   ```jsp
   <%--
     Created by IntelliJ IDEA.
     User: bernardo
     Date: 2021/8/15
     Time: 16:27
     To change this template use File | Settings | File Templates.
   --%>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>welcome</title>
   </head>
   <body>
   <h2>欢迎进入系统！</h2>
   </body>
   </html>
   ```

5. 修改`Controller`类

   ```java
   package xyz.rtx3090.controller;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.servlet.ModelAndView;
   
   @Controller
   public class StudentController {
   
       @RequestMapping(value = "/system.do")
       public ModelAndView doSome() {
           System.out.println("欢迎进入系统");
           return new ModelAndView("welcome");
       }
   }
   ```

6. 新建`PermissionInterceptor`拦截器

   ```java
   package xyz.rtx3090.interceptors;
   
   import org.springframework.web.servlet.HandlerInterceptor;
   import org.springframework.web.servlet.ModelAndView;
   
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   
   public class PermissionInterceptor implements HandlerInterceptor {
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           System.out.println("执行PermissionInterceptor类的preHandle()方法");
           String user = (String) request.getSession().getAttribute("user");
           if (!"beijing".equals(user)) {
               request.getRequestDispatcher("/fail.jsp").forward(request,response);
               return false;
           }
           return true;
       }
   
       @Override
       public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
           System.out.println("执行MyInterceptor的postHandle()");
       }
   
       @Override
       public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
           System.out.println("执行MyInterceptor的afterCompletion()");
       }
   }
   ```

7. 在`springmvc`配置文件注册拦截器

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context
           https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
   
       <!--注册组件扫描器,指定@Controller注解所在的包-->
       <context:component-scan base-package="xyz.rtx3090.controller"/>
   
       <!--注册组件扫描器，指定@ControllerAdvice注解所在的包名-->
       <context:component-scan base-package="xyz.rtx3090.handler"/>
   
       <!--指定视图解析器-->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
           <property name="prefix" value="/WEB-INF/result/"/>
           <property name="suffix" value=".jsp"/>
       </bean>
   
       <!--注册拦截器-->
       <mvc:interceptors>
           <mvc:interceptor>
               <mvc:mapping path="/**"/>
               <bean class="xyz.rtx3090.interceptors.PermissionInterceptor"/>
           </mvc:interceptor>
       </mvc:interceptors>
   
       <!--注册注解驱动-->
       <mvc:annotation-driven/>
   </beans>
   ```

8. 配置并启动tomcat服务器

   1. 首先在浏览器地址栏输入`http://localhost:8080/SpringMVC08_Exception_war/system.do`，尝试直接进入系统，因为没用登录，结果被拦截器拦截

      ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210815170711.png)

   2. 然后在浏览器地址栏输入`http://localhost:8080/SpringMVC08_Exception_war/login.jsp`，进行登录操作

      ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210815170834.png)

   3. 再次在浏览器地址栏输入`http://localhost:8080/SpringMVC08_Exception_war/system.do`，进行第二次进入系统的操作，因为已经登录过了，所以这次没被拦截器拦截，进入系统成功

      ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210815171024.png)

   4. 在浏览器地址栏输入`http://localhost:8080/SpringMVC08_Exception_war/logout.jsp`，进行退出登录的操作

      ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210815171118.png)

   5. 在浏览器地址栏输入`http://localhost:8080/SpringMVC08_Exception_war/system.do`，在退出系统后，尝试进入系统失败，被拦截器拦截

      ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210815171441.png)