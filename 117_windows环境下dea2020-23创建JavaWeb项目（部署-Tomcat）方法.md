---
title: windows环境下Idea2020.23创建JavaWeb项目（部署 Tomcat）方法
date: 2021-02-02 05:24:53
tags:
	- JavaWeb
	- Windows
	- Idea
	- Tomcat
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202052642.png

---

1.创建项目不再是Java Enterprise了,而是先New 一个普通Java项目!

[![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201211003617.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201211003617.png)

2.创建项目后,选择Run->Edit Configuration->左上角加号->Tomcat Server(注意不是TomEE )->Localstep 1
在这里插入图片描述

[![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201211003636.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201211003636.png)

[![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201211003653.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201211003653.png)

3.点击Application右边的Configure,找到你放置的Tomcat的目录,点击OK

[![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201211003716.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201211003716.png)

4.选择你运行项目调试项目的浏览器,我的电脑安装的是Chrome,你可以自行选择,然后ok

[![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201211003728.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201211003728.png)

5.回到项目界面,右键项目名称->add framwork support->选中Web Application->默认勾选创建web.xml

[![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201211003741.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201211003741.png)

[![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201211003750.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201211003750.png)

6.回到刚刚配置Tomcat的页面,选择第二个选项卡Deployment->右边的加号->选择Artifact…

[![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201211003803.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201211003803.png)

[![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201211003810.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201211003810.png)

7.项目就添加完成了,下面的Application context表示虚拟路径,你可以改成”/“,不写项目名,这样在后面开发测试时就不用频繁的填写路径了,当然不一定要改,个人喜好.点击OK,配置部分完成

[![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201211003823.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201211003823.png)

8.项目默认创建了一个index.jsp, 把标签 E N D ENDEND 中间的部分 改成 自己想写的一句话
你可以尝试启动看一下,右键run,启动需要时间
页面访问成功

[![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201211003834.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201211003834.png)

当然你可以选择先add framwork support再edit configurations效果是一样的,

配置正确之后

[![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201211003850.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201211003850.png)

这一部分的Add Configuration文字会变为”Tomcat的图标+Tomcat+版本号”,同时灰色的运行按钮会变成绿色,无需右键Run,可以直接点击run