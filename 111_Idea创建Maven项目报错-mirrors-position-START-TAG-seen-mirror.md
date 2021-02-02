---
title: 'Idea创建Maven项目报错: mirrors'' (position: START_TAG seen ...</mirror>...'
date: 2021-02-02 01:32:50
tags:
	- JavaWeb
	- Idea
	- Maven
	- 报错
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202013335.png
---

# 问题描述

使用Idea创建一个新的Maven项目，在项目自动初始化完成后，控制台报错如下：

```
Some problems were encountered while building the effective settings
Unrecognised tag: 'mirrors' (position: START_TAG seen ...</mirror>\r\n     -->\r\n    <mirrors>... @160:14)  @ D:\apache-maven-3.6.3\conf\settings.xml, line 160, column 14
Unrecognised tag: 'mirrors' (position: START_TAG seen ...</mirror>\r\n     -->\r\n    <mirrors>... @160:14)  @ D:\apache-maven-3.6.3\conf\settings.xml, line 160, column 14
```

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220025635.png)

# 解决方案

将`D:\apache-maven-3.6.3\conf\settings.xml`文件中第160行的标签`<mirrors>...</mirrors>`中的`s`去掉既可。

[![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220025852.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220025852.png)

需要注意的是，Maven可以配置多个镜像，每个镜像在`<mirrors></mirrors>`中使用标签`<mirror></mirror>`包括起来(不带s)。