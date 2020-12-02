---
title: MySQL插入记录出现Incorrent string value错误
date: 2020-11-26 11:14:49
tags:
	- MySQL
	- 数据库
categories:
	- MySQL
	- 报错
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201126112509.jpg
---

# MySQL插入记录出现Incorrent string value错误

## 问题描述

你是否遇到过类似以下错误？

java.sql.SQLException: Incorrect string value: '\xF0\x9F\x92\x9C' for column 'content' at row 1.

产生这种异常的原因在于，mysql中的utf8编码最多会用3个字节存储一个字符，如果一个字符的utf8

编码占用4个字节（最常见的就是ios中的emoji表情字符），那么在写入数据库时就会报错。

mysql从5.5.3版本开始，才支持4字节的utf8编码，编码名称为utf8mb4（mb4的意思是max bytes 4），这种编码方式最多用4个字节存储一个字符。

## 解决方法

1. 修改MySQL的配置文件：windows 查找`C:\ProgramData\MySQL\MySQL Server 5.7\my.ini`，Linux查找my.cnf。在配置文件最后添加如下字段：(先备份)

   ~~~ini
   [mysqld]
   character-set-server=utf8mb4
    
   [mysql]
   default-character-set=utf8mb4
   ~~~

2. 重启MySQL

   ~~~bash
   # 关闭MySQL服务
   net stop <mysql服务名称>
   
   # 启动Mysql服务
   net start <mysql服务名称>
   ~~~

   

3. 按顺序执行下面两个SQL语句：

   ~~~sql
   -- 修改数据库字符集
   ALTER DATABASE 数据库名 CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci
   
   -- 修改表字符集
   ALTER TABLE 表名 CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
   ~~~
   
4. 如果觉得步骤三太过麻烦，也可以重建数据库，进行数据迁移。这样就不需要手动执行命令来修改字符集。



