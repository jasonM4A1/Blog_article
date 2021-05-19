---
title: Idea Services Debug调试基本教学
date: 2021-02-27 18:04:44
tags:
	- Idea
	- Debug
	- Service
Categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/wallhaven-498e2w.png
---

# 开始进行Debug

1. 以Debug方式启动Tomcat运行代码
2. 在所需位置添加断点

# Debug常用按钮

![image-20210227181959757](/Users/bernardo/Library/Application Support/typora-user-images/image-20210227181959757.png)

1. 让代码往下执行一行
2. 可以进入当前方法体内（自己写的代码，非框架源码）
3. 强制进入当前方法体内
4. 跳出当前方法体外
5. 从方法体中跳出去，相当于是返回到调用该方法的那一步，继续下一步还可以再次进入到方法体中
6. 停在光标所在行（相当于临时断点）

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210227184404.png)

1. 停止服务器
2. 一直运行到下一个断点（如果中间有其他断点，会被中断）
3. 临时禁用所有断点

# 变量窗口

用来查看当前方法范围内所有有效的变量

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210227184805.png)

# 方法调用栈窗口

用来查看当前线程有哪些方法调用信息（下面的调用上一行的方法）

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210227184942.png)

> 详细的Debug使用，请查考这篇文章[Idea Debug按钮作用-知乎](https://zhuanlan.zhihu.com/p/341571902)