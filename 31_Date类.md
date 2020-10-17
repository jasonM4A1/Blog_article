---
title: Date类
date: 2020-10-17 18:28:38
tags:
	- Java
	- 基础知识
	- 常用类
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201017182950.jpg
---

# Date类

## 概述

`java.util.Date`类表示特定的瞬间，精确到毫秒。

## 构造函数

继续查阅`Date`类的 描述，发现`Date`拥有多个构造函数，只是部分已经过时，但是其中未为过时的构造函数可以把毫秒值转成日期对象。

+ `Date()`：分配`Date`对象并初始化此对象，以表示分配它的时间（精确到毫秒）
+ `Date(long date)`：分配`Date`对象并初始化此对象，以表示自从标准基准时间（称为“历元epoch”，即1970年1月1日00:00:00 GMT）以来指定的毫秒数。

> 由于我们处于东八区，所以我们的基准时间为1970年1月1日8时0分0秒

简单的来说：使用无参构造，可以自动设置当前系统时间的毫秒时刻；指定`long`类型的构造参数，可以自定义毫秒时刻。

**代码演示**

~~~java
import java.util.Date;

public class Test {
    public static void main(String[] args) {
        //使用当前时间创建日期对象
        Date d1 = new Date();
        System.out.println(d1);//Sat Oct 17 19:16:48 CST 2020
        //使用指定的long时间创建日期对象
        Date d2 = new Date(1000);
        System.out.println(d2);//Thu Jan 01 08:00:01 CST 1970
    }
}
~~~

> 在使用`println`方法时，会自动调用`Date`类的`toString`方法。`Date`类对`Object`类中的`toString`方法进行了覆盖重写，所以结果为指定格式的字符串。

## 成员方法

`Date`类中的多数方法已经过时，我们目前需要学习的只有一种：

+ `long getTime()`：把日期对象转换成对应的时间毫秒值

**代码演示**

~~~java
import java.util.Date;

public class Test {
    public static void main(String[] args) {
        //使用当前时间使用创建日期对象
        Date d1 = new Date();
        //将日期对象转换成对应的时间毫秒值
        long num1 = d1.getTime();
        System.out.println(num1);//1602933856021

        //使用指定时间创建日期对象
        Date d2 = new Date(1000);
        //将日期对象转换成对应的时间毫秒值
        long num2 = d2.getTime();
        System.out.println(num2);//1000
    }
}
~~~

