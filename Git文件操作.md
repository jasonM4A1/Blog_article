---
title: Git文件操作
date: 2020-10-13 23:11:45
tags: 
	- Git
	- 基础知识
categories:
	- Git
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201017141128.jpg
---
# Git文件基本操作

## 创建版本库

版本库又名仓库，英文名**repository**。你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

创建一个版本库非常简单，选择一个目录（空或非空都可以），执行下列命令：

格式：

~~~Git
# 将所在目录变成Git可以管理的仓库
git init
~~~

图示：

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201016133931.png)

## 将文件添加到版本库

### Git对于文本文件和二进制文件的区别

首先明确一点所有的版本控制系统，只能跟踪【文本文件】的改动。

+ 如果是TXT文件，Git可以告诉你这个文件每次的详细改动。比如：在第5行加了一个单词“Linux”，在第8行删了一个单词“Windows”。
+ 但如果是图片、视频这些二进制文件，Git虽然也能进行版本控制，但是没法跟踪文件的详细变化。比如：只知道图片从100KB改成了120KB，但到底改了啥，版本控制系统不知道，也没法知道。

### 查看文件状态

格式：

~~~Git
# 查看所有文件状态
git status

# 查看指定文件状态
git status <file>
~~~

图示：

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201009131758.png)

### 忽略文件

有些时候，你必须把某些文件放到Git工作目录中，但又不能提交它们，比如保存了数据库密码的配置文件啦，等等，每次`git status`都会显示`Untracked files ...`，有强迫症的童鞋心里肯定不爽。

好在Git考虑到了大家的感受，这个问题解决起来也很简单，在Git工作区的根目录下创建一个特殊的`.gitignore`文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。

不需要从头写`.gitignore`文件，GitHub已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：https://github.com/github/gitignore

忽略文件的原则是：

1. 忽略操作系统自动生成的文件，比如缩略图等；
2. 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的`.class`文件；
3. 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

### 添加文件到暂存(stage)区

作用：`git add`命令实际上就是把要提交的所有修改放到暂存区（Stage）

+ 所有修改是指：无论是你新创建的处于untracked状态的文件，还是已经提交过又修改了的处于unmoi状态的文件。这些都算是修改，`git add`都会将它们添加到stage。

格式：

~~~Git
# 将指定文件添加到stage区
git add <file1>  <file2>

# 将所有文件添加到stage区
git add .
~~~

图示：

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201009132512.png)

### 提交文件到本地仓库(Repository)

作用：`git commit`可以一次性把暂存区的所有修改提交到分支。

格式：

~~~Git
# 将所有的文件提交到Repository，<messge>为注解
git commit -m <message>

# 将所有已跟踪文件中执行了修改或删除操作的文件提交到本地仓库，不需要add。
# 但新文件，即未跟踪文件是不会被提交的。【不建议使用】
git commit -a -m <message>

# 将新修改追加到前一次的commit-id中,不会增加一个新的commit-id
git commit --amend
~~~

图示：

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201009133403.png)

### 修改文件

前面我们已经提交了`one.txt`和`two.txt`两个文件到本地仓库了，现在我们需要修改`one.txt`文件里的文本内容，往文件里加入两行文字：

~~~txt
德玛西亚永世长存！
你在干什么？
前两行，是之前的内容。
后两行，是新加的内容。
~~~

执行`git status`查看修改后的文件状态：

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201009134643.png)

使用`git status`查看文件状态，这时命令行告诉我们`one.txt`处于已被修改状态。但如果想要明确的知道文件的那些内容被修改了，就要使用`git diff <file>`。

格式：

~~~Git
# 查看文件修改内容
git diff <file>
~~~

图示：

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201009135758.png)

查看过后文件的修改内容后，我们就可以把文件添加到stage，然后再提交到repository。没错，被修改后的文件，和新文件一样，同样需要先`git add <file>`添加到stage区，然后再`git commit`提交到Repository。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201009142438.png)

### 删除文件

~~~Git
# 删除文件
git rm <file>

# 删除文件夹
git rm -f <folder>
~~~

### 临时保存

~~~Git
# 临时保存尚未提交的文件，message为注备
git stash [save message]

# 查看临时保存列表
git stash list

# 简略显示临时保存的文件的改动情况，默认显示第一个
git stash show [stash@{num}]

# 详细显示临时保存的文件的改动情况，默认显示第一个
git stash show [stash@{num}] -p

#应用指定存储,储存列表不变，默认使用第一个存储
git stash apply [stash@{num}]

# 应用指定存储,同时把该储存从列表删除，默认使用第一个存储
git stash pop [stash@{num}]

# 从列表中删除指定储存，默认第一个
git stash drop [stash@{num}]

# 清空储存列表
git stash clear
~~~



### 操作日志

你每提交到本地仓库一版文件，Git都会为你保存一个快照`commit`。这个快照记录了当前仓库里文件的内容状态。一旦你本地误删了文件，或提交了错的文件，都可以从这个历史快照中恢复文件。

~~~Git
# 获取仓库版本历史记录
git log

# 获取仓库版本历史的简短记录
git log --pretty=oneline

# 获取仓库版本历史的简短记录，版本号只显示前几位
git log --oneline 
或
git log --pretty=oneline --abbrev-commit

# 图形化展示提交记录的全部信息,多分支情况下可以查看分支合并图
git log --graph

# 图形化展示提交记录的简略信息，多分支情况下可以查看分支合并图
git log --graph --oneline

# 查看所有分支的所有操作记录
git reflog

# 查看提交详情
git show [commitId] [fileName]
~~~

图示：

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201009145836.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201016135435.png)

### 版本回退

~~~Git
# 查看所有提交过的版本快照（不包括已经被reset回撤的快照）
git log

# 将仓库往前恢复成前一个版本快照
git reset --hard HEAD~1 # HEAD~1指往前一个版本，HEAD~2指往前两个版本
或
git reset --hard HEAD^ # HEAD^指往前一个版本,HEAD^^指往前两个版本

# 查看所有分支的所有操作记录（包含已被删除的commit记录和reset记录）
git reflog

# 将仓库恢复成指定版本号的快照
git reset --hard <commit id> # 可以往前，可以往后
~~~

图示：

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201009154511.png)

