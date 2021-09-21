<!-- vim-markdown-toc Marked -->

* [NoSQL](#nosql)
    * [数据库应用历程](#数据库应用历程)
    * [什么是NoSQL](#什么是nosql)
    * [NoSQL数据模型](#nosql数据模型)
* [Redis](#redis)
    * [简介](#简介)
    * [特点](#特点)
        * [支持数据持久化](#支持数据持久化)
        * [支持多种数据结构](#支持多种数据结构)
        * [支持数据备份](#支持数据备份)
    * [Redis使用步骤](#redis使用步骤)
    * [Redis的基本命令](#redis的基本命令)
    * [Redis的五种数据结构](#redis的五种数据结构)
        * [字符串类型String](#字符串类型string)
        * [列表类型List](#列表类型list)
        * [集合类型Set](#集合类型set)
        * [哈希类型Hash](#哈希类型hash)
        * [有序集合类型Zset（sorted set）](#有序集合类型zset（sorted-set）)
    * [Redis命令](#redis命令)
        * [关于key的操作命令](#关于key的操作命令)
        * [关于string数据操作命令](#关于string数据操作命令)
        * [关于list数据操作命令](#关于list数据操作命令)
        * [关于set数据操作命令](#关于set数据操作命令)
        * [关于hash数据操作命令](#关于hash数据操作命令)
        * [关于Zset数据操作命令](#关于zset数据操作命令)
    * [Redis配置文件](#redis配置文件)
        * [redis.conf存放位置](#redis.conf存放位置)
        * [Redis网络相关配置](#redis网络相关配置)
        * [Redis常规配置](#redis常规配置)
        * [Redis安全配置](#redis安全配置)
        * [Redis的RDB配置](#redis的rdb配置)
        * [Redis的AOF配置](#redis的aof配置)
    * [Redis的持久化](#redis的持久化)
        * [RDB](#rdb)
            * [RDB原理](#rdb原理)
            * [RDB保存的文件](#rdb保存的文件)
            * [配置RDB持久化策略](#配置rdb持久化策略)
            * [手动保存RDB快照](#手动保存rdb快照)
            * [RDB数据恢复](#rdb数据恢复)
            * [小结](#小结)
        * [AOF](#aof)
            * [AOF原理](#aof原理)
            * [AOF保存的文件](#aof保存的文件)
            * [配置AOF持久化策略](#配置aof持久化策略)
            * [AOF数据恢复](#aof数据恢复)
            * [AOF的重写](#aof的重写)
            * [AOF小结](#aof小结)
    * [Redis事务](#redis事务)
        * [概述](#概述)
        * [Redis事务的常用命令](#redis事务的常用命令)
    * [Redis消息的发布与订阅](#redis消息的发布与订阅)
        * [概述](#概述)
        * [Redis发布订阅示意图](#redis发布订阅示意图)
        * [Redis发布订阅的常用命令](#redis发布订阅的常用命令)
    * [Redis的主从复制](#redis的主从复制)
        * [概述](#概述)
        * [一主二从原理](#一主二从原理)
        * [一主二从搭建](#一主二从搭建)
        * [全量复制](#全量复制)
        * [增量复制](#增量复制)
    * [哨兵模式](#哨兵模式)
        * [哨兵模式原理](#哨兵模式原理)
        * [哨兵模式搭建](#哨兵模式搭建)
        * [小结](#小结)
* [Jedis操作Redis](#jedis操作redis)
    * [概述](#概述)
    * [Jedis操作Key](#jedis操作key)
        * [Jedis操作String类型数据](#jedis操作string类型数据)
        * [Jedis操作List类型数据](#jedis操作list类型数据)
        * [Jedis操作Hash类型数据](#jedis操作hash类型数据)
        * [Jedis操作Set类型数据](#jedis操作set类型数据)
        * [Jedis操作Zset类型数据](#jedis操作zset类型数据)

<!-- vim-markdown-toc -->
---

# NoSQL

## 数据库应用历程

1. 单机数据库时代：一个应用对应一个数据库实例
2. 缓存、水平切分时代
3. 读写分离时代
4. 分表分库时代（集群）
5. NoSQL时代

## 什么是NoSQL

NoSQL = Not Only SQL，直译为：不仅仅是SQL，泛指`non-relational`非关系型数据库。今天随着互联网web2.0网站的兴起，比如谷歌Facebook每天为他们的用户收集万亿比特的数据，这些类型的数据存储不需要固定的模式，无需多余操作就可以横向扩展，就是一个数据量超大。传统的SQL语句库不再适用这些应用了，NoSQL数据库是为了解决大规模数据集合多重数据种类带来的挑战，特别是超大规模数据的存储。彻底改变底层存储机制，不采用关系型数据（表）的方式存储数据了，采用聚合数据结构存储数据。`redis`、`mongoDB`、`HBase`等

NoSQL数据库的一个显著特点就是去掉了关系数据库的关系型特征，数据之间一旦没有关系，是的扩展性、读写性能都大大提高。

NoSQL和传统的关系型数据库不是排斥和取代的关系，在一个分布式应用中往往是结合使用的。复杂的互联网应用通常都是多数据源、多数据类型，应该根据数据的使用情况和特点，存放在合适的数据库中。

## NoSQL数据模型

传统关系型数据库的数据模型是以表为单位的：t_student、t_address、t_course
```
{
 "student":{
   "id":1001,
   "name":"zhangsan",
   "addresses":{"province":"beijing","city":"daxingqu","street":"liangshuihe"},
   "courses":[
   	{
     		"id":01,
     		"name":"java"
     	},
	{
     		"id":02,
     		"name":"mybatis"
     	},
	{
     		"id":03,
     		"name":"spring"
     	}
  ]
}
```

# Redis

## 简介

Redis = Remote Dictionary Server，直译为远程字典服务器，是一个用C语言编写、开源、基于内存运行并支持持久化的、高性能NoSQL数据库，也是当前热门的 NoSQL数据库之一

## 特点

### 支持数据持久化

Redis支持数据的持久化，可以将内存中的数据保持在磁盘中，重启的时候可以再次加载进行使用

### 支持多种数据结构

Redis不仅仅支持简单的`key-value`类型的数据，同时还提供`list`、`set`、`zset`、`hash`等数据结构的存储

### 支持数据备份

Redis支持数据的备份，即`master-slave`模式的数据备份

## Redis使用步骤

+ 下载Redis

进入`https://redis.io/`，选择其他版本页面，选择其他历史版本，点击下载5.0.2版本

+ 安装Redis（Linux环境）

    1. 上传`redis-5.0.2.tar.gz`包到Linux服务器
    2. 执行`tar -zxvf redis-5.0.2.tar.gz`命令解压
    3. 执行`gcc -v`检查是否安装了`gcc`命令，如果没有安装，则执行`yum -y install gcc`来安装`gcc`
    4. 进入解压目录，执行`make`命令来进行编译安装（如果安装失败，则执行`make distclean`命令来清除已经生成的残余文件，解决问题后再重新安装）
    5. 最后执行`make install`命令来将`redis`生成的可用命令提取的`/usr/local/bin`中，即环境变量中，使我们可以随时使用`redis`的命令

+ 启动Redis（Linux环境）

    1. 方式一：前台启动

       任意目录下执行命令 `redis-server`

    2. 方式二：后台启动

       任意目录下执行命令`redis-server &`

    3. 方式三：根据配置文件后台启动

       任意目录下执行命令`redis-server redis.conf &`

       > 如果修改了redis的配置文件`redis.conf`，必须在启动时指定配置文件，否则修改无效

+ 关闭Redis（Linux环境）

    1. 方式一

       在任意目录下执行命令`redis-cli shutdown`

    2. 方式二（不推荐）

       执行`ps -ef | grep redis`查看redis的pid，然后执行`kill -9 redisPid编号`

+ 配置Redis（Linux环境）

如果我们Redis服务端没有进行任何的配置，那么我们远程连接的时候就可以出现警告提醒或者有些功能我们不能使用

例如：我们在连接到远程Redis服务端的时候，使用`ping`命令测试Redis是否可用时候，可以出现`DENIED Redis is running in protected mode because protected mode is enabled`的报错。这个时候我们就需要对远程的Redis服务端进行配置，以解决问题。

    1. 打开Redis服务端的`Redis.conf`配置文件
    
    2. 注释`bind 127.0.0.1`，使其失效
    
    3. 将`daemonize no`设置为no
    
    4. 将`protected-mode no`设置为no
    
    5. 将`requirepass foobared`的注解去掉，`foobared`改成自定义密码
    
    6. 重启redis服务端
    
       1. 在redis服务端使用`ps -ef | grep redis`查询redis的进程号
       2. 然后使用`kill -9 redis进程号`关闭redis服务端
       3. 最后`redis-server redis.conf &`启动redis服务端
    
    7. 使用Redis客户端远程连接Redis服务端
    
       `redis-cli -h 远程Redis的ip地址 -p 远程Redis端口号 -a 密码`
    
    8. 连接远程Redis服务端后，使用`ping`命令测试功能是否正常，如果返回`PONG`则表明正常

+ 连接Reids

上面我们都是说的Redis服务端的相关操作，但我们实际开发中我们更多的是通过Redis的客户端来进行连接的。

其实Redis的服务端已经包括的客户端了，我们在按照Redis服务端后只需要使用下面的命令即可使用Redis客户端连接Redis服务端。

    1. 连接本地的端口号为`6379`的redis服务
    
       执行`redis-cli`
    
    2. 连接本地其他端口号的redis服务
    
       执行`redis-cli -p 指定端口号`
    
    3. 连接其他主机指定端口号的redis服务（前提是连接的一方也安装了redis服务端）
    
       执行`redis-cli -h ip地址 -p 指定端口号`

+ 断开连接Redis

执行命令`exit`或者`quit`

## Redis的基本命令

+ 测试Redis服务的性能

`redis-benchmark`：在Redis服务端执行`redis-benchmark`来测试Redis服务端的性能

+ 查看Redis服务是否正常运行

`ping`：在登录Redis客户端后执行`ping`，执行后正常则返回`pong`

+ 查看Redis服务器的统计信息

`info`：在登录Redis客户端后执行`info`，查看Redis服务的所有统计信息

`info 信息段`：在登录Redis客户端后执行`info 信息段`，查看Redis服务器的指定统计信息，如`info Replication`查看集群的统计信息

+ 切换Redis数据库

> Redis数据库实例作用类似于MySql的数据库实例，Redis中的数据库实例只能由Redis服务来创建和维护，开发人员不能修改和自行创建数据库实例。
>
> 默认情况下，Redis会自动创建16个数据库实例，并且给这些数据库实例进行编号，从0开始一直到15，使用时通过编号来使用数据库。可以通过配置文件，指定Redis自动创建的数据个数。
>
> Redis的每一个数据库实例本身占用的存储空间是很少的，所以也不造成存储空间的太多浪费。

`select 数据库索引`：默认情况下，Redis客户端连接的时编号为0的数据库实例。可以使用`select 数据库索引`，来切换到指定编号的数据库

+ 查看当前数据实例中所有key的数量

`dbsize`：登录客户端后，可使用`dbsize`来查看当前数据实例中所有key的数量

+ 查看当前数据库实例中所有的key

`keys *`：登录客户端后，可使用`keys *`查看当前数据库实例中所有的key

+ 清空当前数据库中的数据

`flushdb`：登录客户端后，可使用`flushdb`清空当前数据库中的数据

+ 清空所有数据库中的数据

`flushall`：登录客户端后，可使用`flushall`清空所有数据库中的数据

+ 查看redis中所有的配置信息

`config get *`：登录客户端后，可使用`config get *`查看redis中所有的配置信息

`config get paramter`：登录客户端后，可使用`config get paramter`查看redis中指定的配置信息

## Redis的五种数据结构

### 字符串类型String
1. 单key——单value
2. Redis中最基本的数据结构
3. 可以存储任何类型的数据，包括二进制数据、序列化后的数据、JSON化的对象甚至是一张图片。最大512M

### 列表类型List
1. 单key——多value
2. 最左侧是表头，最右侧是表尾
3. 下标从0开始，到length-1
4. 按照插入顺序排序
5. 元素有序，并且可以重复
6. 底层实现是一个链表

### 集合类型Set
1. 单key——多无序value
2. 元素无序，但可以重复 

### 哈希类型Hash
Redis hash是一个stirng类型的field和value的映射表，hash特别适合用于存储对象

### 有序集合类型Zset（sorted set）
Redis有序集合zset和集合set一样也是string类型元素的集合，且不允许元素重复。不同的是zset的每个元素都会关联一个分数（分数可以重复），redis通过分数来为集合中的成员进行从大到小的排序

## Redis命令

### 关于key的操作命令

+ 获取数据库中所有key值 
    + 语法: `keys pattern`
    + 作用: 查看数据库中所有符合pattern模式的key (pattern表示通配符)
    + 通配符: 
            + `*`: 表示0或多个字符，例如: keys * 表示查询所有的key
            + `?`: 表示单个字符，例如: wo?d 表示匹配word、wood等
            + `[]`: 表示选择[]内的一个字符，例如wo[or]d 匹配word、wood,但不匹配woord
    ```redis
    /* 查看数据库中所有的key值 */
    keys *
    1) "age"
    2) "usr"
    3) "agge"
    4) "name"
    5) "namme"
    6) "user"
    
    /* 查看数据库中匹配na*e的key值 */
    keys na*e
    1) "name"
    2) "namme"
    
    /* 查看数据库中匹配a?e的key值 */
    keys a?e
    1) "age"
    
    /* 查看数据库中匹配us[er]的key值 */
    keys us[er]
    1) "usr"
    ```

+ 判断key是否存在
    + 语法: `exists key1 [key2 key3]`
    + 作用: 判断key在数据库中是否存在 
    + 返回值: 判断单个key，存在则返回1，不存在返回0；判断多个key，则返回存在的key数量
    ```redis
    /* 判断age这个key是否在数据库存在 */
    exists age
    (integer) 1
    
    /* 判断 address这个key是否在数据库中存在 */
    exists address
    (integer) 0
    
    /* 判断age user name u这个四个key是否在数据库存在 */
    exists age user name u
    (integer) 3
    ```

+ 移动指定key到指定的数据库中
    + 语法: `move key index`
    + 作用: 移动key到指定的数据库中，移动的key在原数据库中被删除
    + 返回值: 移动成功返回1，移动失败返回0
    ```redis
    # 移动key为agge的数据到数据库1中
    move agge 1
    # (integer) 1
    ```

+ 查看key剩余生存时间
    + 语法: `ttl key`
    + 作用: 查看指定key的剩余生存时间，单位秒
    + 返回值: 
            + `-1`: 没有设置key的生存时间，key永不过期
            + `-2`: key不存在，或者已经过期了
    ```redis
    # 查看key为age的数据剩余生存时间为永久 
    ttl age
    # (integer) -1
    
    # 查看key为namme的数据剩余生存时间为13秒
    ttle namme
    # (integer) 13
    ```

+ 设置key的最大生存时间
    + 语法: `expire key seconds`
    + 作用: 设置key的最大生存时间，超过时间key会被自动删除，单位为秒
    + 返回值: 
             + `1`: 设置成功
             + `0`: 设置失败
    ```redis 
    # 设置key为age的元素的最大生存时间为20秒
    expire age 20
    # (integer) 1
    ```

+ 查看key的数据类型
    + 语法: `type key`
    + 作用: 查看指定key所储存的数据的类型
    + 返回值: 
            + `none`——key不存在
            + `string`——字符串
            + `list`——列表
            + `set`——集合
            + `zset`——有序集合
            + `hash`——哈希表
    ```redis
    # 查看到key=user元素所储存的数据类为string    
    type user
    # string
    
    # 查看到key=userList元素所储存的数据类型为list
    type userList
    # list
    ```
+ 重命名key
    + 语法: `rename key newKey`
    + 作用: 将key改名为newKey；当key和newKey相同、或key不存在时，返回一个错误
    ```redis
    # 将password改名为pw
    rename password pw
    ```
+ 删除指定的key
    + 语法: `del key [key...]`
    + 作用: 删除指定的key，如果不存在则忽略
    + 返回值: 整数，为被删除的key的数量
    ```redis
    # 成功删除key为pw的数据
    del pw    
    # (integer) 1
    
    # 成功删除key为usr、user的数据
    del usr user
    # (integer) 2
    
    # 因为key为person的数据不存在，所以删除失败
    del person
    # (integer) 0
    ```
### 关于string数据操作命令

+ 保存字符串数据
    + 语法: `set key value`
    + 作用: 将字符串值value设置到key中，如果key已存在，则覆盖之前的值
    ```redis
    # 保存age=22字符串数据到数据库中
    set age 22
    # OK
    ```
    
+ 取出字符串数据
    + 语法: `get key`
    + 作用: 获取key中设置的字符串value值
    + 返回值: key存在，返回key对应的value值; key不存在，返回nil
    ```redis
    # 获取name对应的字符串value值
    get name
    # "Jason"
    ```
    
+ 追加字符串
    + 语法: `append key value`
    + 作用: 
        + 如果key存在，则将value追加到key原来旧值的末尾
        + 如果key不存在，则为key设置value字符串值
    + 返回值: 追加之后字符串value值的字符个数
    ```redis
    # 将1追加到age对应的value值后面
    append age 1
    # 3
    ```

+ 获取字符串长度
    + 语法: `strlen key`
    + 作用: 返回key所对应的value值字符串的长度
    + 返回值: 
        + key存在，则返回其对应value值字符串的长度
        + key不存在，则返回0
    ```redis
    # 获取name对应value值字符串的长度
    strlen name
    # (integer) 5  
    ```

+ 字符串+1运算
    + 语法: `incr key`
    + 作用: 
        + 若key所对应的value字符串值是一个数字，则进行+1运算
        + 若key不存在，则先创建key并设置其对应的value字符串值为0，然后进行+1运算
        + 若key所对应的value字符串值不是一个数字，则报错
    ```redis
    # 对age对应的value值进行+1运算
    incr age
    # (integer) 222
    
    # 对不存在的height进行+1运算
    incr height
    # (integer) 1
    
    # 对其value字符串值不是数字的name进行+1运算
    incr name
    # (error) ERR value is not an integer or out of range
    ```
    
+ 字符串-1运算 
    + 语法: `decr key`
    + 作用: 
        + 若key所对应的value字符串值是一个数字，则进行-1运算
        + 若key不存在，则先创建key并设置其对应的value字符串值为0，然后进行-1运算
        + 若key所对应的value字符串值不是一个数字，则报错
    ```redis
    # 对age对应的value字符串值进行-1运算
    decr age
    # (integer) 221
    
    # 对不存在的weight进行-1运算
    decr weight
    # (integer) -1
    
    # 对其value字符串值不是数字的name进行-1运算
    decr name
    # (error) ERR value is not an integer or out of range
    ```

+ 字符串增量运算
    + 语法: `incrby key offset`
    + 作用:
        + 若key所对应的value字符串值是一个数字，则进行+offset运算
        + 若key不存在，则先创建key并设置其对应的value字符串值为0，然后进行+offset运算
        + 若key所对应的value字符串值不是一个数字，则报错
    + 返回值: 进行增量运算后的字符串值
    ```redis
    # 对age对应的value字符串值进行+10运算
    incrby age 10
    # (integer) 231
    
    # 对不存在的count进行+20运算
    incrby count 20
    # (integer) 20
    
    # 对其value字符串值不是数字的name进行+30运算
    incrby name 30
    # (error) ERR value is not an integer or out of range
    ```

+ 字符串减量运算
    + 语法: `decyby key offset`
    + 作用: 
        + 若key所对应的value字符串值是一个数字，则进行-offset运算
        + 若key不存在，则先创建key并设置其对应的value字符串值为0，然后进行-offset运算
        + 若key所对应的value字符串值不是一个数字，则报错
    + 返回值: 进行减量运算后的字符串值
    ```redis
    # 对age对应的value字符串值进行-10运算
    decrby age 10
    # (integer) 221
    
    # 对不存在的number进行-20运算
    decrby number 20
    # (integer) -20
    
    # 对其value字符串值不是数字的name进行-30运算
    decrby name 30
    # (error) ERR value is not an integer or out of range 
    ```

+ 截取字符串 
    + 语法: `getrange key startIndex endIndex`
    + 作用: 
        + 获取key对应的字符串值的从startIndex到endIndex处的子字符串
        + 截取范围同时包括startIndex和endIndex
        + 索引从0开始，负数表示从字符串末尾开始计数，例如-2表示倒数第二个字符
    + 返回值: 截取到的子字符串
    ```redis
    # 设置user对应的字符串值为BernardoLiJasonLi
    set user BernardoLiJasonLi
    # OK
    
    # 获取user对应的字符串值的索引1到5的子字符串
    getrange user 1 5
    # "ernar"
    
    # 获取user对应的字符串值的索引3到-2的子字符串
    getrange user 3 -2
    # "nardoLiJasonL"
    ```

+ 修改字符串
    + 语法: `setrange key offset value`
    + 作用: 从offset索引处开始，使用value覆盖key对应的旧值
    + 返回值: 修改后的字符串长度
    ```redis
    # 查看user对应的旧字符串值
    get user
    # "BernardoLiJasonLi"
    
    # 修改user对应的字符串值
    setrange user 8 wang
    # (integer) 17
    
    # 查看user修改后的字符串值
    get user
    # "BernardowangsonLi"
    ```

+ 设置key的value值和生存时间
    + 语法: `setex key seconds value`
    + 作用: 
        + 若key不存在，则保存key及其value值到数据库中，并为其设置最大生存时间，单位秒
        + 若干key存在，则覆盖key的旧值，并为其设置最大生存时间，单位秒
    + 返回值: 返回OK，则表示设置成功
    ```redis
    # 设置rick的字符串值为morty，并同时设置生存时间为30秒
    setex rick 30 morty
    # OK
    
    # 查看rick对应的value字符串值
    get rick
    # "morty"
    
    # 查看rick的剩余生存时间
    ttl rick
    # (integer) 17
    ```

+ 设置不存在的key值
    + 语法: `setnx key value`
    + 作用:
        + 若key不存在，则设置其对应的字符串值
        + 若key存在，则不进行设置
    + 返回值:
        + 返回1，表示设置成功
        + 返回0，表示设置失败
    ```redis
    # 设置不存在的person对应的value值为human
    setnx person human
    # (integer) 1
    
    # 设置存在的user对应的value值为one
    setnx user one
    # (integer) 0
    ```

+ 设置多个key-value值
    + 语法: `mset key value [key value...]`
    + 作用: 同时设置一个或者多个key与其对应的value值
    + 返回值: 返回OK，表示设置成功
    ```redis
    # 同时设置name=Bernardo、age=22、gender=man
    mset name Bernardo age 22 gender man
    # OK
    
    # 查看name的对应字符串值
    get name
    # "Bernardo"
    
    # 查看age的对应字符串值
    get age
    # "22"
    
    # 查看gender的对应字符串值
    get gender
    # "man"
    ```

+ 获取多个value值
    + 语法: `mget key [key...]`
    + 作用: 获取一个或者多个给定key的对应value字符串值
    + 返回值: 所有给定key的对应value字符串值，若key不存在则返回nil
    ```redis
    # 同时获取name、age、gender、player的对应value字符串值，其他只有player不存在
    mget name age gender player
    # 1) "Bernardo"
    # 2) "22"
    # 3) "man"
    # 4) (nil)
    ```

+ 设置多个不存在的key-value值
    + 语法: `msetnx key value [key value...]`
    + 作用: 同时设置一个或者多个key-value值，如果一个key存在，则全部设置失败
    + 返回值: 
        + 返回1，表示设置成功
        + 返回0，表示设置失败
    ```redis
        # 同时设置name=Jason、age=22、gender=man
        msetnx name Jason age 22 gender man
        # (integer) 1
    
        # 同时获取name、age、gender三个对应的value字符串值
        mget name age gender
        # 1) "Jason"
        # 2) "22"
        # 3) "man" 
        
        # 同时设置username=root、password=admin、gender=man，其他gender已经存在
        msetnx username root password admin gender man
        # (integer) 0
    
        # 同时获取username、password、gender三个对应的value字符串值
        # 发现除了已经存在的gender，其他的都设置失败了
        mget username password gender
        # 1) (nil)
        # 2) (nil)
        # 3) "man"
    ```

### 关于list数据操作命令

+ 在list表头插入数据
    + 语法: `lpush key value [value...]`
    + 功能: 将一个或者多个value值依次插入到list列表的表头
    + 返回值: value值插入之后的list列表长度
    ```redis
    # 将one、two、three依次插入到userList列表的表头
    lpush userList one two three
    # (integer) 3
    
    # 获取userList列表的全部数据，查看插入顺序
    lrange userList 0 -1
    # 1) "three"
    # 2) "two"
    # 3) "one"
    ```

+ 在list表尾插入数据
    + 语法: `rpush key value [value...]`
    + 作用: 将一个或者多个value值依次插入到list列表的表尾
    + 返回值: value值插入之后的list列表长度
    ```redis
    # 将22、23、24依次插入到ageList列表的表尾
    rpush ageList 22 23 24
    # nteger) 3
    
    # 获取userList列表的全部数据，查看插入顺序
    lrange ageList 0 -1
    # 1) "22"
    # 2) "23"
    # 3) "24"
    ```

+ 获取list列表元素
    + 语法: `lrange key startIndex endIndex`
    + 作用: 
        + 获取list列表中指定索引区间内的元素
        + 索引区间是两头闭合的
        + 索引从0开始，可以为负数，超出范围不会报错
    + 返回值: 获取到元素列表
    ```redis
    # 获取humanList列表索引2到4处的元素
    lrange humanList 2 4
    # 1) "Justin"
    # 2) "Bernardo"
    # 3) "Jason"
    
    lrange humanList 0 -1
    # 1) "Bob"
    # 2) "Charlie"
    # 3) "Justin"
    # 4) "Bernardo"
    # 5) "Jason"
    ```

+ 弹出list表头的元素
    + 语法: `lpop key`
    + 功能: 移除并返回list列表表头的第一个元素
    + 返回值: list列表左侧第一个(即表头)元素的值，若列表key不存在，则返回nil
    ```redis
    # 查看弹出表头元素前humanList列表的全部数据
    lrange humanList 0 -1
    # 1) "Bob"
    # 2) "Charlie"
    # 3) "Justin"
    # 4) "Bernardo"
    # 5) "Jason"
    
    # 弹出list表头的元素
    lpop humanList
    # "Bob"
    
    # 查看弹出表头元素后humanList列表的全部数据
    lrange humanList 0 -1
    # 1) "Charlie"
    # 2) "Justin"
    # 3) "Bernardo"
    # 4) "Jason"
    ```

+ 弹出list表尾的元素
    + 语法: `rpop key`
    + 作用: 移除并返回列表key尾部最后一个元素
    + 返回值: 列表右侧第一个(即表尾)元素的值，若列表key不存在，则返回nil
    ```redis
    # 查看弹出表尾元素前的humanList列表的全部数据
    lrange humanList 0 -1
    # 1) "Charlie"
    # 2) "Justin"
    # 3) "Bernardo"
    # 4) "Jason"
    
    # 弹出list表尾的元素
    rpop humanList
    # "Jason"
    
    # 查看弹出表尾元素后的humanList列表的全部数据
    lrange humanList 0 -1
    # 1) "Charlie"
    # 2) "Justin"
    # 3) "Bernardo"
    ```

+ 查看list指定索引元素
    +  语法: `lindex key index`
    +  作用: 查看list列表指定索引处的元素，不影响原列表
    +  返回值: 指定索引处元素的值，若list不存在，则返回nil
    ```redis
    # 查看userList列表全部的元素
    lrange userList 0 -1
    # 1) "three"
    # 2) "two"
    # 3) "one"
    
    # 查看userList列表索引处为2的元素
    lindex userList 2
    # "one"
    ```

+ 获取list列表的长度
    + 语法: `llen key`
    + 作用: 获取list列表的元素个数
    + 返回值: 整数，list列表的元素个数；若key不存在则返回0
    ```redis
    # 获取userList列表的元素个数
    llen userList
    # (integer) 3
    ```

+ 删除指定个数的列表同一元素
    + 语法: `lrem key count value`
    + 作用: 从list列表中删除count个与参数value相等的元素
    + 返回值: 整数，list列表中被移除的元素个数
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210823210044.png)

+ 截取list列表
    + 语法: `ltrim key startIndex endIndex`
    + 作用: 
        + 截取key的指定索引区间的元素,并赋值给key
        + startIndex和endIndex超出范围不会报错
    + 返回值: 执行成功返回OK
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825035242.png)

+ 修改指定list列表指定索引处的value值
    + 语法: `lset key index value`
    + 作用: 将列表key下标为index的元素值设置为value
    + 返回值: 
        + 设置成功则返回OK
        + key不存在或者index超出范围则返回错误信息
        ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825035914.png)

+ 在list列表中插入数据
    + 语法: `linsert key before/after pivot value`
    + 功能: 
        + 将值value插入到列表key当中位于值pivot之前或者之后的位置
        + key不存在或者pivot不在列表中，不执行任何操作
        + 如果列表key中存在多个pivot，则采用索引小
    + 返回值:
        + 命令执行成功，则返回新列表的长度
        + 没有找到pivot，则返回-1
        + key不存在，则返回0
        ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825040711.png) 

### 关于set数据操作命令

> Redis的set集合数据是string类型的无序不重复集合
> 集合类型的数据操作总的思想是通过key确定集合，key是集合标识，其元素没有下标，只有直接操作业务数据和数据个数

+ 添加元素到set集合中
    + 语法: `sadd key member [member...]`
    + 作用: 
        + 如果key集合不存在，则创建key集合，并将一个或者多个member元素添加到key集合中
        + 如果key集合存在，则直接将一个或者多个member元素添加到key集合中
        + 已经存在与key集合的元素将被忽略，不会再被添加
    + 返回值: 加入到集合的新元素个数（不包括被忽略的元素）
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825043617.png)

+ 获取set集合中的所有元素
    + 语法: `smembers key`
    + 作用: 获取key集合中所有成员元素，不存在的key视为空集合
    + 返回值: 返回key集合的所有成员元素，不存在的key则返回空集合
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825044352.png)

+ 判断set集合是否存在某元素
    + 语法: `sismember key member`
    + 作用: 判断member元素是否是key集合的成员元素
    + 返回值: 
        + member元素是key集合的成员元素，则返回1
        + member元素不是key集合的成员元素，则返回0
        ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825045111.png)

+ 获取set集合长度
    + 语法: `scard key`
    + 作用: 获取key集合中成员元素个数
    + 返回值: 
        + 整数，key集合中成员元素的个数
        + 其他情况返回0
        ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825045551.png)

+ 删除set集合元素
    + 语法: `srem key member [member...]`
    + 功能: 移除key集合中一个或者多个元素，不存在的元素会被忽略
    + 返回值: 整数——成功移除的key集合中成员元素的个数，不包括被忽略的元素
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825050146.png)

+ 随机获取set集合一个元素
    + 语法: `srandmember key [count]`
    + 作用: 
        + 随机获取一个key集合中的一个或多个元素,不影响原key集合,没有count参数时，默认获取一个
        + 可选参数`count > 0`，则随机获取的count个元素不会重复
        + 可选参数`count < 0`，则随机获取的count个元素可能重复
    + 返回值: 随机获取到的key集合中一个或者多个元素
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825051228.png)

+ 随机弹出set集合一个元素
    + 语法: `spop key [count]`
    + 作用: 随机从key集合中弹出一个或者count个元素
    + 返回值: 
        + key集合中被弹出的元素
        + key不存在或者为空集合，则返回nil
        ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825051948.png)

+ 将元素从两个set集合之间移动
    + 语法: `smove src dest member`
    + 作用: 
        + 将member元素从src集合移动到dest集合
        + 若member不存在，则不进行操作，返回0
    + 返回值: 
        + 移动成功，则返回1
        + 其他情况，则返回0
        ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825053014.png)

+ 获取集合之间的差集
    + 语法: `sdiff key key [key...]`
    + 作用: 返回指定几个集合的差集，以第一个集合为准进行比较，即第一个集合中有但在其他任何集合中都没有的元素组成的元素
    + 返回值: 
        + 返回第一个key集合中有而后面集合中都没有的元素组成的集合
        + 如果第一个集合中的元素在后面集合中都没有，则返回空集合
        ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825054025.png)

+ 获取集合之间的交集
    + 语法: `sinter key key [key...]`
    + 作用: 返回指定几个集合的交集,即指定的所有集合中都有的元素组成的集合
    + 返回值: 交集元素组成的集合，如果没有则返回空集合
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825054602.png)

+ 获取集合之间的并集
    + 语法: `sunion key key [key...]`
    + 作用: 返回指定几个集合的并集，即指定的所有集合元素组成的大集合；如果几个集合中元素有重复，只保存一个
    + 返回值: 所有集合元素组成的大集合，如果所有key都不存在，则返回空集合
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825055049.png)

### 关于hash数据操作命令

> Redis的hash是一个string类型的key和value的映射表，这里的value是一系列的键值对，hash特别适合用于存储对象
> 哈希类型的数据操作总的思想是通过key和field操作value，key是数据标识，field是域，value是我们需要的业务数据

+ 设置值到哈希列表中
    + 语法: `hset key field value [field value...]`
    + 作用: 
        + 将键值对field-value设置到哈希列表key中
        + 如果key还不存在，则先新建哈希列表，再将field-value设置到新建的哈希列表key中
        + 如果key已经存在、但field不存在，则直接将field-value设置到哈希列表key中
        + 如果可以已经存在、field也已经存在，则用value覆盖旧的value值
        ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825172227.png)

+ 从哈希列表中获取单个值
    + 语法: `hget key field`
    + 作用: 获取哈希列表key中给定域field的value值
    + 返回值: key哈希列表中field域的value值，若key或field不存在，则返回nil
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825172709.png)

+ 批量设置值到哈希列表中
    + 语法: `hmset key field value [field value...]`
    + 作用: 
        + 同时将多个field-value设置到哈希列表key中
        + 若key不存在，则先创建新的哈希列表key，然后再将多个field-value设置到哈希列表key中
        + 若key与field同时都已经存在，则用value覆盖旧的值
        ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825173433.png) 

+ 批量从哈希列表中获取值
    + 语法: `hmget key field [field...]`
    + 作用: 顺序获取哈希列表key中一个或者多个给定field域的value值
    + 返回值: 哈希列表key中一个或者多个给定field域顺序对应的value值；如果key或field不存在，则返回nil
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825174014.png) 

+ 同时获取哈希列表的field和value
    + 语法: `hgetall key`
    + 作用: 同时从哈希列表key中或者field和value组成的集合
    + 返回值: 以列表形式返回的哈希列表key中field、value值组成的集合；若key不存在，则返回空哈希列表
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825174926.png) 

+ 批量删除哈希列表中的field-value
    + 语法: `hdel key field [field...]`
    + 作用: 删除哈希列表key中一个或者多个给定field域及其对应的value值；若key或field不存在，则不进行操作
    + 返回值: 成功删除的field-value数据量
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825175525.png)

+ 获取哈希列表的长度
    + 语法: `hlen key`
    + 作用: 获取哈希列表key中field域的个数
    + 返回值: 整数，哈希列表中field域的个数；key不存在，则返回0
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825175950.png)

+ 判断指定field元素是否存在
    + 语法: `hexists key field`
    + 作用: 查看哈希列表key中，给定域field是否存在
    + 返回值: 如果field存在，则返回1；其他情况返回0
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825180311.png)

+ 查看哈希列表所有的field域
    + 语法: `hkeys key`
    + 作用: 查看哈希列表key中的所有field域的集合
    + 返回值: 包含所有field的列表；若key不存在，则返回空集合
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825180630.png)

+ 查看哈希列表所有的value值
    + 语法: `hvals key`
    + 作用: 查看哈希列表key中的所有value值的集合
    + 返回值: 包含所有value的列表；若key不存在，则返回空集合
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825181219.png)

+ 整数增量运算
    + 语法: `hincyby key field int`
    + 作用: 给哈希列表key中给定field域对应的value值进行+int运算(int必须为整数)
    + 返回值: 进行增量运算后的field域对应的value值
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825182407.png)

+ 浮点数增量运算
    + 语法: `hincrbyfloat key field float`
    + 作用: 给哈希列表key中给定的field域对应的value值进行+float运算(float必须为浮点数)
    + 返回值: 增加之后field域对应的value值
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825182923.png)

+ 检查设置值到哈希列表中
    + 语法: `hsetnx key field value`
    + 作用: 将哈希列表key中的field域对应的旧值设置为value，当且仅当field不存在的时才进行设置
    + 返回值: 设置成功返回1，其他情况返回0
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825183356.png)

### 关于Zset数据操作命令

> Redis有序集合zset和集合set一样也是string类型元素的集合，且不允许重复的成员
> 不同的是zset的每个元素都会关联一个分数（分数可以重复），redis通过分数来为集合中的成员进行从小到大的排序

** 添加元素到zset集合 **
    + 语法: `zadd key score member [score member...]`
    + 作用: 
        + 将一个或者多个member元素及其score值加入到有序集合key中
        + 如果member已经存在，则覆盖member原来关联的score
        + score可以是整数，也可以是浮点数
    + 返回值: 整数，新成天添加的member元素个数
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825201115.png)

** 查看zset有序集合指定索引区间的元素 **
    + 语法: `zrange key startIndex endIndex [WITHSCORES]`
    + 作用: 
        + 查看有序集合key指定索引区间的元素，元素按照score值从小到大排序
        + 索引区间是双闭合的，即包含startIndex，包含endIndex处的元素
        + 索引值可以为负数，表示倒数第几个元素；例如：-2 表示倒数第二个元素
    + 返回值: 指定索引区间元素组成的集合
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825202024.png)    

** 查看zset有序集合指定score区间的元素 **
    + 语法: `zrangebyscore key min max [WITHSCORES] [LIMIT offset count]`
    + 作用: 获取有序集合key中所有score值介于min和max之间(包含min和max)的成员; 有序成员是按递增排序的
    + 返回值: 指定score区间的元素
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825204327.png)

** 删除zset有序集合元素 **
    + 语法: `zrem key member [member...]`
    + 作用: 删除有序集合key中的一个或者多个member成员，其中不存在的member成员会被忽略
    + 返回值: 有序集合key中成功被删除的成员数量，不包括被忽略的成员
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825204934.png)

** 获取zset有序集合的元素个数 **
    + 语法: `zcard key`
    + 作用: 获取有序集合key 的元素成员个数
    + 返回值: 有序集合key的元素个数；若key不存在，则返回0
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210825205437.png)

** 获取zset有序集合指定score区间内元素的个数 **
    + 语法: `zcount key min max`
    + 作用: 获取有序集合key中score值在min到max之间(包括min和max)的元素数量
    + 返回值: 有序集合key在指定score值区间内元素的数量
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826002219.png)

** 获取zset有序集合指定元素的排名 **
    + 语法: `zrank key member`
    + 作用: 获取有序集合key中元素member的排名；有序集合key的元素按照score的值从小到大的顺序排序，最小值为0
    + 返回值: 指定元素在有序集合中的排名；如果指定元素不存在，则返回nil
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826002724.png)

** 获取zset有序集合指定元素的分数 **
    + 语法: `zscore key member`
    + 功能: 获取有序集合key中元素member的分数
    + 返回值: 有序集合key中元素member的分数
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826011342.png)

** 获取zset有序集合中元素的反向排名 **
    + 语法: `zrevrank key member`
    + 作用: 获取有序集合key中成员member的反向排名；反向排名即：有序集合元素按score从大到小顺序排列，score最大为0
    + 返回值: 指定元素在有序集合key中的排名；如果指定元素不存在，则返回nil
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826013741.png)

** 逆向查询zset有序集合索引区间内的元素 **
    + 语法: `zrevrange key startIndex endIndex [withscores]`
    + 作用: 
        + 逆向查询有序集合key中索引区间startIndex到endIndex的元素；
        + 逆向查询即为：元素按score值从大到小排序
        + 索引区间包括startIndex和endIndex，索引也可以取负数，表示倒数
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826015606.png)

** 逆向获取zset有序集合指定score区间内元素 **
    + 语法: `zrevrangebyscore key max min [withscores] [limit offset count]`
    + 作用: 
        + 逆向获取有序集合key中，所有score值区间max和min之间元素，从大到小排序
        + 区间包含max和min
        + withscores显示value对应的score
        + limit用来限制返回元素及其score集合的，在结果集中从第offset个开始，取count个
    + 返回值: 逆向查询的有序集合zset指定score区间内的元素
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826022843.png)

## Redis配置文件

### redis.conf存放位置
Redis的安装根目录下，Redis在启动时会加载这个配置文件，在运行时按照配置进行工作。这个文件有时候我们会拿出来，单独存放在某一个位置，启动的时候必须明确指定哪个配置文件，此文件才会生效。

### Redis网络相关配置
1. `bind`: 绑定IP地址，其他机器可以通过此IP访问Redis。默认绑定127.0.0.1，也可以修改为本机的IP地址
2. `prot`: 配置Redis占用的端口，默认是6379
3. `tcp-keepalive`: TCP连接保活策略，可以通过`tcp-keepalive`配置项来进行设置，单位为秒。假如设桌为60秒，则server端会每60秒想连接空闲的客户端发起一次ACK请求，以检查客户端是否已经挂掉，对于无响应的客户端则会关闭其连接。如果设置为0，则不会进行保活检测

### Redis常规配置
1. `loglevel`: 日志级别，开发阶段可以设置成debug，生产阶段通常设置为notice或者warning
2. `lgofile`: 指定日志文件名，如果不指定，Redis只进行标准输出。要保证日志文件所在的目录必须存在，文件可以不存在。还要在Redis启动时指定所使用的配置文件，否则配置不起作用
3. `databases`: 配置Redis数据库的个数，默认是16个

### Redis安全配置
`requirepass`: 配置Redis的访问密码。默认不配置密码，即访问时不需要密码验证。此配置项需要在`protected-mode=yes`时起作用。使用密码登录客户端: redis-cli -h ip -p 6379 -a pwd

### Redis的RDB配置
1. `save <seconds> <changes>`: 配置复合的快照触发条件，即Redis在seconds秒内key改变changes次，Redis把快照内的数据保存到磁盘中一次。如果要禁用Redis的持久化功能，则把所有的save配置都注释掉
2. `stop-writes-on-bgsave-error`: 当bgsave快照操作出错时停止写数据到磁盘，这样能保证内存数据和磁盘数据的一致性，但如果不在乎这种一致性，要在bgsave快照操作出错时继续操作，这里需要配置为no
3. `rdbcompression`: 设置对于存储到磁盘中的快照是否进行压缩。设置为yes时，Redis会采用LZF算法进行压缩。如果不想消耗CPU进行压缩的化，可以设置为no，关闭此功能
4. `rdbchecksum`: 在存储快照以后，还可以让Redis使用CRC64算法来进行数据效验，但这样会消耗一定的性能。如果系统比较在意性能的提升，可以设置为no，关闭此功能
5. `dbfilename`: Redis持久化数据生成的文件名，默认是`dump.rdb`，也可以自己配置
6. `dir`: Redis持久化数据生成文件保存的目录，默认是`./`即redis的启动目录，也可以自己配置

### Redis的AOF配置
1. `appendonly`: 配置是否开启AOF，yes表示开启，no表示关闭。默认为no
2. `appendfilename`: AOF保存文件名
3. `appendfsync`: AOF异步持久化策略
    + `always`: 同步持久化，每次发生数据变化会立刻写入到磁盘中。性能较差但数据完整性比较好
    + `everysec`: 出厂默认推荐，每秒异步记录一次（默认值）
    + `no`: 不即时同步，由操作系统决定何时同步
4. `no-appedfsync-on-rewrite`: 重写时是否可以运用`appendsync`，默认no，可以保证数据的安全性
5. `auto-aof-rewrite-percentage`: 设置重写的基准百分比

## Redis的持久化
Redis是内存数据库，它把数据存储在内存中，这样在加快读取速度的同时也对数据安全性产生了新的问题。即当Redis所在服务器发生宕机后，Redis数据库里的所有数据将会全部丢失。为了解决这个问题，Redis提供了持久化功能——RDB和AOF（Append Only File)

### RDB
RDB(Redis DataBase)是Redis默认的持久化方案。在指定的时间间隔内，执行指定次数的写操作，则会将内存中的数据写入到磁盘中。即在指定目录下生成一个`dump.rdb`文件，Redis重启会通过加载`dump.rdb`文件来恢复数据

#### RDB原理
Redis会复制一个与当前进程一样的进程。新进程的所有数据（变量、环境变量、程序计数器等）数值都和原进程一致，但是是一个全新的进程，并作为原进程的子进程，来进行持久化。
整个过程中，主进程是不进行任何IO操作的，这就确保了极高的性能。
如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感,那RDB方式要是AOF方式更加的高效。RDB的缺点是最后一次持久化后的数据可能丢失

#### RDB保存的文件
RDB保存的文件是`dump.rdb`文件，位置保存在Redis的启动目录。Redis每次同步数据到磁盘都会生成一个`dump.rdb`文件，新的`dump.rdb`会覆盖旧的`dump.rdb`文件

####  配置RDB持久化策略
在Redis.conf的快照配置中，配置RDB保存的策略。
在客户端执行`flushdb`或者`flushall`或者`shutdown`时，也会将快照中的数据保存到`dump.rdb`，只不过这种操作已经把数据清空了，保存的也是空文件，没有意义。

#### 手动保存RDB快照
save命令执行一个同步保存操作，将当前Redis实例的所有数据快照(snapshot)以RDB文件的形式保存到硬盘。
由于save指令会阻塞所有客户端，所以保存数据库的任务通常由BGSAVE命令异步执行，而save作为保存数据的最后手段来使用，当负责保存数据的后台子进程不幸出现问题时使用

#### RDB数据恢复
通过脚本将Redis产生的`dump.rdb`文件备份(cp dump.rdb_bak.rdb), 每次启动Redis前，把备份的`dump.rdb`文件替换到Redis相应的目录(在redis.conf中配的dir目录下), Redis启动时会加载`dump.rdb`文件，并且把数据读到内存中

#### 小结
Redis默认开启RDB持久化方式，适合大规模的数据恢复但它的数据一致性和完整性较差

### AOF
AOF(Append Only File)，Redis默认不开启。它的出现是为了弥补RDB的不足（数据的不一致性），所以它采用日志的形式来记录每个写操作，并追加到文件中。
Redis重启会根据日主文件的内容将写指定从前到后执行一次以完成数据的恢复工作。

#### AOF原理
Redis以日志的形式来记录每个写操作，将Redis执行过的所有写指定记录下来（读操作不记录），只许追加文件但不可以改写文件。
Redis启动之处会读取该文件重新构建数据，换言之，Redis重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作。

#### AOF保存的文件
AOF保存的文件是`appendonly.aof`文件，位置保存在Redis的启动目录。如果开启了AOF，Redis每次记录写操作都会往`appendonly.aof`文件追加新的日志内容

#### 配置AOF持久化策略
在`redis.conf`的`append only mode`配置模块中，配置AOF保存策略

#### AOF数据恢复
通过脚本将Redis产生的`appendonly.aof`文件备份(cp appendonly.aof appendonly_bak.aof), 每次启动Redis前把备份的`appendonly.aof`文件替换到Redis相应的目录(在redis.conf中配的dir目录下)。只要开启AOF功能, Redis每次启动，会根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作。
但在实际开发中，可能因为某些原因导致`appendonly.aof`文件格式异常，从而导致数据还原失败，可以通过命令`redis-check-aof --fix appendonly.aof`进行修复。会把出现异常的部分往后所有写操作日志去掉。

#### AOF的重写
AOF采用文件追加方式，文件会越来越大为避免出现此种情况，新增了重写机制。当AOF文件的大小超过所设定的阈值时，Redis就会启动AOF文件的内容压缩，只保留可以恢复数据的最小指令集。
AOF文件持续增长而过大时，会fork出一条新进程来将文件重写(也是先写临时文件最后再rename), 遍历新进程的内存中的数据，每条记录有一条set语句。重写aof文件的操作，并没有读取旧的aof文件，而是将整个内存中的数据库内容用命令的方式重写了一个新的aof文件，这点和快照有点类似。
Redis会记录上次重写时的AOF大小，默认配置是当AOF文件大小是上次rewrite后大小的一倍且文件大于64M时触发。当然也可以在配置文件中进行配置。

#### AOF小结
Redis 需要手动开启AOF持久化方式，AOF的数据完整性比RDB高，但记录内容多了，会影响数据恢复的效率。
关于Redis持久化的使用：若只打算用Redis做缓存，可以关闭持久化。若打算使用Redis的持久化，建议RDB和AOF都开启。其实RDB更适合做数据的备份，留一后手。AOF出问题了，还有RDB。
AOF与RDB模式可以同时启动，这并不冲突。如果AOF是可用的，那Redis启动时将自动加载AOF，这个文件能够提供更好的持久性保障。

## Redis事务

### 概述
Redis的事务允许在一次单独的步骤中执行一组命令，并且能够保证将一个事务中的所有命令序列化，然后按顺序执行。
事务中的所有命令都会被序列化，然后按顺序执行。事务在执行过程中，不会被其他客户端发来的命令请求所打断，除非使用watch命令监控某些键。
Redis同一个事务中如果一条命令执行失败，其后的命令仍然可能会被执行，redis的事务没有回滚。Redis已经在系统内部进行功能简化，这样可以确保更快的允许速度

### Redis事务的常用命令

** 开始事务 **
+ 语法: `multi`
+ 作用: 用于标记事务块的开始
+ 返回值: 开启成功则返回OK
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826054150.png)

** 执行事务 **
+ 语法: `exec`
+ 作用: 
    + 在一个事务中执行所有先前放入队列的命令，然后恢复正常的连接状态
    + 如果在把命令压入队列的过程中报错，则整个队列中的命令都不会被执行，执行结果报错
    + 如果在把命令压入队列的过程中正常，但在执行队列中某一个命令时报错。则此时只会影响报错的那条命令的执行，其他命令正常运行
    + 当使用watch命令时，只有当受监控的键没有被修改时，exec命令才会执行事务中的e命令；而一旦执行了exec命令，之前加的所有watch监控全部取消
+ 返回值: 
    + 数组，其中的每一个元素分别时原子化事务中的每个命令的返回值。
    + 当使用watch命令时，如果事务执行中止，那么exec命令就会返回一个Null值
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826054951.png)

** 取消事务 **
+ 语法: `discard`
+ 作用: 
    + 清除所有先前在一个事务中放入队列的命令，并且结束事务
    + 如果使用了watch命令，那么discard命令就会将当前连接监控的所有键取消监控
+ 返回值: ；清除成功，则返回OK
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826055427.png)

** 监控事务 **
+ 语法: `watch key [key...]`
+ 作用: 
    + 当某个事务需要按条件执行时，就要使用这个命令将给定的键设置为受监控的
    + 如果被监控的key值在本事务外有修改时，则本事务所有指令都不会被执行
    + watch命令相当与关系型数据库中的乐观锁
+ 返回值: 监控成功，则返回OK
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826060712.png)
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826061117.png)
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826061353.png)

** 取消监控事务 **
+ 语法: `unwatch`
+ 作用: 
    + 清除所有先前为一个事务所设置的监控键
    + 如果在watch命令之后你跳用了exec活discard命令，则不需要在手动调用unwatch命令了
+ 返回值: ；清除监控键成功，则返回OK
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826062722.png)
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826062823.png)
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826063041.png)


## Redis消息的发布与订阅

### 概述
Redis发布订阅（pub/sub）是一种消息通信模式：发送者(pub)发送消息，订阅者(sub)接收消息。Redis客户端可以订阅任意数量的频道。

### Redis发布订阅示意图

消息订阅者(client2、client5、client1), 订阅频道channel1
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826131405.png)

消息发布者发布信息到频道channel1，会被发送到三个订阅者
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826131538.png)

### Redis发布订阅的常用命令

** 订阅频道 **
+ 语法: `subscribe channel [channel...]`
+ 作用: 订阅一个或者多个频道的消息
+ 返回值: 订阅的消息
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826131834.png)

** 发布消息 **
+ 语法: `publish channel message`
+ 作用: 将消息发送到指定的频道
+ 返回值: 整数，接收到消息订阅者的数量
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826132320.png)

** 通配符订阅频道 **
+ 语法: `psubscribe pattern [pattern]`
+ 作用: 订阅一个或者多个符合给定模式的频道。模式以*作为通符，例如：news.* 匹配所有以news开头的频道
+ 返回值: 订阅的信息
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826132622.png)

## Redis的主从复制

### 概述
主机数据更新后根据配置和策略，自动同步到从机的`master/slave`机制。Master以写为主，Slave以读为主。

### 一主二从原理
1. 配从(库)不配主(库)
2. 配从(库): `slaveof 主库ip 主库端口`
3. 主写从读、读写分离
4. 主断从待命，从断重新连

### 一主二从搭建
1. 在同一个主机上模拟三台redis服务器
    + 将`redis.conf`配置文件拷贝三份，名字分别为`redis6379.conf`、`redis6380.conf`、`redis6381.conf`
    + 修改三个文件的port端口、pid文件名、日志文件名、rdb文件名
    ```conf
    如redis6380.conf配置文件的修改,其他两个配置文件的修改类似
    port 6380
    pidfile /var/run/redis_6380.pid
    logfile "6380.log"
    dbfilename dump6380.rdb
    ```
    + 分别打开三个终端窗口，开启三个redis服务
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826190608.png)
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826190654.png)
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826190741.png)

2. 查询主从信息：`info replication`
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826191421.png)
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826191651.png)
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826191753.png)

3. 在端口6379上进行写操作
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826193306.png)

4. 设置端口6380和端口6381的redis主机为端口6379主机的从机
方式一：在需要被设置为从机的redis主机命令行执行`slaveof 主机ip 主机端口`
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826211528.png)
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826211942.png)
方式二：在需要被设置为从机的redis配置文件的最后一行加上`slaveof 主机ip 主机端口`

5. 全量复制：从机都将主机上所有的数据都复制过来
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826213030.png)
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826213203.png)

6. 增量复制：从机会将主机后续写入的数据也都复制过来
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826213511.png)
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826213650.png)

7. 主写从读，主写分离：主机负责写入数据，从机负责读取数据。从机不能进行写入数据的操作，主机虽然可以进行读取的操作，但是不推荐
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826214008.png)
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826214241.png)

8. 主机宕机时，从机状态保持不变.宕机恢复后，从机依旧可以复制数据
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826214648.png)
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826214727.png)
[](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826215257.png)
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826215216.png)

10. 从机宕机时，主机的从机连接数-1，但其他从机连接状态保持不变。宕机恢复后，从机就与主机没关系了。但如果是通过配置文件进行的主机与从机的关联，那么不会失效

11. 从机当主机
    + 制造主机宕机`shutdown`和`quit`
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826230645.png)
    + 取端口6380主机的从机身份`slave no one`
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826230832.png)
    + 将另一个端口6381从机设置到端口6380主机上
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826231033.png)
    在6379主机宕机后，6380从机断开主从关系，6381开始还在原地待命；后来6380从机上位，6381投靠6380，6379主机即使回来但它已是孤寡老人，空头司令。
    一台主机配多台从机，一台从机再配多台从机，从而实现了庞大的集群架构。同时也减轻了一台主机的压力，缺点是增加了服务器间的延迟。

### 全量复制
slave启动成功连接到master后会发送一个sync命令，Master接到命令启动后台的存盘进程，同时收集所有接收到用于修改数据集命令。在后台进程执行完毕之后，master将传送整个数据文件到slave，以完成一次完全同步。slave服务在接收到数据库文件数据后，将其存盘并加载到内存中。只要是重新连接master，一次完全同步(全量复制)将被自动执行。

### 增量复制
Master将新的所有收集到的修改命令依次传给slave，完成同步

## 哨兵模式

### 哨兵模式原理
从机上位的自动版。Redis提供了哨兵的命令，哨兵命令是一个独立的进程，哨兵通过发送命令，来监控主从服务器的运行状态，如果检测到master故障了根据投票自动将某一个slave转换为master，然后通过信息订阅模式通知其他slave，让它们切换主机。然而，一个哨兵进程对Redis服务器进行监控，可能会出现问题。为此，我们可以使用多哨兵进行监控。

### 哨兵模式搭建
+ 前面和一主二从搭建一样：一台服务器模拟三台主机，查询主从信息，写操作6379，设置主从关系，全量复制，增量复制，主写从读，读写分离
+ 创建`redis_sentinel.conf`文件，并编辑里面的内容，加入`sentinel monitor dc-redis 127.0.0.1 6379 1`。指定监控主机的ip地址、port端口，得到哨兵的投票数（当哨兵投票大于或者等于此数时切换主从关系）。
+ 新开窗口，启动哨兵：`redis-sentinel /opt/redis-5.0.2/redis_sentinel.conf`
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826233818.png)
+ 让现主机宕机
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826233850.png)
+ 等待从机投票，在sentinel窗口中查看打印信息
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826233923.png)
+ 查看6380和6381的redis信息
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826233958.png)
+ 原主机恢复，启动6379
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210826234018.png)

### 小结
1. 查看主从复制关系命令: `info replication`
2. 设置主从关系命令: `slaveof 主机ip 主机prot`
3. 取消主从关系命令: `slaveof no one`
4. 开启哨兵模式命令: `./redis-sentinel sentinel.conf`
5. 主从复制原则: 开始是全量复制，之后是增量复制
6. 哨兵模式三大任务：监控、提醒、自动故障迁移
7. Redis的主从复制最大的缺点就是延迟！主机负责写，从机负责备份，这个过程有一定的延迟。当系统很繁忙的时候，延迟问题会更加严重，从机器数量的增加也会使这个问题更加严重。

# Jedis操作Redis

## 概述

使用Redis官方推荐Java使用Jedis来操作Redis数据库。在Java应用中操作Redis，Jedis几乎涵盖了Redis的所有命令，操作Redis命令在Jedis中以方法的形式出现。

## Jedis操作Key
```java
    @Test
    public void testKey() {
        //获取redis连接
        Jedis jedis = new Jedis("localhost", 6379);

        //获取数据库中所有的key值
        Set<String> keys = jedis.keys("*");
        System.out.println("获取数据库中的所有的key值：" + keys);

        System.out.println("------------分割线------------");

        //判断key是否存在
        Boolean ageSet = jedis.exists("ageSet");
        System.out.println("判断key=ageSet是否存在：" + ageSet);

        System.out.println("------------分割线------------");

        //移动指定key到指定的数据库中
        Long ageSet1 = jedis.move("ageSet", 1);
        System.out.println("移动指定key是否成功：" + ageSet1);

        System.out.println("------------分割线------------");

        //查看key剩余生存时间
        Long ageZset = jedis.ttl("ageZset");
        System.out.println("查看key=ageZset的剩余生存时间：" + ageZset);

        System.out.println("------------分割线------------");

        //设置key的最大生存时间
        Long ageZset1 = jedis.expire("ageZset", 30);
        System.out.println("设置是否成功：" + (ageZset1 == 1));

        System.out.println("------------分割线------------");

        //查看key的数据类型
        String ageZset2 = jedis.type("ageZset");
        System.out.println("查看ageZset2的数据类型：" + ageZset2);

        System.out.println("------------分割线------------");

        //重命名key
        String rename = jedis.rename("ageListNew", "ageListOld");
        System.out.println("重命名是否成功：" + rename);

        System.out.println("------------分割线------------");

        //删除指定的key
        Long ageList = jedis.del("ageList","age");
        System.out.println("删除指定的key成功的数量：" + ageList);
    }
```

### Jedis操作String类型数据
```java
    @Test
    public void testString() {
        //获取redis连接
        Jedis jedis = new Jedis("localhost", 6379);

        //保存string字符串到redis数据库
        String id = jedis.set("id", "001");
        String name = jedis.set("name", "Bernardo");
        String age = jedis.set("age", "22");
        String gender = jedis.set("gender", "man");
        System.out.println(id + "; " + name + "; " + age + "; " + gender);

        System.out.println("----------分割线----------");

        //获取key对应的value
        String id1 = jedis.get("id");
        String name1 = jedis.get("name");
        String age1 = jedis.get("age");
        String gender1 = jedis.get("gender");
        System.out.println("id=" + id1);
        System.out.println("name=" + name1);
        System.out.println("age=" + age1);
        System.out.println("gender=" + gender1);

        System.out.println("----------分割线----------");

        //追加字符串
        Long append = jedis.append("name", "Li");
        System.out.println("追加后的字符串长度：" + append);

        //获取字符串长度
        Long name2 = jedis.strlen("name");
        System.out.println("获取字符串name的字符串长度：" + name2);

        System.out.println("----------分割线----------");

        //字符串+1运算
        Long age2 = jedis.incr("age");
        System.out.println("字符串age+1运算：" + age2);

        System.out.println("----------分割线----------");

        //字符串-1运算
        Long age3 = jedis.decr("age");
        System.out.println("字符串-1运算：" + age3);

        System.out.println("----------分割线----------");

        //字符串增量运算
        Long age4 = jedis.incrBy("age", 10);
        System.out.println("字符串增量运算+10: " + age4);

        System.out.println("----------分割线----------");

        //字符串减量运算
        Long age5 = jedis.decrBy("age", 5);
        System.out.println("字符串减量运算-10：" + age5);

        System.out.println("----------分割线----------");

        //截取字符串
        String name3 = jedis.getrange("name", 1, 4);
        System.out.println("截取索引1到4的字符串：" + name3);

        System.out.println("----------分割线----------");

        //修改字符串
        Long name4 = jedis.setrange("name", 1, "GGggg");
        System.out.println("修改后的字符串长度：" + name4);

        System.out.println("----------分割线----------");

        //同时设置key的value值与生存时间
        String setex = jedis.setex("Java", 30, "idea");
        System.out.println("设置成功与否：" + setex);

        System.out.println("----------分割线----------");

        //设置不存在的key值
        Long setnx = jedis.setnx("Python", "pychon");
        System.out.println("设置成功与否：" + setnx);

        System.out.println("----------分割线----------");

        //批量设置多个key-value
        String mset = jedis.mset("book", "bird", "music", "rick");
        System.out.println("设置多个key-value值成功与否：" + mset);

        System.out.println("----------分割线----------");

        //获取多个value值
        List<String> mget = jedis.mget("book", "music");
        System.out.println("批量获取的多个value值：" + mget);

        System.out.println("----------分割线----------");

        //批量设置多个不存在的key-value
        Long msetnx = jedis.msetnx("fuck", "you", "good", "morning");
        System.out.println("批量设置多个不存在的key-value：" + msetnx);
    }
```

### Jedis操作List类型数据
```java
    @Test
    public void testList() {
        //获取jedis连接
        Jedis jedis = new Jedis("localhost", 6379);

        //在list表头插入数据
        Long lpush = jedis.lpush("ageList", "one", "two", "three", "four", "five");
        System.out.println("list列表插入后的列表长度：" + lpush);

        System.out.println("----------分割线----------");

        //在list表尾插入数据
        Long rpush = jedis.rpush("ageList", "six", "seven", "eight", "nine", "ten");
        System.out.println("list列表插入后的列表长度：" + rpush);

        System.out.println("----------分割线----------");

        //获取list列表元素
        List<String> ageList = jedis.lrange("ageList", 0, -1);
        System.out.println("获取list列表的全部元素：" + ageList);

        System.out.println("----------分割线----------");

        //弹出list表头的元素
        String ageList1 = jedis.lpop("ageList");
        System.out.println("弹出list表头的元素：" + ageList1);

        System.out.println("----------分割线----------");

        //弹出list表尾的元素
        String ageList2 = jedis.rpop("ageList");
        System.out.println("弹出list表尾的元素：" + ageList2);

        System.out.println("----------分割线----------");

        //查看list指定索引元素
        String ageList3 = jedis.lindex("ageList", 3);
        System.out.println("查看list索引为3的元素：" + ageList3);

        System.out.println("----------分割线----------");

        //获取list列表的长度
        Long ageList4 = jedis.llen("ageList");
        System.out.println("获取list列表的长度：" + ageList4);

        System.out.println("----------分割线----------");

        //删除指定个数的列表同一元素
        Long ageList5 = jedis.lrem("ageList", 3, "five");
        System.out.println("查看删除成功元素的个数：" + ageList5);

        System.out.println("----------分割线----------");

        //截取list列表
        String ageList6 = jedis.ltrim("ageList", 1, 10);
        System.out.println("截取是否成功: " + ageList6);
        List<String> ageList7 = jedis.lrange("ageList", 0, -1);
        System.out.println("查看截取后的子列表：" + ageList7);

        System.out.println("----------分割线----------");

        //修改指定list列表指定索引处的value值
        String lset = jedis.lset("ageList", 3, "fuck");
        System.out.println("设置元素是否成功：" + lset);

        System.out.println("----------分割线----------");

        //在list列表中插入数据
        Long linsert = jedis.linsert("ageList", ListPosition.BEFORE, "two", "good");
        System.out.println("list列表中插入数据后的新长度：" + linsert);
    }
```

### Jedis操作Hash类型数据
```java
    @Test
    public void testHash() {
        //获取redis连接
        Jedis jedis = new Jedis("localhost", 6379);

        //设置值到hash列表中
        jedis.hset("ageHash","one","10");
        jedis.hset("ageHash","two","20");
        jedis.hset("ageHash","three","30");
        jedis.hset("ageHash","four","40");
        jedis.hset("ageHash","five","50");

        System.out.println("----------分割线----------");

        //从hash列表中获取指定的value值
        String one = jedis.hget("ageHash", "one");
        System.out.println("从hash列表中获取指定的value值：" + one);

        System.out.println("----------分割线----------");

        //批量设置值到哈希列表中
        Map<String, String> map = new HashMap<>();
        map.put("one","Jason");
        map.put("two","Bernardo");
        map.put("three","Bob");
        map.put("four","Justin");
        map.put("five","Charlie");

        jedis.hmset("nameHash",map);

        System.out.println("----------分割线----------");

        //批量从哈希列表中获取值
        List<String> hmget = jedis.hmget("nameHash", "one", "two", "three", "four", "five");
        System.out.println("批量从哈希列表中获取值：" + hmget);

        System.out.println("----------分割线----------");

        //同时获取哈希列表的field和value
        Map<String, String> map1 = jedis.hgetAll("nameHash");
        System.out.println("同时获取哈希列表中的field和value：" + map1);

        System.out.println("----------分割线----------");

        //批量删除哈希列表中的field-value
        Long hdel = jedis.hdel("nameHash", "three", "four");
        System.out.println("成功删除的元素数量：" + hdel);

        System.out.println("----------分割线----------");

        //获取hash列表的元素个数
        Long nameHash = jedis.hlen("nameHash");
        System.out.println("获取hash列表的元素个数：" + nameHash);

        System.out.println("----------分割线----------");

        //判断hash列表中指定field元素是否存在
        Boolean hexists = jedis.hexists("nameHash", "one");
        System.out.println("判断hash列表中one元素是否存在：" + hexists);

        System.out.println("----------分割线----------");

        //查看哈希列表所有的field域
        Set<String> nameHash1 = jedis.hkeys("nameHash");
        System.out.println("查看hash列表所有的field域：" + nameHash1);

        System.out.println("----------分割线----------");

        //查看hash列表中所有的value值
        List<String> nameHash2 = jedis.hvals("nameHash");
        System.out.println("查看hash列表中所有的value值：" + nameHash2);

        System.out.println("----------分割线----------");

        //整数增量运算
        Long aLong = jedis.hincrBy("ageHash", "one", 100);
        System.out.println("整数增量运算：" + aLong);

        System.out.println("----------分割线----------");

        //浮点增量运算
        Double aDouble = jedis.hincrByFloat("aegHash", "two", 200.50);
        System.out.println("浮点增量运算：" + aDouble);

        System.out.println("----------分割线----------");

        //只设置不存在的设置到hash列表中
        Long hsetnx = jedis.hsetnx("ageHash", "six", "60");
        System.out.println("设置值到hash列表中成功与否：" + hexists);

        Long hsetnx1 = jedis.hsetnx("ageHash", "one", "100");
        System.out.println("设置值到hash列表中成功与否：" + hsetnx1);
    }
```

### Jedis操作Set类型数据
```java
package xyz.rtx3090.jedis;

import org.junit.Test;
import redis.clients.jedis.Jedis;

import java.util.List;
import java.util.Set;

public class SetTest {
    @Test
    public void testSet() {
        //获取redis连接
        Jedis jedis = new Jedis("localhost", 6379);

        //添加元素到set集合中
        Long ageSet = jedis.sadd("ageSet", "22", "23", "24", "25", "26", "27");
        System.out.println("成功添加到set集合的元素个数：" + ageSet);

        System.out.println("----------分割线----------");

        //获取set集合中所有元素
        Set<String> ageSet1 = jedis.smembers("ageSet");
        System.out.println("获取set集合中的所有元素：" + ageSet1);

        System.out.println("----------分割线----------");

        //判断指定元素是否在set集合中存在
        Boolean ageSet2 = jedis.sismember("ageSet", "22");
        System.out.println("判断元素22是否在ageSet集合中存在：" + ageSet2);
        Boolean ageSet3 = jedis.sismember("ageSet", "30");
        System.out.println("判断元素30是否在ageSet集合中存在：" + ageSet3);

        System.out.println("----------分割线----------");

        //获取set集合长度
        Long ageSet4 = jedis.scard("ageSet");
        System.out.println("获取ageSet集合的长度：" + ageSet4);

        System.out.println("----------分割线----------");

        //删除set集合中的指定元素
        Long ageSet5 = jedis.srem("ageSet", "23");
        System.out.println("删除ageSet集合中23元素：" + (ageSet5 == 1));

        System.out.println("----------分割线----------");

        //随机获取set集合中的一个或多个元素
        String ageSet6 = jedis.srandmember("ageSet");
        System.out.println("随机获取set集合中的一个元素：" + ageSet6);
        List<String> ageSet7 = jedis.srandmember("ageSet", 3);
        System.out.println("随机获取set集合中3个不会重复的元素：" + ageSet7);
        List<String> ageSet8 = jedis.srandmember("ageSet", -3);
        System.out.println("随机获取set集合中的3个可能重复的元素：" + ageSet8);

        System.out.println("----------分割线----------");

        //随机弹出set集合中的一个元素
        String ageSet9 = jedis.spop("ageSet");
        System.out.println("随机弹出set集合中的一个元素：" + ageSet9);

        System.out.println("----------分割线----------");

        //将指定元素在两个set集合之间移动
        Long smove = jedis.smove("ageSet", "ageSet02", "25");
        System.out.println("将指定元素在两个set集合之间移动：" + smove);
        Set<String> ageSet02 = jedis.smembers("ageSet02");
        System.out.println("移动后ageSet02中的元素：" + ageSet02);

        System.out.println("----------分割线----------");

        //获取集合之间的差集
        Set<String> sdiff = jedis.sdiff("ageSet", "ageSet02");
        System.out.println("获取两个或者多个set集合之间的差集：" + sdiff);

        System.out.println("----------分割线----------");

        //获取集合之间的交集
        Long ageSet021 = jedis.sadd("ageSet02", "22");
        Set<String> sinter = jedis.sinter("ageSet", "ageSet02");
        System.out.println("获取两个或者多个set集合之间的交集：" + sinter);

        System.out.println("----------分割线----------");

        //获取集合之间的并集
        Set<String> sunion = jedis.sunion("ageSet", "ageSet02");
        System.out.println("获取两个或者多个set集合之间的并集：" + sunion);

        System.out.println("----------分割线----------");

    }
}
```

### Jedis操作Zset类型数据
```java
package xyz.rtx3090.jedis;

import org.junit.Test;
import redis.clients.jedis.Jedis;

import java.util.Set;

public class ZsetTest {

    @Test
    public void testZset01() {
        //获取redis连接
        Jedis jedis = new Jedis("localhost",6379);

        //添加元素到zset集合
        jedis.zadd("ageZset",10,"one");
        jedis.zadd("ageZset",20,"two");
        jedis.zadd("ageZset",30,"five");
        jedis.zadd("ageZset",40,"four");
        jedis.zadd("ageZset",50,"three");

        //获取zset集合中到全部元素
        Set<String> ageZset = jedis.zrange("ageZset", 0, -1);
        for (String age :
                ageZset) {
            System.out.println(age);
        }

        System.out.println("--------分割线--------");

        //查看zset有序集合指定score区间的元素
        Set<String> ageZset1 = jedis.zrangeByScore("ageZset", 20, 40);
        for (String age :
                ageZset1) {
            System.out.println(age);
        }

        //删除zset有序集合中的指定元素
        Long zrem = jedis.zrem("ageZset", "five");
        System.out.println(zrem == 1 ? "删除成功" : "删除失败");

        System.out.println("--------分割线--------");

        //获取zset有序集合的元素个数
        Long ageZset2 = jedis.zcard("ageZset");
        System.out.println("zset有序集合的元素个数：" + ageZset2);

        System.out.println("--------分割线--------");

        //获取zset有序集合指定score区间的元素个数
        Long ageZset3 = jedis.zcount("ageZset", 20, 40);
        System.out.println("zset有序集合score值20到40之间的元素个数：" + ageZset3);

        System.out.println("--------分割线--------");

        //获取zset有序集合指定元素的排名
        Long zrank = jedis.zrank("ageZset", "four");
        System.out.println("zset有序集合元素four的排名：" + zrank);

        System.out.println("--------分割线--------");

        //获取有序集合指定元素的分数
        Double zscore = jedis.zscore("ageZset", "four");
        System.out.println("zset有序集合中元素four对应score：" + zscore);

        System.out.println("--------分割线--------");

        //获取zset有序集合中元素的反向排名
        Long zrevrank = jedis.zrevrank("ageZset", "four");
        System.out.println("zset有序集合中指定元素的反向排名：" + zrevrank);

        System.out.println("--------分割线--------");

        //逆向查询zset有序集合索引区间内的元素
        Set<String> ageZset4 = jedis.zrevrange("ageZset", 0, -1);
        System.out.println("zset有序集合指定索引区间内的元素：" + ageZset4);

        System.out.println("--------分割线--------");

        //逆向获取zset有序集合指定score区间内的元素
        Set<String> ageZset5 = jedis.zrevrangeByScore("ageZset", 40, 20);
        System.out.println("逆向获取zset有序集合score值在20到40之间的元素：" + ageZset5);

        System.out.println("--------分割线--------");
    }
}
```
