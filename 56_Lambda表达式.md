---
title: Lambda表达式
date: 2020-10-20 22:43:56
tags:
	- Java
	- 基础知识
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201021143826.jpg
---

# Lambda表达式

## 概述

相对而言，面向对象过分强调“必须通过对象的形式来做事情”，而函数式思想则尽量忽略面向对象的复杂语法——**强调做什么，而不是以什么形式做**。

+ **面向对象的思想:**

  做一件事情,找一个能解决这个事情的对象,调用对象的方法,完成事情.

+ **函数式编程思想:**

  只要能获取到结果,谁去做的,怎么做的都不重要,重视的是结果,不重视过程

## 使用前提

`Lambda`作为`JDK 8`的新特性，可以大大简化代码。但是过于简化的代码语法，往往容易造成代码歧义的问题。为了避免这个问题的产生，`SUN`公司为`Lambda`表示式的使用做出了使用约束。有以下几点：

1. `Lambda`表达式只能用于接口。

2. 使用`Lambda`表达式的接口，被要求有且仅有一个需要实现的方法。

   + 被 default 修饰的方法会有默认实现，不是必须被实现的方法，所以不影响 Lambda 表达式的使用。

3. 使用Lambda必须具有**上下文推断**

   也就是方法的参数或局部变量类型必须为Lambda对应的接口类型，才能使用Lambda作为该接口的实例。

> 有且仅有一个抽象（需要实现的）方法的接口，称为“**函数式接口**”。

## 格式

### 标准格式

~~~java
(参数类型 参数名称) -> {代码语句};
~~~

### 格式说明

+ `() `用来描述参数列、`{} `用来描述方法体、`-> `为 lambda运算符 ，读作(goes to)。
+ 除了上面的三部分，其他的都可以省略。即`new 接口()`和`重写方法的声明`都可以省略。

### 进一步省略格式

除了上述的基本Lambda标准格式的基础上，还可以进一步简化代码，规则如下：

1. 小括号内参数的类型可以省略；
2. 如果小括号内**有且仅有一个参**，则小括号可以省略；
3. 如果大括号内**有且仅有一个语句**，则无论是否有返回值，都可以省略大括号、return关键字及语句分号（必须同时省略）。

> 由于`Lambda`表达式较为复杂，想要完全掌握需要结合实际代码，多加练习，我会在下面的代码中逐一演示，供大家参考。

### 无参无返回

**定义接口**

~~~java
//具有无参无返回值方法的接口
interface NoParamNoReturn {
    public abstract void method();
}
~~~

**创建接口实现类对象**

~~~java
//使用普通的内部类，创建接口实现类对象，并调用接口中的方法
public class Test {
    public static void main(String[] args) {
        NoParamNoReturn npnr = new NoParamNoReturn() {
            @Override
            public void method(){
                System.out.println(Thread.currentThread().getName());
            }
        };
        npnr.method();//main
    }
}

//使用Lambda表达式，创建接口实现类对象，并调用接口中的方法
public class Test {
    public static void main(String[] args) {
        //除去了内部类的类名和方法名
        NoParamNoReturn npnr = () -> {
            System.out.println(Thread.currentThread().getName());
        };
        npnr.method();//main
    }
}

//在Lambda表达式的基础上，使用省略式，进一步简化
public class Test {
    public static void main(String[] args) {
        //在Lambda表达式基础上，进一步简化：
        // 去掉了{}、返回值、分号（有且仅有一个语句）.
        //省略了参数类型
        NoParamNoReturn npnr = () -> System.out.println(Thread.currentThread().getName());
        npnr.method();//main
    }
}
~~~



### 一个参数无返回

**定义接口**

~~~java
//一个参无返回值
interface NoeParamNoReturn {
    public abstract void method(String s);
}
~~~

**创建接口实现类对象**

~~~java
//使用普通的内部类，创建接口实现类对象，并调用接口中的方法
public class Test {
    public static void main(String[] args) {
        NoeParamNoReturn npnr = new NoeParamNoReturn() {
          @Override
          public void method(String s) {
              System.out.println(s);
          }
        };
        npnr.method("Jason");//Jason
    }
}

//使用Lambda表达式，创建接口实现类对象，并调用接口中的方法
public class Test {
    public static void main(String[] args) {
        //除去了内部类的类名和方法名
        NoeParamNoReturn npnr = (String s) -> {
            System.out.println(s);
        };
        npnr.method("Jason");//Jason
    }
}

//在Lambda表达式的基础上，使用省略式，进一步简化
public class Test {
    public static void main(String[] args) {
        //在Lambda表达式基础上，进一步简化：
        // 去掉了{}、返回值、分号（有且仅有一个语句）.
        //省略了参数小括号和参数类型
        NoeParamNoReturn npnr = s -> System.out.println(s);
        npnr.method("Jason");//Jason
    }
}
~~~



### 多参数无返回

**定义接口**

~~~java
//具有多参数无返回值方法的接口
interface MultiParamNoReturn {
    public abstract void method(String name, int age);
}
~~~

**创建接口实现类对象**

~~~java
//使用普通的内部类，创建接口实现类对象，并调用接口中的方法
public class Test {
    public static void main(String[] args) {
        MultiParamNoReturn mpnr = new MultiParamNoReturn() {
          @Override
          public void method(String name, int age) {
              System.out.println(name + " : " + age);
          }
        };
        mpnr.method("Jason",21);//Jason : 21
    }
}

//使用Lambda表达式简化创建接口实现类对象的代码
public class Test {
    public static void main(String[] args) {
        //除去了内部类的类名和方法名
        MultiParamNoReturn mpnr = (String name, int age) -> {
            System.out.println(name + " : " + age);
        };
        mpnr.method("Jason", 21);//Jason : 21
    }
}

//在Lambda表达式的基础上，使用省略式，进一步简化
public class Test {
    public static void main(String[] args) {
        //在Lambda表达式基础上，进一步简化：
        // 去掉了{}、返回值、分号（有且仅有一个语句）.
        //省略了参数类型
        MultiParamNoReturn mpnr = (name, age) -> System.out.println(name + " : " + age);
        mpnr.method("Jason",21);//Jason : 21
    }
}
~~~



### 无参有返回

**定义接口**

~~~java
//具有无参有返回值方法的接口
interface NoParamHaveReturn {
    public abstract boolean method();
}
~~~

**创建接口实现类对象**

~~~java
//使用普通的内部类，创建接口实现类对象，并调用接口中的方法
public class Test { 
    public static void main(String[] args) {
        NoParamHaveReturn nphr = new NoParamHaveReturn() {
          @Override
          public boolean method() {
              return true;
          }  
        };
        boolean b = nphr.method();
        System.out.println(b);//true
    }
}

//使用Lambda表达式简化创建接口实现类对象的代码
public class Test {
    public static void main(String[] args) {
        //除去了内部类的类名和方法名
        NoParamHaveReturn nphr = () -> {
            return true;
        };
        boolean b = nphr.method();
        System.out.println(b);//true
    }
}

//在Lambda表达式的基础上，使用省略式，进一步简化
public class Test {
    public static void main(String[] args) {
        //在Lambda表达式基础上，进一步简化：
        // 去掉了{}、返回值、分号（有且仅有一个语句）.
        //省略了参数类型
        NoParamHaveReturn nphr = () -> true;
        boolean b = nphr.method();
        System.out.println(b);//true
    }
}
~~~



### 一个参数有返回

**定义接口**

~~~java
//具有一个参有返回值方法的接口
interface NoeParamHaveReturn {
    public abstract boolean method(boolean b);
}
~~~

**创建接口实现类对象**

~~~java
//使用普通的内部类，创建接口实现类对象，并调用接口中的方法
public class Test {
    public static void main(String[] args) {
        NoeParamHaveReturn nphr = new NoeParamHaveReturn() {
          @Override
          public boolean method(boolean b) {
              return b;
          }
        };
        boolean b = nphr.method(true);
        System.out.println(b);//true
    }
}

//使用Lambda表达式简化创建接口实现类对象的代码
public class Test {
    public static void main(String[] args) {
        //除去了内部类的类名和方法名
        NoeParamHaveReturn nphr = (boolean b) -> {
            return true;
        };
        boolean b = nphr.method(true);
        System.out.println(b);//true
    }
}

//在Lambda表达式的基础上，使用省略式，进一步简化
public class Test {
    public static void main(String[] args) {
        //在Lambda表达式基础上，进一步简化：
        // 去掉了{}、返回值、分号（有且仅有一个语句）.
        //省略了参数小括号和参数类型
        NoeParamHaveReturn nphr = b -> b;
        boolean b = nphr.method(true);
        System.out.println(b);//true
    }
}
~~~



### 多个参数有返回

**定义接口**

~~~java
//具有多个参数有返回值方法的接口
interface MultiParamHaveReturn {
    public abstract boolean method(int one, int two);
}
~~~

**创建接口实现类对象**

~~~java
//使用普通的内部类，创建接口实现类对象，并调用接口中的方法
public class Test {
    public static void main(String[] args) {
        MultiParamHaveReturn mphr = new MultiParamHaveReturn() {
          @Override
          public boolean method(int one, int two) {
              return one == two;
          }
        };
        boolean b = mphr.method(1,2);
        System.out.println(b);//false
    }
}

//使用Lambda表达式简化创建接口实现类对象的代码
public class Test {
    public static void main(String[] args) {
        //除去了内部类的类名和方法名
        MultiParamHaveReturn mphr = (int one, int two) -> {
            return one == two;
        };
        boolean b = mphr.method(1,2);
        System.out.println(b);//false
    }
}

//在Lambda表达式的基础上，使用省略式，进一步简化
public class Test {
    public static void main(String[] args) {
        //在Lambda表达式基础上，进一步简化：
        // 去掉了{}、返回值、分号（有且仅有一个语句）.
        //省略了参数类型
        MultiParamHaveReturn mphr = (one, two) -> one == two;
        boolean b = mphr.method(1,2);
        System.out.println(b);//false
    }
}
~~~





