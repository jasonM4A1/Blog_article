---
title: 解决JDBC连接MySQL时发出的警告
date: 2021-02-02 05:31:52
tags:
	- JDBC
	- MySQL
	- Java
categories:
	- Java
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210202053256.png
---

# 问题描述

解决JDBC连接MySQL时发出的警告WARN: Establishing SSL connection without server’s identity
verification…

使用C3P0、Druid来连接MySQL数据库的时候，出现了警告一大串的红色警告：

```
Sat Dec 05 19:14:16 GMT+08:00 2020 WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.
Sat Dec 05 19:14:17 GMT+08:00 2020 WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.
Sat Dec 05 19:14:17 GMT+08:00 2020 WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.
Sat Dec 05 19:14:17 GMT+08:00 2020 WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.
Sat Dec 05 19:14:17 GMT+08:00 2020 WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.
```

如图所示：

[![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205192034.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205192034.png)

大概意思是：

不建议在没有服务器身份验证的情况下建立SSL连接。根据MySQL 5.5.45+、5.6.26+和5.7.6+的要求，如果不设置显式选项，则必须建立默认的SSL连接。您需要通过设置useSSL=false显式地禁用SSL，或者设置useSSL=true并为服务器证书验证提供信任存储。

# 解决方法

## 配置文件为XML格式

在配置文件中的`name="jdbcUrl">jdbc:mysql://localhost:3306/mydatabase`后

`?characterEncoding=utf8&useSSL=false`。如下：

[![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201204232210.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201204232210.png)

## 配置文件为properties格式

在配置文件中的`url=jdbc:mysql://127.0.0.1:3306/mydatabase`后加上

`?useUnicode=true&characterEncoding=utf-8&useSSL=false`。如下：

[![img](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205194123.png)](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201205194123.png)