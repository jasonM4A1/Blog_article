---
title: centos7终端中文乱码
date: 2020-10-13 23:07:18
tags: Linux
---
**问题**

出现乱码，如下：

~~~
������ϵ���

=====================================================================
 Package             �ܹ�      �汾                 Դ          ��С
=====================================================================
���ڰ�װ:
 git                 x86_64    1.8.3.1-23.el7_8     updates    4.4 M
Ϊ��������װ:
 perl-Error          noarch    1:0.17020-2.el7      base        32 k
 perl-Git            noarch    1.8.3.1-23.el7_8     updates     56 k
 perl-TermReadKey    x86_64    2.30-20.el7          base        31 k
 rsync               x86_64    3.1.2-10.el7         base       404 k

�����Ҫ
=====================================================================
��װ  1 ����� (+4 ���������)

~~~

**解决方法**

1. 查看服务器编码的命令`locale -a`，看里面是否有下面四项：

   ~~~
   zh_CN.gb18030
   zh_CN.gb2312
   zh_CN.gbk
   zh_CN.utf8
   ~~~

   如果有，则不用安装，如果没有，需要重新安装，使用`yum install kde-l10n-Chinese`

2. 查看服务器编码的命令`locale`，看否是`zh_CN.UTF-8`

   ~~~
   [root@iz2ze0pget2kzcthzz3k6oz ~]# echo $LANG
   zh_CN.UTF-8
   ~~~

   如果是，跳过。如果不是，则需要执行`vim /etc/locale.conf`。用下列内容覆盖原内容。

   ~~~
   LANG="zh_CN.GB18030"
   LANGUAGE="zh_CN.GB18030:zh_CN.GB2312:zh_CN"
   SUPPORTED="zh_CN.UTF-8:zh_CN:zh:en_US.UTF-8:en_US:en"
   SYSFONT="lat0-sun16"
   ~~~

3. 修改终端xshell编码为UTF-8

   ![](https://gitee.com/CharlieLiLi/pictureHost/raw/master/20201013171737.png)

4. 执行`vim /etc/sysconfig/i18n`（可能这个文件本不存在），粘贴以下文本

   ~~~
   LANG="zh_CN.UTF-8"
   ~~~

5. 执行`vim /etc/profile`（与前几个文件不同，这个文件内容较多），直接空一行粘贴以下文本到最后就行了。

   ~~~
   export LANG="zh_CN.UTF-8"
   ~~~

   