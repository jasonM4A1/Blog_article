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
    + 语法: `itrim key startIndex endIndex`
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
