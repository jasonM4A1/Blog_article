---
title: Git文件状态
date: 2020-10-13 23:11:36
tags: 
	- Git
	- 基础知识
categories:
	- Git
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201017135856.jpg
---
# Git文件四种状态

1. `Untracked`：未跟踪状态；此文件只是存于文件夹中，但并没有加入到`Git`库中，不参与版本控制。
   + 执行`Git add`将文件状态变成`Staged`。

2. `Staged`：暂存状态。
   + 执行`git commit`则将修改同步到库中，这时库中的文件和本地文件又变为一致，文件状态变为`Unmodify`。
   + 执行`git reset HEAD filename`取消暂存，文件状态变为`Modified`。

+ `Unmodify`：未修改状态；文件已经入库，未修改，即版本库中的文件快照内容与文件夹中完全一致。
  + 如果它被修改，则变成`Modified`状态。
  + 执行`git rm`移除版本库，则文件状态变为`Untracked`。
+ `Modified`：已修改状态；文件已修改，但仅仅是修改，并没有进行其他的操作。
  + 执行`git add`文件状态变为`Staged`；
  + 执行`git checkout`则丢弃修改，文件状态返回到`Unmodify`（`git checkout`即从库中取出文件，覆盖当前修改）。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201008184145.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201008191521.png)



