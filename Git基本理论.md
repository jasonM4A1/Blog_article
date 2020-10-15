---
title: Git基本理论
date: 2020-10-13 23:11:09
tags: Git
---

# Git基本理论（核心）

## 四个工作区

1. **工作目录**【Working Directory】

   即你平时存放项目代码的地方。

2. **暂存区**【Stage/Index】

   也叫待提交更新区，在提交进入repo之前，我们可以把所有的更新放在暂存区。用于临时存放你的改动，保存即将提交到文件列表的信息（事实上它只是一个文件）。

3. **本地资源库**【Repository或Git Directory】

   用于安全存放数据的位置，这里有你提交的所有版本信息。其中HEAD指向最新放入仓库的版本。

4. **远程资源库**【Remote Directory】

   托管代码的服务器，相当于把你的本地仓库`clone`到了云盘上。如`Github`、`Gitee`。你可以简单的认为是你项目组中的一台用于远程数据交换的电脑。

![](https://gitee.com/CharlieLiLi/pictureHost/raw/master/20201008140952.png)

我们对下面的这张图进行讲解，以加深对`Git`的理解。

![](https://gitee.com/CharlieLiLi/pictureHost/raw/master/20201008142409.png)

1. `Directory`：使用`Git`管理的一个仓库，其中包含了我们使用的工作空间和`Git`管理空间。
2. `WorkSpace`：里面存放着`Git`进行版本控制的目录和文件。
3. `.git`：存放着`Git`管理信息的目录，在初始化仓库的时候就自动创建完毕。
4. `Index/Stage`：暂存区，或者称为待提交更新区。在提交进入`repo`之前，我们可以把所有的更新临时放在暂存区中。
5. `Local Repo`：本地仓库，一个存放在本地的版本库。
6. `Stash`：隐藏，是一个工作状态保存栈。用于保存/回复`WorkSpeace`中的临时状态。

## 工作流程

1. 首先在工作目录中添加、修改文件。对应文件状态——已修改【modified】
2. 然后将需要进行版本管理的文件放入暂存区域。对应文件状态——已暂存【staged】
3. 将暂存区域的文件提交到`Git`仓库。对应文件状态——已提交【committed】

下图为各个工作区域进行更换的命令图解：

![](https://gitee.com/CharlieLiLi/pictureHost/raw/master/20201008143501.png)