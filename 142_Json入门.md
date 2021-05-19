---
title: Json入门
date: 2021-03-05 22:03:53
tags:
	- Java
	- JavaWeb
	- Json
categories:
	- Java
---

# Json概述

JSON (JavaScript Object Notation) 是一种轻量级的数据交换格式。易于人阅读和编写。同时也易于机器解析和生成。JSON 采用完全独立于语言的文本格式，而且很多语言都提供了对 json 的支持（包括 C, C++, C#, Java, JavaScript, Perl, Python 等）。 这样就使得 JSON 成为理想的数据交换格式。

> 轻量级是与xml做比较
>
> 数据交换指是客户端和服务器之间业务数据的传递格式

# Json在JavaScript中的使用

## 定义Json

json 是由键值对组成，并且由花括号（大括号）包围。每个键由引号引起来，键和值之间使用冒号进行分隔， 多组键值对之间进行逗号进行分隔。

```javascript
let jsonObj = {
  "one": 1,
  "two": "second",
  "three": true,
  "four": [2, "third", false],
  "five": {
    "five_01": 3,
    "five_02": "hello",
  },
  "six": [
    {
      "six_01": "world",
      "six_02": 4
    },
    {
      "six_03": "Java",
      "six_04": 5
    }
  ]
};
```

## 访问Json

```javascript
let one = jsonObj.one;
document.write("one: " + one + "<br/>");

let two = jsonObj.two;
document.write("two: " + two + "<br/>");

let three = jsonObj.three;
document.write("three: " + three + "<br/>")

let four = jsonObj.four;
document.write("four: " + four + "<br/>");
for (let i = 0; i < four.length; i++) {
  document.write("four[" + i + "]: " + four[i] + "<br/>");
}

let five = jsonObj.five;
document.write("five: " + five + "<br/>");
let five_01 = five.five_01;
document.write("five_01: " + five_01 + "<br/>");
let five_02 = five.five_02;
document.write("five_02: " + five_02 + "<br/>");

let six = jsonObj.six;
document.write("six: " + six + "<br/>");
let six_01 = six[0].six_01;
document.write("six_01: " + six_01 + "<br/>")
let six_03 = six[1].six_03;
document.write("six_03: " + six_03 + "<br/>");
```

## Json对象转字符串

```javascript
let jsonObjString = JSON.stringify(jsonObj);
document.write(jsonObjString + "<br/>");
```

## 字符串转Json对象

```javascript
let jsonObj01 = JSON.parse(jsonObjString);
document.write(jsonObj01 + "<br/>");
document.write(jsonObj01.one + "<br/>")
```

# Json在Java中的使用

## JavaBean与Json互转

先创建一个JavaBean类

```java
package pojo;

import java.util.Objects;

public class Person {
    private Integer id;
    private String name;

		//有惨、无参构造函数
  
  	//setter and getter
  
  	//hashCode and equals
  
  	//toString
}
```

再写一个测试类，测试JavaBean与Json的互转

```java
package json;

import com.google.gson.Gson;
import com.google.gson.reflect.TypeToken;
import org.junit.Test;
import pojo.Person;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class JsonTest {

    //JavaBean与Json互转
    @Test
    public void test01() {
        //创建JavaBean对象
        Person jason = new Person(01, "Jason");

        //创建Gson对象
        Gson gson = new Gson();

        //toJson方法把Java对象转换为json字符串
        String jsonString = gson.toJson(jason);
        System.out.println("json字符串：" + jsonString);

        //fromJson方法把json字符串转换回Java对象
        Person person = gson.fromJson(jsonString, Person.class);
        System.out.println("JavaBean对象：" + person);
    }
}
```

## List与Json互转

先创建一个`TypeToken`的子类，范型为`ArrayList<Person>`，用于后面的类型转换

```java
package json;

import com.google.gson.reflect.TypeToken;
import pojo.Person;

import java.util.ArrayList;

public class PersonListType extends TypeToken<ArrayList<Person>> {

}
```

再写一个测试类，测试List与Json的互转

```java
package json;

import com.google.gson.Gson;
import com.google.gson.reflect.TypeToken;
import org.junit.Test;
import pojo.Person;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class JsonTest {
    //List与Json互转
    @Test
    public void test02() {
        //创建一个ArrayList集合，并添加两个Person对象
        List<Person> jsonList = new ArrayList<>();
        jsonList.add(new Person(02,"BernardoLi"));
        jsonList.add(new Person(03,"bob"));

        //创建Gson对象
        Gson gson = new Gson();

        //toJson方法把List集合转换为Json字符串
        String jsonString = gson.toJson(jsonList);
        System.out.println("List集合转换成的Json字符串：" + jsonString);

        //fromJson方法把json字符串转换回List集合
        Object o = gson.fromJson(jsonString, new PersonListType().getType());
        List<Person> jsonList02 = (List<Person>) o;
        System.out.println("json字符串转换为List集合：" + jsonList02);
    }
}
```

## Map与Json互转

这一次我们准备采用匿名内部类的方式进行类型转换，所以不需要创建`TypeToken`的子类，直接创建用于测试Map与Json互转的Java测试类

```java
package json;

import com.google.gson.Gson;
import com.google.gson.reflect.TypeToken;
import org.junit.Test;
import pojo.Person;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class JsonTest {
    //map与Json互转
    @Test
    public void test03() {
        //创建一个HashMap()集合，并添加两个Person对象
        Map<String,Person> map =  new HashMap<>();
        map.put("one", new Person(04, "site"));
        map.put("two",new Person(05,"deploy"));

        //创建Gson对象
        Gson gson = new Gson();

        //toJson方法把Map集合转换为json字符串
        String s = gson.toJson(map);
        System.out.println("Map集合转换为json字符串：" + s);

        //fromJson方法把json字符串转换为Map集合（采用匿名内部类）
        Object o = gson.fromJson(s, new TypeToken<HashMap<String, Person>>() {
        }.getType());
        Map<String, Person> jsonMap = (HashMap<String, Person>) o;
        System.out.println("json字符串转换为Map集合：" + jsonMap);
    }
}
```

