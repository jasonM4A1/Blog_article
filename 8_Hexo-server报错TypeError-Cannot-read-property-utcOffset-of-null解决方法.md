---
title: 'Hexo server报错TypeError: Cannot read property ''utcOffset'' of null解决方法'
date: 2020-10-17 00:08:18
tags: 
	- 博客
	- hexo
	- 报错 
categories:
	- 博客
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201017141138.jpg
---

最近刚刚开始使用Hexo，新建了一篇article，运行hexo server时候总是报错Cannot read property 'offset' of null。

最后发现是因为手贱把_config.yml中的时区timezone改成了beijing哭笑不得。

解决办法就是把timezone改成 Asia/Shanghai 就好了。

亲测有效

> 转载于:https://www.cnblogs.com/mmzuo-798/p/10510225.html