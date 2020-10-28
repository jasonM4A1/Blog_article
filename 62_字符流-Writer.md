---
title: 字符流-Writer
date: 2020-10-22 20:52:52
tags:
	- Java
	- IO
	- 基础知识
categories:
	- Java
	- 复习
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201022205427.jpg
---

> 当使用字节流读取文本文件时，可能会有一个小问题。就是遇到中文字符时，可能不会显示完整的字符，那是因为一个中文字符可能占用多个字节存储。所以Java提供一些字符流类，以字符为单位读写数据，专门用于处理文本文件。

# 字符流-Writer

## Writer抽象类

`java.io.Writer `抽象类是表示用于写出字符流的所有类的超类，将指定的字符信息写出到目的地。它定义了字节输出流的基本共性功能方法。

+ `void write(int c)`：写入单个字符。
+ `void write(char[] cbuf)`：写入指定字符数组。 
+ `abstract void write(char[] cbuf, int off, int len)`：写入字符数组的某一部分,从数组索引`off`开始,写入`len`长度的字符。 
+ `void write(String str)`：写入字符串。 
+ `void write(String str, int off ,int len)`：写入字符串的某一部分,从数组索引`off`开始,写入`len`长度的字符串。 
+ `void flush()`：将数据从缓冲区刷新到文件中，流对象可以被再次使用。
+ `void close()`：自动将数据从缓冲区刷新到文件中，但会通知系统释放、关闭资源，流对象不可以被再次使用。

## FileWrite类

### 概述

`java.io.Writer`有众多子类，我们使用它的间接子类`FileWriter`类来举例讲解。

`java.io.FileWriter `类是写出字符到文件的便利类。构造时使用系统默认的字符编码和默认字节缓冲区。

**继承图**

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201023041956.png)

### 构造方法

+ `FileWriter(File file)`：通过给定一个要读取的`File`对象，来创建一个新的`FileWriter`对象。
+ `FileWriter(String fileName)`：通过给定一个要读取的`String`类的文件路径，来创建一个新的`FileWriter`对象。
+ `FileWriter(File file, boolean append`：通过给定一个要读取的`File`对象，来创建一个新的`FileWriter`对象。通过`append`控制是否追加文件。
+ `FileWriter(String fileName, boolean append)`：通过给定一个要读取的`String`类的文件路径，来创建一个新的`FileWriter`对象。通过`append`控制是否追加文件。

> `boolean append`为追加开关，为`false`时，每次创建`FileWriter`对象时，都会创建一个新的空文件，来覆盖原文件。为`true`时，则不会覆盖原文件，而是在原文件后追加。不赋值的情况下，默认为`false`。

**代码演示**

~~~java
import java.io.IOException;
import java.io.FileWriter;
import java.io.File;

public class Test {
    public static void main(String[] args) throws IOException {
        //通过给定一个`File`对象，来创建一个新的`FileWriter`对象。
        FileWriter fw = new FileWriter(new File("D:\\study\\html_css_javaScipt\\untitled","a.txt"));

        //通过给定一个要读取的`String`类的文件路径，来创建一个新的`FileWriter`对象。
        FileWriter fw2 = new FileWriter("D:\\study\\html_css_javaScipt\\untitled\\b.txt");
    }
}
~~~

### 常用方法

+ `void write(int c)`：写入单个字符。
+ `void write(char[] cbuf)`：写入指定字符数组。 
+ `void write(char[] cbuf, int off, int len)`：写入字符数组的某一部分,从数组索引`off`开始,写入`len`长度的字符。 
+ `write(String str)`：写入指定的字符串。
+ `write(String str, int off, int len)`：写入字符串的某一部分,从字符串索引`off`开始,写入`len`长度的字符串。 
+ `void flush()`：将数据从缓冲区刷新到文件中，流对象可以被再次使用。
+ `void close()`：自动将数据从缓冲区刷新到文件中，但会通知系统释放、关闭资源，流对象不可以被再次使用。

> 1. 即便是调用了`flush()`方法写出了数据，操作的最后还是要调用`close()`方法，释放系统资源。
>
> 2. 字符流，只能操作文本文件，不能操作图片，视频等非文本文件。
>
>    当我们单纯读或者写文本文件时  使用字符流 其他情况使用字节流

**代码演示**

`void write(int c)`：写入单个字符。

~~~java
import java.io.IOException;
import java.io.FileWriter;
import java.io.File;

public class Test {
    public static void main(String[] args) throws IOException {
        //通过传入File对象，来创建一个FileWriter对象
        File file = new File("D:\\study\\html_css_javaScipt\\untitled","a.txt");
        FileWriter fw = new FileWriter(file);

        //传入int类型数据每次写入一个字符
        fw.write(25105);
        fw.write(26159);
        fw.write(26446);
        fw.write(21338);
        fw.write(24069);

        //关闭资源
        fw.close();
    }
}
~~~

`void write(char[] cbuf)`：写入指定字符数组。 

~~~java
import java.io.IOException;
import java.io.FileWriter;

public class Test {
    public static void main(String[] args) throws IOException {
        //通过传入文件路径名，创建一个FileWriter对象
        FileWriter fw = new FileWriter("D:\\study\\html_css_javaScipt\\untitled\\b.txt",true);

        //将字符串转换为字符数组
        char[] chars = "我是李博帅".toCharArray();

        //写入指定的字符数组
        fw.write(chars);

        //关闭资源
        fw.close();
    }
}
~~~

`void write(char[] cbuf, int off, int len)`：写入字符数组的某一部分,从数组索引`off`开始,写入`len`长度的字符。 

~~~java
import java.io.IOException;
import java.io.FileWriter;
import java.io.File;

public class Test {
    public static void main(String[] args) throws IOException {
        //通过传入一个File对象，来创建FileWriter对象
        File file = new File("D:\\study\\html_css_javaScipt\\untitled","a.txt");
        FileWriter fw = new FileWriter(file, true);

        //将字符串转化为char[]
        char[] line = "\r\n".toCharArray();//加入换行符
        char[] chars = "--我是王多鱼--".toCharArray();

        //写入字符数组的一部分
        fw.write(line);
        fw.write(chars,2,5);

        //关闭资源
        fw.close();
    }
}
~~~

`write(String str)`：写入指定的字符串。

~~~java
import java.io.IOException;
import java.io.FileWriter;
import java.io.File;

public class Test {
    public static void main(String[] args) throws IOException {
        //通过传入一个File对象，来创建FileWriter对象
        File file = new File("D:\\study\\html_css_javaScipt\\untitled", "a.txt");
        FileWriter fw = new FileWriter(file, true);

        //写入字符串
        fw.write("\r\n");
        fw.write("你能不能不要再说话了！");

        //关闭资源
        fw.close();
    }
}
~~~

`write(String str, int off, int len)`：写入字符串的某一部分,从字符串索引`off`开始,写入`len`长度的字符串。 

~~~java
import java.io.IOException;
import java.io.FileWriter;

public class Test {
    public static void main(String[] args) throws IOException {
        //通过传入一个String类文件路径名，来创建一个FileWriter对象
        FileWriter fw = new FileWriter("b.txt", false);

        //写入部分字符串
        fw.write("-\r\n11",1,2);
        fw.write("其实我还好",2,3);

        //关闭资源
        fw.close();
    }
}
~~~

`void flush()`：将数据从缓冲区刷新到文件中，流对象可以被再次使用。

`void close()`：自动将数据从缓冲区刷新到文件中，但会通知系统释放、关闭资源，流对象不可以被再次使用。

~~~java
import java.io.IOException;
import java.io.FileWriter;
import java.io.File;

public class Test {
    public static void main(String[] args) throws IOException {
        //创建一个File对象
        File file = new File("a.txt");;
        //传入一个File对象，来创建一个FileWriter对象
        FileWriter fw = new FileWriter(file, true);

        //将字符串转化为char数组
        char[] line = "\r\n".toCharArray();
        char[] chars = "闫老板是个大垃圾".toCharArray();
        //写入部分的char数组
        fw.write(line);
        fw.write(chars);

        //刷新缓冲区，流对象还可以被再次使用
        fw.flush();

        //尝试着刷新后，再次写入一个String对象
        fw.write("\r\n不管明天的路有多漫长");//写入数据成功

        //关闭资源
        fw.close();

        //尝试着关闭后，再次写入一个String对象
        fw.write("\r\n我再次启航,带着我的勋章");//异常：java.io.IOException: Stream closed
    }
}
~~~

