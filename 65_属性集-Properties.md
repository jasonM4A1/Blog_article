---
title: 属性集-Properties类
date: 2020-10-23 21:05:40
tags:
	- Java
	- IO
	- 集合
	- 基础知识
categories:
	- Java
	- 复习
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201023210756.jpg
---

# 属性集-Properties类

## 概述

`java.util.Properties`继承于`Hashtable`，来表示一个持久的属性集。它使用键值结构存储数据，每个键及其对应值都是一个字符串。该类也被许多Java类使用，比如获取系统属性时，`System.getProperties` 方法就是返回一个`Properties`对象。

**继承图**

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201023211136.png)

## 构造方法

+ `Properties()`：创建一个没有默认值的空属性列表。

**代码演示**

~~~java
import java.util.Properties;

public class Test {
    public static void main(String[] args) {
        //创建一个没有属性值的空属性列表
        Properties pro1 = new Properties();
    }
}
~~~

## 常用方法

### 存储的方法

+ `Object setProperty(String key, String value)`：调用Hashtable方法put，强制将字符串作为属性键和值保存，返回结果为调用Hashtable方法put的结果。
+ `String getProperty(String key)`：在此属性列表中搜索具有指定键的属性。如果在此属性列表中没有找到该键，则递归检查默认属性列表及其默认值。如果找不到属性值，则返回`null`。
+ `Set<String> stringPropertyNames()`：所有键的名称的集合。

**代码演示**

~~~java
import java.util.Properties;
import java.util.Set;

public class Test {
    public static void main(String[] args) {
        //创建属性集对象
        Properties pro = new Properties();

        //添加键值对到属性集中
        pro.setProperty("fileName", "a.txt");
        pro.setProperty("length", "125");
        pro.setProperty("location", "D:\\D:\\study\\html_css_javaScipt\\untitled\\a.txt");

        //打印属性集对象，验证键值对是否加入到属性集中
        System.out.println(pro);
        System.out.println("--------------------------------------");

        //通过键，获取属性值
        String key1 = pro.getProperty("fileName");
        String key2 = pro.getProperty("length");
        String key3 = pro.getProperty("location");
        System.out.println(key1);
        System.out.println(key2);
        System.out.println(key3);
        System.out.println("--------------------------------------");

        //获取所有键的Set集合
        Set<String> keySet = pro.stringPropertyNames();

        //通过遍历键的Set集合，来获取遍历属性值
        for (String key: keySet) {
            String value = pro.getProperty(key);
            System.out.println(value);
        }
        System.out.println("--------------------------------------");
    }
}
~~~

### 与IO流相关的方法

+ `void load(InputStream inStream)`：从输入字节流中读取属性列表(键和元素对)。

  参数中使用了字节输入流，通过流对象关联到某文件上，这样就能够加载文本中的数据到属性集中了。

  **文本数据格式:**

  ~~~java
  //key=value
  fileName=a.txt
  length=124
  location=D:\\D:\\study\\html_css_javaScipt\\untitled\\a.txt
  ~~~

  > 文本中的数据，必须是键值对形式，可以使用空格、等号、冒号等符号分隔。

**代码演示**

~~~java
import java.util.Set;
import java.util.Properties;
import java.io.FileInputStream;
import java.io.IOException;

public class Test {
    public static void main(String[] args) {
        //创建一个属性集对象
        Properties properties = new Properties();

        //加载文本文件中的数据到属性集中
        FileInputStream fis = null;
        try {
            fis = new FileInputStream("a.txt");
            properties.load(fis);
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (fis != null) {
                    fis.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

        //获取属性集的键的Set集合
        Set<String> keySet = properties.stringPropertyNames();

        //遍历属性集的键，来获取属性集的值
        for (String key: keySet) {
            String value = properties.getProperty(key);
            System.out.println(value);
        }
    }
}
~~~



