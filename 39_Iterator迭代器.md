---
title: Iterator迭代器
date: 2020-10-18 00:41:12
tags:
	- Java
	- 基础知识
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201018004224.jpg
---

# Iterator迭代器



## 概述

在程序开发中，经常需要遍历集合中的所有元素。针对这种需求，JDK专门提供了一个接口`java.util.Iterator`。`Iteraotr`接口也是`Java`集合中的一员，但它与`collection`、`Map`接口有所不同，`Collection`接口与`Map`接口主要用于存储元素，而`Iterator`主要用于迭代方法（即遍历）`Collection`中的元素，因此`Iterator`对象也被称为迭代器。

> **迭代**：即`Collection`集合元素的通用获取方法。在取元素之前先要判断集合中有没有元素，如果有，就把这个元素取出来。继续再判断，如果还有就再取出来。一直把集合中的所有元素全部取出。这种取出方式专业术语称为迭代。



## 获取迭代器

想要遍历`Collection`集合，那么就要获取该集合迭代器完成迭代操作，下面介绍一下获取迭代器的方法：

+ `public Iterator iterator()`：获取集合对应的迭代器，用来遍历集合中的元素的。

  ~~~java
  //创建一个ArrayList对象
  Collection<String> list1 = new ArrayList<>();
          
  //获取ArrayList集合的迭代器
  Iterator<String> iterator = list1.iterator();
  ~~~

  

## 常用方法

+ `boolean hashNex()`：判断集合中是否还有元素可以迭代。
+ `E next()`：返回迭代的下一个元素。

**代码演示**

~~~java
import java.util.Collection;
import java.util.Iterator;
import java.util.ArrayList;

public class Test {
    public static void main(String[] args) {
        //创建一个ArrayList对象
        Collection<String> list1 = new ArrayList<>();

        //向集合中添加元素
        list1.add("first");
        list1.add("second");
        list1.add("third");

        //获取ArrayList集合的迭代器
        Iterator<String> iterator = list1.iterator();

        //结合Iterator类的hasNext()方法和next()方法 遍历集合
        while (iterator.hasNext()) {
            String s = iterator.next();
            System.out.print(s + "; ");//first; second; third; 
        }
    }
}
~~~

> 在进行集合元素取出时，如果集合中已经没有元素了，还在继续使用迭代器的`next`方法，将会发生`java.util.NoSuchElementException`没有这样的元素异常提醒。



## 增强for循环

增强`for`循环，也称为`for each`循环，是JDK1.5以后出来的一个高级`for`循环，专门用来遍历数组和集合的。它的内部原理其实是个`Iterator`迭代器，所以在遍历的过程中，不能对集合中的元素进行增删操作。

**格式如下：**

~~~java
for (元素的数据类型 变量名 : Collection集合 or 数组) {
    //EXEC statement
}
~~~

**代码演示**

~~~java
import java.util.Collection;
import java.util.ArrayList;

public class Test {
    public static void main(String[] args) {
        //创建一个ArrayList对象
        Collection<String> list1 = new ArrayList<>();

        //向集合中添加元素
        list1.add("first");
        list1.add("second");
        list1.add("third");

        //使用foreach遍历集合
        for (String s: list1) {
            System.out.print(s + "; ");//first; second; third; 
        }
    }
}
~~~

>  注：它用于遍历`Collection`和数组。通常只进行遍历元素，不要在遍历的过程中对集合元素进行增删操作！