---
title: Junit测试
date: 2020-11-08 21:39:52
tags:
	- Java
	- 测试
	- 基础知识
categoreis:
	- Java
	- 复习
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201108214236.jpg
---

# Junit测试

## 概述

测试分为两类：

1. 黑盒测试：不需要写代码，给输入值，看程序是否能够输出期望的值。
2. 白盒测试：需要写代码的，关注程序具体的执行流程。

## 使用

由于黑盒测试不太涉及代码层级的东西，所以我们主要讲解的是白盒测试的使用步骤。如下：

1. 定义一个测试类

   + 包名建议：xxx.xxx.xxx.test，例：top.liboshuai.person.test
   + 测试类名建议：被测试的类名 + test，例：methodTest

2. 定义一个测试方法

   + 方法名建议：test + 测试的方法名，例：testMethod()
   + 返回值建议：建议设置为`void`，因为这个测试方法只是我们自己使用，不需要别人去调用。

3. 给测试方法加注解@Test

   + 在Java中只有`main`方法是程序的执行入口，我们的测试方法不是`main`方法，不能直接执行。所以我们需要给测试方法加上注解@Test，达到是测试方法可以独立运行的目的。

4. 导入`Junit`工程依赖。

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201111184120.png)

**代码演示**

+ 被测试代码

  ~~~java
  package top.liboshuai.study;
  
  public class Calculator {
      //加法
      public int add(int x, int y) {
          return x + y;
      }
      //减法
      public int sub(int x, int y) {
          return x - y;
      }
  }
  ~~~

+ 测试

  ~~~java
  package top.liboshuai.Test;
  
  import org.junit.Assert;
  import org.junit.Test;
  import top.liboshuai.study.Calculator;
  
  public class CalculatorTest {
      //测试add方法
      @Test
      public void testAdd() {
          Calculator calc = new Calculator();
          int result = calc.add(1,2);
          //断言结果为3，实际结果也为3
          Assert.assertEquals(3,result);
      }
      //测试sub方法
      @Test
      public void testSub() {
          Calculator calc = new Calculator();
          int result = calc.sub(2,1);
          //断言结果为0，实际结果为1
          Assert.assertEquals(0,result);
  
          //抛出异常：
          //java.lang.AssertionError:
          //Expected :0
          //Actual   :1
      }
  }
  ~~~

## 判定结果

代码若编译上正确，控制台则会为绿色。若有错误，则会为红色。我们可以通过这点来初步判断被测试程序的正确与否。

程序的结果正确与否，我们打印出来也是不能人工判断出来的。所以我们可以使用【断言】——`Assert.assertEquals(断定结果, 程序得出的结果)`，来判定这个结果。

如下：

~~~java
package top.liboshuai.Test;

import org.junit.Assert;
import org.junit.Test;
import top.liboshuai.study.Calculator;

public class CalculatorTest {
    @Test
    public void testAdd() {
        //创建一个被测试类对象
        Calculator calc = new Calculator();

        //调用被测试类方法
        int result = calc.add(1,2);

        //使用断言
        Assert.assertEquals(3,result);
    }
}
~~~

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201111204647.png)

## 两个测试常用的注解

1. `@Before`： 修饰的方法总会在测试方法之前被自动执行，一般我们会在这里进行一些初始化的操作
2. `@After`：修饰的方法总会在测试方法执行之后自动被执行，一般我们可以在这里进行一些关闭资源的操作。以避免测试代码有错误，影响到资源的关闭。

**代码演示**

```java
package top.liboshuai.Test;

import org.junit.After;
import org.junit.Assert;
import org.junit.Before;
import org.junit.Test;
import top.liboshuai.study.Calculator;

public class CalculatorTest {
    //一般我们会在这里做一些初始化的操作
    @Before
    public void init() {
        System.out.println("开始进行初始化...");
    }
    //测试sub()
    @Test
    public void testSub() {
        //手动制造一个错误，测试注解@Before @After
        int x =2;
        int i = x / 0;

        Calculator calc = new Calculator();
        int result = calc.sub(0,1);
        //断言结果为-1，实际结果也为-1
        Assert.assertEquals(-1,result);
    }

    //我们可以在这里关闭测试用到的一些资源
    @After
    public void close() {
        System.out.println("关闭资源中...");
    }
}
```

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201111204805.png)

可以看到，即使测试代码中有出错的，也不影响后面注释代码的执行。