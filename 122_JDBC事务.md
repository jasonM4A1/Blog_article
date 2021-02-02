---
title: JDBC事务
date: 2021-02-02 05:39:14
tags:
	- JDBC
	- JavaWeb
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202053919.jpg
---

# 概述

我们在使用JDBC操作数据库的时候，依然也得使用事务管理，这样可以保证数据的安全。

# 操作

使用`Connection`对象调用一下方法来管理事务

1. ```
   setAutoCommit(boolean autoCommit)
   ```

   - 作用：开启事务
   - 参数：设置为`false`，即为开启事务
   - 时机：在执行SQL语句之前开启事务

2. ```
   commit()
   ```

   - 作用：提交事务
   - 时机：当所有SQL语句都执行完，再提交事务

3. ```
   rollback()
   ```

   - 作用：回滚事务
   - 时机：在处理异常的`catch`中回滚事务

# 不使用事务，数据异常演示

- 数据库`mydatabase.account`

  ```
  +----+------+---------+
  | id | name | balance |
  +----+------+---------+
  |  1 | 张三 | 1000.00 |
  |  2 | 李四 | 1000.00 |
  +----+------+---------+
  2 rows in set (0.00 sec)
  ```

- 配置文件

  ```
  driver=com.mysql.jdbc.Driver
  url=jdbc:mysql://localhost:3306/mydatabase
  user=root
  password=intmain()
  ```

- 代码实现

  我们在执行第一条sql语句和执行第二条sql语句之间，手动制造了一个异常。这样会导致，只有第一天sql语句可以执行成功，而第二条sql语句会因为异常的出现而无法被执行。这时我们观察代码执行后，数据库表格数据的变化。

  ```
  package top.liboshuai.study;
  
  import java.util.ResourceBundle;
  import java.sql.Connection;
  import java.sql.PreparedStatement;
  import java.sql.DriverManager;
  import java.sql.SQLException;
  
  public class Demo {
      public static void main(String[] args) {
          //使用资源绑定器绑定配置文件
          ResourceBundle resource = ResourceBundle.getBundle("jdbc");
          //获取配置文件中的数据
          String driver = resource.getString("driver");
          String url = resource.getString("url");
          String user = resource.getString("user");
          String password = resource.getString("password");
  
          //数据库连接对象
          Connection conn = null;
          //SQL语句的预编译执行对象
          PreparedStatement ps01 = null;
          PreparedStatement ps02 = null;
          //执行结果
          int count = 0;
  
          try {
              //注册数据库驱动
              Class.forName(driver);
              //获取数据库连接
              conn = DriverManager.getConnection(url, user, password);
              //定义SQL语句
              String sql01 = "UPDATE account SET balance = balance - ? WHERE id = ?;";
              String sql02 = "UPDATE account SET balance = balance + ? WHERE id = ?;";
              //获取SQL语句的预编译执行对象
              ps01 = conn.prepareStatement(sql01);
              ps02 = conn.prepareStatement(sql02);
              //给占位符赋值
              ps01.setDouble(1, 455.55);
              ps01.setInt(2, 1);
              ps02.setDouble(1, 455.55);
              ps02.setInt(2, 2);
              //执行SQL语句
              count += ps01.executeUpdate();
              //手动制造异常
              count /= 0;
              count += ps02.executeUpdate();
              //打印执行结果
              System.out.println(count);
          } catch (ClassNotFoundException | SQLException e) {
              e.printStackTrace();
          } finally {
              // 关闭资源
              if (ps02 != null) {
                  try {
                      ps02.close();
                  } catch (SQLException throwables) {
                      throwables.printStackTrace();
                  }
              }
              if (ps01 != null) {
                  try {
                      ps01.close();
                  } catch (SQLException throwables) {
                      throwables.printStackTrace();
                  }
              }
              if (conn != null) {
                  try {
                      conn.close();
                  } catch (SQLException throwables) {
                      throwables.printStackTrace();
                  }
              }
          }
      }
  }
  ```

- 执行后，数据库结果

  我们可以看到数据库表格的结果并不是我们预期的：张三的余额为544.45，李四的余额为1455.55。而只是张三的余额减少了，李四的余额并没有增加。这样就有455.55块钱平白无故的蒸发了！这就是不使用事务管理可能会出现的严重后果！

  ```
  +----+------+---------+
  | id | name | balance |
  +----+------+---------+
  |  1 | 张三 |  544.45 |
  |  2 | 李四 | 1000.00 |
  +----+------+---------+
  2 rows in set (0.00 sec)
  ```

# 使用事务，解决数据异常

- 数据库数据

  ```
  +----+------+---------+
  | id | name | balance |
  +----+------+---------+
  |  1 | 张三 | 1000.00 |
  |  2 | 李四 | 1000.00 |
  +----+------+---------+
  2 rows in set (0.00 sec)
  ```

- 配置文件

  ```
  driver=com.mysql.jdbc.Driver
  url=jdbc:mysql://localhost:3306/mydatabase
  user=root
  password=intmain()
  ```

- 代码实现

  为了避免异常的出现，导致数据库数使异常，我们使用了事务管理。在代码中的具体体现为：我们增加了三步——【开启事务】、【提交事务】、【回滚事务】。

  ```
  package top.liboshuai.study;
  
  import java.util.ResourceBundle;
  import java.sql.Connection;
  import java.sql.PreparedStatement;
  import java.sql.DriverManager;
  import java.sql.SQLException;
  
  public class Demo {
      public static void main(String[] args) {
          //使用资源绑定器绑定配置文件
          ResourceBundle resource = ResourceBundle.getBundle("jdbc");
          //获取配置文件中的数据
          String driver = resource.getString("driver");
          String url = resource.getString("url");
          String user = resource.getString("user");
          String password = resource.getString("password");
  
          //数据库连接对象
          Connection conn = null;
          //SQL语句的预编译执行对象
          PreparedStatement ps01 = null;
          PreparedStatement ps02 = null;
          //执行结果
          int count = 0;
  
          try {
              //注册数据库驱动
              Class.forName(driver);
              //获取数据库连接
              conn = DriverManager.getConnection(url, user, password);
              //开启事务【新增】
              conn.setAutoCommit(false);
              //定义SQL语句
              String sql01 = "UPDATE account SET balance = balance - ? WHERE id = ?;";
              String sql02 = "UPDATE account SET balance = balance + ? WHERE id = ?;";
              //获取SQL语句的预编译执行对象
              ps01 = conn.prepareStatement(sql01);
              ps02 = conn.prepareStatement(sql02);
              //给占位符赋值
              ps01.setDouble(1, 455.55);
              ps01.setInt(2, 1);
              ps02.setDouble(1, 455.55);
              ps02.setInt(2, 2);
              //执行SQL语句
              count += ps01.executeUpdate();
              //手动制造异常
              count /= 0;
              count += ps02.executeUpdate();
              //提交事务【新增】
              conn.commit();
              //打印执行结果
              System.out.println(count);
          } catch (Exception e) { //因为可能出现任何异常，所以要使用Exception进行捕捉
              //如果SQL执行中出现异常，就进行事务回滚【新增】
              if (conn != null) {
                  try {
                      conn.rollback();
                  } catch (SQLException throwables) {
                      throwables.printStackTrace();
                  }
              }
              e.printStackTrace();
          } finally {
              // 关闭资源
              if (ps02 != null) {
                  try {
                      ps02.close();
                  } catch (SQLException throwables) {
                      throwables.printStackTrace();
                  }
              }
              if (ps01 != null) {
                  try {
                      ps01.close();
                  } catch (SQLException throwables) {
                      throwables.printStackTrace();
                  }
              }
              if (conn != null) {
                  try {
                      conn.close();
                  } catch (SQLException throwables) {
                      throwables.printStackTrace();
                  }
              }
          }
      }
  }
  ```

- 执行后，数据库数据

  这一次我们可以看到，在使用了事务管理后，哪怕是执行SQL语句过程中出现了异常，数据也没有出现异常。

  ```
  +----+------+---------+
  | id | name | balance |
  +----+------+---------+
  |  1 | 张三 | 1000.00 |
  |  2 | 李四 | 1000.00 |
  +----+------+---------+
  2 rows in set (0.00 sec)
  ```