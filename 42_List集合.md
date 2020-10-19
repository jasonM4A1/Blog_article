---
title: List集合
date: 2020-10-18 16:11:28
tags:
	- Java
	- 集合
	- 基础知识
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201018161414.jpg
---

# List集合



## 概述

`java.util.List`接口继承自`Collection`接口，是单列集合的一个重要分支。特点如下：

1. 存取有序
2. 带有索引
3. 可以重复



## 常用方法

`List`作为`Collection`集合的子接口，不但继承了`Collection`接口中的全部方法，而且还增加了一些根据元素索引来操作集合的特有方法，如下：

+ `void add(int index, E element)`：将指定的元素，添加到该集合中的指定位置上。
+ `E get(int index)`：返回集合中指定位置的元素。
+ `E remove(int index)`：移除列表中指定位置的元素，并返回被移除的元素。
+ `E set(int index, E element)`：用指定元素替换集合中指定位置的元素，并返回被替换的元素。

**代码演示**

~~~java
import java.util.ArrayList;
import java.util.List;

public class Test {
    public static void main(String[] args) {
        //创建一个ArrayList集合(不能使用多态Collection)
        List<String> list = new ArrayList<>();

        //向集合中添加元素
        list.add("first");
        list.add("second");
        list.add("third");
        list.add("fourth");
        list.add("fifth");
        System.out.println(list);//[first, second, third, fourth, fifth]

        //使用void add(int index, E element)往指定位置添加元素
        list.add(1,"第二");
        System.out.println(list);//[first, 第二, second, third, fourth, fifth]

        //使用E remove(int index)，删除指定索引处的元素，并返回被删除的元素
        String re = list.remove(0);
        System.out.println(re);//first
        System.out.println(list);//[第二, second, third, fourth, fifth]

        //E set(int index, E element)替换指定索引处的元素, 并返回被替换的元素
        String set = list.set(2, "第三");
        System.out.println(set);//third
        System.out.println(list);//[第二, second, 第三, fourth, fifth]

        //使用for循环结合int size()和 E get(int index)来进行遍历集合
        for (int i = 0; i < list.size(); i++) {
            System.out.print(list.get(i) + "; ");//第二; second; 第三; fourth; fifth;
        }
        System.out.println();
        
        //使用foreach进行遍历循环
        for (String s: list) {
            System.out.print(s + "; ");//第二; second; 第三; fourth; fifth; 
        }
    }
}
~~~



## List的子类

### ArrayList集合

+ `java.util.ArrayList`集合数据结构为数组。

+ 特点是：有序，增删慢，查找快。

+ 由于日常开发中使用最多的功能为查询数据、遍历数据，所以`ArrayList`是最常用的集合。

### LinkedList集合

+ `java.util.LinkedList`集合数据结构为双向链表。

+ 特点是：查找慢，增删快。

+ 实际开发中对一个集合元素的添加与删除经常涉及到首尾的操作，而`LinkedList`提供了大量首尾操作的方法，这些方法我们作为了解既可：

  ~~~java
  public void addFirst(E e)：将指定元素插入此列表的开头。
  public void addLast(E e)：将指定元素添加到此列表的结尾。
  public E getFirst()：获取此列表的第一个元素。
  public E getLast()：获取此列表的最后一个元素。
  public E removeFirst()：删除并返回列表中第一个元素。
  public E removeLast()：删除并返回列表中最后一个元素。
  public E pop()：从此列表所表示的堆栈处弹出一个元素。
  public void push(E e)：将一个元素推入此列表所表示的堆栈。
  ~~~

  