---
title: maven编译报错Process terminated
date: 2021-02-02 02:19:36
tags:
	- JavaWeb
	- Maven
	- 报错
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202043056.jpg
---

##### maven项目编译报错如下：

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202022025.png)

##### 点击【项目名】提示找到出错文件

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202022051.png)

##### 点击查看出错文件

在idea中打开了`settings`文件，找到提示的报错位置

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202022103.png)

##### ▐ 原因及解决办法

原因 ：缩进或者空格不一致导致该问题
解决办法：格式化编辑好之后复制再粘贴过来就可以了(推荐 👉[XML 在线格式化](https://c.runoob.com/front-end/710))

> 转载自：[南伯基尼的csdn博客](https://blog.csdn.net/weixin_44203158/article/details/105694724)