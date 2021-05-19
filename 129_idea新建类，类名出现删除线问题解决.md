---
title: idea新建类，类名出现删除线问题解决
date: 2021-02-18 15:43:22
tags:
	- Idea
	- Java	
	- 报错
categories:
	- 踩坑记录
---

# 问题描述

idea新建类，类名出现删除线问题解决

# 解决方法

这个问题是自己的大意，百度没找到答案。

原来是在类的头部注释中，出现笔误。

本想写@Description，结果写成 了@deprecated

@deprecated注解的意思是标志类过时，废弃。

所以类名出现了删除线。大意引起的问题

> 转载自：https://blog.csdn.net/u012817635/article/details/79361292