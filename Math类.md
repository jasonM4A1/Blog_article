---
title: Math类
date: 2020-10-15 16:05:35
tags: Java
---

# Math类

## 概述

`java.lang.Math`类包含用于执行基本数学运算的方法，如初等指数、对数、平方根和三角函数等工具类。其所有的方法均为静态方法，并且不会创建对象，调用起来非常简单。

## 常用方法

+ `static double abs(double a)`：求绝对值。
+ `static double ceil(double a)`：向上取整。
+ `static double floor(double a)`：向下取整。
+ `static long round(double a)`：四舍五入。

**代码演示**

~~~java
public class Test {
    public static void main(String[] args) {
        double one = 21.05;
        double two = -99.53;

        //`static double abs(double a)`：求绝对值。
        double three = Math.abs(one);
        double four = Math.abs(two);
        System.out.println(three);//21.05
        System.out.println(four);//99.53

        //`static double ceil(double a)`：向上取整。
        double five = Math.ceil(one);
        double six = Math.ceil(two);
        System.out.println(five);//22.0
        System.out.println(six);//-99.0

        //`static double floor(double a)`：向下取整。
        double seven = Math.floor(one);
        double eight = Math.floor(two);
        System.out.println(seven);//21.0
        System.out.println(eight);//-100.0

        //`static long round(double a)`：四舍五入。
        double nine = Math.round(one);
        double ten = Math.round(two);
        System.out.println(nine);//21.0
        System.out.println(ten);//-100.0
    }
}
~~~

