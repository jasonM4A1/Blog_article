---
title: Final关键字
date: 2020-10-17 15:38:26
tags:
	- Java
	- 基础知识
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201017154006.jpg
---

# Final关键字

## 概述

`final`被译为`最终的，不可改变的`，在Java中作为一个关键字可以修饰类、方法、变量及参数。被`final`修饰的类、方法、变量及参数都会发生不同的变化，具体有以下几种：

1. 被`final`修饰的类，不能被继承。
2. 被`final`修饰的方法，不能被重写。
3. 被`final`修饰的变量，变量分为`局部变量`和`成员变量`。
   1. 局部变量
      + 不能走重新赋值。
   2. 成员变量
      + 因为成员变量会被赋予默认值，所有加上final后，声明之初就必须手动赋值，不然这个变量今后都是默认值了。
      + 声明之初除了手动赋值外，还可以通过构造方法来赋值，但这二者必须选其一 。
      + 如果选择通过构造方法来赋值，那么就必须保证类当中所有重载的构造方法，最终都会对final修饰的成员变量进行赋值。

## 格式

### final类

~~~java
//final class 类名 {}
final class Person {
    //EXEC statement
}
~~~

### final方法

~~~java
//修饰符 final 返回值 方法名(参数列表) {}
public final void run(boolean b) {
    //EXEC statement
}
~~~

### final变量

1. 局部变量

   + 局部变量——基本数据类型

     局部变量中属于基本数据类型的，从声明开始只能被赋值一次，不可被第二次赋值。

     ~~~java
     public class Test {
         public static void main(String[] args) {
             //声明并进行第一次赋值
             final int one = 100;
             //在声明时已经进行了第一次赋值了，进行第二次赋值时，报错
             one = 200;//Cannot assign a value to final variable 'one'
         }
     }
     ~~~

   + 局部变量——引用数据类型

     局部变量中属于引用数据类型的，从创建变量开始，就只能指向最初的那个对象地址，不能再次更改接收其他对象的地址。但是不影响对象内部内容的改变。

     ~~~java
     public class Test {
         public static void main(String[] args) {
             //创建一个Person类，并将Person类的对象地址赋值给被final修饰的man变量
             final Person man = new Person();
             //尝试对man变量进行第二次赋值时，报错
             man = new Person(); //Cannot assign a value to final variable 'man'
     
             //但是man变量所指向的Person对象内部的成员变量的值仍可以改变,
             man.name = "JasonSpring";
             man.age = 23;
             man.gender = false;
         }
     }
     
     //定义一个Person类
     class Person {
         //定义三个成员变量，并为其赋值
         String name = "Jason Li";
         int age = 21;
         boolean gender = true;
     }
     ~~~

2. 成员变量

   成员变量涉及到初始化的问题，初始化有两种方式，并且只能二选一。

   + 显示初始化——即在成员变量声明的同时就给予赋值。

     ~~~java
     class Person {
         //定义之初就为其赋值
         final String NAME = "Jason Li";
         final int AGE = 21;
         final boolean DGENDER = true;
     }
     ~~~

   + 构造方法初始化——先声明，然后使用有参构造器给变量赋值。

     ~~~java
     class Person {
         //定义变量，但不赋值
         final String NAME;
         final int AGE;
         final boolean GENDER;
     
         //constructor 只创建有参构造器，强制对象创建之初就必须被赋予值
         public Person(String NAME, int AGE, boolean GENDER){
             this.NAME = NAME;
             this.AGE = AGE;
             this.GENDER = GENDER;
         }
     }
     ~~~

## 注意

`final`修饰的的常量名，书写规范规定一般都大写



