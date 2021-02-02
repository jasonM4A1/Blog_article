---
title: Manjaro-kde环境下Idea2020-3创建JavaWeb项目-部署Tomcat-方法
date: 2021-01-31 02:16:32
tags:
	- JavaWeb
	- Tomcat
	- Linux
	- Manjaro
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210201092302.png
---

# 前言

之前我一直在使用windows10系统进行的Java开发的学习，最近为了逼迫自己学习Linux的相关知识，索性直接换到的Manjaro-kde上，前前后后折腾的好几天。

由于更换系统和我备份文件不够及时，windows上的文件都给丢失了一部分，其中就包括Blog的文件。所以现在准备重写丢失的博客文章，权当是复习。

今天是关于idea2020-3创建JavaWeb项目，并部署Tomcat的内容。

# 安装Tomcat

我使用的是Arch Linux下的Manjaro kde，所以我直接直接命令安装（unbunt等linux发行版本安装方式可自行百度，大同小义）。

如果命令行安装失败，可以去tomcat官网下载tar.gz包来进行安装，教学可以自行百度（我懒）。

```shell
# 安装tomcat9
❯ pacman -Syu tomcat9

# 查看tomcat9安装位置
❯ whereis tomcat9
tomcat9: /etc/tomcat9 /usr/share/tomcat9 
# /etc/tomcat9 中有server.xml文件用于服务器的配置，不过一般我们不用去修改它，除非端口号冲突等
# /usr/share/tomcat9 这是我们需要配置到环境变量 和 idea的项目设置路径中

#为tomcat配置环境变量（其实这一步是不是必须的，我也不清楚，但还是配置上为好）
❯ sudo vim /etc/profile

# 在文件最后追加以下内容：
export CATALINA_HOME=/usr/share/tomcat9    #写入前面查看的Tomcat安装路径
export CLASSPATH=.:$JAVA_HOME/lib:$CATALINA_HOME/lib # 不变
export PATH=$PATH:$CATALINA_HOME/bin # 不变

# 进入tomcat的bin目录
❯ cd /usr/share/tomcat9/bin

# 使用管理员权限执行startup.sh，来手动启动tomcat服务器
❯ sudo ./startup.sh
Using CATALINA_BASE:   /usr/share/tomcat9
Using CATALINA_HOME:   /usr/share/tomcat9
Using CATALINA_TMPDIR: /usr/share/tomcat9/temp
Using JRE_HOME:        /usr/lib/jvm/java-8-openjdk
Using CLASSPATH:       /usr/share/tomcat9/bin/bootstrap.jar:/usr/share/tomcat9/bin/tomcat-juli.jar
Using CATALINA_OPTS:   
Tomcat started. # 这表示tomcat已经启动，安装配置成功

# 使用管理员权限执行shutdown.sh，来手动关闭tomcat服务器（避免后面idea启动tomcat端口冲突）
❯ sudo ./shutdown.sh
Using CATALINA_BASE:   /usr/share/tomcat9
Using CATALINA_HOME:   /usr/share/tomcat9
Using CATALINA_TMPDIR: /usr/share/tomcat9/temp
Using JRE_HOME:        /usr/lib/jvm/java-8-openjdk
Using CLASSPATH:       /usr/share/tomcat9/bin/bootstrap.jar:/usr/share/tomcat9/bin/tomcat-juli.jar
Using CATALINA_OPTS:   
```

# 使用idea创建JavaWeb项目，并配置tomcat

Idea2020.3创建JavaWeb的方式和以前不再一样了，创建项目不再是Java Enterprise了,而是先New 一个普通Java项目。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210131025709.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210131024756.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210131024754.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210131024756.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210131024755.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210131031157.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210131031158.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210131031156.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210131031154.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210131031154.png)
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210131031153.png)

