---
title: abstract抽象类
date: 2020-10-15 17:21:24
tags: 
	- Java
	- 常用类
	- 基础知识
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201017135142.jpg
---

# abstract抽象类

## 概述

我们把没有方法主体的方法称为**抽象方法**，包含抽象方法的类就是**抽象类**。

## 格式

### 抽象方法

~~~java
//修饰符 abstract 返回值类型 方法名(参数列表);
public abstract int calc(int first, int second);
~~~

### 抽象类

~~~java
//修饰符 abstract class 类名 {}
public abstract class Test {
    //抽象方法
    public abstract int calc(int first, int second);
    //普通方法
    public String print(String s) {
        return s;
    }
}
~~~

## 注意

1. 继承抽象类的子类**必须重写父类所有的抽象方法**。否则，该子类也必须声明为抽象类。并且，最终必须要有子类来实现该父类的抽象方法

2. 不能通过抽象类创建对象，只能通过抽象类的非抽象子类创建对象。

3. 抽象类中，可以有构造方法，是供子类创建对象时，初始化父类成员使用的。

4. 抽象类中，不一定包含抽象方法。但是有抽象方法的类，必定是抽象类。

   *未包含抽象方法的抽象类，目的就是不想让调用者创建该类对象，通常用于某些特殊的类结构设计。*

5. 抽象类的子类若不为抽象类，那么就必须重写父类中所有的抽象方法。

