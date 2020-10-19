---
title: Comparable接口和Comparator接口
date: 2020-10-19 15:05:07
tags:
	- Java
	- 难点
	- 基础知识
categories:
	- 难点
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201019150833.jpg
---

# Comparable接口和Comparator接口



## Comparable接口

`Comparable`接口需要结合Collections类的`static <T> void sort(List<T> list)`这个方法来理解。

使用`Collections.sort(List<T> list)`方法对集合中的元素进行排序，被排序的类必须实现`Comparable<T>`接口，并且覆盖重写它的`int com	pareTo(T o)`方法，而`compareTo()`内部是制定排序规则的语句。

**代码演示**

实现功能：对ArrayList集合的Person对象按照其元素age升序排序。

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



## Comparator接口

`Comparator`接口需要结合Collections类的`static <T> void sort(List<T> list, Comparator<? super T> c)`这个方法来理解。

使用`Colllections.sort(List<T> list, Comparator<? super T>)`对集合的对象进行排序，集合中对象的类就不需要强制实现`Comparable<T>接口`了，但是使用`sort()`方法时第二参数必须被传入一个`Comparator`的对象，同时必须覆盖重写它的`compare(T o1, T o2)`，这个`compare(T o1, T o2)`内部是制定排序规则的语句。

**代码演示**

实现功能：对ArrayList集合中的Students对象按照其元素age降序排列，如果年龄相等，那么按照其元素name首字母升序排列。

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

