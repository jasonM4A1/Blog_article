---
title: 'Cannot resolve plugin org.apache.maven.plugins:maven-site-plugin:3.7.1报错'
date: 2021-02-18 15:19:05
tags:
	- Java
	- Maven
	- 报错
categories:
	- 踩坑记录
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210218152004.jpg
---

# 问题描述

创建一个新的Maven项目，然后配置新的Jar包时候出现报错：

```
Cannot resolve plugin org.apache.maven.plugins:maven-site-plugin:3.7.1
```

# 解决方法

进入文件夹：`D:\JavaSoftware\apache-maven-3.6.3\maven-repo\org\apache\maven\plugins\maven-site-plugin`。

将多余的那个文件夹删除，我这里是删除的`3.3`，留下了`3.7.1`。

进入Idea右键`pom.xml`文件，在`Maven`选项中选择`Reload project`即可。