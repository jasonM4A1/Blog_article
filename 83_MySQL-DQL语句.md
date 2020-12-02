---
title: MySQL-DQL语句
date: 2020-11-18 23:57:51
tags:
	- MySQL
	- 数据库
categories:
	- MySQL
	- 复习
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201119000016.jpg
---

# MySQL-DQL语句

> 查询不会对数据库中的数据进行修改.只是一种显示数据的方式

## 简单查询

1. 查询表所有数据

   ~~~sql
   SELECT * FROM 表名;
   ~~~

2. 查询指定列的数据

   ~~~sql
   SELECT 字段名1, 字段名2, 字段名3 ... FROM 表名;
   ~~~

## 别名查询

1. 对列指定别名，然后进行查询

   ~~~sql
   SELECT 字段名1 AS 别名, 字段名2 AS 别名, 字段名3 AS 别名 ... FROM 表名;
   ~~~

2. 对列和表同时指定别名，然后进行查询

   ~~~sql
   SELECT 字段名1 AS 别名, 字段名2 AS 别名, 字段名3 AS 别名 ... FROM 表名 AS 表别名;
   ~~~

**代码演示**

~~~sql
-- 对列指定别名，然后进行查询
select name as '名字', age as '年龄' from students;

-- 对列和表同时指定别名，然后进行查询
select st.name as '姓名', st.age as '年龄' from students as st;
~~~

## 清除重复值

使用关键字`DISTINCT`，去除查询结果中的重复数据

~~~sql
SELECT DISTINCT 字段名 FROM 表名;
~~~

**代码演示**

~~~sql
-- 查询指定列的所有记录，包括重复结果
select age from human;

-- 查询指定列的所有记录，去除重复结果
select distinct age from human;
~~~

## 查询结果参与运算

1. 某列数据和固定值运算

   ~~~sql
   SELECT 列名1 + 固定值 FROM 表名;
   ~~~

2. 某列数据和其他列数据参与运算

   ~~~sql
   SELECT 列名1 + 列名2 FROM 表名;
   ~~~

>  注意: 参与运算的必须是数值类型

**代码演示**

~~~sql
-- 将列age+5，然后再查询出来
select age + 5 from human;

-- 将id列与age列相加，再查询出来
select id + age from human;
~~~

**数据准备**

创建一个表格，并导入数据，用于一下代码演示使用。

~~~sql
-- 创建一个表格
create table students (
	id int, -- 编号
    name varchar(20), -- 姓名
    age int, -- 年龄
    sex varchar(5), -- 性别
    address varchar(100), -- 地址
    math int, -- 数学
    english int -- 英语
);

-- 导入数据
INSERT INTO 
	students(id,NAME,age,sex,address,math,english) 
VALUES 
	(1,'马云',55,'男','杭州',66,78),
	(2,'马化腾',45,'女','深圳',98,87),
	(3,'马景涛',55,'男','香港',56,77),
	(4,'柳岩',20,'女','湖南',76,65),
	(5,'柳青',20,'男','湖南',86,NULL),
	(6,'刘德华',57,'男','香港',99,99),
	(7,'马德',22,'女','香港',99,99),
	(8,'德玛西亚',18,'男','南京',56,65);
~~~

## 条件查询

指定查询的条件，对查询记录进行过滤

~~~sql
SELECT 字段名 FROM 表名 WHERE 条件;
~~~

**代码演示**

+ 运算符

~~~sql
-- 查询 math 分数大于 80 分的学生
select * from students where math > 80;

-- 查询 english 分数小于或等于 80 分的学生
select * from students where english <= 80;

-- 查询 age 等于 20 岁的学生
select * from students where age = 20;

-- 查询 age 不等于 20 岁的学生，注：不等于有两种写法
select * from students where age <> 20;
select * from students where age != 20;
~~~

+ 逻辑运算符

~~~sql
-- 查询 age 大于 35 且性别为男的学生(两个条件同时满足)
select * from students where age > 35 and sex = '男';

-- 查询 age 大于 35 或性别为男的学生(两个条件其中一个满足)
select * from students where age > 35 or sex = '男';

-- 查询 id 是 1 或 3 或 5 的学生
select * from students where id = 1 or id = 3 or id = 5;
~~~

+ in 关键字

~~~sql
-- 查询 id 是 1 或 3 或 5 的学生(采用in关键字)
select * from students where id in(1,3,5);

-- 查询 id 不是 1 或 3 或 5 的学生(采用in关键字)
select * from students where id not in(1,3,5);
~~~

+ 范围查询(between...and...)

~~~sql
-- 查询 english 成绩大于等于 75，且小于等于 90 的学生
select * from students where english between 75 and 90;
~~~

+ like 关键字

~~~sql
-- 查询姓马的学生
select * from students where name like '马%';

-- 查询姓名中包含'德'字的学生
select * from students where name like '%德%';

-- 查询姓马，且姓名有两个字的学生
select * from students where name like '马__'; --因为是中文，所有得用两个_，适配一个汉字
~~~

+ is 关键字

~~~sql
-- 查询英语成绩为NULL的学生
select * from students where english is NULL;

-- 查询英语成绩不为NULL的学生
select * from students where english is not null;
~~~

## 排序查询

通过`ORODER BY`子句，可以将查询出的结果进行排序(排序只是显示方式，不会影响数据库中数据的顺序)。

+ 单列排序

  ~~~sql
  SELECT 字段名 FROM 表名 WHERE 字段=值 ORDER BY 字段名 <ASC 或 DESC>; -- ASC:升序，默认值; DESC:降序
  ~~~

+ 组合排序（如果有多个排序条件，则当前边的条件值一样时，才会判断第二条件）

  ~~~sql
  SELECT 字段名 FROM 表名 WHERE 字段=值 ORDER BY 字段名1 <ASC 或 DESC>, 字段名2 <ASC 或 DESC>;
  ~~~

**代码演示**

~~~sql
-- 查询所有数据,使用年龄降序排序
select * from students order by age desc;

-- 查询所有数据,在年龄降序排序的基础上，如果年龄相同再以数学成绩升序排序
select * from students order by age desc, math asc;
~~~

## 聚合函数

之前我们做的查询都是横向查询，它们都是根据条件一行一行的进行判断，而使用聚合函数查询是纵向查询， 它是对一列的值进行计算，然后返回一个结果值。聚合函数会忽略空值` NULL`。

+ 关键字

  | 聚合函数关键字 | 作用                     |
  | -------------- | ------------------------ |
  | max(列名)      | 求这一列的最大值         |
  | min(列名)      | 求这一列的最小值         |
  | avg(列名)      | 求这一列的平均值         |
  | count(列名)    | 统计这一列共有多少条记录 |
  | sum(列名)      | 对这一列求总和           |

+ 语法

  ~~~sql
  SELECT 聚合函数(列名) FROM 表名;
  ~~~

+ 聚合函数中遇到`NULL`的解决办法

  使用关键`IFNULL`，来用默认值取代`NULL`。

  ~~~sql
  IFNULL(列名,默认值)
  ~~~

**代码演示**

~~~sql
-- 查询学生总数
select count(id) '总人数' from students;
select count(*) as '总人数' from students;

-- 使用 id 字段来统计数据总数，如果为 null，则使用 0 代替，这样统计的数据就不会遗漏
select count(ifnull(id,0)) from students;

-- 查询年龄大于 20 的总数
select count(*) from students where age > 20;

-- 查询数学成绩总分
select sum(math) '数学总分' from students;

-- 查询数学成绩平均分
select avg(math) '数学均分' from students;

-- 查询数学成绩最高分
select max(math) '数学最高分' from students;

-- 查询数学成绩最低分
select min(math) '数学最低分' from students;
~~~

## 分组查询

分组查询是指使用`GROUP BY`语句对查询信息进行分组，相同数据作为一组。

~~~sql
SELECT 字段1, 字段2 ... FROM 表名 GROUP BY 分组字段 [HAVING 条件];
~~~

或结合【聚合函数】一起使用

~~~sql
SELECT 列名, 聚合函数(列名) FROM 表名 GROUP BY 分组字段 [HAVING 条件];
~~~

> 注意：
>
> 使用分组函数进行查询时，查询的只能是分组字段或聚合函数，查询其他则没有意义。

**代码演示**

~~~sql
-- 按性别进行分组，求男生和女生数学的平均分
select sex, avg(math) from students group by sex;

-- 统计男女各有多少人
select sex, count(id) from students group by sex;
或
select sex, count(*) from students group by sex;

-- 查询年龄大于 25 岁的人,按性别分组,统计每组的人数
select sex, count(*) from students where age > 25 group by sex;

-- 查询年龄大于 25 岁的人，按性别分组，统计每组的人数，并只显示性别人数大于 2 的数据
select sex, count(*) as '人数' from students where age > 25 group by sex having count(*) > 2;
~~~

## having 与 where 的区别

| 子句       | 作用                                                         |
| ---------- | ------------------------------------------------------------ |
| where子句  | 1. 对查询结果进行分组前，将不符合 where 条件的行去掉，即在分组之前过滤数据，即先过滤 再分组。<br /> 2. where 后的条件不可以使用聚合函数<br />3. where子句必须出现在group by前面 |
| having子句 | 1. having 子句的作用是筛选满足条件的组，即在分组之后过滤数据，即先分组再过滤。<br />2. having 后的条件可以使用聚合函数<br />3. having子句只能出现在group by后面的 |

**面试题**

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201119054405.png)

执行如下 SQL 语句`select product,sum(price) from orders group by product where sum(price) > 30;`，运行结果是？

**答案**

运行有误，group by 后面不能出现 where，应使用 having

**准备数据**

往之前的students表中继续加入数据，以便下面学习使用。

~~~sql
insert into students (id, name, age, sex, address, math, english) values 
(9,'唐僧',25,'男','长安',87,78), (10,'孙悟空',18,'男','花果山',100,66), 
(11,'猪八戒',22,'男','高老庄',58,78), (12,'沙僧',50,'男','流沙河',77,88), 
(13,'白骨精',22,'女','白虎岭',66,66), (14,'蜘蛛精',23,'女','盘丝洞',88,88);
~~~

## 截取查询

`LIMIT`是限制的意思，所以`LIMIT`的作用就是限制查询记录的条数。

~~~sql
SELECT 列名 [as 别名] FROM 表名 [WHERE子句] [group by子句] [HAVING子句] [ORDER BY子句] LIMIT offset, length; 
-- offset：起始行数，从 0 开始计数，如果省略，默认就是 0;  length：返回的行数
~~~

> 类似百度页面的分页公式：`开始的索引`

**代码演示**

~~~sql
-- 查询学生表中数据，从第 3 条开始显示，显示 6 条。
select * from students limit 2, 6;

-- 查询学生表中前5条数据
select * from students limit 5;
~~~

## 查询顺序

~~~sql
SELECT 
	字段列表
FROM 
	表名列表
WHERE
	条件列表
GROUP BY
	分组字段
HAVING
	分组后条件
ORDER BY
	排序
LIMIT
	分页限定
~~~



## 数据库备份和还原

###  备份的应用场景

在服务器进行数据传输、数据存储和数据交换，就有可能产生数据故障。比如发生意外停机或存储介质损坏。 这时，如果没有采取数据备份和数据恢复手段与措施，就会导致数据的丢失，造成的损失是无法弥补与估量的。

### 备份与还原的语句

+ 备份格式：（DOS 下，未登录的时候）

  ~~~bash
  mysqldump -u用户名 -p密码 数据库名 > 文件路径
  ~~~

+  还原格式：（mysql 中的命令，需要登录后才可以操作）

  ~~~sql
  USE 数据库;
  SOURCE 导入文件的路径;
  ~~~

**代码演示**

~~~sql
-- 备份数据库liboshuai中的数据到 d:/one.sql 文件中
mysqldump -uroot -p密码 liboshuai > d:/one.sql
-- 导出结果：数据库中的所有表和数据都会导出成 SQL 语句

-- 还原 liboshuai 数据库中的数据 (还原的时候需要先登录 MySQL,并选中对应的数据库)
source d:/one.sql;
~~~

