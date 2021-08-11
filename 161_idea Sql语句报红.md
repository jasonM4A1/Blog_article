idea中Sql语句报红

---

# 问题描述

Jar等驱动都配置了，数据库url、username、passworde也都没问题，sql语句本身也更没有错误，但idea就是报红！

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210727110853.png)

# 解决方案

取消sql方言，设置sql方言为None。设置完重启idea既可

路径 File | Settings | Languages & Frameworks | SQL Dialects

![image-20210727110839129](/Users/bernardo/Library/Application Support/typora-user-images/image-20210727110839129.png)