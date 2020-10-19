---
title: Set接口
date: 2020-10-18 17:13:09
tags:
	- Java
	- 集合
	- 基础知识
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201018171642.jpg
---

# Set接口



## 概述

`java.util.Set`接口和`java.util.List`接口一样，同样继承自`Collection`接口，它与`Collection`接口中的方法基本一致，并没有对`Collection`接口进行功能上的扩充，只是比`Collection`接口更加严格了。与`List`接口不同的是，`Set`接口中元素无序，并且都会以某种规则保证存入的元素不出现重复。

> `Set`集合有多个子类，这里我们介绍其中的`java.util.HashSet`、`java.util.LinkedHashset`这两个集合。



## HashSet集合

### 介绍

`java.util.HashSet`是`Set`接口的一个实现类，它所存储的元素是不同重复的，并且元素都是无序的（即存取顺序不一致）。`java.util.HashSet`底层的实现其实是一个`java.util.HashMap`。

`HashSet`是根据对象的哈希值来确定元素在集合中的存储位置，因此具有良好的存取和查找性能。保证元素唯一性的方式依赖于：`hashCode`和`equals`方法。

### 格式

~~~java
import java.util.HashSet;

public class Test {
    public static void main(String[] args) {
        //创建一个Set集合
        HashSet<String> set = new HashSet<>();

        //添加元素
        set.add("jason");
        set.add("boy");
        set.add("int");
        set.add(new String("jason"));

        //遍历
        for (String s: set) {
            System.out.print(s + "; ");//jason; boy; int;
        }
    }
}
~~~

> 根据结果我们发现字符串"jason"只存储了一个，也就是说重复的元素set集合不存储。

### 数据结构

Set接口采用的是哈希表作为数据结构。

在`JDK1.8`之前，哈希表底层采用数组+链表实现，即使用链表处理冲突，同一`hash`值的链表都存储在一个链表里。但是当位于一个桶中的元素较多，即`hash`值相等的元素较多时，通过`key`值一次查找的效率较低。而`JDK1.8`中，哈希表存储采用【数组】+【链表】+【红黑树】实现。当链表长度超过阈值8时，将链表转换为红黑树，这样大大减少了查找时间。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201018172932.jpg)

看到这张图就有人要问了，这个是怎么存储的呢？

为了方便大家的理解我们结合一个存储流程图来说明一下：

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201018173127.png)

总而言之，`JDK1.8`引入红黑树大程度优化了`HashMap`的性能。那么对于我们来讲保证`HashSet`集合元素的唯一，其实就是根据对象的`hashCode`和`equals`方法来决定的。如果我们往集合中存放自定义的对象，那么保证其唯一，就必须重写`hashCode`和`equals`方法建立属于当前对象的比较方式。

### HashSet存储自定义类型元素

给`HashSet`中存放自定义类型元素时，需要重写对象中的`hashCode`和`equals`方法，建立自己的比较方式，才能保证`HashSet`集合中对象的唯一性。

**代码演示**

~~~java
import java.util.HashSet;
import java.util.Objects;

public class Test {
    public static void main(String[] args) {
        //创建一个HashSet集合, 该集合只存储Person对象
        HashSet<Person> hash = new HashSet<>();

        //向HashSet集合添加Person对象
        hash.add(new Person("jason",21));
        hash.add(new Person("jason",22));
        hash.add(new Person("jason",21));

        //使用foreach遍历
        for (Person p: hash) {
            System.out.print(p + "; ");//Person{name='jason', age=22}; Person{name='jason', age=21}; 
        }
    }
}

//创建自定义类Person
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

    //setter and getter

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    //重写equals()

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return age == person.age &&
                Objects.equals(name, person.name);
    }

    //重写hashCode()

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }

    //重写toString()

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
~~~

### LinkedHashSet

我们知道`HashSet`保证元素唯一，可以元素存放进去是没有顺序的。那么如果我们要保证元素有序，怎么办呢？

在`HashSet`下面有一个子类`java.util.LinkedHashSet`，它是**链表**和**哈希表**组合的一个数据存储结构。

**代码演示**

~~~java
import java.util.Iterator;
import java.util.HashSet;
import java.util.LinkedHashSet;

public class Test {
    public static void main(String[] args) {
        //创建一个LinkedHashSet集合，用于存储String对象
        HashSet<String> set = new LinkedHashSet<>();

        //添加元素
        set.add("first");
        set.add("first");//重复的元素，不能被添加到集合中
        set.add("second");
        set.add("third");
        set.add("fourth");

        //通过iterator()获取迭代器对象
        Iterator<String> it = set.iterator();
        //利用迭代器对象的hasNext()和next()，进行迭代遍历
        while (it.hasNext()) {
            String s = it.next();
            System.out.print(s + "; ");//first; second; third; fourth; （元素存取有序）
        }
    }
}
~~~

