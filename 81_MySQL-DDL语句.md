---
title: MySQL-DDL语句
date: 2020-11-17 22:06:16
tags:
	- 数据库
	- MySQL
categories:
	- MySQL
	- 复习
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201117221033.jpg
---

# MySQL-DDL语句



## DDL操作数据库

### 创建数据库

1. 创建数据库

   ~~~sql
   CREATE DATABASE 数据库名;
   ~~~

2. 判断数据是否存在，不存在则创建数据库

   ~~~sql
   CREATE DATABASE IF NOT EXISTS 数据库名;
   ~~~

3. 创建数据库并指定其默认字符集

   ~~~sql
   CREATE DATABASE 数据库名 CHARACTER SET 字符集名;
   ~~~

**代码演示**

~~~sql
-- 直接创建一个名为dataOne的数据库
create database dataone;

-- 判断数据库datatwo是否存在，如果不存在，则创建一个名为dataTwo的数据库
create database if not exists datatwo;

-- 创建一个名为dataThree、默认字符集为gbk的数据库
create database datathree default character set gbk;
~~~

### 查看数据库

1. 查看总共有哪些数据库

   ~~~sql
   SHOW DATABASES;
   ~~~

2. 查看指定数据库的定义信息

   ~~~sql
   SHOW CREATE DATABASE 数据库名;
   ~~~

**代码演示**

~~~sql
-- 查看所有的数据库
show databases;

-- 查看名字为mysql的数据库的定义信息
show create database mysql;
~~~

### 修改数据库

修改数据库的默认字符集

~~~sql
ALTER DATABASE 数据库名 DEFAULT CHARACTER SET 字符集名;
~~~

**代码演示**

~~~sql
-- 修改名字为liboshuai的数据库的默认字符集为utf8
alter database liboshuai default character set utf8;
~~~

### 删除数据库

删除指定的数据库。

~~~sql
DROP DATABASE 数据库名;
~~~

**代码演示**

~~~sql
-- 删除名字为data的数据库
drop database data;
~~~

### 使用数据库

1. 查看正在使用的数据库

   ~~~sql
   SELECT DATABASE();
   ~~~

2. 切换到、并使用指定的数据库

   ~~~sql
   USE 数据库名;
   ~~~

**代码演示**

~~~sql
-- 查看正在使用的数据库
select database();

-- 切换到、并使用名为mysql的数据库
use mysql;
~~~

**面试题**

+ 题目：在MySQL数据库软件中，有如下三个数据库：`test1`、`test2`、`test3`。登录数据库之后，输入语句：`select database test2`，运行结果是什么？
+ 答案：这是一条错误的语句，如果要选中名为`test2`的数据库，应用使用语句：`USE test2`。



## DDL操作表结构

### 创建表

创建一个指定名字、并包含指定字段的表

~~~sql
CREATE TABLE 表名 (
字段名1 字段类型1,
字段名2 字段类型2
);
~~~

**代码演示**

~~~sql
-- 创建一个名为student、包含id，name，birthday字段的表
create table student (
	id int, -- 字段名：id，字段类型：整数
    name varchar(20), -- 字段名：name，字段类型：可变字符串，容量为20
    birthday date -- 字段名：birthday，字段类型：日期
);
~~~

### 查看表

1. 查看正在使用的数据库中所有的表

   ~~~sql
   SHOW TABLES;
   ~~~

2. 查看指定表的结构

   ~~~sql
   DESC 表名;
   ~~~

3. 查看指定表的创建SQL语句（也可以当做是详细信息）

   ~~~sql
   SHOW CREATE TABLE 表名;
   ~~~

**代码演示**

~~~sql
-- 查看正在使用的数据库中所有的表
show tables;

-- 查看名为dept表的结构
desc dept;

-- 查看创建student表的SQL语句
show create table student;
~~~

### 复制表

快速创建一个表结构相同的表

~~~sql
CREATE TABLE 新表名 LIKE 旧表名;
~~~

**代码演示**

~~~sql
-- 快速创建一个与student表结构相同的human表
create table human like student;
~~~

### 删除表

1. 直接删除指定名字的表。

   ~~~sql
   DROP TABLE 表名;
   ~~~

2. 判断此表存在与否，如果存在则删除此表。

   ~~~sql
   DROP TABLE IF EXISTS 表名;
   ~~~

**代码演示**

~~~sql
-- 直接删除表human
drop table human;

-- 如果表student存在，则删除表student
drop table if exists student;
~~~

### 修改表结构

1. 添加新的字段行

   ~~~sql
   ALTER TABLE 表名 ADD 字段名 字段类型;
   ~~~

2. 删除字段行

   ~~~sql
   ALTER TABLE 表名 DROP 字段名;
   ~~~

3. 同时修改字段名、类型

   ~~~sql
   ALTER TABLE 表名 CHANGE 旧字段名 新字段名 新字段类型;
   ~~~

4. 仅修改字段类型

   ~~~sql
   ALTER TABLE 表名 MODIFY 字段名 新类型;
   ~~~

5. 修改表名

   ~~~sql
   RENAME TABLE 旧表名 TO 新表名;
   ~~~

6. 修改表的字符集

   ~~~sql
   ALTER TABLE 表名 CHARACTER SET 字符集;
   ~~~

**代码演示**

~~~sql
-- 添加一行新的名为age,类型为int(3)的字段
alter table student add age int(3);

-- 删除字段名为birthday的这一行
alter table student drop birthday;

-- 修改名为age的字段，新字段名为remark，类型为varchar(5)
alter table student change age remark varchar(5);

-- 修改名为remark的字段，新类型为int(3)
alter table student modify remark int(3);

-- 修改名为student的表，新表名为person
rename table student to person;

-- 修改person表的字符集为gbk
alter table person character set gbk;
~~~

