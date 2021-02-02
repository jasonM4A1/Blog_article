---
title: JDBCTemplate
date: 2021-02-02 05:35:38
tags:
	- JDBC
	- JavaWeb
categories:
	- JavaWeb
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202053546.png
---

# 概述

JDBCTemplate是Spring框架对JDBC进行的一个简单的封装，简化了关于JDBC的开发工作。

# 使用步骤

1. 导入jar包（五个）

2. 创建一个依赖于数据源`DataSource`的`JdbcTemplate`对象。

   ```
   JdbcTemplate template = new JdbcTemplate(ds);//ds为一个DataSource对象的引用
   ```

3. 通过JdbcTemplate对象调用下面介绍的方法，来完成CRUD的操作。

# 常用方法

1. `int update(String sql, @Nullable Object... args)`

   - 作用：用来执行DML语句的方法，即执行增、删、改语句的方法。
   - 参数
     1. `Stirng sql`：需要执行的SQL语句
     2. `@Nullable Object... args`：为SQL语句中占位符赋值，直接按顺序传值即可
   - 返回值：SQL语句执行影响到的行数

2. `Map<String, Object> queryForMap(String sql, @Nullable Object... args)`

   - 作用：查询表中数据，并将查询的结果集封装为map集合（查询的结果集长度只能为1）。

   - 参数

     1. `String sql`：需要执行的SQL语句
     2. `@Nullable Object... args`：为SQL语句中占位符赋值，直接按顺序传值即可

   - 返回值

     ```
     Map<String,Object
     ```

     1. `String`类型的键：查询得到的字段名
     2. `Object`类型的值：查询得到的字段值

3. `List<Map<String, Object>> queryForList(String sql, @Nullable Object... args)`

   - 作用：查询表中数据，并将查询的每一条结果封装为Map集合，然后所有的Map集合封装为list集合。
   - 参数
     1. `String sql`：需要执行的SQL语句
     2. `@Nullable Object... args`：为SQL语句中占位符赋值，直接按顺序传值即可
   - 返回值：每一个查询记录封装成的Map集合，然后多个Map集合封装为List集合。

4. `<T> List<T> query(String sql, RowMapper<T> rowMapper, @Nullable Object... args)`

   - 作用：查询表中数据，并将查询的结果集封装为相应的对象。
   - 参数：
     1. `String sql`：需要执行的SQL语句
     2. `RowMapper<T> rowMapper`：我们可以自己实现这个接口，也可以使用别人已经实现好的类`BeanPropertyRowMapper<T>`，这个类可以帮我们自动将数据库中的值封装为对象，然后再封装为List集合。(使用这个实现类时，封装查询数据的类中定义的数据类型，只能为引用数据类型，不能为基本数据类型)
     3. `@Nullable Object... args`：为SQL语句中占位符赋值，直接按顺序传值即可
   - 返回值：装有查询结果封装的对象的集合

5. `<T> T queryForObject(String sql, Class<T> requiredType, @Nullable Object... args)`

   - 作用：查询表中数据，并将结果封装为Object对象。(一般用来执行聚合函数)
   - 参数：
     1. `String sql`：需要执行的SQL语句
     2. `Class<T> requiredType`：你需要得到的数据类型的class对象
     3. `@Nullable Object... args`：为SQL语句中占位符赋值，直接按顺序传值即可
   - 返回值：查询得到的执行类型的结果。

> 上述所有的方法，都有自身的其他重载方法。我只是列出了有代表性的，剩余的其他方法的解释，可以去API中自行查找和阅读。

# 使用演示

- 数据库

  ```
  drop table if exists account;
  
  CREATE TABLE account (
  	id INT PRIMARY KEY AUTO_INCREMENT,
  	NAME VARCHAR(20),
  	balance DOUBLE(9,2)
  );
  
  INSERT INTO account (NAME, balance) VALUES ('张三',1000),('李四',1000);
  ```

- Account类

  ```
  package top.liboshuai.study;
  
  import java.util.Objects;
  
  public class Account {
      //必须使用引用数据类型
      private Integer id;
      private String name;
      private Double balance;
  
      //setter and getter
  
      public Integer getId() {
          return id;
      }
  
      public void setId(Integer id) {
          this.id = id;
      }
  
      public String getName() {
          return name;
      }
  
      public void setName(String name) {
          this.name = name;
      }
  
      public Double getBalance() {
          return balance;
      }
  
      public void setBalance(Double balance) {
          this.balance = balance;
      }
  
      @Override
      public boolean equals(Object o) {
          if (this == o) return true;
          if (o == null || getClass() != o.getClass()) return false;
          Account account = (Account) o;
          return Objects.equals(id, account.id) &&
                  Objects.equals(name, account.name) &&
                  Objects.equals(balance, account.balance);
      }
  
      @Override
      public int hashCode() {
          return Objects.hash(id, name, balance);
      }
  
      @Override
      public String toString() {
          return "Account{" +
                  "id=" + id +
                  ", name='" + name + '\'' +
                  ", balance=" + balance +
                  '}';
      }
  }
  ```

- 修改表中的两个数据

  ```
  package top.liboshuai.study;
  
  import com.alibaba.druid.pool.DruidDataSourceFactory;
  import org.springframework.jdbc.core.JdbcTemplate;
  
  import javax.sql.DataSource;
  import java.io.IOException;
  import java.io.InputStream;
  import java.util.Properties;
  
  public class Demo {
      public static void main(String[] args) {
          //数据库连接池对象
          DataSource ds = null;
  
          //加载配置文件
          Properties pro = new Properties();
          InputStream in = Demo.class.getClassLoader().getResourceAsStream("druid.properties");
          try {
              pro.load(in);
          } catch (IOException e) {
              e.printStackTrace();
          }
  
          //获取数据库连接池对象
          try {
              ds = DruidDataSourceFactory.createDataSource(pro);
          } catch (Exception e) {
              e.printStackTrace();
          }
  
          //创建JDBCTemplate对象
          JdbcTemplate template01 = new JdbcTemplate(ds);
  
          //定义SQL语句
          String sql01 = "UPDATE account SET balance = balance - 455.55 WHERE id = ?;";
          String sql02 = "UPDATE account SET balance = balance + 455.55 WHERE id = ?;";
  
          //给占位符赋值的同时直接执行SQL语句(不需要归还连接对象，也不需要关闭资源）
          int count = template01.update(sql01,1);
          count += template01.update(sql02,2);
  
          //打印执行结果
          System.out.println(count);
      }
  }
  ```

- 向表中添加两条记录

  ```
  package top.liboshuai.study;
  
  import com.alibaba.druid.pool.DruidDataSourceFactory;
  import org.springframework.jdbc.core.JdbcTemplate;
  
  import javax.sql.DataSource;
  import java.io.IOException;
  import java.io.InputStream;
  import java.util.Properties;
  
  public class Demo {
      public static void main(String[] args) {
          //数据库连接池对象
          DataSource ds = null;
  
          //加载配置文件
          Properties pro = new Properties();
          InputStream in = Demo.class.getClassLoader().getResourceAsStream("druid.properties");
          try {
              pro.load(in);
          } catch (IOException e) {
              e.printStackTrace();
          }
  
          //获取数据库连接池对象
          try {
              ds = DruidDataSourceFactory.createDataSource(pro);
          } catch (Exception e) {
              e.printStackTrace();
          }
  
          //创建JDBCTemplated对象
          JdbcTemplate template = new JdbcTemplate(ds);
  
          //定义SQL语句
          String sql = "INSERT INTO account (NAME,balance) VALUES (?,?),(?,?);";
  
          //给占位符赋值的同时直接执行SQL语句(不需要归还连接对象，也不需要关闭资源）
          int count = template.update(sql,"赵六",3000,"刘七",1500);
  
          //打印执行结果
          System.out.println(count);//2
      }
  }
  ```

- 删除刚才添加的两条记录

  ```
  package top.liboshuai.study;
  
  import com.alibaba.druid.pool.DruidDataSourceFactory;
  import org.springframework.jdbc.core.JdbcTemplate;
  
  import javax.sql.DataSource;
  import java.io.IOException;
  import java.io.InputStream;
  import java.util.Properties;
  
  public class Demo {
      public static void main(String[] args) {
          //数据库连接池对象
          DataSource ds = null;
  
          //加载配置文件
          Properties pro = new Properties();
          InputStream in = Demo.class.getClassLoader().getResourceAsStream("druid.properties");
          try {
              pro.load(in);
          } catch (IOException e) {
              e.printStackTrace();
          }
  
          try {
              //获取数据库连接池对象
              ds = DruidDataSourceFactory.createDataSource(pro);
          } catch (Exception e) {
              e.printStackTrace();
          }
  
          //创建JDBCTemplated对象
          JdbcTemplate template = new JdbcTemplate(ds);
  
          //定义SQL语句
          String sql = "DELETE FROM account WHERE id IN(?,?);";
  
          //给占位符赋值的同时，执行SQL语句(不需要归还数据库连接对象，也不需要关闭资源)
          int count = template.update(sql,3,4);
  
          //打印执行结果
          System.out.println(count);
      }
  }
  ```

- 查询表记录，将其封装为Map集合

  ```
  package top.liboshuai.study;
  
  import com.alibaba.druid.pool.DruidDataSourceFactory;
  import org.springframework.jdbc.core.JdbcTemplate;
  
  import javax.sql.DataSource;
  import java.io.IOException;
  import java.io.InputStream;
  import java.util.Map;
  import java.util.Properties;
  import java.util.Set;
  
  public class Demo {
      public static void main(String[] args) {
          //数据库连接池对象
          DataSource ds = null;
  
          //加载配置文件
          Properties pro = new Properties();
          InputStream in = Demo.class.getClassLoader().getResourceAsStream("druid.properties");
          try {
              pro.load(in);
          } catch (IOException e) {
              e.printStackTrace();
          }
  
          try {
              //获取数据库连接池对象
              ds = DruidDataSourceFactory.createDataSource(pro);
          } catch (Exception e) {
              e.printStackTrace();
          }
  
          //创建JDBCTemplate对象
          JdbcTemplate template = new JdbcTemplate(ds);
  
          //定义SQL语句
          String sql = "SELECT * FROM account WHERE id = ?;";
  
          //给占位符赋值的同时，执行SQL语句(不需要归还数据库连接对象，也不需要关闭资源)
          Map<String,Object> map = template.queryForMap(sql,1);
  
          //遍历Map集合
  
          //获取Map集合中键得set集合
          Set<String> keySet = map.keySet();
          //通过遍历keySet获取Map集合中的值
          for (String k: keySet) {
              Object value = map.get(k);
              System.out.println(k+": "+value);
          }
      }
  }
  ```

- 查询表记录，将其封装为List集合

  ```
  package top.liboshuai.study;
  
  import com.alibaba.druid.pool.DruidDataSourceFactory;
  import org.springframework.jdbc.core.JdbcTemplate;
  
  import javax.sql.DataSource;
  import java.io.IOException;
  import java.io.InputStream;
  import java.util.List;
  import java.util.Map;
  import java.util.Properties;
  
  public class Demo {
      public static void main(String[] args) {
          //数据库连接池对象
          DataSource ds = null;
  
          //加载配置文件
          Properties pro = new Properties();
          InputStream in = Demo.class.getClassLoader().getResourceAsStream("druid.properties");
          try {
              pro.load(in);
          } catch (IOException e) {
              e.printStackTrace();
          }
  
          try {
              //获取数据库连接池对象
              ds = DruidDataSourceFactory.createDataSource(pro);
          } catch (Exception e) {
              e.printStackTrace();
          }
  
          //创建JDBCTemplate对象
          JdbcTemplate template = new JdbcTemplate(ds);
  
          //定义SQL语句
          String sql = "SELECT * FROM account WHERE id IN(?,?);";
  
          //给占位符赋值的同时直接执行SQL语句(不需要归还连接对象，也不需要关闭资源）
          List<Map<String, Object>> list = template.queryForList(sql,1,2);
  
          //遍历List集合
          for (Map<String, Object> m: list) {
              System.out.println(m);
          }
      }
  }
  ```

- 查询表记录，将其封装为Account对象的List集合

  ```
  package top.liboshuai.study;
  
  import com.alibaba.druid.pool.DruidDataSourceFactory;
  import org.springframework.jdbc.core.BeanPropertyRowMapper;
  import org.springframework.jdbc.core.JdbcTemplate;
  
  import javax.sql.DataSource;
  import java.io.IOException;
  import java.io.InputStream;
  import java.util.List;
  import java.util.Properties;
  
  public class Demo {
      public static void main(String[] args) {
          //数据库连接池对象
          DataSource ds = null;
  
          //加载配置文件
          Properties pro = new Properties();
          InputStream in = Demo.class.getClassLoader().getResourceAsStream("druid.properties");
          try {
              pro.load(in);
          } catch (IOException e) {
              e.printStackTrace();
          }
  
          //获取数据库连接池对象
          try {
              ds = DruidDataSourceFactory.createDataSource(pro);
          } catch (Exception e) {
              e.printStackTrace();
          }
  
          //创建JDBCTemplate对象
          JdbcTemplate template = new JdbcTemplate(ds);
  
          //定义SQL语句
          String sql = "SELECT * FROM account WHERE id IN(?,?);";
  
          //给占位符赋值的同时直接执行SQL语句(不需要归还连接对象，也不需要关闭资源)
          List<Account> list = template.query(sql,new BeanPropertyRowMapper<Account>(Account.class),1,2);
  
          //遍历查询得到的list集合
          for (Account a: list) {
              System.out.println(a);
          }
      }
  }
  ```

- 查询总记录数

  ```
  package top.liboshuai.study;
  
  import com.alibaba.druid.pool.DruidDataSourceFactory;
  import org.springframework.jdbc.core.JdbcTemplate;
  
  import javax.sql.DataSource;
  import java.io.IOException;
  import java.io.InputStream;
  import java.util.Properties;
  
  public class Demo {
      public static void main(String[] args) {
          //数据库连接池对象
          DataSource ds = null;
  
          //加载配置文件
          Properties pro = new Properties();
          InputStream in = Demo.class.getClassLoader().getResourceAsStream("druid.properties");
          try {
              pro.load(in);
          } catch (IOException e) {
              e.printStackTrace();
          }
  
          //获取数据库连接池对象
          try {
              ds = DruidDataSourceFactory.createDataSource(pro);
          } catch (Exception e) {
              e.printStackTrace();
          }
  
          //创建JDBCTemplate对象
          JdbcTemplate template = new JdbcTemplate(ds);
  
          //定义SQL语句
          String sql = "SELECT COUNT(id) FROM account; ";
  
          //给占位符赋值的同时直接执行SQL语句(不需要归还连接对象，也不需要关闭资源)
          Long total = template.queryForObject(sql,Long.class);
  
          //打印查询结果
          System.out.println(total);
      }
  }
  ```