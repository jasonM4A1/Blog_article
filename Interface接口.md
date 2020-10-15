---
title: Interface接口
date: 2020-10-15 21:03:24
tags: Java
---

# Interface接口

## 概述

接口是Java语言中一种引用类型.

接口的定义，它与定义类方式相似，但是使用的是`interface`关键字。接口不能创建对象，但是可以被子类/子接口实现（implements）。

## 格式

~~~java
修饰符 interface 接口名 {
    //抽象方法
    //静态方法
    //默认方法
    //私有方法
}
~~~

**代码演示**

~~~java
public interface Animal {
    //抽象方法
    public abstract void show();

    //静态方法
    public static String print(String s1) {
        return s1;
    }

    //默认方法
    public default int calc(int one, int two) {
        return one + two;
    }

    //私有方法
    private boolean judge(Object o1, Object o2) {
        return o1 == o2;
    }
}
~~~

## 接口中的成员变量

接口中的成员变量其实就是常量，其值是不可以改变的。

~~~java
//public static final 数据类型 常量名称 = 数据值;(接口内public static final都可以省略)
public static final int num = 100;
~~~

## 接口的多实现

+ 一个接口能继承另一个或者多个接口（类只能继承一个父类）, 接口的继承使用`extends`关键字。
+ 子接口继承父接口的方法时，如果父接口中的默认方法有重名的，那么子接口需要重写一次。

~~~java
interface Donkey {
    default void method(){
        System.out.println("AAAAAAAAA");    
    }
}
interface Horse {
    default void method(){
        System.out.println("BBBBBBBBBB");
    }
}
interface Mule extends Donkey, Horse {
    default void method(){
        System.out.println("CCCCCCCCCC");
    }
}
~~~

> 子接口重写默认方法时，default关键字可以保留。
>
> 子类重写默认方法时，default关键字不可以保留。

## 注意

+ 接口是没有静态代码块或者构造方法的。
+ 一个类只可能有一个直接父类，但是一个类可以同时实现多个接口。
+ 其接口的实现类必须覆盖重写接口的所有抽象方法，除非实现类是抽象类。
+ 如果实现类所实现的多个接口中，存在重复的抽象方法，那么只需要覆盖重写一次即可(相当于只继承一个)。
+ 如果实现类实现的多个接口中，存在重复的默认方法，那么实现类就必须要对重复的默认方法重写一次（而且必须带着default关键字）。
+ 如果一个类的直接父类中的方法，与接口当中的默认方法产生了重复，则优先使用父类当中的方法。
+ 实现的多个接口中，如果存在同名的静态方法并不会冲突，原因是只能通过各自接口名访问静态方法。
+ 接口也会被编译成`.class`文件，但一定要明确它并不是类，而是另外一种引用数据类型。

> 关于抽象方法、默认方法、静态方法、私有方法的内容，可以到《接口内四种方法类型》一文中查看。