---
title: 解决tomcat启动时中文乱码的问题
date: 2021-02-03 02:13:48
tags:
	- JavaWeb
	- Tomcat
	- 乱码
categories:
	- 踩坑记录
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203022101.png
---

# 问题描述

当我们下载配置好Tomcat后，为了测试第一次启动Tomcat时，可能会遇到中文乱码的问题。如图所示：

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203022324.png)

虽然Tomcat还是启动成功了，但是我们无法看到细节，很不舒服。

# 解决方法

进入你Tomcat的安装目录，再进入`conf`目录，使用文本编辑器打开`logging.properties`文件。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203022521.png)

然后大约到位到47行，或者直接搜索`ConsoleHandler.encoding`，找到如图所示的内容。然后修改编码`UTF-8`为`GBK`即可。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203022727.png)

再次启动Tomcat，发现乱码问题已经解决！

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210203022929.png)