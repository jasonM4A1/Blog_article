---
title: Collections工具类
date: 2020-10-18 18:12:32
tags:
	- Java
	- 基础知识
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201018181413.jpg
---

# Collections工具类



## 概述

`java.utils.Collections`是一个用来对List集合进行操作的集合工具类。



## 常用功能

+ `static <T> boolean addAll(Colletion<T> c, T... elements)` ：往集合中添加一些元素。
+ `static void shuffle(List<?> list)`：打乱集合中元素的顺序。
+ `static <T> void sort(List<T> list)`：将集合中元素按照默认规则排序。
+ `static <T> void sort(List<T> list, Comparator<? super T> c)`：将集合中元素按照指定规则排序。

**代码演示**

~~~java
import java.util.ArrayList;
import java.util.Collections;

public class Test {
    public static void main(String[] args) {
        //创建一个ArrayList数组
        ArrayList<Integer> list = new ArrayList<>();

        //采用Collections工具类中的addAll()，完成向集合中一次添加多个元素
        Collections.addAll(list,1,2,3,4,5);
        System.out.println(list);//[1, 2, 3, 4, 5]

        //使用shuffle()打乱集合中元素的顺序
        Collections.shuffle(list);
        System.out.println(list);//4, 3, 5, 1, 2]
    }
}
~~~

## Comparable接口和Comparator接口

上面我们演示了Collections类两种方法的使用，还有两个排序方法没有演示，这里我们开始讲解这两种方法的使用——`static <T> void sort(List<T> list)`和`static <T> sort(List<T> list, Comparator<? super T> c)`。

### 第一种方法（Comparable接口）

第一种对集合元素进行排序的方法就是：`static <T> void sort(List<T> list)`。

使用`Collections.sort(List<T> list)`方法对集合中的元素进行排序，被排序的类必须实现`Comparable<T>`接口，并且覆盖重写它的`int com	pareTo(T o)`方法，而`compareTo()`内部是制定排序规则的语句。

**代码演示**

需要实现功能：对ArrayList集合的Person对象按照其元素age升序排序。

~~~java
import java.util.Collections;
import java.util.ArrayList;

//自定义类实现Comparable接口
class Person implements Comparable<Person> {
    private String name;
    private int age;

    //constructor
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    //重写compareTo()方法，制定排序规则
    @Override
    public int compareTo(Person p) {
        return this.age - p.age;//按照年龄升序排序
    }
}

//测试类
public class Test {
    public static void main(String[] args) {
        //创建ArrayList集合
        ArrayList<Person> list = new ArrayList<>();

        //添加元素
        Collections.addAll(list, new Person("jason", 21),
                new Person("John", 18) ,
                new Person("Bob",16));

        //进行排序
        Collections.sort(list);

        //验证
        System.out.println(list);
        //[Person{name='Bob', age=16},
        // Person{name='John', age=18},
        // Person{name='jason', age=21}]
        //按年龄升序排序成功
    }
}
~~~

> Integer、Long这项包装类和String类都已经默认实现了`Comparable<T>`接口和重写了`compareTo()`方法，所以我们可以直接使用`static <T> void sort(List<T> list)`对仅包含包装类和String类的集合进行默认规则的排序。

### 第二种方法（Comparator接口）

第一种对集合元素进行排序的方法就是：`static <T> void sort(List<T> list, Comparator<? super T> c)`。

使用`Colllections.sort(List<T> list, Comparator<? super T>)`对集合的对象进行排序，集合中对象的类就不需要强制实现`Comparable<T>接口`了，但是使用`sort()`方法时第二参数必须被传入一个`Comparator`的对象，同时必须覆盖重写它的`compare(T o1, T o2)`，这个`compare(T o1, T o2)`内部是制定排序规则的语句。

**代码演示**

需要实现功能：对ArrayList集合中的Students对象按照其元素age降序排列，如果年龄相等，那么按照其元素name首字母升序排列。

~~~java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;

//自定义类
class Students {
    private int age;
    private String name;

    //constructor
    public Students(int id, String name) {
        this.age = id;
        this.name = name;
    }

    //setter and getter
    public int getAge() {
        return age;
    }

    public void setAge(int id) {
        this.age = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Students{" +
                "id=" + age +
                ", name='" + name + '\'' +
                '}';
    }
}

//测试类
public class Test {
    public static void main(String[] args) {
        //创建ArrayLit集合
        ArrayList<Students> list = new ArrayList<>();

        //添加元素
        Collections.addAll(list, new Students(19, "John"),
                new Students(18, "Jason"),
                new Students(29, "Bod"),
                new Students(19, "Charlie"));

        //进行排序
        Collections.sort(list, new Comparator<Students>(){
           @Override
            public int compare(Students s1, Students s2) {
               int num = 0;
               //首先按照age的降序排列
               num = s2.getAge() - s1.getAge();
               //如果age相等，那么按照name的首字母升序排列
               if (num == 0) {
                    num = s1.getName().charAt(0) - s2.getName().charAt(0);
               }
               return num;
           }
        });

        //验证
        System.out.println(list);
        //[Students{id=29, name='Bod'},
        // Students{id=19, name='Charlie'},
        // Students{id=19, name='John'}, 
        // Students{id=18, name='Jason'}]
    }
}
~~~







