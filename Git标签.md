---
title: Git标签
date: 2020-10-13 23:13:11
tags: Git
---
# Git标签

## 作用

git的标签tag相当于commitId的别名，可以让人更容易的去记住它。

## 命令

### 创建标签

~~~Git
# 为指定的版本号打标签，默认为最新的一次
git tag [-a] <tagName> [-m message] <commitId>

# -a指定标签名,省略效果一样
# -m为注释信息
~~~

### 查看标签

~~~Git
# 查看所有标签
git tag

# 查看指定标签上详细的提交信息
git show <tagName>
~~~

### 删除标签

~~~Git
# 删除指定名字的本地标签
git tag -d <tagName>

# 删除指定名字的远程标签
git push origin :refs/tags/<tagName>
~~~

### 推送标签

~~~Git
# 推送指定名字的标签到远程
git push origin <tagName>

# 推送全部未推送的标签到远程
git push origin --tags
~~~

