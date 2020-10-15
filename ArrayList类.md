---
title: ArrayList类
date: 2020-10-15 13:10:10
tags: Java
---

# ArrayList类

## 概述

`java.util.ArrayList`是大小可变的数组的实现. `ArrayList`中可不断添加元素,其大小也自动增长.

而普通数组的长度是创建之初就固定的,无法适应数据变化的需求. 

## 构造方法

+ `ArrayList()`：构造一个初始容量为10的空ArrayList集合。
+ `ArrList(int initialCapacity)`：构造一个具有指定初始容量的空ArrayList集合。
+ `ArrayList(Collection<? extend E> c)`：构造一个包含指定集合元素的列表，按照集合的迭代器返回元素的顺序。

## 成员方法

+ `boolean add(E e)`：将指定的元素追加到列表的末尾。
+ `E remove(int index)`：移除此列表中指定位置的元素，并返回它。
+ `E get(int index)`：返回列表中指定位置的元素。
+ `int size()`：返回列表中元素的数量。

## 如何存储基本数据类型

ArrayList集合不能储存基本数据类型，只能储存引用数据类型。如果想要将诸如`int`、`long`、`boolean`等基本数据类型储存到ArrayList集合中，必须先将`基本数据类型`包装成`引用数据类型`。

| 基本数据类型 | 引用数据类型 |
| :----------: | :----------: |
|     byte     |     Byte     |
|    short     |    Short     |
|     int      |   Integer    |
|     long     |     Long     |
|    float     |    Float     |
|    double    |    Double    |
|     char     |  Character   |
|   boolean    |   Boolean    |