---
title: kde垃圾桶报错 
date: 2020-12-03 16:25:53
tags:
	- linux
    - 报错
    - kde
categories:
	- linux
cover:
	
---

# 问题描述

在使用manjaro kde系统时，点击任务栏上的trash垃圾桶图标，报错`malformed url trash:/`，译为`错误的垃圾桶路径`。

# 解决方法

## 方法一

`System Settings` ---> `default applications` ---> `File manager` 选择为之前系统默认的`Dolphiin`。因为系统的文件管理器选择错误了，所以才会如此。

## 方法二

删除`~/.local/share/Trash`文件夹中的所有文件。可能是因为trash中错误的垃圾文件，导致了trash的错误。
