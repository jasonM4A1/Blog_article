---
title: Mybatis入门
date: 2021-05-16 11:45:05
tags:
	- SSM
	- Mybatis
categories:
	- SSM
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/wallhaven-4gpz9l.jpg
---

# 环境准备

+ jdk 8+
+ MySQL 5.7.19
+ maven-3.6.1
+ IDEA较新版本

# 所需知识

+ JDBC
+ MySQL
+ Java 基础
+ Maven
+ Junit

# 什么是MyBatis?

+ MyBatis 是一款优秀的**持久层框架**
+ MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集的过程
+ MyBatis 可以使用简单的 XML 或注解来配置和映射原生信息，将接口和 Java 的 实体类 【Plain Old Java Objects,普通的 Java对象】映射成数据库中的记录
+ MyBatis 本是apache的一个开源项目ibatis, 2010年这个项目由apache 迁移到了google code，并且改名为MyBatis , 2013年11月迁移到**Github** .

> - Mybatis官方文档 : http://www.mybatis.org/mybatis-3/zh/index.html
> - GitHub : https://github.com/mybatis/mybatis-3

# 什么是持久化

**持久化是将程序数据在持久状态和瞬时状态间转换的机制。**

+ 即把数据（如内存中的对象）保存到可永久保存的存储设备中（如磁盘）。持久化的主要应用是将内存中的对象存储在数据库中，或者存储在磁盘文件中、XML数据文件中等等。
+ JDBC就是一种持久化机制。文件IO也是一种持久化机制。
+ 在生活中 : 将鲜肉冷藏，吃的时候再解冻的方法也是。将水果做成罐头的方法也是。

**为什么需要持久化服务呢？那是由于内存本身的缺陷引起的**

+ 内存断电后数据会丢失，但有一些对象是无论如何都不能丢失的，比如银行账号等，遗憾的是，人们还无法保证内存永不掉电。
+ 内存过于昂贵，与硬盘、光盘等外存相比，内存的价格要高2~3个数量级，而且维持成本也高，至少需要一直供电吧。所以即使对象不需要永久保存，也会因为内存的容量限制不能一直呆在内存中，需要持久化来缓存到外存。

# 什么是持久层?

+ 完成持久化工作的代码块 .  ---->  dao层 【DAO (Data Access Object)  数据访问对象】
+ 大多数情况下特别是企业级应用，数据持久化往往也就意味着将内存中的数据保存到磁盘上加以固化，而持久化的实现过程则大多通过各种**关系数据库**来完成。
+ 不过这里有一个字需要特别强调，也就是所谓的“层”。对于应用系统而言，数据持久功能大多是必不可少的组成部分。也就是说，我们的系统中，已经天然的具备了“持久层”概念？也许是，但也许实际情况并非如此。之所以要独立出一个“持久层”的概念,而不是“持久模块”，“持久单元”，也就意味着，我们的系统架构中，应该有一个相对独立的逻辑层面，专注于数据持久化逻辑的实现.
+ 与系统其他部分相对而言，这个层面应该具有一个较为清晰和严格的逻辑边界。【说白了就是用来操作数据库存在的！】

# 为什么需要Mybatis?

+ Mybatis就是帮助程序猿将数据存入数据库中 , 和从数据库中取数据 .
+ 传统的jdbc操作 , 有很多重复代码块 .比如 : 数据取出时的封装 , 数据库的建立连接等等... , 通过框架可以减少重复代码,提高开发效率 .
+ MyBatis 是一个半自动化的**ORM框架 (Object Relationship Mapping) -->对象关系映射**
+ 所有的事情，不用Mybatis依旧可以做到，只是用了它，所有实现会更加简单！**技术没有高低之分，只有使用这个技术的人有高低之别**

# Mybatis优点

+ 简单易学：本身就很小且简单。没有任何第三方依赖，最简单安装只要两个jar文件+配置几个sql映射文件就可以了，易于学习，易于使用，通过文档和源代码，可以比较完全的掌握它的设计思路和实现。
+ 灵活：mybatis不会对应用程序或者数据库的现有设计强加任何影响。sql写在xml里，便于统一管理和优化。通过sql语句可以满足操作数据库的所有需求。
+ 解除sql与程序代码的耦合：通过提供DAO层，将业务逻辑和数据访问逻辑分离，使系统的设计更清晰，更易维护，更易单元测试。sql和代码的分离，提高了可维护性。
+ 提供xml标签，支持编写动态sql。
+ 等等……
+ 最重要的一点，使用的人多！公司需要！

# 第一个Mybatis程序

## 第一步：搭建数据库

~~~mysql
DROP DATABASE IF EXISTS `Mybatis`;

CREATE DATABASE `Mybatis`;

USE Mybatis;

DROP TABLE IF EXISTS `user`;

CREATE TABLE `user` (
	`id` INT(20) NOT NULL,
	`name` VARCHAR(30) DEFAULT NULL,
	`pwd` VARCHAR(30) DEFAULT NULL,
	PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT charset=utf8;

INSERT INTO `user`(`id`,`name`,`pwd`) VALUES(1,'Jason','123456'),(2,'fuck','123456'),(3,'you','123456');
~~~

## 第二步：导入Mybatis相关Jar包

文件路径为：`pom.xml`

~~~xml
<dependencies>
  <!--mysql驱动-->
  <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
  <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.46</version>
  </dependency>
  <!--mybatis-->
  <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
  <dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.2</version>
  </dependency>
  <!--junit-->
  <!-- https://mvnrepository.com/artifact/junit/junit -->
  <dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
  </dependency>
</dependencies>
~~~

## 第三步：编写Mybatis-config核心配置文件

文件路径为：`src/main/resources/mybatis-config.xml`

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="intmain()"/>
            </dataSource>
        </environment>
    </environments>

    <!--每一个Mapper.xml都需要在Mybatis核心配置文件中注册-->
    <mappers>
        <mapper resource="xyz/rtx3090/dao/UserMapper.xml" />
    </mappers>
</configuration>
~~~

## 第四步：编写Mybatis工具类

文件路径为：`src/main/java/xyz/rtx3090/utils/MybatisUtils.java`

~~~java
package xyz.rtx3090.utils;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

public class MybatisUtils {

    //提升作用域
    private static SqlSessionFactory sqlSessionFactory;

    //使用Mybatis第一步：获取sqlSessionFactory对象
    static{
        try {
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    //使用Mybatis第二步：利用sqlSessionFactory对象创建sqlSession实例 (这个sqlSession对象完全包含了面向数据库执行SQL命令所需的所有方法)
    public static SqlSession getSqlSession() {
        return sqlSessionFactory.openSession();
    }
}
~~~

##  第五步：创建实体类 

文件路径：`src/main/java/xyz/rtx3090/pojo/User.java`

~~~java
package xyz.rtx3090.pojo;

import java.util.Objects;

public class User {
    private int id;
    private String name;
    private String pwd;
		//constructor
		//setter and getter
		//equals and hashcode
		//toString
}
~~~

## 第六步：编写Mapper接口类

文件路径：`src/main/java/xyz/rtx3090/dao/UserMapper.java`

~~~java
package xyz.rtx3090.dao;

import xyz.rtx3090.pojo.User;

import java.util.List;

public interface UserMapper {
    List<User> getUserList();
}
~~~

## 第七步：编写Mapper.xml配置文件 

文件路径：`src/main/java/xyz/rtx3090/dao/UserMapper.xml`

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace用于绑定一个对应的Dao/Mapper接口-->
<mapper namespace="xyz.rtx3090.dao.UserMapper">
    <!--select查询语句: id为UserDao中的方法，resultType为UserDao返回结果集的类型-->
    <select id="getUserList" resultType="xyz.rtx3090.pojo.User">
        select * from mybatis.user
    </select>
</mapper>
~~~

> 配置文件中namespace中的名称为对应Mapper接口或者Dao接口的完整包名,必须一致！

## 第八步：解决Mave静态资源过滤问题

文件路径：`pom.xml`

~~~xml
    <!-- 在build中配置resources,来防止我们资源导出失败的问题-->
    <build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
        </resources>
    </build>
~~~

## 第九步：编写测试类

文件路径：`src/test/java/xyz/rtx3090/dao/UserMapperTest.java`

~~~java
package xyz.rtx3090.dao;

import org.apache.ibatis.session.SqlSession;
import org.junit.Test;
import xyz.rtx3090.pojo.User;
import xyz.rtx3090.utils.MybatisUtils;

import java.util.List;

public class UserMapperTest {
    @Test
    public void test() {
        //使用前面创建好的Mybatis工具类，获取一个sqlSession对象
        SqlSession sqlSession = MybatisUtils.getSqlSession();

        try{
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            List<User> userList = mapper.getUserList();

            for (User user: userList
            ) {
                System.out.println(user);
            }
        } catch(Exception e) {
            e.printStackTrace();
        }finally {
            //关闭 sqlSession
            sqlSession.close();
        }
    }
}
~~~

> 测试遍历结果为：
>
> User{id=1, name='Jason', pwd='123456'}
> User{id=2, name='fuck', pwd='123456'}
> User{id=3, name='you', pwd='123456'}

# 常见错误

## 问题描述

在写第一个mybatis程序时，遇到的连接失败的问题，如下：

~~~
org.apache.ibatis.exceptions.PersistenceException:
### Error querying database.
~~~

## 解决方法
1. 查看mybatis-config.xml文件中的url一项中的value值配置是否正确
   
    > 正确配置为：jdbc:mysql://localhost:3306:/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF-8
2. 在项目中的porm.xml文件中配置下面代码，防止Java代码目录中的静态配置文件导出失败
    ~~~xml
        <!-- 在build中配置resources,来防止我们资源导出失败的问题-->
    <build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
        </resources>
    </build>
    ~~~

# Mybatis的CRUD操作及配置解析

> 我们使用了Mybatis，就不需要为Mapper.java接口类创建实现类。而是直接在Mapper.xml中使用对应的xml标签进行配置即可，下面我来详解一个各个标签的使用和作用。

## select标签——查询

### 作用

用于**实现**Mapper.java接口类中规定的关于**查询**数据库中数据的相关方法。

### 使用

**Mapper.xml文件配置**

~~~xml
<mapper namespace="xyz.rtx3090.dao.UserMapper">
    <!--查询全部用户-->
    <select id="getUserList" resultType="xyz.rtx3090.pojo.User">
        select * from mybatis.user
    </select>

    <!--根据id查找用户-->
    <select id="getUserById" parameterType="int" resultType="xyz.rtx3090.pojo.User">
        select * from mybatis.user where id = #{id}
    </select>
</mapper>
~~~

**Mapper.java接口类**

~~~java
public interface UserMapper {
    //查询全部用户
    List<User> getUserList();

    //根据id查询指定用户
    User getUserById(int id);
}
~~~

## insert标签——添加

### 作用

用于**实现**Mapper.java接口类中规定的关于**添加**数据库中数据的相关方法。

### 使用

**Mapper.xml文件配置**

~~~xml
<mapper namespace="xyz.rtx3090.dao.UserMapper">
		<!--添加指定用户-->
    <insert id="addUser" parameterType="xyz.rtx3090.pojo.User" >
        insert into mybatis.user (id, name, pwd) VALUES (#{id},#{name},#{pwd});
    </insert>
</mapper>
~~~

**Mapper.java接口类**

~~~java
public interface UserMapper {
    //添加指定用户
    int addUser(User user);
}
~~~

## update标签——更改

### 作用

用于**实现**Mapper.java接口类中规定的关于**更改**数据库中数据的相关方法。

### 使用

**Mapper.xml文件配置**

~~~xml
<mapper namespace="xyz.rtx3090.dao.UserMapper">
    <!--修改指定用户-->
    <update id="updateUser" parameterType="xyz.rtx3090.pojo.User">
        update mybatis.user set name=#{name}, pwd=#{pwd} where id=#{id}
    </update>
</mapper>
~~~

**Mapper.java接口类**

~~~java
public interface UserMapper {
    //修改指定用户
    int updateUser(User user);
}
~~~

## delect标签——删除

### 作用

用于**实现**Mapper.java接口类中规定的关于**删除**数据库中数据的相关方法。

### 使用

**Mapper.xml文件配置**

~~~xml
<mapper namespace="xyz.rtx3090.dao.UserMapper">
		<!--删除指定用户-->
    <delete id="deleteUser" parameterType="xyz.rtx3090.pojo.User">
        delete from mybatis.user where id=#{id}
    </delete>
</mapper>
~~~

**Mapper.java接口类**

~~~java
public interface UserMapper {
    //删除指定用户
    int deleteUser(int id);
}
~~~

## 注意事项

+ 配置文件中namespace中的名称为对应Mapper接口或者Dao接口的完整包名,必须一致！
+ 查询使用`<select><select/>`标签，添加使用`<insert><insert/>`标签，更改使用`<update><update/>`标签，删除使用`<delect><delect/>`标签。
+ 属性id的值为：Mapper.java接口类中规定对应查询功能方法名（必须完全一致）
+ 属性parameterType的值为：Mapper.java接口类中规定对应查询功能方法参数类型（必须为完整包名）
+ 属性resultType的值为：Mapper.java接口类中规定的对应查询功能方法返回值类型（必须为完整包名）
+ 标签内的SQL语句中的占位符使用`#{名称}`来表示
+ 标签内的SQL语句末尾不需要写`;`
+ 除了查询，增加、删除、更改这三个操作都需要提交事务，才能生效！
+ 为了规范操作，在SQL的配置文件中，我们尽量将Parameter参数和resultType都写上！
+ 接口所有的普通参数，尽量都写上@Param参数，尤其是多个参数时，必须写上！
+ 有时候根据业务的需求，可以考虑使用map传递参数！
+ Mapper.xml配置文件中的`<mapper><mapper/>`标签的`namespace`属性用`.`进行路径分割
+ Mybatis核心配置文件中的`<mapper><mapper/>`标签的 `resource`属性使用`/`进行路径分割

# SQL占位符传参的三种方式

## 第一种方式

采用直接将对象属性传入`#{属性名}`中的方式，其中`#{属性名}`被要求必须与对象属性名一致。

缺点：我们需要对象所有的属性逐个对应传入，不管是否真正需求。所以当对象包含的属性过多时，这种方式就不在适用了。

**Mapper.java接口类**

~~~java
public interface UserMapper {
    //根据id查询指定用户
    User getUserById(int id);
}
~~~

**Mapper.xml配置文件**

~~~xml
<mapper namespace="xyz.rtx3090.dao.UserMapper">
    <!--根据id查找用户-->
    <select id="getUserById" parameterType="int" resultType="xyz.rtx3090.pojo.User">
        select * from mybatis.user where id = #{id}
    </select>
</mapper>
~~~

**测试类**

~~~java
    @Test
    public void TestGetUserById() {
        //使用前面创建好的Mybatis工具类，获取一个sqlSession对象
        SqlSession sqlSession = MybatisUtils.getSqlSession();

        try {
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            User userById = mapper.getUserById(1);//查找id为1的用户
            System.out.println(userById);
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            //关闭sqlSession
            sqlSession.close();
        }
    }
~~~

## 第二种方式

为了解决第一种无非处理属性过多的对象的问题，我们可以采用第二种：将参数首先存储到Map集合中，然后在`#{Key}`中传入Map集合的key值来获取Map集合存储的对应的value值。

**优点**：即使对象包含属性再多，我们也可以轻松应对。我们不需要规定`#{key}`必须与对象属性名一致，而是与我们自己创建的Map集合的key值一致即可。

**Mapper.java接口类**

~~~java
public interface UserMapper {
    //使用map参数添加指定用户
    int addUser2(Map<String, Object> user);
}
~~~

**Mapper.xml配置文件**

~~~xml
<mapper namespace="xyz.rtx3090.dao.UserMapper">
    <!--使用Mapper参数添加指定用户-->
    <insert id="addUser2" parameterType="map">
        insert into mybatis.user (id, name, pwd) VALUES (#{userId},#{userName},#{userPwd});
    </insert>
</mapper>
~~~

**测试类**

~~~java
    @Test
    public void TestAddUser2() {
        //使用前面创建好的Mybatis工具类，获取一个sqlSession对象
        SqlSession sqlSession = MybatisUtils.getSqlSession();

        try{
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            Map<String, Object> user = new HashMap<String, Object>();//创建用于存储参数的Map集合
            user.put("userId",6);//向Map集合中存储参数
            user.put("userName","charlie");
            user.put("userPwd","497560123");
            int i = mapper.addUser2(user);//将存储着参数的Map集合传入对应方法中
            System.out.println(i == 1 ? "添加成功":"添加失败");
            //提交事务
            sqlSession.commit();
        } catch(Exception e) {
            e.printStackTrace();
        }finally {
            //关闭sqlSession
            sqlSession.close();
        }
    }
~~~

## 第三种方式

参数

# 模糊查询

目前我只知道采用在Java代码中添加Sql通配符的方式来进行模糊查询

**Mapper.java接口类**

~~~java
public interface UserMapper {
    //根据name进行查询用户
    List<User> getUserByName(String name);
}
~~~

**Mapper.xml配置文件**

~~~xml
<mapper namespace="xyz.rtx3090.dao.UserMapper">
    <!--根据name查找用户-->
    <select id="getUserByName" parameterType="String" resultType="xyz.rtx3090.pojo.User">
        select * from mybatis.user where name like #{name}
    </select>
</mapper>
~~~

> 注意：where后面采用的是like，而非=

**测试类代码**

~~~java
    @Test
    public void TestGetUserByName(){
        //使用前面创建好的Mybatis工具类，获取一个sqlSession对象
        SqlSession sqlSession = MybatisUtils.getSqlSession();

        try {
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            List<User> userByName = mapper.getUserByName("%a%");//查询name中有a的用户

            for (User user: userByName
                 ) {
                System.out.println(user);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            //关闭sqlSession
            sqlSession.close();
        }
    }
~~~

# 核心配置文件mybatis-config.xml文件详解

mybatis-config.xml文件是mybatis的核心配置文件，包含了会深深影响 MyBatis 行为的设置和属性信息。

## 配置文件内容完整实例

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--属性-->
    <properties resource="org/mybatis/example/config.properties">
        <property name="username" value="dev_user"/>
        <property name="password" value="F2Fa3!33TYyg"/>
    </properties>

    <!--设置-->
    <settings>
        <setting name="cacheEnabled" value="true"/>
        <setting name="lazyLoadingEnabled" value="true"/>
        <setting name="multipleResultSetsEnabled" value="true"/>
        <setting name="useColumnLabel" value="true"/>
        <setting name="useGeneratedKeys" value="false"/>
        <setting name="autoMappingBehavior" value="PARTIAL"/>
        <setting name="autoMappingUnknownColumnBehavior" value="WARNING"/>
        <setting name="defaultExecutorType" value="SIMPLE"/>
        <setting name="defaultStatementTimeout" value="25"/>
        <setting name="defaultFetchSize" value="100"/>
        <setting name="safeRowBoundsEnabled" value="false"/>
        <setting name="mapUnderscoreToCamelCase" value="false"/>
        <setting name="localCacheScope" value="SESSION"/>
        <setting name="jdbcTypeForNull" value="OTHER"/>
        <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
    </settings>

    <!--类型别名-->
    <typeAliases>
        <typeAlias alias="Author" type="domain.blog.Author"/>
        <typeAlias alias="Blog" type="domain.blog.Blog"/>
        <typeAlias alias="Comment" type="domain.blog.Comment"/>
        <typeAlias alias="Post" type="domain.blog.Post"/>
        <typeAlias alias="Section" type="domain.blog.Section"/>
        <typeAlias alias="Tag" type="domain.blog.Tag"/>
    </typeAliases>

    <!--类型处理器-->
    <typeHandlers>
        <typeHandler handler="org.mybatis.example.ExampleTypeHandler"/>
    </typeHandlers>

    <!--对象工厂-->
    <objectFactory type="org.mybatis.example.ExampleObjectFactory">
        <property name="someProperty" value="100"/>
    </objectFactory>

    <!--插件-->
    <plugins>
        <plugin interceptor="org.mybatis.example.ExamplePlugin">
            <property name="someProperty" value="100"/>
        </plugin>
    </plugins>

    <!--环境配置-->
    <environments default="development">
        <environment id="development">
            <!--事务管理器-->
            <transactionManager type="JDBC">
                <property name="..." value="..."/>
            </transactionManager>
            <!--数据源-->
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>

    <!--数据库厂商表识-->
    <databaseIdProvider type="DB_VENDOR">
        <property name="SQL Server" value="sqlserver"/>
        <property name="DB2" value="db2"/>
        <property name="Oracle" value="oracle" />
    </databaseIdProvider>

    <!--映射器-->
    <!-- 使用相对于类路径的资源引用 -->
    <mappers>
        <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>
        <mapper resource="org/mybatis/builder/BlogMapper.xml"/>
        <mapper resource="org/mybatis/builder/PostMapper.xml"/>
    </mappers>

<!--    使用完全限定资源定位符（URL）
    <mappers>
        <mapper url="file:///var/mappers/AuthorMapper.xml"/>
        <mapper url="file:///var/mappers/BlogMapper.xml"/>
        <mapper url="file:///var/mappers/PostMapper.xml"/>
    </mappers>-->

<!--    使用映射器接口实现类的完全限定类名
    <mappers>
        <mapper class="org.mybatis.builder.AuthorMapper"/>
        <mapper class="org.mybatis.builder.BlogMapper"/>
        <mapper class="org.mybatis.builder.PostMapper"/>
    </mappers>-->

<!--     将包内的映射器接口实现全部注册为映射器
    <mappers>
        <package name="org.mybatis.builder"/>
    </mappers>-->
</configuration>
~~~

## 配置文件内容结构分析

+ configuration——配置
  + properties——属性（重点）
  + settings——设置
  + typeAliases——类型别名（重点）
  + typeHandlers——类型处理器
  + objectFactory——对象工厂
  + plugins——插件
  + environments——环境配置们（重点）
    + environment——环境变量
      + transactionManager——事务管理器
      + dataSource——数据源
  + databaseIdProvider——数据库厂商标识
  + mappers——映射器（重点）

## properties标签

数据库这些属性都是可外部配置且可动态替换的，既可以在典型的 Java 属性文件中配置，亦可通过 properties 元素的子元素来传递。具体的官方文档

我们来优化我们的配置文件

**第一步：在资源目录下新建一个`db.properties`**

~~~properties
driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis?useSSL=false&useUnicode=true&characterEncoding=UTF-8
username=root
password=intmain()
~~~

**第二步 : 将db.properties文件导入mybatis-config.xml配置文件**

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--引用外部文件-->
    <properties resource="db.properties">
      	<!--除了properties文件中的，我们还可以自己定义键值对-->
        <property name="username" value="root"/>
        <property name="password" value="intmain()"/>
    </properties>   

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="xyz/rtx3090/dao/UserMapper.xml"/>
    </mappers>
</configuration>

> + 外部引用的properites配置的内容优先级高于，我们标签内自定义的内容。
> + 引用properties配置文件后，下面`<environments> --> <environment> --> <dataSource> --> <property>`的value值需要用`$`占位符代替。

## typeAliases标签

类型别名是为 Java 类型设置一个短的名字。它只和 XML 配置有关，存在的意义仅在于用来减少类完全限定名的冗余。

我们可以在`mybatis-config.xml`中采用`<typeAliases>`标签来为一个实体类配置自定义的别名。

​~~~xml
<configuration>
    <typeAliases>
        <!--给一个具体的实体类，指定自定义的别名-->
        <typeAlias type="xyz.rtx3090.pojo.User" alias="User"/>
    </typeAliases>
</configuration>
~~~

我们可以在`mybatis-config.xml`中采用`<typeAliases>`标签来为一个包下的所有实体类配置默认别名。

~~~xml
<configuration>
    <typeAliases>
        <!--给一个包下的所有实体类，起别名为类名小写-->
        <package name="xyz.rtx3090.pojo"/>
    </typeAliases>
</configuration>
~~~

除了上述的两种在 `mybatis-config.xml`进行别名配置，我们还可以采用在实体类上注解的方式来起别名

~~~java
package xyz.rtx3090.pojo;

import org.apache.ibatis.type.Alias;

import java.util.Objects;

@Alias("hello")//给这个实体类起了别名为hello
public class User {
    private int id;
    private String name;
    private String pwd;

    //constructor
    //setter and getter
  	//toString
  	//equals and hashCode
}
~~~

## environments标签

mybatis可以配置多套环境，将SQL映射到多个不同的数据库上，但必须指定其中一个为默认运行环境（通过default指定），如下所示

~~~xml
<configuration>
  <!--这里有两套运行环境，我们默认选择了id为development的这套环境-->
    <environments default="development">
        <environment id="development">
          	<!--事务管理器，tyep有两个值：JDBC和MANAGED-->
            <transactionManager type="JDBC"/>
          	<!--数据源，type有三个值：UNPOOLED、POOLED和JNDI-->
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
        <environment id="mood">
            <transactionManager type="JDBC"/>
            <dataSource type="UNPOOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
</configuration>
~~~

> **关于dataSource标签中的type值问题**
>
> 首先有三个值： UNPOOLED、POOLED和JNDI
>
> + **UNPOOLED：**这个数据源的实现只是每次被请求时打开和关闭连接。
> + **POOLED：**这种数据源的实现利用“池”的概念将 JDBC 连接对象组织起来 , 这是一种使得并发 Web 应用快速响应请求的流行处理方式。
> + **JNDI：**这个数据源的实现是为了能在如 Spring 或应用服务器这类容器中使用，容器可以集中或在外部配置数据源，然后放置一个 JNDI 上下文的引用。

## mappers标签

+ mappers映射器，用于定义映射SQL语句文件
+ 既然 MyBatis 的行为其他元素已经配置完了，我们现在就要定义 SQL 映射语句了。但是首先我们需要告诉 MyBatis 到哪里去找到这些语句。Java 在自动查找这方面没有提供一个很好的方法，所以最佳的方式是告诉 MyBatis 到哪里去找映射文件。
+ 你可以使用相对于类路径的资源引用， 或完全限定资源定位符（包括 `file:///` 的 URL），或类名和包名等。
+ 映射器是MyBatis中最核心的组件之一，在MyBatis 3之前，只支持xml映射器，即：所有的SQL语句都必须在xml文件中配置。而从MyBatis 3开始，还支持接口映射器，这种映射器方式允许以Java代码的方式注解定义SQL语句，非常简洁。

```xml
<!-- 使用相对于类路径的资源引用 -->
<mappers>
 <mapper resource="org/mybatis/builder/PostMapper.xml"/>
</mappers>
```

```xml
<!-- 使用完全限定资源定位符（URL） -->
<mappers>
 <mapper url="file:///var/mappers/AuthorMapper.xml"/>
</mappers>
```

```xml
<!--
使用映射器接口实现类的完全限定类名
需要配置文件名称和接口名称一致，并且位于同一目录下
-->
<mappers>
 <mapper class="org.mybatis.builder.AuthorMapper"/>
</mappers>
```

~~~xml
<!--
将包内的映射器接口实现全部注册为映射器
但是需要配置文件名称和接口名称一致，并且位于同一目录下
-->
<mappers>
 <package name="org.mybatis.builder"/>
</mappers>
~~~

## Mapper.xml文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="xyz.rtx3090.dao.UserMapper">
    <!--查询全部用户-->
    <select id="getAllUser" resultType="User">
        select * from mybatis.user
    </select>

    <!--根据id查询指定用户-->
    <select id="getUserById" resultType="User" parameterType="int">
        select * from mybatis.user where id = #{id}
    </select>

    <!--根据name进行模糊查询-->
    <select id="getUserLikeName" parameterType="String" resultType="User">
        select * from mybatis.user where name like #{name}
    </select>

    <!--添加指定用户-->
    <insert id="addUser" parameterType="User">
        insert into mybatis.user (id, name, pwd) VALUES (#{id},#{name},#{pwd})
    </insert>

    <!--更改指定用户-->
    <update id="updateUser" parameterType="map">
        update mybatis.user set name = #{userName}, pwd = #{userPwd} where id = #{userId}
    </update>

    <!--删除指定用户-->
    <delete id="deleteUser" parameterType="int">
        delete from mybatis.user where id = #{id}
    </delete>
</mapper>
```

> + namespace的命名必须跟某个接口同名
> + namespace命名规则 : 包名+类名
> + namespace和子元素的id联合保证唯一  , 区别不同的mapper
> + 作用为绑定DAO接口
> + 接口中的方法与映射文件中sql语句id应该一一对应

## 其他配置（浏览即可）

### settings——设置

~~~xml
    <settings>
        <setting name="cacheEnabled" value="true"/>
        <setting name="lazyLoadingEnabled" value="true"/>
        <setting name="multipleResultSetsEnabled" value="true"/>
        <setting name="useColumnLabel" value="true"/>
        <setting name="useGeneratedKeys" value="false"/>
        <setting name="autoMappingBehavior" value="PARTIAL"/>
        <setting name="autoMappingUnknownColumnBehavior" value="WARNING"/>
        <setting name="defaultExecutorType" value="SIMPLE"/>
        <setting name="defaultStatementTimeout" value="25"/>
        <setting name="defaultFetchSize" value="100"/>
        <setting name="safeRowBoundsEnabled" value="false"/>
        <setting name="mapUnderscoreToCamelCase" value="false"/>
        <setting name="localCacheScope" value="SESSION"/>
        <setting name="jdbcTypeForNull" value="OTHER"/>
        <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
    </settings>

### typeHandlers——类型处理器

​~~~xml
    <typeHandlers>
        <typeHandler handler="org.mybatis.example.ExampleTypeHandler"/>
    </typeHandlers>
~~~

+ 无论是 MyBatis 在预处理语句（PreparedStatement）中设置一个参数时，还是从结果集中取出一个值时， 都会用类型处理器将获取的值以合适的方式转换成 Java 类型。
+ 你可以重写类型处理器或创建你自己的类型处理器来处理不支持的或非标准的类型。【了解即可】

### objectFactory——对象工厂

~~~xml
    <objectFactory type="org.mybatis.example.ExampleObjectFactory">
        <property name="someProperty" value="100"/>
    </objectFactory>
~~~

+ MyBatis 每次创建结果对象的新实例时，它都会使用一个对象工厂（ObjectFactory）实例来完成。
+ 默认的对象工厂需要做的仅仅是实例化目标类，要么通过默认构造方法，要么在参数映射存在的时候通过有参构造方法来实例化。
+ 如果想覆盖对象工厂的默认行为，则可以通过创建自己的对象工厂来实现。【了解即可】

# mybatis生命周期和作用域

理解我们目前已经讨论过的不同作用域和生命周期类是至关重要的，因为错误的使用会导致非常严重的并发问题。

我们可以先画一个流程图，分析一下Mybatis的执行过程

![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7JdnS939HH5TayIhQo5s0aJbReBExSQO1U23XeLAXlhTWUeL87mJZL0lDzPstpY3CSIwvW0dN9ccA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## SqlSessionFactoryBuilder

这个类可以被实例化、使用和丢弃，一旦创建了 SqlSessionFactory，就不再需要它了。 因此 SqlSessionFactoryBuilder 实例的最佳作用域是方法作用域（也就是局部方法变量）。 你可以重用 SqlSessionFactoryBuilder 来创建多个 SqlSessionFactory 实例，但最好还是不要一直保留着它，以保证所有的 XML 解析资源可以被释放给更重要的事情。

## SqlSessionFactory

SqlSessionFactory 一旦被创建就应该在应用的运行期间一直存在，没有任何理由丢弃它或重新创建另一个实例。 使用 SqlSessionFactory 的最佳实践是在应用运行期间不要重复创建多次，多次重建 SqlSessionFactory 被视为一种代码“坏习惯”。因此 SqlSessionFactory 的最佳作用域是应用作用域。 有很多方法可以做到，最简单的就是使用单例模式或者静态单例模式。

## Session

每个线程都应该有它自己的 SqlSession 实例。SqlSession 的实例不是线程安全的，因此是不能被共享的，所以它的最佳的作用域是请求或方法作用域。 绝对不能将 SqlSession 实例的引用放在一个类的静态域，甚至一个类的实例变量也不行。 也绝不能将 SqlSession 实例的引用放在任何类型的托管作用域中，比如 Servlet 框架中的 HttpSession。 如果你现在正在使用一种 Web 框架，考虑将 SqlSession 放在一个和 HTTP 请求相似的作用域中。 换句话说，每次收到 HTTP 请求，就可以打开一个 SqlSession，返回一个响应后，就关闭它。 这个关闭操作很重要，为了确保每次都能执行关闭操作，你应该把这个关闭操作放到 finally 块中。 下面的示例就是一个确保 SqlSession 关闭的标准模式：

```java
try (SqlSession session = sqlSessionFactory.openSession()) {
  // 你的应用逻辑代码
}
```

在所有代码中都遵循这种使用模式，可以保证所有数据库资源都能被正确地关闭。

## 映射器实例

映射器是一些绑定映射语句的接口。映射器接口的实例是从 SqlSession 中获得的。虽然从技术层面上来讲，任何映射器实例的最大作用域与请求它们的 SqlSession 相同。但方法作用域才是映射器实例的最合适的作用域。 也就是说，映射器实例应该在调用它们的方法中被获取，使用完毕之后即可丢弃。 映射器实例并不需要被显式地关闭。尽管在整个请求作用域保留映射器实例不会有什么问题，但是你很快会发现，在这个作用域上管理太多像 SqlSession 的资源会让你忙不过来。 因此，最好将映射器放在方法作用域内。就像下面的例子一样：

```java
try (SqlSession session = sqlSessionFactory.openSession()) {
  BlogMapper mapper = session.getMapper(BlogMapper.class);
  // 你的应用逻辑代码
}
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7JdnS939HH5TayIhQo5s0aJJq1YuJCr3e9PsTBpBgc1tbicoshHB3qLkwgn3Jp2q8qI1dY9vGhIia3w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

> 学会了Crud，和基本的配置及原理，后面就可以学习些业务开发

# ResultMap——结果映射

为了更好的讲述ResultMap，我们首先引述一个问题：**数据库字段名与实体类属性名不一致，导致查询为NULL的问题**

例如，我们数据库中的字段名为id, name, pwd，而我们JavaBen对象的实体类对应的实体类属性名为id, name, password。

## 问题准备

**数据库**

~~~mysql
CREATE TABLE `user` (
  `id` int(10) NOT NULL,
  `name` varchar(20) DEFAULT NULL,
  `pwd` varchar(50) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
~~~

**JavaBen实体类**

~~~java
public class User {
    private int id;
    private String name;
    private String password;
    //constructor
    //setter and getter
		//toString
  	//equals and hashCode
}
~~~

**Mapper接口**

~~~java
//根据id查询用户
User selectUserById(int id);
~~~

**Mapper映射文件**

~~~xml
<select id="selectUserById" resultType="user">
  select * from user where id = #{id}
</select>
~~~

**测试类**

~~~java
@Test
public void testSelectUserById() {
   SqlSession session = MybatisUtils.getSession();  //获取SqlSession连接
   UserMapper mapper = session.getMapper(UserMapper.class);
   User user = mapper.selectUserById(1);//查询Id为1的用户所有信息
   System.out.println(user);
   session.close();//关闭SqlSession连接
}
~~~

**测试结果**

~~~
User{id=1, name='Jason', password='null'}
~~~

查询出来发现 password 为空 . 说明出现了问题！

## 问题分析

我们在Mapper映射文件中，写的SQL语句：

`select * from user where id = #{id}`相当于`select id,name,pwd from user where id = #{id}`

而mybatis会根据这些查询的列名(会将列名转化为小写,数据库不区分大小写) , 去对应的实体类中查找相应列名的set方法设值 , 由于找不到setPwd() , 所以password返回null ; 【自动映射】

## 解决方案

### 方案一

在Mapper映射文件中的书写的SQL语句中，为其列名指定别名 , 其别名和java实体类的属性名一致 。

~~~xml
<mapper namespace="xyz.rtx3090.dao.UserMapper">
    <!--根据id查询指定用户-->
    <select id="selectUserById" resultType="user" parameterType="int">
        select id, name, pwd as password from mybatis.user where id = #{id}
    </select>
</mapper>
~~~

### 方案二(更推荐)

除了上面这个很笨的方法，我们还可以使用`ResultMap`结果集映映射。将数据库列名一一映射到实体类的属性名上。

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="xyz.rtx3090.dao.UserMapper">
  <!--这里的id值与下面select标签resultMap属性对应，type值与实体类名称对应-->
    <resultMap id="one" type="user">
        <!--id标签为主键的映射关系，column是数据库表的列名 , property是对应实体类的属性名 -->
        <id column="id" property="id" />
        <!--result标签为其他健的映射关系，column是数据库表的列名 , property是对应实体类的属性名 -->
        <result column="name" property="name"/>
        <result column="pwd" property="password"/>
    </resultMap>
  
    <!--根据id查询指定用户-->
    <select id="selectUserById" resultMap="one" parameterType="int">
        select * from mybatis.user where id = #{id}
    </select>
</mapper>
~~~

# 日志

思考：我们在测试SQL的时候，要是能够在控制台输出 SQL 的话，是不是就能够有更快的排错效率？

如果一个数据库相关的操作出现了问题，我们可以根据输出的SQL语句快速排查问题。

对于以往的开发过程，我们会经常使用到debug模式来调节，跟踪我们的代码执行过程。但是现在使用Mybatis是基于接口，配置文件的源代码执行过程。因此，我们必须选择日志工具来作为我们开发，调节程序的工具。

Mybatis内置的日志工厂提供日志功能，具体的日志实现有以下几种工具：

+  SLF4J 
+ LOG4J 
+ LOG4J2 
+ JDK_LOGGING
+ COMMONS_LOGGING
+ STDOUT_LOGGING
+ NO_LOGGING

具体选择哪个日志实现工具由MyBatis的内置日志工厂确定。它会使用最先找到的（按上文列举的顺序查找）。如果一个都未找到，日志功能就会被禁用。

## 标准日志实现

首先我们可以在`mybatis-config.xml`文件中设置我们使用标准日志实现。

~~~xml
<settings>
       <setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
~~~

测试，可以看到控制台有大量的输出！我们可以通过这些输出来判断程序到底哪里出了Bug

## Log4j日志实现

### 概述

+ Log4j是Apache的一个开源项目
+ 通过使用Log4j，我们可以控制日志信息输送的目的地：控制台，文本，GUI组件....
+ 我们也可以控制每一条日志的输出格式
+ 通过定义每一条日志信息的级别，我们能够更加细致地控制日志的生成过程。最令人感兴趣的就是，这些可以通过一个配置文件来灵活地进行配置，而不需要修改应用的代码。

### 使用

1. 在`pom.xml`中配置 `log4j`的jar包

   ~~~xml
           <!-- https://mvnrepository.com/artifact/log4j/log4j -->
           <dependency>
               <groupId>log4j</groupId>
               <artifactId>log4j</artifactId>
               <version>1.2.17</version>
           </dependency>
   ~~~

2. 在`resources`目录下，新建`log4j.properties`配置文件

   ~~~properties
   #将等级为DEBUG的日志信息输出到console和file这两个目的地，console和file的定义在下面的代码
   log4j.rootLogger=DEBUG,console,file
   
   #控制台输出的相关设置
   log4j.appender.console = org.apache.log4j.ConsoleAppender
   log4j.appender.console.Target = System.out
   log4j.appender.console.Threshold=DEBUG
   log4j.appender.console.layout = org.apache.log4j.PatternLayout
   log4j.appender.console.layout.ConversionPattern=[%c]-%m%n
   
   #文件输出的相关设置
   log4j.appender.file = org.apache.log4j.RollingFileAppender
   log4j.appender.file.File=./log/bernardo.log
   log4j.appender.file.MaxFileSize=10mb
   log4j.appender.file.Threshold=DEBUG
   log4j.appender.file.layout=org.apache.log4j.PatternLayout
   log4j.appender.file.layout.ConversionPattern=[%p][%d{yy-MM-dd}][%c]%m%n
   
   #日志输出级别
   log4j.logger.org.mybatis=DEBUG
   log4j.logger.java.sql=DEBUG
   log4j.logger.java.sql.Statement=DEBUG
   log4j.logger.java.sql.ResultSet=DEBUG
   log4j.logger.java.sql.PreparedStatement=DEBUG
   ~~~

3. 在`mybatis-config.xml`配置日志实现为LOG4J

   ~~~xml
       <settings>
           <setting name="logImpl" value="LOG4J"/>
       </settings>
   ~~~

4. 在Java程序使用Log4j进行日志输出

   ~~~java
   public class UserMapperTest {
   		//注意导包为：org.apache.log4j.Logger
       static Logger logger = Logger.getLogger(UserMapperTest.class);
   
       @Test
       public void testLog4j() {
           logger.info("info:进入了testLog4j");
           logger.debug("debug:进入了testLog4j");
           logger.error("error:进入了testLog4j");
       }
   }
   ~~~

5. 测试结果，我们可以看过控制台和日志文件都有日志输出。

# 分页功能实现

在学习mybatis等持久层框架的时候，会经常对数据进行增删改查操作，使用最多的是对数据库进行查询操作，如果查询大量数据的时候，我们往往使用分页进行查询，也就是每次处理小部分数据，这样对数据库压力就在可控范围内。

## 分页方案一

结合SQL语句使用limit实现分页

**在Mapper接口类中添加实现分页功能的方法**

~~~java
public interface UserMapper {
    //选择全部用户实现分页
    List<User> selectUserLimit(Map<String, Integer> map);
}
~~~

**修改Mapper.xml文件中SQL语句，加入limit关键字**

~~~xml
<mapper namespace="xyz.rtx3090.dao.UserMapper">
    <resultMap id="four" type="user">
        <!--id标签为主键的映射关系，column是数据库表的列名 , property是对应实体类的属性名 -->
        <id column="id" property="id"/>
        <!--result标签为其他健的映射关系，column是数据库表的列名 , property是对应实体类的属性名 -->
        <result column="name" property="name"/>
        <result column="pwd" property="password"/>
    </resultMap>

    <!--选择全部用户实现分页-->
    <select id="selectUserLimit" parameterType="map" resultMap="four">
        select * from mybatis.user limit #{startIndex}, #{pageSize}
    </select>
</mapper>
~~~

**在测试类中传入测试参数**

~~~java
public class UserMapperTest {
    @Test
    public void testSelectUserLimit() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        try{
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            Map<String, Integer> map = new HashMap<String, Integer>();
            map.put("startIndex",0);
            map.put("pageSize",2);
            List<User> userList = mapper.selectUserLimit(map);
            for (User user: userList
                 ) {
                System.out.println(user);
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            sqlSession.close();
        }
    }
}
~~~

> 查询结果：
>
> ~~~mysql
> User{id=1, name='one', pwd='111111'}
> User{id=3, name='third', pwd='303030'}
> ~~~
>
> 实现分页功能成功！

## 分页方案二

我们除了使用Limit在SQL层面实现分页，也可以使用RowBounds在Java代码层面实现分页，当然此种方式作为了解即可。我们来看下如何实现的！

**在Mapper接口类中添加实现分页功能的方法**

```java
public interface UserMapper {
    //选择全部用户RowBounds实现分页
    List<User> getUserByRowBounds();
}
```

**Mapper.xml文件不需要加入Limit关键字**

~~~xml
<mapper namespace="xyz.rtx3090.dao.UserMapper">
    <!--选择全部用户RowBounds实现分页-->
    <select id="getUserByRowBounds" resultMap="five">
        select * from mybatis.user
    </select>
</mapper>
~~~

**在测试类中使用RowBounds类，在Java代码层次进行分页功能**

~~~java
public class UserMapperTest {
    @Test
    public void testGetUserByRowBounds() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        try{
            int startPage = 2;//起始页面
            int pageSize = 2;//每页数据量
            //传入参数创建RowBounds对象
            RowBounds rowBounds = new RowBounds((startPage - 1) * pageSize, pageSize);
            //通过session.**方法进行传递rowBounds，[此种方式现在已经不推荐使用了]
            List<User> userList = sqlSession.selectList("xyz.rtx3090.dao.UserMapper.getUserByRowBounds",null,rowBounds);
            for (User user: userList
                 ) {
                System.out.println(user);
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            sqlSession.close();
        }
    }
}
~~~

> 测试结果：
>
> ```mysql
> User{id=4, name='four', pwd='444444'}
> User{id=5, name='five', pwd='555555'}
> ```
>
> 实现分页功能成功！

## 分页方案三

如果觉得上述两种方法都太快麻烦，我们还可以采用第三种更为简单的方式，使用分页插件PageHelper来帮助我们实现分页功能。

官方文档：https://pagehelper.github.io/

# 面向接口编程

- 大家之前都学过面向对象编程，也学习过接口，但在真正的开发中，很多时候我们会选择面向接口编程
- **根本原因 :  解耦 , 可拓展 , 提高复用 , 分层开发中 , 上层不用管具体的实现 , 大家都遵守共同的标准 , 使得开发变得容易 , 规范性更好**
- 在一个面向对象的系统中，系统的各种功能是由许许多多的不同对象协作完成的。在这种情况下，各个对象内部是如何实现自己的,对系统设计人员来讲就不那么重要了；
- 而各个对象之间的协作关系则成为系统设计的关键。小到不同类之间的通信，大到各模块之间的交互，在系统设计之初都是要着重考虑的，这也是系统设计的主要工作内容。面向接口编程就是指按照这种思想来编程。

## 关于接口的理解

+ 接口从更深层次的理解，应是定义（规范，约束）与实现（名实分离的原则）的分离。

+ 接口的本身反映了系统设计人员对系统的抽象理解。

+ 接口应有两类：

+ - 第一类是对一个个体的抽象，它可对应为一个抽象体(abstract class)；
  - 第二类是对一个个体某一方面的抽象，即形成一个抽象面（interface）；

+ 一个体有可能有多个抽象面。抽象体与抽象面是有区别的。

## 三个面向的区别

+ 面向过程是指，我们考虑问题时，以一个具体的流程（事务过程）为单位，考虑它的实现 .
+ 面向对象是指，我们考虑问题时，以对象为单位，考虑它的属性及方法 .
+ 接口设计与非接口设计是针对复用技术而言的，与面向对象（过程）不是一个问题.更多的体现就是对系统整体的架构

# 利用注解开发

**mybatis最初配置信息是基于 XML ,映射语句(SQL)也是定义在 XML 中的。而到MyBatis 3提供了新的基于注解的配置。不幸的是，Java 注解的的表达力和灵活性十分有限。最强大的 MyBatis 映射并不能用注解来构建**

**SQL注解开发主要分为四类：**

+ @select()
+ @update()
+ @Insert()
+ @delete()

> 注意：利用注解开发就不需要mapper.xml映射文件了

## 使用步骤

### 在Mapper.java接口的对应方法上添加注解

~~~java
public interface UserMapper {
    //查询全部用户信息（使用注解开发，但是无法只适合简单的SQL）
    @Select("select * from mybatis.user")
    List<User> selectAllUser();

    //根据Id查询指定用户信息
    @Select("select * from mybatis.user where id = #{id}")
    User selectUserById(@Param("id") int id);

    //根据name进行模糊查询
    @Select("select * from mybatis.user where name like #{name}")
    List<User> selectUserLikeName(@Param("name") String name);

    //添加单个用户信息（直接传入对象）
    @Insert("insert into mybatis.user (id, name, pwd) values (#{id},#{name},#{password})")
    int addUserSingle(User user);

    //更改单个用户信息（先将对象封装进map，然后再传入）
    @Update("update mybatis.user set name = #{name}, pwd = #{password} where id = #{id}")
    int updateUser(Map<String, Object> userMap);

    //删除单个用户信息
    @Delete("delete from mybatis.user where id = #{id}")
    int deleteUser(@Param("id") int id);

    //使用Limit实现分页功能
    @Select("select * from mybatis.user limit #{startPage}, #{pageSize}")
    List<User> selectUserLimit(Map<String, Integer> num);

    //使用RowBounds实现分页功能
    @Select("select * from mybatis.user")
    List<User> selectUserRowBounds(Map<String, Integer> num);
}
~~~

### 在mybatis-config.xml核心配置文件中绑定接口

~~~xml
<configuration>
    <!--映射接口-->
    <mappers>
        <mapper class="xyz.rtx3090.mapper.UserMapper"/>
    </mappers>
</configuration>
~~~

### 在测试类中进行测试

~~~java
public class UserMapperTest {
    @Test
    public void testSelectAllUser() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        try{
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            List<User> userList = mapper.selectAllUser();
            for (User user: userList
                 ) {
                System.out.println(user);
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            sqlSession.close();
        }
    }
    @Test
    public void testSelectUserById() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        try{
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            int id = 2;
            User user = mapper.selectUserById(id);
            System.out.println(user);
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            sqlSession.close();
        }
    }
    @Test
    public void testSelectUserLikeName() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        try{
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            String likeName = "%o%";
            List<User> userList = mapper.selectUserLikeName(likeName);
            for (User user: userList
                 ) {
                System.out.println(user);
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            sqlSession.close();
        }
    }
    @Test
    public void testAddUserSingle() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        try{
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            User two = new User(9, "nine", "999999");
            int i = mapper.addUserSingle(two);
            System.out.println(i == 1 ? "添加成功":"添加失败");
            sqlSession.commit();
        } catch (Exception e){
            e.printStackTrace();
        } finally {
            sqlSession.close();
        }
    }
    @Test
    public void testUpdateUser() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        try{
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            Map<String, Object> userMap = new HashMap<>();
            userMap.put("id",9);
            userMap.put("name","nine");
            userMap.put("password","090909");
            int i = mapper.updateUser(userMap);
            System.out.println(i == 1 ? "修改成功":"修改失败");
            sqlSession.commit();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            sqlSession.close();
        }
    }
    @Test
    public void testDeleteUser() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        try{
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            int id = 9;
            int i = mapper.deleteUser(id);
            System.out.println(i == 1 ? "删除成功":"删除失败");
            sqlSession.commit();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            sqlSession.close();
        }
    }
    @Test
    public void testSelectUserLimit() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        try{
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            Map<String, Integer> limitMap = new HashMap<>();
            limitMap.put("startPage",2);
            limitMap.put("pageSize",3);
            List<User> userList = mapper.selectUserLimit(limitMap);
            for (User user: userList
                 ) {
                System.out.println(user);
            }
        } catch (Exception e){
            e.printStackTrace();
        } finally {
            sqlSession.close();
        }
    }
    @Test
    public void testSelectUserRowBounds() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        try{
            int startPage = 0;//起始页面
            int pageSize = 3;//页面数据量
            //传入参数创建RowBounds对象
            RowBounds rowBounds = new RowBounds((startPage - 1) * pageSize, pageSize);
            //通过session.**方法进行传递rowBounds，[此种方式现在已经不推荐使用了]
            List<User> userList = sqlSession.selectList("xyz.rtx3090.mapper.UserMapper.selectUserRowBounds",null,rowBounds);
            for (User user: userList
                 ) {
                System.out.println(user);
            }
        } catch (Exception e){
            e.printStackTrace();
        } finally {
            sqlSession.close();
        }
    }
}
~~~



## 关于@Param参数注解

@Param注解用于给方法参数起一个名字。以下是总结的使用原则：

+ 在方法只接受一个参数的情况下，可以不使用@Param（推荐还是写上）。
+ 在方法接受多个参数的情况下，建议一定要使用@Param注解给参数命名。
+ 如果参数是 JavaBean ， 则不能使用@Param。
+ 不使用@Param注解时，参数只能有一个，并且是Javabean。

## 注解开发本质

**利用Debug查看注解开发的本质**

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210518221737.png)

**发现注解开发本质上使用了jvm的动态代理机制**

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210518221837.png)

**mybatis详细执行流程**

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210518222130.png)

> 使用注解和配置文件协同开发，才是MyBatis的最佳实践！

# #号与$号的区别

+ #{} 的作用主要是替换预编译语句(PrepareStatement)中的占位符? 【推荐使用】

  ~~~mysql
  INSERT INTO user (name) VALUES (#{name});
  # 相当于
  INSERT INTO user (name) VALUES (?);
  ~~~

+ ${} 的作用是直接进行字符串替换（会有sql注入的问题）

  ~~~mysql
  INSERT INTO user (name) VALUES ('${name}');
  # 相当于
  INSERT INTO user (name) VALUES ('kuangshen');
  ~~~

# 开始事务自动提交（不推荐开启）

想要开启事务的自动提交，我们只需要在我们用来获取SqlSession对象的方法中，传入true为参数即可.

~~~java
    public static SqlSession getSqlSession() {
        //传入true为参数，表示开启事务自动提交
        return sqlSessionFactory.openSession(true);
    }
~~~

# Lombok插件

## 简介

Lombok插件是一个java库，一个构建工具。通过简单的注解来实现精简代码，消除冗长代码和提高开发效率的目的。

## 为啥要使用Lombok

大家在写bug的时候，肯定和很多的实体打过交道，然后我们要写getter()、setter()、toString()等等。

不，我们不用写，不管是`idea`还是`eclipse`都有快捷键给我们生成getter()、setter()。所以我们还是能很快的开发。

可是还是有下面的一些问题：

- 众多的`getter()`、`setter()`，占据整个类，影响代码的可读性。如果我们开发将字段穿插在 `getter()`、`setter()`之间呢？
- 我们新增一个字段，那么我们就要新增对应的 `getter()`、`setter()`，修改 `toString()` 等等；
- 我们删除一个字段，要把对应的getter()、setter()删除，修改toString()等等；
- 我们修改一个字段的类型，比如 `Integer` 修改成 `String`。那我们要删除 `getter()`、`setter()` 先，然后生成`getter()`、`setter()`，再修改 `toString()`等等。
- 如果一个实体有很多的属性字段，那么修改其中一个，定位不容易。
- 如果我们想用流式风格创建对象，又要写一堆代码。
- 如果我们想生成带所有参数的构造器，又如何？

Lombok能解决上面所有的问题，只需要几个注解即可。

## Lombok安装

1. 在Idea中安装Lombok插件

   `File` → `Settings...` → `Plugins` → `Browse repositories` → 搜索`Lombok` → 安装插件 。

   安装完毕，重启idea，那我们就安装好`Lombok`插件了

2. 在maven项目的pom.xml中导入Lombok相关的jar配置

   ~~~xml
   <dependency>
       <groupId>org.projectlombok</groupId>
       <artifactId>lombok</artifactId>
       <version>1.18.2</version>
   </dependency>
   ~~~

3. 在我们的JavaBen实体类上加入对应注解

   ~~~java
   @Data//无参构造、getter and setter、toString、hashCode、equals
   @AllArgsConstructor //有参构造（添加这个上面的无参构造就会消失）
   @NoArgsConstructor //无参构造（所以需要补充这个无参构造）
   public class User {
       private int id;
       private String name;
       private String password;
   }
   ~~~

## 常用注解

### @Data

这个注解会帮我们生成无参构造、getter、setter、equals、hashCode、toString等方法。可以说很强大了。

### @Getter、@Setter

这两个注解就是分别生成我们的getter和setter。可以放在类或者属性上

- 放在类上，所有属性都生成getter和setter
- 放在属性上，只有对应的属性生成getter和setter

### @ToString

在类上使用，生成toString()方法。

- @ToString(of = {"id", "name"})
   指定包含的属性
- @ToString(exclude = {"id", "name"})
   指定排除的属性
- @ToString(callSuper = true)
   输出父类的属性

### @EqualsAndHashCode

在类上使用，生成equals和hashcode方法

### @Slf4j

我们开发的时候，很多时候都是要记录日志的，便于以后的问题排查。经常要写类似于下面的代码。

~~~java
private static final Logger log = LoggerFactory.getLogger(User.class);
~~~

@Slf4j帮我们自动生成这行代码。在类上使用

### @NoArgsConstructor

在类上使用，生成无参构造器

### @AllArgsConstructor

在类上使用，生成带所有属性的构造器，不生成默认的无参构造器

### @Builder

用于类上，生成流式API。当我们创建新的对象时，会使我们的代码更优雅。

~~~java
User user = new User();
user.setId(1L);
user.setName("roach");
user.setGender(1);
user.setBirthday(new Date());
user.setAge(18);

// 流式方式设置属性值
User user1 = User.builder()
        .id(1L)
        .name("roach")
        .gender(1)
        .birthday(new Date())
        .age(18)
    .build();
~~~

# 多对一的处理

## 多对一的理解

+ 多个学生对应一个老师
+ 对于学生这边，就是一个多对一的现象，即从学生这边关联一个老师！

## 多对一查询环境准备

**首先创建一个没有模版的Maven项目**

**1. 配置`pom.xml`文件**(我创建的是子项目，所以贴出其父项目的配置文件)

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>xyz.rtx3090</groupId>
    <artifactId>mybatis</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    <modules>
        <module>mybatis01</module>
        <module>mybatis02</module>
        <module>mybatis03</module>
    </modules>

    <dependencies>
        <!--mysql驱动-->
        <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.46</version>
        </dependency>
        <!--mybatis-->
        <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.2</version>
        </dependency>
        <!--junit-->
        <!-- https://mvnrepository.com/artifact/junit/junit -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <!-- https://mvnrepository.com/artifact/log4j/log4j -->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
    </dependencies>

    <!-- 在build中配置resources,来防止我们资源导出失败的问题-->
    <build>
        <plugins>
            <!--规定项目JDK版本-->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.0</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
        </plugins>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
        </resources>
    </build>
</project>
~~~

**2. 在`main->Java`目录下创建三个目录`xyz.rtx3090.mapper`、`xyz.rtx3090.pojo`和`xyz.rtx3090.utils`**

**3. 在`resource`目录下创建一个目录`xyz.rtx3090.mapper`**

**4. 在`text->java`目录下创建一个目录`xyz.rtx3090.mapper`**

**5. 在`java`目录下创建`xyz.rtx3090.utils.MybatisUtils`工具类**

~~~java
package xyz.rtx3090.utils;
import ...

public class MybatisUtils {
    private static SqlSessionFactory sqlSessionFactory;
    //在静态代码块中自动获取sqlSessionFactory
    static {
        String resource = "mybatis-config.xml";
        InputStream inputStream = null;
        try {
            inputStream = Resources.getResourceAsStream(resource);
        } catch (IOException e) {
            e.printStackTrace();
        }
        sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
    }
    //创建一个获取sqlSession对象的静态工具类方法
    public static SqlSession getSqlSession() {
        return sqlSessionFactory.openSession();
    }
}
~~~

**6. 在`java`目录下创建`xyz.rtx3090.pojo.Student`和`xyz.rtx3090.pojo.Teacher`两个JavaBen实体类**

~~~java
package xyz.rtx3090.pojo;
import java.util.Objects;

public class Student {
    private int id;
    private String name;
    //学生需要关联一个老师
    private Teacher teacher;
    //constructor
    //setter and getter
		//equals and hashCode
    //hashCode
		//toString
}
~~~

~~~java
package xyz.rtx3090.pojo;
import java.util.Objects;
public class Teacher {
    private int id;
    private String name;

    //constructor
    //setter and getter
		//equals and hashCode
    //hashCode
		//toString
}

~~~

**7. 在`resource`目录下创建`xyz.rtx3090.mapper.StudentMapper.xml`和`xyz.rtx3090.mapper.TeacherMapper.xml`配置文件**

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="xyz.rtx3090.mapper.StudentMapper">
	
</mapper>
~~~

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="xyz.rtx3090.mapper.TeacherMapper">

</mapper>
~~~

**8. 在`resource`目录下创建`db.properties`、`log4j.properties`和`mybatis-config.xml`配置文件**(下面对应其顺序)

~~~properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis?useSSL=false&useUnicode=true&characterEncoding=UTF-8
~~~

~~~properties
#将等级为DEBUG的日志信息输出到console和file这两个目的地，console和file的定义在下面的代码
log4j.rootLogger=DEBUG,console,file

#控制台输出的相关设置
log4j.appender.console = org.apache.log4j.ConsoleAppender
log4j.appender.console.Target = System.out
log4j.appender.console.Threshold=DEBUG
log4j.appender.console.layout = org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=[%c]-%m%n

#文件输出的相关设置
log4j.appender.file = org.apache.log4j.RollingFileAppender
log4j.appender.file.File=./log/bernardo.log
log4j.appender.file.MaxFileSize=10mb
log4j.appender.file.Threshold=DEBUG
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=[%p][%d{yy-MM-dd}][%c]%m%n

#日志输出级别
log4j.logger.org.mybatis=DEBUG
log4j.logger.java.sql=DEBUG
log4j.logger.java.sql.Statement=DEBUG
log4j.logger.java.sql.ResultSet=DEBUG
log4j.logger.java.sql.PreparedStatement=DEBUG
~~~

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--属性-->
    <properties resource="db.properties">
        <property name="username" value="root"/>
        <property name="password" value="intmain()"/>
    </properties>

    <!--设置-->
    <settings>
        <!--设置日志输出为log4j-->
        <setting name="logImpl" value="LOG4J"/>
    </settings>

    <!--类型别名-->
    <typeAliases>
        <package name="xyz.rtx3090.pojo"/>
    </typeAliases>

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>

    <!--映射mapper.xml文件-->
    <mappers>
        <mapper resource="xyz/rtx3090/mapper/StudentMapper.xml"/>
        <mapper resource="xyz/rtx3090/mapper/TeacherMapper.xml"/>
    </mappers>
</configuration>
~~~

> 注意要在mybatis-config.xml中增加StudentMapper.xml和TeacherMapper.xml对应的映射

**9. 在`test->java`目录下创建`xyz.rtx3090.mapper.StudentMapper`和`xyz.rtx3090.mapper.TeacherMapper`测试类**

~~~java
package xyz.rtx3090.mapper;
import ...

public class StudentMapperTest {
  
}
~~~

~~~java
package xyz.rtx3090.mapper;
import ...

public class TeacherMapperTest {
	
}
~~~



## 多对一处理步骤

### 方案一：按查询嵌套处理

1. **在StudentMapper接口中增加对应方法**

   ~~~java
   public interface StudentMapper {
       //查询全部学生及对应老师信息
       List<Student> selectAllStudent();
   }
   ~~~

2. **编写对应的StudentMapper.xml配置文件【重点】**

   ~~~xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="xyz.rtx3090.mapper.StudentMapper">
       <select id="selectAllStudent" resultMap="one">
           select * from mybatis.student;
       </select>
       <resultMap id="one" type="student">
           <result column="id" property="id"/>
           <result column="name" property="name"/>
         <!--association关联属性 property属性名 javaType属性类型 column在多的一方的表中的列名-->
           <association property="teacher" column="tid" javaType="Teacher" select="selectTeacherById"/>
       </resultMap>
       
       <select id="selectTeacherById" resultType="teacher">
         <!--这里占位符中的名称可以随意写-->
           select * from mybatis.teacher where id = #{tid};
       </select>
   </mapper>
   ~~~

3. **在StudentMapperTest.java中进行测试**

   ~~~java
   package xyz.rtx3090.mapper;
   import ...
   
   public class StudentMapperTest {
       //查询所有学生
       @Test
       public void testSelectAllStudent(){
           SqlSession sqlSession = MybatisUtils.getSqlSession();
           try{
               StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
               List<Student> students = mapper.selectAllStudent();
               for (Student student: students
                    ) {
                   System.out.println(
                           "学生: " + student.getName()
                               + "\t老师: " + student.getTeacher().getName()
                   );
               }
           } catch (Exception e){
               e.printStackTrace();
           } finally {
               sqlSession.close();
           }
       }
   }
   ~~~

   > 测试结果：
   >
   > ~~~
   > 学生: 小明	老师: 秦老师
   > 学生: 小红	老师: 秦老师
   > 学生: 小张	老师: 秦老师
   > 学生: 小李	老师: 秦老师
   > 学生: 小王	老师: 秦老师
   > ~~~
   >
   > 多对一查询成功！

### 方案二：按结果嵌套处理

1. **在StudentMapper接口中增加对应方法**

   ~~~java
   public interface StudentMapper {
       //按结果嵌套：查询全部学生及对应老师信息
       List<Student> getAllStudent();
   }
   ~~~

2. **编写对应的StudentMapper.xml配置文件【重点】**

   ~~~xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="xyz.rtx3090.mapper.StudentMapper">
       <select id="getAllStudent" resultMap="two">
           select s.id sid, s.name sname , t.name tname
           from student s,teacher t
           where s.tid = t.id
       </select>
       <resultMap id="two" type="student">
           <id property="id" column="sid"/>
           <result column="sname" property="name"/>
         <!--重点，难点-->
           <association property="teacher" javaType="Teacher">
               <result property="name" column="tname"/>
           </association>
       </resultMap>
   </mapper>
   ~~~

3. **在StudentMapperTest.java中进行测试**

   ~~~java
   package xyz.rtx3090.mapper;
   import ...
   
   public class StudentMapperTest {
       //方案二：按结果嵌套查询所以学生
       @Test
       public void testGetAllStudent() {
           SqlSession sqlSession = MybatisUtils.getSqlSession();
           try{
               StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
               List<Student> allStudent = mapper.getAllStudent();
               for (Student student: allStudent
                    ) {
                   System.out.println(
                           "学生：" + student.getName()
                           + "\t老师：" + student.getTeacher().getName()
                   );
               }
           } catch (Exception e) {
               e.printStackTrace();
           } finally {
               sqlSession.close();
           }
       }
   }
   ~~~

   > 小结：
   >
   > 按照查询进行嵌套处理就像SQL中的子查询
   >
   > 按照结果进行嵌套处理就像SQL中的联表查询