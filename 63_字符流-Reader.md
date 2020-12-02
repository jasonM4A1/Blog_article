---
title: 字符流-Reader
date: 2020-10-23 03:33:38
tags:
	- Java
	- IO
	- 基础知识
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201023033532.jpg
---

# 字符流-Reader

## Reader抽象类

`java.io.Reader`抽象类是表示用于读取字符流的所有类的超类，可以读取字符信息到内存中。它定义了字符输入流的基本共性功能方法。

+ `void close()`：关闭此流并释放与此流相关联的任何系统资源。    
+ `int read()`：从输入流读取一个字符。 
+ `int read(char[] cbuf)`：从输入流中读取一些字符，将它们存储到字符数组 cbuf中 。并返回读取到的有效字符个数。

## FileReader类

### 概述

`java.io.Reader`抽象类下有众多子类，这里我们使用它的间接子类`FileReader`来进行举例讲解。

`java.io.FileReader`类是读取字符文件的便利类。构造时使用系统默认的字符编码和默认字节缓冲区。

**继承图**

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201023032249.png)

### 构造方法

+ `FileReader(File file)`： 通过给定要读取的File对象，来创建一个新的`FileReader` 。
+ `FileReader(String fileName)`：通过给定要读取的文件的String类型的名称，来创建一个新的`FileReader`。

> 文件不存在，则会抛出`java.io.FileNotFoundException`异常。

**代码演示**

~~~java
import java.io.IOException;
import java.io.FileReader;
import java.io.File;

public class Test {
    public static void main(String[] args) throws IOException {
        //传入一个File对象，来创建一个FileReader对象
        FileReader fr = new FileReader(new File("a.txt"));

        //传入一个String类路径名，来创建一个FileReader对象
        FileReader fr02 = new FileReader("b.txt");
    }
}
~~~

### 常用方法

+ `int read()`：每次读取一个字符，并提示为`int`类型。读取到文件末尾处，则返回`-1`。
+ `int read(char[] cbuf)`：读取一些字符，将它们存储到字符数组 cbuf中 ，并返回读取到的有效字符个数。
+ `String getEncoding()`：返回此流使用的字符编码的名称。

**代码演示**

`int read()`：每次读取一个字符，并提示为`int`类型。读取到文件末尾处，则返回`-1`。

~~~java
import java.io.IOException;
import java.io.FileReader;

public class Test {
    public static void main(String[] args) throws IOException {
        //创建一个FileReader对象
        FileReader fr = new FileReader("a.txt");

        //定义用于接收读取到的字符的变量
        int value;
        //每次读取一个字符
        while ((value = fr.read()) != -1) {
            System.out.println((char) value);
        }

        //关闭资源
        fr.close();
    }
}
~~~

`int read(char[] cbuf)`：读取一些字符，将它们存储到字符数组 cbuf中 ，并返回读取到的有效字符个数。

~~~java
import java.io.IOException;
import java.io.FileReader;

public class Test {
    public static void main(String[] args) throws IOException {
        //创建一个FileReader对象
        FileReader fr = new FileReader("D:\\study\\html_css_javaScipt\\untitled\\a.txt");

        //定义一个用于接收读取到的字符的字符数组
        char[] chars = new char[2];
        //定义一个用于接收读取到有效字符个数的变量
        int b;
        //读取一些字符，将它们存储到字符数组中
        while ((b = fr.read(chars)) != -1) {
            System.out.println(new String(chars,0,b));
        }
        
        //关闭资源
        fr.close();
    }
}
~~~

`String getEncoding()`：返回此流使用的字符编码的名称。

~~~java
import java.io.IOException;
import java.io.FileReader;
import java.io.File;

public class Test {
    public static void main(String[] args) throws IOException {
        //创建一个File对象
        File file = new File("D:\\study\\html_css_javaScipt\\untitled","a.txt");

        //传入File对象，来创建一个FileReader对象
        FileReader fr = new FileReader(file);

        //获取此流的字符编码
        String encoding = fr.getEncoding();
        System.out.println(encoding);//UTF8

        //关闭资源
        fr.close();
    }
}
~~~

## 课后练习：文本文件的复制

**案例实现**

~~~java
import java.io.IOException;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.File;

/**
 * 完成一个makeDown文件的复制
 */
public class Test {
    public static void main(String[] args) throws IOException {
        //创建被复制的文件的File对象
        File file = new File("D:\\study\\Java\\node\\heima","28-Collections.md");
        //创建被复制后的文件的File对象
        File file_bak = new File("D:\\pictures","28-Collections_bak.md");

        //创建被复制的文件的FileReader对象
        FileReader fr = new FileReader(file);
        //创建被复制后的文件的FileWriter对象
        FileWriter fw = new FileWriter(file_bak);

        //定义接收读取的有效字符个数的变量
        int b;
        //定义用于接收读取到的字符的字符数组
        char[] chars = new char[2];

        //进行文本文件的copy
        while ((b = fr.read(chars)) != -1) {
            fw.write(chars,0,b);
        }

        //刷新缓冲流，并关闭资源
        fw.close();
        fr.close();
    }
}
~~~



