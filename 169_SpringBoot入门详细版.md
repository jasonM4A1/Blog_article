<!-- vim-markdown-toc Marked -->

* [SpringBoot2核心技术-基础入门](#springboot2核心技术-基础入门)
    * [Spring与SpringBoot](#spring与springboot)
        * [Spring简介](#spring简介)
            * [Spring能力](#spring能力)
            * [Spring生态](#spring生态)
            * [响应式编程](#响应式编程)
        * [SpringBoot简介](#springboot简介)
            * [SpringBoot优点](#springboot优点)
            * [SpringBoot缺点](#springboot缺点)
        * [时代背景](#时代背景)
            * [微服务](#微服务)
            * [分布式](#分布式)
            * [云原生(CloudNative)](#云原生(cloudnative))
        * [SpringBoot官网文档](#springboot官网文档)
    * [第一个SpringBoot程序](#第一个springboot程序)
        * [环境要求](#环境要求)
        * [创建步骤](#创建步骤)
    * [自动配置原理](#自动配置原理)
        * [SpringBoot特点](#springboot特点)
            * [依赖管理](#依赖管理)
            * [自动配置](#自动配置)
        * [容器功能](#容器功能)
            * [组件添加](#组件添加)

<!-- vim-markdown-toc -->

# SpringBoot2核心技术-基础入门

**学习要求**
+ 熟悉Spring基础
+ 熟悉Maven使用

**环境要求**
+ Java8及以上
+ Maven 3.3及以上：https://docs.spring.io/spring-boot/docs/current/reference/html/getting-started.html#getting-started-system-requirements

**学习资料**
+ 文档地址： https://www.yuque.com/atguigu/springboot （文档不支持旧版本IE、Edge浏览器，请使用chrome或者firefox）
+ 视频地址： http://www.gulixueyuan.com/    https://www.bilibili.com/video/BV19K4y1L7MT?p=1
+ 源码地址：https://gitee.com/leifengyang/springboot2

## Spring与SpringBoot

### Spring简介

#### Spring能力
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210905001619.png)

#### Spring生态
https://spring.io/projects/spring-boot

+ web开发
+ 数据访问
+ 安全控制
+ 分布式
+ 消息服务
+ 移动开发
+ 批处理

#### 响应式编程 
在Spring5进行了重大的升级，增加了响应式编程
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210905001828.png)

### SpringBoot简介
> Spring Boot makes it easy to create stand-alone, production-grade Spring based Applications that you can "just run".
> 能快速创建出生产级别的Spring应用
> SpringBoot是整合Spring技术栈的一站式框架
> SpringBoot是简化Spring技术栈的快速开发脚手架

#### SpringBoot优点
+ 创建独立Spring应用
+ 内嵌web服务器
+ 自动starter依赖，简化构建配置
+ 自动配置Spring以及第三方功能
+ 提供生产级别的监控、健康检查及外部化配置
+ 无代码生产、无需编写XML

#### SpringBoot缺点
+ 人称版本帝，迭代快，需要时刻关注变化
+ 封装太深，内部原理复杂，不容易精通

### 时代背景

#### 微服务
James Lewis and Martin Fowler (2014)  提出微服务完整概念。https://martinfowler.com/microservices/
+ 微服务是一种结构风格
+ 一个应用拆分为一组小型服务
+ 每个服务运行在自己的进程内，也就是可独立部署和升级
+ 服务之间使用轻量级HTTP交互
+ 服务围绕业务功能拆分
+ 可以由全自动部署机制独立部署
+ 去中心化，服务自治。服务可以使用不同的语言、不同的存储技术

#### 分布式
分布式系统是由一组通过网络进行通信、为了完成共同的任务而协调工作的计算机节点组成的系统。分布式系统的出现是为了用廉价的、普通的机器完成单个计算机无法完成的计算、存储任务。其目的是利用更多的机器，处理更多的数据。
首先需要明确的是，只有当单个节点的处理能力无法满足日益增长的计算、存储任务的时候，且硬件的提升（加内存、加磁盘、使用更好的CPU）高昂到得不偿失的时候，应用程序也不能进一步优化的时候，我们才需要考虑分布式系统。因为，分布式系统要解决的问题本身就是和单机系统一样的，而由于分布式系统多节点、通过网络通信的拓扑结构，会引入很多单机系统没有的问题，为了解决这些问题又会引入更多的机制、协议，带来更多的问题。

**分布式的困难点**
+ 远程调用
+ 服务发现
+ 负载均衡
+ 服务容错
+ ......

**解决方案**
SpringBoot + SpringCloud
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210905003925.png)

#### 云原生(CloudNative)

**概述**
+ 云原生是一种构建和运行应用程序的方法，是一套技术体系和方法论
+ Cloud表示应用程序位于云中，而不是传统的数据中心
+ Native表示应用程序从设计之初即考虑到云的环境，原生为云而设计，在云上以最佳姿势运行，充分利用和发挥云平台的弹性+分布式优势
> 总而言之，符合云原生架构的应用程序应该是：采用开源堆栈（K8S+Docker）进行容器化，基于微服务架构提高灵活性和可维护性，借助敏捷方法、DevOps支持持续迭代和运维自动化，利用云平台设施实现弹性伸缩、动态调度、优化资源利用率

**四要素**
1. 微服务
    几乎每个云原生的定义都包含微服务，跟微服务相对的是单体应用. 微服务架构的好处就是按function切了之后，服务解耦，内聚更强，变更更易；另一个划分服务的技巧据说是依据DDD来搞
2. 容器化
    Docker是应用最为广泛的容器引擎，在思科谷歌等公司的基础设施中大量使用，是基于LXC技术搞的，容器化为微服务提供实施保障，起到应用隔离作用，K8S是容器编排系统，用于容器管理，容器间的负载均衡，谷歌搞的，Docker和K8S都采用Go编写，都是好东西
3. DevOps
    这是个组合词，Dev+Ops，就是开发和运维合体，不像开发和产品，经常刀刃相见，实际上DevOps应该还包括测试，DevOps是一个敏捷思维，是一个沟通文化，也是组织形式，为云原生提供持续交付能力
4. 持续交付
    持续交付是不误时开发，不停机更新，小步快跑，反传统瀑布式开发模型，这要求开发版本和稳定版本并存，其实需要很多流程和工具支撑

### SpringBoot官网文档
文档官网: https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/
中文文档: https://www.springcloud.cc/spring-boot.html

**文档分析**
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210905004731.png)
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210905004744.png)

## 第一个SpringBoot程序

### 环境要求
+ Java8 & 兼容Java14
+ Maven 3.3+
+ idea2019+

### 创建步骤
1. 提前设置maven配置文件的镜像和jdk版本
    ```xml
    <mirrors>
      <mirror>
        <id>nexus-aliyun</id>
        <mirrorOf>central</mirrorOf>
        <name>Nexus aliyun</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public</url>
      </mirror>
    </mirrors>

  <profiles>
         <profile>
              <id>jdk-1.8</id>
              <activation>
                <activeByDefault>true</activeByDefault>
                <jdk>1.8</jdk>
              </activation>
              <properties>
                <maven.compiler.source>1.8</maven.compiler.source>
                <maven.compiler.target>1.8</maven.compiler.target>
                <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
              </properties>
         </profile>
  </profiles>
    ```
2. 创建一个普通的Java Maven工程`01-HelloSpringBoot`
3. 配置pom文件，引入springboot依赖
    ```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.4.RELEASE</version>
    </parent>


    <dependencies>
         <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
         </dependency>
    </dependencies>
    ```
4. 创建一个主程序类`Application`
    ```java
    package xyz.rtx3090.springboot;
    
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    
    @SpringBootApplication//表明这是一个SpringBoot应用
    public class Application {
        public static void main(String[] args) {
            SpringApplication.run(Application.class,args);
        }
    }
    ```
5. 编写业务类`HelloController`
    ```java
    package xyz.rtx3090.springboot.controller;
    
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RestController;
    
    @RestController//相当于@Controller + @ResponseBody
    public class HelloController {
        @RequestMapping(value = "/hello")
        public String handle01() {
            return "Hello, Spring Boot 2!";
        }
    }
    ```
6. 在`resources`目录下创建`application.properties`配置文件，对springboot进行配置
    ```properties
    #设置内嵌Tomcat端口号(默认为8080)
    server.port=8088
    ```
7. 直接启动运行`Application`类的main方法，在浏览器输入`http://localhost:8088/hello`进行访问测试
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210905015958.png)
8. 我们在pom文件中加入打包插件，可以将我们的springboot项目打包成jar包（jar包可以直接在其他服务器运行，只需要java环境）
    ```xml
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
    ```
9. 执行打包jar
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210905020501.png)
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210905020629.png)
10. 在jar所在目录执行`java -jar 01-HelloSpringBoot-1.0.0.jar`, 运行jar包
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210905020744.png)
11. 在浏览器依旧输入`http://localhost:8088/hello`来进行访问测试
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210905020837.png)

## 自动配置原理

### SpringBoot特点

#### 依赖管理
引入`spring-boot-starter-parent`父项目依赖
+ 它会自动帮助我们引入许多其他版本依赖, 例如：springmvc的依赖
+ 它帮我们声明开发中常用的依赖版本号，我们使用时只需要导入依赖，而不需要设置版本号，方便了项目依赖版本号的统一（当然我们可以自己手动修改）

#### 自动配置
+ 自动引入Tomcat依赖，并配置Tomcat
+ 引入全套SpringMVC组件，并自动配置SpringMVC常见组件
+ 自动配置解决Web开发中的常见问题：如字符编码问题
+ 自动扫描主程序所在包及其子包的所有组件
    + 通过`@SpringBootApplication(scanBasePackages="xyz.rtx3090")`或者`@ComponentScan()`来自定义扫描路径
+ SpringBoot帮我们引入的各种配置都有默认值，这些默认配置最终都会映射到某个类上
+ 按需加载这些自动配置项目，只要引入了指定的场景，自动配置才会开启

### 容器功能

#### 组件添加

1. @Configuration
    + 作用: 标示一个类为配置类
    + 位置: 类上
    + 属性:
        + 属性名: proxyBeanMethods
        + 属性值: false | true
        + 作用: 值为true时，确保每个@Bean注释的方法组册的组件是单实例的；值为false时，每个@Bean注释的方式组册的组件都是新创建的
        > 实际开发过程中，当配置类组件之间无依赖关系，我们为了加速容器启动过程、减少判断，属性值采用false；当配置类组件之间有依赖关系，方法会被调用得到之间单实例组件，属性值采用true
> 配置类本身也是组件
2. @Bean
    + 作用: 向spring中注册组件
    + 位置: 方法上
    + 属性:
        + 属性名: value
        + 属性值: 注册到容器中的组件名
        + 作用: 手动指定注册到spring容器中的组件的名字
> 组件名为：方法名
> 组件类型：方法返回值类型

```java
package xyz.rtx3090.springboot.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import xyz.rtx3090.springboot.model.Person;
import xyz.rtx3090.springboot.model.User;

//声明这是一个配置类
@Configuration(proxyBeanMethods = true)//开启单例模式
public class MyConfig {
    @Bean//向spring容器中添加组件
    public User user() {
        User user = new User();
        user.setUsername("Jason");
        user.setPassword(123456);
        user.setBob(person());
        return user;
    }

    @Bean("person")//手动指定注册的组件名
    public Person person() {
        Person person = new Person();
        person.setName("BernardoLi");
        person.setAge(22);
        return person;
    }
}
```

3. @Import
    + 作用: 直接向spring容器中添加传入的参数类型的组件（可多个）
    + 位置: 类上
    + 属性: 需要添加到spring容器中的类的class
```java
package xyz.rtx3090.springboot.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;
import xyz.rtx3090.springboot.model.Person;
import xyz.rtx3090.springboot.model.User;

@Import(Person.class)//直接向spring容器中添加Person类的组件
//声明这是一个配置类
@Configuration(proxyBeanMethods = false)//开启单例模式
public class MyConfig {
    @Bean//向spring容器中添加组件
    public User user() {
        User user = new User();
        user.setUsername("Jason");
        user.setPassword(123456);
        return user;
    }
    
}
```

4. @ComponentScan
    + 作用: 手动指定扫描组件的文件夹
    + 位置: 类上
    + 属性: 需要扫描组件的文件夹

5. @ConditionalOnBean
    + 作用: 根据条件进行组件扫描装配，满足条件进行装配，不满足条件则不装配
    + 位置: 类上
    + 属性: name、type等
```java
package xyz.rtx3090.springboot.config;

import org.springframework.boot.autoconfigure.condition.ConditionalOnBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;
import xyz.rtx3090.springboot.model.Person;
import xyz.rtx3090.springboot.model.User;

@ConditionalOnBean(name = "tom")//当类中有注册名为tom的组件时，这个配置类才会生效
@Import(Person.class)//直接向spring容器中添加Person类的组件
//声明这是一个配置类
@Configuration(proxyBeanMethods = false)//开启单例模式
public class MyConfig {
    @Bean//向spring容器中添加组件
    public User user() {
        User user = new User();
        user.setUsername("Jason");
        user.setPassword(123456);
        return user;
    }

}
```

6. @Controller、@Service、@Repository等注解的使用与在spring、springmvc中的使用一致

#### 原生配置文件引入

有时候我们需要将原来通过`<bean>`标签来注册组件的xml配置文件，引入到我们通过方法来注册组件的配置类中

这个时候我们就需要用到`@ImportResource`注解

+ **通过`<bean>`标签注册组件的xml配置文件`beans.xml`**

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
  
      <bean id="one" class="xyz.rtx3090.springboot.model.User">
          <property name="username" value="王多余"/>
          <property name="password" value="123456"/>
          <property name="bob" ref="two"/>
      </bean>
      
      <bean id="two" class="xyz.rtx3090.springboot.model.Person">
          <property name="name" value="卧龙"/>
          <property name="age" value="34"/>
      </bean>
  </beans>
  ```

+ 在通过方式注册组件的配置类中引入上面的xml配置文件

  ```java
  package xyz.rtx3090.springboot.config;
  
  import org.springframework.context.annotation.Bean;
  import org.springframework.context.annotation.Configuration;
  import org.springframework.context.annotation.ImportResource;
  import xyz.rtx3090.springboot.model.User;
  
  @ImportResource("classpath:beans.xml")//导入注册组件的beans.xml配置文件
  //声明这是一个配置类
  @Configuration(proxyBeanMethods = false)//开启单例模式
  public class MyConfig {
      @Bean//向spring容器中添加组件
      public User user() {
          User user = new User();
          user.setUsername("Jason");
          user.setPassword(123456);
          return user;
      }
  
  }
  ```

#### 配置绑定

1. **@ConfigurationProperties**

   我们有时候还有在`application.properties`配置文件中设置一些值，然后将这些值封装到JavaBean实体类中。

   为了实现上述的功能，我们可以通过`@ConfigurationProperties`注解来实现

   **设置值的properties配置文件**

   ```properties
   
   ```

   **配置类**

   ```java
   ```

   

