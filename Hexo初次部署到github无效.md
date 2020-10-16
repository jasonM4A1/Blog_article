---
title: Hexo初次部署到github无效
date: 2020-10-13 23:02:56
tags: 博客
---
# Hexo初次部署到github无效

**问题描述**

`hexo s`本地预览没有博客样式任何问题，github page也可以通过`githubName/githubName.github.io`的方式打开。但是使用`hexo d -g`将博客部署到github上后，再次采用`githubName/githubName.github.io`的方式去访问github页面，发现页面仍然没有变化，还是我之前为github page选择的默认样式。等于说，本地的博客内容没有再github上生效。

**解决思路**

我去github仓库查看，发现仓库根本没有更新。我开始怀疑是本地的文件根本没有推送到github仓库中，于是我去查看一个github仓库的提交记录。发现main 分支中的文件确实没有得到更新，但是我看到还一个分支master。我想到在本地博客_config.yml中`bracnh`后面我是配置的为`master`。所以我在github中把分支切换到了master，发现master中的分支果然在不久前有提交。所以就排除了提交失败的问题。我又抛出github page设置看看了，发现竟然有branch的选项，默认设置的是main分支。也就是说现在我的github page显示的是main分支的内容。

~~~
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type:  git
  repo:  https://github.com/jasonM4A1/jasonM4A1.github.io.git
  branch:  master
~~~

**解决方法**

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201013153839.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201013153856.png)