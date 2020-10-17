---
title: Object类
date: 2020-10-17 17:26:13
tags: 
	- Java
	- 常用类
	- 基础知识
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201017172812.jpg
---

# Object类

## 概述

1. `java.lang.object`类是Java语言中的根类，即所有类的父类。它中描述的所有方法，其他子类都可以使用。

2. 如果一个类没有特别指定父类，则默认继承`Object`类。在对象实例化的时候，最终找的父类也就是`Object`。

3. 根据JDK源代码及Object类的API文档，`Object`类当中包含的方法有11个。我们主要学习其中的2个。
   + `public String toString() `：返回该对象的字符串表示。
   + `public boolean equal(Object obj)`：指示其他某个对象是否与此对象“相等”。

## toString方法

### 方法简述

`public String toString()`：返回该对象的字符串表示。

`toString`方法返回该对象的字符串表示，其实该字符串内容就是**对象的类型+@+内存地址值**。

由于`toString`方法返回的结果是内存地址，而在开发中，经常需要按照对象的属性得到相应的字符串表达形式，因此也需要重写它。

### 覆盖重写

如果不希望使用`toString`方法的默认行为，则可以对它进行覆盖重写。

**代码演示**

~~~java
class Person {
    private String name;
    private int age;

    //constructor

    public Person() {
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    //对toString方法进行覆盖重写
    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
~~~

## equals方法

### 方法简述

`public boolean equals(Object obj)`：指示其他某个对象是否与此对象“相等”。

调用成员方法`equals()`并指定参数为另一个对象，则可以判断这两个对象是否是相同的。这里的“相同”有默认和自定义两种方式。

### 默认地址比较

如果没有覆盖重写`equals()`，那么`Object`类中默认进行的比较是`=`运算符的对象地址比较，只要不是同一个对象，结果必然为`false`。

### 对象内容比较

如果希望进行对象的内容比较，即所有或指定的部分成员变量相同就判定两个对象相同，则可以覆盖重写`equals()`。

**代码演示**

~~~java
import java.util.Objects;

class Person {
    private String name;
    private int age;

    //constructor

    public Person() {
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    //重写equals()方法
    @Override
    public boolean equals(Object o) {
        //若对象地址一致，则认为相同
        if (this == o) return true;
        //若对象地址为null，或者类型信息不一致，则认为不相同
        if (o == null || getClass() != o.getClass()) return false;
        //强制类型转化
        Person person = (Person) o;
        //要求基本类型相等，并且将引用类型交给java.util.Objects类的静态方法取用结果
        return age == person.age &&
                Objects.equals(name, person.name);
    }
}
~~~

