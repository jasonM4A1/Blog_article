---
-
title: Tomcat页面报错Http状态405-方法不允许
date: 2021-02-09 15:51:28
tags:
	- JavaWeb
	- Tomcat
	- 报错
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210209155226.png	
---

# 问题描述

新创建一个简单的servlet项目。代码如下图：

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210209155849.png)

启动Tomcat 浏览器输入项目正确路径。浏览器显示405错误。如下图：

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210209155913.png)

# 解决方法

**删除下列代码**。

```
super.doGet(req.resp);
super.doPost(req.resp);
```

> 转载自：[CSDN](https://blog.csdn.net/qq_45304476/article/details/109583956)