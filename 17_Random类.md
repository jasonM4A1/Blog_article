---
title: Random类
date: 2020-10-15 13:07:49
tags: 
	- 常用类
	- Random
	- Java
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201017133156.jpg
---

# Random类

## 概述

`java.util.Random`**用于生成一个伪随机数**。通过算法产生的随机数都是伪随机！

只有通过真实的随机事件产生的随机数才是真随机，比如，通过机器的硬件噪声产生随机数、通过大气噪声产生的随机数。

## 构造方法

+ `Random()`：创建一个新的随机数生成器。
+ `Random(long seed)`：使用一个long类型的种子创建一个新的随机数生成器。

构造方式的代码演示与下面的成员方法的构造方法一同测试。

## 成员方法

+ `boolean nextBoolean()`：返回这个随机数生成器序列中的下一个伪随机、均匀分布的boolean值。
+ `void nextBytes(byte[] bytes)`：生成随机字节并将其放入用户提供的byte[]中。
+ `int nextInt()`：返回这个随机数生成器序列中的下一个伪随机、均匀分布的int值
+ `int nextInt(int bound)`：返回从这个随机数生成器的序列中[0, bound)之间均匀分布的伪随机int值。
+ `long nextLong()`：返回这个随机数生成器序列的下一个均匀分布的伪随机long值。
+ `double nextDouble()`：返回这个随机数生成器序列中下一个在0.0到1.0之间均匀分布的伪随机double值。
+ `float nextFloat()`：返回这个随机数生成器序列中的下一个伪随机数的在0.0到1.0之间均匀分布的float值。

**代码演示**

~~~java
package review;

import java.util.Random;

public class Test {
    public static void main(String[] args) {
        Random r = new Random();
        int one = r.nextInt();
        int two = r.nextInt(10);
        System.out.println(one);//14391882
        System.out.println(two);//5
    }
}
~~~

