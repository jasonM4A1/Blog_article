---
title: Linux下idea启动tomcat项目报错：Error copying configuration files from
date: 2021-01-31 02:01:04
tags:
	- Linux
	- Java
	- JavaWeb
	- Tomcat
	- 报错
categories:
	- 报错
---

# 问题描述

在Archlinux下，idea运行tomcat出现，如下错误信息。

`Error running 'Tomcat 9': Error copying configuration files from /usr/share/tomcat8/conf....`

# 问题原因

指定文件权限不足（permission denied）。

# 解决方法

1. 进入**tomcat**目录下的**conf**文件夹，对文件夹下所有文件赋予最大权限。
2. 在进入**conf**——>**Catalina**文件夹，对**localhost**文件也赋予最大权限。

(命令如下，目录文件根据实际修改）

~~~shell
#进入tomcat目录下的conf文件夹
cd /usr/share/tomcat9/conf
 
#赋予权限conf下所有文件最大权限
sudo chmod 777 *

#进入conf——>Catalina文件夹
cd Catalina

#赋予权限conf下所有文件最大权限
sudo chmod 777 *
~~~

