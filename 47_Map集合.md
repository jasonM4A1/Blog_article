---
title: Map集合
date: 2020-10-19 15:15:06
tags:
	- Java
	- 集合
	- 基础知识
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201019151658.jpg
---

# Map集合



## 概述

现实生活中，我们常会看到这样的一种集合：IP地址与主机名，身份证号与个人，系统用户名与系统用户对象等，这种一对一对应的关系，就叫作映射。`Java`提供了专门的集合类用来存放这种对象关系的对象，即`java.util.Map`接口。

我们通过查看`Map`接口描述，发现`Map`接口下的集合与`Collection`接口下的集合，它们存储数据的形式不同，如下图：

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201019152027.jpg)

**总结不同点如下：**

1. `Collection`中的集合，元素是单个存储的。而`Map`中的集合，元素是由键值对的方式存储的。
2. `Collection`中的集合称为单列集合，`Map`中的集合称为双列集合。

> `Map`中的集合不能包含重复的键；值可以重复；每个键只能对应一个值。

## Map常用子类

通过查看`Map`接口描述，看到`Map`有多个子类。这里我们主要讲解常用的`HashMap`集合、`LinkedHashMap`集合。

1. `HashMap<K, V>`
   + 数据结构是哈希表
   + 元素存取不一致
   + 由于要保证键的唯一性，需要重写键的`hashCode()`方法、`equals()`方法
2. `LinkedHashMap<K, V>`
   + `LinkedHashMap<k,V>`是`HashMap<K,V>`的子类
   + 数据结构是哈希表+链表
   + 元素存取一致，是通过链表结构来保证元素存取顺序的一致。
   + 通过哈希表结构可以保证键的唯一性，但需要重写键的`hashCode()`方法、`equals()`方法。

> `Map`接口中的集合都有两个泛型变量<K, V>，在使用时，要为两个泛型变量赋予数据类型。两个泛型变量<K, v>的数据类型可以相同，也可以不同。

## Map接口中的常用方法

+ `V put(K key, V value)`：把指定的键与指定的值添加到`Map`集合中
+ `V remove(Object key)`：把指定的键所对应的键值对元素从`Map`集合中删除，并返回被删除的元素。
+ `V get(object key)`：根据指定的键，在`Map`集合中获取对应的值
+ `boolean containsKey(object key)`：判断集合中是否包含指定的键
+ `Set<K> keySet()`：获取`Map`集合中所有的键，存储到`Set`集合中并返回
+ `Set<Map.Entry<K, V>> entrySet()`：获取`Map`集合中所有的键值对对象，存储到`Set`集合中并返回。

**代码演示**

~~~java
import java.util.Map;
import java.util.HashMap;
import java.util.Set;

public class Test {
    public static void main(String[] args) {
        //创建一个HashMap
        Map<String, Integer> map = new HashMap<>();

        //添加元素
        map.put("Jason",21);
        map.put("John",26);
        map.put("Charlie",27);
        map.put("Long",29);
        System.out.println(map);//{Charlie=27, John=26, Long=29, Jason=21}

        //移除指定的元素
        Integer remove = map.remove("Long");
        System.out.println(remove);//29
        System.out.println(map);//{Charlie=27, John=26, Jason=21}

        //获取指定键的值
        Integer get = map.get("Jason");
        System.out.println(get);//21

        //判断集合中是否包含指定的键
        boolean con1 = map.containsKey("John");
        boolean con2 = map.containsKey("Long");
        System.out.println(con1);//true
        System.out.println(con2);//false

        //获取`Map`集合中所有的键，存储到`Set`集合中并返回
        Set<String> keys = map.keySet();
        System.out.println(keys);//[Charlie, John, Jason]

        //获取`Map`集合中所有的键值对，存储到`Set`集合中并返回
        Set<Map.Entry<String, Integer>> keyValues = map.entrySet();
        System.out.println(keyValues);//[Charlie=27, John=26, Jason=21]
    }
}
~~~

> 使用`put`方法时，若指定的键（key）在集合中没有，则没有这个键对应的值，返回null，并把指定的键值添加到集合中。
>
> 若指定的键（key）在集合中存在，则返回值为集合中键对应的值（该值为替换前的值），并把指定键所对应的值，替换成指定的新值。

## Map集合遍历

### Map集合遍历——键找值

键找值方式：即通过元素中的键，获取键所对应的值。

分析步骤：

1. 获取Map中所有的键，由于键是唯一的，所以返回一个`Set`集合存储所有的键。
2. 遍历键的Set集合，得到每一个键。
3. 根据键，获取键所对应的值。

**代码演示**

~~~java
import java.util.Map;
import java.util.HashMap;
import java.util.Set;

public class Test {
    public static void main(String[] args) {
        //创建HashMap集合
        Map<String, String> map = new HashMap<>();

        //向集合中添加键值对
        map.put("name","Jason");
        map.put("age","21");
        map.put("gender","man");
        map.put("address","China");
        map.put("hobby","League Of Legends");

        //获取所有的键的Set集合
        Set<String> keySet = map.keySet();

        //通过遍历key，加get()，获取每一个键所对应的值
        for (String key: keySet) {
            String value = map.get(key);
            System.out.print(value + "; ");
            //China; man; Jason; 21; League Of Legends; 
        }
    }
}
~~~

### Map集合遍历——键值对

在介绍这种遍历方式前，我们需要先了解一个对象——**Entry键值对对象**。

我们已经知道，`Map`中存放的是两种对象。一种称为`key`，一种称为`value`，它们在`Map`中是一一对应的关系。这一对对象又称作`Map`中的一个`Entry`。`Entry`将键值对的对应关系封装成了对象，即键值对对象。这样我们在遍历`Map`集合时，就可以从每一个键值对（`Entry`）对象中获取对应的键与对应的值。

既然`Entry`表示了一对键和值，那么也同样提供了获取对应键和对应值的方法。

+ `K getKey()`：获取`Entry`对象中的键。
+ `V getValue()`：获取`Entry`对象中的值。

在`Map`集合中也提供了获取所有`Entry`对象的方法：

+ `public Set<Map.Entry<K, V>> entrySet()`：获取到`Map`集合中所有的键值对对象的Set集合。

下面我们正式遍历键值对。

**键值对方式**：即通过集合中每个键值对（Entry）对象，获取键值对（Entry）对象中的键与值。

分析步骤：

1. 通过方法`EntrySet()`，获取`Map`集合中，所有的键值对（Entry）对象，以Set集合形式返回。
2. 遍历包含键值对（Entry）对象的Set集合，得到每一个键值对（Entry）对象。
3. 通过方法`getKey()`和方法`getValue()，获取Entry对象中的键与值。

**代码演示**

~~~java
import java.util.Map;
import java.util.HashMap;
import java.util.Set;

public class Test {
    public static void main(String[] args) {
        //创建HashMap集合
        Map<String, Integer> map = new HashMap<>();

        //添加键值对
        map.put("Jason",21);
        map.put("John",26);
        map.put("Charlie",29);

        //使用entrySet()获取所有键值对对象
        Set<Map.Entry<String, Integer>> entrySet = map.entrySet();

        //遍历,得到每一entry对象
        for (Map.Entry<String, Integer> entry: entrySet) {
            //解析，等到每一个键和值
            String key = entry.getKey();
            Integer value = entry.getValue();
            System.out.println(key + " : " + value);
            //Charlie : 29
            //John : 26
            //Jason : 21
        }
    }
}
~~~

## HashMap存储自定义类型键值

每位学生（姓名，年龄）都有自己的家庭住址。那么既然有对应关系，则将学生对象和家庭住址存储到Map集合中。学生对象作为键，家庭住址作为值。（学生姓名、年龄都相同，则视为同一名学生）

~~~java
import java.util.Map;
import java.util.HashMap;
import java.util.Objects;
import java.util.Set;

//自定义学生类
class Students {
    private String name;
    private int age;

    //constructor
    public Students(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Students{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Students students = (Students) o;
        return age == students.age &&
                Objects.equals(name, students.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
}

//测试类
public class Test {
    public static void main(String[] args) {
        //创建一个HashMap集合, 形成Students对象与地址映射的关系
        Map<Students, String> map = new HashMap<>();

        //添加键值对
        map.put(new Students("Jason",21), "zhumadian");
        map.put(new Students("John",25), "shangqiu");
        map.put(new Students("Bob", 29), "anyang");

        //取出元素的键的Set集合
        Set<Students> keySet = map.keySet();

        //通过键找值方式遍历
        for (Students key: keySet) {
            String address = map.get(key);
            System.out.print(address + "; ");//anyang; zhumadian; shangqiu; 
        }
    }
}
~~~

> 1. 当给`HashMap`中存放自定义对象时，如果自定义对象作为`Key`存在。这是要保证对象的唯一性，所以必须要重写对象的`hashCode()`和`equals()`方法。
>
> 2. 如果要保证`Map`中存放的`Key`和取出的顺序一致，可以使用`java.util.LinkedHashMap`集合来存放。

## LinkedHashMap

我们知道`HashMap`保证成对元素唯一，并且查询速度很快。可是成对元素存放进去是没有顺序的，如果我们即需要保证速度快，还要保证有序。我们就需要用到在`HashMap`下面的一个子类`LinkedHashMap`，它是链表和哈希表的一个数据存储结构。

**代码演示**

~~~java
import java.util.Map;
import java.util.HashMap;
import java.util.LinkedHashMap;
import java.util.Set;

public class Test {
    public static void main(String[] args) {
        //创建一个LinkedHashMap对象
        LinkedHashMap<Integer, String> linkedHashMap = new LinkedHashMap<>();

        //向集合中添加元素
        linkedHashMap.put(1,"Jason");
        linkedHashMap.put(2,"John");
        linkedHashMap.put(3,"Bob");

        //获取键值对对象的Set集合
        Set<Map.Entry<Integer, String>> entrySet = linkedHashMap.entrySet();

        //遍历Entry，通过entry的getKey()和getValue()方法获取键与值
        for (Map.Entry<Integer, String> entry: entrySet) {
            Integer key = entry.getKey();
            String value = entry.getValue();
            System.out.println(key + " : " + value);
            //1 : Jason
            //2 : John
            //3 : Bob
        }
    }
}
~~~

