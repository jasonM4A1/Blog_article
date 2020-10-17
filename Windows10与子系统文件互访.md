---
title: Windows10与子系统文件互访
date: 2020-10-17 11:46:17
tags: 子系统
categories: 
	- 其他
cover:	
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201017120747.jpg
---

## Windows10环境下访问Ubuntu文件系统

Windows10环境下可以直接读取Ubuntu内的文件，但是不能往里面写入文件，在资源管理器中直接就可以访问。

```bash
C:\Users\xxx1\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu18.04onWindows_79rhkp1fndgsc\LocalState\rootfs\home\xxx2
```

其中，xxx1表示windows系统的用户名，xxx2表示子系统Ubuntu的用户名。

## 在子系统Ubuntu终端模式下访问Windows10文件系统

在终端模式下输入：

```bash
cd /mnt
1
```

/mnt文件夹中包含了Windows10中所有的盘符。
例如需要访问D盘，则只需输入：

```bash
cd /mnt/d
```

> <a href="http://www.w3school.com.cn">查看原文点这里</a>