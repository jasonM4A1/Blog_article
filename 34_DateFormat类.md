---
title: DateFormat类
date: 2020-10-17 19:41:28
tags:
	- Java
	- 常用类
	- 基础知识
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201017194253.jpg
---

# DateFormat类

## 概述

`java.text.DateFormat`是日期/时间格式化子类的抽象父类，我们通过这个类可以帮我们完成日期和文本之间的转换，也就是可以在`Date`对象与`String`对象之间进行来回转换。

> **格式化**：按照指定的格式，从`Date`对象转换为`String`对象。
>
> **解析**：按照指定的格式，从`String`对象转换为`Date`对象。

## 构造方法

由于`DateFormat`为抽象类，不能直接使用，所以需要用到它的子类`java.text.SimpleDateFormat`。这个类需要一个模式（格式）来指定格式化或解析的标准。

`java.text.SimpleDateFormat`的构造方法为：

+ `public SimpleDateFormat(String pattern)`：用给定的模式和默认语言环境的日期格式符号构造`SimpleDateFormat`对象。

> 参数`pattern`是一个字符串，代表日期时间的自定义格式。

### pattern格式规则

| 标识字母 | 含义 |
| :------: | :--: |
|    y     |  年  |
|    M     |  月  |
|    d     |  日  |
|    H     |  时  |
|    m     |  分  |
|    s     |  秒  |
|    S     | 毫秒 |

> 备注：更详细的格式规则，可以参考`SimpleDateFormat`类的`API`文档。

**代码演示**

~~~java
DateFormat format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss SSS");
~~~

## 常用方法

+ `String format(Date date)`：将Date对象格式化为字符串。
+ `Date parse(String source)`：将字符串解析为Date对象。（需要进行异常处理）

**代码演示**

~~~java
import java.text.ParseException;
import java.util.Date;
import java.text.DateFormat;
import java.text.SimpleDateFormat;

public class Test {
    public static void main(String[] args) {
        //使用当前时间创建一个日期对象
        Date d1 = new Date();
        //创建一个SimpleDateFormat对象
        DateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        //调用format()方式格式化传入的日期对象
        String timeString = sdf.format(d1);
        System.out.println(timeString);

        //定义一个表达日期的字符串
        String DateString = "1999年08月22日";
        //创建一个SimpleDateFormat对象
        DateFormat sdf2 = new SimpleDateFormat("yyyy年MM月dd日");
        //调用parse()方式解析传入的字符串
        try {
            Date d2 = sdf2.parse(DateString);
        } catch (ParseException e) {
            e.printStackTrace();
        }
    }
}
~~~

> 西方周日是一周的开始，用数字0表示。
>
> 在`Calendar`类中，月份的表示是0-11代表1-12月。