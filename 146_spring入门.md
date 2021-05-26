---
title: spring入门
date: 2021-05-25 17:14:00
tags:
	- SSM
	- spring
categories:
	- SSM
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/wallhaven-4gpz9l.jpg
---

# spring概述

Spring : 春天 --->给软件行业带来了春天

2002年，Rod Jahnson首次推出了Spring框架雏形interface21框架。

2004年3月24日，Spring框架以interface21框架为基础，经过重新设计，发布了1.0正式版。

很难想象Rod Johnson的学历 , 他是悉尼大学的博士，然而他的专业不是计算机，而是音乐学。

Spring理念 : 使现有技术更加实用 . 本身就是一个大杂烩 , 整合现有的框架技术

官网 : http://spring.io/

官方下载地址 : https://repo.spring.io/libs-release-local/org/springframework/spring/

GitHub : https://github.com/spring-projects

# spring优点

1、Spring是一个开源免费的框架 , 容器  .

2、Spring是一个轻量级的框架 , 非侵入式的 .

**3、控制反转 IoC  , 面向切面 Aop**

4、对事物的支持 , 对框架的支持

.......

一句话概括：

**Spring是一个轻量级的控制反转(IoC)和面向切面(AOP)的容器（框架）。**

# spring组成

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210525171634.png)

Spring 框架是一个分层架构，由 7 个定义良好的模块组成。Spring 模块构建在核心容器之上，核心容器定义了创建、配置和管理 bean 的方式 .

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210525171723.png)

组成 Spring 框架的每个模块（或组件）都可以单独存在，或者与其他一个或多个模块联合实现。每个模块的功能如下：

- **核心容器**：核心容器提供 Spring 框架的基本功能。核心容器的主要组件是 BeanFactory，它是工厂模式的实现。BeanFactory 使用*控制反转*（IOC） 模式将应用程序的配置和依赖性规范与实际的应用程序代码分开。
- **Spring 上下文**：Spring 上下文是一个配置文件，向 Spring 框架提供上下文信息。Spring 上下文包括企业服务，例如 JNDI、EJB、电子邮件、国际化、校验和调度功能。
- **Spring AOP**：通过配置管理特性，Spring AOP 模块直接将面向切面的编程功能 , 集成到了 Spring 框架中。所以，可以很容易地使 Spring 框架管理任何支持 AOP的对象。Spring AOP 模块为基于 Spring 的应用程序中的对象提供了事务管理服务。通过使用 Spring AOP，不用依赖组件，就可以将声明性事务管理集成到应用程序中。
- **Spring DAO**：JDBC DAO 抽象层提供了有意义的异常层次结构，可用该结构来管理异常处理和不同数据库供应商抛出的错误消息。异常层次结构简化了错误处理，并且极大地降低了需要编写的异常代码数量（例如打开和关闭连接）。Spring DAO 的面向 JDBC 的异常遵从通用的 DAO 异常层次结构。
- **Spring ORM**：Spring 框架插入了若干个 ORM 框架，从而提供了 ORM 的对象关系工具，其中包括 JDO、Hibernate 和 iBatis SQL Map。所有这些都遵从 Spring 的通用事务和 DAO 异常层次结构。
- **Spring Web 模块**：Web 上下文模块建立在应用程序上下文模块之上，为基于 Web 的应用程序提供了上下文。所以，Spring 框架支持与 Jakarta Struts 的集成。Web 模块还简化了处理多部分请求以及将请求参数绑定到域对象的工作。
- **Spring MVC 框架**：MVC 框架是一个全功能的构建 Web 应用程序的 MVC 实现。通过策略接口，MVC 框架变成为高度可配置的，MVC 容纳了大量视图技术，其中包括 JSP、Velocity、Tiles、iText 和 POI。

# spring拓展

**Spring Boot与Spring Cloud**

- Spring Boot 是 Spring 的一套快速配置脚手架，可以基于Spring Boot 快速开发单个微服务;
- Spring Cloud是基于Spring Boot实现的；
- Spring Boot专注于快速、方便集成的单个微服务个体，Spring Cloud关注全局的服务治理框架；
- Spring Boot使用了约束优于配置的理念，很多集成方案已经帮你选择好了，能不配置就不配置 , Spring Cloud很大的一部分是基于Spring Boot来实现，Spring Boot可以离开Spring Cloud独立使用开发项目，但是Spring Cloud离不开Spring Boot，属于依赖的关系。
- SpringBoot在SpringClound中起到了承上启下的作用，如果你要学习SpringCloud必须要学习SpringBoot。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210525171946.png)

# IoC基础

## 采用spring前

新建一个空白的maven项目

我们先用我们原来的方式写一段代码 .

1、先写一个UserDao接口

```java
public interface UserDao {
   public void getUser();
}
```

2、再去写Dao的实现类

```java
public class UserDaoImpl implements UserDao {
   @Override
   public void getUser() {
       System.out.println("获取用户数据");
  }
}
```

3、然后去写UserService的接口

```java
public interface UserService {
   public void getUser();
}
```

4、最后写Service的实现类

```java
public class UserServiceImpl implements UserService {
   private UserDao userDao = new UserDaoImpl();

   @Override
   public void getUser() {
       userDao.getUser();
  }
}
```

5、测试一下

```java
@Test
public void test(){
   UserService service = new UserServiceImpl();
   service.getUser();
}
```

这是我们原来的方式 , 开始大家也都是这么去写的对吧 . 那我们现在修改一下 .

把Userdao的实现类增加一个 .

```java
public class UserDaoMySqlImpl implements UserDao {
   @Override
   public void getUser() {
       System.out.println("MySql获取用户数据");
  }
}
```

紧接着我们要去使用MySql的话 , 我们就需要去service实现类里面修改对应的实现

```java
public class UserServiceImpl implements UserService {
   private UserDao userDao = new UserDaoMySqlImpl();

   @Override
   public void getUser() {
       userDao.getUser();
  }
}
```

在假设, 我们再增加一个Userdao的实现类 .

```java
public class UserDaoOracleImpl implements UserDao {
   @Override
   public void getUser() {
       System.out.println("Oracle获取用户数据");
  }
}
```

那么我们要使用Oracle , 又需要去service实现类里面修改对应的实现 . 假设我们的这种需求非常大 , 这种方式就根本不适用了, 甚至反人类对吧 , 每次变动 , 都需要修改大量代码 . 这种设计的耦合性太高了, 牵一发而动全身 .

**那我们如何去解决呢 ?** 

我们可以在需要用到他的地方 , 不去实现它 , 而是留出一个接口 , 利用set , 我们去代码里修改下 .

```java
public class UserServiceImpl implements UserService {
   private UserDao userDao;
// 利用set实现
   public void setUserDao(UserDao userDao) {
       this.userDao = userDao;
  }

   @Override
   public void getUser() {
       userDao.getUser();
  }
}
```

现在去我们的测试类里 , 进行测试 ;

```java
@Test
public void test(){
   UserServiceImpl service = new UserServiceImpl();
   service.setUserDao( new UserDaoMySqlImpl() );
   service.getUser();
   //那我们现在又想用Oracle去实现呢
   service.setUserDao( new UserDaoOracleImpl() );
   service.getUser();
}
```

大家发现了区别没有 ? 可能很多人说没啥区别 . 但是同学们 , 他们已经发生了根本性的变化 , 很多地方都不一样了 . 仔细去思考一下 , 以前所有东西都是由程序去进行控制创建 , 而现在是由我们自行控制创建对象 , 把主动权交给了调用者 . 程序不用去管怎么创建,怎么实现了 . 它只负责提供一个接口 .

这种思想 , 从本质上解决了问题 , 我们程序员不再去管理对象的创建了 , 更多的去关注业务的实现 . 耦合性大大降低 . 这也就是IOC的原型 !

## 采用spring后

> 推荐先查阅下面的《helloSpring》节的内容，再来阅读此节内容

想使用spring来实现上述程序相同的功能，我们只需要新增并配置一个beans.xml文件即可

1. **编写beans.xml配置文件**

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <!--bean就是java对象，由Spring创建和管理-->
       <bean id="MySQLImpl" class="xyz.rtx3090.dao.UserDaoMySQLImpl"/>
       <bean id="OracleImpl" class="xyz.rtx3090.dao.UserDaoOracleImpl"/>
       <bean id="ServiceImpl" class="xyz.rtx3090.service.UserServiceImpl">
           <!--注意: 这里的name并不是属性 , 而是set方法后面的那部分 , 首字母小写-->
           <!--引用另外一个bean , 不是用value 而是用 ref。值可以为MySQLImpl，也可以为OracleImpl-->
           <property name="userDao" ref="MySQLImpl"/>
       </bean>
   </beans>
   ~~~

2. **在测试类中进行测试**

   ~~~java
       @Test
       public void testUserServiceImpl() {
           ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
           UserServiceImpl serviceImpl = (UserServiceImpl) context.getBean("ServiceImpl");
           serviceImpl.getUser();//MySQL获取用户数据
       }
   ~~~

   > 我们发现，我们只需要在beans.xml配置文件中更改我们配置的内容，就能改变UserServiceImpl类调用的方法。实现了与上述程序相同的功能。
   >
   > OK , 到了现在 , 我们彻底不用再程序中去改动了 , 要实现不同的操作 , 只需要在xml配置文件中进行修改 , 所谓的IoC,一句话搞定 : 对象由Spring 来创建 , 管理 , 装配 ! 

# HelloSpring

前面我们了解了IoC的基本思想，接下来我们直接来演示一个关于Spring的应用，也就是我们的第一个Spring程序。

## 创建第一个spring程序

1. **在使用spring框架时，我们需要导入spring相关的Jar包**

   ~~~xml
   <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.1.10.RELEASE</version>
   </dependency>
   ~~~

2. **编写一个名为Hello的JavaBen实体类**

   ~~~java
   package xyz.rtx3090.pojo;
   
   public class Hello {
       private String name;
   
       //setter and getter
       public String getName() {
           return name;
       }
   
       public void setName(String name) {
           this.name = name;
       }
   
       public void show() {
           System.out.println("Hello," + name);
       }
   }
   ~~~

3. **编写我们的spring文件 , 这里我们命名为beans.xml**

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <!--bean就是java对象，由Spring创建和管理-->
       <bean id="hello" class="xyz.rtx3090.pojo.Hello">
         	<!--这里name的属性值对应的是set方法名，而不是对象中的属性名-->
           <property name="name" value="Spring"/>
       </bean>
   </beans>
   ~~~

4. **在测试类中测试使用spring创建对象、调用对象等**

   ~~~java
   import org.junit.jupiter.api.Test;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   import xyz.rtx3090.pojo.Hello;
   
   public class MyTest {
       @Test
       public void test01() {
           //解析beans.xml文件 , 生成管理相应的Bean对象
           ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
           //getBean: 参数即为spring配置文件中bean的id
           Hello hello = (Hello) context.getBean("hello");
           hello.show();
       }
   }
   ~~~

## 思考

1. **Hello 对象是谁创建的 ?** 

   答：hello 对象是由Spring创建的

2. **Hello 对象的属性是怎么设置的 ?**

   答：hello 对象的属性是由Spring容器设置的，这个过程就叫控制反转

   > - 控制 : 谁来控制对象的创建 , 传统应用程序的对象是由程序本身控制创建的 , 使用Spring后 , 对象是由Spring来创建的
   > - 反转 : 程序本身不创建对象 , 而变成被动的接收对象 .

3. **什么是依赖注入？**

   就是利用set方法来进行注入.

# IoC创建对象方法

## 通过无参构造方法来创建

spring默认就是采用无参构造方法来创建对象的，下面我们演示一下spring采用spring无参构造函数创建对象

1. **编写名为User的JavaBen实体类**

   ~~~java
   package xyz.rtx3090.pojo;
   
   public class User {
       private String name;
   
       public User() {
           System.out.println("User无参构造");
       }
   
       //setter and getter
       public String getName() {
           return name;
       }
   
       public void setName(String name) {
           this.name = name;
       }
   
       public void show() {
           System.out.println("name=" + name);
       }
   }
   ~~~

2. **编写beans.xml配置文件**

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <bean id="user" class="xyz.rtx3090.pojo.User">
           <property name="name" value="Jason"/>
       </bean>
   </beans>
   ~~~

3. **在测试类中进行测试**

   ~~~java
   import org.junit.jupiter.api.Test;
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   import xyz.rtx3090.pojo.User;
   
   public class MyTest {
       @Test
       public void test01() {
           ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");//User无参构造
           //在执行getBean的时候, user已经创建好了 , 通过无参构造
           User user = (User) context.getBean("user");
           //调用对象的方法
           user.show();//name=Jason
       }
   }
   ~~~

> 结果可以发现，在调用show方法之前，User对象已经通过无参构造初始化了！

## 通过有参构造方法来创建

由于spring默认使用无参构造函数创建对象，所以我们想要使用有参函数创建对象，需要在beans.xml配置文件中做些特殊的配置

1. **还是和上面一样，先编写一个名为User的JavaBen实体类**

   ~~~java
   package xyz.rtx3090.pojo;
   
   public class User {
       private String name;
   
       //有参构造函数
       public User(String name) {
           this.name = name;
           System.out.println("有参构造函数");
       }
   
       //setter and getter
       public String getName() {
           return name;
       }
   
       public void setName(String name) {
           this.name = name;
       }
   
       public void show() {
           System.out.println("name = " + name);
       }
   }
   ~~~

2. **实现使用有参构造函数创建对象，beans.xml配置文件我们可以有三种编写方法**

   ~~~xml
       <!--方式一：根据参数名字设置【推荐】-->
       <bean id="user01" class="xyz.rtx3090.pojo.User">
           <!--name为参数名，value为传入的参数值-->
           <constructor-arg name="name" value="bernardo"/>
       </bean>
   ~~~

   ~~~xml
       <!--方式二：根据参数下标设置-->
       <bean id="user02" class="xyz.rtx3090.pojo.User">
           <!--index为参数下标，value为传入的参数值-->
           <constructor-arg index="0" value="bernardo"/>
       </bean>
   ~~~

   ~~~xml
       <!--方式三：根据参数类型设置-->
       <bean id="user03" class="xyz.rtx3090.pojo.User">
           <constructor-arg type="java.lang.String" value="bernardo03"/>
       </bean>
   ~~~

3. **在测试类中进行测试**

   ~~~java
   import ...
     
   public class MyTest {
       @Test
       public void test01() {
           ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");//有参构造函数
           User user = (User) context.getBean("user01");
           user.show();//name = bernardo
       }
   }
   ~~~

# spring配置

## alias标签配置

alias 设置别名 , 为bean设置别名 , 可以设置多个别名

~~~xml
		<!--设置别名：在获取Bean的时候可以使用别名获取-->
    <alias name="user" alias="newUser"/>
~~~

## Bean标签配置

~~~xml
    <!--
       id 是bean的标识符,要唯一,如果没有配置id,name就是默认标识符
       如果配置id,又配置了name,那么name是别名
       name可以设置多个别名,可以用逗号,分号,空格隔开
       如果不配置id和name,可以根据applicationContext.getBean(.class)获取对象;
			 class是bean的全限定名=包名+类名
    -->
    <bean id="user" class="xyz.rtx3090.pojo.User" name="oldUser">
        <property name="name" value="bernardo"/>
    </bean>
~~~

## import

团队的合作通过import来实现 .

~~~xml
		<import resource="beans01.xml"/>
    <import resource="beans02.xml"/>
    <import resource="beans03.xml"/>
~~~

# 依赖注入

## 概述

- 依赖注入（Dependency Injection,DI）。
- 依赖 : 指Bean对象的创建依赖于容器 . Bean对象的依赖资源 .
- 注入 : 指Bean对象所依赖的资源 , 由容器来设置和装配 .

## 构造器注入

我们在之前的案例已经讲过了。

## Set方法注入【重点】

要求被注入的属性 , 必须有set方法 , set方法的方法名由set + 属性首字母大写 , 如果属性是boolean类型 , 没有set方法 , 是 is .

### 准备JavaBen实体类

~~~java
package xyz.rtx3090.pojo;
import ...

public class Student {
    private String name;
    private Address address;
    private String[] books;
    private List<String> hobbies;
    private Map<String,String> card;
    private Set<String> games;
    private String wife;
    private Properties info;

    //setter and getter

    public void show() {
        System.out.println("name=" + name
            + ",address=" + address.getAddress()
                + ",books=");
        for (String book :
                books) {
            System.out.println("<<" + book + ">>\t");
        }
        System.out.println("\n爱好:" + hobbies);
        System.out.println("card:" + card);
        System.out.println("games:" + games);
        System.out.println("wife:" + wife);
        System.out.println("info:" + info);
    }
}
~~~

~~~java
package xyz.rtx3090.pojo;

public class Address {
    private String address;

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }
}
~~~

### 编写applicationContext.xml配置文件

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="student" class="xyz.rtx3090.pojo.Student">
        <!--1.常量注入-->
        <property name="name" value="Bernardo"/>
        <!--2.bean注入-->
        <property name="address" ref="address"/>
        <!--3.array注入-->
        <property name="books">
            <array>
                <value>西游记</value>
                <value>红楼梦</value>
                <value>水浒传</value>
            </array>
        </property>
        <!--4.list注入-->
        <property name="hobbies">
            <list>
                <value>game</value>
                <value>music</value>
                <value>run</value>
            </list>
        </property>
        <!--5.map注入-->
        <property name="card">
            <map>
                <entry key="农行" value="13247912471740"/>
                <entry key="建行" value="34782399237423"/>
                <entry key="工行" value="56140239847534"/>
            </map>
        </property>
        <!--6.set注入-->
        <property name="games">
            <set>
                <value>OW</value>
                <value>APEX</value>
                <value>LOL</value>
                <value>CSGO</value>
            </set>
        </property>
        <!--7.Null注入-->
        <property name="wife">
            <null></null>
        </property>
        <!--8.Properties注入-->
        <property name="info">
            <props>
                <prop key="name">bernardo</prop>
                <prop key="gender">man</prop>
                <prop key="age">22</prop>
            </props>
        </property>
    </bean>

    <bean id="address"  class="xyz.rtx3090.pojo.Address">
        <property name="address" value="Netherlands"/>
    </bean>

</beans>
~~~

### 在测试类中进行测试

~~~java
import ...

public class MyTest {
    @Test
    public void test01() {
        ApplicationContext context = new ClassPathXmlApplicationContext("ApplicationContext.xml");
        Student student = (Student) context.getBean("student");
        student.show();
    }
}
~~~

> **测试结果：**
>
> ~~~
> name=Bernardo,address=Netherlands,books=
> <<西游记>>	
> <<红楼梦>>	
> <<水浒传>>	
> 
> 爱好:[game, music, run]
> card:{农行=13247912471740, 建行=34782399237423, 工行=56140239847534}
> games:[OW, APEX, LOL, CSGO]
> wife:null
> info:{age=22, name=bernardo, gender=man}
> ~~~

## p命名和c命名注入

### 准备JavaBen实体类

~~~java
package xyz.rtx3090.pojo;

public class User {
    private int id;
    private String name;

    //setter and getter
  	//toString
}
~~~

### 编写applicationContext.xml配置文件

1. **P命名空间注入**【需要在头文件中加入约束文件】

   ```xml
   xmlns:p="http://www.springframework.org/schema/p"
   
       <!--P(属性: properties)命名空间 , 属性依然要设置set方法-->
       <bean id="user" class="xyz.rtx3090.pojo.User" p:id="01" p:name="Jason"/>
   ```

2. **C命名空间注入**【需要在头文件中加入约束文件】

   ~~~xml
   xmlns:c="http://www.springframework.org/schema/c"
   
       <!--C(构造：Constructor)命名空间，属性依然要设置set方法-->
       <bean id="user" class="xyz.rtx3090.pojo.User" c:id="02" c:name="Bernardo"/>
   ~~~

   > **发现问题：**爆红了，刚才我们没有写有参构造！
   >
   > **解决：**把有参构造器加上，这里也能知道，c 就是所谓的构造器注入！

### 在测试类中进行测试

```java
    @Test
    public void test02() {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        User user = context.getBean("user", User.class);
        System.out.println(user);
    }
```

# Bean的作用域

在Spring中，那些组成应用程序的主体及由Spring IoC容器所管理的对象，被称之为bean。简单地讲，bean就是由IoC容器初始化、装配及管理的对象 .

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210526130846.png)

几种作用域中，除了singleton和prototype的其他作用域仅在基于web的应用中使用（不必关心你所采用的是什么web应用框架），只能用在基于web的Spring ApplicationContext环境。所以我们目前只需要讲解一下singleton和prototype作用域即可。

## Singleton

当一个bean的作用域为Singleton，那么Spring IoC容器中只会存在一个共享的bean实例，并且所有对bean的请求，只要id与该bean定义相匹配，则只会返回bean的同一实例。Singleton是单例类型，就是在创建起容器时就同时自动创建了一个bean的对象，不管你是否使用，他都存在了，每次获取到的对象都是同一个对象。注意，Singleton作用域是Spring中的缺省作用域。要在XML中将bean定义成singleton，可以这样配置：

~~~xml
    <bean id="user" class="xyz.rtx3090.pojo.User" scope="singleton">
        <property name="id" value="10"/>
        <property name="name" value="Jason"/>
    </bean>
~~~

测试：

~~~java
import ...

public class MyTest {
    @Test
    public void test01() {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        User user = context.getBean("user", User.class);
        User user1 = context.getBean("user", User.class);
        System.out.println(user == user1);//true
    }
}
~~~

## Prototype

当一个bean的作用域为Prototype，表示一个bean定义对应多个对象实例。Prototype作用域的bean会导致在每次对该bean请求（将其注入到另一个bean中，或者以程序的方式调用容器的getBean()方法）时都会创建一个新的bean实例。Prototype是原型类型，它在我们创建容器的时候并没有实例化，而是当我们获取bean的时候才会去创建一个对象，而且我们每次获取到的对象都不是同一个对象。根据经验，对有状态的bean应该使用prototype作用域，而对无状态的bean则应该使用singleton作用域。在XML中将bean定义成prototype，可以这样配置：

~~~xml
    <bean id="user" class="xyz.rtx3090.pojo.User" scope="prototype">
        <property name="id" value="10"/>
        <property name="name" value="Jason"/>
    </bean>
~~~

测试：

~~~java
import ...

public class MyTest {
    @Test
    public void test01() {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        User user = context.getBean("user", User.class);
        User user1 = context.getBean("user", User.class);
        System.out.println(user == user1);//false
    }
}
~~~

## Request

当一个bean的作用域为Request，表示在一次HTTP请求中，一个bean定义对应一个实例；即每个HTTP请求都会有各自的bean实例，它们依据某个bean定义创建而成。该作用域仅在基于web的Spring ApplicationContext情形下有效。考虑下面bean定义：

~~~xml
 <bean id="loginAction" class=cn.csdn.LoginAction" scope="request"/>
~~~

针对每次HTTP请求，Spring容器会根据loginAction bean的定义创建一个全新的LoginAction bean实例，且该loginAction bean实例仅在当前HTTP request内有效，因此可以根据需要放心的更改所建实例的内部状态，而其他请求中根据loginAction bean定义创建的实例，将不会看到这些特定于某个请求的状态变化。当处理请求结束，request作用域的bean实例将被销毁。

## Session

当一个bean的作用域为Session，表示在一个HTTP Session中，一个bean定义对应一个实例。该作用域仅在基于web的Spring ApplicationContext情形下有效。考虑下面bean定义：

~~~xml
<bean id="userPreferences" class="com.foo.UserPreferences" scope="session"/>
~~~

针对某个HTTP Session，Spring容器会根据userPreferences bean定义创建一个全新的userPreferences bean实例，且该userPreferences bean仅在当前HTTP Session内有效。与request作用域一样，可以根据需要放心的更改所创建实例的内部状态，而别的HTTP Session中根据userPreferences创建的实例，将不会看到这些特定于某个HTTP Session的状态变化。当HTTP Session最终被废弃的时候，在该HTTP Session作用域内的bean也会被废弃掉。

# 自动装配

## 概述

- 自动装配是使用spring满足bean依赖的一种方法
- spring会在应用上下文中为某个bean寻找其依赖的bean。

Spring中bean有三种装配机制，分别是：

1. 在xml中显式配置；
2. 在java中显式配置；
3. 隐式的bean发现机制和自动装配。

这里我们主要讲第三种：自动化的装配bean。

Spring的自动装配需要从两个角度来实现，或者说是两个操作：

1. 组件扫描(component scanning)：spring会自动发现应用上下文中所创建的bean；
2. 自动装配(autowiring)：spring自动满足bean之间的依赖，也就是我们说的IoC/DI；

组件扫描和自动装配组合发挥巨大威力，使得显示的配置降低到最少。

**推荐不使用自动装配xml配置 , 而使用注解 .**

## 测试环境搭建

1. **新建如图所示结果的maven项目**

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210526151756.png)

2. 