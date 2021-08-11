---
title: hexo博客部署到github后404
date: 2021-01-28 22:01:00
tags: 
    - hexo
    - 博客
    - github
categories:
    - 报错
cover:
    https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210129042151.png
---

# 问题描述
hexo博客部署到github上后，输入域名发现404了。查看github仓库，发现用于绑定域名的文件CNAME消失了，可能是因为本地hexo博客没有CNAME文件导致的。

# 解决方法
将需要上传至github的内容放在source文件夹，例如CNAME、favicon.ico、images等。

