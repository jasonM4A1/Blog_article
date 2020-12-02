---
title: MySQL-DCL语句
date: 2020-11-25 23:10:52
tags:
	- MySQL
	- 数据库
categories:
	- MySQL
	- 复习
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201125231153.jpg
---

# MySQL-DCL语句

## 概述

我们现在默认使用的都是 root 用户，超级管理员，拥有全部的权限。但是，一个公司里面的数据库服务器上面可能同时运行着很多个项目的数据库。所以，我们应该可以根据不同的项目建立不同的用户，分配不同的权限来管理和维护数据库。

> 注：mysqld是MySQL的主程序，服务器端。mysql是MySQL的命令行工具，客户端。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201125232739.png)

## 查看用户

所有的用户信息，我们都可以去MySQL自带的数据库`mysql`中的表`user`去查询（密码是加密的）。

~~~sql
-- 切换到mysql数据库
use mysql;

-- 查询user表中的用户信息
select * from user;
~~~

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201126235617.png)

## 创建用户

### 语法

~~~sql
CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
~~~

### 关键字

+ `'用户名'`

  将要创建的用户名

+ `'主机名'`

  指定该用户在哪个主机上可以登录，如果是本地用户可用`localhost`。如果想让该用户可以从任意远程主机登录，可以使用通配符`%`

+ `'密码'`

  该用户的登录密码，密码可以为空，如果为空则该用户可以不需要密码登录服务器

### 代码演示

~~~sql
-- 创建 user1 用户，只能在本地登录 mysql 服务器，密码为 空
CREATE USER 'user1'@'127.0.0.1' IDENTIFIED BY '';

-- 创建 user2 用户可以在远程主机'182.92.151.10'电脑上登录 mysql 服务器，密码为 remote
CREATE USER 'user2'@'182.92.151.10' IDENTIFIED BY 'remote';

-- 创建 user3 用户可以在任何电脑上登录 mysql 服务器，密码为 admin
CREATE USER 'user3'@'%' IDENTIFIED BY 'admin';
~~~

## 删除用户

### 语法

~~~sql
DROP USER '用户名'@'主机名'; -- 要与创建用户时的 '用户名'@'主机名'，保持一致
~~~

### 代码演示

~~~sql
-- 删除 user2 用户
drop user 'user2'@'%';
~~~

## 查看权限

### 语法

~~~sql
SHOW GRANTS FOR '用户名'@'主机名'; -- 要与创建用户时的 '用户名'@'主机名'，保持一致
~~~

### 代码演示

~~~sql
-- 查看 user1 用户的权限
show grants for 'user1'@'localhost';

-- 查看 user2 用户的权限
show grants for 'user2'@'%';
~~~

> usage 是指连接（登陆）权限，建立一个用户，就会自动授予其 usage 权限（默认授予）。

## 用户授权

在我们创建一个用户之处，这个用户是没用任何权限的，我们需要单独给这个用户赋予权限。

### 语法

~~~sql
GRANT 权限1, 权限2 ... ON 数据库名.表名 TO '用户名'@'主机名'; 
-- 要与创建用户时的 '用户名'@'主机名'，保持一致
~~~

### 关键字说明

+ `权限`

  授予用户的权限，如`CREATE`、`ALTER`、`DROP`、`SELECT`、`INSERT`、`UPDATE`、`DELETE`、`GRANT`等的权限。如果要授予所有的权限则使用`All`

+ `数据库名.表名`

  该用户可以操作哪个数据库的哪些表。如果要授予该用户对所有数据库和表的相应操作权限则可用`*`表示，如`*.*`

### 代码演示

~~~sql
-- 给 user1 用户分配对 mydatabase 这个数据库操作的权限：创建表，修改表，插入记录，更新记录，查询
grant create, alter, insert, update, select on mydatabase.* to 'user1'@'localhost';

-- 给 user2 用户分配所有权限，对所有数据库的所有表
grant all on *.* to 'user2'@'%';
~~~

## 撤销授权

当我们需要撤销一个用户的权限时，我们可以采用下面的语法语句，撤销一个用户的权限。

### 语法

~~~sql
REVOKE 权限1, 权限2 ... ON 数据库.表名 FROM '用户名'@'主机名';
 -- 要与创建用户时的 '用户名'@'主机名'，保持一致
~~~

### 关键字说明

+ `权限`

  用户的权限，如`CREATE`、`ALTER`、`SELECT`、`INSERT`、`UPDATE`等权限。如果要撤销所有权限，则可以使用`ALL`

+ `数据库名`.`表名`

  对哪些数据库的哪些表的权限进行撤销，如果要撤销该用户对所有数据库和表的操作权限，则可以使用`*.*`

### 代码演示

~~~sql
-- 撤销user1用户对mydatabase数据所有表的操作权限
revoke all on mydatabase.* from 'user1'@'localhost';
~~~

## 修改管理员密码

### 语法

+ 记得root用户密码

  ~~~bash
  mysqladmin -uroot -p password 新密码;
  ~~~

> 需要在未登陆 MySQL 的情况下操作，新密码没有特殊符号，就不需要加上引号。

### 代码演示

~~~bash
# 将 root 管理员的新密码改成 123456
mysqladmin -uroot -p password 123456;
~~~

## 修改普通用户密码

### 语法

~~~sql
SET PASSWORD FOR '用户名'@'主机名'=password('新密码');
或
UPDATE USER SET PASSWORD = PASSWORD('新密码') WHERE USER = '用户名';
~~~

> 需要在登陆 MySQL 的情况下操作，新密码要加单引号。

### 代码演示

~~~sql
-- 将'user1'@'localhost'的密码改成'666666'
set password for 'user1'@'localhost'=password('666666');
~~~

