---
title: System类
date: 2020-10-17 20:51:54
tags: 
	- Java
	- 常用类
	- 基础知识
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201017210417.jpg
---

# System类

## 概述

`java.lang.System`类中提供了大量的静态方法，可以获取与系统相关的信息或系统级操作。

## 常用方法

### currentTimeMillis()

`static long currentTimeMillis()`：返回以毫秒为单位的当前时间。

实际上，`currentTimeMillis()`方法就是获取当前系统时间与1970年01月01日00:00点之间的毫秒差值。

**代码演示**

~~~java
public class Test {
    public static void main(String[] args) {
        //获取当前系统时间毫秒值
        long time = System.currentTimeMillis();
        System.out.println(time);//1602940203176
    }
}
~~~

### arraycopy()

`static void arraycopy(Object src, int srcPos, Object dest, in destPost, int length)`：将数组中指定的数据拷贝到另一个数组中。

数组的拷贝动作是系统级的，性能很高。`System.arraycopy`方法具有5个参数，含义分别为：

| 参数序号 | 参数名称 | 参数类型 |       参数含义       |
| :------: | :------: | :------: | :------------------: |
|    1     |   src    |  Object  |        源数组        |
|    2     |  srcPos  |   int    |  源数组索引起始位置  |
|    3     |   dest   |  Object  |       目标数组       |
|    4     | destPos  |   int    | 目标数组索引起始位置 |
|    5     |  length  |   int    |     复制元素个数     |

**代码演示**

~~~java
public class Test {
    public static void main(String[] args) {
        int[] src = new int[]{1,2,3,4,5};
        int[] dest = new int[]{6,7,8,9,10};
        //将数组中指定的数据拷贝到另一个数组中。
        System.arraycopy(src,1,dest,0,3);
        //遍历数组
        for (int i: dest) {
            System.out.print(i + "; ");//2; 3; 4; 9; 10; 
        }
    }
}
~~~

