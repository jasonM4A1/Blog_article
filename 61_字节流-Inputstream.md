---
title: 字节流-Inputstream
date: 2020-10-22 15:46:31
tags:
	- Java
	- IO
	- 基础知识
categories:
	- Java
	- 复习
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201022154828.jpg
---

# 字节流-Inputstream

## 概述

`java.io.InputStream `抽象类是表示字节输入流的所有类的超类，可以读取字节信息到内存中。它定义了字节输入流的基本共性功能方法。

+ `void close()`：关闭此输入流并释放与此流相关联的任何系统资源。    
+ `abstract int read()`：从输入流读取数据的下一个字节。 
+ `int read(byte[] b)`：从输入流中读取一些字节数，并将它们存储到字节数组中 。并返回获取的有效的字节个数。

## FileInputStream类

### 概述

`java.io.InputStream`有很多子类，我们以它的直接子类`FileInputStream`为例进行讲解。

`java.io.FileInputStream `类是文件输入流，从文件中读取字节。

**继承图**

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201023032709.png)

### 构造方法

+ `FileInputStream(File file)`：通过打开与实际文件的连接来创建一个`FileInputStream`对象，该文件由文件系统中的File对象来命名。 
+ `FileInputStream(String name)`： 通过打开与实际文件的连接来创建一个`FileInputStream`，该文件由文件系统中的路径名命名。

> 若传入的文件路径不存在，则会抛出`java.io.FileNotFoundException`异常。

**代码演示**

~~~java
import java.io.IOException;
import java.io.FileInputStream;
import java.io.File;

public class Test {
    public static void main(String[] args) throws IOException {
        //传入File对象创建一个FileInputStream对象
        FileInputStream fis1 = new FileInputStream(new File("a.txt"));

        //传入String路径名创建一个FileInputStream对象
        FileInputStream fis2 = new FileInputStream("b.txt");
    }
}
~~~

### 常用方法

+ `int read()`：从输入流读取数据的一个字节，并提升为`int`类型。读取到文件末尾，返回`-1`。
+ `int read(byte[] b)`：每次读取b长度个字节到数组中，返回读取到的有效字节个数，读取到末尾时，返回`-1` 。

**代码演示**

`int read()`：从输入流读取数据的一个字节，并提升为`int`类型。读取到文件末尾，返回`-1`。

~~~java
import java.io.IOException;
import java.io.FileInputStream;

public class Test {
    public static void main(String[] args) throws IOException {
        // 创建FileInputStream对象
        FileInputStream fis = new FileInputStream("a.txt");

        // 定义变量，保存数据
        int i;
        // 读取数据，返回一个字节
        while ((i = fis.read()) != -1) {
            System.out.print((char) i + "; ");//a; b; c; 
        }

        //关闭资源
        fis.close();
    }
}
~~~

`int read(byte[] b)`：每次读取b长度个字节到数组中，返回读取到的有效字节个数，读取到末尾时，返回`-1` 。

~~~java
import java.io.IOException;
import java.io.FileInputStream;

public class Test {
    public static void main(String[] args) throws IOException {
        //创建FileInputStream
        FileInputStream fis = new FileInputStream("a.txt");

        //定义变量，作为有效个数
        int length;
        //定义一个长度为2的字节数组，用作保存数据的容器
        byte[] bytes = new byte[2];
        //读取b长度个字节到byte数组中
        while ((length = fis.read(bytes)) != -1) {
            // 每次读取后,把数组变成字符串打印
            System.out.println(new String(bytes,0,length));
        }

        //关闭资源
        fis.close();
    }
}
~~~

> 1. 使用`int read(byte[] b)`时要注意，在最后一次读取时，可能出现上次读取的数据不能被完全覆盖，而产生错误的数据。所以我们需要用`length`接收`int read(byte[] b)`获得有效个数，来避免这种情况。
>
> 2. 使用数组读取，每次读取多个字节，减少了系统间的IO操作次数，从而提高了读写的效率，建议开发中使用。

 ## 课后练习：图片复制

**复制原理图解**

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201022203954.jpg)

**案例实现**

~~~java
import java.io.IOException;
import java.io.File;
import java.io.FileOutputStream;
import java.io.FileInputStream;

public class Test {
    public static void main(String[] args) throws IOException {
        //创建File对象
        File filesSrc = new File("D:\\pictures\\压缩后","one.jpg");
        File filesDest = new File("D:\\study\\Java\\node\\heima","one_bak.jpg");

        //创建字节流对象
        FileInputStream fis = new FileInputStream(filesSrc);
        FileOutputStream fos = new FileOutputStream(filesDest);

        //定义存储数据的byte数组
        byte[] bytes = new byte[1024];
        //定义接收获取的有效个数变量
        int length;

        //进行图片的复制
        while ((length = fis.read(bytes)) != -1) {
            fos.write(bytes,0,length);
        }

        //关闭资源
        fis.close();
        fos.close();
    }
}
~~~

