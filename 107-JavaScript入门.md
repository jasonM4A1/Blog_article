---
title: JavaScript入门
date: 2021-02-02 01:23:23
tags:
	- JavaScript
	- 前端
categories:
	- 前端
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202012435.png
---

# JavaScript 概述

## 介绍

JavaScript是一门脚本语言，不需要编译就可以直接被浏览器解析执行。它是运行在浏览器中的，每一个浏览器都有自己的JavaScript解析引擎。

JavaScript可以用来增强用户和html页面的交互过程，可以来控制html元素。让页面有一些动态的效果，以此来增强用户的体验。

## 特点

1. 交互性（它可以做的就是信息的动态交互）
2. 安全性（不允许直接访问本地硬盘）
3. 跨平台性（只要是可以解释 JS 的浏览器都可以执行，和平台无关）

## 发展史

1. 1992年，Nombase公司，开发出第一门客户端脚本语言，专门用于表单的校验。命名为 C– ，后来更名为ScriptEase
2. 1995年，Netscape(网景)公司，开发了一门客户端脚本语言：LiveScript。后来，请来SUN公司的专家，修改LiveScript，命名为JavaScript
3. 1996年，微软抄袭JavaScript开发出JScript语言
4. 1997年，ECMA(欧洲计算机制造商协会)，制定出客户端脚本语言的标准：ECMAScript，就是统一了所有客户端脚本语言的编码方式。

> JavaScript = ECMAScript + JavaScript自己特有的东西(BOM+DOM)

# JavaScript基础语法

## 与html结合方式

JavaScript与Html结合的方法总有两种，即——内部JS和外部JS。

### 内部JS

#### 概述

直接在html页面中书写JavaScript代码，所书写的JavaScript代码使用`Script`标签包围起来。

#### 格式

```
<script type="text/javascript">
	JS代码
</script>
```

#### 演示

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Demo</title>
    <!--在内部定义JS-->
    <script type="text/javascript">
        alert("Hello World");
    </script>
</head>
<body>

</body>
</html>
```

### 外部JS

#### 概述

使用`script`标签，通过src属性引入外部的js文件。

#### 格式

```
<script type="text/javascript" src="JS文件地址"></script>
```

#### 演示

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Demo</title>
    <!--引用外部JS-->
    <script type="text/javascript" src="../JavaScript/one.js"></script>
</head>
<body>

</body>
</html>
```

> 1. `script`标签可以定义在html页面的任何地方，但是定义的位置会影响其执行顺序。
> 2. `script`标签也可以在一个html页面，同时定义多个。

## JavaScript注释

### 概述

JavaScript的注释格式和Java的完全一致，无论是单行注释，还是多行注释。

### 单行注释

#### 格式

```
//注释内容
```

#### 演示

```
//这是一个单行注释
```

### 多行注释

#### 格式

```
/* 注释内容 */
```

#### 演示

```
/*
    这是一个多行注释
 */
```

## JavaScript数据类型

### 概述

JavaScript也同Java一样，数据类型分为两类，即——原始数据类型、引用数据类型。

### 原始数据类型

#### number类型

- 整数
- 小数
- NaN（not a number，一个不是数字的数字类型）

#### string类型

单引号与双引号定义的都是字符串类型，Javascript没有字符类型的概念。

#### boolean类型

- true
- false

#### null类型

一个对象为空的占位符。

#### undefined类型

未定义。如果一个变量没有给初始化值，则会被默认赋值为undefined。

### 引用数据类型

对象。Javascript中的引用数据类型也都是对象。

## JavaScript变量

### 概述

JavaScript与Java中的变量很相识，但又并不完全相同。根本原因为Javascript为弱类型语言，而Java为强类型语言。什么是弱类型语言，什么是强类型语言呢？下面我来简单的说明一下：

- **弱类型语言：**在开辟变量存储空间时，不定义空间将来的存储数据类型，而是可以存放任意类型的数据。
- **强类型语言：**在开辟变量存储空间时，定义了空间将来存储的数据的数据类型，只能存储固定类型的数据。

正是由于语言类型的不同，导致了两种语言的变量也有不同的用意，下文我主要表述其不同之处。

### 格式

```
//定义变量（默认赋值为undefined）
var 变量名;

//定义并初始化值
var 变量名 = 初始值;
```

### 演示

```
//定义变量（默认赋值为undefined)
var num01;
var num02;
var num03;

//接收number类型值
num01 = 1;
num02 = 3.14;
num03 = NaN;

//定义并初始化string类型值
var str01 = '2';
var str02 = "two";
var str03 = '我是一个二';

//定义并初始化boolean类型值
var flag01 = true;
var flag02 = false;

//定义并初始化null类型值
var obj = null;

//定义并初始化undefined类型值
var und01 = undefined;
var und02;//默认赋值为undefined
```

### 新特性

ES2015(ES6) 新增加了两个重要的 JavaScript 关键字: **let** 和 **const**。

let 声明的变量只在 let 命令所在的代码块内有效。

const 声明一个只读的常量，一旦声明，常量的值就不能改变。

- 使用var声明的变量，其作用域为该语句所在的函数内，且存在变量提升现象；
- 使用let声明的变量，其作用域为该语句所在的代码块内，不存在变量提升；
- 使用const声明的是常量，在后面出现的代码中不能再修改该常量的值。

**总结：**

通常，应该始终使用const声明变量，如果您意识到需要更改变量的值，请返回并将其更改为let。如果项目不支持ES6，再使用var来声明变量。

### typeof函数

#### 概述

由于Javascript的变量可以存储任意类型的数据，这样就导致我们无法判断一个变量中存储的数据是number类型、string类型的，还是boolean类型的。于是这个时候，我们就需要用到`typeof`运算符，来帮助我们区分变量中存储的不同数据的类型。

typeof的作用就是传入一个变量，同时返回这个变量所存储数据的数据类型。

#### 格式

```
typeof(变量名)
```

#### 演示

```
//分别判断num01、str01、flag01、obj、und01的数据类型
let type01 = typeof(num01);
let type02 = typeof(str01);
let type03 = typeof(flag01);
let type04 = typeof(obj);
let type05 = typeof(und01);

//打印到html页面上
document.write(type01 + "<br>");
document.write(type02 + "<br>");
document.write(type03 + "<br>");
document.write(type04 + "<br>");
document.write(type05 + "<br><br>");

/* 打印结果：
	number
    string
    boolean
    object
    undefined
*/
```

## JavaScript运算符

### 一元运算符

#### 概述

只有一个运算数参与的运算符。

#### 主要成员

```
++`、`--`、`+(正号)`、`-(负号)
```

#### 注意事项

1. 关于自增与自减：
   - `a++`，先运算，再自增。
   - `++a`，先自增，再运算。
2. 在JS中，如果运算数不是运算符所要求的类型，那么js引擎会自动的将运算数进行类型转换。如：
   - String类型转number类型：首先按照字面转化为相应的数字，如果字面值不是数字，则转为NaN（不是数字的数字）。
   - boolean类型转number类型：true转为1，false转为0。

### 算数运算符

#### 概述

用于进行一些数学运算的符号。

#### 主要成员

```
+`、`-`、`*`、`/`、`%
```

### 赋值运算符

#### 概述

将一个数据赋予另一数据的符号。

#### 主要成员

```
=`、`+=`、`-=
```

### 比较运算符

#### 概述

用于数据大小比较的运算符。

#### 主要成员

```
>`、`<`、`>=`、`<=`、`==`、`===(全等于)
```

#### 注意事项

1. 若类型相同，则直接进行比较。若数据类型不同，先进行类型转换，再比较。
2. `===(全等于)`：在比较之前，先判断类型，如果类型不一样，则直接返回false。
3. 字符串类型之间的数据进行比较，按照字典顺序比较。按位逐一比较，直到得出大小为止。

### 逻辑运算符

#### 概述

用于进行逻辑判断的运算符。

#### 主要成员

```
&&`、`||`、`!
```

#### 注意事项

在进行逻辑运算时，通常会遇到其他数据类型转为boolean数据。

1. number转boolean时：0或NaN为假，其他为真
2. string转boolean时：除了空字符串为假，其他都是true
3. null和undefined转boolean时：都为false
4. 对象转boolean时：都为true

### 三元运算符

#### 格式

```
//表达式为真，则返回结果1。为假，则返回结果2
表达式 ? 结果1 : 结果2
```

#### 演示

```
let a = 1;
let b = 2;
let c = a > b ? "正确" : "错误";
console.log(c);//错误
```

## 控制语句

### 主要成员

1. `if...else...`
2. `while`
3. `do...while`
4. `for`
5. `switch...case`

### 注意事项

switch语句的差异：

1. 在java中，switch语句可以接受的数据类型只有：`byte`、`short`、`int`、`char`、`枚举`、`String`。
2. 在Javascript中，switch语句可以接受任意的原始数据类型

## JS特殊语法

1. 语句以`;`结尾，如果一行只有一条语句则可以省略，但不建议

2. 变量的定义使用

   ```
   let
   ```

   关键字，但也可以不使用

   - 使用`let`关键字定义变量，定义的变量为局部变量。
   - 不是用`let`关键字定义变量，定义的变量为全局变量，但不建议

# JavaScript对象

JavaScript中的函数相当于Java中的方法，大致可以分为以下几种：

- function-方法对象
- Array-数组对象
- Date-日期对象
- Math-数学对象
- RegExp-正则对象
- Global-全局对象
- Number-数字包装对象
- String-字符串对象
- Boolean-布尔对象
- Error-错误对象

下面我会逐个讲解。

## function-函数对象

### 创建

#### 格式

```
//创建函数格式一
function 函数名(形参列表) {
    函数体
}

//创建函数格式二
let 函数名 = function(形参列表) {
    函数体
}

//创建函数格式三（不建议使用）
let 函数名 = new function(形参列表,函数体);
```

#### 演示

```
//创建函数格式一
function fun01(a, b) {
    console.log("fun01的结果为：" + (a + b));
}
//创建函数格式二
let fun02 = function(a,b){
    console.log("fun02的结果为：" + (a + b));
}
```

### 调用

#### 格式

```
函数名(实参列表);
```

#### 演示

```
//调用fun01
fun01(10,20);//fun01的结果为：30
//调用fun02
fun02(30,40);//fun02的结果为：70
```

### 属性

`length`：函数形参的个数。

```
let d = fun01.length;

console.log("函数fun01的形参个数为：" + d);//函数fun01的形参个数为：2
```

### 特点

1. 创建方法时，形参类型不需要写，返回值类型也不需要写。
2. 方法是一个对象，如果定义名称相同的方法，会后者后覆盖前者。
3. 在JS中，方法的调用只与方法的名称有关，和参数列表无关。即只要方法名正确，你参数传少、传多，都是可以正常调用方法的。
4. 在方法声明中有一个隐藏的内置对象（数组）——arguments，封装所有的实际参数。所以哪怕方法形参只有两个，但是你也可以传入三个，因为实参都是先被`arguments`数组所接收了。

## Array-数组对象

### 创建

#### 格式

```
//创建数组格式一
let 数组名 = new Array();

//创建数组格式二
let 数组名 = new Array(默认长度);

//创建数组格式三
let 数组名 = new Array(元素列表);

//创建数组格式四
let 数组名 = [元素列表];
```

#### 演示

```
//创建数组格式一
let arr01 = new Array();

//创建数组格式二
let arr02 = new Array(3);

//创建数组格式三
let arr03 = new Array(1,2,3);

//创建数组格式四
let arr04 = [4,5,6,7,8];
```

### 遍历

#### 格式

```
数组名[索引];
```

#### 演示

```
//遍历数组
for (let i = 0; i < arr04.length; i++) {
    console.log(arr04[i]);
}
```

### 属性

`length`：数组的长度

```
let num = arr03.length;//获取数组arr03的长度
```

### 方法

#### join()

将数组中的元素按照指定的分隔符拼接为字符串

```
//将数组中的元素按照指定的分隔符拼接为字符串
let arr05 = arr04.join("-");
console.log(arr05);////4-5-6-7-8
```

#### push()

向数组的末尾添加一个或更多元素，并返回新的长度。

```
//向数组的末尾添加一个或更多元素，并返回新的长度
let length = arr04.push(9,10,11);
console.log(length);//8
```

### 特点

1. JS中，数组存储元素的类型不是统一的，是可变的。而Java数组只能存储单一类型的元素。
2. JS中，数组长度也不是固定的，数组长度可变。而Java数组长度创建之初就固定，不可改变。

## Date-日期对象

### 创建

```

```

#### 格式

```
let 日期对象名 = new Date();
```

#### 演示

```
let newTime = new Date();
document.write(newTime);//Tue Dec 22 2020 22:45:04 GMT+0800 (GMT+08:00)
```

### 方法

- `toLocaleString()`：返回当前date对象对应的时间本地字符串格式

  ```
  let newTime = new Date();
  let localeTime = newTime.toLocaleDateString();
  document.write(localeTime);//2020/12/22
  ```

- `getTime()`：获取毫秒值。返回当前如期对象描述的时间到1970年1月1日零点的毫秒值差

  ```
  let newTime = new Date();
  let MSTime = newTime.getTime();
  document.write(MSTime);//1608648497413
  ```

## Math-数学对象

### 创建

Javascript中的Math也不需要创建了，直接通过`Math.方法名()`的方式来使用数学对象。

### 属性

`PI`：圆周率

```
let a = Math.PI;
document.write(a);//3.141592653589793
```

### 方法

- `random()`：返回一个`[0,)`之间的伪随机数。
- `ceil()`：对传入的参数，向上取整。
- `floor()`：对传入的参数，向下取整。
- `round()`：对传入的参数，四舍五入。

## RegExp-正则对象

### 创建

#### 格式

```
//创建正则格式一
let 正则对象名 = new RegExp("正则表达式");

//创建正则格式二
let 正则对象名 = /正则表达式/;
```

#### 演示

```
//创建正则格式一
let regExp01 = new RegExp("^0\\\\d{2}.[1-9].*6$");
document.write(regExp01);///^0\\d{2}.[1-9].*6$/

//创建正则格式二
let regExp02 = /^0\\d{2}.[1-9].*6$/;
document.write(regExp02);///^0\\d{2}.[1-9].*6$//^0\\d{2}.[1-9].*6$/
```

### 规则

规则太多了，说也说不清，来这里学习吧~

点我——>[正则学习网站](https://codejiaonang.com/#/)

### 方法

`test()`：传入字符串，验证是否符合正则定义的规范

```
let regExp01 = new RegExp("^0\\\\d{2}.[1-9].*6$");
let result = regExp01.test("3093");
document.write(result);//false
```

## Global-全局对象

### 创建

全局对象也是不需要创建的，Global对象中的方法也不需要对象，就可以直接被调用。如：`alert()`

### 方法

- `encodeURI()`：将字符串作为 URI 进行编码。
- `encodeURIComponent()`：将字符串作为 URI 组件进行编码。编码的字符更多。
- `decodeURI()`：对 encodeURI() 函数编码过的 URI 进行解码。
- `decodeURIComponent()`：可对 encodeURIComponent() 函数编码的 URI 进行解码。
- `eval()`：解析字符串形式的JavaScript代码，并执行它。
- `isNaN()`：因为用`==`与NaN进行比较，哪怕是NaN自身，也只能得到false。使用此方法，可以检查数据本身其是否为NaN。
- `parseInt()`：逐一判断每一个字符是否是数字，直到不是数字为止，将前边数字部分转为number。

## Number-数字包装对象

### 概述

Number 对象是原始数值的包装对象。

### 创建

#### 格式

```
let 变量名 = new Number(value);
```

#### 演示

```
let num = new Number(40);
```

### 方法

- `toString()`：数字转为字符串

  ```
  let num = new Number(40);
  document.write(typeof(num) + "<br/>");//object
  
  let string = num.toString();
  document.write(typeof(string));//string
  ```

- `valueof()`：拆箱

  ```
  let num = new Number(40);
  document.write(typeof(num) + "<br/>");//object
  
  let number = num.valueOf();
  document.write(typeof(number));//number
  ```

## String-字符串对象

### 概述

String 对象用于处理文本（字符串），和Java的String类几乎一致。

### 创建

#### 格式

```
//创建string对象格式一
let 变量名 = new String("字符串内容");

//创建string对象格式二
let 变量名 = new String('字符串内容');

//创建string对象格式三
let 变量名 = "字符串内容";

//创建string对象格式四
let 变量名 = '字符串内容';
```

#### 演示

```
//创建string对象格式一
let s01 = new String("字符串一");
document.write(s01 + "<br/>");

//创建string对象格式二
let s02 = new String('字符串二');
document.write(s02 + "<br/>");

//创建string对象格式三
let s03 = "字符串三";
document.write(s03 + "<br/>");

//创建string对象格式四
let s04 = '字符串四';
document.write(s04 + "<br/>");
```

### 方法

很多，也是基本和Java的差不多。您啊，干脆直接去【菜鸟教程】学得了。[JavaScript String 对象 | 菜鸟教程](https://www.runoob.com/jsref/jsref-obj-string.html)

## Boolean-布尔对象

### 概述

Boolean 对象用于转换一个不是 Boolean 类型的值转换为 Boolean 类型值 (true 或者false).

### 创建

#### 格式

```
//创建Boolean对象格式一
let boolean01 = new Boolean();

//创建Boolean对象格式二
let boolean02 = new Boolean(true);

//创建Boolean对象格式三
let boolean03 = false;
```

#### 演示

```
//创建Boolean对象格式一
let boolean01 = new Boolean();
document.write(boolean01 + "<br/>");//false

//创建Boolean对象格式二
let boolean02 = new Boolean(true);
document.write(boolean02 + "<br/>");//true

//创建Boolean对象格式三
let boolean03 = false;
document.write(boolean03 + "<br/>");//false
```

## Error-错误对象

### 概述

error是指程序中的非正常运行状态，在其他编程语言中称为“异常”或“错误”，解释器会为每个错误情形创建并抛出一个Error对象，其中包含错误的描述信息；

ECMAScript定义了六种类型的错误，除此之外，还可以使用Error构造方法创建自定义的Error对象，并使用throw语句抛出该对象。

### 创建

#### 格式

```
//创建Error对象格式一
let 变量名 = new Error();

//创建Error对象格式二
let 变量名 = new Error(message);
```

#### 演示

```
//创建Error对象格式一
let error01 = new Error();

//创建Error对象格式二
let error02 = new Error("空指针异常");
```

### 分类

1. `ReferenceError`：引用错误，要用的东西没找到；
2. `TypeError`：类型错误，错误的调用了对象的方法；
3. `RangeError`：范围错误，专指参数超范围；
4. `SyntaxError`：语法写错了；
5. `EvalError`：eval()方法错误的使用；
6. `URIError`：URI地址错误；

### 属性

- `name`： 设置或返回一个错误名
- `message`： 设置或返回一个错误信息(字符串)

```
let error02 = new Error("空指针异常");

let errorName = error02.name = "我的Error对象";
document.write(errorName + "<br/>");//我的Error对象

let errorMessage01 = error02.message;
document.write(errorMessage01 + "<br/>");//空指针异常

error02.message = "德玛西亚的异常";
document.write(error02.message + "<br/>");//德玛西亚的异常
```

## 自定义Object对象（扩展）

这个自定义的Object对象，和我们Java中的类很相识，都是由属性和方法(函数)组成的。下面我会来详细介绍一下这个自定Object对象的创建和使用。

### 创建

#### 格式

```
//创建自定Object对象格式一
let 变量名 = new Object();

//创建自定义Object对象格式二
let 变量名 = {
    属性名: 属性值,
    函数名: 函数体
}
```

#### 演示

```
//创建自定Object对象格式一
let obj01 = new Object();

//创建自定义Object对象格式二
let obj02 = {
    name: "Jason",
    age: 21,
    reading: function(){
        document.write("我在读书!");
    }
};
```

### 添加、修改值

#### 格式

```
Object对象.属性名 = 属性值;

Object对象.函数名 = 函数体;
```

#### 演示

```
obj01.name = "Bernardo Li";

obj01.reading = function () {
    document.write("我也在读书");
}
```

> 修改值与添加值一样，修改值无非是在对已存在的值进行赋值。

### 使用值

#### 格式

```
Object对象.属性名;

Object对象.函数名();
```

#### 演示

```
document.write(obj02.name + "<br/>");//Jason
obj02.reading();//我在读书!
```

# BOM对象

## 概述

BOM(Browser Object Model) 是指浏览器对象模型，是用于描述这种对象与对象之间层次关系的模型，浏览器对象模型提供了独立于内容的、可以与浏览器窗口进行互动的对象结构。

## 组成

BOM由多个对象组成，其中代表浏览器窗口的Window对象是BOM的顶层对象，其他对象都是该对象的子对象。其子对象如下：

- document——文档对象
- Navigator——浏览器对象
- Screen——显示器对象
- History——历史记录对象
- Location——地址栏对象

## Window-窗口对象

### 创建

首先window对象是不需要手动去创建的。我们可以通过`window.方法名()`的方式调用它的方法，甚至不需要`window.`，就可以直接调用其方法。

```
//window.方法名()
window.alert("我是一个弹窗");

//省略window，直接调用方法
alert("我也是一个弹窗");
```

### 属性

我们可以通过BOM对象中的属性来获取其子对象。如下：

1. `Window.Document`：获取Document文档对象`Window.`可省略）。
2. `Window.History`：获取History历史记录对象（`Window.`可省略）。
3. `Window.Navigator`：获取Navigator浏览器对象（`Window.`可省略）。
4. `Window.Screen`：获取Screen显示器对象（`Window.`可省略）。
5. `Window.Location`：获取Location地址栏对象（`Window.`可省略）。

### 方法

# DOM对象

## 前言

我们已经讲解过BOM对象了，知道DOM对象只是BOM顶层对象Window的几个子对象之一。既然DOM对象是BOM对象的子对象，为什么不包含在BOM对象中讲解，而是要单独提取出来进行说明呢？原因就是：DOM对象与History对象、Location对象等虽都为BOM的子对象，但DOM对象在我们日常开发中的使用频率远高于同级的BOM其他子对象。所以我们要对其详细讲解一下。

## 概述

通过 DOM，可以访问所有的 HTML 元素，连同它们所包含的文本和属性。可以对其中的内容进行修改和删除，同时也可以创建新的元素。

对于Dom对象的理解，可以总结为四点：

1. Document 它管理了所有的 HTML 文档内容。
2. document 它是一种树结构的文档，有层级关系。
3. 它让我们把所有的标签都对象化
4. 我们可以通过 document 访问所有的标签对象。

[![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201223072456.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201223072456.png)

## 分类

document对象包含了以下几种对象：

1. Element-元素对象
2. Attribute-属性对象
3. Text-文本对象
4. Comment-注释对象
5. Node-节点对象

## 创建

### 方式一

通过windown对象来获取，格式为：`window.document`

### 方式二

```
window.`可以省略，直接调用document对象，格式为：`document
```

### 获取其他对象

- `document.createElement(tagName)`：给定的标签名，创建一个标签对象。tagName为要创建的标签名。
- `document.createAttribute(attributeName)`：创建一个指定名称的属性，并返回Attr 对象属性。
- `document.createComment(text)`：创建注释节点。text为注释文本。
- `document.createTextNode(text)`：创建文本节点。text为文本节点的文本。

## Element-元素对象

### 获取

- `document.getElementById(elementId)`：通过标签的 id 属性查找标签 dom 对象，elementId 是标签的 id 属性值。
- `document.getElementsByName(elementName)`：通过标签的 name 属性查找标签 dom 对象，elementName 标签的 name 属性值、
- `document.getElementsByTagName(tagName)`：通过标签名查找标签 dom 对象，tagname 为标签名。

> 三个查询方法的优先级问题：
>
> 1. 如果有 id 属性，优先使用 getElementById 方法来进行查询
> 2. 如果没有 id 属性，则优先使用 getElementsByName 方法来进行查询
> 3. 如果 id 属性和 name 属性都没有最后再按标签名查 getElementsByTagName

**代码演示**

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Demo01</title>
    <style type="text/css">
        #title {
            font-weight: bold;
            color: cornflowerblue;
        }
    </style>
</head>
<body>
<!--一个表单-->
<form id="table" action="/">
    <span id="title">请选择你的爱好</span><br/>
    <span>Game：</span><input type="checkbox" name="hobbies" value="Game">
    <span>Reading:</span><input type="checkbox" name="hobbies" value="Reading">
    <span>Boxing:</span><input type="checkbox" name="hobbies" value="Boxing"><br/>
    <input type="submit" value="提交">
</form>
</body>
<script type="text/javascript">
    //通过Id获取dom对象
    var tableElement = document.getElementById("table");
    document.write(tableElement + "<br/>");//[object HTMLFormElement]

    //通过name获取dom对象们
    var hobbiesElement = document.getElementsByName("hobbies");
    document.write(hobbiesElement + "<br/>");//[object NodeList]

    //通过tagName获取dom对象们
    var spanElement = document.getElementsByTagName("span");
    document.write(spanElement + "<br/>");//[object HTMLCollection]
</script>
</html>
```

### 方法

- `setAttribute("属性名",属性值)`：设置调用者的指定属性名的属性值。
- `removeAttribute("属性名")`：移除调用者的指定属性。

## Node-节点对象

### 属性

- `parentNode`：获取当前节点的父节点。
- `childNodes`：获取当前节点的所有子节点
- `firstChild`：获取当前节点的第一个子节点
- `lastChid`：获取当前节点的最后一个子节点
- `previousSibling`：获取当前节点的上一个节点
- `nextSibling`：获取当前节点的下一个节点
- `className`：获取或设置标签的 class 属性值
- `innerHTML`：获取/设置起始标签和结束标签中的内容
- `innerText`：获取/设置起始标签和结束标签中的文本

### 方法

- `appendChild()`：向节点的子节点列表的结尾添加新的子节点。
- `removeChild()`：删除（并返回）当前节点的指定子节点。
- `replaceChild()`：用新节点替换一个子节点。

> document对象下的其他对象在这里就不在过多的叙述了，有兴趣的可以去菜鸟教程中学习。

# 事件

## 概述

某些组件被执行了某些操作后，触发某些代码的执行。

## 点击事件

1. `onclick`：单击事件
2. `ondblclick`：双击事件

## 焦点事件

1. ```
   onblur
   ```

   ：失去焦点

   - 一般用于表单验证

2. `onfocus`：元素获得焦点

## 加载事件

`onload`：一张页面或一副图像完成加载。

## 鼠标事件

1. `onmousedown`：鼠标按钮被按下。
2. `onmouseup`：鼠标按钮被松开。
3. `onmousemove`：鼠标被移动。
4. `onmouseover`：鼠标移到某元素之上。
5. `onmouseout`：鼠标从某元素移开。

## 键盘事件

1. `onkeydown`：某个键盘按键被按下。
2. `onkeyup`：某个键盘按键被松开。
3. `onkeypress`：某个键盘按键被按下并松开。

## 选择和改变

1. `onchange`：域的内容被改变。
2. `onselect`：文本被选中。

## 表单事件

1. `onsubmit`：确认按钮被点击。
2. `onreset`:重置按钮被点击。