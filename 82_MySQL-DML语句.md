---
title: MySQL-DML语句
date: 2020-11-18 19:22:41
tags:
	- MySQL
	- 数据库
categories:
	- MySQL
	- 复习
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201117194600.jpg
---

# MySQL-DML语句

## DML操作表中的数据

### 插入字段

1. 方式一：(这种键值对的方式，适合给所有字段赋值，也适合给部分字段赋值)

   ~~~sql
   INSERT INTO 表名 (字段名1, 字段名2, 字段名3...) VALUES (值1, 值2, 值3...);
   ~~~

2. 方式二：（这种匿名直接赋值的方式，只能完全给所有的字段进行赋值）

   ~~~sql
   INSERT INTO 表名 VALUES (值1, 值2, 值3...);
   ~~~

> 注意事项：
>
> 1. 插入的数据应与字段的数据类型相同
> 2. 数据的大小应在列的规定范围内，例如：不能将一个长度为 80 的字符串加入到长度为 40 的列中。
> 3. 在 values 中列出的数据位置必须与被加入的列的排列位置相对应。在 mysql 中可以使用 value，但不建议使 用，功能与 values 相同。
> 4. 字符和日期型数据应包含在单引号中。MySQL 中也可以使用双引号做为分隔符。
> 5. 不指定列或使用 null，表示插入空值。

**代码演示**

~~~sql
-- 给所有的字段都赋值
insert into students (id, name, age, gender) values (1, 'Jason', 21, '男');

-- 给所有的字段匿名直接赋值
insert into students values (2, 'Charlie', 22, '男');

-- 给部分字段赋值
insert into students (id, gender) values (3, '女');

~~~

### 蠕虫复制

**概述**

将一张已经存在的表中的数据复制到另一张表中，即为蠕虫复制。

**格式**

+ 将【表名1】中的所有的列复制到【表名2】中

  ~~~sql
  INSERT INTO 表名2 SELECT * FROM 表名1;
  ~~~

+ 将【表名1】中的部分的列复制到【表名2】中

  ~~~sql
  INSERT INTO 表名2 (字段名1, 字段名2) SELECT 字段名1, 字段名2 FROM 表名1;
  ~~~

**代码演示**

~~~sql
-- 将students表中所有的数据都添加到human表中（human表结构与students表结构相同）
insert into human select * from students;
~~~

### 更新表记录

1. 直接修改同一字段名下的所有值

   ~~~sql
   UPDATE 表名 SET 字段名 = 字段值;
   ~~~

2. 满足条件时，修改字段值

   ~~~sql
   UPDATE 表名 SET 字段名 = 字段值 where 字段名 = 字段值;
   ~~~

**代码演示**

~~~sql
-- 将students表字段名为age的值都更新为30
update students set age = 30;

-- 将students表字段名为name，且满足id=2的值更新为June
UPDATE students set name = 'June' where id = 2;
~~~

### 删除表记录

1. 直接删除表中的所有记录

   ~~~sql
   DELETE FROM 表名;
   ~~~

2. 删除表中满足条件的记录

   ~~~sql
   DELETE FROME 表名 WHERE 字段名 = 字段值;
   ~~~

3. 使用truncate删除表中所有的记录（当需要删除所有的数据时，推荐使用）

   ~~~sql
   TRUNCATE TABLE 表名;
   ~~~

   > 注：truncate 相当于删除表的结构，再创建一张表。

**代码演示**

~~~sql
-- 直接删除表students中的所有记录
delete from students;

-- 删除表中id=3的记录
delete from students where id = 3;

-- 使用truncate删除表中所有的记录
truncate table students;
~~~





