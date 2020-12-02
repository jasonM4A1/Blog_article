---
title: File类
date: 2020-10-21 17:46:02
tags:
	- Java
	- IO流
	- 基础知识
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201021174935.jpg
---

# File类

## 概述

`java.io.File` 类是文件和目录路径名的抽象表示，主要用于文件和目录的创建、查找和删除等操作。

## 构造方法

+ `File(String pathname)`：通过将给定的**路径名字符串**转换为抽象路径名来创建新的 File实例
+ `File(String parent, String child)`：从**父路径名字符串和子路径名字符串**创建新的 File实例。
+ `File(File parent, String child)`：从**父抽象路径名和子路径名字符串**创建新的 File实例。  

**代码演示**

~~~java
import java.io.File;

public class Test {
    public static void main(String[] args) {
        //通过将给定的路径名字符串转换为抽象路径名来创建新的File实例。
        File file1 = new File("a.txt");//相对路径
        File file2 = new File("D:\\study\\html_cs_javaScipt\\untitled\\JDBC\\src\\b.txt");//绝对路径

        //从父路径名字符串和子路径名字符串创建新的File实例。
        File file3 = new File("D:\\study\\html_cs_javaScipt\\untitled\\JDBC\\src",
                "c.txt");

        //从父抽象路径名和子路径名字符串创建新的File实例。  
        File file4 = new File(new File("D:\\study\\html_cs_javaScipt\\untitled\\JDBC\\src"),
                "d.txt");
    }
}
~~~

> 1. 一个File对象代表硬盘中实际存在的一个文件或者目录。
> 2. 无论该路径下是否存在文件或者目录，都不影响File对象的创建。

## 常用方法

### 获取的方法

+ `String getAbsolutePath()`：返回此File的绝对路径名字符串。
+ `String getPath()`：将此File转换为路径名字符串（可以是绝对，也可以相对）。 
+ `String getName()`：返回由此File表示的文件或目录的名称。  
+ `long length()`：返回由此File表示的文件的大小。 

**代码演示**

~~~java
import java.io.File;

public class Test {
    public static void main(String[] args) {
        //创建一个File对象
        File file1 = new File("a.txt");

        //返回此File对象的绝对路径名字符串。
        String absolutePath = file1.getAbsolutePath();
        System.out.println("文件的绝对路径：" + absolutePath);
        //文件的绝对路径：D:\study\html_css_javaScipt\untitled\a.txt

        //将此File转换为路径名字符串。
        String path = file1.getPath();
        System.out.println("文件的路径：" + path);//文件的路径：a.txt

        //返回由此File表示的文件或目录的名称。
        String name = file1.getName();
        System.out.println("文件的名字：" + name);//文件的名字：a.txt

        //返回由此File表示的文件的大小
        long length = file1.length();
        System.out.println("文件的大小：" + length);//文件的大小：31554
    }
}
~~~

> 文件夹本身是没有大小的概念的，所以使用`long length()`获取文件夹的大小，只会得到0。

### 判断的方法

+ `boolean exists()`：判断此File对象表示的文件或目录是否实际存在。
+ `boolean isDirectory()`：判断此File对象表示的是否为目录。
+ `boolean isFile()`：判断此File对象表示的是否为文件。

**代码演示**

~~~java
import java.io.File;

public class Test {
    public static void main(String[] args) {
        //使用一个文件路径，创建一个File对象
        File file1 = new File("a.txt");
        //使用一个文件夹路径，创建一个File对象d
        File file2 = new File(".");//.表示当前目录

        //判断此File对象表示的文件或目录是否实际存在
        boolean b1 = file1.exists();
        System.out.println("b1File对象是否存在：" + b1);//b1File对象是否存在：true
        boolean b2 = file2.exists();
        System.out.println("b2File对象是否存在：" + b2);//b2File对象是否存在：true

        //判断此File对象表示的是否为目录
        boolean b3 = file1.isDirectory();
        System.out.println("b1File对象是否为目录：" + b3);//b1File对象是否为目录：false
        boolean b4 = file2.isDirectory();
        System.out.println("b2File对象是否为目录：" + b4);//b2File对象是否为目录：true

        //判断此File对象示的是否为文件
        boolean b5 = file1.isFile();
        System.out.println("b1File对象是否为文件：" + b5);//b1File对象是否为文件：true
        boolean b6 = file2.isFile();
        System.out.println("b2File对象是否为文件：" + b6);//b2File对象是否为文件：false
    }
}
~~~

### 删除的方法

+ `boolean createNewFile()`：如果文件不存在，则创建此文件，同时返回`true`。如果此文件存在，则不作为，同时返回`false`。
+ `boolean mkdir()`：创建由此File表示的单层目录。创建成功，返回true。创建失败，返回false。 
+ `boolean mkdirs()`：创建由此File表示的多层目录。创建成功，返回true。创建失败，返回false。 
+ `boolean delete()`：删除由此File表示的文件或目录（目录必须为空）。 删除成功，返回true。删除失败，返回false。 

**代码演示**

~~~java
import java.io.File;
import java.io.IOException;

public class Test {
    public static void main(String[] args) throws IOException {
        //创建一个硬盘已经存在实体文件的File文件对象
        File file = new File("D:\\study\\Java\\code","a.txt");
        //创建一个硬盘上不存在实体文件的File文件对象
        File file1 = new File("D:\\study\\Java\\code","b.txt");
        //创建一个单层目录File对象
        File file2 = new File("D:\\study\\Java\\code","Human");
        //创建一个多层目录File对象
        File file3 = new File("D:\\study\\Java\\code","Animal\\Cat\\OrangeCat");

        //如果文件不存在，则创建此文件，同时返回true。如果此文件存在，则不作为，同时返回false。
        System.out.println(file.createNewFile() ? "a.txt文件创建成功" : "a.txt文件创建失败");//a.txt文件创建失败
        System.out.println(file1.createNewFile() ? "b.txt文件创建成功" : "b.txt文件创建失败");//b.txt文件创建成功

        //创建由此File表示的单层目录。创建成功，返回true。创建失败，返回false。
        System.out.println(file2.mkdir() ? "单层目录Human创建成功" : "单层Human创建失败");//单层目录Human创建成功
        System.out.println(file3.mkdir() ? "多层目录Animal\\Cat\\OrangeCat创建成功" : "多层目录Animal\\Cat\\OrangeCat创建失败");//多层目录Animal\Cat\OrangeCat创建失败，因为mkdir()只能创建单层目录

        //创建由此File表示的多层目录。创建成功，返回true。创建失败，返回false。
        System.out.println(file3.mkdirs() ? "多层目录Animal\\Cat\\OrangeCat成功" : "多层目录Animal\\Cat\\OrangeCat失败");//多层目录Animal\Cat\OrangeCat成功

        //删除由此File表示的文件或目录（目录必须为空）。 创建成功，返回true。创建失败，返回false。
        System.out.println(file1.delete() ? "b.txt文件删除成功" : "b.txt文件删除失败");//b.txt文件删除成功
        System.out.println(file2.delete() ? "单层目录Human删除成功" : "单层Human删除失败");//单层目录Human删除成功
        System.out.println(new File("Animal").delete() ? "多层目录Animal删除成功" : "多层目录Animal删除失败");//多层目录Animal删除失败,因为Animal目录下面不为空，还有其他目录
    }
}
~~~

### 遍历的方法

+ `String[] list()`：返回一个String数组，表示该File目录中的所有子文件或目录。
+ `File[] listFiles()`：返回一个File数组，表示该File目录中的所有的子文件或目录。  

**代码演示**

~~~java
import java.io.File;

public class Test {
    public static void main(String[] args) {
        File file1 = new File("D:\\study\\Java\\doc\\黑马Java\\02-Java语言进阶\\day01_Object类、常用API");

        String[] filesString = file1.list();
        for (String file: filesString) {
            System.out.println(file);
            //code
            //day01【Object类、常用API】-笔记.md
            //resource
        }

        File[] filesList = file1.listFiles();
        for (File file: filesList){
            System.out.println(file);
            //D:\study\Java\doc\黑马Java\02-Java语言进阶\day01_Object类、常用API\code
            //D:\study\Java\doc\黑马Java\02-Java语言进阶\day01_Object类、常用API\day01【Object类、常用API】-笔记.md
            //D:\study\Java\doc\黑马Java\02-Java语言进阶\day01_Object类、常用API\resource
        }
    }
}
~~~

