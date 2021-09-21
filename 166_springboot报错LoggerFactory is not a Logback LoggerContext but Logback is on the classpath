# 问题描述
在整合springboot和dubbo时，启动提供者类时，springboot报错"LoggerFactory is not a Logback LoggerContext but Logback is on the classpath"
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210830212717.png)

# 解决方案

修改出错项目的pom文件
```xml
    <!--springboot框架web项目起步依赖-->
    <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!--注册中心-->
    <dependency>
            <groupId>com.101tec</groupId>
            <artifactId>zkclient</artifactId>
            <version>0.10</version>
    </dependency>
```
将上面的内容替换为下面的内容
```xml
    <!--springboot框架web项目起步依赖-->
    <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <exclusions>
                    <exclusion>
                            <groupId>org.slf4j</groupId>
                            <artifactId>slf4j-log4j12</artifactId>
                    </exclusion>
            </exclusions>
    </dependency>
    <!--注册中心-->
    <dependency>
            <groupId>com.101tec</groupId>
            <artifactId>zkclient</artifactId>
            <version>0.10</version>
            <exclusions>
                    <exclusion>
                            <groupId>org.slf4j</groupId>
                            <artifactId>slf4j-log4j12</artifactId>
                    </exclusion>
            </exclusions>
    </dependency>
```
