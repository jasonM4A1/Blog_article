---
title: Hexo主题Butterfly报错
date: 2020-10-13 21:50:00
tags: 博客
---

# Hexo主题Butterfly报错

直接用github上作者提供的代码

```
git clone -b master [https://github.com/jerryc127/hexo-theme-butterfly.git](https://github.com/jerryc127/hexo-theme-butterfly.git) themes/Butterfly
```

结果是页面根本无法正常渲染，只有一行字：

```
extends includes/layout.pug block content include ./includes/mixins/post-ui.pug #recent-posts.recent-posts +postUI include includes/pagination.pug
```

## 问题解决

参照原作者提供的文章[Butterfly](https://links.jianshu.com/go?to=https%3A%2F%2Fdocs.jerryc.me%2F%23%2Fconfig%2Fquestion)

按照方法安装`npm install hexo-renderer-pug hexo-renderer-stylus --save`然后打开

**丝滑进入**



作者：SightVanish
链接：https://www.jianshu.com/p/aa936a8369fb
来源：简书