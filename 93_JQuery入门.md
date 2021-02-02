---
title: JQuery入门
date: 2020-12-01 16:24:53
tags:
	- 前端
    - JavaScript
    - JQuery
categories:
	- 前端
cover:
    https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210129042146.jpg
---

# JQuery概述

## 什么是JQuery？

jQuery， 顾名思义， 也就是 JavaScript 和查询（Query） ， 它就是辅助 JavaScript 开发的 js 类库。

## JQuery核心思想

它的核心思想是 write less,do more(写得更少,做得更多)， 所以它实现了很多浏览器的兼容问题。

## JQuery流行程度

jQuery 现在已经成为最流行的 JavaScript 库， 在世界前 10000 个访问最多的网站中， 有超过 55%在使用jQuery。

## jQuery 好处

jQuery 是免费、 开源的， jQuery 的语法设计可以使开发更加便捷， 例如操作文档对象、 选择 DOM 元素、制作动画效果、 事件处理、 使用 Ajax 以及其他功能。

# JQuery初体验

需求： 使用 jQuery 给一个按钮绑定单击事件?

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JQuery初体验</title>
    <!--引入jquery文件-->
    <script src="jquery-3.5.1.js"></script>
    <script>
        $(function() {
           $("#one").click(function() {
               alert("使用JQuery给一个按钮绑定单击事件");
           });
        });
    </script>
</head>
<body>
    <button id="one">你真还好吗？</button>
</body>
</html>
~~~

# JQuery核心函数

$ 是 jQuery 的核心函数， 能完成 jQuery 的很多功能。 $()就是调用$这个函数

1. 传入参数为 [ 函数 ] 时：

   表示页面加载完成之后。 相当于 window.onload = function(){}

2. 传入参数为 [ HTML 字符串 ] 时：

   会对我们创建这个 html 标签对象

3. 传入参数为 [ 选择器字符串 ] 时：

   $(“#id 属性值”); id 选择器， 根据 id 查询标签对象

   $(“标签名”); 标签名选择器， 根据指定的标签名查询标签对象

   $(“.class 属性值”); 类型选择器， 可以根据 class 属性查询标签对象

4.  传入参数为 [ DOM 对象 ] 时：

   会把这个 dom 对象转换为 jQuery 对象

# jQuery 对象和 dom 对象区分

## 什么是 jQuery 对象？ 什么是 dom 对象？

### DOM对象

1. 通过 getElementById()查询出来的标签对象是 Dom 对象
2. 通过 getElementsByName()查询出来的标签对象是 Dom 对象
3. 通过 getElementsByTagName()查询出来的标签对象是 Dom 对象
4. 通过 createElement() 方法创建的对象， 是 Dom 对象

> DOM 对象 Alert 出来的效果是： [object HTML 标签名 Element]

### JQuery对象

1. 通过 JQuery 提供的 API 创建的对象， 是 JQuery 对象
2. 通过 JQuery 包装的 Dom 对象， 也是 JQuery 对象
3. 通过 JQuery 提供的 API 查询到的对象， 是 JQuery 对象

> jQuery 对象 Alert 出来的效果是： [object Object]

## jQuery 对象的本质是什么？

jQuery 对象是 dom 对象的数组 + jQuery 提供的一系列功能函数。

## jQuery 对象和 Dom 对象使用区别

jQuery 对象不能使用 DOM 对象的属性和方法
DOM 对象也不能使用 jQuery 对象的属性和方法

## Dom 对象和 jQuery 对象互转

### dom 对象转化为 jQuery 对象

将DOM对象传入`$()`中即可

### jQuery 对象转为 dom 对象

jQuery 对象[下标]取出相应的 DOM 对象

# jQuery 选择器

## 基本选择器

+ `#id属性值`：id选择器，根据 id 查找标签对象
+ `.class属性值`：类选择器，根据 class 查找标签对象
+ `element标签名`：标签选择器，根据标签名查找标签对象
+ `*`：星选择器，表示任意的， 所有的元素
+ `selector1, selector2`：组合选择器，合并选择器 1， 选择器 2 的结果并返回

### 演示

~~~javascript
// 选择id="lastname" 的元素
$("#lastname")

// 选择class="intro" 的所有元素
$(".intro");

//选择所有 <p> 元素
$("p");

// 选择所有 <h1>、<div> 和 <p> 元素
$("h1, div, p");
~~~



## 层级选择器

+ `ancestor descendant`：后代选择器，在给定的祖先元素下匹配所有的后代元素
+ `parent > child`：子元素选择器，在给定的父元素下匹配所有的子元素
+ `prev + next`：邻居元素选择器，匹配所有紧接在 prev 元素后的 next 元素
+ `prev ~ siblings`：兄弟元素选择器，匹配 prev 元素之后的所有 siblings 元素

### 演示

~~~javascript
// 选择<form>中的所有后代<input>元素
$("form input");

//选择<form>中的仅子代<input>元素
$("form > input");

//选择<div>之后的一个同级<span>元素
$("div + span");

//选择<a>之后所有的同级<button>元素
$("a ~ button");
~~~



## 基本过滤选择器

+ `:first`：获取第一个元素
+ `:last`：获取最后一个元素
+ `:not(selector)`：去除所有与给定选择器匹配的元素
+ `:even`：匹配所有索引值为偶数的元素， 从 0 开始计数
+ `:odd`：匹配所有索引值为奇数的元素， 从 0 开始计数
+ `:eq(index)`：匹配一个给定索引值的元素
+ `:gt(index)`：匹配所有大于给定索引值的元素
+ `:lt(index)`：匹配所有小于给定索引值的元素
+ `:header`：匹配如 h1, h2, h3 之类的标题元素
+ `:animated`：匹配所有正在执行动画效果的元素

### 演示

~~~javascript
//选择第一个<input>元素
$("input:first");

//选择最后一个<a>元素
$("a:last");

//所有不为空的输入元素
$("input:not(:empty)");

//所有偶数 <tr> 元素，索引值从 0 开始，第一个元素是偶数 (0)，第二个元素是奇数 (1)，以此类推。
$("tr:even");

//所有奇数 <tr> 元素，索引值从 0 开始，第一个元素是偶数 (0)，第二个元素是奇数 (1)，以此类推。
$("tr:odd");

//列表中的第四个元素（index 值从 0 开始）
$("ul li:eq(3)");

//列举 index 大于 3 的元素
$("ul li:gt(3)");

//所有标题元素 <h1>, <h2> ...
$(":header");

//所有动画元素
$(":animated");
~~~



## 内容过滤器

+ `:contains("text")`：匹配包含给定文本的元素
+ `:empty`：匹配所有不包含子元素或者文本的空元素
+ `:parent`：匹配含有子元素或者文本的元素
+ `:has(selector)`：匹配含有选择器所匹配的元素的元素
+ `:hidden`：匹配所有不可见元素 display:none 或 input type=hidden

### 演示

~~~javascript
//所有包含文本 "Hello" 的元素
$(":contains('Hello')");

//所有不为空的输入元素
$("input:not(:empty)");

//匹配所有含有子元素或者文本的父元素。
$(":parent");

//所有包含有 <p> 元素在其内的 <div> 元素
$("div:has(p)");

//所有隐藏的 <p> 元素
$("p:hidden");
~~~



## 属性过滤器

+ `[attribute]`：匹配包含给定属性的元素
+ `[attribute=value]`：匹配给定的属性是某个特定值的元素
+ `[attribute!=value]`：匹配所有不含有指定的属性， 或者属性不等于特定值的元素
+ `[attribute^=value]`：匹配给定的属性是以某些值开始的元素
+ `[attribute$=value]`：匹配给定的属性是以某些值结尾的元素
+ `[attribute|=value]`：选取每个带有指定属性的元素，该元素的值等于指定字符串（比如 "en"）或以该字符串后跟连接符作为开头的字符串（比如 "en-us"）
+ `[attribute*=value]`：匹配给定的属性是以包含某些值的元素
+ `[attrSel1][attrSel2][attrSelN]`：复合属性选择器， 需要同时满足多个条件时使用

### 演示

~~~javascript
//所有带有 href 属性的元素
$("[href]");

//所有带有 href 属性且值等于 "default.htm" 的元素
$("[href='default.htm']");

//所有带有 href 属性且值不等于 "default.htm" 的元素
$("[href!='default.htm']");

//所有带有 title 属性且值以 "Tom" 开头的元素
$("[title^='Tom']");

//所有带有 href 属性且值以 ".jpg" 结尾的元素
$("[href$='.jpg']");

//所有带有 title 属性且值等于 'Tomorrow' 或者以 'Tomorrow' 后跟连接符作为开头的字符串
$("[title|='Tomorrow']");

//所有带有 title 属性且值包含字符串 "hello" 的元素
$("[title*='hello']");

//带有 id 属性，并且 name 属性以 man 结尾的输入框
$( "input[id][name$='man']" );
~~~



## 表单过滤器

+ `:input`：匹配所有 input, textarea, select 和 button 元素
+ `:text`：匹配所有的文本输入框
+ `:password`：匹配所有的密码输入框
+ `:radio`：匹配所有的单选框
+ `:checkbox`：匹配所有的复选框
+ `:submit`：匹配所有提交按钮
+ `:image`：匹配所有 img 标签
+ `:reset`：匹配所有重置按钮
+ `:button`：匹配所有 input type=button 按钮
+ `:file`：匹配所有 input type=file 文件上传框

### 演示

~~~javascript
//所有 input 元素
$(":input");

//所有带有 type="text" 的 input 元素
$(":text");

//所有带有 type="password" 的 input 元素
$(":password");

//所有带有 type="radio" 的 input 元素
$(":radio");

//所有带有 type="checkbox" 的 input 元素
$(":checkbox");

//所有带有 type="submit" 的 input 元素
$(":submit");

//所有带有 type="image" 的 input 元素
$(":image");

//所有带有 type="reset" 的 input 元素
$(":reset");

//所有带有 type="button" 的 input 元素
$(":button");

//所有带有 type="file" 的 inpu
$(":file");
~~~



## 表单对象属性过滤器

+ `:enabled`：匹配所有可用元素
+ `:disabled`：匹配所有不可用元素
+ `:checked`：匹配所有选中的单选， 复选， 和下拉列表中选中的 option 标签对象
+ `:selected`：匹配所有选中的 option

### 演示

```javascript
//所有启用的元素
$(":enabled");

//所有禁用的元素
$(":disabled");

//所有选中的复选框选项
$(":checked");

//所有选定的下拉列表元素
$(":selected");
```



# JQuery元素筛选

+ `eq(index)`：获取给定索引的元素（功能与`:eq(index)`一致）
+ `first()`：获取第一个元素（功能与`:first`一致）
+ `last()`：获取最后一个元素（功能与`:last`一致）
+ `filter(exp)`：留下匹配的元素
+ `is(exp)`：判断是否匹配给定的选择器， 只要有一个匹配就返回， true
+ `has(exp)`：返回包含有匹配选择器的元素的元素（功能与`:has(selector)`一致
+ `not(exp)`：删除匹配选择器的元素（功能与`:not(selector)`一致）
+ `children(exp)`：返回匹配给定选择器的子元素（功能与`:parent > child`一致）
+ `find(exp)`：返回匹配给定选择器的后代元素（功能与`ancestor descendant`一致）
+ `next()`：返回当前元素的下一个兄弟元素（功能与`prev + next`一致）
+ `nextAll()`：返回当前元素后面所有的兄弟元素（功能与`prev ~ siblings`一致）
+ `nextUntil()`：返回当前元素到指定匹配的元素为止的后面元素
+ `parent()`：返回父元素
+ `prev(exp)`：返回当前元素的上一个兄弟元素
+ `prevAll()`：返回当前元素前面所有的兄弟元素
+ `prevUnit(exp)`：返回当前元素到指定匹配的元素为止的前面元素
+ `siblings(exp)`：返回所有兄弟元素
+ `add()`：把 add 匹配的选择器的元素添加到当前 jquery 对象中

# JQuery的属性操作

+ `html()`：它可以设置和获取起始标签和结束标签中的内容
+ `text()`：它可以设置和获取起始标签和结束标签中的文本
+ `val()`：它可以设置和获取表单项的 value 属性值，还可以操作单选、复选、下拉列表的勾选
+ `attr()`：可以设置和获取属性的值，还可以添加自定义的属性、属性值（但不推荐操作 checked、 readOnly、 selected、 disabled 等等）
+ `prop()`：可以设置和获取属性的值（只推荐操作 checked、 readOnly、 selected、 disabled 等等）

~~~javascript
//获取id='one'的<div>元素中的html内容
$("div[id='one']").html();

//设置id='one'的<div>元素中的html内容
$("div[id='one']").html("<span>我是王多鱼</span>");//页面效果为：我是王多鱼

//获取id='two'的<div>元素中的text内容
$("div[id='two']").text();

//设置id='two'的<div>元素中的text内容
$("div[id='two']").text("<span>我是王多鱼</span>");//页面效果为：<span>我是王多鱼</span>

//获取id='three'且type='text'的<input>的value值
$(":text[id='three']").val();

//设置id='three'且type='text'的<input>的value值
$(":text[id='three']").val("HelloWorld");

//操作单选、复选、下拉列表的勾选
$(".radio,.checkbox,#single,#multiple").val(["man","game","Japanese","male","female"]);

//获取第一个复选框的name属性值
$(":checkbox:first").attr("name");

//设置最后一个复选框的name属性值
$(":checkbox:last").attr("name","hobbies");

//为第一个复选框添加自定义的love='never'属性值
$(":checkbox:first").attr("love","never");

//获取索引为0的单选框的选中状态
$(":radio:eq(0)").prop("checked");

//设置索引为1的单选框的选中状态
$(":radio:eq(1)").prop("checkbox","true");
~~~

# 案例-JQuery全选/全不选

## 案例要求

要求实现`全选/全不选`、`全选`、`全不选`、`反选`、`提交`按钮的功能。

## 案例实现

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Exercise</title>
    <script src="jquery-3.5.1.js"></script>
    <script>
        $(function () {
            //给全选按钮绑定单击事件
            $("#checkAllBtn").click(function () {
                $(":checkbox").prop("checked", true);
            });
            //给全不选按钮绑定单击事件
            $("#checkNotAllBtn").click(function () {
                $(":checkbox").prop("checked", false);
            });
            //给反选绑定按钮单击事件
            $("#invert").click(function () {
                //遍历每个name='sport'复选框的勾选状态
                $(":checkbox[name='sport']").each(function () {
                    //进行反选
                    this.checked = !this.checked;
                });
                //全部name='sport'复选框的个数
                let $allNum = $(":checkbox[name='sport']").length;
                //已经勾选的name='sport'复选框个数
                let $checkedNum = $(":checkbox[name='sport']:checked").length;
                //判断全选/全不选按钮选取状态
                $("#checkAllBox").prop("checked", $allNum === $checkedNum);
            });
            //给提交按钮绑定单击事件
            $("#submit").click(function () {
                //创建储存已选中按钮value值的数组
                let checkedValue = [];
                //遍历name='sport'的已选中复选框
                $(":checkbox[name='sport']:checked").each(function () {
                    //获取value值，并放入数组
                    checkedValue.push(this.value);
                });
                //弹出value值数组
                alert(checkedValue);
            });
            //给全选/全不选按钮绑定单击事件
            $("#checkAllBox").click(function () {
                //通过全选/全不选按钮的选中状态给所有name='sport'的复选框选中状态赋值
                $(":checkbox[name='sport']").prop("checked",this.checked);
            });
            //手动全选name='sport'复选框，全选/全不选复选框也要自动选中
            $(":checkbox[name='sport']").click(function(){
                //获取全部name='sport'复选框数量
                let $allSport = $(":checkbox[name='sport']").length;
                //获取全部已选中的name='sport'复选框数量
                let $allChecked = $(":checkbox[name='sport']:checked").length;
                //判断全选/全不选checked状态
                $("#checkAllBox").prop("checked",$allSport === $allChecked);
            });
        });
    </script>
</head>
<body>
<form method="post" action="">
    <span>What's your favorite sport?</span>
    <input type="checkbox" id="checkAllBox"/>全选/全不选
    <br/>
    <input type="checkbox" name="sport" value="football"/>football
    <input type="checkbox" name="sport" value="basketball"/>basketball
    <input type="checkbox" name="sport" value="badminton"/>badminton
    <input type="checkbox" name="sport" value="tableTennis"/>tableTennis
    <br/>
    <input type="button" id="checkAllBtn" value="全选"/>
    <input type="button" id="checkNotAllBtn" value="全不选"/>
    <input type="button" id="invert" value="反选"/>
    <input type="button" id="submit" value="提交"/>
</form>
</body>
</html>
```

# DOM对象的增删改

## 内部插入

+ `appendTo()`：例如`a.prependTo(b)`，把 a 插入到 b 子元素末尾， 成为最后一个子元素
+ `prependTo()`：例如`a.prependTo(b)`，把 a 插到 b 所有子元素前面， 成为第一个子元素

## 外部插入

+ `insertAfter()`：例如`a.insertAfter(b)`，把 a 插到 b 后面， 成为同级一元素
+ `insertBefore()`：例如`a.insertBefore(b)`，把 a 插到 b 前面， 成为同级一元素

## 替换

+ `replaceWith()`：例如`a.replaceWith(b)`，用b替换掉所有a
+ `replaceAll()`：例如`a.replaceAll(b)`，用a替换掉所有b

## 删除

+ `remove()`：例如`a.remove()`，删除a标签
+ `empty()`：例如`a.empty()`，清空a标签里的内容

## 演示

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>DOM对象增删改</title>
    <script src="jquery-3.5.1.js"></script>
    <script>
        $(function(){
            //创建一个DOM对象，并添加class='one'的div中的最后面
            $("<p>原来我也是一个Joker</p>").appendTo($("div[class='one']"));
            //创建一个DOM对象，并添加class='one'的div中的最前面
            $("<a>www.pornhub.com</a><br/>").prependTo($("div[class='one']"));
            //创建一个DOM对象，并添加class='one'的div外的最前面
            $("<img src=\"\" alt=\"骗你的，其实没有图片\"/>").insertBefore($("div[class='one']"));
            //创建一个DOM对象，并添加class='one'的div外的最后面
            $("<ol>什么情况啊！你！</ol>").insertAfter($("div[class='one']"));
            //前替换所有后
            $("    <form>\n" +
                "        <span>装逼犯姓名：</span>\n" +
                "        <input type=\"text\"/>\n" +
                "    </form>").replaceAll($("div[class='two']"));
            //后替换所有前
            $("div[class='three']").replaceWith("<p>别叫了，行不行啊！</p>");
            //删除指定DOM对象
            $("div[class='four']:first").remove();
            //清楚指定DOM对象中的内容
            $("div[class='four']:first").empty();
        });
    </script>
</head>
<body>
    <div class="one">
        <span>严老板都已经拉了夸，你是真小丑！</span>
    </div>
    <div class="two">
        <span>不是，我都不知道，你在装什么？</span>
    </div>
    <div class="two">
        <span>你就是一个因溜狗！</span>
    </div>
    <div class="three">
        都太经典了，你知道不？
    </div>
    <div class="three">
        龙哥的S6第一个王者，韩晶亮你懂不懂啊！
    </div>
    <div class="four">
        上路被三人越塔！打野都不在，我为什么要去啊！
        不是，不是抬杠！你来告诉我，打野都不在，我为什么要去！
    </div>
    <div class="four">
        来，这个叫尊尼获加的臭JB钢筋！我给你房管来，来你给我说话！
        你说不明白，明天你妈出门被车撞死！
    </div>
</body>
</html>
```

# 案例-从左到右，从右到左

## 案例要求

做出两个下拉列表，再做出四个按钮。按钮的功能分别为：`添加选中项到右边`、`添加全部项到右边`、`添加选中项到左边`、`添加全部项到左边`。

## 案例实现

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>从左到右，从右到左</title>
    <script src="jquery-3.5.1.js"></script>
    <style>
        select {
            width: 100px;
            height: 140px;
        }

        div {
            width: 130px;
            float: left;
            text-align: center;
        }
    </style>
    <script>
        $(function () {
            //实现"添加选中项到右边"按钮功能
            $("button:eq(0)").click(function () {
                $("select:eq(0) > option:selected").appendTo($("select:eq(1)"));
            });
            //实现"添加全部项到右边"按钮功能
            $("button:eq(1)").click(function () {
                $("select:eq(0) > option").appendTo($("select:eq(1)"));
            });
            //实现"添加选中项到左边"按钮功能
            $("button:eq(2)").click(function () {
                $("select:eq(1) > option:selected").appendTo($("select:eq(0)"));
            });
            //实现"添加全部项到左边"按钮功能
            $("button:eq(3)").click(function () {
                $("select:eq(1) > option").appendTo($("select:eq(0)"));
            });
        });
    </script>
</head>
<body>
<div id="left">
    <select multiple="multiple" name="sel01">
        <option value="one">one</option>
        <option value="two">two</option>
        <option value="three">three</option>
        <option value="four">four</option>
        <option value="five">five</option>
        <option value="six">six</option>
        <option value="seven">seven</option>
        <option value="eight">eight</option>
    </select><br/>
    <button>添加选中项到右边</button>
    <button>添加全部项到右边</button>
</div>
<div id="right">
    <select multiple="multiple" name="sel02"></select><br/>
    <button>添加选中项到左边</button>
    <button>添加全部项到左边</button>
</div>
</body>
</html>
```

# 案例-表格的动态添加、删除

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>动态添加、删除表格记录</title>
    <style>
        #employeeTable {
            border-spacing: 1px;
            background-color: black;
            margin: 80px auto 10px auto;
        }

        th, td {
            background-color: white;
        }

        #formDiv {
            width: 250px;
            border-style: solid;
            border-width: 1px;
            margin: 50px auto 10px auto;
            padding: 10px;
        }

        #formDiv input {
            width: 100%;
        }

        .word {
            width: 40px;
        }

        .inp {
            width: 200px;
        }
    </style>
    <script src="jquery-3.5.1.js"></script>
    <script>
        $(function () {
            //Delete函数
            let deleteFun = function () {
                //获取被删除表格行对象
                let $trObj = $(this).parent().parent();
                //获取被删除表格行name
                let name = $trObj.find($("td:first")).text();
                //用户确认删除
                if (confirm("确定删除[" + name + "]用户？")) {
                    $trObj.remove();
                }
                //阻止a标签的默认跳转
                return false;
            };
            //实现提交按钮功能
            $("#addEmpButton").click(function () {
                //获取输入框中 姓名、邮箱、工资的内容
                let name = $(":text[name='empName']").val();
                let email = $(":text[name='email']").val();
                let salary = $(":text[name='salary']").val();
                //创建一个新的表格行，将数据添加进去
                let $newTrObj = $("    <tr>" +
                    "        <td>"+name+"</td>" +
                    "        <td>"+email+"</td>" +
                    "        <td>"+salary+"</td>" +
                    "        <td><a href=\"deleteEmp?id=new\">Delete</a></td>" +
                    "    </tr>");
                //将新的表格行添加进员工表格中
                $newTrObj.appendTo($("#employeeTable"));
                //与新添加的表格行绑定Delete事件
                $newTrObj.find("a").click(deleteFun);
            });
            //实现Delete<a>标签功能
            $("a").click(deleteFun);
        });
    </script>
</head>
<body>
<table id="employeeTable">
    <tr>
        <th>Name</th>
        <th>Email</th>
        <th>Salary</th>
        <th>Delete</th>
    </tr>
    <tr>
        <td>Jason</td>
        <td>your_email@example.com</td>
        <td>5000</td>
        <td><a href="deleteEmp?id=001">Delete</a></td>
    </tr>
    <tr>
        <td>Bob</td>
        <td>your_email@example.com</td>
        <td>6000</td>
        <td><a href="deleteEmp?id=002">Delete</a></td>
    </tr>
    <tr>
        <td>Bernardo</td>
        <td>your_email@example.com</td>
        <td>7000</td>
        <td><a href="deleteEmp?id=003">Delete</a></td>
    </tr>
</table>
<div id="formDiv">
    <h4>添加新员工</h4>
    <table>
        <tr>
            <td class="word">name</td>
            <td class="inp">
                <input type="text" name="empName" id="empName"/>
            </td>
        </tr>
        <tr>
            <td class="word">email</td>
            <td class="inp">
                <input type="text" name="email" id="email"/>
            </td>
        </tr>
        <tr>
            <td class="word">salary</td>
            <td class="inp">
                <input type="text" name="salary" id="salary"/>
            </td>
        </tr>
        <tr>
            <td colspan="2" align="center">
                <button id="addEmpButton" value="abc">
                    Submit
                </button>
            </td>
        </tr>
    </table>
</div>
</body>
</html>
```

# JQuery操作CSS样式

+ `addClass()`：添加样式

+ `removeClass()`：删除样式

+ `toggleClass()`：样式存在即删除，不存在即添加

+ `offset()`：获取和设置元素的坐标位置

  + 属性：top 和 left，分别为到dom顶的距离、和到dom左的距离。

  > 在设置`offset()`时，必须传入键值对数据。如`offset({left:100,top:100})`

## 演示

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Css样式操作</title>
    <style>
        div {
            width: 100px;
            height: 260px;
        }

        div.yellowBorder {
            border: 2px yellow solid;
        }

        div.redDiv {
            background-color: red;
        }

        div.blueBorder {
            border: 5px blue solid;
        }
    </style>
    <script src="jquery-3.5.1.js"></script>
    <script>
        $(function () {
            //实现addClass()按钮功能
            $(":button[value='addClass()']").click(function () {
                $("div.border").addClass("redDiv yellowBorder");
            });
            //实现removeClass()按钮功能
            $(":button[value='removeClass()']").click(function () {
                $("div.border").removeClass("redDiv");
            });
            //实现toggleClass()按钮功能
            $(":button[value='toggleClass()']").click(function () {
                $("div.border").toggleClass("blueBorder");
            });
            //实现offset()按钮的获取功能
            $(":button[value='offset()01']").click(function () {
                let leftNum = $("div.border").offset().left;
                let topNum = $("div.border").offset().top;
                alert("left: "+leftNum+"    top: "+topNum);
            });
            //实现offset()按钮的设置功能
            $("#btn05").click(function () {
                let $offset = $("div.border").offset({
                    top: 100,
                    left: 100
                });
            });
        });
    </script>
</head>
<body>
<table align="center">
    <tr>
        <!--空白元素，以便测试css样式-->
        <td>
            <div class="border"></div>
        </td>
        <td>
            <div class="btn">
                <input type="button" value="addClass()" id="btn01"/>
                <input type="button" value="removeClass()" id="btn02"/>
                <input type="button" value="toggleClass()" id="btn03"/>
                <input type="button" value="offset()01" id="btn04"/>
                <input type="button" value="offset()02" id="btn05"/>
            </div>
        </td>
    </tr>
</table>
</body>
</html>
```

# JQuery操作动画

## 基本动画

+ `show()`：将隐藏的元素显示
+ `hide()`：将可见的元素隐藏
+ `toggle()`：可见则隐藏，不可见则显示=

## 淡入淡出动画

+ `fadeIn()`：淡入（慢慢可见）
+ `fadeOut()`：淡出（慢慢消失）
+ `fadeTo()`：在指定时长内慢慢的将透明度修改到指定的值。 0 透明， 1 完成可见， 0.5 半透明（此方法较为特殊，第二个参数为透明度，第三个方法才为回调函数）
+ `fadeToggle()`：淡入则切换为淡出，淡出则切换为淡入

> 上述所有方法都可以添加两个参数：
>
> 1. 参数一，动画执行的时长
> 2. 参数二，动画的回调函数

## 演示

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JQuery操作动画演示</title>
    <style>
        #tableClass {
            border: 1px solid;
            border-collapse: collapse;
            float: left;
        }
        #divClass {
            border: 1px solid;
            float: left;
            width: 300px;
            height: 200px;
            background-color: #0044DD;
        }
        table, td {
            border: 1px solid;
            border-collapse: collapse;
        }
    </style>
    <script src="../javascript/jquery-3.5.1.js"></script>
    <script>
        $(document).ready(function () {
            //实现'隐藏hide()'按钮功能
            $("#btn01").click(function () {
                $("#divClass").hide(2000,function () {
                    alert("隐藏hide()动画执行完成");
                });
            });
            //实现'显示show()'按钮功能
            $("#btn02").click(function () {
                $("#divClass").show(2000, function () {
                    alert("显示show()动画执行完成");
                });
            });
            //实现'切换显示/隐藏toggle()'按钮功能
            $("#btn03").click(function () {
                $("#divClass").toggle(3000, function () {
                    alert("切换显示/隐藏toggle()动画执行完成");
                });
            });
            //实现'淡出fadeOut()'按钮功能
            $("#btn04").click(function () {
                $("#divClass").fadeOut(1000, function () {
                    alert("淡出fadeOut()动画执行完成");
                });
            });
            //实现'淡入fadeIn()'按钮功能
            $("#btn05").click(function () {
                $("#divClass").fadeIn(2000, function () {
                    alert("淡入fadeIn()动画执行完成");
                });
            });
            //实现'切换淡出/淡入fadeToggle()'按钮功能
            $("#btn06").click(function () {
                $("#divClass").fadeToggle(1000, function () {
                    alert("切换淡出/淡入fadeToggle()动画执行完成");
                });
            });
            //实现'淡化到fadeTo()'按钮功能
            $("#btn07").click(function () {
                $("#divClass").fadeTo(3000, 0.5, function () {
                    alert("淡化到fadeTo()动画执行完成");
                });
            });
        });
    </script>
</head>
<body>
<table id="tableClass">
    <tr>
        <td>
            <button id="btn01">隐藏hide()</button>
        </td>
    </tr>
    <tr>
        <td>
            <button id="btn02">显示show()</button>
        </td>
    </tr>
    <tr>
        <td>
            <button id="btn03">显示/隐藏切换toggle()</button>
        </td>
    </tr>
    <tr>
        <td>
            <button id="btn04">淡出fadeOut()</button>
        </td>
    </tr>
    <tr>
        <td>
            <button id="btn05">淡入fadeIn()</button>
        </td>
    </tr>
    <tr>
        <td>
            <button id="btn06">淡化切换fadeToggle()</button>
        </td>
    </tr>
    <tr>
        <td>
            <button id="btn07">淡化到fadeTo()</button>
        </td>
    </tr>
</table>
<div id="divClass"></div>
</body>
</html>
~~~

# 案例-CSS动画之品牌展示

## 案例要求

1. 点击按钮的时候， 隐藏和显示卡西欧之后的品牌。
2. 当显示全部内容的时候， 按钮文本为“显示精简品牌”。然后小三角形向上， 所有品牌产品为默认颜色。
3. 当只显示精简品牌的时候， 要隐藏卡西欧之后的品牌， 按钮文本为“显示全部品牌”。然后小三形向下，并且把 佳能，尼康的品牌颜色改为红色（给 li 标签添加 promoted 样式即可）

## 案例实现

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>CSS动画之商品展示练习</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        body {
            font-size: 12px;
            text-align: center;
        }

        a {
            color: #0044DD;
            text-decoration: none;
        }

        a:hover {
            color: #FF5500;
            text-decoration: underline;
        }

        .SubCategoryBox {
            width: 600px;
            margin: 0 auto;
            text-align: center;
            margin-top: 40px;
        }

        .SubCategoryBox ul {
            list-style: none;
        }

        .SubCategoryBox ul li {
            display: block;
            float: left;
            width: 200px;
            line-height: 20px;
        }

        .showMore, .showLess {
            clear: both;
            text-align: center;
            paddin-top: 10px;
        }

        .showMore a, .showLess a {
            display: block;
            width: 120px;
            margin: 0 auto;
            line-height: 24px;
            border: 1px solid #AAAAAA;
        }

        .showMore a span {
            padding-left: 15px;
            background: url("img/down.gif") no-repeat 0 0;
        }

        .showLess a span {
            padding-left: 15px;
            background: url("img/up.gif") no-repeat 0 0;
        }

        .promoted a {
            color: #FF5500;
        }
    </style>
    <script src="jquery-3.5.1.js"></script>
    <script>
        $(function () {
            //页面初始状态
            $("li:gt(4):not(:last)").hide();
            //实现展示/收起按钮功能
            $("a:last").click(function () {
                //让下面的品牌显示或隐藏
                $("li:gt(4):not(:last)").toggle();
                //判断下面的品牌是显示还是隐藏
                if ($("li:gt(4):not(:last)").is(":hidden")) {
                    //若下面的品牌是隐藏的，则text为'显示全部品牌'，图标为向下
                    $("div div a span").text("显示全部品牌");
                    $("div div").removeClass();
                    $("div div").addClass("showMore")
                    //除去广告高亮
                    $("li:contains('佳能')").removeClass("promoted");
                    $("li:contains('索尼')").removeClass("promoted");
                    $("li:contains('三星')").removeClass("promoted");
                } else {
                    //若下面的品牌是显示的，则text为'显示精简品牌'，图标为向上
                    $("div div a span").text("显示精简品牌");
                    $("div div").removeClass();
                    $("div div").addClass("showLess");
                    //添加广告高亮
                    $("li:contains('佳能')").addClass("promoted");
                    $("li:contains('索尼')").addClass("promoted");
                    $("li:contains('三星')").addClass("promoted");
                }
                //取消<a>标签自动跳转
                return false;
            });
        });
    </script>
</head>
<body>
<div class="SubCategoryBox">
    <ul>
        <li>
            <a href="#">佳能</a>
            <i>(30440)</i>
        </li>
        <li>
            <a href="#">索尼</a>
            <i>(20001)</i>
        </li>
        <li>
            <a href="#">三星</a>
            <i>(19028)</i>
        </li>
        <li>
            <a href="#">尼康</a>
            <i>(17821)</i>
        </li>
        <li>
            <a href="#">松下</a>
            <i>(12289)</i>
        </li>
        <li>
            <a href="#">卡西欧</a>
            <i>(8242)</i>
        </li>
        <li>
            <a href="#">富士</a>
            <i>(14894)</i>
        </li>
        <li>
            <a href="#">柯达</a>
            <i>(9520)</i>
        </li>
        <li>
            <a href="#">宾得</a>
            <i>(2195)</i>
        </li>
        <li>
            <a href="#">理光</a>
            <i>(4114)</i>
        </li>
        <li>
            <a href="#">奥林巴斯</a>
            <i>(12205)</i>
        </li>
        <li>
            <a href="#">明基</a>
            <i>(1466)</i>
        </li>
        <li>
            <a href="#">爱国者</a>
            <i>(3091)</i>
        </li>
        <li>
            <a href="#">小米</a>
            <i>(1010)</i>
        </li>
        <li>
            <a href="#">其他品牌</a>
            <i>(7275)</i>
        </li>
    </ul>
    <div class="showMore">
        <a href="more.html">
            <span>显示全部品牌</span>
        </a>
    </div>
</div>
</body>
</html>
```

# JQuery操作事件

## JQuery页面加载后事件

### 什么是'JQuery页面加载后事件'?

`$(document).ready()`

### JQuery页面加载后事件与原生JS页面加载后事件的触发顺序？

+ JQuery的`$(document).ready()`先执行
+ 原生JS的`window.onload`后执行

### JQuery页面加载后事件与原生JS页面加载后事件的触发时机？

+ JQuery的`$(document).ready()`是浏览器的内核解析完页面的标签，创建好 DOM 对象之后就会马上执行
+ 原生JS的`window.onload`除了要等浏览器内核解析完标签创建好 DOM 对象， 还要等标签显示时需要的内容加载
  完成才能执行

### JQuery页面加载后事件与原生JS页面加载后事件的可执行次数？

+ JQuery的`$(document).ready()`，有多少执行多少（依次）
+ 原生JS的`window.onload`再多，也只能执行最后依次的赋值函数

## JQuery其他常见事件

+ `click()`：鼠标单击事件（被调用时传入函数为绑定，不传入函数为触发）
+ `mouseover()`：鼠标移入事件（被调用时传入函数为绑定，不传入函数为触发）
+ `mouseout()`：鼠标移出事件（被调用时传入函数为绑定，不传入函数为触发）
+ `bind()`：给元素绑定一个或多个事件
+ `one()`：给元素绑定一个或多个事件，但其绑定的事件只能被触发一次（使用格式与`bind()`一致）
+ `unbind()`：解除元素绑定的一个或多个事件（功能与`bind()`相反，调用时只需传入字符串形式的事件名即可）
+ `$(document).on(events,[selector],[data],fn) `：动态绑定事件，即使元素是后创建的

### 演示

~~~javascript
//click()绑定单击事件
$("h5").click(function(){
    alert("h5的单击事件");
});
//click()触发单击事件
$("h5").click();

//mouseover()绑定鼠标移入事件
$("h5").mouseover(function () {
    alert("h5的鼠标移入事件");;
});
//mouseover()触发鼠标移入事件
$("h5").mouseover();

//mouseout()绑定鼠标移出事件
$("h5").mouseout(function () {
    alert("鼠标移出事件");
});
//mouseout()触发鼠标移出事件
$("h5").mouseout();

//bind()元素绑定三个事件
$("h5").bind("click mouseover mouseout", function () {
    console.log("别JB叫！");
});
//bind()分别触发三个事件
$("h5").click();
$("h5").mouseover();
$("h5").mouseout();

//one()元素绑定三个事件
$("h5").one("click mouseover mouseout", function () {
    console.log("我说的，眼老板是垃圾！");
});
//one()分别触发三个事件（一个事件只能被触发一次）
$("h5").click();
$("h5").mouseover();
$("h5").mouseout();

//unbind()解除两个事件的绑定
$("h5").unbind("mouseover mouseout");
//unbind()解除所有事件的绑定（不传参，表示解除全部事件绑定）
$("h5").unbind();

//$(document).on(events,[selector],[data],fn)动态绑定事件
$(document).on("click mouseover mouseout", $("h5"), function(){
    console.log("我是回调函数");
});
~~~

# 事件冒泡

## 什么是事件冒泡？

事件的冒泡是指：父子元素同时监听同一个事件，当触发子元素的事件的时候， 同一个事件也被传递到了父元素的事件里去响应。

### 演示

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JQuery操作事件</title>
    <style>
        #ancestor {
            width: 300px;
            height: 300px;
            border: black solid 2px;
        }

        #descendant {
            width: 200px;
            height: 200px;
            border: blue solid 1px;
        }

    </style>
    <script src="jquery-3.5.1.js"></script>
    <script>
        $(document).ready(function () {
            //父元素div与子元素div绑定同一个事件，
            //当子元素div的单击事件被触发，
            //父元素div的单击事件也会被自动触发
            $("div").click(function () {
                alert("事件冒泡");
            });
        });
    </script>
</head>
<body>
<div id="ancestor">one
    <div id="descendant">two</div>
</div>
</body>
</html>
```



## 怎么解决事件冒泡？

在子元素事件函数体内， return false; 可以阻止事件的冒泡传递。

### 演示

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JQuery操作事件</title>
    <style>
        #ancestor {
            width: 300px;
            height: 300px;
            border: black solid 2px;
        }

        #descendant {
            width: 200px;
            height: 200px;
            border: blue solid 1px;
        }
    </style>
    <script src="jquery-3.5.1.js"></script>
    <script>
        $(document).ready(function () {
            //与上面的是同样的代码，现在只需要加上return false
            //即可阻止事件冒泡的产生
            $("div").click(function () {
                alert("事件冒泡");
                return false;
            });
        });
    </script>
</head>
<body>
<div id="ancestor">one
    <div id="descendant">two</div>
</div>
</body>
</html>
```

# 事件对象

## 什么是事件对象？

事件对象， 是封装有触发的事件信息的一个 javascript 对象。

## 如何获取事件对象？

在给元素绑定事件的时候， 在事件的 function( event ) 参数列表中添加一个参数， 这个参数名， 我们习惯取名为 event。这个 event 就是 javascript 传递参事件处理函数的事件对象。

### 演示

```javascript
//原生Js获取事件对象event
window.onload = function () {
    document.getElementsByTagName("div")[0].onclick = function (event) {
        console.log(event);
    };
};

//JQuery获取事件对象event
$(document).ready(function () {
    $("div:first").click(function (event) {
        console.log(event);
    });
});
```

## 事件对象的作用

在我们使用`bind()`给一个元素一次性绑定多个事件时，我们该如何分别给这些事件设置单独的函数呢？这个时候，我们就需要用到事件对象event了。

```javascript
//使用bind()给一个元素一次性绑定多个事件
$("div:first").bind("mouseover mouseout click", function (event) {
    //若是mouseover事件，控制台打印mouseover
    if (event.type == "mouseover") {
        console.log("mouseover");
    } else if (event.type == "mouseout") {
        //若是mouseout事件，控制台打印mouseout
        console.log("mouseout");
    } else if (event.type == "click") {
        //若是click事件，控制台打印click
        console.log("click");
    }
});
```

# 案例-图片放大跟随

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>图片跟随</title>
    <style>
        body {
            text-align: center;
        }
        #small {
            width: 20%;
            height: 20%;
        }
        #big {
            width: 600px;
            height: 300px;
        }
        #showBig {
            position: absolute;
            display: none;
        }
    </style>
    <script src="jquery-3.5.1.js"></script>
    <script>
        $(document).ready(function () {
            $("#small").bind("mouseover mouseout mousemove", function (event) {
                //当鼠标到图片上时，添加大图片
                if (event.type == "mouseover") {
                    $("#showBig").show();

                    //当鼠标移动时，大图片也要跟随鼠标移动
                } else if (event.type == "mousemove") {
                    $("#showBig").offset({
                       left: event.pageX + 10,
                       top: event.pageY + 10
                    });

                    //当鼠标移出图片上时，大图片消失
                } else if (event.type == "mouseout") {
                    $("#showBig").hide();
                }
            });
        });
    </script>
</head>
<body>
<img id="small" src="img/wallhaven-6oqzgq.jpg"/>
<div id="showBig">
    <img id="big" src="img/wallhaven-6oqzgq.jpg"/>
</div>
</body>
</html>
```



