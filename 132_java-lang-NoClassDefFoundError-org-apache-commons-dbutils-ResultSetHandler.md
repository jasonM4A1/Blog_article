---
title: 'java.lang.NoClassDefFoundError: org/apache/commons/dbutils/ResultSetHandler'
date: 2021-02-27 04:25:44
tags:
	- 报错
	- Tomcat
	- Java
categories:
	- 踩坑记录
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/wallhaven-7315m3.jpg
---

# 问题描述

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210227042832.png)

```
java.lang.NoClassDefFoundError: org/apache/commons/dbutils/ResultSetHandler
```

```
java.lang.ClassNotFoundException: org.apache.commons.dbutils.ResultSetHandler
```

# 问题分析

缺少commons-dbutils-1.7.jar包，或者jar包的位置不对。

# 解决方法

不要使用maven来导入jar，我试了无用。也不用直接从外面导入jar包，我也试了，也没用。先去官网下载co mmons-dbutils-1.7.jar包，然后在WEB-INF目录下创建lib目录，将jar放入其中，然后导入。

最后重启tomcat即可。