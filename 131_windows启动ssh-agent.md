---
title: windows启动ssh-agent
date: 2021-02-20 09:09:25
tags:
	- Windows
	- git
	- ssh
categories:
	- 踩坑记录
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210220091020.jpg
---

# 问题描述

之前一直是使用git bash进行博客部署提交的，最近开始使用更好看点的windows Terminal。但是进行博客部署提交的时候，出现了现在问题。

也就是腾讯的coding代码仓库认证失败，看网上说好像是coding更新了，ssh的拉取方式好像不行了，所以不要用coding了。

不过还是学习了一下ssh-agent如何启动，和添加私钥。

```
On branch master
nothing to commit, working tree clean
git@e.coding.net: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
FATAL {
  err: Error: Spawn failed
      at ChildProcess.<anonymous> (D:\Blog\node_modules\hexo-deployer-git\node_modules\hexo-util\lib\spawn.js:51:21)
      at ChildProcess.emit (events.js:315:20)
      at ChildProcess.cp.emit (D:\Blog\node_modules\cross-spawn\lib\enoent.js:34:29)
      at Process.ChildProcess._handle.onexit (internal/child_process.js:277:12) {
    code: 128
  }
} Something's wrong. Maybe you can find the solution here: %s https://hexo.io/docs/troubleshooting.html
```

# 解决方法

进入设置——>应用和功能，选择 可选功能。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210220094247.png)

在添加功能中输入`openssh`，将服务端和客户端都安装上。我已经安装了，所以没用查找到。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210220094332.png)

然后进入`服务管理`，将openssh相关的服务都设置为自动启动，并启动。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210220094517.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210220094549.png)

打开windows Terminal，输入命令(如果有多个私钥的话，我们需要添加默认的私钥)

```
ssh-add github私钥
ssh-add coding私钥
```

