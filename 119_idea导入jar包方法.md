---
title: idea2020.2导入jar包方法
date: 2021-02-02 05:33:49
tags:
	- Idea
	- JavaWeb
	- 配置
categories:
	- JavaWeb
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202053451.jpg
---

# 前言

我在学习JDBC的时候，总是会遇到一些新的jar包需要添加到项目中，网上关于如何添加jar包到idea项目的教程五花八门的。导致我用的也迷迷糊糊的，最后我用了半个多小时，百度+自己试错总结了一套idea如何导入jar包的教程步骤。

# 方法一：复制jar包到项目目录中

第一种方法，需要复制或移动jar包到，idea项目目录中。然后`Add as Library`。

1. 在项目中新建一个目录，并命名为`libs`。

   [![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205212521.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205212521.png)

   [![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205212646.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205212646.png)

   [![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205212724.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205212724.png)

2. 将你需要导入的jar包统一copy到这个`libs`目录下

   [![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205214702.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205214702.png)

3. 右键`libs`目录，选择`Add as Library...`

   [![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205214800.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205214800.png)

   [![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205214905.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205214905.png)

4. 结果所图所示，这些jar都变成了可展开的了

   [![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205215027.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205215027.png)

# 方法二：直接使用原地址jar包

这二种方法，直接使用jar包的原地址，不会去复制和移动这个jar包。而是将这个jar包作为外部库来使用。

[![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205220028.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205220028.png)

[![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205220114.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205220114.png)

[![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205220235.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205220235.png)

[![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205220329.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205220329.png)

[![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205220454.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205220454.png)

# 方法三：使用Maven来自动下载管理jar包

上面两个方法其实都挺麻烦的，只适合我们初步学习的时候使用。我们在实际开发过程中，肯定不会用这么笨的方法，手动下载导入Jar包的。

而是去使用Maven来帮助我们下载和管理Jar包，这样既方便快捷，也可以帮助我们避免例如Jar包冲突等问题。

关于Maven的使用方式，你可以查看我的这篇[文章](https://www.kuangstudy.com/bbs/1356634744057495554)

