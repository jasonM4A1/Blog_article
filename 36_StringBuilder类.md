---
title: StringBuilder类
date: 2020-10-17 21:37:01
tags:	
	- Java
	- 常用类
	- 基础知识
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201017213821.jpg
---

# StringBuilder类

## 概述

由于`String`类的对象内容不可改变，所以每当进行字符串拼接时，总是会在内存中创建一个新的对象。例如：

~~~java
public class Review01 {
    public static void main(String[] args) {
        //第一个字符串"jason"
        String s1 = "jason";
        //第二个字符串"Spring"
        s1 += "Spring";
        //第三个字符串"jasonSpring"
        System.out.println(s1);//jasonSpring
    }
}
~~~

在`API`中对`String`类有这样的描述：字符串是常量，它们的值在创建后不能被更改。

根据这句话分析我们的代码，其实总共产生了三个字符串对象，即`"jason"`、`Spring`、`jasonSpring`。引用变量`s1`首先指向`jason`对象，最终指向拼接出来的新字符串对象，即`jasonSpring`。

由此可知，如果对字符串进行拼接操作，每次拼接都会构建一个新的`String`对象。即耗时，又浪费空间。为了解决这一问题，可以使用`java.lang.StringBuilder`类。

## 构造方法

+ `StringBuilder()`：构造一个空的`StringBuilder`容器。
+ `StringBuilder(String str)`：构造一个`StringBuilder`容器，并将参数的字符串添加进去。

**代码演示**

~~~java
public class Test {
    public static void main(String[] args) {
        //无参构造
        StringBuilder sb1 = new StringBuilder();
        System.out.println(sb1);//【空】
        //有参构造
        StringBuilder sb2 = new StringBuilder("jason");
        System.out.println(sb2);//jason
    }
}
~~~

## 常用方法

+ `StringBuilder append(...)`：添加任意类型数据的字符串形式，并返回当前对象自身。

  append()具有多种重载形式，可以接收任意类型的参数。任何数据作为参数都会将对应的字符串内容添加到`StringBuilder`中。例如：

  ~~~java
  public class Test {
      public static void main(String[] args) {
          //创建一个StringBuilder对象
          StringBuilder builder = new StringBuilder();
          //使用StringBuilder类里面的append()来添加其他对象到字符串里面
          StringBuilder builder1 = builder.append(100);
          //判断调用append()方法后返回的是不是对象自身
          System.out.println(builder == builder1);//true
          //append()还可以添加任意类型的数据到StringBuilder对象里面
          builder.append(true);
          builder.append("jason");
          builder.append(new Person());
          System.out.println(builder);//100truejasonPerson@70177ecd
  
          //在我们开发中，遇到调用一个方法后，返回一个对象，然后使用返回的对象继续调用方法。
          //这种时候，我们就可以采用“链式编程”，代码如下：
          StringBuilder builder2 = new StringBuilder();
          builder2.append("Jason").append(21).append(true);
          System.out.println(builder2);//Jason21true
      }
  }
  
  //定义一个Person类
  class Person {
  
  }
  ~~~

  > `StringBuilde`已经覆盖重写了`Object`当中的`toString`方法。

+ toString()

  通过`toString()`，`StringBuilder`对象将会转换为不可变的`String`对象。

  ~~~java
  public class Test {
      public static void main(String[] args) {
          StringBuilder builder = new StringBuilder("jason");
          //使用toString()方法将StringBuilder对象转化为String对象
          String string = builder.toString();
      }
  }
  ~~~

  ## StringBuffer
  
  由于 StringBuilder 相较于 StringBuffer 有速度优势，**所以多数情况下建议使用 StringBuilder 类**。
  
  然而在应用程序要求线程安全的情况下，则必须使用 StringBuffer 类。