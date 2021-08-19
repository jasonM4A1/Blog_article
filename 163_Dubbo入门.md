Dubbo入门

---

# RPC基础知识

## 软件架构

### 单一应用架构

当网站流量很小时，应用规模小时，只需一个应用，将所有的功能都部署在一起，以减少部署服务数量和成本。此时，用于简化增删改查工作量的数据访问框架(ORM)是关键。数据库的处理时间影响应用的性能。

这种结果的应用适合小型系统、小型网站、或者企业的内部系统，用户较少，请求量不大，对请求的处理时间没有太高的要求。将所有功能都部署到一个服务器，简单易用。开发项目的难度低。

但缺点也显而易见：

1. 性能扩展比较困难
2. 不利于多人同时开发
3. 不利于升级维护
4. 整个系统的空间占用比较大

### 分布式服务架构

当应用越来越多，应用之间交互不可避免，将核心业务抽取出来，作为独立的服务，逐渐形成稳定的服务中心，是前段应用能更快速的响应多变的市场需求。此时用于提高业务服用及整合的**分布式服务框架（RPC）**是关键。分布式系统将服务作为独立的应用，实现服务共享和重用。

## 分布式系统

### 什么是分布式系统

分布式系统是若干独立计算机（服务器）的集合，这些计算机对于用户来说就像单个相关系统，分布式系统（distributed system) 是建立在网络之上的服务器端的一种结构。

分布式系统中的计算机可以使用不同的操作系统，可以运行不同应用程序提供服务，将服务分散部署到多个计算机服务器上。

### 什么是RPC

RPC（Remote Procedure Call）是指远程过程调用，是一种进程间通信方式，是一种技术思想，而不是规范。它允许程序调用另一个地址空间（网络的另一台机器上）的过程或函数，而不用开发人员显示编码这个调用的细节。调用本地方法和调用远程方法一样。

RPC的实现方法可以不同，例如Java的`rmi`、`spring`远程调用等。

RPC概念是在上世纪80年代由`Brue Jay Nelson`（布鲁·杰伊·纳尔逊）提出，使用RPC可以将本地的调用扩展到远程调用（分布式系统的其他服务器）

RPC的特点：

1. 简单：使用简单，建立分布式应用更容易
2. 高效：调用过程看起来十分清晰，效率高
3. 通用：进程间通讯的方式，有通用的规则

### RPC基本原理

#### 图解

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210816035311.png)

#### RPC调用过程

1. 调用方client要使用右侧server的功能（方法），发起对方法的调用
2. client stub是RPC中定义的存根，看做是client的助手。stub把要调用的方法参数进行序列化、方法名称和其他数据包装起来
3. 通过网络socket（网络通信的技术），把方法调用的细节内容发送给右侧的server
4. server端通过socket接受请求的方法名称、参数等数据，传给stub.
5. server端接到的数据由server stub（server的助手）处理，来调用server的真正方法、处理业务
6. server方法处理完业务，把处理的结果对象（Object）交给了助手，助手把Object进行序列化，对象转为二进制数据
7.  server助手二进制数据交给网络处理程序
8. 通过网络将二进制数据，发送给client
9. client接到数据，交给client助手
10. client助手，接收到数据通过反序列化为Java对象（Object），作为远程方法调用结果

> RPC通讯是基于tcp或udp协议
>
> 序列化方法（xml/json/二进制）

# Dubbo框架

## Dubbo概述

Apache Dubbo是一款高性能、轻量级的开源Java RPC框架，它提供了三大核心能力：

1. 面向接口的远程方法调用
2. 只能容错和负载均衡
3. 服务自动注册和发现

Dubbo是一个车分布式服务框架，致力于提供高性能和透明化的RPC远程服务调用方案及服务治理方案

> **面向接口代理：**
>
> 调用接口的方法，在A服务器调用B服务器的方法，由Dubbo实现对B的调用，无需关心实现的细节。
>
> 就像Mybatis访问Dao接口可以操作数据库一样，不要管线Dao接口方法的实现。简化了开发流程和难度。

## 基本架构

### 图解

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210816040625.png)

### 对象角色说明

+ **服务提供者（provider）**：暴露服务的提供方，服务提供者在启动时，向注册中心注册自己提供的服务。
+ **服务消费者（Consumer）**：调用远程服务的消费方，服务消费者在启动时，向注册中心订阅自己所需的服务。服务消费者从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。
+ **注册中心（Registry）**：注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者
+ **监控中心（Monitor）**：服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心

### 调用关系说明

+ **服务容器**负责启动、加载、运行服务提供者
+ 服务提供者在启动时，向注册中心注册自己提供的服务
+ 服务消费者在启动时，向注册中心订阅自己所需的服务
+ 注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长链接推送变更数据给消费者
+ 服务消费者从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用
+ 服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心

## Dubbo支持的协议

Dubbo支持多种协议，如：`dubbo`、`hessian`、`rmi`、`http`、`webservice`、`thrift`、`memcached`、`redis`等

Dubbo官方推荐使用`dubbo`协议，dubbo协议默认端口20880.

使用`dubbo`协议，需要在spring配置文件中加入下列标签注解：

`<dubbo:protocol name="dubbo" port="20880"`/>

## Dubbo直连实现

1. 首先创建一个空的普通Java工程

2. 创建一个子maven web工程`001-link-userService-provider`，最终项目目录结构如图所示

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210816042659.png)

3. 新建`readme.md`文件，编写项目思路

   ```markdown
   1. 创建一个maven web工程——服务的提供者
   2. 配置pom.xml文件——添加需要的依赖
   3. 创建一个实体bean——User
   4. 提供一个服务接口——UserService
   5. 实现这个服务接口——UserServiceImpl
   6. 配置dubbo服务提供者的核心配置文件
       + 声明dubbo服务提供者的名称（保证唯一性）
       + 声明dubbo使用的协议和端口号
       + 暴露服务，使用直连方式
   7. 添加监听器
   ```

4. 编写`pom.xml`文件，添加依赖

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
     <modelVersion>4.0.0</modelVersion>
   
     <groupId>xyz.rtx3090</groupId>
     <artifactId>001-link-userService-provider</artifactId>
     <version>1.0-SNAPSHOT</version>
     <!--packaging值默认为jar-->
     <packaging>war</packaging>
   
     <dependencies>
       <!--spring依赖-->
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-context</artifactId>
         <version>4.3.16.RELEASE</version>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-webmvc</artifactId>
         <version>4.3.16.RELEASE</version>
       </dependency>
       <!--dubbo依赖-->
       <!-- https://mvnrepository.com/artifact/com.alibaba/dubbo -->
       <dependency>
         <groupId>com.alibaba</groupId>
         <artifactId>dubbo</artifactId>
         <version>2.6.2</version>
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
   
   </project>
   ```

5. 新建包`xyz.rtx3090.dubbo.model`，并在其下创建`User`实体类

   ```java
   package xyz.rtx3090.dubbo.model;
   
   import java.io.Serializable;
   
   public class User implements Serializable {
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

6. 新建包`xyz.rtx3090.dubbo.service`，并在其下创建`UserService`接口类

   ```java
   package xyz.rtx3090.dubbo.service;
   
   import xyz.rtx3090.dubbo.model.User;
   
   public interface UserService {
       //根据id查询用户信息
       User queryUserById(Integer id);
   }
   ```

7. 新建包`xyz.rtx3090.dubbo.service.impl`，并在其下创建`UserServiceImpl`接口类

   ```java
   package xyz.rtx3090.dubbo.service.impl;
   
   import xyz.rtx3090.dubbo.model.User;
   import xyz.rtx3090.dubbo.service.UserService;
   
   public class UserServiceImpl implements UserService {
       @Override
       public User queryUserById(Integer id) {
           //模拟获取持久层中的数据
           User user = new User();
           user.setId(001);
           user.setName("Json");
           user.setAge(22);
   
           return user;
       }
   }
   ```

8. 在`resources`目录创建`dubbo-userService-provider.xml`spring配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">
       <!--服务提供者声明名称：必须保证服务名称的唯一性，它的名称是dubbo内部使用的唯一标示-->
       <dubbo:application name="001-link-userService-provider"/>
   
       <!--访问服务协议的名称及端口号，dubbo官方推荐使用的是dubbo协议，端口号默认为20880
           属性：
               name: 执行协议的名称
               port: 执行协议的端口号（默认为20880)-->
       <dubbo:protocol name="dubbo"/>
   
       <!--暴露服务接口
           属性：
               interface: 暴露服务接口的权限定类名
               ref: 接口引用的实现类的spring容器中的标示
               registry: 如果不使用注册中心，则值为N/A-->
   
       <dubbo:service interface="xyz.rtx3090.dubbo.service.UserService" ref="userService" registry="N/A"/>
   
       <!--将接口的实现类加载到spring容器中-->
       <bean id="userService" class="xyz.rtx3090.dubbo.service.impl.UserServiceImpl"/>
   </beans>
   ```

9. 编写`web.xml`配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
     
   	<!--注册监听器-->
     <context-param>
       <param-name>contextConfigLocation</param-name>
       <param-value>classpath:dubbo-userService-provider.xml</param-value>
     </context-param>
     <listener>
       <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
     </listener>
   </web-app>
   ```

10. 注释`pom.xml`文件中的`<packaging>war</packaging>`标签，是项目打包方式变为默认的`jar`，然后在右侧的`Maven->Lifecyde->install`把项目打包为Jar包，最后再取消`<packaging>war</packaging>`标签的注解

11. 再创建一个新的maven web子工程`002-link-consumer`，最终项目目录结构如图所示

    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210816043854.png)

12. 新建`readme.md`文件，编写项目思路

    ```markdown
    1. 创建一个maven web工程——服务的消费者
    2. 配置pom文件——添加需要的依赖
    3. 设置dubbo的核心配置文件
    4. 编写controller类
    5. 配置DispatcherServlet中央调度器
    ```

13. 编写`pom.xml`文件，添加依赖

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
      <modelVersion>4.0.0</modelVersion>
    
      <groupId>xyz.rtx3090</groupId>
      <artifactId>002-link-consumer</artifactId>
      <version>1.0-SNAPSHOT</version>
      <packaging>war</packaging>
    
      <dependencies>
        <!--spring依赖-->
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>4.3.16.RELEASE</version>
        </dependency>
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-webmvc</artifactId>
          <version>4.3.16.RELEASE</version>
        </dependency>
        <!--dubbo依赖-->
        <dependency>
          <groupId>com.alibaba</groupId>
          <artifactId>dubbo</artifactId>
          <version>2.6.2</version>
        </dependency>
        <!--依赖服务提供者-->
        <dependency>
          <groupId>xyz.rtx3090</groupId>
          <artifactId>001-link-userService-provider</artifactId>
          <version>1.0-SNAPSHOT</version>
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
    </project>
    ```

14. 新建包`xyz.rtx3090.dubbo.web`，并在其下新建类`UserController`

    ```java
    package xyz.rtx3090.dubbo.web;
    
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.stereotype.Controller;
    import org.springframework.ui.Model;
    import org.springframework.web.bind.annotation.RequestMapping;
    import xyz.rtx3090.dubbo.model.User;
    import xyz.rtx3090.dubbo.service.UserService;
    
    @Controller
    public class UserController {
        @Autowired
        private UserService userService;
    
        @RequestMapping(value = "/user")
        public String userDetail(Model model,Integer id) {
            User user = userService.queryUserById(id);
            model.addAttribute("user",user);
            return "userDetail";
        }
    }
    ```

15. 在`webapp`目录下新建`userDetail.jsp`页面

    ```jsp
    <%--
      Created by IntelliJ IDEA.
      User: bernardo
      Date: 2021/8/16
      Time: 03:16
      To change this template use File | Settings | File Templates.
    --%>
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <html>
    <head>
        <title>用户详情</title>
    </head>
    <body>
    <h1>用户详情</h1>
    <div>用户标识: ${user.getId()}</div>
    <div>用户名称:${user.getName()}</div>
    </body>
    </html>
    ```

16. 在`resources`目录下，新建`dubbo-consumer.xml`配置文件

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">
    
        <!--声明服务消费者的名称——保证唯一性-->
        <dubbo:application name="002-link-consumer"/>
    
        <!--引用远程服务接口：
            id——远程服务接口对象的名称
            interface——调用远程接口的全限定类名称
            url——访问服务接口的地址
            registry——不使用注册中心，值为：N/A-->
        <dubbo:reference
                id="userService"
                interface="xyz.rtx3090.dubbo.service.UserService"
                url="dubbo://localhost:20880" registry="N/A"/>
    </beans>
    ```

17. 在`resources`目录下，新建`application.xml`spring核心配置文件

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xmlns:mvc="http://www.springframework.org/schema/mvc"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd">
        <!--扫描组件-->
        <context:component-scan base-package="xyz.rtx3090.dubbo.web"/>
    
        <!--配置注解驱动-->
        <mvc:annotation-driven/>
    
        <!--配置视图解析器-->
        <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
            <property name="prefix" value="/"/>
            <property name="suffix" value=".jsp"/>
        </bean>
    </beans>
    ```

18. 修改`web.xml`配置文件

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
             version="4.0">
    	
      <!--注册DispatcherServlet中央调度器-->
      <servlet>
        <servlet-name>dispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
          <param-name>contextConfigLocation</param-name>
          <param-value>classpath:application.xml,classpath:dubbo-consumer.xml</param-value>
        </init-param>
      </servlet>
      <servlet-mapping>
        <servlet-name>dispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
      </servlet-mapping>
    </web-app>
    ```

19. 配置`001-link-userService-provider`项目的Tomcat服务器，重点修改`Http port`端口和`JMX port`端口

    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210816044925.png)

    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210816044848.png)

20. 配置`002-link-consumer`项目的Tomcat服务器

    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210816045003.png)

    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210816045024.png)

21. 先启动`001-link-userService-provider`项目的Tomcat服务器，再启动`002-link-consumer`项目的Tomcat服务器，并在浏览器地址栏中输入`http://localhost:8080/user?001`，查看页面结果

    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210816045215.png)

## Dubbo服务化最佳实践

### 分包

建议将服务接口、服务模型等均放在公共包中

### 粒度

+ 服务接口尽可能大粒度，每个服务方法应代表一个功能，而不是某功能的一个步骤
+ 服务接口建议以业务场景为单位划分，并对相近业务做抽象，防止接口数量暴涨
+ 不建议使用过于抽象的通用接口，如`Map query(Map)`，这样的接口没有明确语义，会给后期维护带来不便

### 版本

每个接口都应定义版本号，区分同一个接口的不同实现

如： `<dubbo:service interface="com.xxx.XxxService" version="1.0"`

### Dubbo常用标签 

Dubbo中常用标签分为三个类别：

1. 共用标签
2. 服务提供者标签
3. 服务消费者标签

#### 共用标签

1. 配置应用信息

   `<dubbo:appliation name="服务的名称"/>`

2. 配置注册中心

   `<dubbo:registry address="ip:port" protocol="协议"/>`

#### 服务提供者标签

1. 配置暴露的服务

   `<dubbo:service interface="服务接口名" ref="服务实现对象bean"/>`

#### 服务消费者标签

1. 配置服务消费者引用远程服务

   `<dubbo:reference id="服务引用bean的id" interface="服务接口名"/>`

## Dubbo直连-改造案例

为了解决`Dubbo直连实现`中的问题，我们需要引入一个`接口工程`，将bean实体类和公共接口放入到这个`接口工程 `

1. 新建普通maven工程（不带web模版）`003-link-interface`，最终项目目录结构如图所示：

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210817164430.png)

2. 创建`readme.md`文件，书写项目思路

   ```markdown
   link-interface是一个maven java工程
   dubbo官方推荐使用的一个模式，将实体bean和业务接口存放到接口工程中
   ```

3. 修改`pom.xml`配置文件，去除不需要的内容

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
   
       <groupId>xyz.rtx3090</groupId>
       <artifactId>003-link-interface</artifactId>
       <version>1.0-SNAPSHOT</version>
   
   
   </project>
   ```

4. 在`java`目录下创建包`xyz.rtx3090.model`，并新建`User`实体类

   ```java
   package xyz.rtx3090.model;
   
   import java.io.Serializable;
   
   public class User implements Serializable {
   
       private Integer id;
       private String username;
   
       //setter and getter
       public Integer getId() {
           return id;
       }
   
       public void setId(Integer id) {
           this.id = id;
       }
   
       public String getUsername() {
           return username;
       }
   
       public void setUsername(String username) {
           this.username = username;
       }
   }
   ```

5. 在`java`目录下创建包`xyz.rtx3090.service`，并新建`UserService`接口

   ```java
   package xyz.rtx3090.service;
   
   import xyz.rtx3090.model.User;
   
   public interface UserService {
   
       /**
        * 通过id查询用户信息
        * @param id 用户id号
        * @return 用户对象
        */
       User queryUserById(Integer id);
   
       /**
        * 查询用户总人数
        * @return 用户总人数
        */
       Integer queryAllUserCount();
   }
   ```

6. 新建maven web工程`004-link-userService-provider`，最终项目目录结构如图所示：

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210817164952.png)

7. 修改`pom.xml`文件，添加所需要的依赖

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
     <modelVersion>4.0.0</modelVersion>
   
     <groupId>xyz.rtx3090</groupId>
     <artifactId>004-link-userService-provider</artifactId>
     <version>1.0-SNAPSHOT</version>
     <packaging>war</packaging>
   
     <dependencies>
       <!--spring依赖-->
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-context</artifactId>
         <version>4.3.16.RELEASE</version>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-webmvc</artifactId>
         <version>4.3.16.RELEASE</version>
       </dependency>
       <!--dubbo依赖-->
       <dependency>
         <groupId>com.alibaba</groupId>
         <artifactId>dubbo</artifactId>
         <version>2.6.2</version>
       </dependency>
   
       <!--接口工程-->
       <dependency>
         <groupId>xyz.rtx3090</groupId>
         <artifactId>003-link-interface</artifactId>
         <version>1.0-SNAPSHOT</version>
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
     </build>
   </project>
   ```

8. 在`java`目录下新建包`xyz.rtx3090.service.impl`，并在其包下新建`UserServiceImpl`实现类

   ```java
   package xyz.rtx3090.service.impl;
   
   import xyz.rtx3090.model.User;
   import xyz.rtx3090.service.UserService;
   
   public class UserServiceImpl implements UserService {
       @Override
       public User queryUserById(Integer id) {
           User user = new User();
           user.setId(id);
           user.setUsername("Jason");
           return user;
       }
   
       @Override
       public Integer queryAllUserCount() {
           return 52;
       }
   }
   ```

9. 在`resources`目录下新建`dubbo-userService-provider.xml`配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">
   
       <!--声明dubbo服务提供者名称——保证唯一性-->
       <dubbo:application name="004-link-userService-provider"/>
   
       <!--设置dubbo使用协议和端口号
           name：协议名称
           port：端口号-->
       <dubbo:protocol name="dubbo" port="20880"/>
   
       <!--暴露服务接口-->
       <dubbo:service interface="xyz.rtx3090.service.UserService" ref="userServiceImpl" registry="N/A"/>
   
       <!--加载业务接口的实现类到spring容器中-->
       <bean id="userServiceImpl" class="xyz.rtx3090.service.impl.UserServiceImpl"/>
   </beans>
   ```

10. 修改`web.xml`配置文件，注册监听器

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
             version="4.0">
    
      <!--注册监听器-->
      <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:dubbo-userService-provider.xml</param-value>
      </context-param>
      <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
      </listener>
    
    </web-app>
    ```

11. 新建maven web工程`005-link-consumer`，最终项目目录结构如图所示：

    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210817170928.png)

12. 修改`pom.xml`文件，添加所需要的依赖

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
      <modelVersion>4.0.0</modelVersion>
    
      <groupId>xyz.rtx3090</groupId>
      <artifactId>005-link-consumer</artifactId>
      <version>1.0-SNAPSHOT</version>
      <packaging>war</packaging>
    
      <dependencies>
        <!--spring依赖-->
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>4.3.16.RELEASE</version>
        </dependency>
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-webmvc</artifactId>
          <version>4.3.16.RELEASE</version>
        </dependency>
        <!--dubbok依赖-->
        <dependency>
          <groupId>com.alibaba</groupId>
          <artifactId>dubbo</artifactId>
          <version>2.6.2</version>
        </dependency>
        <!--接口工程-->
        <dependency>
          <groupId>xyz.rtx3090</groupId>
          <artifactId>003-link-interface</artifactId>
          <version>1.0-SNAPSHOT</version>
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
      </build>
    </project>
    ```

13. 在`java`目录下新建包`xyz.rtx3090.web`，并在其包下新建`UserController`类

    ```java
    package xyz.rtx3090.web;
    
    import org.springframework.stereotype.Controller;
    import org.springframework.ui.Model;
    import org.springframework.web.bind.annotation.RequestMapping;
    import xyz.rtx3090.model.User;
    import xyz.rtx3090.service.UserService;
    
    import javax.annotation.Resource;
    
    @Controller
    public class UserController {
        @Resource
        private UserService userService;
    
        @RequestMapping(value = "/userDetail")
        public String userDetail(Model model, Integer id) {
            //根据用户标示获取用户详情
            User user = userService.queryUserById(id);
            //获取用户总人数
            Integer count = userService.queryAllUserCount();
    
            model.addAttribute("user",user);
            model.addAttribute("allUserCount",count);
            return "userDetail";
        }
    }
    ```

14. 在`webapp`目录下新建`userDetail.jsp`页面

    ```jsp
    <%--
      Created by IntelliJ IDEA.
      User: bernardo
      Date: 2021/8/17
      Time: 12:52
      To change this template use File | Settings | File Templates.
    --%>
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <html>
    <head>
        <title>用户详情</title>
    </head>
    <body>
    <h2>用户详情页面</h2>
    <div>用户标示：${user.id}</div>
    <div>用户姓名：${user.username}</div>
    <div>用户人数：${allUserCount}</div>
    </body>
    </html>
    ```

15. 在`resources`目录下新建`dubbo-consumer.xml`配置文件

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">
    
        <!--声明服务消费者名称——保证唯一性-->
        <dubbo:application name="005-link-consumer"/>
    
        <!--引用远程接口服务-->
        <dubbo:reference
                id="userService"
                interface="xyz.rtx3090.service.UserService"
                url="dubbo://localhost:20880"
                registry="N/A"/>
    </beans>
    ```

16. 在`resources`目录下新建`applicationContext.xml`配置文件

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xmlns:mvc="http://www.springframework.org/schema/mvc"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd">
    
        <!--扫描组件-->
        <context:component-scan base-package="xyz.rtx3090.web"/>
    
        <!--配置注解驱动-->
        <mvc:annotation-driven/>
    
        <!--视图解析器-->
        <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
            <property name="prefix" value="/"/>
            <property name="suffix" value=".jsp"/>
        </bean>
    </beans>
    ```

17. 修改`web.xml`配置文件，注册中央调度器

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
             version="4.0">
    
        <!--注册中央调度器-->
        <servlet>
          <servlet-name>dispatcherServlet</servlet-name>
          <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
          <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:applicationContext.xml,classpath:dubbo-consumer.xml</param-value>
          </init-param>
        </servlet>
        <servlet-mapping>
          <servlet-name>dispatcherServlet</servlet-name>
          <url-pattern>/</url-pattern>
        </servlet-mapping>
    </web-app>
    ```

18. 配置并启动`004-link-userService-provider`的tomcat服务器

    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210817171649.png)

    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210817171722.png)

19. 配置并启动`005-link-consumer`的tomcat服务器

    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210817171826.png)

    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210817171846.png)

20. 在浏览器地址栏输入`http://localhost:8080/005/userDetail?id=1`，查看测试结果

    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210817172557.png)

# 注册中心

## 概述

对于服务提供方它需要发布服务，而且由于应用系统的复杂性——服务的数量、类型也不断膨胀；

对于服务消费者，它最关心如何获取到它所需的服务，而面对复杂的应用系统，需要管理大量的服务调用。

而且，对于服务提供方和服务消费方来说，他们还有可能兼具这两种角色，即需要提供服务，又需要消费服务。

通过将服务统一管理起来，可以有效地优化内部应用对服务发布/使用的流程和管理。

服务注册中心可以通过特定协议来完成服务对外的统一，Dubbo提供的注册中心有如下几种类型可供选择：

1. `Multicast`注册中心：组播方式
2. `Redis`注册中心：使用Redis作为注册中心
3. `Simple`注册中心：就是一个dubbo服务，作为注册中心，提供查找服务的功能
4. **`Zookeeper`注册中心：使用Zookeeper作为注册中心（推荐）**

## 注册中心工作方式

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210817172752.png)

## Zookeeper注册中心

Zookeeper是一个高性能的、开源的分布式应用程序协调服务，简称`ZK`。Zookeeper直译为`动物园管理员`，可以理解为windows中的资源管理器或者注册表。他是一个树形结构，这种树形结构和标准文件系统相似。Zookeeper数中的每个节点可以拥有子节点，每个节点表示一个唯一服务资源，Zookeeper运行需要Java环境。

### 下载

> 注意我们这里要下载`3.5.6`版本的，不然可能会出现不兼容的情况

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210817173430.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210817173534.png)

### 安装

将下载的`apache-zookeeper-3.5.6-bin.tar.gz`文件，解压到指定目录位置即可

```bash
tar -zxvf apache-zookeeper-3.5.9-bin.tar.gz -C /usr/local/
```

### 配置文件

#### 配置文件详解

1. `tickTime`：直接翻译为心跳时间，单位毫秒。Zookeeper服务器之间或客户端与服务器之间维持心跳的时间间隔，也就是每个tickTime时间就会发送一个心跳，表明存活状态
2. `dataDir`：数据目录，可以是任意目录。存错zookeeper的快照文件、pid文件，默认为`/tmp/zookeeper`，建议在`zookeeper`安装目录下创建`data`目录，将`dataDir`配置改为`/usr/local/zookeeper-3.5.9/data`

#### 自定义配置文件内容

1. 复制`conf/zoo_sample.cfg`文件到同级目录中改名为`zoo.cfg`
2. 在`apache-zookeeper-3.5.9-bin`根目录下创建`data`文件夹
3. 修改配置文件`conf/zoo.cfg`中的`dataDir=/Users/bernardo/JavaDoc/apache-zookeeper-3.5.9-bin/data`（即上述创建的文件夹路径）
4. 修改配置文件`conf/zoo.cfg`，在` clientPort=2181`下面加入`admin.serverPort=8888`

`conf/zoo.cfg`最终内容：

```cfg
# The number of milliseconds of each tick
tickTime=2000
# The number of ticks that the initial 
# synchronization phase can take
initLimit=10
# The number of ticks that can pass between 
# sending a request and getting an acknowledgement
syncLimit=5
# the directory where the snapshot is stored.
# do not use /tmp for storage, /tmp here is just 
# example sakes.
dataDir=/Users/bernardo/JavaDoc/zookeeper-3.5.9/data
# the port at which the clients will connect
clientPort=2181
admin.serverPort=8888
# the maximum number of client connections.
# increase this if you need to handle more clients
#maxClientCnxns=60
#
# Be sure to read the maintenance section of the 
# administrator guide before turning on autopurge.
#
# http://zookeeper.apache.org/doc/current/zookeeperAdmin.html#sc_maintenance
#
# The number of snapshots to retain in dataDir
#autopurge.snapRetainCount=3
# Purge task interval in hours
# Set to "0" to disable auto purge feature
#autopurge.purgeInterval=1
```

### 启动

 切换到`bin`目录下，执行命令`./zkServer.sh start`

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210817190028.png)

### 关闭

切换到`bin`目录下，执行命令`./zkServer.sh stop`

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210817190144.png)

## Dubbo案例改造—本地使用zookeeper

1. 新建普通maven java子工程`001-zk-interface`，最终项目目录如图所示：

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210818161536.png)

2. 在`java`目录下新建包`xyz.rtx3090.model`，并在其下新建`User`实体类

   ```java
   package xyz.rtx3090.model;
   
   import java.io.Serializable;
   
   public class User implements Serializable {
       private Integer id;
       private String username;
   
       //setter and getter
   
       public Integer getId() {
           return id;
       }
   
       public void setId(Integer id) {
           this.id = id;
       }
   
       public String getUsername() {
           return username;
       }
   
       public void setUsername(String username) {
           this.username = username;
       }
   }
   ```

3. 在`java`目录下新建包`xyz.rtx3090.service`，并在其下新建`UserService`接口

   ```java
   package xyz.rtx3090.service;
   
   import xyz.rtx3090.model.User;
   
   public interface UserService {
       /**
        * 根据用户id查询用户信息
        * @param id 用户id
        * @return 用户信息
        */
       User queryUserById(Integer id);
   }
   ```

4. 新建maven web子工程`002-zk-userService-provider`，最终目录结构如图所示：

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210818161819.png)

5. 修改`pom.xml`文件，添加所需的依赖

   ```xml
   <dependencies>
           <!--spring依赖-->
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-context</artifactId>
               <version>4.3.16.RELEASE</version>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-webmvc</artifactId>
               <version>4.3.16.RELEASE</version>
           </dependency>
           <!--dubbo依赖-->
           <dependency>
               <groupId>com.alibaba</groupId>
               <artifactId>dubbo</artifactId>
               <version>2.6.2</version>
           </dependency>
           <!--接口工程依赖-->
           <dependency>
               <groupId>xyz.rtx3090</groupId>
               <artifactId>001-zk-interface</artifactId>
               <version>1.0-SNAPSHOT</version>
           </dependency>
           <!--注册中心依赖-->
           <dependency>
               <groupId>org.apache.curator</groupId>
               <artifactId>curator-framework</artifactId>
               <version>4.1.0</version>
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
       </build>
   ```

6. 在`java`目录下新建包`xyz.rtx3090.service.impl`，并在其下新建`UserServiceImpl`类

   ```java
   package xyz.rtx3090.service.impl;
   
   import xyz.rtx3090.model.User;
   import xyz.rtx3090.service.UserService;
   
   public class UserServiceImpl implements UserService {
       /**
        * 根据用户id获取用户信息
        * @param id 用户id
        * @return 用户对象
        */
       @Override
       public User queryUserById(Integer id) {
           User user = new User();
           user.setId(id);
           user.setUsername("Jason" + id);
           return user;
       }
   }
   ```

7. 在`resources`目录下创建`dubbo-userService-provider.xml`配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">
   
       <!--声明dubbo服务提供者名称——保证唯一性-->
       <dubbo:application name="002-zk-userService-provider"/>
   
       <!--注册中心zookeeper-->
       <dubbo:registry address="zookeeper://localhost:2181"/>
   
       <!--设置dubbo使用的协议和端口号-->
       <dubbo:protocol name="dubbo" port="20880"/>
   
       <!--暴露服务接口-->
       <dubbo:service interface="xyz.rtx3090.service.UserService" ref="userServiceImpl"/>
   
       <!--加载业务接口的实现类到spring容器中-->
       <bean id="userServiceImpl" class="xyz.rtx3090.service.impl.UserServiceImpl"/>
   </beans>
   ```

8. 修改`web.xml`配置文件，注册监听器

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
   
     <!--注册监听器-->
     <context-param>
       <param-name>contextConfigLocation</param-name>
       <param-value>classpath:dubbo-userService-provider.xml</param-value>
     </context-param>
     <listener>
       <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
     </listener>
   
   </web-app>
   ```

9. 新建maven web子工程`003-zk-consumer`，最终目录结构如图所示：

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210818162352.png)

10. 修改`pom.xml`配置文件，添加所需的依赖

    ```xml
        <dependencies>
            <!--spring依赖-->
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-context</artifactId>
                <version>4.3.16.RELEASE</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-webmvc</artifactId>
                <version>4.3.16.RELEASE</version>
            </dependency>
            <!--dubbo依赖-->
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>dubbo</artifactId>
                <version>2.6.2</version>
            </dependency>
            <!--接口工程依赖-->
            <dependency>
                <groupId>xyz.rtx3090</groupId>
                <artifactId>001-zk-interface</artifactId>
                <version>1.0-SNAPSHOT</version>
            </dependency>
            <!--注册中心依赖-->
            <dependency>
                <groupId>org.apache.curator</groupId>
                <artifactId>curator-framework</artifactId>
                <version>4.1.0</version>
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
        </build>
    ```

11. 在`java`目录下新建包`xyz.rtx3090.web`，并在其下新建`UserController`类

    ```java
    package xyz.rtx3090.web;
    
    import org.springframework.stereotype.Controller;
    import org.springframework.ui.Model;
    import org.springframework.web.bind.annotation.RequestMapping;
    import xyz.rtx3090.model.User;
    import xyz.rtx3090.service.UserService;
    
    import javax.annotation.Resource;
    
    @Controller
    public class UserController {
        @Resource
        private UserService userService;
    
        @RequestMapping(value = "/userDetail")
        public String userDetail(Model model, Integer id) {
            User user = userService.queryUserById(id);
            model.addAttribute("user",user);
            return "userDetail";
        }
    }
    ```

12. 在`webapp`目录下新建`userDetail.jsp`页面

    ```jsp
    <%--
      Created by IntelliJ IDEA.
      User: bernardo
      Date: 2021/8/18
      Time: 15:04
      To change this template use File | Settings | File Templates.
    --%>
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <html>
    <head>
        <title>用户信息</title>
    </head>
    <body>
    <h2>用户详情信息页面</h2>
    <div>用户编号：${user.id}</div>
    <div>用户姓名：${user.username}</div>
    </body>
    </html>
    ```

13. 在`resources`目录下新建`dubbo-consumer.xml`配置文件

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">
    
        <!--声明服务消费者名称——保证唯一性-->
        <dubbo:application name="003-zk-consumer"/>
    
        <!--注册中心zookeeper-->
        <dubbo:registry address="zookeeper://localhost:2181"/>
    
        <!--引用远程接口服务-->
        <dubbo:reference id="userServiceImpl" interface="xyz.rtx3090.service.UserService"/>
    </beans>
    ```

14. 在`resources`目录下新建`applicationContext.xml`spring配置文件

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xmlns:mvc="http://www.springframework.org/schema/mvc"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd">
    
        <!--扫描组件-->
        <context:component-scan base-package="xyz.rtx3090.web"/>
    
        <!--配置注解驱动-->
        <mvc:annotation-driven/>
    
        <!--视图解析器-->
        <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
            <property name="prefix" value="/"/>
            <property name="suffix" value=".jsp"/>
        </bean>
    </beans>
    ```

15. 修改`web.xml`配置文件

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
             version="4.0">
    
        <!--配置中央调度器-->
        <servlet>
            <servlet-name>dispatcherServlet</servlet-name>
            <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
            <init-param>
                <param-name>contextConfigLocation</param-name>
                <param-value>classpath:applicationContext.xml,classpath:dubbo-consumer.xml</param-value>
            </init-param>
        </servlet>
        <servlet-mapping>
            <servlet-name>dispatcherServlet</servlet-name>
            <url-pattern>/</url-pattern>
        </servlet-mapping>
    </web-app>
    ```

16. 切换到`zookeeper-3.5.6/bin/`目录，执行`./zkServer.sh start`来启动zookeeper注册中心

    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210818163839.png)

17. 配置并启动`002-zk-userService-provider`项目的tomcat服务器

    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210818164826.png)

    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210818164903.png)

18. 配置并启动`003-zk-consumer`项目的tomcat服务器

    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210818164927.png)

    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210818164944.png)

19. 在浏览器地址栏输入`http://localhost:8080/003/userDetail?id=1`，查看页面测试结果

    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210818165041.png)



## Dubbo案例改造—远程主机使用zookeeper

> 在`Dubbo案例改造—本地使用zookeeper`的代码基础上进行修改

1. 在远程主机上**下载**、**配置**并**启动**zookeeper，步骤和在本地主机上的一样（记得开放端口`2181`和`8888`)

2. 修改`002-zk-userService-provider`的`dubbo-userService-provider`配置文件

   ```xml
   <!--修改注册中心zookeeper，将localhost换成远程主机的ip-->
   <dubbo:registry address="zookeeper://1.117.144.59:2181"/>
   ```

3. 修改`003-zk-consumer`的`dubbo-consumer`配置文件

   ```xml
   <!--修改注册中心zookeeper，将localhost换成远程主机的ip-->
   <dubbo:registry address="zookeeper://1.117.144.59:2181"/>
   ```

4. 配置并启动`002`、`003`项目的tomcat服务器，在浏览器地址栏输入`http://localhost:8080/003/userDetail?id=2`，查看页面测试结果

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210818171538.png)

# Dubbo配置

## 关闭检查

dubbo缺省会在启动时检查依赖的服务是否可用，不可用时会抛出异常，阻止Spring初始化完成，以便上线时能及时发现问题，默认`check=true`。通过`check=false`关闭检查。比如测试时，有些服务不关心，或者出现了循环依赖，必须有一方先启动。

1. 例一：关闭某人服务的启动时检查

   ```
   <dubbo:reference interface="xyz.rtx3090.BarService" check="false"/>
   ```

2. 例二：关闭注册中心启动时检查

   ```
   <dubbo:registry check="false"/>
   ```

   > 默认启动服务时，检查中心存在并已运行，注册中心不启动会报错

## 重试次数

消费者访问提供者，如果访问失败，则切换重试访问其他服务器，但重试会带来更长延迟。访问时间变长，用户的体验较差。多次重新访问服务器有可能访问成功。可通过 `retries="2"`来设置重试次数（不含第一次）

1. `<dubbo:service retries="2"/>`
2. `<dubbo:reference retries="2"/>`

## 超时时间

由于网络或服务端不可靠，会导致调用出现一种不确定的中间状态（超时）。为了避免超时导致客户端资源（线程）挂起耗尽，必须设置超时时间。

通过设置`timeout`来调整远程服务超时时间（毫秒）

1. 调整消费端服务超时时间

   ```
   <dubbo:reference interface="xyz.rtx3090.BerServicee" timeout="2000"/>
   ```

2. 调整服务端超时时间

   ```
   <dubbo:server interface="xyz.rtx3090.BarService" timeout="2000"/>
   ```

## 版本号

每个接口都应定义版本号，为后续不兼容升级提供可能。当一个接口有不同的实现，项目早期使用的一个实现类，之后创建接口的新实现类。区分不同接口实现使用version，特别是项目需要把早期接口的实现全部换位新的实现类，也需要实现version。

可以用版本号从早期实现过度到欣的接口实现，版本号不同的服务相互间不引用。

可以按照以下的步骤进行版本迁移：

1. 在低压力时间段，先升级一半提供者为新版本
2. 再将所有消费者升级为新版本
3. 然后将剩下的一半提供者升级为新版本

## Dubbo案例改造-多版本+远程zookeeper

有时候我们在升级新版本的时候同样需要兼顾旧版本，这个时候我们就需要【多版本】的方案了

1. 新建普通maven java工程`009-zk-multi-interface`，最终项目目录结构如图所示：

   ![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210819160428.png)

2. 在java目录下新建包`xyz.rtx3090.model`，并在其下新建`User`实体类

   ```java
   package xyz.rtx3090.model;
   
   import java.io.Serializable;
   
   public class User implements Serializable {
       private Integer id;
       private String username;
   
       //setter and getter
   
       public Integer getId() {
           return id;
       }
   
       public void setId(Integer id) {
           this.id = id;
       }
   
       public String getUsername() {
           return username;
       }
   
       public void setUsername(String username) {
           this.username = username;
       }
   }
   ```

3. 在java目录下新建包`xyz.rtx3090.service`，并在其下新建`UserService`类

   ```java
   package xyz.rtx3090.service;
   
   import xyz.rtx3090.model.User;
   
   public interface UserService {
       /**
        * 根据用户id获取用户消息
        * @param id 用户id
        * @return 用户对象
        */
       User queryUserById(Integer id);
   }
   ```

4. 新建maven web工程，最终项目结构如图所示：

   ![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210819160651.png)

5. 修改`pom.xml`文件，添加所需依赖

   ```xml
     <dependencies>
       <!--spring依赖-->
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-context</artifactId>
         <version>4.3.16.RELEASE</version>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-webmvc</artifactId>
         <version>4.3.16.RELEASE</version>
       </dependency>
       <!--dubbo依赖-->
       <dependency>
         <groupId>com.alibaba</groupId>
         <artifactId>dubbo</artifactId>
         <version>2.6.2</version>
       </dependency>
   
       <!--接口工程-->
       <dependency>
         <groupId>xyz.rtx3090</groupId>
         <artifactId>009-zk-multi-interface</artifactId>
         <version>1.0-SNAPSHOT</version>
       </dependency>
       <!--注册中心依赖-->
       <dependency>
         <groupId>org.apache.curator</groupId>
         <artifactId>curator-framework</artifactId>
         <version>4.1.0</version>
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
     </build>
   ```

6. 在java目录下新建包`xyz.rtx3090.service.impl`包，并在其下新建`UserServiceImpl`类和`UserServiceImpl2`类

   ```java
   package xyz.rtx3090.service.impl;
   
   import xyz.rtx3090.model.User;
   import xyz.rtx3090.service.UserService;
   
   public class UserServiceImpl implements UserService {
       @Override
       public User queryUserById(Integer id) {
           User user = new User();
           user.setId(id);
           user.setUsername("Jason" + id);
           return user;
       }
   }
   ```

   ```java
   package xyz.rtx3090.service.impl;
   
   import xyz.rtx3090.model.User;
   import xyz.rtx3090.service.UserService;
   
   public class UserServiceImpl2 implements UserService {
       @Override
       public User queryUserById(Integer id) {
           User user = new User();
           user.setId(id);
           user.setUsername("Bernardo" + id);
           return user;
       }
   }
   ```

7. 在resources目录下新建`dubbo-userService-multi-provider.xml`配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
          xmlns:dubbbo="http://dubbo.apache.org/schema/dubbo"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">
   
       <!--声明dubbo服务提供者名称：保证唯一性-->
       <dubbo:application name="010-zk-userService-multi-provider"/>
   
       <!--声明dubbo协议和端口号-->
       <dubbbo:protocol name="dubbo" port="20880"/>
   
       <!--声明注册中心-->
       <dubbo:registry address="zookeeper://1.117.144.59:2181"/>
   
   
       <!--多版本，暴露服务接口（不管是不是多版本，最好都提供版本号）-->
       <dubbo:service interface="xyz.rtx3090.service.UserService" ref="userService1" version="1.0.0"/>
       <dubbo:service interface="xyz.rtx3090.service.UserService" ref="userService2" version="2.0.0"/>
   
       <bean id="userService1" class="xyz.rtx3090.service.impl.UserServiceImpl"/>
       <bean id="userService2" class="xyz.rtx3090.service.impl.UserServiceImpl2"/>
   </beans>
   ```

8. 修改`web.xml`配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
   
       <!--注册监听器-->
       <context-param>
         <param-name>contextConfigLocation</param-name>
         <param-value>classpath:dubbo-userService-multi-provider.xml</param-value>
       </context-param>
       <listener>
         <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
       </listener>
   </web-app>
   ```

9. 新建maven web工程`011-zk-multi-consumer`，最终项目目录结构如图所示：

   ![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210819161106.png)

10. 修改`pom.xml`配置文件，添加项目所需依赖

    ```xml
      <dependencies>
        <!--spring依赖-->
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>4.3.16.RELEASE</version>
        </dependency>
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-webmvc</artifactId>
          <version>4.3.16.RELEASE</version>
        </dependency>
        <!--dubbo依赖-->
        <dependency>
          <groupId>com.alibaba</groupId>
          <artifactId>dubbo</artifactId>
          <version>2.6.2</version>
        </dependency>
    
        <!--接口工程-->
        <dependency>
          <groupId>xyz.rtx3090</groupId>
          <artifactId>009-zk-multi-interface</artifactId>
          <version>1.0-SNAPSHOT</version>
        </dependency>
        <!--注册中心依赖-->
        <dependency>
          <groupId>org.apache.curator</groupId>
          <artifactId>curator-framework</artifactId>
          <version>4.1.0</version>
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
      </build>
    ```

11. 在java目录下新建包`xyz.rtx3090.web`，并在其下新建类`UserController`

    ```xml
    package xyz.rtx3090.web;
    
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.beans.factory.annotation.Qualifier;
    import org.springframework.stereotype.Controller;
    import org.springframework.ui.Model;
    import org.springframework.web.bind.annotation.RequestMapping;
    import xyz.rtx3090.model.User;
    import xyz.rtx3090.service.UserService;
    
    @Controller
    public class UserController {
        @Autowired
        @Qualifier("userServiceImpl")
        //这里的byName自动注入是依据`dubbo-multi-consumer`配置文件中的`dubbo:reference`标签属性`id`
        private UserService userServiceImpl;
    
        @Autowired
        @Qualifier("userServiceImpl2")
        private UserService userServiceImpl2;
    
        @RequestMapping(value = "/userDetail")
        public String userDetail(Model model, Integer id) {
            User user1 = userServiceImpl.queryUserById(id);
            User user2 = userServiceImpl2.queryUserById(id);
            model.addAttribute("user1",user1);
            model.addAttribute("user2",user2);
            return "userDetail";
        }
    }
    ```

12. 在resources目录下新建配置文件`dubbo-multi-consumer.xml`

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">
    
        <!--声明dubbo服务消费者名称：保证服务名称的唯一性-->
        <dubbo:application name="011-zk-multi-consumer"/>
    
        <!--指定注册中心-->
        <dubbo:registry address="zookeeper://1.117.144.59:2181"/>
    
        <!--引用远程接口服务-->
        <dubbo:reference id="userServiceImpl" interface="xyz.rtx3090.service.UserService" version="1.0.0"/>
        <dubbo:reference id="userServiceImpl2" interface="xyz.rtx3090.service.UserService" version="2.0.0"/>
    </beans>
    ```

13. 在resources目录下新建配置文件`applicationContext.xml`

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xmlns:mvc="http://www.springframework.org/schema/mvc"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd">
    
        <!--组件扫描器-->
        <context:component-scan base-package="xyz.rtx3090.web"/>
    
        <!--配置注解驱动-->
        <mvc:annotation-driven/>
    
        <!--视图解析器-->
        <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
            <property name="prefix" value="/"/>
            <property name="suffix" value=".jsp"/>
        </bean>
    </beans>
    ```

14. 修改`web.xml`配置文件

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
             version="4.0">
    
        <!--注册中央调度器-->
        <servlet>
          <servlet-name>dispatcherServlet</servlet-name>
          <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
          <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:dubbo-multi-consumer.xml,classpath:applicationContext.xml</param-value>
          </init-param>
        </servlet>
        <servlet-mapping>
          <servlet-name>dispatcherServlet</servlet-name>
          <url-pattern>/</url-pattern>
        </servlet-mapping>
    </web-app>
    ```

15. 在webapp目录下新建`userDetail.jsp`页面

    ```xml
    <%--
      Created by IntelliJ IDEA.
      User: bernardo
      Date: 2021/8/19
      Time: 14:51
      To change this template use File | Settings | File Templates.
    --%>
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <html>
    <head>
        <title>用户信息页面</title>
    </head>
    <body>
    <h2>用户一</h2>
    <div>用户编号：${user1.id}</div>
    <div>用户姓名：${user1.username}</div>
    
    <h2>用户二</h2>
    <div>用户编号：${user2.id}</div>
    <div>用户姓名：${user2.username}</div>
    </body>
    </html>
    ```

16. 启动远程主机的zookeeper

17. 配置并启动项目`010-zk-userService-multi-provider`的tomcat服务器（注意修改端口号，避免两个tomcat服务器冲突）

    ![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210819161614.png)

    ![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210819161650.png)

18. 配置比启动项目`011-zk-multi-consumer`的tomcat服务器

    ![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210819161710.png)

    ![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210819161725.png)

19. 在浏览器地址栏中输入`http://localhost:8080/011/userDetail?id=1`，查看页面测试结果

# 监控中心

## 什么是监控中心

dubbo的使用其实只需要有注册中心、消费者、提供者三个就可以使用了，但是并不能看到有哪些消费者和提供者。为了更好的调试、发现问题、解决问题，因此引入`dubbo-admin`，通过`dubbo-admin`可以对消费者和提供者进行管理，可以在dubbo引用部署做动态的调整、服务管理。

1. dubbo-admin

   图形化服务管理页面，安装时需要执行注册中心地址，即可从注册中心中获取到所有的提供这者、消费者进行配置管理

2. dubbo-monitor-simple

   简单的监控中心

## 使用监控中心

1. 进入`https://github.com/apache/dubbo-admin`下载`dubbo-admin`工程

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210820045845.png)

2. 使用idea打开下载的`dubbo-admin`工程

3. 进入`dubbo-admin-develop/dubbo-admin-server`目录，打开`pom.xml`文件，添加`maven-antrun-plugin`的`grouid`为`org.apache.maven.plugins`，并确定其他内容没有报红

   ```xml
               <plugin>
                   <groupId>org.apache.maven.plugins</groupId>
                   <artifactId>maven-antrun-plugin</artifactId>
                   <version>1.8</version>
                   <executions>
                       <execution>
                           <phase>verify</phase>
                           <configuration>
                               <tasks>
                                   <copy file="target/dubbo-admin-server-${project.version}.jar"
                                         tofile="../dubbo-admin-distribution/target/dubbo-admin-${project.version}.jar"/>
                               </tasks>
                           </configuration>
                           <goals>
                               <goal>run</goal>
                           </goals>
                       </execution>
                   </executions>
               </plugin>
   ```

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210820050819.png)

4. 进入`dubbo-admin-develop/dubbo-admin-server/srcd/main/resources`目录，打开`application.properties`配置文件，设置一些ip和端口信息

   ```properties
   # ip地址和端口号改为自己zookeeper设置ip端口
   admin.registry.address=zookeeper://127.0.0.1:2181
   admin.config-center=zookeeper://127.0.0.1:2181
   admin.metadata-report.address=zookeeper://127.0.0.1:2181
   
   # 后面在浏览器登录admin系统的账号密码
   admin.root.user.name=root
   admin.root.user.password=root
   
   # 新增设置端口号为7001（默认为8080，与tomcat冲突）
   server.port=7001
   ```

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210820050503.png)

5. 点击右侧的maven工具，进入`dubbo-admin(root)/Lifecyde`先点击`clean`选项，然后点击`package`选择构建Jar包

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210820051014.png)

6. 启动`zookeeper`和`dubbo`项目

7. 等Jar包构建完成和上面启动完毕，进入`dubbo-admin-develop/dubbo-admin-distribution/target`目录，在此目录下打开终端，执行`java -jar dubbo-admin-0.3.0.jar`命令

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210820051222.png)

8. 在浏览器地址栏输入`http://localhost:7001`（7001是上面我自己设置的端口号），即可对dubbo项目进行监控

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210820051545.png)

---

# END