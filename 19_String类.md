---
title: String类
date: 2020-10-15 13:24:18
tags: 
	- Java
	- String类
	- 常用类
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201017133332.jpg
---

# String类

##  概述

`java.lang.String`类代表字符串。java程序中所有的字符串文字（例如：`"abc"`）都可以被看作是实现此类的实例。

`String`类中包括用于检查各个字符串的方法，比如用于**比较**字符串、**搜索**字符串、**提取**字符串以及创建具有翻译为**大写**或**小写**的所有字符的字符串的副本。

### 不变性

字符串的值在被创建后不能被更改。

~~~java
package review;

public class Test {
    public static void main(String[] args) {
        String s1 = "jason";
        s1 += "Spring";
        System.out.println(s1);//jasonSpring
        //表面上只有两个字符串，其实内存中有三个字符串，分别是"jason"、"Spring"、"jasonSpring"。、
        // s1一开始指向"jason"，后来指向"jason"、"Spring"拼接而成的"jasonSpring"。
    }
}
~~~

### 共享性

因为String对象是不可变的，所以它们可以被共享。

~~~java
String s1 = "jason";
String s2 = "jason";
//内存中只用一个"jason"被创建，同时指向s1和s1俩个变量。
~~~

### 底层实现

String底层是靠字符数组实现的。

~~~java
package review;

public class Test {
    public static void main(String[] args) {
        String s1 = "jason";
        //等效于
        char data[] = {'j','a','s','o','n'};
        String s2 = new String(data);

        System.out.println(s1);//jason
        System.out.println(s2);//jason
    }
}
~~~

## 构造方法

+ `String()`：初始化新创建的字符串对象，使其表示空字符序列。
+ `String(byte[] bytes)`：使用平台的默认字符集对指定的字节数组进行解码，构造一个新字符串。
+ `String(char[] value)`：分配一个新字符串，以便它表示字符数组参数中当前包含的字符序列。

**代码演示**

~~~java
package review;

public class Test {
    public static void main(String[] args) {
        String s1 = new String();
        System.out.println(s1);//【空】

        byte[] bytes = new byte[]{97,98,99};
        String s2 = new String(bytes);
        System.out.println(s2);//abc

        char[] chars = new char[]{'j','a','s','o','n'};
        String s3 = new String(chars);
        System.out.println(s3);//jason
    }
}
~~~

## 成员方法

### 判断的方法

+ `boolean equals(Object anObject)`：将此字符串与指定的对象进行比较。
+ `boolean equalsIgnoreCase(String anotherString)`：将此字符串与另一个字符串进行比较，忽略大小写因素。
+ `boolean isEmpty()`：判断字符串是否为空。

~~~java
package review;

public class Test {
    public static void main(String[] args) {
        //通过字面量创建字符串
        String s1 = "jason";
        String s2 = "jason";
        String s3 = "Jason";

        //`boolean equals(Object anObject)`：
        // 将此字符串与指定的对象进行比较。
        boolean one = s1.equals(s2);
        System.out.println(one);//true
        boolean two = s1.equals(s3);
        System.out.println(two);//false

        //`boolean equalsIgnoreCase(String anotherString)`：
        // 将此字符串与另一个字符串进行比较，忽略大小写因素。
        boolean three = s1.equalsIgnoreCase(s2);
        System.out.println(three);//true
    }
}
~~~

### 获取的方法

+ `int length()`：返回该字符串的长度。
+ `String concat(String str)`：将指定的字符串连接到此字符串的末尾。
+ `char charAt(int index)`：返回指定索引处的字符值。
+ `int indexOf(String str)`：返回此字符串中指定子字符串的第一个匹配项的索引。
+ `String substring(int beginIndex)`：返回一个字符串，该字符串是该字符串的子字符串。子字符串从指定索引处的字符开始，并扩展到该字符串的末尾。
+ `String substring(int beginIndex, int endIndex)`：回一个字符串，该字符串是该字符串的子字符串。子字符串从指定的beginIndex开始，并扩展到索引endIndex - 1处的字符。

**代码演示**

~~~java
package review;

public class Test {
    public static void main(String[] args) {
        //创建一个字符串对象
        String s1 = "jason";

        //`int length()`：返回该字符串的长度。
        int one = s1.length();
        System.out.println(one);//5

        //`String concat(String str)`：将指定的字符串连接到此字符串的末尾。
        String s2 = s1.concat("Spring");
        System.out.println(s2);//jasonSpring

        //`char charAt(int index)`：返回指定索引处的字符值。
        char two = s1.charAt(4);
        System.out.println(two);//n

        //`int indexOf(String str)`：返回此字符串中指定子字符串的第一个匹配项的索引。
        int three = s1.indexOf("a");
        System.out.println(three);

        //`String substring(int beginIndex)`：
        // 返回一个字符串，该字符串是该字符串的子字符串。
        // 子字符串从指定索引处的字符开始，并扩展到该字符串的末尾。
        String four = s1.substring(2);
        System.out.println(four);//son

        //`String substring(int beginIndex, int endIndex)`：
        // 返回一个字符串，该字符串是该字符串的子字符串。
        // 子字符串从指定的beginIndex开始，并扩展到索引endIndex - 1处的字符。
        String five = s1.substring(1,4);
        System.out.println(five);//aso
    }
}
~~~

### 转换的方法

+ `char[] toCharArray()`：将此字符串转换为新的字符数组。
+ `byte[] getBytes()`：使用平台的默认字符集将该字符串编码为字节序列，并将结果存储到新的字节数组中。
+ `char[] toCharArray()`：将此字符串转换为新的字符数组。
+ `String toLowerCase()`：使用默认语言环境的规则将此字符串中的所有字符转换为小写。
+ `String toUpperCase()`：使用默认语言环境的规则将此字符串中的所有字符转换为大写。
+ `Static valueOf(int i)`：返回int参数的字符串表示形式，其他类型类似。
+ `String replace(CharSequence target, CharSequence replacement)`：将此字符串中与文字目标序列匹配的每个子字符串替换为指定的文字替换序列。

**代码演示**

~~~java
package review;

public class Test {
    public static void main(String[] args) {
        //创建一个字符串对象
        String s1 = "jason";

        //`char[] toCharArray()`：将此字符串转换为新的字符数组。
        char[] one = s1.toCharArray();
        for (char c: one) {//使用foreace遍历char[]数组
            System.out.print(c + "; ");//j; a; s; o; n;
        }
        System.out.println();

        //`byte[] getBytes()`：
        // 使用平台的默认字符集将该字符串编码为字节序列，并将结果存储到新的字节数组中。
        byte[] two = s1.getBytes();
        for (byte b: two) {
            System.out.print(b + "; ");//106; 97; 115; 111; 110;
        }
        System.out.println();

        //`String replace(CharSequence target, CharSequence replacement)`：
        // 将此字符串中与文字目标序列匹配的每个子字符串替换为指定的文字替换序列。
        String three = s1.replace("n","m");
        System.out.println(three);//jasom
    }
}
~~~

### 分割的方法

+ `String[] split(String regex)`：将此字符串拆分为与给定正则表达式匹配的字符串。
+ `String trim[]`：返回值为此字符串的字符串，并删除前导和末尾的空白。

**代码演示**

~~~java
package review;

public class Test {
    public static void main(String[] args) {
        //创建一个字符串
        String s1 = "M4A1-S";

        //`String[] split(String regex)`：
        // 将此字符串拆分为与给定正则表达式匹配的字符串。
        String[] one = s1.split("-");
        for (String s: one) {
            System.out.print(s + "; ");//M4A1; S; 
        }
    }
}
~~~



