---
title: Idea2020.2创建maven项目.md
date: 2021-02-01 23:27:34
tags:
	- JavaWeb
	- Idea
	- Maven
	- 配置
categores:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210201233133.jpg
---

# 下载和配置Maven

## 下载并解压Maven

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202031521.png)

下载后，解压到一个任意位置，但是你要记住这个位置！

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202031511.png)

## 创建和配置本地Maven仓库

进入你下载解压好的Maven文件夹，新建一个目录`maven-repo`。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202085159.png)

> 如果你不自己创建本地仓库，那么Maven会在你的用户目录下创建一个`.m2`目录作为本地仓库。但是这个可能会出现Maven创建失败的情况，如果出现情况文章最后的解决方法。无论无何还是建议你自己创建本地仓库！

后还是打开`conf`文件夹中的`setting.xml`文件，搜索`<!-- localRepository`，在此条注释后加入刚才新建的目录地址，用来指定本地仓库地址。

```
<!-- localRepository
   | The path to the local repository maven will use to store artifacts.
   |
   | Default: ${user.home}/.m2/repository
  <localRepository>/path/to/local/repo</localRepository>
  -->
  <localRepository>D:\apache-maven-3.6.3\maven-repo</localRepository>
```

## 配置Maven仓库镜像

进入你下载解压好的Maven文件夹，打开`conf`文件夹中的`setting.xml`文件，搜索`<mirrors>`，在这里面加入几个镜像。

```
    <mirror>
      <id>aliyunmaven</id>
      <mirrorOf>central</mirrorOf>
      <name>aliyun maven</name>
      <url>https://maven.aliyun.com/repository/public </url>
    </mirror>
    <mirror>
      <id>aliyunmaven</id>
      <mirrorOf>central</mirrorOf>
      <name>阿里云公共仓库</name>
      <url>https://maven.aliyun.com/repository/central</url>
    </mirror>
    <mirror>
      <id>repo1</id>
      <mirrorOf>central</mirrorOf>
      <name>central repo</name>
      <url>http://repo1.maven.org/maven2/</url>
    </mirror>
    <mirror>
      <id>aliyunmaven</id>
      <mirrorOf>apache snapshots</mirrorOf>
      <name>阿里云阿帕奇仓库</name>
      <url>https://maven.aliyun.com/repository/apache-snapshots</url>
    </mirror>
  </mirrors>
```

> 一定要注意缩进格式！不然会后面的步骤会报错的！
>
> 可以配置多个镜像，每个镜像使用标签`<mirror></mirror>`包括起来(不带s)。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202032256.png)

## 配置Maven环境变量

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202085448.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202085519.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202085559.png)



1. 新建系统变量，变量名为：`M2_HOME`，变量值为：`D:\JavaSoftware\apache-maven-3.6.3\bin`（路径根据自己的情况修改）。
2. 新建系统变量，变量名为：`MAVEN_HOME`，变量值为：`D:\JavaSoftware\apache-maven-3.6.3`（路径根据自己的情况修改）。
3. 在系统变量Path中新建，变量值为：`%MAVEN_HOME%\bin`（这个不用修改）。

> 自此Maven下载和配置已经完成，下面就可以直接使用Idea创建Maven项目了。

# 使用Idea创建Maven项目

到此为止Maven的基本配置已经完成了，下面我们要进行的是在Idea中创建Maven项目。

首先新建一个工程。

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220150130.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202090212.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202030046.png)

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220032227.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202030037.png)

# 在Idea中配置Maven项目

首先进入设置中，一定要检查一个下面框住的三个选项是否符合你需要的设置。因为我们是使用的自己下载的Maven，所以这里一定要确保不是使用的Idea自带的Maven路径。很多错误都是处在这里!

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202090908.png)

我们每次使用Idea创建Maven后，都要手动更改Maven的更目录文件夹、配置文件和本地仓库。我们可以全局对Maven进行设置，避免每次重复。如图：

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220163544.png)

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220163616.png)

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220163724.png)

# Maven远程库的使用

终于到重头戏了，我们之所以使用Maven，就是为了让Maven帮助我们管理项目和Jar包。下面我们来学习怎么使用Maven导入Jar包。

首先进入https://mvnrepository.com/，找到自己需要的Jar包，然后复制配置代码，粘贴到pom.xml对应的位置。

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220162348.png)

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220162209.png)

> 这里需要注意的是：我们需要先写一对`dependencies`的xml标签，然后再把我们复制的Maven库Jar包代码粘进入。

然后Maven就会自动将这jar包所需的相关Jar包都给download下来。我们就不用再自己辛苦的下载、导入Jar包了，开心~

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220162538.png)

这里需要注意的是，自己Maven配置是否正确。需要保证Maven本地仓库和配置文件是自己设置的（否则粘贴进入的代码，会报红）。如图：

![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201220163302.png)



----

# 没有创建Maven本地仓库解决方法

如果你**没有在自己下载的Maven目录中创建本地仓库`maven-repo`**，你可能会遇到Maven并没有给在你的用户目录下生成`.m2`目录的问题。下面是解决方法：

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202031502.png)

下一步所需命令：`mvn help:describe -Dplugin=help -e -X`。

注意这个命令执行需要较长的时间，请耐心等待吧（反正我等了至少一二十分钟）。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202031448.png)

下一步所需命令：`mvn help:system`。

其实网上说的是直接执行这一个命令，但是我的设备这个命令执行的超级慢，所以加上了上个命令来解决这个问题。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202031440.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202031721.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202032004.png)

> 当然你直接在自己下载Maven目录创建一个本地仓库`maven-repo`，就不用做上面的麻烦事了。

