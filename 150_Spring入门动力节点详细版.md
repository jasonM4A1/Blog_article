# Spring介绍

## Spring框架是什么？

**Spring 是于 2003 年兴起的一个轻量级的 Java 开发框架，它是为了解决企业应用开发的复杂性而创建的。Spring 的核心是控制反转（IoC）和面向切面编程（AOP）。Spring 是可以在 Java SE/EE 中使用的轻量级开源框架。**

Spring 的主要作用就是为代码“解耦”，降低代码间的耦合度。就是让对象和对象（模块和模块）之间关系不是使用代码关联，而是通过配置来说明。即在 Spring 中说明对象（模 块）的关系。 

Spring 根据代码的功能特点，使用 Ioc 降低业务对象之间耦合度。IoC 使得主业务在相互调用过程中，不用再自己维护关系了，即不用再自己创建要使用的对象了。而是由 Spring 容器统一管理，自动“注入”,注入即赋值。 而 AOP 使得系统级服务得到了最大复用，且不用再由程序员手工将系统级服务“混杂”到主业务逻辑中了，而是由 Spring 容器统一完成 “织入”。

## Spring的优点

Spring 是一个框架，是一个半成品的软件。有 20 个模块组成。它是一个容器管理对象， 容器是装东西的，Spring 容器不装文本，数字。装的是对象。Spring 是存储对象的容器。

### 轻量

Spring 框架使用的 jar 都比较小，一般在 1M 以下或者几百 kb。Spring 核心功能的所需 的 jar 总共在 3M 左右。 Spring 框架运行占用的资源少，运行效率高。不依赖其他 jar

###  针对接口编程，解耦合

Spring 提供了 Ioc 控制反转，由容器管理对象，对象的依赖关系。原来在程序代码中的 对象创建方式，现在由容器完成。对象之间的依赖解耦合。

### AOP编程的支持

通过 Spring 提供的 AOP 功能，方便进行面向切面的编程，许多不容易用传统 OOP 实现 的功能可以通过 AOP 轻松应付 在 Spring 中，开发人员可以从繁杂的事务管理代码中解脱出来，通过声明式方式灵活地 进行事务的管理，提高开发效率和质量。

### 方便集成各种优秀的框架

Spring 不排斥各种优秀的开源框架，相反 Spring 可以降低各种框架的使用难度，Spring 提供了对各种优秀框架（如 Struts,Hibernate、MyBatis）等的直接支持。简化框架的使用。 Spring 像插线板一样，其他框架是插头，可以容易的组合到一起。需要使用哪个框架，就把这个插头放入插线板。不需要可以轻易的移除。

## Spring体系结构

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210525171634.png)

Spring 由 20 多个模块组成，它们可以分为数据访问/集成（Data Access/Integration）、 Web、面向切面编程（AOP, Aspects）、提供JVM的代理（Instrumentation）、消息发送（Messaging）、 核心容器（Core Container）和测试（Test）。

## 为什么使用Spring

 目的就是减少对代码的改动， 也能实现不同的功能。 实现解耦合。 

# IoC——控制反转

## Java中创建对象有哪些方式

1. 构造方法
2. 反射
3. 序列化
4. 克隆
5. IOC
6. 动态代理

## IoC概述

控制反转（IoC，Inversion of Control），是一个概念，是一种思想。指将传统上由程序代码直接操控的对象调用权交给容器，通过容器来实现对象的装配和管理。控制反转就是对对象控制权的转移，从程序代码本身反转到了外部容器。通过容器实现对象的创建，属性赋值， 依赖的管理。把对象的创建，赋值，管理工作都交给代码之外的容器实现， 也就是对象的创建是有其它外部资源完成。

Spring 框架使用依赖注入（DI）实现 IoC。

Spring 容器是一个超级大工厂，负责创建、管理所有的 Java 对象，这些 Java 对象被称 为 Bean。Spring 容器管理着容器中 Bean 之间的依赖关系，Spring 使用“依赖注入”的方式 来管理 Bean 之间的依赖关系。使用 IoC 实现对象之间的解耦和。

+ **控制**：创建对象，对象的属性赋值，对象之间的关系管理
+ **反转**：把原来的开发人员管理，创建对象的权限转移给代码之外的容器实现。 由容器代替开发人员管理对象。创建对象，
          给属性赋值。
+ **正转**：由开发人员在代码中，使用new 构造方法创建对象， 开发人员主动管理对象
+ **容器**：是一个服务器软件， 一个框架（spring）

## 依赖与依赖注入

### 依赖

classA 类中含有 classB 的实例，在 classA 中调用 classB 的方法完成功能，即 classA 对 classB 有依赖。

### 依赖注入

DI(Dependency Injection)，程序代码不做定位查询，这些工作由容器自行完成。 依赖注入 DI 是指程序运行过程中，若需要调用另一个对象协助时，无须在代码中创建 被调用者，而是依赖于外部容器，由外部容器创建后传递给程序。 Spring 的依赖注入对调用者与被调用者几乎没有任何要求，完全支持对象之间依赖关系 的管理。

依赖注入， 只需要在程序中提供要使用的对象名称就可以， 至于对象如何在容器中创建， 赋值，查找都由容器内部实现。

DI 是ioc的技术实现。

spring是使用的di实现了ioc的功能， spring底层创建对象，使用的是反射机制。

spring是一个容器，管理对象，给属性赋值， 底层是反射创建对象。

## 第一个Spring程序

### 开发环境准备

1. jdk版本1.8
2. idea版本2020.2.4
3. Maven版本3.6.1

### 创建工程项目

1. 创建一个`maven-archetype-quickstart`模版的Maven工程

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210624143121.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210624143343.png)

2. 配置pom.xml文件(我创建的子项目)

   ~~~xml
       <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-context</artifactId>
         <version>5.3.8</version>
       </dependency>
   ~~~
   
3. 定义SomeService接口

   ~~~java
   package xyz.rtx3090.service;
   
   public interface SomeService {
       void doSome();
   }
   ~~~

4. 编写接口的实现类SomeServiceImpl

   ~~~java
   package xyz.rtx3090.service;
   
   public class SomeServiceImpl implements SomeService{
       public SomeServiceImpl() {
           super();
           System.out.println("SomeServiceImpl实现类的无参构造方法");
       }
       @Override
       public void doSome() {
           System.out.println("业务方法doSome()");
       }
   }
   ~~~

5. 在src/main/resources/目录下创建一个名为`applicationContext.xml`的配置文件，并在配置文件中编写bean

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <!--注册bean对象
           id：自定义对象的名称，通过id在代码中使用对象
           class：类的全限定名称，不能是接口-->
       <bean id="someService" class="xyz.rtx3090.service.SomeServiceImpl"/>
   </beans>
   ~~~

6. 定义测试类，并进行Spring框架使用的测试

   ```java
   package xyz.rtx3090;
   
   import org.junit.Test;
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   import xyz.rtx3090.service.SomeServiceImpl;
   
   public class MyTest {
       @Test
       public void test01() {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           SomeServiceImpl someService = context.getBean("someService", SomeServiceImpl.class);
           someService.doSome();
       }
   }
   ```

   > 测试结果：
   >
   > SomeServiceImpl实现类的无参构造方法
   > 业务方法doSome()

到这里我们这一个Spring程序已经创建并运行成功了，我们可以仅仅通过xml配置文件让IoC容器来帮助我们创建和配置Java对象。

## 使用Spring创建非自定义类对象

1. 我们在`第一个Spring程序`项目代码的基础上修改`applicationContext.xml`配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <!--使用Spring创建非自定义类对象-->
       <bean id="myDate" class="java.util.Date"/>
   </beans>
   ```

2. 在测试类中进行测试

   ```java
   package xyz.rtx3090;
   
   import org.junit.Test;
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   
   import java.util.Date;
   
   public class MyTest {
   
       @Test
       public void test02() {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           Date myDate = context.getBean("myDate", Date.class);
           System.out.println(myDate);
       }
   }
   ```

   > 测试结果：
   >
   > SomeServiceImpl实现类的无参构造方法
   > Thu Jun 24 15:11:04 CST 2021

## 容器接口和实现类

### ApplicationContext容器接口

ApplicationContext 用于加载 Spring 的配置文件，在程序中充当“容器”的角色。其实现类有两个。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210624151745.png)

我们主要使用`ClassPathXmlApplicationContext`这个实现类，来加载在项目路径中的配置文件。

```java
ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
```

#### 使用Spring容器创建Java对象

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210624152138.png)

## 基于XML的依赖注入(DI)

### 依赖注入概述

bean 实例在调用无参构造器创建对象后，就要对 bean 对象的属性进行初始化。初始化是由容器自动完成的，称为注入。

### 依赖注入方式分类

根据注入方式的不同，常用的有两类：**set方法注入**、**构造方法注入**。

#### set方法注入

**set 注入也叫设值注入，是指通过 setter 方法传入被调用者的实例。这种注入方式简单、 直观，因而在 Spring 的依赖注入中大量使用。**

```xml
    <bean id="student" class="xyz.rtx3090.pojo.Student">
        <property name="id" value="01"/><!--setId(01)-->
        <property name="name" value="Jason"/><!--setName("Jason")-->
        <property name="school" ref="school"/><!--setSchool(school)-->
    </bean>
```

1. 创建一个Maven项目，结构目录如图所示

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210624154804.png)

2. 编写pom.xml配置文件，导入相关Jar包

   ```xml
       <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-context</artifactId>
         <version>5.3.8</version>
       </dependency>
   ```
   
3. 编写School类（只写setter方法和重写toString方法）

   ~~~java
   package xyz.rtx3090.pojo;
   
   public class School {
       private String name;
       private String address;
   
       //setter
   		//toString
   }
   ~~~
   
4. 编写Student类（只写setter方法和重写toString方法）

   ```java
   package xyz.rtx3090.pojo;
   
   public class Student {
       private int id;
       private String name;
       private School school;
   
       //setter
   		//toString
   }
   ```
   
5. 编写applicationContext.xml配置文件，并进行set方法依赖注入

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <bean id="school" class="xyz.rtx3090.pojo.School">
           <property name="name" value="清华大学"/><!--setName("清华大学")-->
           <property name="address" value="北京"/><!--setAddress("北京")-->
       </bean>
   
       <bean id="student" class="xyz.rtx3090.pojo.Student">
           <property name="id" value="01"/><!--setId(01)-->
           <property name="name" value="Jason"/><!--setName("Jason")-->
           <property name="school" ref="school"/><!--setSchool(school)-->
       </bean>
   </beans>
   ```

   > + 简单属性赋值采用value
   > + 引用属性赋值采用ref

6. 编写测试类进行测试

   ```java
   package xyz.rtx3090.pojo;
   
   import org.junit.Test;
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   
   public class MyTest {
       @Test
       public void test01() {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           Student student = context.getBean("student", Student.class);
           System.out.println(student);
       }
   }
   ```

   > 测试结果：
   >
   > Student{id=1, name='Jason', school=School{name='清华大学', address='北京'}}

#### 构造方法注入

**构造注入是指，在构造调用者实例的同时，完成被调用者的实例化。即使用构造器设 置依赖关系。**

```xml
    <bean id="dog" class="xyz.rtx3090.pojo.Dog">
        <constructor-arg name="id" value="1011"/>
        <constructor-arg name="name" value="小胖"/>
        <constructor-arg name="healthy" value="true"/>
        <constructor-arg name="animal" ref="animal"/>
    </bean>
```

1. 创建一个Maven项目，结构目录如图所示

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210624162120.png)

2. 编写pom.xml配置文件，导入相关Jar包

   ```xml
       <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-context</artifactId>
         <version>5.3.8</version>
       </dependency>
   ```
   
3. 编写Animal类（只写有参构造方法和重写toString方法）

   ```java
   package xyz.rtx3090.pojo;
   
   public class Animal {
       private String type;
       private String place;
   
       //constructor(有参)
       //toString
   }
   ```
   
4. 编写Cat类（只写有参构造方法和重写toString方法）

   ```java
   package xyz.rtx3090.pojo;
   
   public class Cat {
       private int id;
       private String name;
       private Boolean healthy;
       private Animal animal;
   
       //constructor(有参)
       //toString
   }
   ```
   
5. 编写Dog类（只写有参构造方法和重写toString方法）

   ```java
   package xyz.rtx3090.pojo;
   
   public class Dog {
       private int id;
       private String name;
       private Boolean healthy;
       private Animal animal;
   
       //constructor
       public Dog(int id, String name, Boolean healthy, Animal animal) {
           this.id = id;
           this.name = name;
           this.healthy = healthy;
           this.animal = animal;
       }
   
       @Override
       public String toString() {
           return "Dog{" +
                   "id=" + id +
                   ", name='" + name + '\'' +
                   ", healthy=" + healthy +
                   ", animal=" + animal +
                   '}';
       }
   }
   ```

6. 编写applicationContext.xml配置文件，并进行构造方法依赖注入

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <bean id="animal" class="xyz.rtx3090.pojo.Animal">
           <constructor-arg name="type" value="pets"/>
           <constructor-arg name="place" value="黄石公园"/>
       </bean>
   
       <bean id="cat" class="xyz.rtx3090.pojo.Cat">
           <constructor-arg name="id" value="1010"/>
           <constructor-arg name="name" value="元宝儿"/>
           <constructor-arg name="healthy" value="true"/>
           <constructor-arg name="animal" ref="animal"/>
       </bean>
   
       <bean id="dog" class="xyz.rtx3090.pojo.Dog">
           <constructor-arg name="id" value="1011"/>
           <constructor-arg name="name" value="小胖"/>
           <constructor-arg name="healthy" value="true"/>
           <constructor-arg name="animal" ref="animal"/>
       </bean>
   </beans>
   ```

7. 编写测试类进行测试

   ```java
   package xyz.rtx3090.pojo;
   
   import org.junit.Test;
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   
   public class MyTest {
       @Test
       public void test01() {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           Cat cat = context.getBean("cat", Cat.class);
           Dog dog = context.getBean("dog", Dog.class);
           System.out.println(cat);
           System.out.println(dog);
       }
   }
   ```

   > 测试结果：
   >
   > Cat{id=1010, name='元宝儿', healthy=true, animal=Animal{type='pets', place='黄石公园'}}
   > Dog{id=1011, name='小胖', healthy=true, animal=Animal{type='pets', place='黄石公园'}}

##### 案例：使用构造注入创建一个系统类 File 对象

1. 编写applicationContext.xml配置文件，并进行构造方法依赖注入

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <!--通过构造注入，创建一个jdk自带的类对象，路径为/Users/bernardo/Public/星辰大海.mp3-->
       <bean id="file" class="java.io.File">
           <constructor-arg name="parent" value="/Users/bernardo/Public"/>
           <constructor-arg name="child" value="星辰大海.mp3"/>
       </bean>
   </beans>
   ```

2. 在测试类中进行测试

   ```java
   package xyz.rtx3090.pojo;
   
   import org.junit.Test;
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   
   import java.io.File;
   import java.io.IOException;
   
   public class MyTest {
       @Test
       public void test02() {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           File file = context.getBean("file", File.class);
           boolean newFile = false;
           try {
               newFile= file.createNewFile();
           } catch (IOException e) {
               e.printStackTrace();
           }
           if (newFile) {
               System.out.println("创建mp3文件成功!");
           } else {
               System.out.println("创建mp3文件失败!");
           }
       }
   }
   ```

   > 测试结果：
   >
   > 创建mp3文件成功!

### 属性自动注入

> 注意自动注入只能是对于引用类型，基本属性只能手动赋值，不能自动注入

对于引用类型属性的注入，也可不在配置文件中显示的注入。可以通过为标签 设置 autowire 属性值，为引用类型属性进行隐式自动注入（默认是不自动注入引用类型属性）。根据自动注入判断标准的不同，可以分为两种：

+ byName——根据名称自动注入
+ byType——根据类型自动注入

#### byName方式自动注入

当配置文件中被调用者 bean 的 id 值与代码中调用者 bean 类的属性名相同时，可使用 byName 方式，让容器自动将被调用者 bean 注入给调用者 bean。容器是通过调用者的 bean 类的属性名与配置文件的被调用者 bean 的 id 进行比较而实现自动注入的。

1. 编写Teacher类（只写setter方法和重写toString方法）

   ```java
   package xyz.rtx3090.pojo;
   
   public class Teacher {
       private int id;
       private String name;
   
       //setter
   		//toString
   }
   ```

2. 编写Student类（只写setter方法和重写toString方法）

   ```java
   package xyz.rtx3090.pojo;
   
   public class Student {
       private int id;
       private String name;
       private Teacher teacher;
   
       //setter
   		//toString
   }
   ```

3. **编写applicationContext.xml配置文件，采用byName方式实现引用属性的自动注入**

   ```xml
       <bean id="teacher" class="xyz.rtx3090.pojo.Teacher">
           <property name="id" value="01"/>
           <property name="name" value="Jason"/>
       </bean>
   
       <bean id="student" class="xyz.rtx3090.pojo.Student" autowire="byName">
           <property name="id" value="01"/>
           <property name="name" value="Bernardo"/>
           <!--上面bean的id属性与student类的属性名一致，所以可以采用byName的方式自动注入-->
       </bean>
   ```

#### byType方式自动注入

使用 byType 方式自动注入，要求：配置文件中被调用者 bean 的 class 属性指定的类， 要与代码中调用者 bean 类的某引用类型属性类型同源。即要么相同，要么有 is-a 关系（子 类，或是实现类）。但这样的同源的被调用 bean 只能有一个。多于一个，容器就不知该匹配哪一个了。

1. 编写Cat类（只写setter方法和重写toString方法）

   ```java
       package xyz.rtx3090.pojo;
   
       public class Cat {
           private String name;
           private String color;
           private Boolean healthy;
   
           //setter
   				//toString
       }
   ```

2. 编写Person类（只写setter方法和重写toString方法）

   ```java
   package xyz.rtx3090.pojo;
   
   public class Person {
       private int id;
       private String name;
       private Cat cat;
   
       //setter
   		//toString
   }
   ```

3. **编写applicationContext.xml配置文件，采用byType方式实现引用属性的自动注入**

   ```xml
       <bean id="cat" class="xyz.rtx3090.pojo.Cat">
           <property name="name" value="元宝儿"/>
           <property name="color" value="yellow"/>
           <property name="healthy" value="true"/>
       </bean>
   
       <bean id="person" class="xyz.rtx3090.pojo.Person" autowire="byType">
           <property name="id" value="01"/>
           <property name="name" value="Jason"/>
           <!--上面bean的class属性类型与person类的某引用属性类型同源，所以可以采用byType的方式自动注入-->
       </bean>
   ```

### 为应用指定多个 Spring 配置文件

在实际应用里，随着应用规模的增加，系统中 Bean 数量也大量增加，导致配置文件变 得非常庞大、臃肿。为了避免这种情况的产生，提高配置文件的可读性与可维护性，可以将 Spring 配置文件分解成多个配置文件。

多个配置文件中有一个总文件，总配置文件将各其它子文件通过引入。在 Java 代码中只需要使用总配置文件对容器进行初始化即可。

1. 创建第一个spring配置文件`xyz/rtx3090/pojo/spring-cat.xml`

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <bean id="cat" class="xyz.rtx3090.pojo.Cat">
           <property name="name" value="元宝儿"/>
           <property name="color" value="yellow"/>
           <property name="healthy" value="true"/>
       </bean>
   </beans>
   ```

2. 创建第二个spring配置文件`xyz/rtx3090/pojo/spring-person.xml`

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <bean id="person" class="xyz.rtx3090.pojo.Person" autowire="byType">
           <property name="id" value="01"/>
           <property name="name" value="Jason"/>
           <!--上面bean的class属性类型与person类的某引用属性类型同源，所以可以采用byType的方式自动注入-->
       </bean>
   </beans>
   ```

3. **创建第三个spring配置文件`total.xml`，用于整合之前的两个配置文件**

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <import resource="classpath:xyz/rtx3090/pojo/spring-cat.xml"/>
       <import resource="classpath:xyz/rtx3090/pojo/spring-person.xml"/>
   </beans>
   ```

4. 三个文件的目录结构如图所示

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210625093947.png)

## 基于注解的DI

### 组件扫描

对于 DI 使用注解，将不再需要在 Spring 配置文件中声明 bean 实例。Spring 中使用注解， 需要在原有 Spring 运行环境基础上再做一些改变。 需要在 Spring 配置文件中配置组件扫描器，用于在指定的基本包中扫描注解。

#### 扫描单个包

```xml
<context:component-scan base-package="xyz.rtx3090.pojo"/>
```

#### 扫描多个包

1. 使用多个 context:component-scan 指定不同的包路径

   ```xml
       <context:component-scan base-package="xyz.rtx3090.pojo"/>
       <context:component-scan base-package="xyz.rtx3090.mapper"/>
   ```

2. 指定 base-package 的值使用分隔符【分隔符可以使用逗号 ，分号；还可以使用空格，但不建议使用空格】

   ```xml
   <context:component-scan base-package="xyz.rtx3090.pojo;xyz.rtx3090.mapper"/>
   ```

3. 指定base-package 到父包名

   ```xml
   <context:component-scan base-package="xyz.rtx3090"/>
   ```

   > 但不建议使用顶级的父包，扫描的路径比较多，导致容器启动时间变慢。指定到目标包和合 适的。也就是注解所在包全路径。

### 定义 Bean 的注解@Component

需要在类上使用注解@Component，该注解的 value 属性用于指定该 bean 的 id 值。作用相等于applicationContext.xml配置文件中的`<bean id="" class=""/`

```java
@Component(value = "student")
public class Student{
  private int id;
  private String name;
  private School school;
}
```

> @Component(value = "student")还可以有下面两种写法：
>
> 1. @Component("student")——省略value
> 2. @Component——省略赋值命名，这样value的值默认为小写类名

**另外，Spring 还提供了 3 个创建对象的注解：**

1. @Repository——用于对 DAO 实现类进行注解
2. @Service——用于对 Service 实现类进行注解
3. @Controller——用于对 Controller 实现类进行注解

> 这三个注解与@Component 都可以创建对象，但这三个注解还有其他的含义，@Service 创建业务层对象，业务层对象可以加入事务功能，@Controller 注解创建的对象可以作为处 理器接收用户的请求。 @Repository，@Service，@Controller 是对@Component 注解的细化，标注不同层的对 象。即持久层对象，业务层对象，控制层对象。

### 简单类型属性注入@Value

需要在属性上使用注解@Value，该注解的 value 属性用于指定要注入的值。 使用该注解完成属性注入时，类中无需 setter。当然，若属性有 setter，则也可将其加 到 setter 上。

```java
@Component(value = "school")
public class School {
    @Value("清华大学")
    private String name;
    @Value("北京")
    private String address;
  
}
```

### byType 引用类型属性自动注入@Autowired

需要在引用属性上使用注解@Autowired，该注解默认使用按类型自动装配 Bean 的方式。 使用该注解完成属性注入时，类中无需 setter。当然，若属性有 setter，则也可将其加 到 setter 上。

```java
@Component(value = "student")
public class Student {
    @Value("10")
    private int id;
    @Value("Bernardo")
    private String name;
    @Autowired
    private School school;

}
```

### byName 自动注入@Autowired 与@Qualifier

如果需要指定name自动注入，就需要在引用属性上联合使用注解@Autowired 与@Qualifier。@Qualifier 的 value 属性用 于指定要匹配的 Bean 的 id 值。类中无需 set 方法，也可加到 set 方法上(@Qualifier不能单独使用)。

```java
@Component(value = "student")
public class Student {
    @Value("10")
    private int id;
    @Value("Bernardo")
    private String name;
    @Autowired
    @Qualifier("school")
    private School school;

}
```

@Autowired 还有一个属性 required，默认值为 true，表示当匹配失败后，会终止程序运 行。若将其值设置为 false，则匹配失败，将被忽略，未匹配的属性值为 null。

```java
@Component(value = "student")
public class Student {
    @Value("10")
    private int id;
    @Value("Bernardo")
    private String name;
    @Autowired(required = false)
    @Qualifier("school")
    private School school;

}
```

### JDK 注解@Resource 自动注入

Spring提供了对 jdk中@Resource注解的支持。@Resource 注解既可以按名称匹配Bean， 也可以按类型匹配 Bean。默认是按名称注入。使用该注解，要求 JDK 必须是 6 及以上版本。 @Resource 可在属性上，也可在 set 方法上。

+ 首先按照默认的与属性名相同的名称匹配。
+ 默认名称匹配失败，则按照类型进行匹配
+ 当然我们也可以采用name属性来指定匹配的名称

1. **默认按名称注入，其次按照类型注入**

   @Resource 注解若不带任何参数，采用默认按属性名的方式注入，如果按默认属性名注入失败， 则会按照类型进行 Bean 的匹配注入。

   ```java
   @Component(value = "student")
   public class Student {
       @Value("10")
       private int id;
       @Value("Bernardo")
       private String name;
       @Resource
       private School school;
   
   }
   ```

2. **设置按指定名称注入**

   @Resource 注解指定其 name 属性，则 name 的值即为按照名称进行匹配的 Bean 的 id。

   ```java
   @Component(value = "student")
   public class Student {
       @Value("10")
       private int id;
       @Value("Bernardo")
       private String name;
       @Resource(name = "school")
       private School school;
   
   }
   ```

### 小结

1. @Autowired与@Resource都可以用来装配bean。都可以写在字段上，或写在setter方法上。
2. @Autowired默认按类型装配（属于spring规范），其次按照名字装配，但无法指定名字。默认情况下必须要求依赖对象必须存在，如果要允许null 值，可以设置它的required属性为false，如：@Autowired(required=false) ，如果我们想使用名称装配可以结合@Qualifier注解进行使用
3. @Resource（属于J2EE规范），默认按照名称进行装配，名称可以通过name属性进行指定。如果没有指定name属性，当注解写在字段上时，默认取字段名进行按照名称查找，如果注解写在setter方法上默认取属性名进行装配。当找不到与名称匹配的bean时才按照类型进行装配。但是需要注意的是，如果name属性一旦指定，就只会按照名称进行装配。
4. 它们的作用相同都是用注解方式注入对象，但执行顺序不同。@Autowired先byType，@Resource先byName。

### 案例演示

1. 创建Maven项目，工程结构如图所示

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210625101210.png)

2. 配置pom.xml文件

   ```xml
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-context</artifactId>
         <version>5.3.8</version>
       </dependency>
   ```

3. 在Spring配置文件中配置组件扫描器，用于在指定的基本包中扫描注解

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
   
       <context:component-scan base-package="xyz.rtx3090.pojo"/>
   </beans>
   ```

4. 编写School类，采用注解给进行属性的依赖注入

   ```java
   package xyz.rtx3090.pojo;
   
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.stereotype.Component;
   
   @Component(value = "school")
   public class School {
       @Value("清华大学")
       private String name;
       @Value("北京")
       private String address;
   
       //setter
   		//toString
   }
   ```

5. 编写Student类，采用注解给进行属性的依赖注入

   ```java
   package xyz.rtx3090.pojo;
   
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.beans.factory.annotation.Qualifier;
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.stereotype.Component;
   
   @Component(value = "student")
   public class Student {
       @Value("10")
       private int id;
       @Value("Bernardo")
       private String name;
       @Autowired//根据类型进行自动注入
       private School school;
   
       //setter
   		//toString
   }
   ```

6. 在测试类中进行测试

   ```java
       @Test
       public void test01() {
           ApplicationContext context = new ClassPathXmlApplicationContext("xyz/rtx3090/pojo/applicationContext.xml");
           Student student = context.getBean("student", Student.class);
           System.out.println(student);
       }
   ```

   > 创建调用对象成功，并成功注入依赖！测试结果：
   >
   > Student{id=10, name='Bernardo', school=School{name='清华大学', address='北京'}}

## xml与注解对比

### xml优点与缺点

#### 优点

+ 方便
+ 直观
+ 高效（代码少，没有配置文件的书写那么复杂）

#### 缺点

以硬编码的方式写入到 Java 代码中，修改是需要重新编译代码的

### 注解优点与缺点

#### 优点

+ 配置和代码是分离的 
+ 在 xml 中做修改，无需编译代码，只需重启服务器即可将新的配置加载

#### 缺点

编写麻烦，效率低，大型项目过于复杂

# AOP面向面编程

## 不使用AOP的开发方式

1. 创建Maven项目，工程目录如图所示

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210625155541.png)

2. 配置pom.xml文件

   ```xml
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-context</artifactId>
         <version>5.3.8</version>
       </dependency>
   ```

3. 定义接口UserService

   ```java
   package xyz.rtx3090.service;
   
   public interface UserService {
       void doSome();
       void doOther();
   }
   ```

4. 实现接口类UserServiceImpl

   ```java
   package xyz.rtx3090.service;
   
   import org.springframework.stereotype.Service;
   
   @Service("userService")
   public class UserServiceImpl implements UserService{
       @Override
       public void doSome() {
           printLog();
           System.out.println("UserServiceImpl所实现的doSome方法");
           addTrans();
       }
   
       @Override
       public void doOther() {
           printLog();
           System.out.println("UserServiceImpl所实现的doOther方法");
           addTrans();
       }
   
       public void printLog() {
           System.out.println("打印日志,非业务功能");
       }
   
       public void addTrans() {
           System.out.println("加入事务，非业务功能");
       }
   }
   ```

5. 编写applicationContext.xml配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
   
       <context:component-scan base-package="xyz.rtx3090.service"/>
   </beans>
   ```

6. 在测试类中进行测试

   ```java
   package xyz.rtx3090.service;
   
   import org.junit.Test;
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   
   public class MyTest {
       @Test
       public void test01() {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           UserServiceImpl userService = context.getBean("userService", UserServiceImpl.class);
           userService.doSome();
           userService.doOther();
       }
   }
   ```

   > 测试结果：
   >
   > 打印日志,非业务功能
   > UserServiceImpl所实现的doSome方法
   > 加入事务，非业务功能
   > 打印日志,非业务功能
   > UserServiceImpl所实现的doOther方法
   > 加入事务，非业务功能

我们可以发现真正与业务相关的方法只有`doSome()`方法和`doOther()`方法，而`printLog()`方法和`addTrans()`方法都是与业务无关的方法。这就照成了业务代码与非业务代码的混搅。交叉业务与主业务深度耦合在一起。当交叉业务逻辑 较多时，在主业务代码中会出现大量的交叉业务逻辑代码调用语句，大大影响了主业务逻辑 的可读性，降低了代码的可维护性，同时也增加了开发难度。 所以，可以采用动态代理方式。在不修改主业务逻辑的前提下，扩展和增强其功能。

## 采用动态代理的开发方式

### 动态代理实现方式

+ jdk动态代理，使用jdk中的Proxy，Method，InvocaitonHanderl创建代理对象。jdk动态代理要求目标类必须实现接口
+ cglib动态代理：第三方的工具库，创建代理对象，原理是继承。 通过继承目标类，创建子类。子类就是代理对象。 要求目标类不能是final的， 方法也不能是final的

### 动态代理作用

+ 在目标类源代码不改变的情况下，增加功能。
+ 减少代码的重复
+ 专注业务逻辑代码
+ 解耦合，让你的业务功能和日志，事务非业务功能分离。

### 代码实现

1. 创建Maven项目，工程目录如图所示

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210625163918.png)

2. 配置pom.xml文件

   ```xml
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-context</artifactId>
         <version>5.3.8</version>
       </dependency>
   ```

3. 定义接口UserService

   ```java
   package xyz.rtx3090.service;
   
   public interface UserService {
       void doSome();
       void doOther();
   }
   ```

4. 编写接口实现类UserServiceImpl

   ```java
   package xyz.rtx3090.service;
   
   import org.springframework.stereotype.Service;
   
   @Service("userService")
   public class UserServiceImpl implements UserService{
       @Override
       public void doSome() {
           System.out.println("UserServiceImpl实现类的doSome方法");
       }
   
       @Override
       public void doOther() {
           System.out.println("UserServiceImpl实现类的doOther方法");
       }
   }
   ```

5. 编写非业务方法工具类ServiceUtils

   ```java
   package xyz.rtx3090.service;
   
   public class ServiceUtils {
       public static void printLog() {
           System.out.println("非业务方法，打印日志");
       }
       public static void addTrans() {
           System.out.println("非业务方法，添加事务");
       }
   }
   ```

6. **编写动态代理类MyInvocationHandler**

   ```java
   package xyz.rtx3090.service;
   
   import java.lang.reflect.InvocationHandler;
   import java.lang.reflect.Method;
   
   public class MyInvocationHandler implements InvocationHandler {
       private Object target;
   
       public MyInvocationHandler() {
           super();
       }
   
       public MyInvocationHandler(Object target) {
           super();
           this.target = target;
       }
   
       @Override
       public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
           Object obj = null;
           //在方法之前输出日志
           ServiceUtils.printLog();
           //执行目标方法，执行target对象的方法
           obj = method.invoke(target, args);
           //在方法之后，执行添加事务方法
           ServiceUtils.addTrans();
           return obj;
       }
   }
   ```

7. 编写配置文件applicationContext.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
   
       <context:component-scan base-package="xyz.rtx3090.service"/>
   </beans>
   ```

8. 编写测试类进行测试

   ```java
   package xyz.rtx3090.service;
   
   import org.junit.Test;
   
   import java.lang.reflect.Proxy;
   
   public class MyTest {
       @Test
       public void test01() {
           //创建代理对象
           UserService userService = new UserServiceImpl();
           MyInvocationHandler myInvocationHandler = new MyInvocationHandler(userService);
           UserService proxy = (UserService) Proxy.newProxyInstance(userService.getClass().getClassLoader(), userService.getClass().getInterfaces(), myInvocationHandler);
           //通过带出对象执行业务方法，实现日志，事务的增强
           proxy.doSome();
           proxy.doOther();
       }
   }
   ```

   > 测试结果：
   >
   > 非业务方法，打印日志
   > UserServiceImpl实现类的doSome方法
   > 非业务方法，添加事务
   > 非业务方法，打印日志
   > UserServiceImpl实现类的doOther方法
   > 非业务方法，添加事务

采用动态代理的方式进行开发，可以有效的避免业务代码与非业务的混搅。但是由于实现的思想不只一种，所以不统一，对于项目交流不利。所以我们可以通过aop的方式来帮助我们更好的动态代理的开发业务。

## AOP概述

AOP面向切面编程， 基于动态代理的，可以使用jdk，cglib两种代理方式。Aop就是动态代理的规范化， 把动态代理的实现步骤，方式都定义好了， 让开发人员用一种统一的方式，使用动态代理。

AOP（Aspect Orient Programming），面向切面编程。面向切面编程是从动态角度考虑程序运行过程。 AOP 底层，就是采用动态代理模式实现的。采用了两种代理：JDK 的动态代理，与 CGLIB 的动态代理。

AOP可通过运行期动态代理实现程序功能的统一维护的一种技术。AOP 是 Spring 框架中的一个重要内容。利用 AOP 可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。

面向切面编程，就是将交叉业务逻辑封装成切面，利用 AOP 容器的功能将切面织入到 主业务逻辑中。所谓交叉业务逻辑是指，通用的、与主业务逻辑无关的代码，如安全检查、 事务、日志、缓存等。 若不使用 AOP，则会出现代码纠缠，即交叉业务逻辑与主业务逻辑混合在一起。这样， 会使主业务逻辑变的混杂不清。 例如，转账，在真正转账业务逻辑前后，需要权限控制、日志记录、加载事务、结束事务等交叉业务逻辑，而这些业务逻辑与主业务逻辑间并无直接关系。但，它们的代码量所占比重能达到总代码量的一半甚至还多。它们的存在，不仅产生了大量的“冗余”代码，还大 大干扰了主业务逻辑---转账。

+ **Aspect**：切面，给你的目标类增加的功能，就是切面。 像上面用的日志，事务都是切面。切面的特点： 一般都是非业务方法，独立使用的。
+ **Orient**：面向， 对着
+ **Programming** :编程

## AOP好处

1. 减少重复
2. 专注业务

> 注意：面向切面编程只是面向对象编程的一种补充。

## AOP术语

### 切面（Aspect）

**切面**泛指交叉业务逻辑。上例中的事务处理、日志处理就可以理解为切面。常用的切面 是通知（Advice）。实际就是对主业务逻辑的一种增强。

### 连接点（JoinPoint）

**连接点**指可以被切面织入的具体方法。通常业务接口中的方法均为连接点。

### 切入点（Pointcut）

**切入点**指声明的一个或多个连接点的集合。通过切入点指定一组方法。 被标记为 final 的方法是不能作为连接点与切入点的。因为最终的是不能被修改的，不能被增强的。

### 目标对象（Target）

**目标对象**指将要被增强的对象 。 即包含主业务逻辑的类的对象 。 上例中的StudentServiceImpl 的对象若被增强，则该类称为目标类，该类对象称为目标对象。当然， 不被增强，也就无所谓目标不目标了。

### 通知（Advice）

**通知**表示切面的执行时间，Advice 也叫增强。上例中的 MyInvocationHandler 就可以理 解为是一种通知。换个角度来说，通知定义了增强代码切入到目标代码的时间点，是目标方 法执行之前执行，还是之后执行等。通知类型不同，切入时间不同。 切入点定义切入的位置，通知定义切入的时间。

## AOP的实现——AspectJ

### AspectJ概述

对于 AOP 这种编程思想，很多框架都进行了实现。Spring 就是其中之一，可以完成面向 切面编程。然而，AspectJ 也实现了 AOP 的功能，且其实现方式更为简捷，使用更为方便， 而且还支持注解式开发。所以，Spring 又将 AspectJ 的对于 AOP 的实现也引入到了自己的框 架中。 在 Spring 中使用 AOP 开发时，一般使用 AspectJ 的实现方式

AspectJ 是一个优秀面向切面的框架，它扩展了 Java 语言，提供了强大的切面实现

### AspectJ的通知类型

**AspectJ 中常用的通知有五种类型**： 

1. 前置通知
2. 后置通知
3. 环绕通知 
4. 异常通知
5. 最终通知

### AspectJ的切入点表达式

AspectJ 定义了专门的表达式用于指定切入点。表达式的原型是：

```java
execution(modifiers-pattern? ret-type-pattern declaring-type-pattern?name-pattern(param-pattern) throws-pattern?)
```

+ modifiers-pattern——访问权限类型
+ ret-type-pattern——返回值类型
+ declaring-type-pattern——包名类名
+ name-pattern(param-pattern)——方法名(参数类型及个数)
+ throws-pattern——抛出异常类型
+ ? ——表示可选的部分

总结来说AspectJ切入点表达式为：

```java
execution(访问权限 方法返回值 方法声明(参数) 异常类型)
```

> 访问权限、异常类型都可省略

AspectJ表达式还是使用到一些符号，如下：

| 符号 |                             意义                             |
| :--: | :----------------------------------------------------------: |
|  *   |                       0至多个任意字符                        |
|  ..  | 用在包名后，表示当前包及其子包路径<br />用在方法参数中，表示任意多个参数 |
|  +   | 用在类名后，表示当前类及其子类<br />用在接口后，表示当前接口及其实现类 |

**举例**

1. `execution(public * *(..))`

   切入点为：任意公共方法

2. `execution(* set*(..))`

   切入点为：任意以“set”开头的方法

3. `execution(* xyz.rtx3090.service.*.*(..))`

   切入点为：xyz.rtx3090.service包内任意类的任意方法

4. `execution(* xyz.rtx3090.service..*.*(..))`

   切入点为：xyz.rtx3090.service包或子包内任意类的任意方法

5. `execution(* *..service.*.*(..))`

   切入点为：所有包下的service包的任意类的任意方法

6. `* xyz.rtx3090.service.IAccountService+.*(..)`

   切入点为：IAccountService若为接口，则表示为该接口中、及其实现类中的任意方法；IAccountService若为类，则表示为该类中、及其子类中的任意方法

7. `execution(* joke(String, int))`

   切入点为：所有名为joke的方法，且第一参数为String类型，第二个参数为int类型。【如果方法中的参数类型是 java.lang 包下的类，可以直接使用类名，否则必须使用 全限定类名，如 joke( java.util.List, int)】

8. `execution(* joke(String,*))`

   切入点为：所有名为joke的方法，且第一个参数为String类型，第二个参数任意类型。

   如：joke(String s1,String s2)和joke(String s1,double d2)都是，但joke(String s1,double d2,String  s3)不是。

9. `execution(* joke(String,..))`

   切入点为：所有名为joke的方法，且第一个参数类型为String，后面可以有任意个参数且类型不限。

   如： joke(String s1)、joke(String s1,String s2)和 joke(String s1,double d2,String s3) 都是。

10. `execution(* joke(Object))`

    切入点为：所有名为joke的方法，只拥有一个参数，且参数类型为Object类型。

    如：joke(Object ob) 是，但joke(String s)与 joke(User u)均不是。

11. `exectuion(* joke(Object+))`

    切入点为：所有名为joke的方法，只拥有一个参数，且参数类型为Object或Object子类。

    如：不仅 joke(Object ob)是，joke(String s)和 joke(User u)也是。

### AspectJ基本实现步骤演示

1. 创建Maven项目，工程结构如图所示：

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210626093242.png)

2. 配置pom.xml

   ```xml
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-context</artifactId>
         <version>5.3.8</version>
       </dependency>
   
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-aspects</artifactId>
         <version>5.3.7</version>
       </dependency>
   ```

3. 定义业务接口UserService

   ```java
   package xyz.rtx3090.service;
   
   public interface UserService {
       void doSome();
   }
   ```

4. 实现业务接口类UserServiceImpl

   ```java
   package xyz.rtx3090.service;
   
   import org.springframework.stereotype.Service;
   
   @Service("userService")
   public class UserServiceImpl implements UserService{
       @Override
       public void doSome() {
           System.out.println("执行了业务方法doSome()");
       }
   }
   ```

5. 定义切面类MyAspect

   ```java
   package xyz.rtx3090.service;
   
   import org.aspectj.lang.annotation.Aspect;
   import org.aspectj.lang.annotation.Before;
   import org.springframework.stereotype.Component;
   
   @Component("myAspect")
   @Aspect
   public class MyAspect {
       /**
        * @Before: 前置通知
        *      value——切入点表达式，表示切面执行的位置
        */
       @Before(value = "execution(* xyz.rtx3090.service.*.*(..))")
       public void myBefore() {
           //切面代码的功能，例如日志输出，事务处理
           System.out.println("前置通知：在目标方法之前执行");
       }
   }
   ```

6. 编写配置文件applicationContext.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">
   
       <!--扫描包中的注解声明-->
       <context:component-scan base-package="xyz.rtx3090.service"/>
   
       <!--声明自动代理生成器，创建代理-->
       <aop:aspectj-autoproxy/>
   
       <!--注册UserServiceImpl类到SpringIOC容器中-->
       <bean id="userService" class="xyz.rtx3090.service.UserServiceImpl"/>
   </beans>
   ```

7. 在测试类中进行测试

   ```java
   package xyz.rtx3090.service;
   
   import org.junit.Test;
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   
   public class MyTest {
       @Test
       public void test01(UserService userService1) {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           UserService userService1 = (UserService) context.getBean("userService");
           //查看生成的代理类，运行时类型为什么
           String name = userService1.getClass().getName();
           System.out.println("代理类运行时类型为：" + name);
           //通过代理执行业务方法，实现功能增强
           userService1.doSome();
       }
   }
   ```

   > 加入切面成功，测试结果：
   >
   > 代理类运行时类型为：com.sun.proxy.$Proxy14
   > 前置通知：在目标方法之前执行
   > 执行了业务方法doSome()

### @Before——前置通知

> 前面我们已经演示过AspectJ具体的代码实现步骤了，所有下面我只会展示关键的Aspect类的定义

#### 说明

+ @Before——前置通知
+ 作用：在目标方法执行之前，来执行我们需要执行的代码
+ 属性：
  + value——切入点表达式，表示切面执行的位置
+ 方法参数： 
  + JoinPoint——前置通知可以包含一个JoinPoint类型的参数，形参名可自定义，作用是可用来获取切入点表达式、方法签名、 目标对象等（不只@Before前置通知用这个方法参数，其他通知也都有，可写可不写）
+ 方法返回值：void无

#### 代码演示

1. **Aspect类的定义编写**

   ```java
   package xyz.rtx3090.service;
   
   import org.aspectj.lang.JoinPoint;
   import org.aspectj.lang.annotation.Aspect;
   import org.aspectj.lang.annotation.Before;
   import org.springframework.stereotype.Component;
   
   @Component("myAspect")
   @Aspect
   public class MyAspect {
       /**
        * @Before: 前置通知
        *      value——切入点表达式，表示切面执行的位置
        */
       @Before(value = "execution(* xyz.rtx3090.service.*.*(..))")
       public void myBefore(JoinPoint joinPoint) {
           //JoinPoint能够获取到方法的定义、参数等消息
           System.out.println("连接点的表达式定义：" + joinPoint.getSignature());
           System.out.println("连接点方法的参数个数：" + joinPoint.getArgs().length);
           //切面代码的功能，例如日志输出，事务处理
           System.out.println("前置通知：在目标方法之前执行");
       }
   }
   ```

2. 在测试类中进行测试

   ```java
       @Test
       public void testBefore() {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           UserService userService = context.getBean("userService", UserService.class);
           userService.doSome();
       }
   ```

   > **测试成功，测试结果：**
   >
   > 连接点的表达式定义：void xyz.rtx3090.service.UserService.doSome()
   > 前置通知：在目标方法之前执行
   > 执行了业务方法doSome()

### @AfterReturning——后置通知

#### 说明

+ @AfterReturning——后置通知
+ 作用：在目标方法执行之后，来执行我们需要执行的代码
+ 属性：
  + value——切入点表达式，表示切面执行的位置
  + returning——接受目标方法返回值【属性名要与下面的方法形参result名一致】
+ 方法参数：
  + JoinPoint——后置通知可以包含一个JoinPoint类型的参数，形参名可自定义，作用是可用来获取切入点表达式、方法签名、 目标对象等（不只@AfterReturning后置通知用这个方法参数，其他通知也都有，可写可不写）
  + result——用于接受目标方法返回值，形参名可自定义但类型建议为Object，因为目标方法的返回值可能是任何类型【形参名要与上面的属性名returning名一致】
+ 方法返回值：void无

#### 代码演示

1. **Aspect类的定义编写**

   ```java
   package xyz.rtx3090.service;
   
   import org.aspectj.lang.JoinPoint;
   import org.aspectj.lang.annotation.AfterReturning;
   import org.aspectj.lang.annotation.Aspect;
   import org.springframework.stereotype.Component;
   
   @Component("myAspect")
   @Aspect
   public class MyAspect {
       @AfterReturning(value = "execution(* doOther(String, int))", returning = "result")
       public void myAfterReturning(JoinPoint joinPoint, Object result) {
           //切面代码的功能，例如日志输出，事务处理
           System.out.println("后置通知：在目标方法之后执行");
           //JoinPoint能够获取到方法的定义、参数等消息
           System.out.println("连接点：" + joinPoint.getSourceLocation());
           //result用来接受目标方法的返回数值
           System.out.println("目标方法的返回值: " + result);
       }
   }
   ```

2. 在测试类中进行测试

   ```java
       @Test
       public void testAfterReturning() {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           UserService userService = context.getBean("userService", UserService.class);
           String s = userService.doOther("2", 3);
           System.out.println("doOther执行的返回结果："+s);
       }
   ```

   > **测试成功，测试结果：**
   >
   > 执行了业务方法doOther()
   > 后置通知：在目标方法之后执行
   > 连接点方法的参数个数：2
   > 目标方法的返回值: 23
   > doOther执行的返回结果：23

### @Around——环绕通知

#### 说明

+ @Around——环绕通知
+ 作用：在目标方法执行之前、之后，来执行我们需要执行的代码
+ 属性：
  + value——切入点表达式，表示切面执行的位置
+ 方法参数：
  + ProceedingJoinPoint ——环绕通知方法可以包含一个ProceedingJoinPoint类型的参数，接口 ProceedingJoinPoint 除了和其父接口JoinPoint相同的获取切入点表达式、方法签名、 目标对象等功能外，其还有一个 proceed()方法，用于执行目标方法。若目标方法有返回值，则该方法的返回值就是目标方法 的返回值，最后，环绕增强方法将其返回值返回【该增强方法实际是拦截了目标方法的执行】
+ 方法返回值：为Object类型，用于返回执行proceed()方法的返回值，即目标方法的执行结果的返回值

#### 代码演示

1. **Aspect类的定义编写**

   ```java
   package xyz.rtx3090.service;
   
   import org.aspectj.lang.ProceedingJoinPoint;
   import org.aspectj.lang.annotation.Around;
   import org.aspectj.lang.annotation.Aspect;
   import org.springframework.stereotype.Component;
   
   @Component("myAspect")
   @Aspect
   public class MyAspect {
       @Around(value = "execution(* love(String, String))")
       public Object myAround(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
           //ProceedingJoinPoint也能够获取到方法的定义、参数等消息
           Object[] args = proceedingJoinPoint.getArgs();
           System.out.println("-----遍历目标方法的参数列表-----");
           for (Object o :
                   args) {
               System.out.println(o);
           }
           //在目标方法之前执行的增强功能
           System.out.println("环绕通知：在目标方法之前执行的，例如输出日志");
           //执行目标方法的调用
           Object proceed = proceedingJoinPoint.proceed();
           //在目标方法之后执行的增强功能
           System.out.println("环绕通知：在目标方法之后执行的，例如处理事务");
           return proceed;
       }
   }
   ```

2. 在测试类中进行测试

   ```java
       @Test
       public void testAround() {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           UserService userService = context.getBean("userService", UserService.class);
           Boolean love = userService.love("root", "admin123456");
           System.out.println(love ? "登录成功" : "登录失败");
       }
   ```

   > **测试成功，测试结果：**
   >
   > -----遍历目标方法的参数列表-----
   > root
   > admin123456
   > 环绕通知：在目标方法之前执行的，例如输出日志
   > 执行业务方法love()
   > 环绕通知：在目标方法之后执行的，例如处理事务
   > 登录成功

### @Pointcut——定义切入点

#### 说明

+ @Pointcut——定义切入点
+ 作用：
  + 当较多的通知增强方法使用相同的 execution 切入点表达式时，编写、维护均较为麻烦。 AspectJ 提供了@Pointcut 注解，用于定义 execution 切入点表达式。（相当与给定义点起别名）
  + 其用法是，将@Pointcut 注解在一个方法之上，以后所有的 execution 的 value 属性值均 可使用该方法名作为切入点。代表的就是@Pointcut 定义的切入点。这个使用@Pointcut 注解 的方法一般使用 private 的标识方法，即没有实际作用的方法。
+ 属性：
  + value——切入点表达式，表示切面执行的位置
+ 方法参数：无
+ 方法返回值：void无

#### 代码演示

1. **Aspect类的定义编写**

   ```java
   package xyz.rtx3090.service;
   
   import org.aspectj.lang.ProceedingJoinPoint;
   import org.aspectj.lang.annotation.*;
   import org.springframework.stereotype.Component;
   
   @Component("myAspect")
   @Aspect
   public class MyAspect {
       @Pointcut(value = "execution(* xyz.rtx3090.service.UserServiceImpl.returnA(..))")
       private void myPointcut() {
           //不需要写代码
       }
   
       @Around(value = "myPointcut()")
       public Object myAround02(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
           Object obj = null;
           //在目标方法执行之前的增强功能
           System.out.println("环绕通知：在目标代码之前执行的增强功能，例如打印日志");
           //执行目标方法的调用
           obj = proceedingJoinPoint.proceed();
           //在目标方法执行之后的增强功能
           System.out.println("环绕通知：在目标代码之后执行的增强功能，例如处理事务");
           return obj;
       }
   }
   ```

2. 在测试类中进行测试

   ```java
       @Test
       public void testPointcut() {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           UserService userService = context.getBean("userService", UserService.class);
           char a = userService.returnA();
           System.out.println("返回结果：" + a);
       }
   ```

   > **测试成功，测试结果：**
   >
   > 环绕通知：在目标代码之前执行的增强功能，例如打印日志
   > 执行业务方法returnA
   > 环绕通知：在目标代码之后执行的增强功能，例如处理事务
   > 返回结果：A

### @After——最终通知（了解）

#### 说明

+ @After——最终通知
+ 作用：在目标方法执行后被执行，且无论抛出异常都会被执行
+ 属性：
  + value——切入点表达式，表示切面执行的位置
+ 方法参数：
  + JoinPoint——可用来获取切入点表达式、方法签名、 目标对象等（不只@After前置通知用这个方法参数，其他通知也都有，可写可不写）
+ 方法返回值：void无

#### 代码演示

1. **Aspect类的定义编写**

   ```java
   package xyz.rtx3090.service;
   
   import org.aspectj.lang.annotation.*;
   import org.springframework.stereotype.Component;
   
   @Component("myAspect")
   @Aspect
   public class MyAspect {
       @After(value = "execution(public void xyz..*.fk(..))")
       public void myAfter() {
           //切面代码的功能，例如事务处理
           System.out.println("最终通知：总是会被执行的方法");
       }
   }
   ```

2. 在测试类中进行测试

   ```java
       @Test
       public void testAfter() {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           UserService userService = context.getBean("userService", UserService.class);
           userService.fk();
       }
   ```

   > **测试成功，测试结果：**
   >
   > 执行业务方法fk()
   > 最终通知：总是会被执行的方法

### @AfterThrowing——异常通知（了解）

#### 说明

+ @AfterThrowing——异常通知
+ 作用：用于在目标方法抛出异常后执行
+ 属性：
  + value——切入点表达式，表示切面执行的位置
  + throwing——用于指定所发生的异常类对象【与下面方法参数throwing名一致】
+ 方法参数：
  + JoinPoint——可用来获取切入点表达式、方法签名、 目标对象等（不只@After前置通知用这个方法参数，其他通知也都有，可写可不写）
  + Throwable——用于指定所发生的异常类对象【与上面属性throwing值一致】
+ 方法返回值：void无

#### 代码演示

1. **Aspect类的定义编写**

   ```java
   package xyz.rtx3090.service;
   
   import org.aspectj.lang.annotation.*;
   import org.springframework.stereotype.Component;
   
   @Component("myAspect")
   @Aspect
   public class MyAspect {
       @AfterThrowing(value = "execution(* doSend(..))", throwing = "ex")
       public void myAfterThrowing(Throwable ex) {
           //切面代码的功能,当发生异常时执行
           System.out.println("异常通知：在目标方法抛出异常时执行；\n 异常原因：" + ex.getMessage());
       }
   }
   ```

2. 在测试类中进行测试

   ```java
       @Test
       public void testAfterThrowing() {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           UserService userService = context.getBean("userService", UserService.class);
           int i = userService.doSend(true);
           System.out.println(i == 1 ? "抢票成功" : "抢票失败");
       }
   ```

   > **测试成功，测试结果：**
   >
   > 异常通知：在目标方法抛出异常时执行；
   >  异常原因：/ by zero
   >
   > java.lang.ArithmeticException: / by zero

# Spring集成Mybatis

## 概述

将 MyBatis 与 Spring 进行整合，主要解决的问题就是将 SqlSessionFactory 对象交由 Spring 来管理。所以，该整合，只需要将 SqlSessionFactory 的对象生成器 SqlSessionFactoryBean 注 册在 Spring 容器中，再将其注入给 Dao 的实现类即可完成整合。 实现 Spring 与 MyBatis 的整合常用的方式：扫描的 Mapper 动态代理 Spring 像插线板一样，mybatis 框架是插头，可以容易的组合到一起。插线板 spring 插 上 mybatis，两个框架就是一个整体。

## 代码实现

1. 准备数据库数据

   ```mysql
   /*如果数据库mybatis存在，则删除它*/
   drop database if exists mybatis;
   
   /*创建名为mybatis的数据库*/
   create database mybatis;
   
   /*使用为名mybatis的数据库*/
   use mybatis;
   
   /*创建表user*/
   CREATE TABLE `user`
   (
       `id`   int(10) NOT NULL,
       `name` varchar(20) DEFAULT NULL,
       `pwd`  varchar(50) DEFAULT NULL,
       PRIMARY KEY (`id`)
   ) ENGINE = InnoDB
     DEFAULT CHARSET = utf8;
   
   /*向表user插入4条数据*/
   insert into mybatis.user (id, name, pwd)
   VALUES (1,'Jason','Crimson'),
          (2,'BernardoLi','Assault'),
          (3,'Mybatis','Shortcuts'),
          (4,'Spring','404NOTFOUND');
   ```

2. 新建Maven项目，工程结构目录如图所示

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210701093407.png)

3. 编写`pom.xml`文件，导入项目所需要的Jar包依赖

   ```xml
     <dependencies>
       <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>4.11</version>
         <scope>test</scope>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-context</artifactId>
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
       <dependency>
         <groupId>org.mybatis</groupId>
         <artifactId>mybatis</artifactId>
         <version>3.5.1</version>
       </dependency>
       <dependency>
         <groupId>org.mybatis</groupId>
         <artifactId>mybatis-spring</artifactId>
         <version>1.3.1</version>
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
       <resources>
         <resource>
           <directory>src/main/java</directory><!--所在的目录-->
           <includes><!--包括目录下的.properties,.xml 文件都会扫描到-->
             <include>**/*.properties</include>
             <include>**/*.xml</include>
           </includes>
           <filtering>false</filtering>
         </resource>
       </resources>
       <plugins>
         <plugin>
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

4. 编写实体类`User`

   ```java
   package xyz.rtx3090.pojo;
   
   public class User {
       private int id;
       private String name;
       private String pwd;
   
       //constructor
       //setter and getter
   
   }
   ```

5. 编写Dao接口`UserDao`

   ```java
   package xyz.rtx3090.dao;
   
   import xyz.rtx3090.pojo.User;
   
   import java.util.List;
   
   public interface UserDao {
       //查询全部用户信息
       List<User> selectAllUser();
   }
   ```

6. 编写Dao接口的对应的配置文件`UserDao.xml`

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="xyz.rtx3090.dao.UserDao">
       <!--查询全部用户信息 -->
       <select id="selectAllUser" resultType="xyz.rtx3090.pojo.User">
           select * from mybatis.user;
       </select>
   </mapper>
   ```

7. 编写service接口`UserService`

   ```java
   package xyz.rtx3090.service;
   
   import xyz.rtx3090.pojo.User;
   
   import java.util.List;
   
   public interface UserService {
       //查询全部用户信息
       List<User> selectAllUser();
   }
   ```

8. 编写service接口的实现类`UserServiceImpl`（通过Service接口来调用Dao接口）

   ```java
   package xyz.rtx3090.service.impl;
   
   import xyz.rtx3090.dao.UserDao;
   import xyz.rtx3090.pojo.User;
   import xyz.rtx3090.service.UserService;
   
   import java.util.List;
   
   public class UserServiceImpl implements UserService {
     	//引用Dao接口
       private UserDao userDao;
   
       public void setUserDao(UserDao userDao) {
           this.userDao = userDao;
       }
   
       @Override
       public List<User> selectAllUser() {
           return userDao.selectAllUser();
       }
   }
   ```

9. 编写Mybatis核心配置文件`mybatisConfig.xml`

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
       <!--设置-->
       <settings>
           <setting name="logImpl" value="STDOUT_LOGGING"/>
       </settings>
       <!--类型别名-->
       <typeAliases>
           <package name="xyz.rtx3090.pojo"/>
       </typeAliases>
       <!--映射文件-->
       <mappers>
           <package name="xyz.rtx3090.dao"/>
       </mappers>
   </configuration>
   ```

10. 编写数据库配置文件

    ```properties
    driver=com.mysql.jdbc.Driver
    url=jdbc:mysql://localhost:3306/mybatis?useSSL=false&useUnicode=true&characterEncoding=UTF-8
    username=root
    password=intmain()
    max=20
    ```

11. 编写Spring核心配置文件`application.xml`

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    
        <!--读取数据库配置文件信息-->
        <context:property-placeholder location="classpath:jdbc.properties"/>
    
        <!--声明数据源，作用是链接数据库-->
        <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
              init-method="init" destroy-method="close">
            <property name="url" value="${url}"/>
            <property name="username" value="${username}"/>
            <property name="password" value="${password}"/>
            <property name="maxActive" value="${max}"/>
        </bean>
    
        <!--配置SqlSessionFactory-->
        <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
            <!--关联数据源-->
            <property name="dataSource" ref="dataSource"/>
            <!--关联mybatis核心配置文件-->
            <property name="configLocation" value="classpath:mybatisConfig.xml"/>
        </bean>
    
        <!--注册Mapper扫描配置器-->
        <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
            <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
            <!--指定基本扫描包，即Dao接口包-->
            <property name="basePackage" value="xyz.rtx3090.dao"/>
        </bean>
    
            <!--注册实现类UserServiceImpl到springIOC容器中-->
        <bean id="userServiceImpl" class="xyz.rtx3090.service.impl.UserServiceImpl">
            <property name="userDao" ref="userDao"/>
        </bean>
    </beans>
    ```

12. 编写测试类进行测试

    ```java
    package xyz.rtx3090.service;
    
    import org.junit.Test;
    import org.springframework.context.ApplicationContext;
    import org.springframework.context.support.ClassPathXmlApplicationContext;
    import xyz.rtx3090.dao.UserDao;
    import xyz.rtx3090.pojo.User;
    
    import java.util.List;
    
    public class UserServiceImplTest {
        @Test
        public void testSelectAllUser() {
            ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
    				UserService userServiceImpl = context.getBean("userServiceImpl", UserService.class);
            List<User> userList = userServiceImpl.selectAllUser();
            for (User user :
                    userList) {
                System.out.println(user);
            }
        }
    }
    ```

    > 测试成功，输出结果：
    >
    > User{id=1, name='Jason', pwd='Crimson'}
    > User{id=2, name='BernardoLi', pwd='Assault'}
    > User{id=3, name='Mybatis', pwd='Shortcuts'}
    > User{id=4, name='Spring', pwd='404NOTFOUND'}

# Spring事务

## 概述

事务原本是数据库中的概念在 Dao 层，但一般情况下需要将事务提升到业务层， 即 Service 层。这样做是为了能够使用事务的特性来管理具体的业务。

在 Spring 中通常可以通过以下两种方式来实现对事务的管理：

1. 使用 Spring 的事务注解管理事务
2. 使用 AspectJ 的 AOP 配置管理事务

## Spring事务管理API

Spring 的事务管理，主要用到两个事务相关的接口。

### 事务管理器接口

事务管理器是 PlatformTransactionManager 接口对象。其主要用于完成事务的提交、回 滚，及获取事务的状态信息。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210702081334.png)

#### 常用的两个实现类

PlatformTransactionManager 接口有两个常用的实现类：

1. `DataSourceTransactionManager`：使用 JDBC 或 MyBatis 进行数据库操作时使用。
2. `HibernateTransactionManager`：使用 Hibernate 进行持久化数据时使用。

#### Spring的回滚方式

Spring事务的默认回滚方式是：**发生运行时异常和 error 时回滚，发生受查(编译)异常时提交。**不过对于受查异常程序员也可以手工设置其回滚方式

#### 回顾错误与异常

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210702081737.png)

Throwable 类是 Java 语言中所有错误或异常的超类。只有当对象是此类(或其子类之一) 的实例时，才能通过 Java 虚拟机或者 Java 的 throw 语句抛出。

Error 是程序在运行过程中出现的无法处理的错误，比如 OutOfMemoryError、 ThreadDeath、NoSuchMethodError 等。当这些错误发生时，程序是无法处理（捕获或抛出） 的，JVM 一般会终止线程。

程序在编译和运行时出现的另一类错误称之为异常，它是 JVM 通知程序员的一种方式。 通过这种方式，让程序员知道已经或可能出现错误，要求程序员对其进行处理。

异常分为运行时异常与**受查异常**。

**运行时异常**是 RuntimeException 类或其子类，即只有在运行时才出现的异常。如， NullPointerException、ArrayIndexOutOfBoundsException、IllegalArgumentException 等均属于运行时异常。这些异常由 JVM 抛出，在编译时不要求必须处理（捕获或抛出）。但只要代码编写足够仔细，程序足够健壮，运行时异常是可以避免的。

**受查异常**也叫编译时异常，即在代码编写时要求必须捕获或抛出的异常，若不处理， 则无法通过编译。如 SQLException，ClassNotFoundException，IOException 等都属于受查异常。

RuntimeException 及其子类以外的异常，均属于受查异常。当然，用户自定义的 Exception 的子类，即用户自定义的异常也属受查异常。程序员在定义异常时，只要未明确声明定义的 为 RuntimeException 的子类，那么定义的就是受查异常。

### 事务定义接口

事务定义接口 TransactionDefinition 中定义了事务描述相关的三类常量：事务隔离级别、 事务传播行为、事务默认超时时限，及对它们的操作。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210702082503.png)

#### 定义了五个事务隔离级别常量

> 这些常量均是以 ISOLATION_开头。即形如 ISOLATION_XXX。

1. `DEFAULT`：采用 DB 默认的事务隔离级别，MySql 的默认为 REPEATABLE_READ； Oracle 默认为 READ_COMMITTED
2. `READ_UNCOMMITTED`：**读未提交**，未解决任何并发问题
3. `READ_COMMITTED：`：**读已提交**，解决脏读，存在不可重复读与幻读
4. `REPEATABLE_READ`：**可重复读**，解决脏读、不可重复读，存在幻读
5. `SERIALIZABLE`：串行化。不存在并发问题。

#### 定义了七个事务传播行为常量

所谓事务传播行为是指，处于不同事务中的方法在相互调用时，执行期间事务的维护情况。如A 事务中的方法 doSome()调用 B 事务中的方法 doOther()，在调用执行期间事务的维护情况，就称为事务传播行为。事务传播行为是加在方法上的。

> 事务传播行为常量都是以 PROPAGATION_ 开头，形如 PROPAGATION_XXX。

1. `PROPAGATION_REQUIRED`【掌握】

   指定的方法必须在事务内执行。若当前存在事务，就加入到当前事务中；若当前没有事务，则创建一个新事务。这种传播行为是最常见的选择，也是 Spring 默认的事务传播行为。

   如该传播行为加在 doOther()方法上。若 doSome()方法在调用 doOther()方法时就是在事务内运行的，则 doOther()方法的执行也加入到该事务内执行。若 doSome()方法在调用 doOther()方法时没有在事务内执行，则 doOther()方法会创建一个事务，并在其中执行。

2. `PROPAGATION_REQUIRES_NEW`【掌握】

   指定的方法支持当前事务，但若当前没有事务，也可以以非事务方式执行。

3. `PROPAGATION_SUPPORTS`【掌握】

   总是新建一个事务，若当前存在事务，就将当前事务挂起，直到新事务执行完毕。

4. `PROPAGATION_MANDATORY`

5. `PROPAGATION_NESTED`

6. `PROPAGATION_NEVER`

7. `PROPAGATION_NOT_SUPPORTED`

#### 定义了默认事务超时时限

常量 TIMEOUT_DEFAULT 定义了事务底层默认的超时时限，sql 语句的执行时长。

注意，事务的超时时限起作用的条件比较多，且超时的时间计算点较复杂。所以该值一般就使用默认值即可。

## 事务管理

事务管理的方式分为两种：**Spring事务注解管理**和**AspectJ的AOP配置管理事务** 

### Spring事务注解管理

**通过@Transactional 注解方式，可将事务织入到相应 public 方法中，实现事务管理**

@Transactional 的所有可选属性如下所示：

+ `propagation`：用于设置事务传播属性。该属性类型为 Propagation 枚举，默认值为`Propagation.REQUIRED`
+ `isolation`：用于设置事务的隔离级别。该属性类型为 Isolation 枚举，默认值为`Isolation.DEFAULT`
+ `readOnly`：用于设置该方法对数据库的操作是否是只读的。该属性为 boolean，默认值 为 false
+ `timeout`：用于设置本操作与数据库连接的超时时限。单位为秒，类型为 int，默认值为 -1，即没有时限
+ `rollbackFor`：指定需要回滚的异常类。类型为`Class[]`，默认值为空数组。当然，若只有 一个异常类时，可以不使用数组
+ `rollbackForClassName`：指定需要回滚的异常类类名。类型为`String[]`，默认值为空数组。 当然，若只有一个异常类时，可以不使用数组
+ `noRollbackFor`：指定不需要回滚的异常类。类型为`Class[]`，默认值为空数组。当然，若只有一个异常类时，可以不使用数组
+ ` noRollbackForClassName`：指定不需要回滚的异常类类名。类型为`String[]`，默认值为空数组。当然，若只有一个异常类时，可以不使用数组

需要注意的是，**@Transactional 若用在方法上，只能用于 public 方法上**。对于其他非 public 方法，如果加上了注解@Transactional，虽然 Spring 不会报错，但不会将指定事务织入到该 方法中。因为 Spring 会忽略掉所有非 public 方法上的@Transaction 注解。

**若@Transaction 注解在类上，则表示该类上所有的方法均将在执行时织入事务**

### AspectJ的AOP配置管理事务 

使用 XML 配置事务代理的方式的不足是，每个目标类都需要配置事务代理。当目标类较多，配置文件会变得非常臃肿

使用 XML 配置顾问方式可以自动为每个符合切入点表达式的类生成事务代理。其用法 很简单，只需将前面代码中关于事务代理的配置删除，再替换为如下内容即可

## 事务演示

本例要实现购买商品，模拟用户下订单，向订单表添加销售记录，从商品表减少库存。

### 开启事务前

1. 准备数据库数据

   ```mysql
   /*如果存在数据库spring，则进行删除*/
   drop database if exists spring;
   
   /*创建名为spring的数据库*/
   create database spring;
   
   /*使用名为spring的数据库*/
   use spring;
   
   /*创建表sale*/
   create table `sale`
   (
       `id`   int(11) auto_increment,
       `gid`  int(11) not null,
       `nums` int(11),
       primary key (`id`)
   ) ENGINE = INNODB
     default charset = utf8;
   
   /*创建表goods*/
   create table `goods`
   (
       `id`     int(11) auto_increment,
       `name`   varchar(100) not null,
       `amount` int(11)      not null,
       `price`  float        not null,
       primary key (`id`)
   ) ENGINE = INNODB
     default charset = utf8;
   
   /*向表goods中插入数据*/
   insert into spring.goods (id, name, amount, price)
   VALUES ('1001' , '笔记本' , '10' , '7999'),
          ('1002' , '手机' , '20' , '5999');
   ```

2. 创建maven项目，工程目录结构如图所示

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210702102351.png)

3. 配置`pom.xml`文件

   ```xml
   <dependencies>
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.11</version>
               <scope>test</scope>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-context</artifactId>
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
           <dependency>
               <groupId>org.mybatis</groupId>
               <artifactId>mybatis</artifactId>
               <version>3.5.1</version>
           </dependency>
           <dependency>
               <groupId>org.mybatis</groupId>
               <artifactId>mybatis-spring</artifactId>
               <version>1.3.1</version>
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

4. 编写实体类`Sale`、`Goods`

   ```java
   package xyz.rtx3090.pojo;
   
   public class Sale {
       private Integer id;
       private Integer gid;
       private Integer nums;
   
       //constructor
       //setter and getter
   		//toString
   }
   ```

   ```java
   package xyz.rtx3090.pojo;
   
   public class Goods {
       private Integer id;
       private String name;
       private Integer amount;
       private float price;
   
       //constructor
       //setter and getter
     	//toString
   }
   ```

5. 编写Dao接口`SaleDao`、`GoodsDao`

   ```java
   package xyz.rtx3090.dao;
   
   import xyz.rtx3090.pojo.Sale;
   
   public interface SaleDao {
       //插入指定销售信息
       int insertSale(Sale sale);
   }
   ```

   ```java
   package xyz.rtx3090.dao;
   
   import xyz.rtx3090.pojo.Goods;
   
   public interface GoodsDao {
       //根据商品id号查询指定商品信息
       Goods selectGoods(Integer goodsId);
   
       //更新指定商品信息
       int updateGoods(Goods goods);
   }
   ```

6. 编写Dao接口对应的xml配置文件`SaleDao.xml`、`GoodsDao.xml`

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="xyz.rtx3090.dao.SaleDao">
       <!--插入指定销售信息-->
       <insert id="insertSale" parameterType="sale">
           insert into sale (gid, nums) VALUES (#{gid},#{nums});
       </insert>
   </mapper>
   ```

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="xyz.rtx3090.dao.GoodsDao">
       <!--根据商品id号查询指定商品信息-->
       <select id="selectGoods" resultType="goods">
           select *
           from spring.goods
           where id = #{id};
       </select>
   
       <!--更新指定商品信息-->
       <update id="updateGoods" parameterType="goods">
           update spring.goods
           set amount = amount - #{amount}
           where id = #{id};
       </update>
   </mapper>
   ```

7. 编写自定义异常类`NotEnoughException`，以便service调用异常时使用

   ```java
   package xyz.rtx3090.exception;
   
   public class NotEnoughException extends RuntimeException{
       //constructor
       public NotEnoughException() {
           super();
       }
       public NotEnoughException(String msg) {
           super(msg);
       }
   }
   ```

8. 编写service接口`BuyGoodsService`，及其实现类`BuyGoodsServiceImpl`

   ```java
   package xyz.rtx3090.service;
   
   public interface BuyGoodsService {
       void buy(Integer gid, Integer amount);
   }
   ```

   ```java
   package xyz.rtx3090.service.impl;
   
   import xyz.rtx3090.dao.GoodsDao;
   import xyz.rtx3090.dao.SaleDao;
   import xyz.rtx3090.exception.NotEnoughException;
   import xyz.rtx3090.pojo.Goods;
   import xyz.rtx3090.pojo.Sale;
   import xyz.rtx3090.service.BuyGoodsService;
   
   public class BuyGoodsServiceImpl implements BuyGoodsService {
       private GoodsDao goodsDao;
       private SaleDao saleDao;
   
       //setter and getter
       public GoodsDao getGoodsDao() {
           return goodsDao;
       }
   
       public void setGoodsDao(GoodsDao goodsDao) {
           this.goodsDao = goodsDao;
       }
   
       public SaleDao getSaleDao() {
           return saleDao;
       }
   
       public void setSaleDao(SaleDao saleDao) {
           this.saleDao = saleDao;
       }
   
       @Override
       public void buy(Integer gid, Integer amount) {
           //更新销售表
           Sale sale = new Sale();
           sale.setGid(gid);
           sale.setNums(amount);
           saleDao.insertSale(sale);
           //更新商品表
           Goods goods = goodsDao.selectGoods(gid);
           if (goods == null) {
               throw new NullPointerException("无此商品");
           } else if (goods.getAmount() < amount) {
               throw new NotEnoughException("库存不足");
           } else {
               goods = new Goods();
               goods.setId(gid);
               goods.setAmount(amount);
               goodsDao.updateGoods(goods);
           }
       }
   }
   ```

9. 编写mybatis核心配置文件`mybatisConfig.xml`

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
       <!--设置-->
       <settings>
           <setting name="logImpl" value="STDOUT_LOGGING"/>
       </settings>
       <!--类型别名-->
       <typeAliases>
           <package name="xyz.rtx3090.pojo"/>
       </typeAliases>
       <!--映射路径-->
       <mappers>
           <package name="xyz.rtx3090.dao"/>
       </mappers>
   </configuration>
   ```

10. 编写数据库配置文件`jdbc.properties`

    ```properties
    driver=com.mysql.jdbc.Driver
    url=jdbc:mysql://localhost:3306/spring?useSSL=false&useUnicode=true&characterEncoding=UTF-8
    username=root
    password=intmain()
    max=20
    ```

11. 编写spring核心配置文件`applicationContext.xml`

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    
        <!--读取数据库配置文件信息-->
         <context:property-placeholder location="classpath:jdbc.properties"/>
    
        <!--声明数据源DataSource，使用Druid数据库链接池 -->
        <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
              init-method="init" destroy-method="close">
            <property name="url" value="${url}"/>
            <property name="username" value="${username}"/>
            <property name="password" value="${password}"/>
            <property name="maxActive" value="${max}"/>
        </bean>
    
        <!--配置SqlSessionFactory-->
        <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
            <!--关联数据源-->
            <property name="dataSource" ref="dataSource"/>
            <!--关联Mybatis核心配置文件-->
            <property name="configLocation" value="classpath:mybatisConfig.xml"/>
        </bean>
    
        <!--注册Mapper扫描配置器-->
        <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
            <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
            <!--指定基本扫描包，即Dao接口包-->
            <property name="basePackage" value="xyz.rtx3090.dao"/>
        </bean>
    
        <!--导入beans.xml配置文件-->
        <import resource="classpath:beans.xml"/>
    </beans>
    ```

12. 编写另一个spring核心配置文件`beans.xml`（用于注册我们自定义类到springIOC容器）

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd">
    
        <!--注册BuyGoodsServiceImpl实现类到SpringIOC容器中-->
        <bean id="buyServiceImpl" class="xyz.rtx3090.service.impl.BuyGoodsServiceImpl">
            <!--通过set方法将spring扫描的dao接口注入-->
            <property name="saleDao" ref="saleDao"/>
            <property name="goodsDao" ref="goodsDao"/>
        </bean>
    </beans>
    ```

13. 编写测试类`BuyGoodsServiceImplTest`进行测试

    ```java
    package xyz.rtx3090.service.impl;
    
    import org.junit.Test;
    import org.springframework.context.ApplicationContext;
    import org.springframework.context.support.ClassPathXmlApplicationContext;
    import xyz.rtx3090.service.BuyGoodsService;
    
    public class BuyGoodsServiceImplTest {
        @Test
        public void testBuy() {
            ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
            BuyGoodsService buyServiceImpl = context.getBean("buyServiceImpl", BuyGoodsService.class);
            buyServiceImpl.buy(1003,10);
        }
    }
    ```

    > 在测试中我们发现：因为‘商品不存在’、‘商品不足’等原因购买失败，`goods`表数据未能被更新，但`sale`表的数据却被更新。这就存在了事务问题！

### 加入Spring事务注解管理

> 说明：以下代码演示均在上面演示的代码【开启事务前】的基础进行改进的

1. 在spring核心配置文件`application.xml`，增加 `事务管理器`、`注解驱动`相关配置（此文件无需作出其他任何修改）

   ```xml
       <!--声明事务管理器-->
       <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
           <property name="dataSource" ref="dataSource"/>
       </bean>
   
       <!--开启注解驱动,关联事务管理器(注意导入约束头时，不要选择错误了，要选择:schema/tx/这个)-->
       <tx:annotation-driven transaction-manager="transactionManager"/>
   ```

2. 在业务层service实现类`BuyGoodsServiceImpl`的public方法上加入事务注解

   ```java
   		//后面的属性值也都可以不加，直接采用默认值即可
   		@Transactional(propagation = Propagation.REQUIRED, rollbackFor = {NotEnoughException.class, NullPointerException.class})
       @Override
       public void buy(Integer gid, Integer amount) {
           //更新销售表
           Sale sale = new Sale();
           sale.setGid(gid);
           sale.setNums(amount);
           saleDao.insertSale(sale);
           //更新商品表
           Goods goods = goodsDao.selectGoods(gid);
           if (goods == null) {
               throw new NullPointerException("无此商品");
           } else if (goods.getAmount() < amount) {
               throw new NotEnoughException("库存不足");
           } else {
               goods = new Goods();
               goods.setId(gid);
               goods.setAmount(amount);
               goodsDao.updateGoods(goods);
           }
       }
   ```

3. 再次在测试类中进行测试

   ```java
   package xyz.rtx3090.service.impl;
   
   import org.junit.Test;
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   import xyz.rtx3090.service.BuyGoodsService;
   
   public class BuyGoodsServiceImplTest {
       @Test
       public void testBuy() {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           BuyGoodsService buyServiceImpl = context.getBean("buyServiceImpl", BuyGoodsService.class);
           buyServiceImpl.buy(1003,10);
       }
   }
   ```

   > 这次我们发现`商品表`更新失败，`销售表`也没有新添加数据。事务问题得到解决了。

### 加入AspectJ的AOP配置管理事务

> 说明：以下代码演示均在上面演示的代码【开启事务前】的基础进行改进的

除了上面这种方式，我们还可以采用这种方法来管理事务。

1. 新加入`AspectJ`的依赖坐标

   ```xml
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-aspects</artifactId>
               <version>5.2.5.RELEASE</version>
           </dependency>
   ```

2. 在spring核心配置文件和加入`事务管理器`、`事务通知（切面）`、`aop配置，切入点`的配置

   ```xml
       <!--声明事务管理器-->
       <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
           <property name="dataSource" ref="dataSource"/>
       </bean>
   
       <!--事务通知（切面）-->
       <tx:advice id="buyAdvice" transaction-manager="transactionManager">
           <tx:attributes>
               <tx:method name="buy" propagation="REQUIRED" isolation="DEFAULT" rollback-for="java.lang.NullPointerException,xyz.rtx3090.exception.NotEnoughException"/>
               <tx:method name="add*" propagation="REQUIRED" isolation="DEFAULT" rollback-for="java.lang.Exception"/>
               <tx:method name="*" propagation="SUPPORTS"/>
           </tx:attributes>
       </tx:advice>
   
       <!--aop配置：通知应用的切入点-->
       <aop:config>
           <aop:pointcut id="servicePt" expression="execution(* *..service..*.*(..))"/>
           <aop:advisor advice-ref="buyAdvice" pointcut-ref="servicePt"/>
       </aop:config>
   ```

3. 再次在测试类中进行测试

   ```java
   package xyz.rtx3090.service.impl;
   
   import org.junit.Test;
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   import xyz.rtx3090.service.BuyGoodsService;
   
   public class BuyGoodsServiceImplTest {
       @Test
       public void testBuy() {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           BuyGoodsService buyServiceImpl = context.getBean("buyServiceImpl", BuyGoodsService.class);
           buyServiceImpl.buy(1003,10);
       }
   }
   ```

   > 这次我们发现`商品表`更新失败，`销售表`也没有新添加数据。事务问题得到解决了。

# Spring与Web

在 Web 项目中使用 Spring 框架，首先要解决在 web 层（这里指 Servlet）中获取到 Spring 容器的问题。只要在 web 层获取到了 Spring 容器，便可从容器中获取到 Service 对象。

## SpringWeb项目创建

### 详细搭建步骤

1. 创建带`maven-archetype-webapp`模版的maven项目，工程目录如图所示

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210702161952.png)

2. 配置`pom.xml`文件

   ```xml
   <dependencies>
       <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>4.11</version>
         <scope>test</scope>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-context</artifactId>
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
       <dependency>
         <groupId>org.mybatis</groupId>
         <artifactId>mybatis</artifactId>
         <version>3.5.1</version>
       </dependency>
       <dependency>
         <groupId>org.mybatis</groupId>
         <artifactId>mybatis-spring</artifactId>
         <version>1.3.1</version>
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
       <!-- servlet依赖 -->
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
     </dependencies>
   
     <build>
       <resources>
         <resource>
           <directory>src/main/java</directory><!--所在的目录-->
           <includes><!--包括目录下的.properties,.xml 文件都会扫描到-->
             <include>**/*.properties</include>
             <include>**/*.xml</include>
           </includes>
           <filtering>false</filtering>
         </resource>
       </resources>
       <plugins>
         <plugin>
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

3. 编写实体类`User`

   ```java
   package xyz.rtx3090.pojo;
   
   public class User {
       private int id;
       private String name;
       private String pwd;
   
       //constructor
       //setter and getter
     	//toString
   }
   ```

4. 编写Dao接口`UserDao`

   ```java
   package xyz.rtx3090.dao;
   
   import xyz.rtx3090.pojo.User;
   
   public interface UserDao {
       //注册并添加用户信息
       int insertUser(User user);
   }
   ```

5. 编写Dao接口对应的xml配置文件`UserDao.xml`

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="xyz.rtx3090.dao.UserDao">
       <!--注册并添加用户信息-->
       <insert id="insertUser" parameterType="user">
           insert into mybatis.user (id, name, pwd)
           VALUES (#{id}, #{name}, #{pwd});
       </insert>
   </mapper>
   ```

6. 编写service接口`UserService`

   ```java
   package xyz.rtx3090.service;
   
   import xyz.rtx3090.pojo.User;
   
   public interface UserService {
       //注册并添加用户信息
       int insertUser(User user);
   }
   ```

7. 编写service接口实现类`UserServiceImpl`

   ```java
   package xyz.rtx3090.service.impl;
   
   import xyz.rtx3090.dao.UserDao;
   import xyz.rtx3090.pojo.User;
   import xyz.rtx3090.service.UserService;
   
   public class UserServiceImpl implements UserService {
       private UserDao userDao;
   
       public void setUserDao(UserDao userDao) {
           this.userDao = userDao;
       }
   
       @Override
       public int insertUser(User user) {
           return userDao.insertUser(user);
       }
   }
   ```

8. 编写controller类`RegisterServlet`

   ```java
   package xyz.rtx3090.controller;
   
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   import xyz.rtx3090.pojo.User;
   import xyz.rtx3090.service.UserService;
   
   import javax.servlet.ServletException;
   import javax.servlet.http.HttpServlet;
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   import java.io.IOException;
   
   public class RegisterServlet extends HttpServlet {
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           //接受请求参数
           String name = req.getParameter("name");
           String pwd = req.getParameter("pwd");
   
           //创建spring容器，获取Service对象
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           System.out.println("容器的信息: " + context);
           UserService userServiceImpl = context.getBean("userServiceImpl", UserService.class);
   
           User user = new User();
           user.setName(name);
           user.setPwd(pwd);
   
           //调用service方法
           userServiceImpl.insertUser(user);
   
           //显示处理结果的jsp
           req.getRequestDispatcher("/result.jsp").forward(req,resp);
       }
   
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doGet(req,resp);
       }
   }
   ```

9. 编写mybatis核心配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
       <!--设置-->
       <settings>
           <setting name="logImpl" value="STDOUT_LOGGING"/>
       </settings>
       <!--类型别名-->
       <typeAliases>
           <package name="xyz.rtx3090.pojo"/>
       </typeAliases>
       <!--映射文件-->
       <mappers>
           <package name="xyz.rtx3090.dao"/>
       </mappers>
   </configuration>
   ```

10. 数据库配置文件`jdbc.properites`

    ```properties
    driver=com.mysql.jdbc.Driver
    url=jdbc:mysql://localhost:3306/mybatis?useSSL=false&useUnicode=true&characterEncoding=UTF-8
    username=root
    password=intmain()
    max=20
    ```

11. 编写spring核心配置文件

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    
        <!--读取数据库配置文件信息-->
        <context:property-placeholder location="classpath:jdbc.properties"/>
    
        <!--声明数据源，作用是链接数据库-->
        <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
              init-method="init" destroy-method="close">
            <property name="url" value="${url}"/>
            <property name="username" value="${username}"/>
            <property name="password" value="${password}"/>
            <property name="maxActive" value="${max}"/>
        </bean>
    
        <!--配置SqlSessionFactory-->
        <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
            <!--关联数据源-->
            <property name="dataSource" ref="dataSource"/>
            <!--关联mybatis核心配置文件-->
            <property name="configLocation" value="classpath:mybatisConfig.xml"/>
        </bean>
    
        <!--注册Mapper扫描配置器-->
        <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
            <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
            <!--指定基本扫描包，即Dao接口包-->
            <property name="basePackage" value="xyz.rtx3090.dao"/>
        </bean>
    
        <!--导入beans.xml文件-->
        <import resource="classpath:beans.xml"/>
    </beans>
    ```

12. 编写另一个spring核心配置文件 `beans.xml`（用于注册自定义类到springIOC容器中）

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd">
    
        <!--注册实现类UserServiceImpl到springIOC容器中-->
        <bean id="userServiceImpl" class="xyz.rtx3090.service.impl.UserServiceImpl">
            <property name="userDao" ref="userDao"/>
        </bean>
    </beans>
    ```

13. 配置Tomcat服务器

    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210702162717.png)

14. 启动tomcat，在页面输入注册信息，若跳转到登录成功的页面，则表明工程成功

### 运行结果分析

当表单提交，跳转到 result.jsp 后，多刷新几次页面，查看后台输出，发现每刷新一次 页面，就 new 出一个新的 Spring 容器。即，每提交一次请求，就会创建一个新的 Spring 容 器。对于一个应用来说，只需要一个 Spring 容器即可。所以，将 Spring 容器的创建语句放 在 Servlet 的 doGet()或 doPost()方法中是有问题的。

此时，可以考虑，将 Spring 容器的创建放在 Servlet 进行初始化时进行，即执行 init()方 法时执行。并且，Servlet 还是单例多线程的，即一个业务只有一个 Servlet 实例，所有执行 该业务的用户执行的都是这一个 Servlet 实例。这样，Spring 容器就具有了唯一性了。 但是，Servlet 是一个业务一个 Servlet 实例，即 LoginServlet 只有一个，但还会有 StudentServlet、TeacherServlet 等。每个业务都会有一个 Servlet，都会执行自己的 init()方法， 也就都会创建一个 Spring 容器了。这样一来，Spring 容器就又不唯一了。

**为了上面的问题，我们就需要用到【监听器ContextLoaderListener】**

## Spring 的监听器 ContextLoaderListener

对于 Web 应用来说，ServletContext 对象是唯一的，一个 Web 应用，只有一个 ServletContext 对象，该对象是在 Web 应用装载时初始化的。若将 Spring 容器的创建时机， 放在 ServletContext 初始化时，就可以保证 Spring 容器的创建只会执行一次，也就保证了 Spring 容器在整个应用中的唯一性。

当 Spring 容器创建好后，在整个应用的生命周期过程中，Spring 容器应该是随时可以被访问的。即，Spring 容器应具有全局性。而放入 ServletContext 对象的属性，就具有应用的 全局性。所以，将创建好的 Spring 容器，以属性的形式放入到 ServletContext 的空间中，就 保证了 Spring 容器的全局性。