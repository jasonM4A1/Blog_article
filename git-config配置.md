---
title: git_config配置
date: 2020-10-13 23:11:23
tags: 
	- Git
	- 基础知识
categories:
	- Git
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201017141027.jpg
---
# git config配置

## 优先级

在git中，我们使用git config 命令用来配置git的配置文件，git配置级别主要有以下3类：

1. 仓库级别`local`【优先级最高】
2. 用户级别`global`【优先级次之】
3. 系统级别`system`【优先级最低】

## 配置文件目录

+ `git`仓库级别对应的配置文件是当前仓库下的`.git/config`。

  ![](https://gitee.com/jasonm4a1/pictureHost/raw/master/20201008133713.png)

+ `git`用户级别对应的配置文件是用户宿主目录下的`~/.gitconfig`。

  ![](https://gitee.com/jasonm4a1/pictureHost/raw/master/20201008134639.png)

+ `git`系统级别对应的配置文件是`git`安装目录下的`/ect/gitconfig`。

  ![](https://gitee.com/jasonm4a1/pictureHost/raw/master/20201008134715.png)

## 配置命令

+ `git config --local -l`查看仓库配置【必须要进入到具体的目录下，比如要查看Blog仓库的配置信息】

  ![](https://gitee.com/jasonm4a1/pictureHost/raw/master/20201008195630.png)

+ `git config --global -l`查看用户配置

  ![](https://gitee.com/jasonm4a1/pictureHost/raw/master/20201008195740.png)

+ `git config --system -l`查看系统配置

  ![](https://gitee.com/jasonm4a1/pictureHost/raw/master/20201008195804.png)

+  `git config -l`查看所有的配置信息，依次是系统级别、用户级别、仓库级别

  ![](https://gitee.com/jasonm4a1/pictureHost/raw/master/20201008195507.png)
  
  + `git config --global --edit`直接对配置文件进行编辑

**补充**：

1. `git config -e`编辑配置文件

   ~~~Git
   git config --local -e	//编辑仓库级别配置文件
   git config --global -e	//编辑用户级别配置文件
   git config --system -e 	//编辑系统级别配置文件
   ~~~

2. `git config`添加配置项目（必须要配置的）

   ~~~Git
   git config --global user.name "you name"
   git config --global user.email "you@example.com"
   ~~~

   这里的操作是添加用户级别的配置信息，相当于修改用户宿主目录下面的配置文件。

## 配置文件权限级别原理

对于`git`来说，配置文件的权重是仓库>全局>系统。`Git`会使用这一系列的配置文件来存储你定义的偏好，它首先会查找`/etc/gitconfig`文件（系统级），该文件含有对系统上所有用户及他们所拥有的仓库都生效的配置值。接下来`Git`会查找每个用户的`~/.gitconfig`文件（全局级）。最后`Git`会查找由用户定义的各个库中`Git`目录下的配置文件`.git/config`（仓库级），该文件中的值只对当前所属仓库有效。

## 增加配置项

~~~Git
git config [--local|--global|--system] section.key value

# 参数详解：
# [--local|--global|--system] 可选的，对应本地、全局、系统不同级别的设置
# section.key 对应区域下的键
# value 对应的值
~~~

在student区域下添加一个名称为sum值为32的配置项，执行结果如下：

![](https://gitee.com/jasonm4a1/pictureHost/raw/master/20201008195217.png)

## 获取单个配置项

~~~Git
git config [--local|--global|--system] --get section.key
~~~

只获取student域下面的`age = 32`这一条配置信息，操作如下：

![](https://gitee.com/jasonm4a1/pictureHost/raw/master/20201008195316.png)

如果获取一个section不存在的key值，不会返回任何值

如果获取一个不存在的section的key值，则会报错

## 删除一个配置项

~~~Git
git config [--local|--global|--system] --unset section.key
~~~

删除前面我们添加到students域下的`age = 32`这条配置信息，操作如下：

![](https://gitee.com/jasonm4a1/pictureHost/raw/master/20201008195406.png)

## 所有config命令参数

~~~Git
语法: git config [<options>]        
        
文件位置        
    --global                  #use global config file 使用全局配置文件
    --system                  #use system config file 使用系统配置文件
    --local                   #use repository config file    使用存储库配置文件
    -f, --file <file>         #use given config file    使用给定的配置文件
    --blob <blob-id>          #read config from given blob object    从给定的对象中读取配置
        
动作        
    --get                     #get value: name [value-regex]    获得值：[值]名[正则表达式]
    --get-all                 #get all values: key [value-regex]    获得所有值：[值]名[正则表达式]
    --get-regexp          #get values for regexp: name-regex [value-regex]    得到的值根据正则
    --get-urlmatch            #get value specific for the URL: section[.var] URL    为URL获取特定的值
    --replace-all             #replace all matching variables: name value [value_regex]    替换所有匹配的变量：名称值[ value_regex ]
    --add                     #add a new variable: name value    添加一个新变量：name值
    --unset                   #remove a variable: name [value-regex]    删除一个变量名[值]：正则表达式
    --unset-all               #remove all matches: name [value-regex]    删除所有匹配的正则表达式：名称[值]
    --rename-section          #rename section: old-name new-name    重命名部分：旧名称 新名称
    --remove-section          #remove a section: name    删除部分：名称
    -l, --list                #list all    列出所有
    -e, --edit            #open an editor    打开一个编辑器
    --get-color               #find the color configured: slot [default]    找到配置的颜色：插槽[默认]
    --get-colorbool           #find the color setting: slot [stdout-is-tty]    发现颜色设置：槽[ stdout是TTY ]
        
类型        
    --bool                    #value is "true" or "false"    值是“真”或“假”。
    --int                     #value is decimal number    值是十进制数。
    --bool-or-int             #value is --bool or --int    值--布尔或int
    --path                    #value is a path (file or directory name)    值是路径（文件或目录名）
        
其它        
    -z, --null                #terminate values with NUL byte    终止值与null字节
    --name-only               #show variable names only    只显示变量名
    --includes                #respect include directives on lookup    尊重包括查找指令
    --show-origin             #show origin of config (file, standard input, blob, command line)    显示配置（文件、标准输入、数据块、命令行）的来源
~~~

## 更多配置项

~~~Git
git config --global color.ui true   #打开所有的默认终端着色
# 配置别名
git config --global alias.ci commit   #别名 ci 是commit的别名
[alias]  
co = checkout  
ci = commit  
st = status  
pl = pull  
ps = push  
dt = difftool  
l = log --stat  
cp = cherry-pick  
ca = commit -a  
b = branch 

user.name  #用户名
user.email  #邮箱
core.editor  #文本编辑器  
merge.tool  #差异分析工具  
core.paper "less -N"  #配置显示方式  
color.diff true  #diff颜色配置  
alias.co checkout  #设置别名
git config user.name  #获得用户名
git config core.filemode false  #忽略修改权限的文件  
~~~

