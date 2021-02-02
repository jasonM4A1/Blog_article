---
title: IDEA 服务器热部署详解（On Update action/On frame deactivation）
date: 2021-02-02 01:27:46
tags:
	- JavaWeb
	- Idea
	- 配置
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202012836.png
---

场景：一般服务器（比如tomcat，jboss等）启动以后，我们还需要进一步修改java代码，或者是jsp代码。一般来说，改完重启以后才会生效。但如果配置了服务器的热部署，就可以改完代码后立即生效，而不是重启服务器再生效。这样就会节省大量时间！

目前有两个选项：

On Update action : 顾名思义，当代码改变的时候，需要IDEA为你做什么；

On Frame deactivation : 当失去焦点（比如你最小化了IDEA窗口），需要IDEA为你做什么。

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220174809.png)

-————————————————————————————————————————–

下面以tomcat为例，并且使用war exploded方式部署

具体配置如下：

1.打开tomcat的相关配置，右上角Edit Configurations ，找到tomcat的配置项

2.在配置项中找到On Update action与On Frame deactivation选项

On Update action 里面有四个选项（一般选Update classes and resources）：

-Update resources ：如果发现有更新，而且更新的是资源文件（*.jsp，*.xml等，不包括java文件）,就会立刻生效

-Update classes and resources ： 如果发现有更新，这个是同时包含java文件和资源文件的，就会立刻生效

 这里需要注意一下：在运行模式下，修改java文件时不会立刻生效的；而debug模式下，修改java文件时可以立刻生效的。当然，两种运行模式下，修改resources资源文件都是可以立刻生效的。

-Redploy ： 重新部署，只是把原来的war删掉，不重启服务器

-Restart ： 重启服务器

On Frame deactivation：

-Do nothing : 不做任何事 （一般推荐这个，因为失去焦点的几率太大）

-Update resources : 失去焦点后，修改的resources文件都会立刻生效

-Update classes and resources ： 失去焦点后，修改的java ，resources文件都会立刻生效（与On update action中的Update classes and resources一样，也是运行模式修改的java文件不会生效，debug模式修改的java文件会立刻生效）

-————————————————————————————————————————–

另外，如果Artifact是war包形式的话，On Update action与On frame deactivation中的选项也是不一样的：没有Update resources和 Update classes and resources这种选项，取而代之的是Hot Swap Classes选项，本质的意思是一样的。

------

IntelliJ IDEA默认文件是自动保存的，但是手头有个项目jsp文件改动后，在tomcat中不能立即响应变化。想要jsp文件改动后立刻看到变化，可以通过修改配置来实现。

在IDEA tomcat 中server的配置里，有个On frame deactivation，选择Update classes and resources。另外有个配置on update action，就是手动操作的时候采取什么动作，可以重启服务器，也可以像上面一样更新类和资源文件，我选的是Update classes and resources，也可以选择Redeploy。



On update action：当发现更新时的操作 选择Update classes and resources

On frame deactivation：当IDEA 切换时的操作 （比如缩下去、打开网页等） 选择Update classes and resources

 可是当前项目没有Update classes and resources这个选项，有个Hot Swap classes。这是由于服务器添加的Artifact类型问题，一般一个module对应两种类型的Artifact，一种是war，一种是war explored。war就是已war包形式发布，当前项目是这种形式，在这种形式下On frame deactivation配置没有Update classes and resources选项。war explored是发布文件目录，选择这种形式，On frame deactivation中就出现Update classes and resources选项了。

> 转载自：[(6条消息) IDEA 服务器热部署详解（On Update action/On frame deactivation）_Hern（宋兆恒）-CSDN博客](https://blog.csdn.net/qq_36761831/article/details/88780777)