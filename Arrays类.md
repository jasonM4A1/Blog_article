---
title: Arrays类
date: 2020-10-15 15:31:46
tags: Java
---

# Arrays类

## 概述

`java.util.Arrays`此类包含用来操作数组的各种方法，比如排序和搜索等。其所有方法均为静态方法，调用起来非常简单。

## 常用方法

+ `static String toString(int[] a)`：返回指定int数组内容的字符串表示形式（long、double等数组同理）。
+ `static void sort(int[] a) `：将指定的int数组按升序排序（long、double等数组同理）。

**代码演示**

~~~java
import java.util.Arrays;

public class Test {
    public static void main(String[] args) {
        //static String toString(int[] a)`：返回指定int数组内容的字符串表示形式
        int[] ints = new int[]{97,98,99,100,101};
        String s1 = Arrays.toString(ints);
        System.out.println(s1);//[97, 98, 99, 100, 101]

        //`static void sort(int[] a) `：将指定的int数组按升序排序
        int[] ints01 = new int[]{20,19,33,56,34};
        Arrays.sort(ints01);
        for (int i: ints01) {
            System.out.print(i + "; ");//19; 20; 33; 34; 56;
        }
    }
}
~~~

