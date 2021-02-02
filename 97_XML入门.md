---
title: XML入门 
date: 2020-12-05 16:24:53
tags:
	- 前端
    - XML
categories:
	- 前端
cover:
    https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210129042150.jpg
--- 

# XML简介

## 什么是XML？

xml 是可扩展的标记性语言，和我们的HTML是兄弟。

## XML的作用

1. 用来保存数据， 而且这些数据具有自我描述性
2. 它还可以做为项目或者模块的配置文件
3. 还可以做为网络传输数据的格式（现在 JSON 为主）

# XML初体验

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--
    上面一行为XML声明，是必须存在的
-->
<books><!--books表示多个图书信息-->
    <book sn="1010"><!--book表示一个图书信息，sn表示序列号-->
        <name>那些比拼命努力更重要的事</name><!--name表示书名-->
        <author>乔治•维兰特</author><!--author表示作者-->
        <price>32</price><!--price表示价格-->
    </book>
    <booo sn="1011">
        <name>Java从入门到入土</name>
        <author>Jason</author>
        <price>99.80</price>
    </booo>
</books>
```

# XML语法

## 声明

XML声明用来表示这个文件是xml文件，是必不可少的，不能出错的。不然浏览器就无法解析这个xml文件。

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
```

+ `version`：xml版本号
+ `encoding`：xml文件采用的编码方式
+ `standalone="yes/no"`：设置这个xml文件是否独立

## 注释

xml的注释与html完成一致。

```xml
<!--这是xml注释-->
```

## 元素（标签）

### 什么是xml元素？

元素是指从开始标签到结束标签的内容。

### xml命名规则

+ 名称可以含字母、 数字以及其他的字符
+ 名称不能以数字或者标点符号开始
+ 名称不能包含空格

### 单标签和双标签

xml 中的元素（ 标签） 也 分成 单标签和双标签。

+ 单标签：<标签名 属性=”值” 属性=”值” ...... />
+ 双标签：< 标签名 属性=”值” 属性=”值” ......>文本数据或子标签</标签名>

```xml
<!--单标签-->
<human name="Jason" age="21"/>

<!--双标签-->
<person name="bob" age="22">
    其实我是王多鱼
</person>
```

## 属性

xml 的标签属性和 html 的标签属性是非常类似的， 属性可以提供元素的额外信息。

一个标签上可以书写多个属性，每个属性的值必须使用 引号 引起来，与html的标签书写规则一致。

## 文本区域（CDATA区）

有的时候，我们需要在xml写一些纯文本的内容。这个时候我们不希望自己写的文本被xml解析，就可以使用`CDATA`来实现。

```xml
<![CDATA[纯文本内容]]>
```

## 其他语法规定

+ 所有 XML 元素都须有关闭标签（也就是闭合）
+ XML 标签对大小写敏感
+ XML 必须正确地嵌套
+ XML 文档必须有根元素，且仅只能有一个（没有父标签的元素，被称为跟元素）
+ XML 的属性值须加引号
+ XML 中想要显示`<`、`>`等符号，我们必须使用字符实体（如`lt;`、`gt;`等）

# 约束

## 概述

规定XML文档的书写规则。

## 分类

1. `DTD`：一种简单的约束技术
2. `Schema`：一种复杂的约束技术

## DTD

### 内部DTD

```
<!DOCTYPE 根标签 [DTD约束内容]>
```

### 外部DTD

- 本地DTD

  ```
  <!DOCTYPE 根标签名 SYSTEM "dtd文件的位置">
  ```

- 网络DTD

  ```
  <!DOCTYPE 根标签名 PUBLIC "dtd文件名" "dtd文件url">
  ```

## Schema

1. 填写xml文档的根元素
2. 引入xsi前缀. xmlns:xsi=”http://www.w3.org/2001/XMLSchema-instance"
3. 引入xsd文件命名空间. xsi:schemaLocation=”http://www.itcast.cn/xml student.xsd”
4. 为每一个xsd约束声明一个前缀,作为标识 xmlns=”http://www.itcast.cn/xml"

```
<students   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
				xmlns="http://www.itcast.cn/xml"
				xsi:schemaLocation="http://www.itcast.cn/xml  student.xsd">
```

# XML解析

## 概述

操作xml文档，将文档中的数据读取到内存中。

1. 解析：将文档中的数据读取到内存中
2. 写入：将内存中的数据保存到xml文档中。持久化的存储

## 解析xml的方式

### DOM

将标记语言文档一次性加载进内存，在内存中形成一颗dom树

- 优点：操作方便，可以对文档进行CRUD的所有操作
- 缺点：占内存

### SAX

逐行读取，基于事件驱动的。

- 优点：不占内存。
- 缺点：只能读取，不能增删改。

## XML常见解析器

1. `JAXP`：sun公司提供的解析器，支持dom和sax两种思想
2. `DMO4J`：一款非常优秀的解析器
3. `PULL`：Android操作系统内置的解析器，sax方式的
4. `Jsoup`：一款Java 的HTML解析器，可直接解析某个URL地址、HTML文本内容。它提供了一套非常省力的API，可通过DOM，CSS以及类似于jQuery的操作方法来取出和操作数据。

# Jsoup解析器

## 快速入门

### 步骤

1. 导入jar包
2. 获取Document对象
3. 获取对应的标签Element对象
4. 获取数据

### 代码

```
package top.liboshuai.study;

import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;

import java.io.File;
import java.io.IOException;


public class Demo {
    public static void main(String[] args) throws IOException {
        //获取student.xml的路径
        String path = Demo.class.getClassLoader().getResource("student.xml").getPath();
        //解析xml文档，加载文档进内存，获取dom树
        Document document = Jsoup.parse(new File(path), "utf-8");
        //获取元素对象集合
        Elements elements = document.getElementsByTag("name");
        //获取第一个name的Element对象
        Element element = elements.get(0);
        //获取标签名
        String name = element.text();
        System.out.println(name);//tom
    }
}
```

## 对象的使用

1. ```
   Jsoup
   ```

   ：工具类，调用

   ```
   parse()
   ```

   可以解析html或xml文档，返回Document

   - `parse(File in, String charsetName)`：解析xml或html文件的。
   - `parse(String html)`：解析xml或html字符串
   - `parse(URL url, int timeoutMillis)`：通过网络路径获取指定的html或xml的文档对象

2. ```
   Document
   ```

   ：文档对象。代表内存中的dom树。通过下列方法来获取Element对象

   - `getElementById(String id)`：据id属性值获取唯一的element对象
   - `getElementsBytag(String tagName)`：根据标签名称获取元素对象集合
   - `getElementsByAttribute(String key)`：根据属性名称获取元素对象集合
   - `getElementsByAttributeValue(String key, String value)`：根据对应的属性名和属性值获取元素对象集合

3. `Elements`：元素Element对象的集合。可以当做 ArrayList来使用

4. ```
   Element
   ```

   ：元素对象。

   - `getElementById(String id)`：根据id属性值获取唯一的element对象
   - `getElementsByTag(String tagName)`：根据标签名称获取元素对象集合
   - `getElementsByAttribute(String key)`：根据属性名称获取元素对象集合
   - `getElementsByAttributeValue(String key, String value)`：根据对应的属性名和属性值获取元素对象集合

5. 获取属性值

   - `String attr(String key)`：根据属性名称获取属性值

6. 获取文本内容

   - `String text()`：获取文本内容
   - `String html()`：获取标签体的所有内容（包括子标签的字符串内容）

7. ```
   Node
   ```

   ：节点对象

   - 是Document和Element的父类

## 快捷查询方法

### select选择器

- 使用方法：`Elements select(String cssQuery)`
- 语法：参考Selector类中定义的语法

### XPath

XPath即为XML路径语言，它是一种用来确定XML（标准通用标记语言的子集）文档中某部分位置的语言

- 使用Jsoup的Xpath需要额外导入jar包。
- 查询w3cshool参考手册，使用xpath的语法完成查询

```
//1.获取student.xml的path
String path = JsoupDemo6.class.getClassLoader().getResource("student.xml").getPath();
//2.获取Document对象
Document document = Jsoup.parse(new File(path), "utf-8");
//3.根据document对象，创建JXDocument对象
JXDocument jxDocument = new JXDocument(document);

        //4.结合xpath语法查询
        //4.1查询所有student标签
        List<JXNode> jxNodes = jxDocument.selN("//student");
        for (JXNode jxNode : jxNodes) {
            System.out.println(jxNode);
        }

        System.out.println("--------------------");

        //4.2查询所有student标签下的name标签
        List<JXNode> jxNodes2 = jxDocument.selN("//student/name");
        for (JXNode jxNode : jxNodes2) {
            System.out.println(jxNode);
        }

        System.out.println("--------------------");

        //4.3查询student标签下带有id属性的name标签
        List<JXNode> jxNodes3 = jxDocument.selN("//student/name[@id]");
        for (JXNode jxNode : jxNodes3) {
            System.out.println(jxNode);
        }
        System.out.println("--------------------");
        //4.4查询student标签下带有id属性的name标签 并且id属性值为itcast

        List<JXNode> jxNodes4 = jxDocument.selN("//student/name[@id='itcast']");
        for (JXNode jxNode : jxNodes4) {
            System.out.println(jxNode);
        }
```

# Dom4j解析器

## 下载并导入Dom4j的jar包

由于 dom4j 它不是 sun 公司的技术，而属于第三方公司的技术，我们需要使用 dom4j 就需要到 dom4j 官网下载 dom4j 的 jar 包。

> 我这里提供了快速下载通道：
>
> https://wws.lanzous.com/iNo0xl8stcd	密码:e07m

下载上面的压缩包并解压，然后将根目录中的jar包导入到Idea的项目中。关于的Jar包的如何导入，可以查看我的另一篇文章[如何使用idea2020.2导入jar包](https://www.kuangstudy.com/bbs/1356635551163219969)

## 使用Dom4j解析XML

### Dom4j编程步骤

1. 创建SAXReader类对象
2. 通过SAXReader类对象调用`read("xml文件路径")`方法，获取xml文件的Document对象
3. 通过Document对象调用`getRootElement()`方法，获取Document对象的根标签元素
4. 通过根标签元素调用`elements("子标签名")`方法，获取根标签元素的子标签元素对象们，并储存到List集合
5. 遍历储存子标签元素对象们的List集合，以此来取出每个子标签元素
   + 如果子标签自身还有子标签，还可以通过调用`elements("子标签名")`方法继续取下一级子元素。如果已经是最后一级，则进行第六步
6. 通过子标签元素调用`attributeValue("属性名")`方法，获取子标签自身的属性值的字符串形式
7. 通过子标签元素调用`getText("标签名")`方法，获取子标签自身的文本内容的字符串形式
8. 当然我们也可以通过父标签调用`elementText("子标签名")`的方法，来直接获取子标签中的文本内容。而不需要先获取子标签，再获取其文本内容。作用等于第6步+第7步。

### Dom4j编程常用类和方法

+ `SAXReader`类：根据SAX解析事件创建DOM4J树
+ `SAXReader`类的`Document reade(URL url)`：从给定的文档中读取文档
+ `Document`接口：定义一个XML文档
+ `Docuemnt`接口的`Element getRootElement()`：获取Document对象的根元素。
+ `Element`接口的`List elements()`：获取调用者元素中的所有子元素
+ `Element`接口的`Element element(String name)`：获取调用者元素中的指定名字的子元素
+ `Element`接口的`String attributeValue(String name)`：获取调用者元素中的指定名字的属性值
+ `Element`接口的`String getText()`：获取调用者元素中的文本内容
+ `Element`接口中的`String elementText(String name)`：直接获取其指定名字的子元素的文本内容，无需遍历子元素

## Dom4j解析XML案例实战

### 案例要求

使用Dom4j解析一个XML文件，并将其XML文件中的内容封装到一个java自定义类中。最后打印验证，其是否封装成功。

> 案例所需的Jar包，我都提供了资源。
>
> 测试框架Junit-4的jar包：https://wws.lanzous.com/i89Jal8suuh	密码:23fu
>
> Junit-4依赖的hamcrest-core的jar包：https://wws.lanzous.com/iraUMl8rogb	密码:8hab

### 案例实现

首先创建一个名为`Humans.xml`的文件，文件内容如下：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<Human>
    <person id="001">
        <name>Jason</name>
        <age>21</age>
        <gender>male</gender>
    </person>
    <person id="002">
        <name>Bob</name>
        <age>22</age>
        <gender>female</gender>
    </person>
</Human>
```

创建一个`Human.java`文件，用于封装`Human.xml`的内容。如下：

```java
import java.util.Objects;

public class Human {
    private int id;
    private String name;
    private int age;
    private String gender;

    public Human() {
    }

    public Human(int id, String name, int age, String gender) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.gender = gender;
    }

    //setter and getter

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getGender() {
        return gender;
    }

    public void setGender(String gender) {
        this.gender = gender;
    }

    @Override
    public String toString() {
        return "Human{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", age=" + age +
                ", gender='" + gender + '\'' +
                '}';
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Human human = (Human) o;
        return id == human.id &&
                age == human.age &&
                Objects.equals(name, human.name) &&
                Objects.equals(gender, human.gender);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id, name, age, gender);
    }
}
```

最后创建一个`HumanText.java`文件，用于测试实现`Human`类对`Humans.xml`文件内容的封装。如下：

```java
import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;
import org.junit.Test;

import java.util.List;

public class HumanTest {

    //测试注解
    @Test
    public void test02() throws Exception {
        //1.创建SAXReader类对象
        SAXReader reader = new SAXReader();
        //2.获取Humans.xml的文档对象(在Junit测试中，相对路径是从模块开始)
        Document document = reader.read("XML/Humans.xml");
        //3.通过Document对象获取根元素
        Element rootElement = document.getRootElement();
        //4.通过根元素获取person标签对象
        List<Element> persons = rootElement.elements("person");
        //5.遍历每个person标签对象
        for (Element person: persons
             ) {
            //5.1 通过person标签获取其子标签name标签对象
            Element name = person.element("name");
            //5.1.1 获取name标签的文本内容
            String nameText = name.getText();

            //5.2 通过person标签获取其子标签age标签对象
            Element age = person.element("age");
            //5.2.1 获取age标签的文本内容
            String ageText = age.getText();
            //5.2.2 因为Human类的age属性类型为int，所以要转类型
            int ageNum = Integer.parseInt(ageText);

            //5.3 通过person标签获取其子标签gender标签对象
            Element gender = person.element("gender");
            //5.3.1 获取gender标签的文本内容
            String genderText = gender.getText();

            //5.4 通过person标签获取其自身的id属性字符串形式
            String idText = person.attributeValue("id");
            //5.5 因为Human类的id属性类型为int，所以要转类型
            int idNum = Integer.parseInt(idText);

            //5.5 将上面获取到的值，都封装进Human类
            Human man = new Human(idNum, nameText, ageNum, genderText);

            //5.6 打印封装号的Human类对象
            System.out.println(man);
        }

        System.out.println("-----------------------------------------");
        //6.再次遍历每个person标签对象(这是为了演示elementText()方法)
        for (Element person: persons
             ) {
            //6.1 直接通过子元素标签名，获取其子标签的文本内容(作用等于5.1 + 5.1.1)
            String nameText = person.elementText("name");
            System.out.println(nameText);

            //6.2 直接通过子元素标签名，获取其子标签的文本内容(作用等于5.2 + 5.2.1)
            String ageText = person.elementText("age");
            System.out.println(ageText);

            //6.3 直接通过子元素标签名，获取其子标签的文本内容(作用等于5.3 + 5.3.1)
            String genderText = person.elementText("gender");
            System.out.println(genderText);
            System.out.println();
        }
    }
}
/**
 * 封装结果：
 * Human{id=1, name='Jason', age=21, gender='male'}
 * Human{id=2, name='Bob', age=22, gender='female'}
 * -----------------------------------------
 * Jason
 * 21
 * male
 *
 * Bob
 * 22
 * female
 */
```



