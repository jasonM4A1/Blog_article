---
title: Thread类
date: 2020-10-19 21:34:10
tags:
	- Java
	- 线程
	- 基础知识
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201019213509.jpg
---

# Thread类

## 概述

Thread类用于操作线程，是所以涉及到线程操作(如并发)的基础。

## 构造方法

+ `Thread()`：分配一个新的线程对象。
+ `Thread(String name)`：分配一个指定名字的新线程对象。
+ `Thread(Runnable target)`：分配一个带有指定目标的新线程对象。
+ `Thread(Runnable target, String name)`：分配一个带有指定目标的新线程对象，并为其指定名字。

**代码演示**

> 需要演示的代码实在过长，还请私下自行练习吧~

## 常用方法

+ `String getName()`：获取当前线程的名称。
+ `void setName()`：设置当前线程的名称。
+ `void start()`：：启动线程，同时`Java`虚拟机开始调用此线程的`run()`方法。
+ `void run()`：在此编写此线程将要执行的代码语句。
+ `static void sleep(long millis)`：使当前线程以指定的毫秒数暂停。
+ `static Thread currentThread()`：获取当前正在执行的线程对象的引用。

**代码演示**

~~~java
class MyThread extends Thread {
    @Override
    // void run()：在此编写此线程将要执行的代码语句。
    public void run() {
        for (int i = 0; i < 1000; i++) {
            //String getName()：获取当前线程的名称。
            System.out.println(getName() + "------->" + i);
            //static void sleep(long millis)：使当前线程以指定的毫秒数暂停。
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

public class Test {
    public static void main(String[] args) {
        //创建线程对象
        MyThread mt = new MyThread();

        //void setName()：设置当前线程的名称。
        mt.setName("myThread");

        //public void start()：启动线程，同时`Java`虚拟机开始调用此线程的`run()`方法
        mt.start();

        //主线程中的for循环
        for (int i = 0; i < 1000; i++) {
            //public static Thread currentThread()：获取当前正在执行的线程对象的引用
            System.out.println(Thread.currentThread().getName() + "-------->" + i);
        }
    }
}
~~~

