---
title: Collection集合
date: 2020-10-17 23:22:58
tags:
	- Java
	- 集合
	- 基础知识
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201017232412.jpg
---

# Collection集合



## 概述

集合是java中提供的一种可以存储多个数据的容器。

**既然集合和数组都是容器，它们有什么区别呢？**

+ 数组的长度是固定的；集合的长度是可变的。
+ 数组中存储的是同一类型的元素，可以存储基本数据类型值。集合存储的都是对象，而且对象的类型可以不一致。在开发中一般当对象多的时候，应使用集合进行存储。



## 集合框架

集合按照其存储结构可以分为两大类：

+ 单列集合`java.util.Collection`
+ 双列集合`java.util.Map`

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201017232849.jpg)



## Collection特点

`Collection`单列集合类的根接口，用于存储一系列符合某种规则的元素。它有两个重要的子接口，分别是：

1. `java.util.List`：
   + 特点是元素有序、元素可重复。
   + 主要实现类有`java.util.ArrayList`、`java.util.LinkedList`、`java.util.Vector`
2. `java.util.Set`：
   + 特点是元素无序，而且不可重复。
   + 主要实现类有`java.util.HashSet`、`java.util.TreeSet`、`java.util.LinkedHashSet`



## Collection常用功能

> `collection`是所有单列集合的父接口，因此在`Collection`中定义了单列集合`List`和`Set`通用的一些方法，这些方法可用于操作所有的单列集合。方法如下：

+ `boolean add(E e)`：把给定的对象添加到当前集合末尾。
+ `void clear()`：清空集合中所有的元素。
+ `boolean remove(E e)`：把给定的对象在当前集合中删除。
+ `boolean contains(E e)`：判断当前集合中是否包含给定的对象。
+ `boolean isEmpty()`：判断当前集合是否为空。
+ `int size()`：返回集合中元素的个数。
+ `Object[] toArray()`：把集合中的元素，存储到数组中。

**代码演示**

~~~java
import java.util.ArrayList;
import java.util.Collection;

public class Test {
    public static void main(String[] args) {
        //使用 多态+泛型 创建集合对象
        Collection<String> c1 = new ArrayList<>();

        //使用boolean add()方法向集合中添加元素
        c1.add("first");
        c1.add("second");
        boolean b1 = c1.add("third");
        System.out.println(b1);//true，表示添加成功

        //使用boolean contains(E e)判断集合是否包含e元素
        boolean b2 = c1.contains("first");
        System.out.println(b2);//true，表示有此元素
        boolean b3 = c1.contains("one");
        System.out.println(b3);//false, 表示没有包含此元素

        //使用boolean remove(E e)删除在集合中的e元素
        boolean b4 = c1.remove("third");
        System.out.println(b4);//true,表示删除成功

        //使用int size()来获取集合中元素的数量
        int num = c1.size();
        System.out.println(num);//2

        //使用Object[] toArray()将集合转换为一个Object数组
        Object[] o = c1.toArray();
        //遍历数组
        for (Object i: o) {
            System.out.print(i + "; ");//first; second;
        }

        //使用void clear()清空集合
        c1.clear();

        System.out.println();//换行

        //使用boolean isEmpty()判断集合是否为空
        boolean b5 = c1.isEmpty();
        System.out.println(b5);//true
    }
}
~~~

> 有关`collection`中的方法可不止上面这些，其他方法可以自行查看API学习。