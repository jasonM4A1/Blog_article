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

## Singleton

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

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210526152442.png)

2. **编写`xyz.rtx3090.pojo`包下的三个实体类**

   ~~~java
   package xyz.rtx3090.pojo;
   
   public class Cat {
       public void shout() {
           System.out.println("喵喵～");
       }
   }
   ~~~

   ~~~java
   package xyz.rtx3090.pojo;
   
   public class Dog {
       public void shout() {
           System.out.println("汪汪～");
       }
   }
   ~~~

   ~~~java
   package xyz.rtx3090.pojo;
   
   public class Person {
       private Cat cat;
       private Dog dog;
       private String str;
     	//setter and getter
   }
   ~~~

3. **编写applicationContext配置文件**

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <bean id="cat" class="xyz.rtx3090.pojo.Cat"/>
       <bean id="dog" class="xyz.rtx3090.pojo.Dog"/>
   
       <bean id="person" class="xyz.rtx3090.pojo.Person">
           <property name="cat" ref="cat"/>
           <property name="dog" ref="dog"/>
           <property name="str" value="Jason"/>
       </bean>
   
   </beans>
   ~~~

4. **在测试类中进行测试**

   ~~~java
   import org.junit.jupiter.api.Test;
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   import xyz.rtx3090.pojo.Person;
   
   public class MyTest {
       @Test
       public void test01() {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           Person person = context.getBean("person", Person.class);
           person.getCat().shout();//喵喵～
           person.getDog().shout();//汪汪～
       }
   }
   ~~~

   > 结果正常输出，环境OK

## ByName自动装配

### 概述

**autowire byName (按名称自动装配)**

由于在手动配置xml过程中，常常发生字母缺漏和大小写等错误，而无法对其进行检查，使得开发效率降低。

采用自动装配将避免这些错误，并且使配置简单化。

### 测试

1. **修改bean配置，去除我们手动配置的两个property，增加一个属性  autowire="byName"**

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <bean id="cat" class="xyz.rtx3090.pojo.Cat"/>
       <bean id="dog" class="xyz.rtx3090.pojo.Dog"/>
   
       <bean id="person" class="xyz.rtx3090.pojo.Person" autowire="byName">
           <property name="str" value="Jason"/>
       </bean>
   
   </beans>
   ~~~

2. **再次测试，结果依旧成功输出！**

3. **我们将 cat 的bean id修改为 catXXX**

4. **再次测试， 执行时报空指针java.lang.NullPointerException。因为按byName规则找不对应set方法，真正的setCat就没执行，对象就没有初始化，所以调用时就会报空指针错误。**

### 总结

当一个bean节点带有 autowire byName的属性时。

1. 将查找其类中所有的set方法名，例如setCat，获得将set去掉并且首字母小写的字符串，即cat。
2. 去spring容器中寻找是否有此字符串名称id的对象。
3. 如果有，就取出注入；如果没有，就报空指针异常。

## ByType自动装配

### 概述

**autowire byType (按类型自动装配)**

使用autowire byType首先需要保证：同一类型的对象，在spring容器中唯一。如果不唯一，会报不唯一的异常`NoUniqueBeanDefinitionException`。

### 测试

1. **修改bean配置，改变属性为  autowire="byType"**

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <bean id="cat" class="xyz.rtx3090.pojo.Cat"/>
       <bean id="dog" class="xyz.rtx3090.pojo.Dog"/>
   
       <bean id="person" class="xyz.rtx3090.pojo.Person" autowire="byType">
           <property name="str" value="Jason"/>
       </bean>
   
   </beans>
   ~~~

2. **再次测试，结果依旧成功输出！**

3. **在注册一个cat 的bean对象！**

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <bean id="cat" class="xyz.rtx3090.pojo.Cat"/>
       <bean id="dog" class="xyz.rtx3090.pojo.Dog"/>
       <bean id="goodDog" class="xyz.rtx3090.pojo.Dog"/>
   
       <bean id="person" class="xyz.rtx3090.pojo.Person" autowire="byType">
           <property name="str" value="Jason"/>
       </bean>
   
   </beans>
   ~~~

4. **Idea直接在配置文件中提示报错**

5. **删掉cat2，将cat的bean名称改掉(任意名称）！**

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <bean id="YellowCat" class="xyz.rtx3090.pojo.Cat"/>
       <bean id="GoodGog" class="xyz.rtx3090.pojo.Dog"/>
   
       <bean id="person" class="xyz.rtx3090.pojo.Person" autowire="byType">
           <property name="str" value="Jason"/>
       </bean>
   
   </beans>
   ~~~

6. **测试！因为是按类型装配，所以并不会报异常，也不影响最后的结果。甚至将id属性去掉，也不影响结果。**

## 使用注解

dk1.5开始支持注解，spring2.5开始全面支持注解。

### 环境准备

**在applicationContext配置文件中引入约束，并开启属性注解支持**

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

</beans>
~~~

### @Autowired注解

@Autowired是按先类型自动转配的，然后按名字自动转配（但不能指定其他名字）

#### 测试

1. 将JavaBen实体类Person中的set方法去掉，使用@Autowired注解

   ~~~java
   package xyz.rtx3090.pojo;
   
   import org.springframework.beans.factory.annotation.Autowired;
   
   public class Person {
       @Autowired
       private Cat cat;
       @Autowired
       private Dog dog;
       private String name;
   
       //getter
       public Cat getCat() {
           return cat;
       }
   
       public Dog getDog() {
           return dog;
       }
   
       public String getName() {
           return name;
       }
   
     	//setter
       public void setName(String name) {
           this.name = name;
       }
   }
   ~~~

2. 编写applicationContext.xml配置文件

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context
           https://www.springframework.org/schema/context/spring-context.xsd">
   
       <context:annotation-config/>
   
       <bean id="cat" class="xyz.rtx3090.pojo.Cat"/>
       <bean id="dog" class="xyz.rtx3090.pojo.Dog"/>
   
       <bean id="person" class="xyz.rtx3090.pojo.Person">
           <property name="name" value="Jason"/>
       </bean>
   </beans>
   ~~~

3. 在测试类中进行测试

   ~~~java
   import ...
   
   public class MyTest {
       @Test
       public void test01() {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           Person person = context.getBean("person", Person.class);
           person.getCat().shout();//喵喵喵～
           person.getDog().shout();//汪汪汪～
           System.out.println(person.getName());//Jason
       }
   }
   ~~~

   > **补充：**
   >
   > @Autowired(required=false)  说明：false，对象可以为null；true，对象必须存对象，不能为null。
   >
   > ~~~java
   > //如果允许对象为null，设置required = false,默认为true
   > @Autowired(required = false)
   > private Cat cat;
   > ~~~

### @Qualifier

- @Autowired不可以指定其他名字，但加上@Qualifier就可以指定其他名字了
- @Qualifier不能单独使用。

#### 测试

1. 修改配置文件内容，保证类型存在对象，且名字不为类的默认名字！

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context
           https://www.springframework.org/schema/context/spring-context.xsd">
   
       <context:annotation-config/>
   
       <bean id="cat01" class="xyz.rtx3090.pojo.Cat"/>
       <bean id="cat02" class="xyz.rtx3090.pojo.Cat"/>
       <bean id="dog01" class="xyz.rtx3090.pojo.Dog"/>
       <bean id="dog02" class="xyz.rtx3090.pojo.Dog"/>
   
       <bean id="person" class="xyz.rtx3090.pojo.Person">
           <property name="name" value="Jason"/>
       </bean>
   </beans>
   ~~~

2. 没有加Qualifier测试，直接报错

3. 在属性上添加Qualifier注解

   ~~~java
   package xyz.rtx3090.pojo;
   import ...
   
   public class Person {
       @Autowired
       @Qualifier(value = "cat01")
       private Cat cat;
       @Autowired
       @Qualifier(value = "dog01")
       private Dog dog;
       private String name;
   
       //setter and getter
   }
   ~~~

4. 再次测试，结果输出成功！

### @Resource

- @Resource如有指定的name属性，先按该属性进行byName方式查找装配；
- 其次再进行默认的byName方式进行装配；
- 如果以上都不成功，则按byType的方式自动装配。
- 都不成功，则报异常。

#### 测试

1. **修改Java实体类上的注解**

   ~~~java
   package xyz.rtx3090.pojo;
   import ...
   
   public class Person {
       //如果允许对象为null，设置required = false,默认为true
       @Resource(name="cat02")
       private Cat cat;
       @Resource
       private Dog dog;
       private String name;
   
       //setter and getter
   }
   ~~~
   
2. **编写applicationContext.xml配置文件**

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context
           https://www.springframework.org/schema/context/spring-context.xsd">
   
       <context:annotation-config/>
   
       <bean id="cat01" class="xyz.rtx3090.pojo.Cat"/>
       <bean id="cat02" class="xyz.rtx3090.pojo.Cat"/>
       <bean id="dog" class="xyz.rtx3090.pojo.Dog"/>
       <bean id="dog02" class="xyz.rtx3090.pojo.Dog"/>
   
       <bean id="person" class="xyz.rtx3090.pojo.Person">
           <property name="name" value="Jason"/>
       </bean>
   </beans>
   ~~~

3. **在次进行测试，结果输入成功**

   ~~~java
   import org.junit.jupiter.api.Test;
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   import xyz.rtx3090.pojo.Person;
   
   public class MyTest {
       @Test
       public void test01() {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           Person person = context.getBean("person", Person.class);
           person.getCat().shout();//喵喵喵～
           person.getDog().shout();//汪汪汪～
           System.out.println(person.getName());//Jason
       }
   }
   ~~~

4. **再次修改实体类中的注解**

   ~~~java
   package xyz.rtx3090.pojo;
   import javax.annotation.Resource;
   
   public class Person {
       @Resource
       private Cat cat;
       @Resource
       private Dog dog;
       private String name;
   
       //setter and getter
   }
   ~~~

5. **相应的修改applicationContext.xml配置文件**

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context
           https://www.springframework.org/schema/context/spring-context.xsd">
   
       <context:annotation-config/>
   
       <bean id="cat01" class="xyz.rtx3090.pojo.Cat"/>
       <bean id="dog" class="xyz.rtx3090.pojo.Dog"/>
   
       <bean id="person" class="xyz.rtx3090.pojo.Person">
           <property name="name" value="Jason"/>
       </bean>
   </beans>
   ~~~

6. **再次进行测试，结果输出成功**

   ~~~java
   import ...
   
   public class MyTest {
       @Test
       public void test01() {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           Person person = context.getBean("person", Person.class);
           person.getCat().shout();//喵喵喵～
           person.getDog().shout();//汪汪汪～
           System.out.println(person.getName());//Jason
       }
   }
   ~~~

   > **结论：**
   >
   > 先进行byName查找，失败；再进行byType查找，成功。

### 小结

**@Autowire与@Resource的异同**

1. @Autowired与@Resource都可以用来装配bean。都可以写在字段上，或写在setter方法上。
2. @Autowired默认按类型装配（属于spring规范），其次按照名字装配，但无法指定名字。默认情况下必须要求依赖对象必须存在，如果要允许null 值，可以设置它的required属性为false，如：@Autowired(required=false) ，如果我们想使用名称装配可以结合@Qualifier注解进行使用
3. @Resource（属于J2EE复返），默认按照名称进行装配，名称可以通过name属性进行指定。如果没有指定name属性，当注解写在字段上时，默认取字段名进行按照名称查找，如果注解写在setter方法上默认取属性名进行装配。当找不到与名称匹配的bean时才按照类型进行装配。但是需要注意的是，如果name属性一旦指定，就只会按照名称进行装配。
4. 它们的作用相同都是用注解方式注入对象，但执行顺序不同。@Autowired先byType，@Resource先byName。

# 使用注解开发

在spring4之后，想要使用注解形式，必须得要引入aop的包

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210526195632.png)

在配置文件当中，还得要引入一个context约束

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:context="http://www.springframework.org/schema/context"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd">

</beans>
~~~

## Bean的实现

我们之前都是使用 bean 的标签进行bean注入，但是实际开发中，我们一般都会使用注解！

1. 编写applicationContext.xml配置文件，增加下述配置用来扫描指定包的注解

   ~~~xml
   <!--指定注解扫描包-->
   <context:component-scan base-package="com.kuang.pojo"/>
   ~~~

2. 编写JavaBen实体类，并为其增加上注解

   ~~~java
   package xyz.rtx3090.pojo;
   import org.springframework.stereotype.Component;
   
   // 相当于配置文件中 <bean id="user" class="当前注解的类"/>
   @Component("user")
   public class User {
       private int id = 10;
       private String name = "王多余";
   
       //setter and getter
   		//toString
   }
   ~~~

3. 在测试类中进行测试

   ~~~java
   import ...
   
   public class MyTest {
       @Test
       public void test01() {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           User user = context.getBean("user", User.class);
           System.out.println(user);//User{id=10, name='王多余'}
       }
   }
   ~~~

   > 测试成功，结果输出正确

## 属性注入

使用注解注入属性

1. 可以不用提供set方法，直接在直接名上添加@value("值")

   ~~~java
   package xyz.rtx3090.pojo;
   import ...
   
   // 相当于配置文件中 <bean id="user" class="当前注解的类"/>
   @Component("user")
   public class User {
     	// 相当于配置文件中 <property name="id" value="10"/>
       @Value("10")
       private int id;
      	// 相当于配置文件中 <property name="name" value="Jason"/>
       @Value("Jason")
       private String name;
   
       //toString
   }
   ~~~

2. 如果提供了set方法，在set方法上添加@value("值");

   ~~~java
   package xyz.rtx3090.pojo;
   import ...
   
   // 相当于配置文件中 <bean id="user" class="当前注解的类"/>
   @Component("user")
   public class User {
       private int id;
       private String name;
   
       //setter and getter
       public int getId() {
           return id;
       }
     	// 相当于配置文件中 <property name="id" value="10"/>
       @Value("20")
       public void setId(int id) {
           this.id = id;
       }
   
       public String getName() {
           return name;
       }
     	// 相当于配置文件中 <property name="name" value="Jason"/>
       @Value("spring")
       public void setName(String name) {
           this.name = name;
       }
   
       //toString
   }
   ~~~

## 衍生注解

我们这些注解，就是替代了在配置文件当中配置步骤而已！更加的方便快捷！

**@Component三个衍生注解**

为了更好的进行分层，Spring可以使用其它三个注解，功能一样，目前使用哪一个功能都一样。

- @Controller：web层
- @Service：service层
- @Repository：dao层

写上这些注解，就相当于将这个类交给Spring管理装配了！

## 自动装配注解

在Bean的自动装配已经讲过了，可以回顾！

## 作用域

@scope

- singleton：默认的，Spring会采用单例模式创建这个对象。关闭工厂 ，所有的对象都会销毁。
- prototype：多例模式。关闭工厂 ，所有的对象不会销毁。内部的垃圾回收机制会回收

~~~java
package xyz.rtx3090.pojo;
import ...

// 相当于配置文件中 <bean id="user" class="当前注解的类"/>
@Component("user")
@Scope("singleton")
public class User {
    @Value("10")
    private int id;
    @Value("Jason")
    private String name;

		//toString
}
~~~

## 小结

### **XML与注解比较**

- XML可以适用任何场景 ，结构清晰，维护方便
- 注解不是自己提供的类使用不了，开发简单方便

### **xml与注解整合开发** ：推荐最佳实践

- xml管理Bean
- 注解完成属性注入
- 使用过程中， 可以不用扫描，扫描是为了类上的注解

### 下面标签及属性作用

~~~xml
<context:annotation-config/>  
~~~

- 进行注解驱动注册，从而使注解生效

- 用于激活那些已经在spring容器里注册过的bean上面的注解，也就是显示的向Spring注册

- 如果不扫描包，就需要手动配置bean

- 如果不加注解驱动，则注入的值为null！


## 基于Java类进行配置

### 概述

JavaConfig 原来是 Spring 的一个子项目，它通过 Java 类的方式提供 Bean 的定义信息，在 Spring4 的版本， JavaConfig 已正式成为 Spring4 的核心功能 。

### 测试

1. 编写JavaBen实体类Dog

   ~~~java
   package xyz.rtx3090.pojo;
   import ...
     
   @Component//将这个类标注为Spring的一个组件，放到容器中
   public class Dog {
     	//配置属性值
       @Value("王多余")
       public String name;
   
       //setter and getter
       public String getName() {
           return name;
       }
   
       public void setName(String name) {
           this.name = name;
       }
   }
   ~~~

2. 新建一个config配置包，编写一个MyConfig配置类

   ~~~java
   package xyz.rtx3090.config;
   import ...
   
   @Configuration//代表这是一个配置类
   public class MyConfig {
   
       @Bean//通过方法注册一个bean，这里的返回值就Bean的类型，方法名就是bean的id
       public Dog getDog() {
           return new Dog();
       }
   }
   ~~~

3. 在测试类中进行测试

   ~~~java
   import ...
   
   public class MyTest {
       @Test
       public void test01() {
           ApplicationContext context = new AnnotationConfigApplicationContext(MyConfig.class);
           Dog getDog = context.getBean("getDog", Dog.class);
           String name = getDog.getName();
           System.out.println(name);//王多余
       }
   }
   ~~~

   > 测试成功，输出结果正确

4. 我们再编写一个配置类

   ~~~java
   package xyz.rtx3090.config;
   
   import org.springframework.context.annotation.Configuration;
   
   @Configuration//代表这是一个配置类
   public class MyConfig2 {
   }
   ~~~

5. 在之前的配置类中我们来选择导入这个配置类

   ~~~java
   package xyz.rtx3090.config;
   import ...
   
   @Configuration//代表这是一个配置类
   @Import(MyConfig2.class) //导入合并其他配置类，类似于配置文件中的 inculde 标签
   public class MyConfig {
   
       @Bean//通过方法注册一个bean，这里的返回值就Bean的类型，方法名就是bean的id
       public Dog getDog() {
           return new Dog();
       }
   }
   ~~~

   > 关于这种Java类的配置方式，我们在之后的SpringBoot 和 SpringCloud中还会大量看到，我们需要知道这些注解的作用即可！

# 代理模式

为什么要学习代理模式，因为AOP的底层机制就是动态代理！

**代理模式：**

- 静态代理
- 动态代理

学习aop之前 , 我们要先了解一下代理模式！

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210527102942.png)

## 静态代理

### 静态代理角色分析

- 抽象角色 : 一般使用接口或者抽象类来实现
- 真实角色 : 被代理的角色
- 代理角色 : 代理真实角色 ; 代理真实角色后 , 一般会做一些附属的操作 .
- 客户  :  使用代理角色来进行一些操作 .

### 代码实现

1. Rent接口，即抽象角色

   ~~~java
   package xyz.rtx3090.demo01;
   
   //抽象角色：租房
   public interface Rent {
       //出租房子的抽象动作
       public void rent();
   }
   ~~~

2. Host类，即真实角色

   ~~~java
   package xyz.rtx3090.demo01;
   
   //真实角色: 房东，房东要出租房子
   public class Host implements Rent{
       //房东的具体出租房子动作
       public void rent() {
           System.out.println("房东出租房子");
       }
   }
   ~~~

3. Proxy类，即代理角色

   ~~~java
   package xyz.rtx3090.demo01;
   
   //代理角色：中介
   public class Proxy implements Rent{
       private Host host;
   
       //constructor
       public Proxy() {
       }
   
       public Proxy(Host host) {
           this.host = host;
       }
   
       //中介具体的出租房子动作
       public void rent() {
           seeHouse();
           host.rent();
           fare();
       }
   
       //看房
       public void seeHouse() {
           System.out.println("带客户看房");
       }
   
       //收中介费
       public void fare() {
           System.out.println("收中介费");
       }
   }
   ~~~

4. Client类，即客户角色

   ~~~java
   package xyz.rtx3090.demo01;
   
   //客户类，一般客户都会去找代理
   public class Client {
       public static void main(String[] args) {
           //创建一个房东对象,房东需要出租房子
           Host host = new Host();
           //创建一个中介对象，房东找中介帮助出租房子
           Proxy proxy = new Proxy(host);
           //中介出租房子
           proxy.rent();
       }
   }
   ~~~

### 代码分析

在这个过程中，你直接接触的就是中介，就如同现实生活中的样子，你看不到房东，但是你依旧租到了房东的房子通过代理，这就是所谓的代理模式，程序源自于生活，所以学编程的人，一般能够更加抽象的看待生活中发生的事情。

**静态代理的好处:**

- 可以使得我们的真实角色更加纯粹 . 不再去关注一些公共的事情 .
- 公共的业务由代理来完成 . 实现了业务的分工 ,
- 公共业务发生扩展时变得更加集中和方便 .

静态代理的缺点 :

- 类多了 , 多了代理类 , 工作量变大了 . 开发效率降低 .

我们想要静态代理的好处，又不想要静态代理的缺点，所以 , 就有了动态代理 !

### 静态代理再理解

同学们练习完毕后，我们再来举一个例子，巩固大家的学习！

1. 创建一个抽象角色，比如咋们平时做的用户业务，抽象起来就是增删改查！

   ~~~java
   package xyz.rtx3090.demo02;
   
   //抽象角色：增删改查业务
   public interface UserService {
       void add();
       void delete();
       void update();
       void query();
   }
   ~~~

2. 我们需要一个真实对象来完成这些增删改查操作

   ~~~java
   package xyz.rtx3090.demo02;
   
   //真实对象，完成增删改查操作的人
   public class UserServiceImpl implements UserService{
   
       public void add() {
           System.out.println("增加了一个用户");
       }
   
       public void delete() {
           System.out.println("删除了一个用户");
       }
   
       public void update() {
           System.out.println("更新了一个用户");
       }
   
       public void query() {
           System.out.println("查询了一个用户");
       }
   }
   ~~~

3. 需求来了，现在我们需要增加一个日志功能，怎么实现！

   - 思路1 ：在实现类上增加代码 【麻烦！】
   - 思路2：使用代理来做，能够不改变原来的业务情况下，实现此功能就是最好的了！

4. 设置一个代理类来处理日志！代理角色

   ~~~java
   package xyz.rtx3090.demo02;
   
   //代理角色，在这里面增加日志的实现
   public class UserServiceImplProxy implements UserService{
       private UserServiceImpl userService;
   
       public UserServiceImplProxy(UserServiceImpl userService) {
           this.userService = userService;
       }
   
   
       public void add() {
           printLog("add");
           userService.add();
       }
   
       public void delete() {
           printLog("delete");
           userService.delete();
       }
   
       public void update() {
           printLog("update");
           userService.update();
       }
   
       public void query() {
           printLog("query");
           userService.query();
       }
   
       //打印日志
       public void printLog(String msg) {
           System.out.println("调用了[" + msg + "]方法");
       }
   }
   ~~~

5. 在测试类中进行测试

   ~~~java
   import org.junit.jupiter.api.Test;
   import xyz.rtx3090.demo02.UserServiceImpl;
   import xyz.rtx3090.demo02.UserServiceImplProxy;
   
   public class MyTest {
       @Test
       public void test01() {
           UserServiceImplProxy userServiceImplProxy = new UserServiceImplProxy(new UserServiceImpl());
           userServiceImplProxy.add();
           userServiceImplProxy.delete();
           userServiceImplProxy.update();
           userServiceImplProxy.query();
       }
   }
   ~~~

   OK，到了现在代理模式大家应该都没有什么问题了，重点大家需要理解其中的思想；

   **我们在不改变原来的代码的情况下，实现了对原有功能的增强，这是AOP中最核心的思想**   

   聊聊AOP：纵向开发，横向开发

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210527113149.png)

## 动态代理

- 动态代理的角色和静态代理的一样 .

- 动态代理的代理类是动态生成的 . 静态代理的代理类是我们提前写好的

- 动态代理分为两类 : 一类是基于接口动态代理 , 一类是基于类的动态代理

- - 基于接口的动态代理----JDK动态代理
  - 基于类的动态代理--cglib
  - 现在用的比较多的是 javasist 来生成动态代理 . 百度一下javasist
  - 我们这里使用JDK的原生代码来实现，其余的道理都是一样的！

**JDK的动态代理需要了解两个类**

`InvocationHandler`和`Proxy`

### InvocationHandler类

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210530132346.png)

~~~java
Object invoke(Object proxy, 方法 method, Object[] args)；
//参数
//proxy - 调用该方法的代理实例
//method -所述方法对应于调用代理实例上的接口方法的实例。方法对象的声明类将是该方法声明的接口，它可以是代理类继承该方法的代理接口的超级接口。
//args -包含的方法调用传递代理实例的参数值的对象的阵列，或null如果接口方法没有参数。原始类型的参数包含在适当的原始包装器类的实例中，例如java.lang.Integer或java.lang.Boolean 。
~~~

### Proxy类

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210530133209.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210530133218.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210530133306.png)

~~~java
/生成代理类
public Object getProxy(){
   return Proxy.newProxyInstance(this.getClass().getClassLoader(),
                                 rent.getClass().getInterfaces(),this);
}
~~~

### 代码实现

1. 抽象动作角色`Rent`接口

   ~~~java
   package xyz.rtx3090.demo01;
   
   //抽象角色：租房
   public interface Rent {
       void rent();
   }
   ~~~

2. 真实动作角色`Host`类 ，继承`Rent`接口

   ```java
   package xyz.rtx3090.demo01;
   
   //真实角色: 房东，房东要出租房子
   public class Host implements Rent{
       public void rent() {
           System.out.println("房屋出租");
       }
   }
   ```

3. 代理角色`ProxyInvocationHandler`类，继承`InvocationHandler`接口

   ~~~java
       package xyz.rtx3090.demo01;
   
       import java.lang.reflect.InvocationHandler;
       import java.lang.reflect.Method;
       import java.lang.reflect.Proxy;
   
       public class ProxyInvocationHandler implements InvocationHandler {
           private Object target;
   
           public void setTarget(Object target) {
               this.target = target;
           }
   
           //生成代理类，重点是第二个参数，获取要代理的抽象角色！之前都是一个角色，现在可以代理一类角色
           public Object getProxy() {
               return Proxy.newProxyInstance(this.getClass().getClassLoader(), target.getClass().getInterfaces(),this);
           }
   
           // proxy : 代理类 method : 代理类的调用处理程序的方法对象.
           // 处理代理实例上的方法调用并返回结果
           public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
               seeHouse();
               //核心：本质利用反射实现！
               Object invoke = method.invoke(target, args);
               fare();
               return invoke;
           }
   
           //看房
           public void seeHouse() {
               System.out.println("带客户看房");
           }
   
           //收中介费
           public void fare() {
               System.out.println("收中介费");
           }
       }
   
   ~~~

4. 真实动作角色`Host`类

   ~~~java
   package xyz.rtx3090.demo01;
   
   //租客
   public class Client {
       public static void main(String[] args) {
           //真实角色
           Host host = new Host();
           //代理实例的调用处理程序
           ProxyInvocationHandler handler = new ProxyInvocationHandler();
           //将真实角色放置进去！
           handler.setRent(host);
           //动态生成对应的代理类
           Rent proxy = (Rent) handler.getProxy();
           //进行房子出租
           proxy.rent();
       }
   }
   ~~~

   > 核心：**一个动态代理 , 一般代理某一类业务 , 一个动态代理可以代理多个类，代理的是接口！**

### 动态代理的好处

静态代理有的它都有，静态代理没有的，它也有！

- 可以使得我们的真实角色更加纯粹 . 不再去关注一些公共的事情 .
- 公共的业务由代理来完成 . 实现了业务的分工 ,
- 公共业务发生扩展时变得更加集中和方便 .
- 一个动态代理 , 一般代理某一类业务
- 一个动态代理可以代理多个类，代理的是接口！

# AOP

## 概述

AOP（Aspect Oriented Programming）意为：面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210531091437.png)

## 作用

提供声明式事务；允许用户自定义切面

以下名词需要了解下：

- 横切关注点：跨越应用程序多个模块的方法或功能。即是，与我们业务逻辑无关的，但是我们需要关注的部分，就是横切关注点。如日志 , 安全 , 缓存 , 事务等等 ....
- 切面（ASPECT）：横切关注点 被模块化 的特殊对象。即，它是一个类。
- 通知（Advice）：切面必须要完成的工作。即，它是类中的一个方法。
- 目标（Target）：被通知对象。
- 代理（Proxy）：向目标对象应用通知之后创建的对象。
- 切入点（PointCut）：切面通知 执行的 “地点”的定义。
- 连接点（JointPoint）：与切入点匹配的执行点。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210531091550.png)

SpringAOP中，通过Advice定义横切逻辑，Spring中支持5种类型的Advice:

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210531091638.png)

即 Aop 在 不改变原有代码的情况下 , 去增加新的功能 .

## 代码实现（使用Spring）

### 导入依赖Jar包

~~~xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
   <groupId>org.aspectj</groupId>
   <artifactId>aspectjweaver</artifactId>
   <version>1.9.4</version>
</dependency>
~~~

### 方式一：通过 Spring API 实现

1. 编写业务接口

   ~~~java
   package xyz.rtx3090.demo02;
   
   public interface UserService {
       void add();
       void delete();
       void update();
       void search();
   }
   ~~~

2. 编写业务实现类

   ~~~java
   package xyz.rtx3090.demo02;
   
   public class UserServiceImpl implements UserService{
       public void add() {
           System.out.println("增加一个用户");
       }
   
       public void delete() {
           System.out.println("删除一个用户");
       }
   
       public void update() {
           System.out.println("更新一个用户");
       }
   
       public void search() {
           System.out.println("查询一个用户");
       }
   }
   ~~~

3. 编写前置增强类

   ~~~java
   package xyz.rtx3090.demo02;
   import ...
   
   public class BeforeLog implements MethodBeforeAdvice {
       //method : 要执行的目标对象的方法
       //objects : 被调用的方法的参数
       //o : 目标对象
       public void before(Method method, Object[] objects, Object o) throws Throwable {
           System.out.println(o.getClass().getClass().getName() + "的" + method.getName() + "方法被执行了");
       }
   }
   ~~~

4. 编写后置增强类

   ~~~java
   package xyz.rtx3090.demo02;
   import ...
   
   public class AfterLog implements AfterReturningAdvice {
       //o 返回值
       //method 被调用的方法
       //objects 被调用的方法的对象的参数
       //o1 被调用的目标对象
       public void afterReturning(Object o, Method method, Object[] objects, Object o1) throws Throwable {
           System.out.println(o1.getClass().getName() + "的" + method.getName() + "方法被执行了；" + " 返回值为：" + o + "\n");
       }
   }
   ~~~

5. 编写applicationContext.xml配置文件

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">
   
       <!--bean配置-->
       <bean id="userServiceImpl" class="xyz.rtx3090.demo02.UserServiceImpl"/>
       <bean id="before" class="xyz.rtx3090.demo02.Before"/>
       <bean id="after" class="xyz.rtx3090.demo02.After"/>
   
       <!--aop配置-->
       <aop:config>
           <!--切入点 expression: 表达式匹配要执行的方法-->
           <aop:pointcut id="pointcut" expression="execution(* xyz.rtx3090.demo02.UserServiceImpl.*(..))"/>
           <!--执行环绕; advice-ref执行方法 . pointcut-ref切入点-->
           <aop:advisor advice-ref="before" pointcut-ref="pointcut"/>
           <aop:advisor advice-ref="after" pointcut-ref="pointcut"/>
       </aop:config>
   </beans>
   ~~~

6. 在测试类中进行测试

   ~~~java
   import ...
   
   public class MyTest {
       @Test
       public void test01() {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           UserService userServiceImpl = (UserService) context.getBean("userServiceImpl");
           userServiceImpl.add();
           userServiceImpl.delete();
           userServiceImpl.update();
           userServiceImpl.query();
       }
   }
   ~~~

   > 结果为：
   >
   > ~~~
   > xyz.rtx3090.demo02.UserServiceImpl的add方法被执行了
   > 增加了一个用户
   > xyz.rtx3090.demo02.UserServiceImpl的add被执行了; 返回值：xyz.rtx3090.demo02.UserServiceImpl@32ee6fee
   > 
   > xyz.rtx3090.demo02.UserServiceImpl的delete方法被执行了
   > 删除了一个用户
   > xyz.rtx3090.demo02.UserServiceImpl的delete被执行了; 返回值：xyz.rtx3090.demo02.UserServiceImpl@32ee6fee
   > 
   > xyz.rtx3090.demo02.UserServiceImpl的update方法被执行了
   > 更新了一个用户
   > xyz.rtx3090.demo02.UserServiceImpl的update被执行了; 返回值：xyz.rtx3090.demo02.UserServiceImpl@32ee6fee
   > 
   > xyz.rtx3090.demo02.UserServiceImpl的query方法被执行了
   > 查询了一个用户
   > xyz.rtx3090.demo02.UserServiceImpl的query被执行了; 返回值：xyz.rtx3090.demo02.UserServiceImpl@32ee6fee
   > ~~~
   >
   > 
   >
   > Aop的重要性 : 很重要 . 一定要理解其中的思路 , 主要是思想的理解这一块 .
   >
   > Spring的Aop就是将公共的业务 (日志 , 安全等) 和领域业务结合起来 , 当执行领域业务时 , 将会把公共业务加进来 . 实现公共业务的重复利用 . 领域业务更纯粹 , 程序猿专注领域业务 , 其本质还是动态代理 . 

### 方式二：自定义类来实现Aop

1. 编写业务接口

   ```java
   package xyz.rtx3090.service;
   
   public interface UserService {
       void add();
       void delete();
       void update();
       void query();
   }
   ```

2. 编写业务实现类

   ~~~java
   package xyz.rtx3090.service;
   
   public class UserServiceImpl implements UserService
   {
       public void add() {
           System.out.println("增加了一个用户");
       }
   
       public void delete() {
           System.out.println("删除了一个用户");
       }
   
       public void update() {
           System.out.println("更新了一个用户");
       }
   
       public void query() {
           System.out.println("查询了一个用户");
       }
   }
   ~~~

3. 编写自定义类

   ~~~java
   package xyz.rtx3090.service;
   
   public class DiyPointcut {
       public void before() {
           System.out.println("——————————方法执行前——————————");
       }
       public void after() {
           System.out.println("——————————方法执行后——————————\n");
       }
   }
   ~~~

4. 编写applicationContext.xml配置文件

   ```java
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">
   
       <!--bean配置-->
       <bean id="userServiceImpl" class="xyz.rtx3090.service.UserServiceImpl"/>
       <bean id="diyPointcut" class="xyz.rtx3090.service.DiyPointcut"/>
   
       <!--aop配置-->
       <aop:config>
           <aop:aspect ref="diyPointcut">
               <aop:pointcut id="pointcut" expression="execution(* xyz.rtx3090.service.UserServiceImpl.*(..))"/>
               <aop:before method="before" pointcut-ref="pointcut"/>
               <aop:after method="after" pointcut-ref="pointcut"/>
           </aop:aspect>
       </aop:config>
   
   </beans>
   ```

5. 在测试类中进行测试

   ~~~java
   import ...
   
   public class MyTest {
       @Test
       public void test01() {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           UserService userServiceImpl = (UserService) context.getBean("userServiceImpl");
           userServiceImpl.add();
       }
   }
   ~~~

   > 测试结果：
   >
   > ~~~
   > ——————————方法执行前——————————
   > 增加了一个用户
   > ——————————方法执行后——————————
   > 
   > ——————————方法执行前——————————
   > 删除了一个用户
   > ——————————方法执行后——————————
   > 
   > ——————————方法执行前——————————
   > 更新了一个用户
   > ——————————方法执行后——————————
   > 
   > ——————————方法执行前——————————
   > 查询了一个用户
   > ——————————方法执行后——————————
   > ~~~

### 方式三：使用注解实现

1. 编写业务接口

   ```java
   package xyz.rtx3090.service;
   
   public interface UserService {
       void add();
       void delete();
       void update();
       void query();
   }
   ```

2. 编写业务实现类

   ~~~java
   package xyz.rtx3090.service;
   
   public class UserServiceImpl implements UserService
   {
       public void add() {
           System.out.println("增加了一个用户");
       }
   
       public void delete() {
           System.out.println("删除了一个用户");
       }
   
       public void update() {
           System.out.println("更新了一个用户");
       }
   
       public void query() {
           System.out.println("查询了一个用户");
       }
   }
   ~~~

3. 编写注解实现的增强类

   ~~~java
   package xyz.rtx3090.config;
   
   import org.aspectj.lang.ProceedingJoinPoint;
   import org.aspectj.lang.annotation.After;
   import org.aspectj.lang.annotation.Around;
   import org.aspectj.lang.annotation.Aspect;
   import org.aspectj.lang.annotation.Before;
   
   @Aspect
   public class AnnotationPointcut {
       @Before("execution(* xyz.rtx3090.service.UserServiceImpl.*(..))")
       public void before() {
           System.out.println("——————————方法执行前——————————");
       }
   
       @After("execution(* xyz.rtx3090.service.UserServiceImpl.*(..))")
       public void after() {
           System.out.println("——————————方法执行后——————————");
       }
   
       @Around("execution(* xyz.rtx3090.service.UserServiceImpl.*(..))")
       public void around(ProceedingJoinPoint joinPoint) throws  Throwable {
           System.out.println("环绕前");
           System.out.println("签名：" + joinPoint.getSignature());
           //执行目标方法proceed
           Object proceed = joinPoint.proceed();
           System.out.println("环绕后");
           System.out.println(proceed);
       }
   }
   ~~~

4. 编写applicationContext.xml配置文件

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">
   
       <!--bean配置-->
       <bean id="userServiceImpl" class="xyz.rtx3090.service.UserServiceImpl"/>
       <bean id="annotationPointcut" class="xyz.rtx3090.config.AnnotationPointcut"/>
   
       <aop:aspectj-autoproxy/>
   </beans>
   ~~~

   > 通过aop命名空间的<aop:aspectj-autoproxy />声明自动为spring容器中那些配置@aspectJ切面的bean创建代理，织入切面。当然，spring 在内部依旧采用AnnotationAwareAspectJAutoProxyCreator进行自动代理的创建工作，但具体实现的细节已经被<aop:aspectj-autoproxy />隐藏起来了
   >
   > <aop:aspectj-autoproxy />有一个proxy-target-class属性，默认为false，表示使用jdk动态代理织入增强，当配为<aop:aspectj-autoproxy  poxy-target-class="true"/>时，表示使用CGLib动态代理技术织入增强。不过即使proxy-target-class设置为false，如果目标类没有声明接口，则spring将自动使用CGLib动态代理。

5. 在测试类中进行测试

   ~~~java
   import ...
   
   public class MyTest {
       @Test
       public void test01() {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           UserService userServiceImpl = (UserService) context.getBean("userServiceImpl");
           userServiceImpl.add();
       }
   }
   ~~~


# Spring整合Mybatis

## 回顾Mybatis项目创建

1. 导入相关Jar包（spring, mybatis, jdbc, spring-mybatis)

   ~~~xml
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.12</version>
           </dependency>
           <dependency>
               <groupId>org.mybatis</groupId>
               <artifactId>mybatis</artifactId>
               <version>3.5.2</version>
           </dependency>
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
               <version>5.1.47</version>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-webmvc</artifactId>
               <version>5.1.10.RELEASE</version>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-jdbc</artifactId>
               <version>5.1.10.RELEASE</version>
           </dependency>
           <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
           <dependency>
               <groupId>org.aspectj</groupId>
               <artifactId>aspectjweaver</artifactId>
               <version>1.9.4</version>
           </dependency>
           <dependency>
               <groupId>org.mybatis</groupId>
               <artifactId>mybatis-spring</artifactId>
               <version>2.0.2</version>
           </dependency>
           <!-- https://mvnrepository.com/artifact/log4j/log4j -->
           <dependency>
               <groupId>log4j</groupId>
               <artifactId>log4j</artifactId>
               <version>1.2.17</version>
           </dependency>
   ~~~

2. 配置Maven静态资源过滤问题

   ~~~xml
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
           <!--解决Maven静态资源过滤问题-->
           <resources>
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
   ~~~

3. 编写日志实现配置文件`log4j.properties`

   ```properties
   #将等级为DEBUG的日志信息输出到console和file这两个目的地，console和file的定义在下面的代码
   log4j.rootLogger=DEBUG,console,file
   
   #控制台输出的相关设置
   log4j.appender.console = org.apache.log4j.ConsoleAppender
   log4j.appender.console.Target = System.out
   log4j.appender.console.Threshold=DEBUG
   log4j.appender.console.layout = org.apache.log4j.PatternLayout
   log4j.appender.console.layout.ConversionPattern=[%c]-%m%n
   
   #文件输出的相关设置
   log4j.appender.file = org.apache.log4j.RollingFileAppender
   log4j.appender.file.File=./log/bernardo.log
   log4j.appender.file.MaxFileSize=10mb
   log4j.appender.file.Threshold=DEBUG
   log4j.appender.file.layout=org.apache.log4j.PatternLayout
   log4j.appender.file.layout.ConversionPattern=[%p][%d{yy-MM-dd}][%c]%m%n
   
   #日志输出级别
   log4j.logger.org.mybatis=DEBUG
   log4j.logger.java.sql=DEBUG
   log4j.logger.java.sql.Statement=DEBUG
   log4j.logger.java.sql.ResultSet=DEBUG
   log4j.logger.java.sql.PreparedStatement=DEBUG
   ```

4. 编写获取SqlSession对象的工具类

   ~~~java
   package xyz.rtx3090.utils;
   import ...
   
   public class MybatisUtils {
   
       private static SqlSessionFactory sqlSessionFactory;
   
       static {
           String resource = "mybatis-config.xml";
           InputStream inputStream = null;
           try {
               inputStream = Resources.getResourceAsStream(resource);
           } catch (IOException e) {
               e.printStackTrace();
           }
           sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
       }
   
       //获取SqlSession方法
       public static SqlSession getSqlSession() {
           return sqlSessionFactory.openSession();
       }
   }
   ~~~

5. 创建编写实体类

   ~~~java
   package xyz.rtx3090.mapper;
   import xyz.rtx3090.pojo.User;
   import java.util.List;
   
   public interface UserMapper {
       //查询全部用户
       List<User> queryAllUser();
   }
   ~~~

6. 编写Mapper接口

   ~~~java
   package xyz.rtx3090.mapper;
   
   import xyz.rtx3090.pojo.User;
   
   import java.util.List;
   
   public interface UserMapper {
       //查询全部用户
       List<User> queryAllUser();
   }
   
   ~~~

7. 编写mapper接口对应的xml配置文件

   ~~~xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="xyz.rtx3090.mapper.UserMapper">
       <!--查询全部用户-->
       <select id="queryAllUser" resultType="user">
           select * from mybatis.user;
       </select>
   </mapper>
   ~~~

8. 编写mybatis核心配置文件

   ~~~xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
     
         <!--设置-->
       <settings>
           <!--设置日志输出为log4j-->
           <setting name="logImpl" value="LOG4J"/>
           <!--设置数据名称转化-->
           <setting name="useActualParamName" value="true"/>
       </settings>
     
       <!--类型别名-->
       <typeAliases>
           <package name="xyz.rtx3090.pojo"/>
       </typeAliases>
   
     	<!--环境配置-->
       <environments default="development">
           <environment id="development">
               <transactionManager type="JDBC"/>
               <dataSource type="POOLED">
                   <property name="driver" value="com.mysql.jdbc.Driver"/>
                   <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=utf8"/>
                   <property name="username" value="root"/>
                   <property name="password" value="123456"/>
               </dataSource>
           </environment>
       </environments>
   
     	<!--包映射-->
       <mappers>
           <package name="xyz.rtx3090.mapper"/>
       </mappers>
   
   </configuration>
   ~~~

9. 编写测试类

   ~~~java
   package xyz.rtx3090.mapper;
   import ...
   
   public class UserMapperTest {
       @Test
       public void testQueryAllUser() {
           SqlSession sqlSession = MybatisUtils.getSqlSession();
           try{
               UserMapper mapper = sqlSession.getMapper(UserMapper.class);
               List<User> userList = mapper.queryAllUser();
               for (User user: userList
               ) {
                   System.out.println(user);
               }
           } catch (Exception e) {
               e.printStackTrace();
           } finally {
               sqlSession.close();
           }
       }
   }
   ~~~

## 学习Mybatis-Spring

MyBatis-Spring 会帮助你将 MyBatis 代码无缝地整合到 Spring 中。

在开始使用 MyBatis-Spring 之前，你需要先熟悉 Spring 和 MyBatis 这两个框架和有关它们的术语。这很重要

MyBatis-Spring 需要以下版本：

| MyBatis-Spring | MyBatis | Spring 框架 | Spring Batch | Java    |
| :------------- | :------ | :---------- | :----------- | :------ |
| 2.0            | 3.5+    | 5.0+        | 4.0+         | Java 8+ |
| 1.3            | 3.4+    | 3.2.2+      | 2.1+         | Java 6+ |

如果使用 Maven 作为构建工具，仅需要在 pom.xml 中加入以下代码即可：

```xml
<dependency>
   <groupId>org.mybatis</groupId>
   <artifactId>mybatis-spring</artifactId>
   <version>2.0.2</version>
</dependency>
```

要和 Spring 一起使用 MyBatis，需要在 Spring 应用上下文中定义至少两样东西：一个 SqlSessionFactory 和至少一个数据映射器类。

在 MyBatis-Spring 中，可使用SqlSessionFactoryBean来创建 SqlSessionFactory。要配置这个工厂 bean，只需要把下面代码放在 Spring 的 XML 配置文件中：

```xml
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
 <property name="dataSource" ref="dataSource" />
</bean>
```

注意：SqlSessionFactory需要一个 DataSource（数据源）。这可以是任意的 DataSource，只需要和配置其它 Spring 数据库连接一样配置它就可以了。

在基础的 MyBatis 用法中，是通过 SqlSessionFactoryBuilder 来创建 SqlSessionFactory 的。而在 MyBatis-Spring 中，则使用 SqlSessionFactoryBean 来创建。

在 MyBatis 中，你可以使用 SqlSessionFactory 来创建 SqlSession。一旦你获得一个 session 之后，你可以使用它来执行映射了的语句，提交或回滚连接，最后，当不再需要它的时候，你可以关闭 session。

SqlSessionFactory有一个唯一的必要属性：用于 JDBC 的 DataSource。这可以是任意的 DataSource 对象，它的配置方法和其它 Spring 数据库连接是一样的。

一个常用的属性是 configLocation，它用来指定 MyBatis 的 XML 配置文件路径。它在需要修改 MyBatis 的基础配置非常有用。通常，基础配置指的是 < settings> 或 < typeAliases>元素。

需要注意的是，这个配置文件并不需要是一个完整的 MyBatis 配置。确切地说，任何环境配置（<environments>），数据源（<DataSource>）和 MyBatis 的事务管理器（<transactionManager>）都会被忽略。SqlSessionFactoryBean 会创建它自有的 MyBatis 环境配置（Environment），并按要求设置自定义环境的值。

SqlSessionTemplate 是 MyBatis-Spring 的核心。作为 SqlSession 的一个实现，这意味着可以使用它无缝代替你代码中已经在使用的 SqlSession。

模板可以参与到 Spring 的事务管理中，并且由于其是线程安全的，可以供多个映射器类使用，你应该总是用 SqlSessionTemplate 来替换 MyBatis 默认的 DefaultSqlSession 实现。在同一应用程序中的不同类之间混杂使用可能会引起数据一致性的问题。

可以使用 SqlSessionFactory 作为构造方法的参数来创建 SqlSessionTemplate 对象。

```xml
<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
 <constructor-arg index="0" ref="sqlSessionFactory" />
</bean>
```

现在，这个 bean 就可以直接注入到你的 DAO bean 中了。你需要在你的 bean 中添加一个 SqlSession 属性，就像下面这样：

```java
public class UserDaoImpl implements UserDao {

 private SqlSession sqlSession;

 public void setSqlSession(SqlSession sqlSession) {
   this.sqlSession = sqlSession;
}

 public User getUser(String userId) {
   return sqlSession.getMapper...;
}
}
```

按下面这样，注入 SqlSessionTemplate：

```xml
<bean id="userDao" class="org.mybatis.spring.sample.dao.UserDaoImpl">
 <property name="sqlSession" ref="sqlSession" />
 </bean>
```

## Mybatis-Spring整合实现一

1. 删除整个原Mybatis项目中用于获取`SqlSession`对象的工具类`MybatisUtils`

2. 修改原Mybatis项目中的核心配置文件`mybatis-config.xml`，将其`环境配置`、`映射`部分删除，只留下基础的设置即可

3. 添加Spring配置文件`applicationContext.xml`

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <!--配置数据源：数据源有非常多，可以使用第三方的，也可使使用Spring的-->
       <bean id="dataSource"
             class="org.springframework.jdbc.datasource.DriverManagerDataSource">
           <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
           <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
           <property name="username" value="root"/>
           <property name="password" value="intmain()"/>
       </bean>
   
       <!--配置SqlSessionFactory-->
       <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
           <property name="dataSource" ref="dataSource"/>
           <!--关联Mybatis-->
           <property name="configLocation" value="classpath:mybatis-config.xml"/>
           <property name="mapperLocations" value="classpath:xyz/rtx3090/mapper/UserMapper.xml"/>
       </bean>
   
       <!--注册sqlSessionTemplate , 关联sqlSessionFactory-->
       <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
           <!--利用构造器注入-->
           <constructor-arg index="0" ref="sqlSessionFactory"/>
       </bean>
   
     	<!--提前导入beans.xml文件-->
       <import resource="beans.xml"/>
   
   </beans>
   ~~~

4. 添加mapper接口的实现类

   ```java
   package xyz.rtx3090.mapper;
   import ...
   
   public class UserMapperImpl implements UserMapper{
       //sqlSession不用我们自己创建了，Spring来管理
       private SqlSessionTemplate sqlSession;
   
       public void setSqlSession(SqlSessionTemplate sqlSession) {
           this.sqlSession = sqlSession;
       }
   
       @Override
       public List<User> queryAllUser() {
         	//注意这里的操作
           UserMapper mapper = sqlSession.getMapper(UserMapper.class);
           return mapper.queryAllUser();
       }
   
   }
   ```

5. 创建`beas.xml`文件用来注册上面添加的mapper接口的实现到spring中（这个文件我们已经提前导入到前面的`applicationContext.xml`中了）

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <bean id="userMapperImpl" class="xyz.rtx3090.mapper.UserMapperImpl">
           <property name="sqlSession" ref="sqlSession"/>
       </bean>
   </beans>
   ```

6. 编写测试类进行测试

   ~~~java
   package xyz.rtx3090.mapper;
   import ...
   
   public class UserMapperTest {
       //整合实现一
       @Test
       public void testQueryAllUser() {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           UserMapper userMapper = (UserMapper) context.getBean("userMapperImpl");
           List<User> userList = userMapper.queryAllUser();
           for (User user :
                   userList) {
               System.out.println(user);
           }
       }
   }
   ~~~

   > 输出结果：
   >
   > ```
   > User{id=1, name='one', pwd='111111'}
   > User{id=2, name='two', pwd='202020'}
   > User{id=3, name='third', pwd='303030'}
   > User{id=4, name='谢奕欣', pwd='xieyixin'}
   > User{id=5, name='帐务', pwd='呜呜呜呜呜呜'}
   > User{id=6, name='sixsixsix', pwd='6060sixsix'}
   > User{id=7, name='琪琪', pwd='qiqiqiqiqiqi'}
   > User{id=8, name='eight', pwd='888888'}
   > User{id=9, name='two', pwd='222222'}
   > ```
   >
   > 测试成功！mybatis-spring整合成功！

## Mybatis-Spring整合实现二

mybatis-spring1.2.3版以上才可以实现第二种整合

dao继承Support类 , 直接利用 getSqlSession() 获得 , 然后直接注入SqlSessionFactory . 比起方式1 , 不需要管理SqlSessionTemplate , 而且对事务的支持更加友好 . 可跟踪源码查看

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210602085744.png)

1. 修改mapper接口的实现类

   ```java
   package xyz.rtx3090.mapper;
   import ...
   
   public class UserMapperImpl extends SqlSessionDaoSupport implements UserMapper {
       //这里我们直接通过getSqlSession方法来获取SqlSession对象
       @Override
       public List<User> queryAllUser() {
           return getSqlSession().getMapper(UserMapper.class).queryAllUser();
       }
   }
   ```

2. 对应的去修改mapper接口实现类的xml配置配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">
       
       <bean id="userMapperImpl02" class="xyz.rtx3090.mapper.UserMapperImpl02">
           <property name="sqlSessionFactory" ref="sqlSessionFactory"/>
       </bean>
   </beans>
   ```

3. 在测试类中进行测试

   ```java
   package xyz.rtx3090.mapper;
   import ...
   
   public class UserMapperTest {
       //整合实现二
       @Test
       public void testQueryAllUser02() {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           UserMapper userMapperImpl02 = (UserMapper) context.getBean("userMapperImpl02");
           List<User> userList = userMapperImpl02.queryAllUser();
           for (User user :
                   userList) {
               System.out.println(user);
           }
       }
   }
   ```

   > 输出结果：
   >
   > ```
   > User{id=1, name='one', pwd='111111'}
   > User{id=2, name='two', pwd='202020'}
   > User{id=3, name='third', pwd='303030'}
   > User{id=4, name='谢奕欣', pwd='xieyixin'}
   > User{id=5, name='帐务', pwd='呜呜呜呜呜呜'}
   > User{id=6, name='sixsixsix', pwd='6060sixsix'}
   > User{id=7, name='琪琪', pwd='qiqiqiqiqiqi'}
   > User{id=8, name='eight', pwd='888888'}
   > User{id=9, name='two', pwd='222222'}
   > ```
   >
   > 测试成功！Mybatis-Spring整合实现方案二成功！
   >
   > **整合到spring以后可以完全不要mybatis的配置文件，除了这些方式可以实现整合之外，我们还可以使用注解来实现，这个等我们后面学习SpringBoot的时候还会测试整合！**

## Spring中的事务管理

### 回顾事务

- 事务在项目开发过程非常重要，涉及到数据的一致性的问题，不容马虎！
- 事务管理是企业级应用程序开发中必备技术，用来确保数据的完整性和一致性。

事务就是把一系列的动作当成一个独立的工作单元，这些动作要么全部完成，要么全部不起作用。

**事务四个属性ACID**

1. 原子性（atomicity）

2. - 事务是原子性操作，由一系列动作组成，事务的原子性确保动作要么全部完成，要么完全不起作用

3. 一致性（consistency）

4. - 一旦所有事务动作完成，事务就要被提交。数据和资源处于一种满足业务规则的一致性状态中

5. 隔离性（isolation）

6. - 可能多个事务会同时处理相同的数据，因此每个事务都应该与其他事务隔离开来，防止数据损坏

7. 持久性（durability）

8. - 事务一旦完成，无论系统发生什么错误，结果都不会受到影响。通常情况下，事务的结果被写到持久化存储器中

### 不开启事务测试

1. 在上述代码的基础下进行修改，我们给mapper接口添加两个方法（添加和删除用户的方法）

   ~~~java
   package xyz.rtx3090.mapper;
   import xyz.rtx3090.pojo.User;
   import java.util.List;
   
   public interface UserMapper {
   
       //添加指定用户
       int insertUser(User user);
   
       //删除指定用户
       int deleteUser(int id);
   }
   ~~~

2. mapper接口的xml配置文件中，我故意把删除用户的SQL语句写错

   ~~~xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="xyz.rtx3090.mapper.UserMapper">
   
       <!--添加指定用户-->
       <insert id="insertUser" parameterType="user">
           insert into mybatis.user (id, name, pwd) VALUES (#{id},#{name},#{pwd});
       </insert>
   
       <!--删除指定用户-->
       <delete id="deleteUser" parameterType="_int">
         	<!--注意这里我故意将其写错-->
           deletes from mybatis.user where id = #{id};
       </delete>
   </mapper>
   ~~~

3. 我们编写mapper接口的实现类，实现我们刚才编写的两个方法 

   ```java
   package xyz.rtx3090.mapper;
   import org.mybatis.spring.support.SqlSessionDaoSupport;
   import xyz.rtx3090.pojo.User;
   import java.util.List;
   
   public class UserMapperImpl extends SqlSessionDaoSupport implements UserMapper{
   
       @Override
       public int insertUser(User user) {
           return getSqlSession().getMapper(UserMapper.class).insertUser(user);
       }
   
       @Override
       public int deleteUser(int id) {
           return getSqlSession().getMapper(UserMapper.class).deleteUser(id);
       }
   }
   ```

4. 在测试类中进行测试

   ```java
   package xyz.rtx3090.mapper;
   import ...
     
   public class mapperTest {
   
       @Test
       public void testTransaction() {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           UserMapper userMapperImpl = context.getBean("userMapperImpl", UserMapper.class);
   
           User ten = new User(10, "ten", "10101010");
           int i = userMapperImpl.insertUser(ten);
           System.out.println(i == 1 ? "添加成功":"添加失败");
   
           int id = 9;
           int i1 = userMapperImpl.deleteUser(id);
           System.out.println(i == 1 ? "删除成功":"删除失败");
       }
   }
   ```

   > 结果不出意料的报错了，由于是deleteUser方法的SQL语句错了，所以添加用户的动作成功了，而删除用户的动作失败。这就出现了事务问题。
   >
   > 没有进行事务的管理；我们想让他们都成功才成功，有一个失败，就都失败，我们就应该需要**事务！**
   >
   > 以前我们都需要自己手动管理事务，十分麻烦！但是Spring给我们提供了事务管理，我们只需要配置即可；

### 开启事务测试

Spring在不同的事务管理API之上定义了一个抽象层，使得开发人员不必了解底层的事务管理API就可以使用Spring的事务管理机制。Spring支持编程式事务管理和声明式的事务管理。

**编程式事务管理**

- 将事务管理代码嵌到业务方法中来控制事务的提交和回滚
- 缺点：必须在每个事务操作业务逻辑中包含额外的事务管理代码

**声明式事务管理**

- 一般情况下比编程式事务好用。
- 将事务管理代码从业务方法中分离出来，以声明的方式来实现事务管理。
- 将事务管理作为横切关注点，通过aop方法模块化。Spring中通过Spring AOP框架支持声明式事务管理。

**使用Spring管理事务，注意头文件的约束导入**

```xml
xmlns:tx="http://www.springframework.org/schema/tx"

http://www.springframework.org/schema/tx
http://www.springframework.org/schema/tx/spring-tx.xsd">
```

**事务管理器**

- 无论使用Spring的哪种事务管理策略（编程式或者声明式）事务管理器都是必须的。
- 就是 Spring的核心事务管理抽象，管理封装了一组独立于技术的方法。

**spring事务传播特性**

事务传播行为就是多个事务方法相互调用时，事务如何在这些方法间传播。spring支持7种事务传播行为：

- propagation_requierd：如果当前没有事务，就新建一个事务，如果已存在一个事务中，加入到这个事务中，这是最常见的选择。
- propagation_supports：支持当前事务，如果没有当前事务，就以非事务方法执行。
- propagation_mandatory：使用当前事务，如果没有当前事务，就抛出异常。
- propagation_required_new：新建事务，如果当前存在事务，把当前事务挂起。
- propagation_not_supported：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。
- propagation_never：以非事务方式执行操作，如果当前事务存在则抛出异常。
- propagation_nested：如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与propagation_required类似的操作

Spring 默认的事务传播行为是 PROPAGATION_REQUIRED，它适合于绝大多数的情况。

假设 ServiveX#methodX() 都工作在事务环境下（即都被 Spring 事务增强了），假设程序中存在如下的调用链：Service1#method1()->Service2#method2()->Service3#method3()，那么这 3 个服务类的 3 个方法通过 Spring 的事务传播机制都工作在同一个事务中。

就好比，我们刚才的几个方法存在调用，所以会被放在一组事务当中！

1. 修改spring配置文件`applicationContext.xml`，添加以下内容（注意添加头文件约束：tx、aop）

   ```xml
       <!--JDBC事务-->
       <bean id="transactionManager"
             class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
           <property name="dataSource" ref="dateSource"/>
       </bean>
   
       <!--配置事务通知-->
       <tx:advice id="txAdvice" transaction-manager="transactionManager">
           <tx:attributes>
               <!--配置哪些方法使用什么样的事务,配置事务的传播特性-->
               <tx:method name="add" propagation="REQUIRED"/>
               <tx:method name="delete" propagation="REQUIRED"/>
               <tx:method name="update" propagation="REQUIRED"/>
               <tx:method name="search" propagation="REQUIRED"/>
               <tx:method name="get" read-only="true"/>
               <!--日常开发，直接使用这个即可-->
               <tx:method name="*" propagation="REQUIRED"/>
           </tx:attributes>
       </tx:advice>
   
       <!--配置aop织入事务-->
       <aop:config>
           <aop:pointcut id="txPointcut" expression="execution(* xyz.rtx3090.mapper.*.*(..))"/> 
           <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointcut"/>
       </aop:config>
   ```

2. 在mapper接口中添加一个方法，用来测试事务管理

   ~~~java
   package xyz.rtx3090.mapper;
   
   import xyz.rtx3090.pojo.User;
   
   
   public interface UserMapper {
   
       //添加指定用户
       int insertUser(User user);
   
       //删除指定用户
       int deleteUser(int id);
   
       //测试事务管理
       int transactionManger();
   }
   ~~~

3. 修改mapper接口的实现类

   ~~~java
   package xyz.rtx3090.mapper;
   
   import org.mybatis.spring.support.SqlSessionDaoSupport;
   import xyz.rtx3090.pojo.User;
   
   
   public class UserMapperImpl extends SqlSessionDaoSupport implements UserMapper{
   
       @Override
       public int insertUser(User user) {
           return getSqlSession().getMapper(UserMapper.class).insertUser(user);
       }
   
       @Override
       public int deleteUser(int id) {
           return getSqlSession().getMapper(UserMapper.class).deleteUser(id);
       }
   
       @Override
       public int transactionManger() {
           int x = getSqlSession().getMapper(UserMapper.class).insertUser(new User(11,"uiyi","十一十一"));
           int y = getSqlSession().getMapper(UserMapper.class).deleteUser(10);
           return x + y;
       }
   }
   ~~~

4. mapper接口对应的xml配置文件中的错误保持不变

5. 在测试类中进行测试

   ~~~java
   package xyz.rtx3090.mapper;
   import ...
   
   
   public class mapperTest {
       @Test
       public void testTransaction() {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           UserMapper userMapperImpl = context.getBean("userMapperImpl", UserMapper.class);
           int i = userMapperImpl.transactionManger();
           System.out.println(i);
       }
   }
   ~~~

   > 结果当然还是同意的报错，但是这一次开启了事务，就没有出现添加用户动作成功，但是删除用户动作失败的情况了。保证了数据的安全

--------

# END

