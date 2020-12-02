---
title: JDBC入门
date: 2020-11-27 20:42:51
tags:
	- MySQL
	- 数据库
categories:
	- MySQL
	- 复习
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201127204636.jpg
---

# JDBC概述

JDBC 是 Java 访问数据库的标准规范，真正怎么操作数据库还需要具体的实现类，也就是数据库驱动。每个数据库厂商根据自家数据库的通信格式编写好自己数据库的驱动。所以我们只需要会调用 JDBC 接口中的方法即 可，数据库驱动由数据库厂商提供。

程序员如果要开发访问数据库的程序，只需要会调用 JDBC 接口中的方法即可，不用关注类是如何实现的。 

 使用同一套 Java 代码，进行少量的修改就可以访问其他 JDBC 支持的数据库。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201127205158.png)

# JDBC相关开发包

+ `java.sql`：包含所有与JDBC访问数据库相关的接口和类。
+ `javax.sql`：数据库扩展包，提供数据库额外的功能。如：连接池
+ 数据库的驱动：由各大数据库厂商提供，需要额外去下载，是对 JDBC 接口实现的类

# JDBC核心API

+ `DriverManager`类：管理和注册数据库驱动，得到数据库连接对象。
+ `Connection`接口：一个连接对象，可用于创建`Statement`和`PreparedStatement`对象
+ `Statement`接口：一个SQL语句对象，用于将SQL语句发送给数据库服务器。
+ `PreparedStatement`接口：一个SQL语句对象，是Statement的子接口
+ `ResultSet`接口：用于封装数据库查询的结果集，返回给客户端Java程序。

# JDBC使用步骤

1. Java 连接 MySQL 需要驱动包，官网下载地址为[MySQL驱动包官网下载](https://dev.mysql.com/downloads/connector/j/)
   还是建议大家下载以前的版本

2. 导入驱动Jar包到Java项目中

   [ JDBC驱动jar包的下载&导入IDEA](https://blog.csdn.net/weixin_47330507/article/details/106192548)

3. 加载和注册驱动

   使用`Class.forName`(数据库驱动实现类)来加载和注册数据库驱动。

```java
package top.liboshuai.study;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class Human {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        //注册数据库驱动
        Class.forName("com.mysql.jdbc.Driver");
    }
}
```

> 从 JDBC3 开始，目前已经普遍使用的版本。可以不用注册驱动而直接使用。Class.forName 这句话可以省略。

# JDBC初级代码模板

+ 在`mydatabase`数据库中创建一个`students`表格（执行DDL语句）

  ~~~java
  package top.liboshuai.study;
  
  import java.sql.Connection;
  import java.sql.DriverManager;
  import java.sql.Statement;
  import java.sql.SQLException;
  
  @SuppressWarnings("all")
  public class Demo {
      public static void main(String[] args) {
          Connection conn = null;
          Statement state = null;
          try {
              //注册数据库驱动
              Class.forName("com.mysql.jdbc.Driver");
              //获取数据库连接对象
              conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydatabase", "root", "helloWorld");
              //定义SQL语句
              String sql = "CREATE TABLE students (id INT PRIMARY KEY AUTO_INCREMENT,NAME VARCHAR(20) NOT NULL,gender VARCHAR(2) NOT NULL,age INT NOT NULL,join_date DATE NOT NULL);";
              //获取执行SQL语句的对象
              state = conn.createStatement();
              //执行SQL语句
              int count = state.executeUpdate(sql);
              //处理结果
              System.out.println(count);//0
          } catch (ClassNotFoundException | SQLException e) {
              e.printStackTrace();
          } finally {
              //关闭资源
              if (state != null) {
                  try {
                      state.close();
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
  ~~~

  

+ 向`students`表格中添加三条记录（执行DML语句）

  ~~~java
  package top.liboshuai.study;
  
  import java.sql.DriverManager;
  import java.sql.Connection;
  import java.sql.Statement;
  import java.sql.SQLException;
  
  public class Demo {
      public static void main(String[] args) {
          Connection conn = null;
          Statement state = null;
          try {
              //注册数据库驱动
              Class.forName("com.mysql.jdbc.Driver");
              //获取数据库连接
              conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydatabase", "root", "helloWorld");
              //定义SQL语句
              String sql01 = "INSERT INTO students (NAME, gender, age, join_date) VALUES ('Jason', '男', 21, '1999-08-22');";
              String sql02 = "INSERT INTO students (NAME, gender, age, join_date) VALUES ('Charlie', '女', 20, '1999-07-12');";
              String sql03 = "INSERT INTO students (NAME, gender, age, join_date) VALUES ('June', '男', 23, '2020-01-01');";
              //获取执行SQL语句的对象
              state = conn.createStatement();
              //执行SQL语句
              int count = state.executeUpdate(sql01);
              count += state.executeUpdate(sql02);
              count += state.executeUpdate(sql03);
              //处理结果
              System.out.println(count);//3
          } catch (ClassNotFoundException | SQLException e) {
              e.printStackTrace();
          } finally {
              //关闭资源
              if (state != null) {
                  try {
                      state.close();
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
  ~~~

  

+ 查询`students`表格中的所有记录（执行DQL语句）

  ~~~java
  package top.liboshuai.study;
  
  import java.sql.*;
  
  public class Demo {
      public static void main(String[] args) {
          Connection conn = null;
          Statement state = null;
          ResultSet result = null;
          try {
              //注册数据库驱动
              Class.forName("com.mysql.jdbc.Driver");
              //获取数据库连接对象
              conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydatabase", "root", "helloWorld");
              //定义SQL语句
              String sql = "select * from students";
              //获取执行SQL语句的对象
              state = conn.createStatement();
              //执行SQL语句
              result = state.executeQuery(sql);
              //处理结果
              while (result.next()) {
                  int id = result.getInt("id");
                  String name = result.getString(2);
                  String gender = result.getString("gender");
                  int age = result.getInt(4);
  
                  System.out.println(id + ", " + name + "," + gender + "," + age);
              }
          } catch (ClassNotFoundException | SQLException e) {
              e.printStackTrace();
          } finally {
              //关闭资源
              if (result != null) {
                  try {
                      result.close();
                  } catch (SQLException throwables) {
                      throwables.printStackTrace();
                  }
              }
              if (state != null) {
                  try {
                      state.close();
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
  ~~~

  

## 步骤详解

### 一：注册数据库驱动

#### 功能

告诉程序该使用哪一个数据库驱动

#### 方法

+ `Class.forName("com.mysql.jdbc.Driver")`

  通过查看源代码发现，使用此方法实际上是调用`com.mysql.jdbc.Driver`类中的静态代码块

  ~~~java
  static {
      try {
          DriverManager.registerDriver(new Driver());
      } catch (SQLException E) {
  		throw new RuntimeException("Can't register driver!");
      }
  }
  ~~~

  通过`DriverManager`类调用静态方法`registerDriver()`来进行数据库驱动注册的。

  在`mysql 5`版本之后的驱动jar都是可以直接省略注册驱动的。

### 二：获取数据库连接对象

#### 功能

通过传入URL、用户名、密码，来获取数据库的连接。

#### 方法

`java.sql.DriverManager`类中的`static Connection getConnection(String url, String user, String password)`

+ `user`：用户名

+ `password`：密码

+ `URL`

  格式：`jdbc:mysql://ip:端口/数据库名称`

  实例：`jdbc:mysql://localhost:3306/mydatabase`

  备注：如果连接的是本机`mysql`服务器，并且默认端口为3306，则URL可以简写为`jdbc:mysql:///mydatabase`

### 三：定义sql语句

定义一个字符串引用，用于接收字符串形式的SQL语句。

#### 占位符？

使用占位符：`select * from user where username = ? and password = ?`

给占位符赋值：setXxx(参数1, 参数2)

### 四：获取执行sql语句对象

#### 功能

获取用于执行SQL语句的对象。

#### 方法

1. `java.sql.Connection`接口中的`Statement createStatement()`

   创建一个Statement对象，用于向数据库发送SQL语句。

2. `java.sql.Connection`接口中的`PreparedStatement prepareStatement(String sql)`

   创建用于向数据库发送参数化SQL语句的PreparedStatement对象。

### 五：执行sql语句

#### 功能

执行传入的String类型的SQL语句。

#### 方法

1. `java.sql.Statement()`接口中的`int executeUpdate(String sql)`

   + 作用

     执行insert、delete、update和DDL语句。

   + 返回值

     影响的行数，可以通过这个影响的行数判断DML语句是否执行成功 返回值>0的则执行成功，反之，则失败。

2. `java.sql.Statement()`接口的`ResultSet executeQuery(String sql)`

   + 作用

     执行给定的DQL语句，该语句返回单个ResultSet对象。

   + 返回值

     查询得到的结果集

3. `java.sql.PreparedStatement()`接口中的`int executeUpdate()`

   + 作用

     执行PreparedStatement对象中的SQL语句，该对象必须是SQL数据操作语言(DML)语句，如insert、update或delete;或者不返回任何内容的SQL语句，比如DDL语句。

   + 返回值

     影响的行数，可以通过这个影响的行数判断DML语句是否执行成功 返回值>0的则执行成功，反之，则失败。

4. `java.sql.PreparedStatement()`接口中的`ResultSet executeQuery()`

   + 作用

     在这个PreparedStatement对象中执行SQL查询，并返回查询生成的ResultSet对象。

   + 返回值

     查询得到的结果集

### 六：处理结果集

#### 功能

+ `executeUpdate()`返回值为`int`类型

  执行DDL语句，成功则返回0。执行DML语句，成功则返回影响的行数。我们需要利用这个返回得到的int类型的数值进行一些代码操作。

+ `executeQuery`返回值为`ResultSet`类型。两种的处理方式不同。

下面我只讲述一下关于`executeQuery()`返回的`ResultSet`类型的结果的处理。

#### 方法

1. `java.sql.Result`接口的`boolean next()`

   将光标从当前位置向前移动一行，当光标移动到最后一行之后则返回true，其余返回false。

2. `java.sql.Result`接口的`xxx getXxx(int columnIndex)`

   通过传入索引获取表中的字段值。如：getString()不管数据库中的数据类型是什么，都已String类型取出。其他方法也类似。并且jdbc下标是从1开始，而不是0。

3. `java.sql.Result`接口的`xxx getXxx(String columnLabel)`（推荐这一种）

   通过传入列名获取表中的值。注意这里的列名，是指我们查询之后得到的列名，这个列名可能会被起别名。

#### 常用数据类型转换表

| SQL类型         | JDBC对应方法      | 返回类型                           |
| --------------- | ----------------- | ---------------------------------- |
| BIT(1) bit(n)   | getBoolean()      | boolean                            |
| TINYINT         | getByte()         | byte                               |
| SMALLINT        | getShort()        | short                              |
| INT             | getInt()          | int                                |
| BIGINT          | getLong()         | long                               |
| CHAR, VARCHAR   | getString()       | String                             |
| Text(Clob) Blob | getClob getBlob() | Clob Blob                          |
| DATE            | getDate()         | java.sql.Date只代表日期            |
| TIME            | getTime()         | java.sql.Time只表示时间            |
| TIMESTAMP       | getTimestamp()    | java.sql.Timestamp同时有日期和时间 |

### 七：关闭资源

首先判断需要关闭的资源引用是否为`null`，如果不为`null`，则调用`close()`方法关闭资源。

# JDBC结合属性配置文件

+ 配置文件

  ~~~properties
  driver=com.mysql.jdbc.Driver
  url=jdbc:mysql://localhost:3306/mydatabase
  user=root
  password=helloWorld
  sql=CREATE TABLE person (id INT PRIMARY KEY AUTO_INCREMENT,NAME VARCHAR(20),age INT,gender VARCHAR(2));
  ~~~

+ 实例代码

  ~~~java
  package top.liboshuai.study;
  
  import java.sql.Connection;
  import java.sql.DriverManager;
  import java.sql.Statement;
  import java.sql.SQLException;
  import java.util.ResourceBundle;
  
  @SuppressWarnings("all")
  public class Demo {
      public static void main(String[] args) {
          //使用资源绑定器绑定属性配置文件
          ResourceBundle bundle = ResourceBundle.getBundle("jdbc");
          String driver = bundle.getString("driver");
          String url = bundle.getString("url");
          String user = bundle.getString("user");
          String password = bundle.getString("password");
          String sql = bundle.getString("sql");
  
          Connection conn = null;
          Statement state = null;
          try {
              //注册数据库驱动
              Class.forName(driver);
              //获取数据库连接
              conn = DriverManager.getConnection(url,user,password);
              //sql语句已经在配置文件中定义好了
              //获取执行SQL语句的对象
              state = conn.createStatement();
              //执行SQL语句
              int count = state.executeUpdate(sql);
              //处理结果
              System.out.println(count);//0
          } catch (ClassNotFoundException | SQLException e) {
              e.printStackTrace();
          } finally {
              //关闭资源
              if (state != null) {
                  try {
                      state.close();
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
  ~~~

  

# JDBC案例：用户登录

+ 用户账号数据库`mydatabase.account`

  ~~~sql
  +----+---------------+-----------+
  | id | user          | password  |
  +----+---------------+-----------+
  |  1 | fortune       | wswdy     |
  |  2 | dragon        | xhssf     |
  |  3 | dinosaur      | xxxxxxx   |
  |  4 | dragonPortal  | wswswswsw |
  |  5 | questionnaire | ds.o2.cgi |
  +----+---------------+-----------+
  ~~~

+ `jdbc.properties`配置文件

  ~~~properties
  driver=com.mysql.jdbc.Driver
  url=jdbc:mysql://localhost:3306/mydatabase
  user=root
  password=helloWorld
  ~~~
  
+ 代码实现

  ~~~java
  package top.liboshuai.study;
  
  import java.sql.*;
  import java.util.HashMap;
  import java.util.Map;
  import java.util.ResourceBundle;
  import java.util.Scanner;
  
  public class Demo {
      public static void main(String[] args) {
          //初始化一个界面
          Map<String, String> userLoginInfo = initUI();
          //验证用户名和密码
          boolean loginSuccess = login(userLoginInfo);
          System.out.println(loginSuccess ? "登录成功" : "登录失败");
      }
  
      /**
       * 验证用户名和密码
       * @param userLoginInfo 用户输入的用户名和密码
       * @return 是否登录成功的布尔值
       */
      private static boolean login(Map<String, String> userLoginInfo) {
          //登录成功的布尔标记
          boolean loginSuccess = false;
  
          //取出用户输入的用户名和密码
          String loginName = userLoginInfo.get("userName");
          String loginPwd = userLoginInfo.get("password");
  
          //使用资源绑定器绑定配置文件
          ResourceBundle resource = ResourceBundle.getBundle("jdbc");
          //获取配置文件中值
          String driver = resource.getString("driver");
          String url = resource.getString("url");
          String user = resource.getString("user");
          String password = resource.getString("password");
  
          Connection conn = null;
          Statement state = null;
          ResultSet result = null;
  
          try {
              //注册数据库驱动
              Class.forName(driver);
              //获取数据库连接对象
              conn = DriverManager.getConnection(url, user, password);
              //定义SQL语句
              String sql = "select * from account where user = '"+ loginName +"' and password = '"+ loginPwd +"';";
              //获取SQL语句执行对象
              state = conn.createStatement();
              //执行SQL语句
              result = state.executeQuery(sql);
              //判断是否登录成功
              if (result.next()) {
                  loginSuccess = true;
              }
          } catch (ClassNotFoundException | SQLException e) {
              e.printStackTrace();
          } finally {
              //关闭资源
              if (state != null) {
                  try {
                      state.close();
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
  
          return loginSuccess;
    }
  
      /**
       * 初始化用户界面
       * @return 用户输入的用户名和密码
       */
      private static Map<String, String> initUI() {
          Scanner scan = new Scanner(System.in);
  
          System.out.print("请输入用户名：");
          String userName = scan.nextLine();
  
          System.out.print("请输入密码：");
          String password = scan.nextLine();
  
          Map<String, String> userLoginInfo = new HashMap<>();
          userLoginInfo.put("userName",userName);
          userLoginInfo.put("password",password);
  
          return userLoginInfo;
      }
  }
  ~~~
  

# SQL注入

## 问题描述

经过测试发现，上面的程序有如下的问题：

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201201130142.png)

当密码设置为`xxx' OR '1'='1`时，无论是否此用户，输入的密码是否正确，都会登录成功。此种现象被称为【SQL注入】

## 问题原因

因为用户输入的信息中含有SQL语句的关键字，并且这些关键字参与SQL语句的编译过程，导致SQL语句的原意被扭曲，进行出现了SQL注入的问题。

## 解决问题

只要用户提供的信息不参与SQL语句e编译过程，即使信息中含有SQL语句的关键字，SQL注入问题也不会产生。为了达成上述目的，我们就需要使用到`java.sql.PreparedStatement`。下面我们先对于`java.sqlPreparedStatement`进行一个简短的说明：

+ `java.sql.PreparedStatement`接口是`java.sql.Statement`的子接口。
+ `java.sql.PreparedStatement`是被称为预编译的数据库操作对象.
+ `java.sql.PreparedStatement`的原理为：预先对SQL语句的框架进行编译，然后再给SQL语句传值。

# JDBC案例：利用SQL注入，进行数据排序

+ 数据库`mydatabase.account`

  ~~~sql
  +----+---------------+-----------+
  | id | user          | password  |
  +----+---------------+-----------+
  |  1 | fortune       | wswdy     |
  |  2 | dragon        | xhssf     |
  |  3 | dinosaur      | xxxxxxx   |
  |  4 | dragonPortal  | wswswswsw |
  |  5 | questionnaire | ds.o2.cgi |
  +----+---------------+-----------+
  ~~~

  

+ 配置文件

  ~~~properties
  driver=com.mysql.jdbc.Driver
  url=jdbc:mysql://localhost:3306/mydatabase
  user=root
  password=helloWorld
  ~~~

+ 实现代码

~~~java
package top.liboshuai.study;

import java.sql.*;
import java.util.ResourceBundle;
import java.util.Scanner;

public class Demo {
    public static void main(String[] args) {
        //初始化
        String keyWord = initUI();

        //排序
        order(keyWord);
    }

    /**
     * 根据用户输入的命令对查询的数据进行排序
     *
     * @param keyWord 用户输入的命令
     */
    private static void order(String keyWord) {
        //使用资源绑定器绑定配置文件
        ResourceBundle source = ResourceBundle.getBundle("jdbc");
        //获取配置文件中的值
        String driver = source.getString("driver");
        String url = source.getString("url");
        String user = source.getString("user");
        String password = source.getString("password");

        Connection conn = null;
        Statement state = null;
        ResultSet result = null;

        try {
            //注册数据库驱动
            Class.forName(driver);
            //获取数据库连接
            conn = DriverManager.getConnection(url, user, password);
            //定义SQL语句
            String sql = "select * from account order by id " + keyWord;
            //获取sql语句执行的对象
            state = conn.createStatement();
            //执行SQL语句
            result = state.executeQuery(sql);
            //打印查询结果集
            while (result.next()) {
                int id = result.getInt(1);
                String userName = result.getString("user");
                String userPwd = result.getString("password");

                System.out.println(id + ", " + userName + ", " + userPwd);
            }
        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        }
    }

    /**
     * 初始化用户界面,接收用户输入的命令
     *
     * @return 用户输入的命令
     */
    private static String initUI() {
        Scanner scan = new Scanner(System.in);

        System.out.println("输入命令(desc或asc)：");
        String keyWord = scan.nextLine();

        return keyWord;
    }
}
~~~

# JDBC案例:用户登录(防SQL注入PreparedStatement)

~~~java
package top.liboshuai.study;

import java.sql.*;
import java.util.HashMap;
import java.util.Map;
import java.util.ResourceBundle;
import java.util.Scanner;

public class Demo {
    public static void main(String[] args) {
        //初始化一个界面
        Map<String, String> userLoginInfo = initUI();
        //验证用户名和密码
        boolean loginSuccess = login(userLoginInfo);
        System.out.println(loginSuccess ? "登录成功" : "登录失败");
    }

    /**
     * 验证用户名和密码
     * @param userLoginInfo 用户输入的用户名和密码
     * @return 是否登录成功的布尔值
     */
    private static boolean login(Map<String, String> userLoginInfo) {
        //登录成功的布尔标记
        boolean loginSuccess = false;

        //取出用户输入的用户名和密码
        String loginName = userLoginInfo.get("userName");
        String loginPwd = userLoginInfo.get("password");

        //使用资源绑定器绑定配置文件
        ResourceBundle resource = ResourceBundle.getBundle("jdbc");
        //获取配置文件中值
        String driver = resource.getString("driver");
        String url = resource.getString("url");
        String user = resource.getString("user");
        String password = resource.getString("password");

        Connection conn = null;
        PreparedStatement pState = null;
        ResultSet result = null;

        try {
            //注册数据库驱动
            Class.forName(driver);
            //获取数据库连接对象
            conn = DriverManager.getConnection(url, user, password);
            //定义SQL语句，使用占位符
            String sql = "select * from account where user = ? and password = ?;";
            //获取SQL语句预编译的执行对象（将SQL语句提前发送给DBMS进行预编译）
            pState = conn.prepareStatement(sql);
            //给占位符赋值(JDBC的index都是从1开始)
            pState.setString(1,loginName);
            pState.setString(2,loginPwd);
            //执行SQL语句
            result = pState.executeQuery();
            //判断是否登录成功
            if (result.next()) {
                loginSuccess = true;
            }
        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        } finally {
            //关闭资源
            if (pState != null) {
                try {
                    pState.close();
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

        return loginSuccess;
    }

    /**
     * 初始化用户界面
     * @return 用户输入的用户名和密码
     */
    private static Map<String, String> initUI() {
        Scanner scan = new Scanner(System.in);

        System.out.print("请输入用户名：");
        String userName = scan.nextLine();

        System.out.print("请输入密码：");
        String password = scan.nextLine();

        Map<String, String> userLoginInfo = new HashMap<>();
        userLoginInfo.put("userName",userName);
        userLoginInfo.put("password",password);

        return userLoginInfo;
    }
}
~~~

# Statement对比PreparedStatement

1. Statement存在SQL注入问题，而PreparedStatement解决了SQL注入问题。
2. Statement效率低于PreparedStatement。因为Statement是编译一次执行一次，而PreparedStatement是编译一次，可以执行N次。
3. Statement安全性也低于PreparedStatement。因为PreparedStatement会再编译阶段进行安全检查。
4. 我们工作中大多数使用PreparedStatement，但遇到业务方面要求需要进行SQL语句拼接、SQL注入时，就必须使用Statement了。

# JDBC代码模板PreparedStatement版

+ 配置文件

  ~~~properties
  driver=com.mysql.jdbc.Driver
  url=jdbc:mysql://localhost:3306/mydatabase
  user=root
  password=helloWorld
  ~~~

+ 在`mydatabase`中创建一个`person`表格

  ~~~java
  package top.liboshuai.study;
  
  import java.sql.*;
  import java.util.ResourceBundle;
  
  /**
   * 使用PreparedStatement版的JDBC代码模板
   * 创建一个person表格
   */
  public class Demo {
      public static void main(String[] args) {
          //使用资源绑定器绑定配置文件
          ResourceBundle resource = ResourceBundle.getBundle("jdbc");
          //获取配置文件中的信息
          String driver= resource.getString("driver");
          String url= resource.getString("url");
          String user= resource.getString("user");
          String password=resource.getString("password");
          
          Connection conn = null;
          PreparedStatement ps = null;
  
          try {
              //注册驱动
              Class.forName(driver);
              //获取数据库连接
              conn = DriverManager.getConnection(url, user, password);
              //定义SQL语句
              String sql = "CREATE TABLE person (id INT PRIMARY KEY AUTO_INCREMENT,NAME VARCHAR(20) NOT NULL,age INT);";
              //获取SQL语句预编译的执行对象
              ps = conn.prepareStatement(sql);
              //执行SQL语句
              int count = ps.executeUpdate();
              //处理结果
              System.out.println(count);//0
          } catch (ClassNotFoundException | SQLException e) {
              e.printStackTrace();
          } finally {
              //关闭资源
              if (ps != null) {
                  try {
                      ps.close();
                  } catch (SQLException throwables) {
                      throwables.printStackTrace();
                  }
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
  ~~~

+ 向`person`表格中插入三条记录

  ~~~java
  package top.liboshuai.study;
  
  import java.sql.*;
  import java.util.ResourceBundle;
  
  /**
   * JDBC模板preparedStatement版
   * 向`person`表格中插入三条记录
   */
  public class Demo {
      public static void main(String[] args) {
          //使用资源绑定器绑定配置文件
          ResourceBundle resource = ResourceBundle.getBundle("jdbc");
          //获取配置文件中的信息
          String driver = resource.getString("driver");
          String url = resource.getString("url");
          String user = resource.getString("user");
          String password = resource.getString("password");
  
          Connection conn = null;
          PreparedStatement ps01 = null;
          PreparedStatement ps02 = null;
          PreparedStatement ps03 = null;
  
          try {
              //注册数据库驱动
              Class.forName(driver);
              //获取数据库连接
              conn = DriverManager.getConnection(url,user,password);
              //定义SQL语句
              String sql01 = "INSERT INTO person (NAME,age) VALUES (?,?);";
              String sql02 = "INSERT INTO person (NAME,age) VALUES (?,?);";
              String sql03 = "INSERT INTO person (NAME,age) VALUES (?,?);";
              //获取SQL语句的预编译执行对象
              ps01 = conn.prepareStatement(sql01);
              ps02 = conn.prepareStatement(sql02);
              ps03 = conn.prepareStatement(sql03);
              //给占位符赋值
              ps01.setString(1,"one");
              ps01.setInt(2,21);
              ps02.setString(1,"two");
              ps02.setInt(2,22);
              ps03.setString(1,"three");
              ps03.setInt(2,23);
              //执行SQL语句
              int count = 0;
              count += ps01.executeUpdate();
              count += ps02.executeUpdate();
              count += ps03.executeUpdate();
              //处理结果
              if (count == 3) {
                  System.out.println("三条记录插入成功");
              } else {
                  System.out.println("三条记录插入失败");
              }
          } catch (ClassNotFoundException | SQLException e) {
              e.printStackTrace();
          } finally {
              //关闭资源
              if (ps01 != null) {
                  try {
                      ps01.close();
                  } catch (SQLException throwables) {
                      throwables.printStackTrace();
                  }
              }
              if (ps02 != null) {
                  try {
                      ps02.close();
                  } catch (SQLException throwables) {
                      throwables.printStackTrace();
                  }
              }
              if (ps03 != null) {
                  try {
                      ps03.close();
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
  ~~~

+ 删除`person`表格中的一条记录

  ~~~java
  package top.liboshuai.study;
  
  import java.sql.*;
  import java.util.ResourceBundle;
  
  public class Demo {
      public static void main(String[] args) {
          //使用资源绑定器绑定配置文件
          ResourceBundle resource = ResourceBundle.getBundle("jdbc");
          //获取配置文件中的信息
          String driver=resource.getString("driver");
          String url=resource.getString("url");
          String user=resource.getString("user");
          String password=resource.getString("password");
  
          Connection conn = null;
          PreparedStatement ps = null;
  
          try {
              //注册数据库驱动
              Class.forName(driver);
              //获取数据库连接
              conn = DriverManager.getConnection(url,user,password);
              //定义SQL语句
              String sql = "DELETE from person WHERE id = ?;";
              //获取SQL语句的预编译执行对象
              ps = conn.prepareStatement(sql);
              //给占位符赋值
              ps.setInt(1,2);
              //执行SQL语句
              int count = ps.executeUpdate();
              //处理结果
              System.out.println(count);//1
          } catch (ClassNotFoundException | SQLException e) {
              e.printStackTrace();
          } finally {
              if (ps != null) {
                  try {
                      ps.close();
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
  ~~~

+ 查询`person`表格中的指定列名记录

  ~~~java
  package top.liboshuai.study;
  
  import java.util.ResourceBundle;
  import java.sql.*;
  
  public class Demo {
      public static void main(String[] args) {
          //使用资源绑定器绑定配置文件
          ResourceBundle resource = ResourceBundle.getBundle("jdbc");
          //获取配置文件中的信息
          String driver=resource.getString("driver");
          String url=resource.getString("url");
          String user=resource.getString("user");
          String password=resource.getString("password");
  
          Connection conn = null;
          PreparedStatement ps = null;
          ResultSet result = null;
  
          try {
              //注册数据库驱动
              Class.forName(driver);
              //获取数据库连接
              conn = DriverManager.getConnection(url,user,password);
              //定义SQL
              String sql = "SELECT * FROM person WHERE NAME = ?;";
              //获取SQL语句的预编译执行对象
              ps = conn.prepareStatement(sql);
              //给占位符赋值
              ps.setString(1,"one");
              //执行SQL语句
              result = ps.executeQuery();
              //打印查询结果集
              while (result.next()) {
                  String name = result.getString("name");
                  int age = result.getInt("age");
  
                  System.out.println(name + ", " + age);
              }
          } catch (ClassNotFoundException | SQLException e) {
              e.printStackTrace();
          }
      }
  }
  ~~~

  