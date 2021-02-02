---
title: 'Idea新建Maven项目,警告:No archetype found in remote catalog...'
date: 2021-02-02 01:31:20
tags:
	- JavaWeb
	- Idea
	- Maven
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202013125.png
---

# 问题描述

Idea新建Maven项目时，初始化完成后，控制台报警：

```
No archetype found in remote catalog. Defaulting to internal catalog
```

翻译：在远程目录中没有找到原型。默认为内部目录。

> maven报错Archetype not found in any catalog，也是同样的解决方法。

# 问题解决

可以不用管【因为使用了maven模版构建项目，要从网上获取模版，然而没有找到这个模版或者网络慢，导致获取失败 】，选择File —> 再选择Close Project，关闭项目后重启就可以了。

> 转载自：https://www.cnblogs.com/loufangcheng/p/12861762.html