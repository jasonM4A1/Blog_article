---
title: 字节流-OutputStream
date: 2020-10-22 13:59:52
tags:
	- Java
	- IO
	- 基础知识
categories:
	- Java
	- 复习
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201022140111.jpg
---

# 字节流-OutputStream

> 一切文件数据(文本、图片、视频等)在存储时，都是以二进制数字的形式保存，都一个一个的字节，那么传输时一样如此。所以，字节流可以传输任意文件数据。在操作流的时候，我们要时刻明确，无论使用什么样的流对象，底层传输的始终为二进制数据。

## OutputStream抽象类

`java.io.OutputStream`抽象类是表示字节输出流的所有类的超类，将指定的字节信息写出到目的地。它定义了字节输出流的基本共性功能方法。

+ `void close()`：关闭此输出流并释放与此流相关联的任何系统资源。  
+ `void flush()`：刷新此输出流并强制任何缓冲的输出字节被写出。  
+ `void write(byte[] b)`：将指定的字节数组写入此输出流。  
+ `void write(byte[] b, int off, int len)`：写入字符数组的某一部分，从数组索引`off`开始,写入`len`长度的字符。 
+ `abstract void write(int b)`：将指定的字节写入此输出流。

## FileOutputStream类

`OutputStream`有很多子类，我们从最简单的一个子类开始。

`java.io.FileOutputStream `类是文件输出流，用于将数据写出到文件。

**继承图**

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201023034527.png)

### 构造方法

+ `FileOutputStream(File file, boolean append)`：通过传入`File`对象，来创建一个`FileOutputStream`对象。
+ `FileOutputStream(String name, boolean append)`：通过传入`String`类文件路径名，来创建一个`FileOutputStream`对象。

> 当你创建一个流对象时，必须传入一个文件路径。该路径下：
>
> + 如果没有这个文件，则会创建该文件。
> + 如果有这个文件，apped默认为false，则会清空这个文件的数据。如果不想清空文件的数据，则可以把append设置为true，就会在文件后面追加。

**代码演示**

~~~java
import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;

public class Test {
    public static void main(String[] args) throws IOException {
        File file = new File("a.txt");
        //使用File对象，创建字节流对象
        FileOutputStream fos = new FileOutputStream(file);

		//使用文件名称，创建字节流对象
        FileOutputStream fos1 = new FileOutputStream("a.md");
    }
}
~~~

### 常用方法

+ `void write(int b)`：将指定的一个字节写入此输出流。
+ `void write(byte[] b)`：将指定的字节数组写入此输出流。  
+ `void write(byte[] b, int off, int len)`：写入字符数组的某一部分,从数组索引`off`开始,写入`len`长度的字符。 

**代码演示**

`void write(int b)`：将指定的一个字节写入此输出流。

~~~java
import java.io.IOException;
import java.io.File;
import java.io.FileOutputStream;

public class Test {
    public static void main(String[] args) throws IOException {
        //使用file对象创建一个FileOutputStream对象
        File file = new File("a.txt");
        FileOutputStream fos = new FileOutputStream(file);

        //使用write()写入硬盘文件数据
        fos.write(97);
        fos.write(98);
        fos.write(99);

        //关闭资源
        fos.close();
    }
}
~~~

`void write(byte[] b)`：将指定的字节数组写入此输出流。  

~~~java
import java.io.FileOutputStream;
import java.io.IOException;

public class Test {
    public static void main(String[] args) throws IOException {
        //使用文件名称创建流对象
        FileOutputStream fos = new FileOutputStream("a.txt");

        //将字符串转换为字节数组
        byte[] b = "Jason".getBytes();

        //将指定的字节数组写入此输出流
        fos.write(b);

        //关闭资源
        fos.close();
    }
}
~~~

`void write(byte[] b, int off, int len)`：将写入字符数组的某一部分,从数组索引`off`开始,写入`len`长度的字符。 

~~~java
import java.io.IOException;
import java.io.FileOutputStream;
import java.io.File;

public class Test {
    public static void main(String[] args) throws IOException {
        //使用File对象创建FileOutputStream对象
        FileOutputStream fos = new FileOutputStream(new File("a.txt"));

        //将字符串转为byte数组
        byte[] bytes = "public static void main(String[] args)".getBytes();

        //将byte数组从指定索引，写入指定长度的byte数据到输出流
        fos.write(bytes,7,6);

        //关闭资源
        fos.close();
    }
}
~~~

> 1. 虽然参数为int类型四个字节，但是只会保留一个字节的信息写出。
> 2. 流操作完毕后，必须释放系统资源，调用close方法，千万记得。

## 写出换行

我们直接写入数据是没有换行的，在Windows系统里，换行符号`\r\n`。我们可以手动把换行符号加进入。

~~~java
import java.io.IOException;
import java.io.FileOutputStream;

public class Test {
    public static void main(String[] args) throws IOException {
        // 使用文件名称创建流对象
        FileOutputStream fos = new FileOutputStream("a.txt", true);

        //定义字节数组
        byte[] bytes = {97,98,99,100,101};

        //将换行写入输出流
        fos.write("\r\n".getBytes());

        //将byte数组写入输出流
        fos.write(bytes);

        //将换行写入输出流
        fos.write("\r\n".getBytes());

        //关闭资源
        fos.close();
    }
}
~~~

> 1. 回车符`\r`和换行符`\n` ：
>    + 回车符：回到一行的开头（return）。
>    + 换行符：下一行（newline）。
> 2. 不同系统中换行符的区别：
>    + Windows系统里，每行结尾是 `回车+换行` ，即`\r\n`；
>    + Unix系统里，每行结尾只有 `换行` ，即`\n`；
>    + Mac系统里，每行结尾是 `回车` ，即`\r`。从 Mac OS X开始与Linux统一。

