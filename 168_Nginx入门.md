# Nginx简介

## 背景介绍
Nginx（“engine x”）一个具有高性能的【HTTP】和【反向代理】的【WEB服务器】，同时也是一个【POP3/SMTP/IMAP代理服务器】，是由伊戈尔·赛索耶夫(俄罗斯人)使用C语言编写的，Nginx的第一个版本是2004年10月4号发布的0.1.0版本。另外值得一提的是伊戈尔·赛索耶夫将Nginx的源码进行了开源，这也为Nginx的发展提供了良好的保障。

## 名词解释

### WEB服务器
WEB服务器也叫网页服务器，英文名叫Web Server，主要功能是为用户提供网上信息浏览服务。

### HTTP
HTTP是超文本传输协议的缩写，是用于从WEB服务器传输超文本到本地浏览器的传输协议，也是互联网上应用最为广泛的一种网络协议。HTTP是一个客户端和服务器端请求和应答的标准，客户端是终端用户，服务端是网站，通过使用Web浏览器、网络爬虫或者其他工具，客户端发起一个到服务器上指定端口的HTTP请求。

### POP3/SMTP/IMAP
1. `POP3`: Post Offic Protocol 3,邮局协议的第三个版本
2. `SMTP`: Simple Mail Transfer Protocol, 简单邮件传输协议
3. `IMAP`: Internet Mail Access Protocol, 交互式邮件存取协议
> 通过上述名词的解释，我们可以了解到Nginx也可以作为电子邮件代理服务器

### 正反向代理
1. 正向代理
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210903132618.png)

2. 反向代理
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210903132650.png)

# 常见服务器

## 概述

在介绍这一节内容之前，我们先来认识一家公司叫Netcraft。
> Netcraft公司于1994年底在英国成立，多年来一直致力于互联网市场以及在线安全方面的咨询服务，其中在国际上最具影响力的当属其针对网站服务器、SSL市场所做的客观严谨的分析研究，公司官网每月公布的调研数据（Web Server Survey）已成为当今人们了解全球网站数量以及服务器市场分额情况的主要参考依据，时常被诸如华尔街杂志，英国BBC，Slashdot等媒体报道或引用。

我们先来看一组数据，我们先打开Nginx的官方网站  <http://nginx.org/>,找到Netcraft公司公布的数据，对当前主流服务器产品进行介绍。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210903133108.png)

上面这张图展示了2019年全球主流Web服务器的市场情况，其中有Apache、Microsoft-IIS、google Servers、Nginx、Tomcat等，而我们在了解新事物的时候，往往习惯通过类比来帮助自己理解事物的概貌。所以下面我们把几种常见的服务器来给大家简单介绍下：

## IIS
全称(Internet Information Services)即互联网信息服务，是由微软公司提供的基于windows系统的互联网基本服务。windows作为服务器在稳定性与其他一些性能上都不如类UNIX操作系统，因此在需要高性能Web服务器的场合下，IIS可能就会被"冷落".

## Tomcat
Tomcat是一个运行Servlet和JSP的Web应用软件，Tomcat技术先进、性能稳定而且开放源代码，因此深受Java爱好者的喜爱并得到了部分软件开发商的认可，成为目前比较流行的Web应用服务器。但是Tomcat天生是一个重量级的Web服务器，对静态文件和高并发的处理比较弱。

## Apache
Apache的发展时期很长，同时也有过一段辉煌的业绩。从上图可以看出大概在2014年以前都是市场份额第一的服务器。Apache有很多优点，如稳定、开源、跨平台等。但是它出现的时间太久了，在它兴起的年代，互联网的产业规模远远不如今天，所以它被设计成一个重量级的、不支持高并发的Web服务器。在Apache服务器上，如果有数以万计的并发HTTP请求同时访问，就会导致服务器上消耗大量能存，操作系统内核对成百上千的Apache进程做进程间切换也会消耗大量的CUP资源，并导致HTTP请求的平均响应速度降低，这些都决定了Apache不可能成为高性能的Web服务器。这也促使了Lighttpd和Nginx的出现。

## Lighttpd
Lighttpd是德国的一个开源的Web服务器软件，它和Nginx一样，都是轻量级、高性能的Web服务器，欧美的业界开发者比较钟爱Lighttpd,而国内的公司更多的青睐Nginx，同时网上Nginx的资源要更丰富些。

## 其他服务器
Google Servers，Weblogic, Webshpere(IBM)...
经过各个服务器的对比，种种迹象都表明，Nginx将以性能为王。这也是我们为什么选择Nginx的理由。

## Nginx

### 优点
1. 速度更快、并发更高
    单次请求或者高并发请求的环境下，Nginx都会比其他Web服务器响应的速度更快。一方面在正常情况下，单次请求会得到更快的响应，另一方面，在高峰期(如有数以万计的并发请求)，Nginx比其他Web服务器更快的响应请求。Nginx之所以有这么高的并发处理能力和这么好的性能原因在于Nginx采用了多进程和I/O多路复用(epoll)的底层实现。
2. 配置简单，扩展性强
    Nginx的设计极具扩展性，它本身就是由很多模块组成，这些模块的使用可以通过配置文件的配置来添加。这些模块有官方提供的也有第三方提供的模块，如果需要完全可以开发服务自己业务特性的定制模块。
3. 高可靠性
    Nginx采用的是多进程模式运行，其中有一个master主进程和N多个worker进程，worker进程的数量我们可以手动设置，每个worker进程之间都是相互独立提供服务，并且master主进程可以在某一个worker进程出错时，快速去"拉起"新的worker进程提供服务。
4. 热部署
    现在互联网项目都要求以7x24小时进行服务的提供，针对于这一要求，Nginx也提供了热部署功能，即可以在Nginx不停止的情况下，对Nginx进行文件升级、更新配置和更换日志文件等功能。
5. 成本低、BSD许可证
    BSD是一个开源的许可证，世界上的开源许可证有很多，现在比较流行的有六种分别是GPL、BSD、MIT、Mozilla、Apache、LGPL。这六种的区别是什么，我们可以通过下面一张图来解释下：
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210903134140.png)
    Nginx本身是开源的，我们不仅可以免费的将Nginx应用在商业领域，而 且还可以在项目中直接修改Nginx的源码来定制自己的特殊要求。这些 点也都是Nginx为什么能吸引无数开发者继续为Nginx来贡献自己的智慧 和青春。OpenRestry [Nginx+Lua] Tengine[淘宝]

### 功能特性
Nginx提供的基本功能服务从大体上归纳为"基本HTTP服务"、“高级HTTP服务”和"邮件服务"等三大类。

#### 基本HTTP服务
Nginx可以提供基本HTTP服务，可以作为HTTP代理服务器和反向代理服务器，支持通过缓存加速访问，可以完成简单的负载均衡和容错，支持包过滤功能，支持SSL等。
+ 处理静态文件、处理索引文件以及支持自动索引；
+ 提供反向代理服务器，并可以使用缓存加上反向代理，同时完成负载均衡和容错；
+ 提供对FastCGI、memcached等服务的缓存机制，，同时完成负载均衡和容错;
+ 使用Nginx的模块化特性提供过滤器功能。Nginx基本过滤器包括gzip压缩、ranges支持、chunked响应、XSLT、SSI以及图像缩放等。其中针对包含多个SSI的页面，经由FastCGI或反向代理，SSI过滤器可以并行处理;
+ 支持HTTP下的安全套接层安全协议SSL;
+ 支持基于加权和依赖的优先权的HTTP/2;

#### 高级HTTP服务
+ 支持基于名字和IP的虚拟主机设置
+ 支持HTTP/1.0中的KEEP-Alive模式和管线(PipeLined)模型连接
+ 自定义访问日志格式、带缓存的日志写操作以及快速日志轮转
+ 提供3xx~5xx错误代码重定向功能
+ 支持重写（Rewrite)模块扩展
+ 支持重新加载配置以及在线升级时无需中断正在处理的请求
+ 支持网络监控
+ 支持FLV和MP4流媒体传输

#### 邮件服务
Nginx提供邮件代理服务也是其基本开发需求之一，主要包含以下特性：
+ 支持IMPA/POP3代理服务功能
+ 支持内部SMTP代理服务功能

> **Nginx常用的功能模块**
> 静态资源部署
> Rewrite地址重写
> 反向代理
> 负载均衡
> Web缓存
> 环境部署
> 用户认证模块...

> **Nginx的核心组成**
> nginx二进制可执行文件
> nginx.conf配置文件
> error.log错误的日志记录
> access.log访问日志记录

# Nginx环境准备

## 服务器环境准备
1. 确保centos内容为2.6以上，可以使用`uname -a`命令来查询linux的内核版本。
2. 确保centos可以联网，使用命令`ping www.baidu.com`
3. 确保关闭防火墙
    + `systemctl stop firewalld`: 关闭运行的防火墙，系统重新启动后，防火墙将重新打开
    + `systemctl disable firewalld`: 永久关闭防火墙，，系统重新启动后，防火墙依然关闭
    + `systemctl status firewalld`: 查看防火墙状态
4. 确保停用selinux，修改`/etc/selinux/config`文件，将SELINUX=enforcing，改为SELINUX=disabled, 然后重启系统生效
5. 确保相关库都已安装，执行`yum install -y gcc pcre pcre-devel zlib zlib-devel openssl openssl-devel`来安装全部相关库

## 下载Nginx
打开网站`http://nginx.org/download/`，就可以查看到Nginx的所有版本，选中自己需要的版本进行下载。下载我们可以直接在windows上下载然后上传到服务器，也可以直接从服务器上下载，这个时候就需要准备一台服务器。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210903142019.png)

## 安装Nginx

### 方式一：源码简单安装
1. 将本机下载好的nginx源码包上传至服务器
    ```bash
    cd /usr/local/tools
    rz -y #选择本机的nginx源码包
    ```
2. 解压包
    ```bash
    tar -zxvf nginx-1.16.1.tar.gz
    ```
3. 进行安装
    ```bash
    cd nginx-1.16
    
    ./configure --prefix=/usr/local/nginx #指定安装位置
    
    make $ make install
    ```

### 方式二：源码复杂安装
1. 将本机下载好的nginx源码包上传至服务器
    ```bash
    cd /usr/local/tools
    rz -y #选择本机的nginx源码包
    ```
2. 解压包
    ```bash
    tar -zxvf nginx-1.16.1.tar.gz
    ```
3. 进行安装（简单复杂安装差异）
    ```bash
    cd nginx-1.16
    
    ./configure --prefix=/usr/local/nginx \
    --sbin-path=/usr/local/nginx/sbin/nginx \
    --modules-path=/usr/local/nginx/modules \
    --conf-path=/usr/local/nginx/conf/nginx.conf \
    --error-log-path=/usr/local/nginx/logs/error.log \
    --http-log-path=/usr/local/nginx/logs/access.log \
    --pid-path=/usr/local/nginx/logs/nginx.pid \
    --lock-path=/usr/local/nginx/logs/nginx.lock
    
    make & make install
    ```
    这种方式和简单的安装配置不同的地方在三步，通过`./configure`来对编译参数进行设置，需要我们手动来指定。那么都有哪些参数可以进行设置，接下来我们进行一个详细的说明。

PATH:是和路径相关的配置信息

with:是启动模块，默认是关闭的

without:是关闭模块，默认是开启的

我们先来认识一些简单的路径配置已经通过这些配置来完成一个简单的编译：

+ `--prefix=PATH`: 指向Nginx的安装目录，默认值为/usr/local/nginx   
+ `--sbin-path=PATH`: 指向(执行)程序文件(nginx)的路径,默认值为<prefix>/sbin/nginx
+ `--modules-path=PATH`: 指向Nginx动态模块安装目录，默认值为<prefix>/modules
+ `--conf-path=PATH`: 指向配置文件(nginx.conf)的路径,默认值为<prefix>/conf/nginx.conf
+ `--error-log-path=PATH`: 指向错误日志文件的路径,默认值为<prefix>/logs/error.log
+ `--http-log-path=PATH`: 指向访问日志文件的路径,默认值为<prefix>/logs/access.log
+ `--pid-path=PATH`: 指向Nginx启动后进行ID的文件路径，默认值为<prefix>/logs/nginx.pid
+ `--lock-path=PATH`: 指向Nginx锁文件的存放路径,默认值为<prefix>/logs/nginx.lock

### 方式三：yum安装
使用源码进行简单安装，我们会发现安装的过程比较繁琐，需要提前准备GCC编译器、PCRE兼容正则表达式库、zlib压缩库、OpenSSL安全通信的软件库包，然后才能进行Nginx的安装。
1. 安装yum-utils
    ```bash
    sudo yum  install -y yum-utils
    ```
2. 添加yum源文件
    ```bash
    vim /etc/yum.repos.d/nginx.repo
    ```
    ```repo
    [nginx-stable]
    name=nginx stable repo
    baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
    gpgcheck=1
    enabled=1
    gpgkey=https://nginx.org/keys/nginx_signing.key
    module_hotfixes=true
    
    [nginx-mainline]
    name=nginx mainline repo
    baseurl=http://nginx.org/packages/mainline/centos/$releasever/$basearch/
    gpgcheck=1
    enabled=0
    gpgkey=https://nginx.org/keys/nginx_signing.key
    module_hotfixes=true
    ```
3. 使用yum安装nginx
    ```bash
    yun install -y nginx
    ```
4. 查看nginx的安装位置
    ```bash
    whereis nginx
    ```
### 源码简单安装和yum安装的差异
开门见山，源码简单安装与yum安装的差异就在于：安装时进行的配置不同！
关于在这个差异的问题，我们可以使用源码复杂安装指定我们的相关配置来弥补。
我们使用命令`./nginx -V`（要进入相应目录）来查看安装nginx的版本及其相关配置信息

+ 简单安装
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210903231819.png)
    
+ yum安装
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210903232037.png)

## 卸载Nginx
1. 需要将nginx的进程关闭
    ```bash
    # 进入nginx下的sbin目录
    
    ./nginx -s stop
    ```
2. 删除安装的nginx文件夹
    ```bash
    rm -rf /usr/local/nginx
    ```
3. 将安装包之前编译的环境清除掉(如果不再使用这个安装包，就不需要进行此步)
    ```bash
    # 进入nginx解压后的安装包
    
    make clean
    ```

## 启动nginx
1. 首先cd到nginx根目录下的sbin目录（yum安装则cd到/usr/sbin）
    ```bash
    /usr/local/nginx/sbin
    ```
2. 执行nginx命令，来启动nginx
    ```bash
    ./nginx
    ```
3. 查看nginx是否启动成功
    ```bash
    ps -ef | grep nginx
    ```
## 停止nginx

### 硬停止(推荐)
硬停止会直接杀死nginx进程及其子进程，不管数据是否接受完毕
+ 优点：速度快，不需要等待
+ 缺点：有数据丢失的风险

#### 方式一
直接执行nginx封装好的命令来停止nginx
    ```bash
    cd /usr/local/nginx/sbin

    ./nginx -s stop
    ```

#### 方式二
1. 先查看nginx-master进程对应的pid
    ```bash
    ps -ef | grep nginx
    ```
2. 执行命令杀死nginx进程
    ```bash
    kill -TERM 6963 # 6963为nginx的PID 
    ```

### 软停止
软停止会让nginx不再接收新的请求，但是会等待nginx处理完已接收的请求，然后再杀死nginx及其子进程

1. 先查看nginx-master进程对应的pid
    ```bash
    ps -ef | grep nginx
    ```
2. 执行命令杀死nginx进程
    ```bash
    kill -QUIT 6963 #6963为nginx的PID
    ```

## 重新加载Nginx
当我们修改Nginx的配置文件后，我们需要重加载Nginx，以使修改的配置文件生效(注意这并不是真正意义上的重启)

1. 首先cd到nginx的sbin目录
    ```bash
    cd /usr/local/nginx/sbin
    ```
2. 根据配置文件重新加载Nginx
    ```bash
    ./nginx -s reload
    ```

## 重启Nginx
Nginx没有直接重新启动的命令，上面的`-s reload`也只是根据配置文件重新加载nginx，而不是重启nginx。
所以为了实现重启Nginx，我们只能笨重的先关闭Nginx，然后再启动Nginx

1. 首先停止Nginx
    ```bash
    /usr/local/nginx/sbin/nginx -s stop
    ```
2. 然后再次启动Nginx
    ```bash
    /usr/local/nginx/sbin/nginx
    ```

# Nginx自身相关命令

此方式是通过Nginx安装目录下的sbin下的可执行文件nginx来进行Nginx状态的控制，我们可以通过`nginx -h`来查看都有哪些参数可以用：
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210904023430.png)

+ `-?和-h`: 显示帮助信息
+ `-v`: 打印版本号信息并退出
+ `-V`: 打印版本号信息和配置信息并退出
+ `-t`: 测试nginx的配置文件语法是否正确并退出
+ `-T`: 测试nginx的配置文件语法是否正确并列出用到的配置文件信息然后退出
+ `-q`: 在配置测试期间禁止显示非错误消息
+ `-s`: signal信号，后面可以跟下列值
    + `stop`: 快速关闭，类似于TERM/INT信号的作用
    + `quit`: 优雅的关闭，类似于QUIT信号的作用
    + `reload`: 重新打开日志文件类似于USR1信号的作用
    + `reload`: 根据配置文件重启nginx，类似于HUP信号的作用
+ `-p`: prefix，指定Nginx的prefix路径，(默认为: /usr/local/nginx/)
+ `-c`: filename,指定Nginx的配置文件路径,(默认为: conf/nginx.conf)
+ `-g`: 用来补充Nginx配置文件，向Nginx服务指定启动时应用全局的配置

# Nginx用到的linxu相关停止命令

## 概述

我们使用命令`ps -ef | grep nginx`查看linux服务器中nginx的进程，如下图：
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210904014744.png)

从上图中可以看到,Nginx后台进程中包含一个master进程和多个worker进程，master进程主要用来管理worker进程，包含接收外界的信息，并将接收到的信号发送给各个worker进程，监控worker进程的状态，当worker进程出现异常退出后，会自动重新启动新的worker进程。而worker进程则是专门用来处理用户请求的，各个worker进程之间是平等的并且相互独立，处理请求的机会也是一样的。nginx的进程模型，我们可以通过下图来说明下：
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210904014907.png)

我们现在作为管理员，只需要通过给master进程发送信号就可以来控制 Nginx,这个时候我们需要有两个前提条件，一个是要获取master进程PID号，一个是给予master进程信号。

## 获取PID号
要想操作Nginx的master进程，就需要获取到master进程的PID号。简单介绍两个获取进程PID号的
1. 执行命令`ps -ef | grep nginx`来伙同nginx-master进程PID号
2. 查看nginx目录下的logs目录中的nginx.pid文件来查看nginx-master进程PID号

## 给予信号
我们通过命令`kill -signal PID`来给予对应nginx进程信号，下面表格为`signal`的可选值

|信号     |作用     |
|   ----  |   ----  |
|TERM/INT |立即关闭整个服务|
|QUIT     |等数据接收完毕关闭服务|
|HUP      |重读配置文件并使用服务对新配置项生效|
|USR1     |重写打开日志文件，可以用来进行日志切割|
|USR2     |平滑升级到最新版的nginx|
|WHINCH   |所有子进程不再接收处理新连接，相当于给work进程发送QUIT指令|

1. 立即关闭nginx服务
    ```bash
    kill -TERM PID
    
    kill -INT PID
    ```
2. 等待处理完所有请求，再关闭nginx服务
    ```bash
    kill -QUIT PID
    ```
3. 等待处理完所有请求，再根据配置文件重启nginx服务
    ```bash
    kill -HUP PID
    ```
4. 重新创建开启日志文件
    ```bash
    kill -USR1 PID
    ```
5. 复制原来的nginx进程，根据`nginx`可执行文件创造出新的nginx进程，旧nginx进程保持不变
    ```bash
    kill -USR2 PID
    ```
6. 等待处理完所有请求，关闭所有nginx子进程(work进程)
    ```bash
    kill -WINCH PID
    ```

# Nginx安装后目录结构分析

在使用Nginx之前，我们先对安装好的Nginx目录文件进行一个分析，在这块给大家介绍一个工具tree，通过tree我们可以很方面的去查看centos系统上的文件目录结构，当然，如果想使用tree工具，就得先通过`yum install -y tree`来进行安装，安装成功后，可以通过执行`tree /usr/local/nginx`(tree后面跟的是Nginx的安装目录)，获取的结果如下：
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210903225600.png)

## conf目录
**conf目录是nginx所有配置文件的总目录**

> CGI(Common Gateway Interface)通用网关【接口】，主要解决的问题 是从客户端发送一个请求和数据，服务端获取到请求和数据后可以调用 调用CGI【程序】处理及相应结果给客户端的一种标准规范. 而fastcgi则是它改进版本

+ `mime.types`: 记录的是HTTP协议中的Content-Type的值和文件后缀名的对应关系
+ `nginx.conf`: Nginx的核心配置文件，这个文件非常重要，也是我们即将要学习的重点
+ `koi-utf`、`koi-win`、`win-utf`: 这三个文件都是与编码转换映射相关的配置文 件，用来将一种编码转换成另一种编码
> 后缀为.default的文件都备份文件
> 其他目录我们无需了解

## html目录
**存放nginx自带的两个静态的html页面**

+ `50x.html`: 访问失败后的失败页面
+ `index.html`: 成功访问的默认首页

## logs目录
**日志文件的总目录**

+ `access.log`: 每次成功访问nginx都会在此文件生成记录
+ `error.log`: 每次nginx访问出错，都会在此文件生成记录
+ `nginx.pid`: nginx进程pid的记录值文件

## sbin目录
**用来存放可执行文件的总目录**

+ `nginx`: 用来控制Nginx的启动和停止等相关的命令

# Nginx原解压目录结构分析
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210903232941.png)

+ `auto`: 存放的是编译相关的脚本
+ `CHANGES`: 版本变更记录
+ `CHANGES.ru`: 俄罗斯文的版本变更记录
+ `conf`: 存放nginx默认的配置文件
+ `configure`: nginx软件的自动脚本程序,是一个比较重要的文件，作用如下:
    + 检测环境及根据环境检测结果生成C代码
    + 生成编译代码需要的Makefile文件
+ `contrib`: 存放的是几个特殊的脚本文件，其中README中对脚本有着详细的说明
+ `html`: 存放的是Nginx自带的两个html页面，访问Nginx的首页和错误页面
+ `LICENSE`: 许可证的相关描述文件
+ `man`: nginx的man手册
+ `README`: Nginx的阅读指南
+ `src`: Nginx的源代码

# Nginx服务器版本升级和新增模块

## 概述

如果想对Nginx的版本进行更新，或者要应用一些新的模块，最简单的做法就是停止当前的Nginx服务，然后开启新的Nginx服务。但是这样会导致在一段时间内，用户是无法访问服务器。为了解决这个问题，我们就需要用到Nginx服务器提供的平滑升级功能。这个也是Nginx的一大特点，使用这种方式，就可以使Nginx在7x24小时不间断的提供服务了。接下来我们分析下需求：
+ Nginx的版本最开始使用的是Nginx-1.14.2,由于服务升级，需要将Nginx的版本升级到Nginx-1.16.1,要求Nginx不能中断提供服务。

为了应对上述的需求，这里我们给大家提供两种解决方案:
1. 方案一:使用Nginx服务信号完成Nginx的升级
2. 方案二:使用Nginx安装目录的make命令完成升级

## 实现

### 环境准备
1. 先准备两个版本的Nginx分别是 1.14.2和1.16.1
2. 使用Nginx源码安装的方式将1.14.2版本安装成功并正确访问
    ```bash
    # 首先解压并进入nginx1.14.2的安装包
    
    # 配置
    ./configure
    
    # 编译&安装
    make & make install
    
    # 运行
    /usr/local/nginx/sbin/nginx
    ```
3. 将Nginx1.16.1进行参数配置和编译，不需要进行安装
    ```bash
    # 首先解压并进入nginx1.14.2的安装包
    
    # 配置
    
    # 编译
    make
    ```

### 方案一：使用Nginx服务信号进行升级
1. 将1.14.2版本的sbin目录下的nginx进行备份
    ```bash
    cd /usr/local/nginx/sbin
    
    mv nginx nginx_old
    ```
2. 将Nginx1.16.1安装目录编译后的objs目录下的nginx文件，拷贝到原来1.14.2`/usr/local/nginx/sbin`目录下
    ```bash
    cd ~/nginx/core/nginx-1.16.1/objs
    
    cp nginx /usr/local/nginx/sbin
    ```
3. 发送信号USR2给Nginx的1.14.2版本对应的master进程，根据`nginx`可执行文件复制创建一个新的进程
    ```bash
    kill -USR2 `cat /usr/local/nginx/logs/nginx.pid`
    ```
4. 发送信号QUIT给Nginx的1.14.2版本对应的master进程, 关闭这个1.14.2的老进程
    ```bash
    kill -QUIT `cat /usr/local/nginx/logs/nginx.pid.oldbin`
    ```
5. 查看现在Nginx运行的版本,是否为1.16.1版本
    ```bash
    cd /usr/local/nginx/sbin
    
    ./nginx -v
    ```

### 方案二：使用Nginx安装目录的make命令完成升级
1. 将1.14.2版本的sbin目录下的nginx进行备份
    ```bash
    cd /usr/local/nginx/sbin
    
    mv nginx nginx_old
    ```
2. 将Nginx1.16.1安装目录编译后的objs目录下的nginx文件，拷贝到原来1.14.2`/usr/local/nginx/sbin`目录下
    ```bash
    cp /usr/local/core/nginx-1.16.1/objs/nginx /usr/local/nginx/sbin
    ```
3. 进入到1.16.1解压后的安装包的根目录中,执行`make upgrade`命令，进行升级(这一步其实就是包括了方案一的第三四步)
    ```bash
    cd /usr/local/core/nginx-1.16.1
    
    make upgrade
    ```
4. 查看现在Nginx运行的版本,是否为1.16.1版本
    ```bash
    /usr/local/nginx/sbin/nginx -v
    ```

# Nginx核心配置文件nginx.conf

## 概述

Nginx的核心配置文件存放在`/usr/local/nginx/conf/nginx.conf`中，下面为去除原注释，加上中文注释解释的`nginx.conf`文件内容：
```conf
worker_processes  1;

# events块：主要用于设置Nginx服务器与用户的网络连接，这一部分对Nginx服务器的性能影响较大
events {
    worker_connections  1024;
}

# http块：是Nginx服务器配置中很重要的部分，用于代理、缓存、日志记录、第三方模块配置等
http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;

    # server块：是Nginx配置和虚拟主机相关的内容
    server {
        # 指令名     # 指令值
        listen       80;
        server_name  localhost;

        # location块：基于Nginx服务器接收请求字符串与location后面的值进行匹配，对特定请求进行处理
        location / {
        # 指令名   # 指令值
            root   html;
            index  index.html index.htm;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }

}
```

Nginx.conf配置文件中默认分为三大块：
+ 全局块
+ events块
+ http块

http块中可以配置多个server块，每个server块又可以配置多个location块

## 指令
在nginx.conf配置文件中，我们在http块中可以看到这样的内容：
```conf
server {
    # 指令名     # 指令值
    listen       80;
    server_name  localhost;
    ......
}
```
这里的listen就是指令名，而80为指令值。
同理下面的server_name也为指令名，localhost为指令值。

全局块、events块中关于指令名与指令值的定义也是如此。

## 全局块

下面为nginx.conf配置文件，全局块的默认内容：
```bash
#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;
```

### user指令
```conf
user  jason;
```

+ 位置: user指令位于全局块中
+ 默认值: 默认是被注释掉的，初始化的默认值为nobody，即不需要登录任何用户，我们也可以手动设置为其他用户
+ 作用: 用于配置运行Nginx服务器的work进程的用户及用户组, 对于系统的权限访问控制的更加精细，也更加安全
> 该属性也可以在编译的时候指定，语法如下`./configure --user=user --group=group`,如果两个地方都进行了设置，最终生效的是配置文件中的配置

**使用演示**
1. 在linux服务器中创建一个jason用户
    ```bash
    useradd jason
    ```
2. 打开nginx的nginx.conf配置文件，设置user指令
    ```conf
    user jason
    ```
3. 在linux服务器中创建`root/html/index.html`文件
    ```html
    <!DOCTYPE html>
    <html>
    <head>
    <title>Welcome to nginx!</title>
    <style>
        body {
            width: 35em;
            margin: 0 auto;
            font-family: Tahoma, Verdana, Arial, sans-serif;
        }
    </style>
    </head>
    <body>
    <h1>Welcome to nginx!</h1>
    <p>If you see this page, the nginx web server is successfully installed and
    working. Further configuration is required.</p>
    
    <p>For online documentation and support please refer to
    <a href="http://nginx.org/">nginx.org</a>.<br/>
    Commercial support is available at
    <a href="http://nginx.com/">nginx.com</a>.</p>
    
    <p><em>Thank you for using nginx.</em></p>
    <p><em>I am jason</em></p>
    </body>
    </html>
    ```
4. 重新打开nginx.conf配置文件，修改location块内容
    ```conf
    location / {
        root   /root/html; #原内容为：html
        index  index.html index.htm;
    }
    ```
5. 根据配置文件重启nginx服务，并进行访问测试，结果访问失败
    ```bash
    # 重启nginx
    /usr/local/nginx/sbin/nginx -s reload
    ```
6. 访问失败的原因分析
    当前nginx-work进程是运行在jason这个普通用户上的，而nginx.conf配置文件中设置的资源访问路径为`/root/html`。
    我们知道普通用户是没有办法访问root用户目录下的文件，所以我们在进行访问测试时失败了。
7. 我们再次修改nginx.conf配置文件，将资源目录设置为用户jason的家目录下的html目录, 这个目录jason是有权限访问的
    ```conf
    location / {
        root   /home/jason/html; #原内容为：/root/html
        index  index.html index.htm;
    }
    ```
8. 在linux服务器创建`/home/jason/html/index.html`文件，内容与`/root/html/index.html`一致
    ```html
    <!DOCTYPE html>
    <html>
    <head>
    <title>Welcome to nginx!</title>
    <style>
        body {
            width: 35em;
            margin: 0 auto;
            font-family: Tahoma, Verdana, Arial, sans-serif;
        }
    </style>
    </head>
    <body>
    <h1>Welcome to nginx!</h1>
    <p>If you see this page, the nginx web server is successfully installed and
    working. Further configuration is required.</p>
    
    <p>For online documentation and support please refer to
    <a href="http://nginx.org/">nginx.org</a>.<br/>
    Commercial support is available at
    <a href="http://nginx.com/">nginx.com</a>.</p>
    
    <p><em>Thank you for using nginx.</em></p>
    <p><em>I am jason</em></p>
    </body>
    </html>
    ```
9. 根据配置文件重启nginx服务,并进行访问测试，结果成功
    ```bash
    /usr/local/nginx/sbin/nginx -s reload
    ```

根据上述案例的演示，我们明确的知道`user指令`的作用就是设置启动运行工作进程的用户及用户组，使用nginx对于对于系统的权限访问控制的更加精细，也更加安全

### master_process指令
```conf
# on表示开启worker进程，off表示关闭worker进程
master_process on;
```

+ 语法: master_process on|off;
+ 位置: 全局块中
+ 默认值: on, 即开启worker进程
+ 作用: 用于指定是否开启worker进程

**使用演示**
1. 设置nginx.conf配置文件中master_process指令值为off
    ```conf
    master_process off;
    ```
2. 重新启动nginx，使其配置生效
    ```bash
    # 停止nginx
    /usr/local/nginx/sbin/nginx -s stop
    
    # 启动nginx
    /usr/local/nginx/sbin/nginx
    ```
3. 查看nginx的worker进程数量,发现nginx的worker进程一个都没有了
    ```bash
    ps -ef | grep nginx
    ```
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210904140936.png)

### work_processes指令
```conf
# 1表示开启一个worker进程
worker_processes  1;
```

+ 语法: worker_processes num;
+ 位置: 全局块中
+ 默认值: 1, 即开启一个worker进程
+ 作用: 用于指定开启worker进程的数量

**使用演示**
1. 设置nginx.conf配置文件中worker_processes指令值为2
    ```conf
    worker_processes  2;
    ```
2. 根据配置文件重新加载nginx
    ```conf
    /usr/local/nginx/sbin/nginx -s reload
    ```
3. 查看nginx的worker进程数量,发现nginx的worker进程成为了两个
    ```bash
    ps -ef | grep nginx
    ```
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210904134912.png)

### daemon指令
```conf
daemon on;
```

+ 语法: daemon on|off;
+ 位置: 全局块中
+ 默认值: on, 即以守护进程的方式运行nginx。修改为off，则表示不以守护进程的方式运行nginx
+ 作用: 设定Nginx是否以守护进程的方式启动

### pid指令
```conf
pid        logs/nginx.pid;
```

+ 语法: pid file;
+ 位置: 全局块中
+ 默认值: `logs/nginx.pid`, 即设置nginx的pid值保存到nginx根目录下的`logs/nginx.pid`文件中
+ 作用: 用来配置Nginx当前master进程的进程号ID存储的文件路径
> 该属性可以通过`./configure --pid-path=PATH`来指定

### error_log指令
```conf
error_log  logs/error.log;
```

+ 语法: error_log file [日志级别];
+ 位置: 任何位置
+ 默认值: `logs/error.log`, 即设置nginx的错误日志保存到nginx根目录下的`logs/error.pid`文件中
+ 作用: 用来配置Nginx的错误日志存放路径, 还可以指定日志级别 
> 该属性可以通过`./configure --error-log-path=PATH`来指定

### include指令
```conf
include main_nginx.conf;
```

+ 语法: include file;
+ 位置: 任意位置
+ 作用: 用来引入其他配置文件，使Nginx的配置更加灵活

## events块

下面为nginx.conf配置文件，events块的默认内容：
```conf
events {
    worker_connections  1024;
}
```

### accept_mutex指令
```conf
accept_mutex on;
```

+ 语法: accept_mutex on|off;
+ 默认值: on，即默认对nginx网络连接进行序列化
+ 作用: 这个配置主要可以用来解决常说的"惊群"问题。大致意思是在某一个时刻，客户端发来一个请求连接，Nginx后台是以多进程的工作模式，也就是说有多个worker进程会被同时唤醒，但是最终只会有一个进程可以获取到连接，如果每次唤醒的进程数目太多，就会影响Nginx的整体性能。如果将上述值设置为on(开启状态)，将会对多个Nginx进程接收连接进行序列号，一个个来唤醒接收，就防止了多个进程对连接的争抢。
+ 位置: events块中

### multi_accept指令
```conf
multi_accept on;
```

+ 语法: worker_connections number;
+ 默认值: 512，即默认单个worker进程最大连接数为512个
+ 作用: 用来配置单个worker进程最大的连接数; 这里的连接数不仅仅包括和前端用户建立的连接数，而是包括所有可能的连接数。另外，number值不能大于操作系统支持打开的最大文件句柄数量
+ 位置: events块中

### use指令
```conf
use epoll;
```

+ 语法: use method;
+ 默认值: 根据操作系统定
+ 作用: 用来设置Nginx服务器选择哪种事件驱动来处理网络消息
+ 位置: events块中
> 此处所选择事件处理模型是Nginx优化部分的一个重要内容，method的可选值有select/poll/epoll/kqueue等，之前在准备centos环境的时候，我们强调过要使用linux内核在2.6以上，就是为了能使用epoll函数来优化Nginx
> 另外这些值的选择，我们也可以在编译的时候使用

### events指令配置实例
```conf
events {
    accept_mutex on;
    multi_accept on;
    use epoll;
    worker_connections  1024;
}
```

## http块

### 定义MIME-Type
我们都知道浏览器中可以显示的内容有HTML、XML、GIF等种类繁多的文件、媒体等资源，浏览器为了区分这些资源，就需要使用MIME Type。所以说MIME Type是网络资源的媒体类型。Nginx作为web服务器，也需要能够识别前端请求的资源类型。
在Nginx的配置文件中，默认有两行配置：
```conf
include mime.types;
default_type application/octet-stream;
```

### default-type指令

+ 语法: default_type mime-type;
+ 默认值: default_type text/plain;
+ 位置: http块、server块、location块
+ 作用: 有些时候请求某些接口的时候需要返回指定的文本字符串或者json字符串，如果逻辑非常简单或者干脆是固定的字符串，那么可以使用nginx快速实现，这样就不用编写程序响应请求了，可以减少服务器资源占用并且响应性能非常快

> Nginx中如果返回一个中间不带的空格的字符串,则不需要带引号

**使用演示**
1. 修改nginx配置文件，添加一下内容
    ```conf
    location /get_text {
        default_type text/html;
        return 200 "This is nginx's text";
    }
    location /get_json {
        default_type application/json;
        return 200 "{'name':'jason','age':'22'}";
    }
    ```
2. 重新加载nginx服务器
    ```bash
    cd /usr/local/nginx/sbin/nginx
    
    ./nginx -s reload
    ```
3. 在浏览器地址栏输入`linux主机ip/get_text`和`linux主机ip/get_json`,进行测试
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210904195729.png)
    ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210904195802.png)

### 自定义服务日志
Nginx中日志类型为`access.log`、`error.log`
+ `access.log`: 用来记录用户所有的访问请求
+ `error.log`: 记录nginx本身运行时的错误信息，不会记录用户的访问请求

Nginx服务器支持对服务日志的格式、大小、输出等进行设置，需要使用到两个指令，分别时`access_log`和`log_format`指令

#### access_log指令

+ 语法: access_log path[format[buffer=size]]
+ 默认值: access_log logs/access.log combined;
+ 位置: http块、server块、locatin块
+ 作用: 用来设置用户访问日志的相关属性

#### log_format指令

+ 语法: log_format name[escape=default|json|none] string...
+ 默认值: log_format combined "...";
+ 位置: http块
+ 作用: 用来指定日志的输出格式

### 其他配置指令

#### sendfile指令

+ 语法: sendfile on|off;
+ 默认值: sendfile off;
+ 位置: http块、server块、locaiton块
+ 作用: 用来设置Nginx服务器是否使用sendfile()传输文件，该属性可以大大提高Nginx处理静态资源的性能

#### keepalive_timeout指令

+ 语法: keepalive_timeout time
+ 默认值: keepalive_timout 75s;
+ 位置: http块、server块、location块
+ 作用: 用来设置长连接的超时时间
> 为什么要使用keepalive？
> 我们都知道HTTP是一种无状态协议，客户端向服务端发送一个TCP请求，服务端响应完毕后断开连接。
> 如何客户端向服务端发送多个请求，每个请求都需要重新创建一次连接，效率相对来说比较多，使用keepalive模式，可以告诉服务器端在处理完一个请求后保持这个TCP连接的打开状态.
> 若接收到来自这个客户端的其他请求，服务端就会利用这个未被关闭的连接，而不需要重新创建一个新连接，提升效率，但是这个连接也不能一直保持，这样的话，连接如果过多，也会是服务端的性能下降，这个时候就需要我们进行设置其的超时时间。

#### keepalive_requests指令

+ 语法: keepalive_requests number;
+ 默认值: keepalive_requests 100;
+ 位置: http块、server块、location块
+ 作用: 用来设置一个keep-alive连接使用的次数

## server块和locaiton块
server块和location块都是我们要重点讲解和学习的内容，因为我们后面会对Nginx的功能进行详细讲解，所以这块内容就放到静态资源部署的地方给大家详细说明。
本节我们主要来认识下Nginx默认给的nginx.conf中的相关内容，以及server块与location块在使用的时候需要注意的一些内容。
```conf
server {
    # 设置nginx端口
    listen       80;
    # 设置nginx服务器ip
    server_name  localhost;
    location / {
        root   html;
        index  index.html index.htm;
    }

    error_page   500 502 503 504 404  /50x.html;
    location = /50x.html {
        root   html;
    }
}
```

## Nginx配置文件实例

### 概述

前面我们已经对Nginx服务器默认配置文件的结构和涉及的基本指令做了详细的阐述。通过这些指令的合理配置，我们就可以让一台Nginx服务器正常工作，并且提供基本的web服务器功能。
接下来我们将通过一个比较完整和最简单的基础配置实例，来巩固下前面所学习的指令及其配置。

### 案例需求
1. 在浏览器输入`http://192.168.123.15:8081/server1/location1`时，实际访问的是`index_sr1_location1.html`页面
2. 在浏览器输入`http://192.168.123.15:8081/server1/location2`时，实际访问的是`index_sr1_location2.html`页面
3. 如果访问的资源不存在，则返回自定义的404页面
4. 将`/server`和`/server2`的配置使用不同的配置文件分割，将文件放到`/home/jason/conf`目录下，然后使用include进行合并
5. 为为/server1和/server2各自创建一个访问日志文件

### 案例准备
linux服务器相关文件资源准备，如图所示：
![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210904214242.png)

### 案例实现
**nginx.conf**
```conf
##全局块——begin##

user  jason;

#配置运行nginx-worker进程数
worker_processes  2;

#配置nginx服务器运行对错误日志存放的路径
error_log  logs/error.log;

#配置Nginx服务器运行是记录Nginx-master进程的pid文件路径和名称
pid        logs/nginx.pid;

##全局块——end##


##events块——being##

events {
    #设置Nginx网络连接序列化
    accept_mutex on;
    #设置Nginx-worker进程是否可以同时接收多个请求
    multi_accept on;
    #设置Nginx-worker进程的最大连接数
    worker_connections  1024;
    #设置Nginx使用的事件驱动模型
    use epoll;
}

##events块——end##


##http块——begin##

http {
    #定义MIME-Type
    include       mime.types;
    default_type  application/octet-stream;

    #配置允许使用sendfile方式运行
    sendfile        on;

    #配置连接超时时间
    keepalive_timeout  65;

    #配置请求处理日志格式
    log_format server1 '---->server1 access log';
    log_format server2 '---->server2 access log';

    
    ##server块——begin##

    #引入其他配置文件
    include /home/jason/conf/*.conf;

    ##server块——end##

}
```

**server1.conf**
```conf
server{	
    #配置监听端口和主机名称
    listen 8081;
    server_name localhost;

    #配置请求处理日志存放路径
    access_log /home/jason/myweb/server1/logs/access.log server1;

    #配置错误页面
    error_page 404 /404.html;

    #配置处理/server1/location1请求的location
    location /server1/location1{
        root /home/jason/myweb;
        index index_sr1_lcation1.html;
    }

    #配置处理/server1/location2请求的location
    location /server1/location2{
        root /home/jason/myweb;
        index index_sr1_lcation2.html;
    }

    #配置错误页面
    location = /404.html {
        root /home/jason/myweb;
        index 404.html;
    }
}
```

**server2.conf**
```conf
server {
    #配置监听端口和主机ip
    listen 8082;
    server_name localhost;
    
    #配置请求处理日志存放路径
    access_log /home/jason/myweb/server2/logs/access.log server2;

    #配置错误页面对404.html做了定向配置
    error_page 404 /404.html;

    #配置处理/server2/location1请求的location
    location /server2/location1 {
        root /home/jason/myweb;
        index index_sr2_location1.html;
    }

    #配置处理/server2/location2请求的location
    location /server2/location2 {
        root /home/jason/myweb;
        index index_sr2_location2.html;
    }   

    #配置错误页面转向
    location = /404.html {
        root /home/jason/myweb;
        index 404.html;
    }
}
```

### 案例测试

1. 首先验证nginx配置文件是否正确

   ```bash
   /usr/local/nginx/sbin/nginx -t
   
   > nginx: the configuration file /usr/local/nginx/conf/nginx.conf syntax is ok
   > nginx: configuration file /usr/local/nginx/conf/nginx.conf test is successful
   > 配置文件正确
   ```

2. 启动nginx服务

   ```bash
   /usr/local/nginx/sbin/nginx
   ```

3. 查看nginx进程是否有两个worker进程

   ```bash
   ps -ef | grep nginx
   
   > root      1884     1  0 10:31 ?        00:00:00 nginx: master process ./nginx
   > jason     1885  1884  0 10:31 ?        00:00:00 nginx: worker process
   > jason     1886  1884  0 10:31 ?        00:00:00 nginx: worker process
   > root      1888  1757  0 10:31 pts/0    00:00:00 grep --color=auto nginx
   > 我们可以看到配置文件设置的两个worker进程生效了
   ```

4. 访问`http://192.168.123.15:8081/server1/location1/`，访问成功

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210905110303.png)

5. 访问`http://192.168.123.15:8081/server1/location2/`，访问成功

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210905110433.png)

6. 访问`http://192.168.123.15:8082/server2/location1/`，访问成功

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210905110514.png)

7. 访问`http://192.168.123.15:8082/server2/location2/`，访问成功

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210905110550.png)

8. 分别查看`/home/jason/myweb/server1/logs/access.log`和`/home/jason/myweb/server2/logs/access.log`两个日志文件

   ```log
   ---->server1 access log
   ---->server1 access log
   ---->server1 access log
   ---->server1 access log
   ---->server1 access log
   ```

   ```log
   ---->server2 access log
   ---->server2 access log
   ---->server2 access log
   ---->server2 access log
   ```

   我们可以看到两个日志文件都成功的在我们访问nginx时，输出了我们设置的日志信息！

**案例实现测试成功！**

# Nginx命令配置到系统环境

> **我们并不推荐使用使用集成到系统环境中的Nginx命令对Nginx服务进行操作，因为这样会产生许多意料之外的错误！**

前面我们介绍过Nginx安装目录下的二级制可执行文件`nginx`的很多命令，要想使用这些命令前提是需要进入sbin目录下才能使用，很不方便，如何去优化，我们可以将该二进制可执行文件加入到系统的环境变量，这样的话在任何目录都可以使用nginx对应的相关命令。具体实现步骤如下:

1. 修改`/etc/profile`文件，在文件最后加入以下内容（如果只想在单用户生效，则修改用户目录下的`.bash_profile`文件）

   ```
   # Nginx
   export PATH=$PATH:/usr/local/nginx/sbin
   # Nginx END
   ```

2. 使`/etc/profile`文件的修改立即生效

   ```bash
   source /etc/profile
   ```

3. 在任意目录执行nginx命令

   ```bash
   nginx -V
   
   > nginx version: nginx/1.16.1
   > built by gcc 4.8.5 20150623 (Red Hat 4.8.5-44) (GCC)
   > configure arguments:
   ```

# Nginx配置成系统服务

把Nginx应用服务设置成为系统服务，方便对Nginx服务的启动和停止等相关操作，具体实现步骤:

1. 在`/usr/lib/systemd/system`目录下创建`nginx.service`文件，其文件内容如下

   ```service
   [Unit]
   Description=nginx web service
   Documentation=http://nginx.org/en/docs/
   After=network.target
   
   [Service]
   Type=forking
   PIDFile=/usr/local/nginx/logs/nginx.pid
   ExecStartPre=/usr/local/nginx/sbin/nginx -t -c /usr/local/nginx/conf/nginx.conf
   ExecStart=/usr/local/nginx/sbin/nginx
   ExecReload=/usr/local/nginx/sbin/nginx -s reload
   ExecStop=/usr/local/nginx/sbin/nginx -s stop
   PrivateTmp=true
   
   [Install]
   WantedBy=default.target
   ```

2. 对其创建的文件给予权限

   ```bash
   chmod 755 /usr/lib/systemd/system/nginx.service
   ```

3. 使用配置好的系统服务命令来操作Nginx

   + 启动Nginx：`systemctl start nginx`
   + 停止Nginx：`systemctl stop nginx`
   + 重启Nginx：`systemctl restart nginx`
   + 重新加载配置文件：`systemctl reload nginx`
   + 查看Nginx状态：`systemctl status nginx`
   + 设置Nginx开机自启：`systemctl enable nginx`

# Nginx静态资源部署

## 概述

上网去搜索访问资源对于我们来说并不陌生，通过浏览器发送一个HTTP请求实现从客户端发送请求到服务器端获取所需要内容后并把内容回显展示在页面的一个过程。这个时候，我们所请求的内容就分为两种类型，一类是静态资源、一类是动态资源。
**静态资源**：即指在服务器端真实存在并且能直接拿来展示的一些文件，比如常见的html页面、css文件、js文件、图 片、视频等资源；
**动态资源**：即指在服务器端真实存在但是要想获取需要经过一定的业务逻辑处理，根据不同的条件展示在页面不同这 一部分内容，比如说报表数据展示、根据当前登录用户展示相关具体数据等资源；

Nginx处理静态资源内容，我们需要考虑下面这几个问题：

1. 静态资源的配置指令
2. 静态资源的配置优化
3. 静态资源的压缩配置指令
4. 静态资源的缓存处理
5. 静态资源的访问控制，包括跨域问题和防盗链问题

## Nginx静态资源配置指令

### listen指令

+ 语法: `listen address[:port][default_server]...;`或者`listen port [default_server]...;`
+ 默认值: `listen *:80`
+ 位置: server块中
+ 作用: 用来设置nginx的监听端口和主机ip

listen的设置比较灵活，我们通过几个例子来把常用的设置方式熟悉下：

```
# 监听指定IP的指定端口
listen localhost:8000;

# 监听指定IP的所有端口
listen localhost;

# 监听指定端口上的所有IP连接
listen 8000;

# 监听指定端口上的所有IP连接
listen *:8000;
```

**default_server**属性是一个标识符，用来将此虚拟机设置成默认主机。所谓默认主机是指如果没有匹配到对应的`address:port`主机，则会默认选择这个。如果不手动使用`default_server`属性指定默认主机，则第一个server配置块则会被当作默认主机。

```conf
server{
	listen 8080 default_server;
	server_name localhost;
	default_type text/plain;
	return 444 'This is a error request';
}
```

### server_name指令

+ 语法: `server_name name...;`(name可以有多个，用空格分隔)
+ 默认值: `server_name "";`
+ 位置: server块中
+ 作用: 设置虚拟主机服务名称

server_name的配置方式有三种，分别是：

1. 精确匹配
2. 通配符匹配
3. 正则表达式匹配

**精确匹配**

> hosts是一个没有扩展名的系统文件，可以用记事本等工具打开，其作用就是将一些常用的网址域名与其对应的IP地址建立一个关联“数据库”，当用户在浏览器中输入一个需要登录的网址时，系统会首先自动从hosts文件中寻找对应的IP地址，一旦找到，系统会立即打开对应网页，如果没有找到，则系统会再将网址提交DNS域名解析服务器进行IP地址的解析。
>
> centos路径：`/etc/hosts`
>
> windows路径：`C:\Windows\System32\drivers\etc`

因为域名是要收取一定的费用，所以我们可以使用修改hosts文件来制作一些虚拟域名来使用。需要修改 `/etc/hosts`文件来添加以下内容:

```
127.0.0.1 www.rtx3090.xyz
127.0.0.1 www.rtx3090.asia
```

修改nginx配置文件，增加以下内容:

```
server {
    listen 80;
    #配置虚拟主机名
    server_name www.rtx3090.xyz www.rtx3090.asia;

    #配置处理/请求
    location / {
        root /home/jason/myweb/server1/location1;
        index index_sr1_lcation1.html;
    }
}
```

重新加载配置文件`nginx -s reload`，然后在linux服务器进行测试访问`www.rtx3090.xyz`或者`www.rtx3090.asia`，观察是否可以正常访问nginx

```bash
[root@localhost conf]# curl www.rtx3090.xyz

<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>
....

<p><em>index_sr1_location1.html</em></p>
</body>
</html>
```

```bash
[root@localhost conf]# curl www.rtx3090.asia

<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>
....

<p><em>index_sr1_location1.html</em></p>
</body>
</html>
```

我们可以看到我们的利用精确匹配的虚拟主机名访问nginx成功！

 **通配符匹配**

server_name中支持通配符"*",但需要注意的是**通配符不能出现在域名的中间，只能出现在首段或尾段**（这是重点）

> 在精确匹配中，我们已经修改设置过`/etc/hosts` 文件了

修改nginx配置文件，采用通配符匹配配置虚拟主机名称（需要删除前面`精确匹配`案例时配置的server块）

```conf
server {
    listen 80;
    server_name *.rtx3090.xyz www.rtx3090.*;

    #配置处理请求
    location / {
        root /home/jason/myweb/server1/location2;
        index index_sr1_lcation2.html;
    }
}
```

重新加载nginx服务，然后在linux服务器进行测试访问`www.rtx3090.xyz`或者`www.rtx3090.asia`，观察是否可以正常访问nginx

```bash
[root@localhost conf]# curl www.rtx3090.xyz

<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>
....

<p><em>index_sr1_location2.html</em></p>
</body>
</html>
```

```bash
[root@localhost conf]# curl www.rtx3090.asia

<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>
....

<p><em>index_sr1_location2.html</em></p>
</body>
</html>
```

我们可以看到我们的利用通配符匹配的虚拟主机名访问nginx成功！

**正则表达式匹配**

server_name中可以使用正则表达式，并且使用 ~ 作为正则表达式字符串的开始标记

| 代码  |                            说明                             |
| :---: | :---------------------------------------------------------: |
|   ^   |                   匹配搜索字符串开始位置                    |
|   $   |                   匹配搜索字符串结束位置                    |
|   .   |              匹配除换行符\n之外的任何单个字符               |
|   \   |            转义字符，将下一个字符标记为特殊字符             |
| [xyz] |               字符集，与任意一个指定字符匹配                |
| [a-z] |             字符范围，匹配指定范围内的任何字符              |
|  \w   | 与以下任意字符匹配 A-Z a-z 0-9 和下划线,等效于[A-Za-z0- 9_] |
|  \d   |                  数字字符匹配，等效于[0-9]                  |
|  {n}  |                         正好匹配n次                         |
| {n,}  |                         至少匹配n次                         |
| {n,m} |                     匹配至少n次至多m次                      |
|   *   |                   零次或多次，等效于{0,}                    |
|   +   |                   一次或多次，等效于{1,}                    |
|   ?   |                   零次或一次，等效于{0,1}                   |

修改nginx配置文件，采用正则表达式匹配配置虚拟主机名称（需要删除前面`精确匹配`、`通配符匹配`案例时配置的server块）

```conf
server {
    listen 80;
    server_name ~^www\.\w+\.xyz$;
    default_type text/plain;
    return 200 'regex_success';
}
```

重新加载nginx服务，然后在linux服务器进行测试访问`www.rtx3090.xyz`或者`www.rtx3090.asia`，观察是否可以正常访问nginx

```bash
[root@localhost conf]# curl www.rtx3090.xyz

> regex_success
```

```bash
[root@localhost conf]# curl www.rtx3090.asia

> regex_success
```

我们可以看到我们的利用正则表达式匹配的虚拟主机名访问nginx成功！

**匹配执行顺序**

由于server_name指令支持通配符和正则表达式，因此在包含多个虚拟 主机的配置文件中，可能会出现一个名称被多个虚拟主机的 server_name匹配成功，当遇到这种情况，当前的请求交给谁来处理呢?

**下面我们配置五个server块，测试它们的匹配执行顺序**

```conf
#正则表达式匹配
server {
    listen 80;
    server_name ~^www\.\w+\.xyz$;
    default_type text/plain;
    return 200 'regex_success';
}

#在尾部使用通配符匹配
server {
    listen 80;
    server_name www.rtx3090.*;
    default_type text/plain;
    return 200 'wildcard_after_success';
}

#在头部使用通配符匹配
server {
    listen 80;
    server_name *.rtx3090.asia;
    default_type text/plain;
    return 200 'wildcard_before_success';
}

#精确匹配
server {
    listen 80;
    server_name www.rtx3090.xyz;
    default_type text/plain;
    return 200 'exact_success';
}

#默认匹配
server {
    listen 80 default_server;
    server_name _;
    default_type text/plain;
    return 404 'default_server not found server';
}
```

**配置结果**

```bash
#1.精确匹配
exact_success

#2.在头部使用通配符匹配
wildcard_before_success

#3.在尾部使用通配符匹配
wildcard_after_success

#4.正则表达式匹配
regex_success

#5.默认匹配
default_server not found server!!
```

### location块

+ 语法: `location[=|~|~*|^~|@] uri{...}`
+ 默认值: `_`
+ 位置: server块中
+ 作用: 设置处理请求的URI

> uri变量是待匹配的请求字符串，可以不包含正则表达式，也可以包含正则表达式，那么nginx服务器在搜索匹配location的时候，是先使用不包含正则表达式进行匹配，找到一个匹配度最高的一个，然后在通过包含正则表达式的进行匹配，如果能匹配到直接访问，匹配不到，就使用刚才匹配度最高的那个location来处理请求。

**没有任何符号，表示URI路径必须以指定字符为开头，但后面可以跟其他**

```conf
server {
    listen 80;
    server_name 127.0.0.1;

    #location后纯路径不带符号
    location /abc {
        default_type text/plain;
        return 200 "access success";
    }
}
```

```
# 在浏览器中下列链接都是可以访问nginx的
http://192.168.123.15/abc 
http://192.168.123.15/abc?p1=TOM 
http://192.168.123.15/abc/ 
http://192.168.123.15/abcdef
```

**以符号`=`开头，表示URI路径必须以精确匹配，不能有一点差错（可以跟参数）**

```conf
server {
    listen 80;
    server_name 127.0.0.1;

    #带符号`=`，表示路径必须以精确匹配，不能有一点差错
    location =/abc {
        default_type text/plain;
        return 200 "access success";
    }
}
```

```
#可以匹配到
http://192.168.123.15/abc
http://192.168.123.15/abc?p1=TOM 12
#匹配不到
http://192.168.123.15/abc/
http://192.168.123.15/abcdef
```

**以符号`~`开头，表示URI路径使用正则表达式匹配；以符号`~*`开头，表示URI路径使用正则表达式匹配且不区分大小写**

```conf
#以符号`~`开头，表示URI路径使用正则表达式匹配；
location ~^/abc\w$ {
    default_type text/plain;
    return 200 "access success";     
}

#以符号`~*`开头，表示URI路径使用正则表达式匹配且不区分大小写
location ~*^/abc\w$ {
    default_type text/plain;
    return 200 "access success";
}
```

**以符号`^~`开头，表示URI路径使用精确匹配，与不加符号功能几乎一致，但是如果精确匹配到了，就不会继续搜索其他匹配模式了**

```conf
location ^~/abc {
    default_type text/plain;
    return 200 "access success";
}
```

### root指令和alias指令

#### root指令

+ 语法: `root path`
+ 默认值: `root html`
+ 位置: http块、server块、location块
+ 作用: 设置请求的根目录

#### alias指令

+ 语法：`alias path`
+ 默认值：—
+ 位置：location块
+ 作用：用来替换更改location的URI路径

#### 区别

以上两个指令都可以来指定访问资源的路径，那么这两者之间的区别是什么?

1. **root的处理结果是**: root路径+location路径
2. **alias的处理结果是**: 使用alias路径替换location路径
3. alias是一个目录别名的定义，root则是最上层目录的含义
4. 如果location路径是以/结尾,则alias也必须是以/结尾，root没有要求

### index指令

+ 语法：`index file ...`
+ 默认值：`index index.html`
+ 位置：http块、server块、location块
+ 作用：设置网站的默认首页

index指令后面的文件路径可以设置多个，如果在客户端进行访问的时候没有指定具体的资源路径，则nginx服务器就会采用我们设置的默认首页。

优先采用index指令后的第一个资源文件，第一个资源文件不存在，会继续向下查找第二个资源文件。

**举例**

```conf
location / {
    root /usr/local/nginx/html;
    index index.html index.htm;  
}
```

### error_page指令

+ 语法：`error_page code ... [=[response]] uri`
+ 默认值：`-`
+ 位置：http块、server块、lcation块
+ 作用：设置网站的错误页面

**举例**

1. 指定具体跳转的地址

   ```conf
   error_page 404 http://www.rtx3090.xyz;
   ```

2. 指定重定向的地址

   ```conf
   error_page 404 /50x.html;
   error_page 500 502 503 504 /50x.html;
   
   location =/50x.html {
       root html;
   }
   ```

3. 使用lcation的@符号完成错误信息展示

   ```conf
   error_page 404 @jump_to_error;
   
   location @jump_to_error {
       default_type text/plain;
       return 404 'NOT FOUND 404'; 
   }
   ```

4. `=[repsonse]`为可选项，可以修改状态码

   ```conf
   error_page 404 =200 /50x.html;
   
   location =/50x.html {
       root html;
   }
   ```

   > 这样当我们访问一个不存在的资源时，我们虽然还是会跳转到`50x.html`错误页面
   >
   > 但是我们的响应码就不再是404了，而是被我们手动修改为了200

## 静态资源优化配置

我们如果要对Nginx静态资源的处理进行优化，就需要从Nginx配置文件中的下面三点进行入手：

+ `sendfile on;``
+ ``tcp_nopush on;`
+ tcp_nodelay on;`

下面我们分别阐述一下这三个指令配置的作用：

### sendfile指令

+ 语法：`sendfile on | off`
+ 默认值：`sendfile off;`
+ 位置：http块、server块、location块
+ 作用：设置是否开启高效的文件传输模式

> 请求静态资源的过程:客户端通过网络接口向服务端发送请求，操作系 统将这些客户端的请求传递给服务器端应用程序，服务器端应用程序会 处理这些请求，请求处理完成以后，操作系统还需要将处理得到的结果 通过网络适配器传递回去

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210906043103.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210906043118.png)

### tcp_nopush指令

+ 语法：`tcp_nopush on | off`
+ 默认值：`tcp_nopush off;`
+ 位置：http块、server块、location块
+ 作用：设置是否提升网络包的传输效率（该指令必须在sendfile指令为on时可可以生效）

### tcp_nodelay指令

+ 语法：`tcp_nodelay on | off`
+ 默认值：`tcp_nodplay on;`
+ 位置：http块、server块、location块
+ 作用：设置是否提高网络包传输的实时性（该指令必须在keep-alive连接开启的情况下才会生效）

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210906043434.png)

**配置实例**

```conf
sendfile on;
tcp_nopush on;
tcp_ondelay;
```

> 项目开发中我们建议这三个指令都手动设置为on

## Nginx静态资源压缩

为了提高数据的传输效率，我们有时可以先对数据进行压缩，然后在进行传输。

Nginx对静态资源进行压缩需要用到下面这三个模块：

+ `ngx_http_gzip_module`模块
+ `ngx_http_gzip_static_module`模块
+ `ngx_http_gunzip_module`模块

首先我们先介绍一下Gzip模块的一些指令:

### Gzip模块指令

> 接下来所学习的指令都来自ngx_http_gzip_module模块，该模块会在 nginx安装的时候内置到nginx的安装环境中，也就是说我们可以直接使 用这些指令

#### Gzip指令

+ 语法：`gzip on | off`
+ 默认值：`gzip off;`
+ 位置：http块、server块、location块
+ 作用：设置是否开启gzip功能

```conf
http {
	gzip on;
}
```

#### gzip_types指令

+ 语法：`gzip_types mime-type ...;`
+ 默认值：`gzip_types text/html;`
+ 位置：http块、server块、location块
+ 作用：根据响应页的MIME类型选择性地开启Gzip压缩功能

> 此指令只有在`gzip`指令开启时，才能生效

```conf
http{
	gzip on;
	#所选择的值可以从mime.types文件中进行查找，也可以使用"*"代表所 有
	gzip_types application/javascript;
}
```

#### gzip_comp_level指令

+ 语法：`gzip_comp_level level;`
+ 默认值：`gzip_comp_level 1;`
+ 位置：http块、server块、location块
+ 作用：设置Gzip压缩等级，级别从1-9。级别为1压缩程度最小，级别为9压缩程度最大，效率与压缩程度成反比

> 压缩等级我们推荐设置为6，因为压缩等级太高会消耗太多的cup资源，并且压缩的比例提升也很小
>
> 此指令只有在`gzip`指令开启时，才能生效

```conf
http{
	gzip on;
	gzip_comp_level 6;
}
```

#### gzip_vary指令

+ 语法：`gzip_vary on | off;`
+ 默认值：`gzip_vary off;`
+ 位置：http块、server块、location块
+ 作用：设置使用Gzip进行压缩发送是否携带“Vary:Accept-Encoding”头域的响应头部，主要是告诉接收方，所发送的数据经过了Gzip压缩处理

> 此指令只有在`gzip`指令开启时，才能生效

```conf
http{
		gzip on;
		gzip_vary on;
}
```

#### gzip_buffers指令

+ 语法：`gzip_buffers number size;`
+ 默认值：`gzip_buffers 32 4k | 16 8k;`
+ 位置：http块、server块、location块
+ 作用：设置请求压缩缓冲区数量和大小

> 其中number: 指定Nginx服务器向系统申请缓存空间个数，size指的是每个缓存空间的大小。主要实现的是申请number个每个大小为size的内存空间。这个值的设定一般会和服务器的操作系统有关，所以建议此项不设置，使用默认值即可
>
> 此指令只有在`gzip`指令开启时，才能生效

```conf
http{
	gzip on;
	gzip_buffers 4 16k;
}
```

#### gzip_disable指令

+ 语法：`gzip_disable regex...;`
+ 默认值：`-`
+ 位置：http块、server块、location块
+ 作用：针对不同种类客户端发起的请求，可以选择性地开启和关闭Gzip功能

> regex正则表达式: 根据客户端的浏览器标志(user-agent)来设置，支持使用正则表达 式。指定的浏览器标志不使用Gzip.该指令一般是用来排除一些明显不支持Gzip的浏览器
>
> 此指令只有在`gzip`指令开启时，才能生效

```conf
http{
	gzip on;
	gzip_disable "MSIE [1-6]\.";
}
```

#### gzip_http_version指令

+ 语法：`gzip_http_version 1.0 | 1.1;`
+ 默认值：`gzip_http_version 1.1;`
+ 位置：http块、server块、location块
+ 作用：指定使用Gzip的HTTP最低版本，该指令一般采用默认值即可

> 此指令只有在`gzip`指令开启时，才能生效

#### gzip_min_length指令

+ 语法：`gzip_min_length length;`
+ 默认值：`gzip_min_length 20;`
+ 位置：http块、server块、location块
+ 作用：设置开启gzip的数据大小限度

> 此指令只有在`gzip`指令开启时，才能生效
>
> Gzip压缩功能对大数据的压缩效果明显，但是如果要压缩的数据比较小 的化，可能出现越压缩数据量越大的情况。因此我们需要根据响应内容的大小来决定是否使用Gzip功能，响应页面的大小可以通过头信息中的Content-Length 来获取。但是如何使用了Chunk编码动态压缩，该指令将被忽略。建议设置为1K或以上

#### gzip_proxied指令

+ 语法：`gzip_proxied off | expired | no-cache | no-store | private | no_last_modified | no_etag | auth | any;`
+ 默认值：`gzip_proxied off;`
+ 位置：http块、server块、location块
+ 作用：设置是否对服务端返回的结果进行Gzip压缩

> off - 关闭Nginx服务器对后台服务器返回结果的Gzip压缩
>
> expired - 启用压缩，如果header头中包含 "Expires" 头信息
>
> no-cache - 启用压缩，如果header头中包含 "Cache-Control:no-cache" 头信息
>
> no-store - 启用压缩，如果header头中包含 "Cache-Control:no-store" 头信息
>
> private - 启用压缩，如果header头中包含 "Cache-Control:private" 头信息
>
> no_last_modified - 启用压缩,如果header头中不包含 "Last-Modified" 头信息
>
> no_etag - 启用压缩 ,如果header头中不包含 "ETag" 头信息
>
> auth - 启用压缩 , 如果header头中包含 "Authorization" 头信息
>
> any - 无条件启用压缩

### Gzip压缩功能实例配置

> 这些配置在很多地方可能都会用到，所以我们可以将这些内容抽取到一 个配置文件中，然后通过include指令把配置文件再次加载到nginx.conf 配置文件中，方法使用

**nginx_gzip.conf**

```conf
gzip on;										#开启gzip功能
gzip_types *;								#压缩元文件类型，根据具体的访问资源类型设定
gzip_comp_level 6;					#gzip压缩级别
gzip_min_length 1024;				#压缩响应页面的最小长度
gzip_buffers 4 16k;					#缓存空间大小
gzip_http_version 1.1;			#指定压缩响应所需要的最低HTTP请求版本
gzip_vary on;								#往头信息中添加压缩标示
gzip_disable "MSIE [1-6]\.";#对IE6以下的版本都不进行压缩
gzip_proxied off;						#nginx作为反向代理压缩服务端返回数据的条件
```

**nginx.conf**

```conf
include nginx_gzip.conf
```

### Gzip和sendfile共存问题

前面在讲解sendfile的时候，提到过开启sendfile以后，在读取磁盘上的静态资源文件的时候，可以减少拷贝的次数，可以不经过用户进程将静态文件通过网络设备发送出去，但是Gzip要想对资源压缩，是需要经过用户进程进行操作的。所以Gzip就与sendfile指令产生了冲突！

可以使用ngx_http_gzip_static_module模块的gzip_static指令来解决！

#### gzip_static指令

+ 语法：`gzip_static on | off | always;`
+ 默认值：`gzip_static off;`
+ 位置：http块、server块、location块
+ 作用：检查与访问资源同名的.gz文件时，response中以gzip相关的header返回.gz文件内容

> 添加上述命令后，会报错`unknown directive "gzip_static"`.
>
> 报错的原因为Nginx默认没有添加`ngx_http_gzip_static_module`模块的.
>
> 所有首先我们需要来添加这个模块

**添加 `ngx_http_gzip_static_module`模块**

1. 查询并记录当前Nginx的配置参数（便于后面使用）

   ```bash
   nginx -V
   ```

2. 将Nginx安装目录下的sbin目录中的nginx二进制文件进行更名备份，以防模块添加失败，便于我们恢复Nginx配置

   ```bash
   cd /usr/local/nginx/sbin
   
   mv nginx nginx_old
   ```

3. 进入Nginx源码解压包，清空之前编译内容

   ```bash
   cd /usr/local/core/nginx-1.16.1
   
   make clean
   ```

4. 使用之前的参数+新模块参数来配置Nginx

   ```bash
   ./configure --with-http_gzip_static_module
   ```

5. 进行编译

   ```bash
   make
   ```

6. 将objs目录下的nginx二进制执行文件复制到nginx安装目录下的sbin 目录中

   ```bash
   cp objs/nginx /usr/local/nginx/sbin
   ```

7. 执行更新命令（注意这一步一定要确保当前的Nginx进程是通过 `/usr/local/nginx/sbin/nginx`这个绝对路径命令启动的，而不是环境变量快捷命令启动的，否则会报错！）

   ```bash
   make upgrade
   ```

8. 修改nginx.conf配置文件，在http块加入下列内容

   ```conf
   gzip_static on;
   ```

9. 测试配置文件语法是否正确

   ```bash
   nginx -t
   ```

10. 重新加载Nginx服务，没有报错则表示模块添加成功

    ```bash
    nginx -s reload
    ```

至此我们的`ngx_http_gzip_static_module`模块已经安装成功了，我们现在需要使用它来解决我们遇到的`sendfile`指令与`gzip`指令冲突的问题了。

**解决冲突问题**

1. 修改nginx.conf配置文件，关闭`gzip`指令，添加开启 `gzip_static` 指令

   ```conf
   gzip off;
   gzip_static on;
   ```

2. 检查配置文件是否有错

   ```bash
   nginx -t
   ```

3. 重新加载Nginx

   ```bash
   nginx -s reload
   ```

4. 手动在命令行使用`gzip` 命令来压缩指定的静态资源

   ```bash
   cd /usr/local/nginx/html
   
   gzip jquery-3.6.0.js
   ```

5. 查看压缩后的静态资源，变为了`.gz`结尾的文件

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210906103828.png)

6. 在浏览器中访问这个静态资源，发现没有开启`gzip`压缩指令，静态资源依旧被压缩了

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210906110416.png)

## 静态资源的缓存处理

### 什么是缓存

缓存(cache)，原始意义是指访问速度比一般随机存取存储器(RAM) 快的一种高速存储器，通常它不像系统主存那样使用DRAM技术，而使用昂贵但较快速的SRAM技术。缓存的设置是所有现代计算机系统发挥高性能的重要因素之一

### 什么是web缓存

Web缓存是指一个Web资源(如html页面，图片，js，数据等)存在于 Web服务器和客户端(浏览器)之间的副本。缓存会根据进来的请求保存输出内容的副本;当下一个请求来到的时候，如果是相同的URL，缓存会根据缓存机制决定是直接使用副本响应访问请求，还是向源服务器再次发送请求。比较常见的就是浏览器会缓存访问过网站的网页，当再次访问这 个URL地址的时候，如果网页没有更新，就不会再次下载网页，而是直接使用本地缓存的网页。只有当网站明确标识资源已经更新，浏览器才会再次下载网页

### 什么是浏览器缓存

为了节约网络的资源加速浏览，浏览器在用户磁盘上对最近请求过的文档进行存储，当访问者再次请求这个页面时，浏览器就可以从本地磁盘显示文档，这样就可以加速页面的阅览

+ 成本最低的一种缓存实现
+ 减少网络带宽消耗
+ 降低服务器压力
+ 减少网络延迟，加快页面打开速度

### 浏览器缓存的执行流程

在了解浏览器缓存执行流程之前，我们需要先认识一下HTTP协议中和页面缓存相关的字段。

|    header     |                   说明                    |
| :-----------: | :---------------------------------------: |
|    Expires    |           缓存国企的日期和时间            |
| Cache-Control |         设置和缓存相关的配置信息          |
| Last-Modified |          请求资源最后的修改时间           |
|     ETag      | 请求变量的实体标签当前值，比如文件的MD5值 |

**浏览器缓存执行流程图**

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210906113821.png)

1. 用户首次通过浏览器发送请求到服务端获取数据，客户端是没有对应的缓存，所以需要发送request请求来获取数据
2. 服务端接收到请求后，获取服务端的数据及服务端缓存的允许后，返回200状态码并且响应头上附加对应资源以及缓存信息
3. 当用户再次访问相同资源的时候，客户端会在浏览器的缓存目录中查找是否存在响应的缓存文件
4. 如果没有找到对应的缓存文件，则重回第2步
5. 如果有缓存文件，接下来对缓存文件进行过期与否的判断（过期判断的标准是字段`Expires`）
6. 如果字段`Expires`显示缓存没有过期，则直接从本地缓存中返回数据进行展示
7. 如果字段`Expires` 显示缓存已经过期，接下来需要判断缓存文件是否发生过变化
8. 判断的标准有两个，一个是字段`ETag`，一个是字段`Last-Modified`
9. 判断结果若为未发生变化，则服务端返回304，直接从缓存文件中获取数据
10. 判断结果若为已发生变化，则重新从服务端获取数据，并根据缓存协商（服务端所设置的是否需要进行缓存数据的相关配置）来进行数据缓存

### 浏览器缓存相关指令

Nginx需要进行缓存相关设置，就需要用到如下的指令

#### expires指令

+ 语法：`expires [modified] time expires epoch | max | off;`
+ 默认值：`expires off;`
+ 位置：http块、server块、lcoation块
+ 作用：控制页面缓存的作用，即控制 HTTP响应中的`Expires`和`Cache-Control`

> `time`：作用是指定过期时间，可以为正整数，也可以为负整数。若为正整数，则`Cache-Control`的值为`max-age=time`。若为负整数，则`Cache-control`的值为`no-cache`。
>
> `epoch`：作用是指定`Expires`的值为`1 January,1970,00:00:01 GMT'(1970-01-01 00:00:00)`，指定`Cache-Control`的值为`no-cache`
>
> `max` ：作用为指定 `Expires`的值为`'31 December2037 23:59:59GMT' (2037-12- 31 23:59:59) `，指定`Cache-Control`的值为10年
>
> `off`：作用为不缓存

#### add_header指令

+ 语法：`add_header name value [always];`
+ 默认值：`-`
+ 位置：http块、server块、location块
+ 作用：添加指定响应头和响应值

若`name` 为`Cache-Control`，那么`value`可以设置成以下的数据：

|       指令       |                      说明                      |
| :--------------: | :--------------------------------------------: |
| must-revalidate  |       可以缓存单必须再向原服务器进行确认       |
|     no-cache     |             缓存前必须确认其有效性             |
|     no-store     |           不缓存请求或响应的任何内容           |
|   no-transform   |              代理不可更改媒体类型              |
|      public      |            可向任意方提供响应的缓存            |
|     private      |            仅向特定用户提供响应缓存            |
| proxy-revalidate | 要求中间缓存服务器对缓存的响应有效性再进行确认 |
|   max-age=[秒]   |                 响应最大Age值                  |
|  s-maxage=[秒]   |         公共缓存服务器响应的最大Age值          |

# Nginx的跨域问题

在解释什么是`跨域问题`之前，我们需要先了解一下什么是`同源策略`。

## 同源策略

浏览器的同源策略是一种约定,是浏览器最核心也是醉基本的安全功能.如果浏览器少了同源策略,则浏览器的正常功能可能都会受到影响.

**什么是同源?**

协议、域名、端口都相同即为**同源**(注意三者条件需要同时满足)

**举例说明**

1. 第一组

   + `http://192.168.123.15/user/1`
   + `https://192.168.123.15/user/1`

   **不是**! 协议不同, 一个是http协议, 一个是https协议, 所以不满足同源条件

2. 第二组

   + `http://192.168.123.15/user/1`
   + `http://192.168.123.16/user/1`

   **不是**! IP地址(域名)不同, 不满足同源条件

3. 第三组

   + `http://192.168.123.15/user/1`
   + `http://192.168.123.15:8080/user/1`

   **不是**! 端口号不同,不满足同源条件

4. 第四组

   + `http://www.rtx3090.xyz/user/1`
   + `http://www.rtx3090.asia/user/1`

   **不是**! 域名(IP地址)不同,不满足同源条件

5. 第五组

   + `http://192.168.123.15/user/1`
   + `http://192.168.123.15/user/2`

   **是**! 协议、IP地址(域名)、端口三者都相同, 满足同源条件

解释完是什么`同源`后, 我们来说明一下什么是`跨域问题`!

## 什么是跨域问题

简单描述一下:

有两台服务器分别为A,B,如果从服务器A的页面发送异步请求到服务器B获取数据，如果服务器A和服务器B不满足同源策略，则就会出现跨域问题。

## 跨域问题案例演示

出现跨域问题会有什么效果?,接下来通过一个需求来给大家演示下:

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210907015005.png)

1. 在Nginx的html目录下新建一个`a.html` 文件, 内容如下:

   ```html
   <html>
       <head>
           <meta charset="utf-8">
           <title>跨域问题演示</title>
           <script src="jquery-3.6.0.js"></script>
           <script>
               $(function () {
                   $("#btn").click(function () {
                       $.get('http://192.168.123.15:8080/getUser',function (data) {
                           alert(JSON.stringify(data));
                       });
                   })
               })
           </script>
       </head>
       <body>
           <input type="button" value="获取数据" id="btn">
       </body>
   </html>
   ```

2. 修改Nginx配置文件, 在http块中添加一下内容

   ```conf
   server {
       listen  8080;
       server_name localhost;
   
       location /getUser {
           default_type application/json;
           return 200 '{"id":1,"name":"Jason","age":18}';
       }    
   }
   ```

3. 检查配置文件无误后, 重新加载Nginx服务器

   ```bash
   nginx -t
   
   nginx -s reload
   ```

4. 在浏览器中输入`http://192.168.123.15/a.html`来访问页面, 并点击按钮试图去访问`/getUser`请求, 发现访问失败!

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210907015530.png)

这里的访问失败, 就是因为跨域问题产生的, 因为`a.html`的端口是默认的80, 而它要访问的`/getUser`请求端口是8080, 端口不同所以不同源.

## 解决跨域问题案例演示

为了解决跨问题, 我们需要在被访问的服务页面中加入两个头信息`Access-Control-Allow-Origin`和`Access-Control-Allow-Methods`. 

至于如何添加这两个头部信息, 我们就需要用到`add_header`指令了.

**复习add_header指令**

+ 语法: `add_header name value...`
+ 默认值: `-`
+ 位置: http块、server块、location块
+ 作用: 往响应头部信息中添加信息

> `Access-Control-Allow-Origin`: 直译过来是允许跨域访问的源地址信息， 可以配置多个(多个用逗号分隔)，也可以使用 * 代表所有源
>
> `Access-Control-Allow-Methods`:  直译过来是允许跨域访问的请求方式， 值可以为 GET POST PUT DELETE...,可以全部设置，也可以根据需要设 置，多个用逗号分隔

1. 在上述案例的基础上, 对Nginx配置文件进行修改, 添加两个指令`Access-Control-Allow-Origin`和`Access-Control-Allow-Methods`

   ```conf
   server {
       listen  8080;
       server_name localhost;
   
       location /getUser {
           add_header Access-Control-Allow-Origin  *;	#新增
           add_header Access-Control-Allow-Methods GET,POST,PUT,DELETE;	#新增
           default_type application/json;
           return 200 '{"id":1,"name":"Jason","age":18}';
       }    
   }
   ```

2. 测试配置文件是否无误, 重新加载Nginx

   ```bash
   nginx -t
   
   nginx -s reload
   ```

3. 在浏览器中输入`http://192.168.123.15/a.html`来访问页面, 并点击按钮试图去访问`/getUser`请求, 发现这一次请求成功!

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210907021029.png)

# 静态资源防盗链

## 什么是资源盗链

资源盗链指的是此内容不在自己服务器上，而是通过技术手段，绕过别 人的限制将别人的内容放到自己页面上最终展示给用户。以此来盗取大 网站的空间和流量。简而言之就是用别人的东西成就自己的网站

下面我们来通过一个案例来进一步说明什么是资源盗链:

1. 找到一个可以直接在浏览器访问的图片链接

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210907021813.png)

2. 在静态资源`a.html`文件中引入这个图片的链接

   ```html
   <html>
       <head>
           <meta charset="utf-8">
           <title>跨域问题演示</title>
           <script src="jquery-3.6.0.js"></script>
           <script>
               $(function () {
                   $("#btn").click(function () {
                       $.get('http://192.168.123.15:8080/getUser',function (data) {
                           alert(JSON.stringify(data));
                       });
                   })
               })
           </script>
       </head>
       <body>
           <input type="button" value="获取数据" id="btn">
         	<!--引入外部图片-->
           <img src="https://www.google.com/imgres?imgurl=https%3A%2F%2Fimg95.699pic.com%2Fphoto%2F50046%2F5562.jpg_wh300.jpg&imgrefurl=https%3A%2F%2F699pic.com%2Ftupian%2Fai.html&tbnid=bThQJnKEBtdITM&vet=12ahUKEwjsxpnY-eryAhUsJaYKHZB8DvkQMygBegUIARDKAQ..i&docid=embemvTtTQwcHM&w=458&h=300&q=%E5%9B%BE%E7%89%87&ved=2ahUKEwjsxpnY-eryAhUsJaYKHZB8DvkQMygBegUIARDKAQ"/><br/>
       </body>
   </html>
   ```

3. 在浏览器中访问`a.html`页面,可以看到我们并看不到刚才引入的图片,但是这个图片单独在浏览器中是可以被访问的,这就是资源防盗链

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210907022044.png)

## 资源盗链案例演示

上面的资源防盗链演示,是基于别人已经对我们引入的图片做了资源防盗链的处理,但是如果我们自己的静态资源没有做防盗链的处理,会怎么样呢?

下面我们来演示一下资源盗链的情况

1. 在`nginx/html/`目录下新建目录`images`并上传添加`forest.png`图片

2. 修改nginx配置文件,添加一下内容

   ```conf
   server {
       listen  8080;
       server_name localhost;
   
       location ~ .*\.(png|jpg|gif)$ {
           root html/images;
       }
   ```

3. 然后直接在浏览器输入`http://192.168.123.15:8080/forest.png`,就可以直接访问到图片资源

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210907035211.png)

4. 在`nginx/html/`目录下新建`a.html`文件,并引入上面的图片

   ```html
   <html>
       <head>
           <meta charset="utf-8">
           <title>资源盗链案例演示</title>
       </head>
       <body>
           <img src="http://192.168.123.15:8080/forest.png"/>
       </body>
   </html>
   ```

5. 在浏览器输入`http://192.168.123.15/a.html`,来访问`a.html`页面,可以观察到我们是可以值`a.html`页面中直接访问上述图片的,这就是资源盗链

## 防盗链实现原理

了解防盗链原理之前,我们得闲学习一个HTTP的头信息`Referer`. 当浏览器向web服务器发送请求的时候,一般都会带上Referer, 来告诉浏览器该页面是从哪个页面链接过来的.

后台服务器可以根据获取到的这个Referer信息来判断是否为自己信任的网站地址,如果是则放行继续访问,如果不是则可以返回403状态码(服务端拒绝访问)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210907023354.png)

## 防盗链实现方法

为了防止盗链的出现,我们需要用到指令`valid_referers`.

### valid_referer指令

+ 语法: `valid_referers none|blocked|server_names|string...`
+ 默认值: `-`
+ 位置: server块、location块
+ 作用: 这个指令会让Nginx通过查看referer自动和valid_referers后面的内容进行匹配,如果匹配到了就将`$invalid_referer`变量重置为0.如果没有匹配到,则将`$invalid_referer`变量变为1,匹配的过程中不区分大小写.

> `valid_referer`指令值说明
>
> + `none`: 如果Header中的Referer为空,则允许访问
> + `blocked`: 在Header中的Referer不为空,但是该值被防火墙或代理进行伪装过,如不带`http://`、`http://`等协议头的资源被允许被访问
> + `server_names`: 指定具体的域名或者IP
> + `string`: 可以支持正则表达式和*字符串

所以我们为了解决资源盗链的问题,我们需要在我们的location块中加入指令`valid_referers`和值判断`invalid_referers`,下面是模版

```conf
location ~ .*\.(png|jpg|gif)$ {
    #除了请求头referers为空、被伪装、和为www.baidu.com的都要被防盗链
    valid_referers none blocked www.baidu.com;
    #如果请求头信息referers不为上面三种的任何一种,invalid_referers的值就为1
    #则进行防盗链操作,返回状态码403拒绝访问
    if ($invalid_referer) {
        return 403;
    }
    root html/images;
}
```

## 手动解决资源盗链案例演示

1. 在`资源盗链案例演示`的基础上,对nginx配置文件进行修改,添加一下内容

   ```conf
   location ~ .*\.(png|jpg|gif)$ {
       valid_referers  none blocked www.baidu.com;
       if ($invalid_referer){
           return 403;
       }
       root    html/images;
   }
   ```

2. 检查nginx配置文件无误,重新加载nginx服务器

   ```bash
   nginx -t
   
   nginx -s reload
   ```

3. 在浏览器直接输入`http://192.168.123.15:8080/forest.png`,直接访问图片,还是成功访问了

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210907044212.png)

4. 但在浏览器中输入`http://192.168.123.15/a.html`,来访问`a.html`页面,这时`a.html`页面引用的图片资源就不能被成功访问了,我们实现了防盗链

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210907044420.png)

5. 上面我们是针对文件类型来进行防盗链的,我们还可以对同一目录下的所有文件进行防盗链,无论类型. 下面我们修改nginx配置文件

   ```conf
   location /images {
       #除了请求头referers为空、被伪装、和为www.baidu.com的都要被防盗链
       valid_referers none blocked www.baidu.com;
       #如果请求头信息referers不为上面三种的任何一种,invalid_referers的值就为1
       #则进行防盗链操作,返回状态码403拒绝访问
       if ($invalid_referer) {
           return 403;
       }
       root html;
   }
   ```

6. 注意此时直接访问图片的路径变成了`http://192.168.123.15:8080/images/forest.png`,所以我们也需要修改`a.html` 中对图片的引用

   ```html
   <html>
       <head>
           <meta charset="utf-8">
           <title>资源盗链案例演示</title>
       </head>
       <body>
           <img src="http://192.168.123.15:8080/images/forest.png"/>
       </body>
   </html>
   ```

7. 再次在浏览器中输入` http://192.168.123.15/a.html`来访问`a.html`页面,此时发现图片的访问也是失败的.我们针对目录的防盗链也是成功的

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210907045719.png)

## 这种防盗链方式的问题

Referer的限制比较粗,比如随意加一个Referer,上面的方式是无法进行限制的,那么这个问题该如何解决呢?

此处我们需要用到Nginx的第三方模块`ngx_http_accesskey_module`.关于这个第三方模块,是如何实现盗链的,我们会在后面的Nginx模块篇在进行详细的讲解.

# Rewrite功能配置

## 概述

Rewrite是Nginx服务器提供的一个重要基本功能,是web服务器产品中几乎必备的功能.主要的作用是用来实现URL的重写.

> 注意:Nginx服务器的Rewrite功能的实现依赖于PCRE库的支持,因此在编译安装Nginx服务器之前,需要安装PCRE库.Nginx使用的是`ngx_http_rewrite_module`模块来解析和处理Rewrite功能的相关配置

## "地址重写"与"地址转发"的区别

+ 地址重写浏览器地址会发生变化,而地址转发则不会
+ 一次地址重写会产生两次请求,而一次地址转发只会产生一次请求
+ 地址重写到的页面必须是一个完整的路径,而地址转发则不需要
+ 地址重写因为是两次请求所以request范围内不能传递给新页面,而地址转发因为是一次请求所以可以传递值
+ 地址转发速度快于地址重写

## Rewrite规则

### set指令

+ 语法: `set $variable value;`
+ 默认值: `-`
+ 位置: server块、location块、if指令
+ 作用: 设置一个新的变量

> `variable`: 变量的名称, 该变量名称要用"$"作为变量的第一个字符,且不能与Nginx服务器预设的全局变量同名
>
> `value`: 变量的值,可以是字符串、其他变量或者变量的组合等

**Rewrite常用的全局变量**

|        变量        |                             说明                             |
| :----------------: | :----------------------------------------------------------: |
|       $args        |                       请求URL中的参数                        |
|   $query_string    |                 请求URL中的参数(与$args一致)                 |
|  $http_user_agent  |      用户访问服务的代理信息(一般是浏览器的相关版本信息)      |
|       $host        |                  访问服务器的server_name值                   |
|   $document_uri    |                      访问地址的URI链接                       |
|        $uri        |              访问地址的URI链接(与$document_uri)              |
|   $document_root   | 当前请求所对应location块中的root值;如果未设置,默认指向Nginx自带的html目录 |
|  $content_length   |                   请求头中Content-Length值                   |
|   $content_type    |                    请求头中Content_Type值                    |
|    $http_cookie    |                      客户端的cookie信息                      |
|    $limit_rate     |      Nginx服务器对网络链接速率的限制值,默认为0,即不限制      |
|    $remote_addr    |                        客户端的IP地址                        |
|    $remote_port    |                客户端与服务端建立连接的端口号                |
|    $remote_user    |                客户端的用户名(需要有认证模块)                |
|      $scheme       |                           访问协议                           |
|    $server_addr    |                         服务端的地址                         |
|    $serve_name     |                         服务端的名称                         |
|    $server_port    |                         服务端的端口                         |
|  $server_protocol  |                   客户端请求的HTTP协议版本                   |
| $request_body_file |               发给后端服务器的本地文件资源名称               |
|  $request_method   |                       客户端的请求方式                       |
| $request_filename  |                   当前请求资源文件的路径名                   |
|    $request_uri    |               当前请求的URI连接,并携带请求参数               |

**在日志文件中打印Rewrite全局变量**

1. 修改nginx配置文件,在http块中添加以下内容

   ```conf
   log_format main '$args - $remote_addr - $request - $status - $request_uri - $http_user_agent';
   
   
   #用于测试学习Rewrite
   server {
       listen 8081;
       server_name localhost;
   
       location /server {
           access_log logs/access.log main;
           root /root/html;
           set $name Jason;
           set $age 18; 
           default_type text/plain;
           return 200 "test Rewrite";
           }
   }
   ```

2. 检查配置文件无误,并重新加载nginx服务器

   ```bash
   nginx -t
   
   nginx -s reload
   ```

3. 监控日志文件

   ```bash
   tail -f /usr/local/nginx/logs/access.log
   ```

4. 在浏览器中输入`http://192.168.123.15:8081/server?username=bernardo&password=admin`,查看控制台日志文件的输出

   ```bash
   username=bernardo&password=admin - 192.168.123.124 - GET /server?username=bernardo&password=admin HTTP/1.1 - 200 - /server?username=bernardo&password=admin - Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/93.0.4577.63 Safari/537.36
   ```

### if指令

该指令用来支持条件判断,并根据条件判断结果选择不同的Nginx配置

+ 语法: `if(condition) {...}`
+ 位置: server块、location块
+ 作用: 进行条件判断,根据判断结果选择不同的Nginx配置执行

condition为判定条件，可以支持以下写法:

1. 判断条件为变量名; 如果变量名对应的值为空或者是0，if都判断为false,其他条件为true

   ```conf
   location /if {
       #设置变量username值为ROSE
       set $username 'ROSE';
       #设置响应的数据类型
       default_type text/plain;
   
       #如果变量username不为空,则返回自身的值
       #如果变量username为空,则返回一个字符串
       if ($username) {
           return 200 $username;
       }
       return 200 'param is empty';
   }
   ```

2. 判断条件可以使用`=`和`!=`逻辑判断符号,来对两个变量进行比较

   ```conf
   location /if03 {
       #设置响应的数据类型
       default_type text/plain;
   
       #请求方式若为POST,则返回状态码200和字符串
       #请求方式若不为POST,则返回状态码404
       if ($request_method = POST) {
           return 200 'request method is POST';
       }
       return 404;
   }
   ```

3. 判断条件还可以使用正则表达式与变量进行匹配, 根据匹配结果返回true或者false. 变量与正则表达式之间使用`~`、`~*`、`!~`、`!~*`来连接

   + `~`: 正则表达式匹配变量区分大小写, 匹配则返回true, 否则返回false
   + `~*`:正则表达式匹配变量不区分大小写, 匹配则返回true, 否则返回false
   + `!~`: 正则表达式匹配变量区分大小写, 匹配则返回false, 否则返回true(与`~`相反)
   + `!~*`: 正则表达式匹配变量不区分大小写,匹配则返回false, 否则返回true(与`~*`相反)

   ```conf
   location /if04 {
       #设置响应的数据类型
       default_type text/plain;
   
       #将变量与正则表达式进行匹配
       #匹配成功返回状态码200和字符串
       #匹配失败返回状态404
       if ($http_user_agent ~ Safari) {
           return 200 Chrome;
       }
       return 404;
   }
   ```

4. 判断条件还可以判断文件是否存在,当使用`-f`时,若请求文件存在返回true,不存在返回false;当使用`!-f`时,若请求文件不存在返回true,存在返回false

   ```conf
   location /if07 {
       #设置响应的数据类型
       default_type text/html;
       #设置别名路径
       alias html/b.html;
   
       #如果文件不存在,则返回状态码200和一个html页面
       if (!-f $request_filename) {
          return 200 '<h1>404 NOT FOUND</h1>';
       }
   }
   ```

5. 判断请求的目录是否存在使用"-d"和"!-d",

   当使用"-d"时，如果请求的目录存在，if返回true，如果目录不存在则返回false

   当使用"!-d"时，如果请求的目录不存在但该目录的上级目录存在则返回true，该目录和它上级目录都不存在则返回false,如果请求目录存 在也返回false.

6.  判断请求的目录或者文件是否存在使用"-e"和"!-e" 

   当使用"-e",如果请求的目录或者文件存在时，if返回true,否则返回false. 

   当使用"!-e",如果请求的文件和文件所在路径上的目录都不存在返回true,否则返回false

7. 判断请求的文件是否可执行使用"-x"和"!-x"

   当使用"-x",如果请求的文件可执行，if返回true,否则返回false 

   当使用"!-x",如果请求文件不可执行，返回true,否则返回false

### break指令

该指令用于中断当前相同作用域中的其他Nginx配置。与该指令处于同一作用域的Nginx配置中，位于它前面的指令配置生效，位于后面的指令配置无效。

| 语法   | break；                    |
| ------ | -------------------------- |
| 默认值 | -                          |
| 位置   | server块、location块、if块 |

**演示**

```bash
location /testbreak {
    default_type text/plain;
    set $username Jason;
    if ($args) {
        set $username Bernardo;
        break;
        set $username FuckYou;
    }
    add_header username $username;
    return 200 $username;
}
```

### return指令

+ 语法：`return code[text] / code URL / URL;`
+ 位置：`server块、loccation块、if块`
+ 作用：该指令用于完成对请求的处理，直接向客户端返回响应状态代码。在return后的所有Nginx配置都是无效的。

> **code:** 为返回给客户端的HTTP状态代理,可以返回的状态代码为0~999 的任意HTTP状态代理
>
> **text:** 为返回给客户端的响应体内容，支持变量的使用
>
> **URL:** 为返回给客户端的URL地址

### rewrite指令

+ 语法：`rewrite regex replacement [flag]`
+ 位置：server块、location块、if块
+ 作用：该指令通过正则表达式的使用来改变URI。可以同时存在一个或者多个指令，按照顺序依次对URL进行匹配和处理。

> **regex:** 用来匹配URI的正则表达式
>
> **eplacement:** 匹配成功后，用于替换URI中被截取内容的字符串。如果该字符串是以"http://"或者"https://"开头的，则不会继续向下对URI进行其他处理，而是直接返回重写后的URI给客户端。
>
> **flag:** 用来设置rewrite对URI的处理行为，可选值有如下:`last`、`break`、`redirect`、`permanent`

### rewrite_log指令

+ 语法：`rewrite_log on | off;`
+ 默认值：`off`
+ 位置：http块、server块、location块、if块
+ 作用：该指令配置是否开启URL重写日志的输出功能。

**演示**

```bash
location /rewrite {
		#开启日志访问功能
    rewrite_log on;
    #设置日志级别
    error_log logs/error.log notice;
    #rewrite ^/rewrite/url\w*$ https://www.baidu.com;
    rewrite ^/rewrite/(test)\w*$ /$1 permanent;
    rewrite ^/rewrite/(demo)\w*$ /$1 permanent;
}
```

## 域名跳转案例演示

### 案例需求和分析

先来看一个效果，如果我们想访问京东网站，大家都知道我们可以输入www.jd.com ,但是同样的我们也可以输入 www.360buy.com 同样也都能 访问到京东网站。这个其实是因为京东刚开始的时候域名就是www.360 buy.com，后面由于各种原因把自己的域名换成了www.jd.com, 虽然说域名变量，但是对于以前只记住了www.360buy.com的用户来说，我们如何把这部分用户也迁移到我们新域名的访问上来，针对于这个问题， 我们就可以使用Nginx中Rewrite的域名跳转来解决。

### 环境准备

+ 准备两个域名 www.360buy.com | www.jd.com

  ```bash
  vim /etc/hosts
  ```

  ```bash
  127.0.0.1 www.360buy.com 
  127.0.0.1 www.jd.com
  ```

+ 在`/usr/local/nginx/html/hm`目录下创建一个访问页面

  ```html
  <html>
      <title></title>
  <body> <h1>欢迎来到我们的网站</h1>
      </body>
  </html>
  ```

+ 通过Nginx实现当访问www.访问到系统的首页

  ```bash
  server {
      listen 80;
      server_name www.hm.com; 
      location /{
              root /usr/local/nginx/html/hm;
      				index index.html; 
      }
  }
  ```

  ```bash
  server {
      listen 80;
      server_name www.360buy.com;
      rewrite ^(.*) http://www.jd.com$1 permanent; 
  }
  ```

我们除了上述说的www.jd.com 、www.360buy.com其实还有 我们也可以通过www.jingdong.com来访问，那么如何通过Rewrite来实 现多个域名的跳转?

添加域名

```
vim /etc/hosts
192.168.200.133 www.jingdong.com
```

修改配置信息

```
server{
    listen 80;
    server_name www.360buy.com www.jingdong.com;
    rewrite ^(.*) http://www.jd.com$1 permanent; 
}
```

## 域名镜像

上述案例中，将www.360buy.com 和 www.jingdong.com都能跳转到www.jd.com，那么www.jd.com我们就可以把它起名叫主域名，其他两个就是我们所说的镜像域名，当然如果我们不想把整个网站做镜像，只想为其中某一个子目录下的资源做镜像，我们可以在location块中配置 rewrite功能，比如:

```
server {
    listen 80;
		server_name rewrite.myweb.com; 
		location ^~ /source1{
				rewrite ^/resource1(.*) http://rewrite.myweb.com/web$1 last;
		}
		location ^~ /source2{
				rewrite ^/resource2(.*) http://rewrite.myweb.com/web$1 last;
		} 
}
```

## 独立域名

一个完整的项目包含多个模块，比如购物网站有商品商品搜索模块、商品详情模块已经购物车模块等，那么我们如何为每一个模块设置独立的域名。

**需求**

```bash
http://search.hm.com 访问商品搜索模块 
http://item.hm.com 访问商品详情模块 
http://cart.hm.com 访问商品购物车模块
```

**实现**

```conf
server{
    listen 80;
		server_name search.hm.com;
		rewrite ^(.*) http://www.hm.com/bbs$1 last; 
}
server{
    listen 81;
		server_name item.hm.com;
		rewrite ^(.*) http://www.hm.com/item$1 last; 
}
server{
    listen 82;
		server_name cart.hm.com;
		rewrite ^(.*) http://www.hm.com/cart$1 last; 
}
```

## 合并目录

搜索引擎优化(SEO)是一种利用搜索引擎的搜索规则来提供目的网站的有关搜索引擎内排名的方式。我们在创建自己的站点时，可以通过很多中方式来有效的提供搜索引擎优化的程度。其中有一项就包含URL的目录 层级一般不要超过三层，否则的话不利于搜索引擎的搜索也给客户端的 输入带来了负担，但是将所有的文件放在一个目录下又会导致文件资源 管理混乱并且访问文件的速度也会随着文件增多而慢下来，这两个问题是相互矛盾的，那么使用rewrite如何解决上述问题?

举例，网站中有一个资源文件的访问路径时`/server/11/22/33/44/20.html`,也就是说20.html存在于第5级目录下，如果想要访问该资源文件，客户端的URL地址就要写成`http://www.web.name/server/11/22/33/44/20.html`

```conf
server {
    listen 80;
	server_name www.web.name; 
	location /server{
		root html; 
	}
}
```

但是这个是非常不利于SEO搜索引擎优化的，同时客户端也不好记.使用 rewrite我们可以进行如下配置:

```conf
server {
    listen 80;
		server_name www.web.name; 
		location /server{
			rewrite ^/server-([0-9]+)-([0-9]+)-([0-9]+)- ([0-9]+)\.html$ /server/$1/$2/$3/$4/$5.html last;
		} 
}
```

这样的话，客户端只需要输入`http://www.web.name/server-11-22-33- 44-20.html`就可以访问到20.html页面了。这里也充分利用了rewrite指令支持正则表达式的特性

## 防盗链

防盗链之前我们已经介绍过了相关的知识，在rewrite中的防盗链和之前 将的原理其实都是一样的，只不过通过rewrite可以将防盗链的功能进行 完善下，当出现防盗链的情况，我们可以使用rewrite将请求转发到自定 义的一张图片和页面，给用户比较好的提示信息。下面我们就通过根据文件类型实现防盗链的一个配置实例:

```conf
server{
    listen 80;
		server_name www.web.com;
		locatin ~* ^.+\.(gif|jpg|png|swf|flv|rar|zip)${
				valid_referers none blocked server_names *.web.com;
     		if ($invalid_referer){
            rewrite ^/	http://www.web.com/images/forbidden.png; 
        }
		}
}
```

根据目录实现防盗链配置:

```conf
server{
    listen 80;
		server_name www.web.com;
		locatin /file/{
				root /server/file/;
				valid_referers none blocked server_names *.web.com;
     		if ($invalid_referer){
            rewrite ^/	http://www.web.com/images/forbidden.png; 
        }
		}
}
```

# Nginx反向代理

## 概述

关于正向代理和反向代理，我们在前面的章节已经通过一张图给大家详 细的介绍过了，简而言之就是正向代理代理的对象是客户端，反向代理代理的是服务端，这是两者之间最大的区别。

## 正向代理

我们先来通过一个小案例演示下Nginx正向代理的简单应用。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210916143737.png)

代理服务器配置文件设置

```conf
server {
    listen      82;
    resolver    8.8.8.8; #设置DNS的IP 用来解析proxy_pass中的域名
    location / {
    		#主要是这个设置正向代理
        proxy_pass http://$host$request_uri;
    }

```

## 反向代理配置语法

Nginx反向代理模块的指令是由`ngx_http_proxy_module`模块进行解析，该模块在安装Nginx的时候已经自己加装到Nginx中了，接下来我们把反向代理中的常用指令一一介绍下:

+ `proxy_pass`
+ `proxy_set_header`
+ `proxy_redirect`

### proxy_pass指令

+ 语法：`proxy_pass URL;`
+ 位置：`location`
+ 作用：该指令用来设置被代理服务器地址，可以是主机名称、IP地址加端口号形式

> **URL:** 为要设置的被代理服务器地址，包含传输协议( 主机名称或IP地址加端口号、URI等要素

**演示**

```bash
server {
    listen 8080;
    server_name localhost;
    location /server {
        proxy_pass http://192.168.123.98/;
    }
}
```

> 关于`proxy_pass`后的地址结尾的`/`
>
> + 不加`/`，则会将`location`后`/server`给拼接上
> + 加`/`，则不会拼接`location`后的`/server`

### proxy_set_header指令

+ 语法：`proxy_set_header Host $proxy_host;`、`proxy_set_header Connection close;`
+ 位置：http块、server块、location块
+ 作用：该指令可以更改Nginx服务器接收到的客户端请求的请求头信息，然后将新的请求头发送给代理的服务器

> 需要注意的是，如果想要看到结果，必须在被代理的服务器上来获取添加的头信息。

**演示**

+ 代理服务器配置文件

  ```conf
  server {
      listen 8080;
      server_name localhost;
      location /server {
          proxy_pass http://192.168.123.98:8080/;
          proxy_set_header    username    Jason;
      }
  ```

+ 被代理服务器配置文件

  ```conf
  server {
      listen      8080;
      server_name localhost;
      location / {
          default_type    text/plain;
          return      200 $http_username;
      }
  ```

### proxy_redirect指令

+ 语法：
+ 默认值：
+ 位置：
+ 作用：

## Nginx反向代理实战

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210917135620.png)

服务器1,2,3存在两种情况

```
第一种情况: 三台服务器的内容不一样。
第二种情况: 三台服务器的内容是一样。
```

1. 如果服务器1、服务器2和服务器3的内容不一样，那我们可以根据用户请求来分发到不同的服务器。

```
代理服务器
server {
        listen          8082;
        server_name     localhost;
        location /server1 {
                proxy_pass http://192.168.200.146:9001/;
        }
        location /server2 {
                proxy_pass http://192.168.200.146:9002/;
        }
        location /server3 {
                proxy_pass http://192.168.200.146:9003/;
        }
}

服务端
server1
server {
        listen          9001;
        server_name     localhost;
        default_type text/html;
        return 200 '<h1>192.168.200.146:9001</h1>'
}
server2
server {
        listen          9002;
        server_name     localhost;
        default_type text/html;
        return 200 '<h1>192.168.200.146:9002</h1>'
}
server3
server {
        listen          9003;
        server_name     localhost;
        default_type text/html;
        return 200 '<h1>192.168.200.146:9003</h1>'
}
```

2. 如果服务器1、服务器2和服务器3的内容是一样的，该如何处理?

## Nginx的安全控制

关于web服务器的安全是比较大的一个话题，里面所涉及的内容很多，Nginx反向代理是如何来提升web服务器的安全呢？

```
安全隔离
```

**什么是安全隔离?**

通过代理分开了客户端到应用程序服务器端的连接，实现了安全措施。在反向代理之前设置防火墙，仅留一个入口供代理服务器访问。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210917135642.png)

### 如何使用SSL对流量进行加密

翻译成大家能熟悉的说法就是将我们常用的http请求转变成https请求，那么这两个之间的区别简单的来说两个都是HTTP协议，只不过https是身披SSL外壳的http.

HTTPS是一种通过计算机网络进行安全通信的传输协议。它经由HTTP进行通信，利用SSL/TLS建立全通信，加密数据包，确保数据的安全性。

SSL(Secure Sockets Layer)安全套接层

TLS(Transport Layer Security)传输层安全

上述这两个是为网络通信提供安全及数据完整性的一种安全协议，TLS和SSL在传输层和应用层对网络连接进行加密。

总结来说为什么要使用https:

```
http协议是明文传输数据，存在安全问题，而https是加密传输，相当于http+ssl，并且可以防止流量劫持。
```

Nginx要想使用SSL，需要满足一个条件即需要添加一个模块`--with-http_ssl_module`,而该模块在编译的过程中又需要OpenSSL的支持，这个我们之前已经准备好了。

### nginx添加SSL的支持

（1）完成 `--with-http_ssl_module`模块的增量添加

```
》将原有/usr/local/nginx/sbin/nginx进行备份
》拷贝nginx之前的配置信息
》在nginx的安装源码进行配置指定对应模块  ./configure --with-http_ssl_module
》通过make模板进行编译
》将objs下面的nginx移动到/usr/local/nginx/sbin下
》在源码目录下执行  make upgrade进行升级，这个可以实现不停机添加新模块的功能
```

### Nginx的SSL相关指令

因为刚才我们介绍过该模块的指令都是通过ngx_http_ssl_module模块来解析的。

》ssl:该指令用来在指定的服务器开启HTTPS,可以使用 listen 443 ssl,后面这种方式更通用些。

| 语法   | ssl on \| off; |
| ------ | -------------- |
| 默认值 | ssl off;       |
| 位置   | http、server   |

```
server{
	listen 443 ssl;
}
```

》ssl_certificate:为当前这个虚拟主机指定一个带有PEM格式证书的证书。

| 语法   | ssl_certificate file; |
| ------ | --------------------- |
| 默认值 | —                     |
| 位置   | http、server          |

》ssl_certificate_key:该指令用来指定PEM secret key文件的路径

| 语法   | ssl_ceritificate_key file; |
| ------ | -------------------------- |
| 默认值 | —                          |
| 位置   | http、server               |

》ssl_session_cache:该指令用来配置用于SSL会话的缓存

| 语法   | ssl_sesion_cache off\|none\|[builtin[:size]] [shared:name:size] |
| ------ | ------------------------------------------------------------ |
| 默认值 | ssl_session_cache none;                                      |
| 位置   | http、server                                                 |

off:禁用会话缓存，客户端不得重复使用会话

none:禁止使用会话缓存，客户端可以重复使用，但是并没有在缓存中存储会话参数

builtin:内置OpenSSL缓存，仅在一个工作进程中使用。

shared:所有工作进程之间共享缓存，缓存的相关信息用name和size来指定

》ssl_session_timeout：开启SSL会话功能后，设置客户端能够反复使用储存在缓存中的会话参数时间。

| 语法   | ssl_session_timeout time; |
| ------ | ------------------------- |
| 默认值 | ssl_session_timeout 5m;   |
| 位置   | http、server              |

》ssl_ciphers:指出允许的密码，密码指定为OpenSSL支持的格式

| 语法   | ssl_ciphers ciphers;          |
| ------ | ----------------------------- |
| 默认值 | ssl_ciphers HIGH:!aNULL:!MD5; |
| 位置   | http、server                  |

可以使用`openssl ciphers`查看openssl支持的格式。

》ssl_prefer_server_ciphers：该指令指定是否服务器密码优先客户端密码

| 语法   | ssl_perfer_server_ciphers on\|off; |
| ------ | ---------------------------------- |
| 默认值 | ssl_perfer_server_ciphers off;     |
| 位置   | http、server                       |

### 生成证书

方式一：使用阿里云/腾讯云等第三方服务进行购买。

方式二:使用openssl生成证书

先要确认当前系统是否有安装openssl

```
openssl version
```

安装下面的命令进行生成

```
mkdir /root/cert
cd /root/cert
openssl genrsa -des3 -out server.key 1024
openssl req -new -key server.key -out server.csr
cp server.key server.key.org
openssl rsa -in server.key.org -out server.key
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
```

### 开启SSL实例

```
server {
    listen       443 ssl;
    server_name  localhost;

    ssl_certificate      server.cert;
    ssl_certificate_key  server.key;

    ssl_session_cache    shared:SSL:1m;
    ssl_session_timeout  5m;

    ssl_ciphers  HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers  on;

    location / {
        root   html;
        index  index.html index.htm;
    }
}
```

（4）验证

### 反向代理系统调优

反向代理值Buffer和Cache

Buffer翻译过来是"缓冲"，Cache翻译过来是"缓存"。

![1581879638569](/Users/bernardo/Learning/Nginx/assets/1581879638569.png)

总结下：

```
相同点:
两种方式都是用来提供IO吞吐效率，都是用来提升Nginx代理的性能。
不同点:
缓冲主要用来解决不同设备之间数据传递速度不一致导致的性能低的问题，缓冲中的数据一旦此次操作完成后，就可以删除。
缓存主要是备份，将被代理服务器的数据缓存一份到代理服务器，这样的话，客户端再次获取相同数据的时候，就只需要从代理服务器上获取，效率较高，缓存中的数据可以重复使用，只有满足特定条件才会删除.
```

（1）Proxy Buffer相关指令

》proxy_buffering :该指令用来开启或者关闭代理服务器的缓冲区；

| 语法   | proxy_buffering on\|off; |
| ------ | ------------------------ |
| 默认值 | proxy_buffering on;      |
| 位置   | http、server、location   |

》proxy_buffers:该指令用来指定单个连接从代理服务器读取响应的缓存区的个数和大小。

| 语法   | proxy_buffers number size;                |
| ------ | ----------------------------------------- |
| 默认值 | proxy_buffers 8 4k \| 8K;(与系统平台有关) |
| 位置   | http、server、location                    |

number:缓冲区的个数

size:每个缓冲区的大小，缓冲区的总大小就是number*size

》proxy_buffer_size:该指令用来设置从被代理服务器获取的第一部分响应数据的大小。保持与proxy_buffers中的size一致即可，当然也可以更小。

| 语法   | proxy_buffer_size size;                     |
| ------ | ------------------------------------------- |
| 默认值 | proxy_buffer_size 4k \| 8k;(与系统平台有关) |
| 位置   | http、server、location                      |

》proxy_busy_buffers_size：该指令用来限制同时处于BUSY状态的缓冲总大小。

| 语法   | proxy_busy_buffers_size size;    |
| ------ | -------------------------------- |
| 默认值 | proxy_busy_buffers_size 8k\|16K; |
| 位置   | http、server、location           |

》proxy_temp_path:当缓冲区存满后，仍未被Nginx服务器完全接受，响应数据就会被临时存放在磁盘文件上，该指令设置文件路径

| 语法   | proxy_temp_path  path;      |
| ------ | --------------------------- |
| 默认值 | proxy_temp_path proxy_temp; |
| 位置   | http、server、location      |

注意path最多设置三层。  

》proxy_temp_file_write_size：该指令用来设置磁盘上缓冲文件的大小。

| 语法   | proxy_temp_file_write_size size;    |
| ------ | ----------------------------------- |
| 默认值 | proxy_temp_file_write_size 8K\|16K; |
| 位置   | http、server、location              |

通用网站的配置

```
proxy_buffering on;
proxy_buffer_size 4 32k;
proxy_busy_buffers_size 64k;
proxy_temp_file_write_size 64k;
```

根据项目的具体内容进行相应的调节。

# 负载均衡

## 概述

早期的网站流量和业务功能都比较简单，单台服务器足以满足基本的需求，但是随着互联网的发展，业务流量越来越大并且业务逻辑也跟着越来越复杂，单台服务器的性能及单点故障问题就凸显出来了，因此需要多台服务器进行性能的水平扩展及避免单点故障出现。那么如何将不同用户的请求流量分发到不同的服务器上呢?

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210917161846.png)

## 原理及流程

系统的扩展可以分为**纵向扩展**和**横向扩展**。

**纵向扩展**是从单机的角度出发，通过增加系统的硬件处理能力来提升服 务器的处理能力

**横向扩展**是通过添加机器来满足大型网站服务的处理能力。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210918072223.png)

这里面涉及到两个重要的角色分别是"应用集群"和"负载均衡器"。 

**应用集群:**将同一应用部署到多台机器上，组成处理集群，接收负载均衡设备分发的请求，进行处理并返回响应的数据。

**负载均衡器:**将用户访问的请求根据对应的负载均衡算法，分发到集群中的一台服务器进行处理。

## 负载均衡作用

1. 解决服务器的高并发压力，提高应用程序的处理性能。
2. 提供故障转移，实现高可用。
3. 通过添加或减少服务器数量，增强网站的可扩展性。
4. 在负载均衡器上进行过滤，可以提高系统的安全性。

## 负载均衡常用的处理方式

### 方式一:用户手动选择

这种方式比较原始，只要实现的方式就是在网站主页上面提供不同线路、不同服务器链接方式，让用户来选择自己访问的具体服务器，来实现负载均衡。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210918072418.png)

### 方式二:DNS轮询方式

> 域名系统(服务)协议(DNS)是一种分布式网络目录服务，主要用于域 名与 IP 地址的相互转换。

大多域名注册商都支持对同一个主机名添加多条A记录，这就是DNS轮询，DNS服务器将解析请求按照A记录的顺序，随机分配到不同的IP上， 这样就能完成简单的负载均衡。DNS轮询的成本非常低，在一些不重要的服务器被经常使用。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210918072504.png)

我们发现使用DNS来实现轮询，不需要投入过多的成本，虽然DNS轮询 成本低廉，但是DNS负载均衡存在明显的缺点。

1. **可靠性低**

   假设一个域名DNS轮询多台服务器，如果其中的一台服务器发生故障， 那么所有的访问该服务器的请求将不会有所回应，即使你将该服务器的IP从DNS中去掉，但是由于各大宽带接入商将众多的DNS存放在缓存中，以节省访问时间，导致DNS不会实时更新。所以DNS轮流上一定程 度上解决了负载均衡问题，但是却存在可靠性不高的缺点。

2. **负载均衡不均衡**

   DNS负载均衡采用的是简单的轮询负载算法，不能区分服务器的差异， 不能反映服务器的当前运行状态，不能做到为性能好的服务器多分配请求，另外本地计算机也会缓存已经解析的域名到IP地址的映射，这也会导致使用该DNS服务器的用户在一定时间内访问的是同一台Web服务器，从而引发Web服务器减的负载不均衡。

   负载不均衡则会导致某几台服务器负荷很低，而另外几台服务器负荷确很高，处理请求的速度慢，配置高的服务器分配到的请求少，而配置低的服务器分配到的请求多。

### 方式三:四/七层负载均衡

#### OSI四/七层协议

介绍四/七层负载均衡之前，我们先了解一个概念，OSI(open system interconnection),叫开放式系统互联模型，这个是由国际标准化组织ISO 指定的一个不基于具体机型、操作系统或公司的网络体系结构。该模型将网络通信的工作分为七层。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210918072707.png)

**应用层:**为应用程序提供网络服务。 

**表示层:**对数据进行格式化、编码、加密、压缩等操作。 

**会话层:**建立、维护、管理会话连接。 

**传输层:**建立、维护、管理端到端的连接，常见的有TCP/UDP。 

**网络层:**IP寻址和路由选择 

**数据链路层:**控制网络层与物理层之间的通信。 

**物理层:**比特流传输。

#### 四层负载均衡

**所谓四层负载均衡指的是OSI七层模型中的传输层，主要是基于IP+PORT 的负载均衡**

实现四层负载均衡的方式: 

	+ 硬件:F5 BIG-IP、Radware等 
	+ 软件:LVS、Nginx、Hayproxy等

#### 七层负载均衡

**所谓的七层负载均衡指的是在应用层，主要是基于虚拟的URL或主机IP 的负载均衡**

实现七层负载均衡的方式: 

	+ 软件:Nginx、Hayproxy等

#### 四层和七层负载均衡的区别

四层负载均衡数据包是在底层就进行了分发，而七层负载均衡数据包则在最顶端进行分发，所以四层负载均衡的效率比七层负载均衡的要高。 

四层负载均衡不识别域名，而七层负载均衡识别域名。

> 处理四层和七层负载以为其实还有二层、三层负载均衡，
>
> 二层是在数据 链路层基于mac地址来实现负载均衡
>
> 三层是在网络层一般采用虚拟IP地址的方式实现负载均衡。
>
> 实际环境采用的模式: 四层负载(LVS)+七层负载(Nginx)

## Nginx七层负载均衡

### 概述

Nginx要实现七层负载均衡需要用到`proxy_pass`代理模块配置。Nginx默认安装支持这个模块，我们不需要再做任何处理。Nginx的负载均衡是在Nginx的反向代理基础上把用户的请求根据指定的算法分发到一组 【upstream虚拟服务池】。

### 指令

#### upstream指令

+ 语法：`upstream name{...}`
+ 位置：http块
+ 作用：该指令是用来定义一组服务器，它们可以是监听不同端口的服务器，并且也可以是同时监听TCP和Unix socket的服务器。服务器可以指定不同的权重，默认为1。

#### server指令

+ 语法：`server name [paramerters]`
+ 位置：upstream块中
+ 作用：该指令用来指定后端服务器的名称和一些参数，可以使用域名、IP、端口或者unix socket

### 实现流程

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210918073630.png)

### 案例演示

+ 代理服务器

  ```conf
  upstream backend {
      server  192.168.123.98:9001 down;
      server  192.168.123.98:9002 backup;
      server  192.168.123.98:9003;
  }
  
  server {
      listen      8083;
      server_name localhost;
      location / {
          proxy_pass  http://backend;
      }
  }
  ```

+ 被代理服务器

  ```conf
  server {
      listen      9001;
      server_name localhost;
      default_type    text/html;
      location / {
          return  200 '<h1>192.168.123.98:9001</h1>';
      }
  }
  
  server {
      listen      9002;
      server_name localhost;
      default_type    text/html;
      location / {
          return  200 '<h1>192.168.123.98:9002</h1>';
      }
  }
  
  server {
      listen      9003;
      server_name localhost;
      default_type    text/html;
      location / {
          return  200 '<h1>192.168.123.98:9003</h1>';
      }
  }
  ```

### 负载均衡状态

#### 概述

代理服务器在负责均衡调度中的状态有以下几个:

|     状态     |               概述                |
| :----------: | :-------------------------------: |
|     down     |  当前的server暂时不参与负载均衡   |
|    backup    |         预留的备份服务器          |
|  max_fails   |        允许请求失败的次数         |
| fail_timeout | 经过max_fails失败后, 服务暂停时间 |
|  max_conns   |       限制最大的接收连接数        |

#### down

将该服务器标记为永久不可用，那么该代理服务器将不参与负载均衡。

```conf
upstream backend{
		#将这台服务器设置不参与负载均衡
		server 192.168.200.146:9001 down; 
		server 192.168.200.146:9002;
		server 192.168.200.146:9003;
}
server {
    listen 8083;
    server_name localhost;
    location /{
        proxy_pass http://backend;
    }
}
```

> 该状态一般会对需要停机维护的服务器进行设置。

#### backup

将该服务器标记为备份服务器，当主服务器不可用时，将用来传递请求。当主服务器恢复可用时，备份服务器再次恢复到备用状态不可用。

```conf
upstream backend{
		server 192.168.200.146:9001;
    #将这台服务器设置为备份服务器
		server 192.168.200.146:9002 backup; 
		server 192.168.200.146:9003;
}
server {
    listen 8083;
    server_name localhost;
    location /{
        proxy_pass http://backend;
    }
}
```

#### max_conns

用来设置代理服务器同时活动链接的最大数量， 默认为0，表示不限制，使用该配置可以根据后端服务器处理请求的并发 量来进行设置，防止后端服务器被压垮。

#### max_fails

设置允许请求代理服务器失败的次数，默认为1。

#### fail_timeout

设置经过max_fails失败后，服务暂停的时间，默认是10秒。

```conf
upstream backend {
    server  192.168.123.98:9001 down;
    server  192.168.123.98:9002 backup;
    #设置连接超时次数为3，连接超时时间为15
    server  192.168.123.98:9003 max_fails=3 fail_timeout=15;
}

server {
    listen      8083;
    server_name localhost;
    location / {
        proxy_pass  http://backend;
    }
}
```

### 负载均衡策略

#### 概述

介绍完Nginx负载均衡的相关指令后，我们已经能实现将用户的请求分发到不同的服务器上，那么除了采用默认的分配方式以外，我们还能采用什么样的负载算法?

Nginx的upstream支持如下六种方式的分配算法，分别是:

|  算法名称  |       说明       |
| :--------: | :--------------: |
|    轮询    |     默认方式     |
|   weight   |     权重方式     |
|  ip_hash   |  依据ip分配方式  |
| least_conn | 依据最少连接方式 |
|  url_hash  | 依据URL分配方式  |
|    fair    | 依据响应时间方式 |

#### 轮询

upstream模块负载均衡默认的策略。每个请求会按时间顺序逐个分配到不同的后端服务器。轮询不需要额外的配置。

```conf
upstream backend{
		#不需要额外配置，默认采用了轮询
		server 192.168.200.146:9001; 
		server 192.168.200.146:9002;
		server 192.168.200.146:9003;
}
server {
    listen 8083;
    server_name localhost;
    location /{
        proxy_pass http://backend;
    }
}
```

#### weight加权【加权轮询】

用来设置服务器的权重默认为1，权重数据越大被分配到请求的几率越大; 该权重值主要是针对实际工作环境中不同的后端服务器硬件配置进行调整的，此策略比较适合服务器的硬件配置差别比较大的情况。

```conf
upstream backend{
		#weight加权	
		server 192.168.200.146:9001 weight=10; 
		server 192.168.200.146:9002 weight=5; 
		server 192.168.200.146:9003 weight=3;
}
server {
    listen 8083;
    server_name localhost;
    location /{
        proxy_pass http://backend;
    }
}
```

#### ip_hash

当对后端的多台动态应用服务器做负载均衡时，ip_hash指令能够将某个客户端IP的请求通过哈希算法定位到同一台后端服务器上。这样当来 自某一个IP的用户在后端Web服务器A上登录后，在访问该站点的其他 URL能保证其访问的还是后端web服务器A。

```conf
upstream backend{
    ip_hash;
		server 192.168.200.146:9001; 
		server 192.168.200.146:9002; 
		server 192.168.200.146:9003;
}
server {
    listen 8083;
    server_name localhost;
    location /{
        proxy_pass http://backend;
    }
}
```

> 需要额外多说一点的是使用ip_hash指令无法保证后端服务器的负载均衡，可能导致有些后端服务器接收到的请求多，有些后端服务器接收的请求少，而且设置后端服务器权重等方法将不起作用。

#### least_conn

最少连接把请求转发给连接数较少的后端服务器。轮询算法是把请求平均的转发给各个后端，使它们的负载大致相同;但是有些请求占用 的时间很长，会导致其所在的后端负载较高。这种情况下least_conn这种方式就可以达到更好的负载均衡效果。此负载均衡策略适合请求处理时间长短不一造成服务器过载的情况。

```conf
upstream backend{
    least_conn;
		server 192.168.200.146:9001; 
		server 192.168.200.146:9002; 
		server 192.168.200.146:9003;
}
server {
    listen 8083;
    server_name localhost;
    location /{
        proxy_pass http://backend;
    }
}
```

#### url_hash

按访问url的hash结果来分配请求，使每个url定向到同一个后端服务器，要配合缓存命中来使用。同一个资源多次请求可能会到达不同的服务器上，导致不必要的多次下载，缓存命中率不高，以及一些资源时间的浪费。而使用url_hash可以使得同一个url(也就是同一个资源请求)会到达同一台服务器一旦缓存住了资源，再此收到请求就可以从缓存中读取。

```conf
upstream backend{
		hash &request_uri;
		server 192.168.200.146:9001; 
		server 192.168.200.146:9002; 
		server 192.168.200.146:9003;
}
server {
    listen 8083;
    server_name localhost;
    location /{
        proxy_pass http://backend;
    }
}
```

#### fair

fair采用的不是内建负载均衡使用的轮换的均衡算法，而是可以根据页面 大小、加载时间长短智能的进行负载均衡。那么如何使用第三方模块的 fair负载均衡策略。

```conf
upstream backend{
    fair;
		server 192.168.200.146:9001; 
		server 192.168.200.146:9002; 
		server 192.168.200.146:9003;
}
server {
    listen 8083;
    server_name localhost;
    location /{
        proxy_pass http://backend;
    }
}
```

但是如何直接使用会报错，因为fair属于第三方模块实现的负载均衡。需要添加`nginx-upstream-fair` ,如何添加对应的模块:

1. 下载nginx-upstream-fair模块`https://github.com/gnosek/nginx-upstream-fair`
2. 将下载的文件上传到服务器并进行解压缩`unzip nginx-upstream-fair-master.zip`
3. 重命名资源`mv nginx-upstream-fair-master fair`
4. 使用进入nginx源码根目录将资源添加到Nginx模块中`./configure --add-module=/root/fair`
5. 修改`src/http/ngx_http_upstream.h`文件，在`ngx_http_upstream_srv_conf_s`块中加入`in_port_t default_port`
6. 回到nginx源码根目录进行编译`make`
7. 将nginx安装根目录下sbin目录下的nginx二进制进行备份`mv /usr/local/nginx/sbin/nginx /usr/local/nginx/sbin/nginxold`
8. 将源码目录下`objs`下nginx二进制文件拷贝到安装目录下`sbin`目录下`cp /root/core/nginx-1.16.1/objs/nginx /usr/local/nginx/sbin`
9. 回到nginx源码根目录下执行`make upgrade`

#### 案例

1. 对特定资源实现负载均衡

   ```conf
   upstream videobackend{
   	server 192.168.200.146:9001;
   	server 192.168.200.146:9002;
   }
   upstream filebackend{
   	server 192.168.200.146:9003;
   	server 192.168.200.146:9004;
   }
   server {
   	listen 8084;
   	server_name localhost;
   	location /video/ {
   		proxy_pass http://videobackend;
   	}
   	location /file/ {
   		proxy_pass http://filebackend;
   	}
   }
   ```

2. 对不同域名实现负载均衡

   ```conf
   upstream itcastbackend{
   	server 192.168.200.146:9001;
   	server 192.168.200.146:9002;
   }
   upstream itheimabackend{
   	server 192.168.200.146:9003;
   	server 192.168.200.146:9004;
   }
   server {
   	listen	8085;
   	server_name www.itcast.cn;
   	location / {
   		proxy_pass http://itcastbackend;
   	}
   }
   server {
   	listen	8086;
   	server_name www.itheima.cn;
   	location / {
   		proxy_pass http://itheimabackend;
   	}
   }
   ```

3. 实现带有URL重写的负载均衡

   ```conf
   upstream backend{
   	server 192.168.200.146:9001;
   	server 192.168.200.146:9002;
   	server 192.168.200.146:9003;
   }
   server {
   	listen	80;
   	server_name localhost;
   	location /file/ {
   		rewrite ^(/file/.*) /server/$1 last;
   	}
   	location / {
   		proxy_pass http://backend;
   	}
   }
   ```

## Nginx四层负载均衡

### 概述

Nginx在1.9之后，增加了一个stream模块，用来实现四层协议的转发、 代理、负载均衡等。stream模块的用法跟http的用法类似，允许我们配置一组TCP或者UDP等协议的监听，然后通过proxy_pass来转发我们的请求，通过upstream添加多个后端服务，实现负载均衡。

四层协议负载均衡的实现，一般都会用到LVS、HAProxy、F5等，要么很贵要么配置很麻烦，而Nginx的配置相对来说更简单，更能快速完成工作。

### 添加stream模块的支持

Nginx默认是没有编译这个模块的，需要使用到stream模块，那么需要在编译的时候加上 --with-stream 。 完成添加 --with-stream 的实现步骤:

1. 将原有/usr/local/nginx/sbin/nginx进行备份
2. 拷贝nginx之前的配置信息
3. 在nginx的安装源码进行配置指定对应模块 ./configure --with-stream
4. 通过make模板进行编译
5. 将objs下面的nginx移动到/usr/local/nginx/sbin下
6. 在源码目录下执行 make upgrade进行升级，这个可以实现不停机添加新模块的功能

### 指令

#### stream指令

该指令提供在其中指定流服务器指令的配置文件上下文，和http指令同级。

#### upstream指令

该指令和http的upstream指令是类似的。

### 案例

#### 需求分析

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210918113655.png)

#### 实现步骤

(1)准备Redis服务器,在一条服务器上准备三个Redis，端口分别是6379,6378

1.上传redis的安装包，`redis-4.0.14.tar.gz`

2.将安装包进行解压缩

```
tar -zxf redis-4.0.14.tar.gz
```

3.进入redis的安装包

```
cd redis-4.0.14
```

4.使用make和install进行编译和安装

```
make PREFIX=/usr/local/redis/redis01 install
```

5.拷贝redis配置文件`redis.conf`到/usr/local/redis/redis01/bin目录中

```
cp redis.conf	/usr/local/redis/redis01/bin
```

6.修改redis.conf配置文件

```
port  6379      #redis的端口
daemonize yes   #后台启动redis
```

7.将redis01复制一份为redis02

```
cd /usr/local/redis
cp -r redis01 redis02
```

8.将redis02文件文件夹中的redis.conf进行修改

```
port  6378      #redis的端口
daemonize yes   #后台启动redis
```

9.分别启动，即可获取两个Redis.并查看

```
ps -ef | grep redis
```

使用Nginx将请求分发到不同的Redis服务器上。

(2)准备Tomcat服务器.

1.上传tomcat的安装包，`apache-tomcat-8.5.56.tar.gz`

2.将安装包进行解压缩

```
tar -zxf apache-tomcat-8.5.56.tar.gz
```

3.进入tomcat的bin目录

```
cd apache-tomcat-8.5.56/bin
./startup
```

nginx.conf配置

```
stream {
        upstream redisbackend {
                server 192.168.200.146:6379;
                server 192.168.200.146:6378;
        }
        upstream tomcatbackend {
        		server 192.168.200.146:8080;
        }
        server {
            listen  81;
            proxy_pass redisbackend;
        }
        server {
        		listen	82;
        		proxy_pass tomcatbackend;
        }
}
```

访问测试。

# 缓存集成

## 概念

缓存就是数据交换的缓冲区(称作:Cache),当用户要获取数据的时候会先从缓存中去查询获取数据，如果缓存中有就会直接返回给用户，如果缓存中没有则会发请求从服务器重新查询数据，将数据返回给用户的同时将数据放入缓存，下次用户就会直接从缓存中获取数据。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210918120618.png)

缓存其实在很多场景中都有用到，比如：

| 场景             | 作用                     |
| ---------------- | ------------------------ |
| 操作系统磁盘缓存 | 减少磁盘机械操作         |
| 数据库缓存       | 减少文件系统的IO操作     |
| 应用程序缓存     | 减少对数据库的查询       |
| Web服务器缓存    | 减少对应用服务器请求次数 |
| 浏览器缓存       | 减少与后台的交互次数     |

**缓存的优点**

1. 减少数据传输，节省网络流量，加快响应速度，提升用户体验；
2. 减轻服务器压力；
3. 提供服务端的高可用性；

**缓存的缺点**

1. 数据的不一致
2. 增加成本

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210918120740.png)

本次课程注解讲解的是Nginx, Nginx作为web服务器，Nginx作为Web缓存服务器，它介于客户端和应用服务器之间，当用户通过浏览器访问一 个URL时，web缓存服务器会去应用服务器获取要展示给用户的内容， 将内容缓存到自己的服务器上，当下一次请求到来时，如果访问的是同一个URL，web缓存服务器就会直接将之前缓存的内容返回给客户端，而不是向应用服务器再次发送请求。web缓存降低了应用服务器、数据库的负载，减少了网络延迟，提高了用户访问的响应速度，增强了用户的体验。

Nginx是从0.7.48版开始提供缓存功能。Nginx是基于Proxy Store来实现的，其原理是把URL及相关组合当做Key,在使用MD5算法对Key进行哈希，得到硬盘上对应的哈希目录路径，从而将缓存内容保存在该目录中。它可以支持任意URL连接，同时也支持404/301/302这样的非200状态码。Nginx即可以支持对指定URL或者状态码设置过期时间，也可以使用purge命令来手动清除指定URL的缓存。

## 指令

Nginx的web缓存服务主要是使用`ngx_http_proxy_module`模块相关指令集来完成，接下来我们把常用的指令来进行介绍下。

### proxy_cache_path指令

+ 语法：`proxy_cache_path path [levels=number] keys_zone=zone_name:zone_size [inactive=time]\[max_size=size];`
+ 位置：http块 
+ 作用：指定用于设置缓存文件的存放路径

> **path:** 缓存路径地址,如：`/usr/local/proxy_cache`
>
> **levels:** 指定该缓存空间对应的目录，最多可以设置3层
>
> ```
> levels=1:2   缓存空间有两层目录，第一次是1个字母，第二次是2个字母
> 举例说明:
> itheima[key]通过MD5加密以后的值为 43c8233266edce38c2c9af0694e2107d
> levels=1:2   最终的存储路径为/usr/local/proxy_cache/d/07
> levels=2:1:2 最终的存储路径为/usr/local/proxy_cache/7d/0/21
> levels=2:2:2 最终的存储路径为??/usr/local/proxy_cache/7d/10/e2
> ```
>
> **keys_zone:** 用来为这个缓存区设置名称和指定大小，如：`keys_zone=itcast:200m  缓存区的名称是itcast,大小为200M,1M大概能存储8000个keys`
>
> **inactive:** 指定缓存的数据多次时间未被访问就将被删除，如：`inactive=1d   缓存数据在1天内没有被访问就会被删除`
>
> **max_size:** 设置最大缓存空间，如果缓存空间存满，默认会覆盖缓存时间最长的资源，如 `max_size=20g`

**演示**

```conf
http{
	proxy_cache_path /usr/local/proxy_cache keys_zone=itcast:200m  levels=1:2:1 inactive=1d max_size=20g;
}
```

### proxy_cache指令

+ 语法：`proxy_cache zone_name\|off;`
+ 默认值：`off`
+ 位置：http、server、location
+ 作用：用来开启或关闭代理缓存，如果是开启则自定使用哪个缓存区来进行缓存。

> **zone_name：**指定使用缓存区的名称

### proxy_cache_key指令

+ 语法：`proxy_cache_key key;`
+ 默认值：
+ 位置：
+ 作用：用来设置web缓存的key值，Nginx会根据key值MD5哈希存缓存。

### proxy_cache_valid指令

+ 语法：`proxy_cache_valid [code ...] time;`
+ 位置：http、server、location
+ 作用：用来对不同返回状态码的URL设置不同的缓存时间

> 举例：
>
> ```
> proxy_cache_valid 200 302 10m;
> proxy_cache_valid 404 1m;
> 为200和302的响应URL设置10分钟缓存，为404的响应URL设置1分钟缓存
> proxy_cache_valid any 1m;
> 对所有响应状态码的URL都设置1分钟缓存
> ```

### proxy_cache_min_uses指令

+ 语法：`proxy_cache_min_uses number;`
+ 默认值：`proxy_cache_methods GET HEAD;`
+ 位置：http、server、location
+ 作用：用来设置资源被访问多少次后被缓存

> 默认缓存HTTP的GET和HEAD方法，不缓存POST方法。

### 案例演示

#### 需求分析

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210918122324.png)

#### 步骤实现

1.环境准备

应用服务器的环境准备

（1）在192.168.200.146服务器上的tomcat的webapps下面添加一个js目录，并在js目录中添加一个jquery.js文件

（2）启动tomcat

（3）访问测试

```
http://192.168.200.146:8080/js/jquery.js
```

Nginx的环境准备

（1）完成Nginx反向代理配置

```
http{
	upstream backend{
		server 192.168.200.146:8080;
	}
	server {
		listen       8080;
        server_name  localhost;
        location / {
        	proxy_pass http://backend/js/;
        }
	}
}
```

（2）完成Nginx缓存配置

4.添加缓存配置

```
http{
	proxy_cache_path /usr/local/proxy_cache levels=2:1 keys_zone=itcast:200m inactive=1d max_size=20g;
	upstream backend{
		server 192.168.200.146:8080;
	}
	server {
		listen       8080;
        server_name  localhost;
        location / {
        	proxy_cache itcast;
            proxy_cache_key itheima;
            proxy_cache_min_uses 5;
            proxy_cache_valid 200 5d;
            proxy_cache_valid 404 30s;
            proxy_cache_valid any 1m;
            add_header nginx-cache "$upstream_cache_status";
        	proxy_pass http://backend/js/;
        }
	}
}
```

## Nginx缓存的清除

### 方式一：删除对应的缓存目录

```bash
rm -rf /usr/local/proxy_cache/......
```

### 方式二：使用第三方扩展模块

（1）下载ngx_cache_purge模块对应的资源包，并上传到服务器上。

```
ngx_cache_purge-2.3.tar.gz
```

（2）对资源文件进行解压缩

```
tar -zxf ngx_cache_purge-2.3.tar.gz
```

（3）修改文件夹名称，方便后期配置

```
mv ngx_cache_purge-2.3 purge
```

（4）查询Nginx的配置参数

```
nginx -V
```

（5）进入Nginx的安装目录，使用./configure进行参数配置

```
./configure --add-module=/root/nginx/module/purge
```

（6）使用make进行编译

```
make
```

（7）将nginx安装目录的nginx二级制可执行文件备份

```
mv /usr/local/nginx/sbin/nginx /usr/local/nginx/sbin/nginxold
```

（8）将编译后的objs中的nginx拷贝到nginx的sbin目录下

```
cp objs/nginx /usr/local/nginx/sbin
```

（9）使用make进行升级

```
make upgrade
```

（10）在nginx配置文件中进行如下配置

```
server{
	location ~/purge(/.*) {
		proxy_cache_purge itcast itheima;
	}
}
```

## Nginx设置资源不缓存

### 概述

前面咱们已经完成了Nginx作为web缓存服务器的使用。但是我们得思考一个问题就是不是所有的数据都适合进行缓存。比如说对于一些经常发生变化的数据。如果进行缓存的话，就很容易出现用户访问到的数据不是服务器真实的数据。所以对于这些资源我们在缓存的过程中就需要进行过滤，不进行缓存。

Nginx也提供了这块的功能设置，需要使用到如下两个指令

### proxy_no_cache指令

+ 语法：`proxy_no_cache string ...;`
+ 位置：http、server、location
+ 作用：定义不将数据进行缓存的条件。

```conf
proxy_no_cache $cookie_nocache $arg_nocache $arg_comment;
```

### proxy_cache_bypass指令

+ 语法：`proxy_cache_bypass string ...;`
+ 位置：http、server、location
+ 作用：设置不从缓存中获取数据的条件。

```conf
proxy_cache_bypass $cookie_nocache $arg_nocache $arg_comment;
```

上述两个指令都有一个指定的条件，这个条件可以是多个，并且多个条件中至少有一个不为空且不等于"0",则条件满足成立。上面给的配置实例是从官方网站获取的，里面使用到了三个变量，分别是`\$cookie_nocache、\$arg_nocache、\$arg_comment`

+ **$cookie_nocache**: 指的是当前请求的cookie中键的名称为nocache对应的值
+ **$arg_nocache和$arg_comment**: 指的是当前请求的参数中属性名为nocache和comment对应的属性值

**最终案例演示**

```conf
server{
	listen	8080;
	server_name localhost;
	location / {
		if ($request_uri ~ /.*\.js$){
           set $nocache 1;
        }
		proxy_no_cache $nocache $cookie_nocache $arg_nocache $arg_comment;
    proxy_cache_bypass $nocache $cookie_nocache $arg_nocache $arg_comment;
	}
}
```

# Nginx实现服务器端集群搭建

## Nginx与Tomcat部署

### 需求

前面课程已经将Nginx的大部分内容进行了讲解，我们都知道了Nginx在高并发场景和处理静态资源是非常高性能的，但是在实际项目中除了静态资源还有就是后台业务代码模块，一般后台业务都会被部署在Tomcat，weblogic或者是websphere等web服务器上。那么如何使用Nginx接收用户的请求并把请求转发到后台web服务器？

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210918134623.png)

### 思路

1. 准备Tomcat环境，并在Tomcat上部署一个web项目
2. 准备Nginx环境，使用Nginx接收请求，并把请求分发到Tomat上

### 实现

1. 浏览器访问`http://192.168.200.146:8080/demo/index.html`

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210918134746.png)

2. 浏览器访问`http://192.168.200.146:8080/demo/getAddress`获取动态资源的链接地址

本次课程将采用Tomcat作为后台web服务器

（1）在Centos上准备一个Tomcat

```
1.Tomcat官网地址:https://tomcat.apache.org/
2.下载tomcat,本次课程使用的是apache-tomcat-8.5.59.tar.gz
3.将tomcat进行解压缩
mkdir web_tomcat
tar -zxf apache-tomcat-8.5.59.tar.gz -C /web_tomcat
```

（2）准备一个web项目，将其打包为war

```
1.将资料中的demo.war上传到tomcat8目录下的webapps包下
2.将tomcat进行启动，进入tomcat8的bin目录下
./startup.sh
```

（3）启动tomcat进行访问测试。

```
静态资源: http://192.168.200.146:8080/demo/index.html
动态资源: http://192.168.200.146:8080/demo/getAddress
```

### 环境准备(Nginx)

（1）使用Nginx的反向代理，将请求转给Tomcat进行处理。

```
upstream webservice {
	server 192.168.200.146:8080;
}
server{
    listen		80;
    server_name localhost;
    location /demo {
    	proxy_pass http://webservice;
    }
}
```

（2）启动访问测试

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210918134910.png)

学习到这，可能大家会有一个困惑，明明直接通过tomcat就能访问，为什么还需要多加一个nginx，这样不是反而是系统的复杂度变高了么?
那接下来我们从两个方便给大家分析下这个问题，

第一个使用Nginx实现动静分离

第二个使用Nginx搭建Tomcat的集群

## Nginx实现动静分离

### 什么是动静分离?

**动:**后台应用程序的业务处理

**静:**网站的静态资源(html,javaScript,css,images等文件)

**分离:**将两者进行分开部署访问，提供用户进行访问。举例说明就是以后所有和静态资源相关的内容都交给Nginx来部署访问，非静态内容则交个类似于Tomcat的服务器来部署访问。

### 为什么要动静分离?

前面我们介绍过Nginx在处理静态资源的时候，效率是非常高的，而且Nginx的并发访问量也是名列前茅，而Tomcat则相对比较弱一些，所以把静态资源交个Nginx后，可以减轻Tomcat服务器的访问压力并提高静态资源的访问速度。

动静分离以后，降低了动态资源和静态资源的耦合度。如动态资源宕机了也不影响静态资源的展示。

### 如何实现动静分离?

实现动静分离的方式很多，比如静态资源可以部署到CDN、Nginx等服务器上，动态资源可以部署到Tomcat,weblogic或者websphere上。本次课程只要使用Nginx+Tomcat来实现动静分离。

### 案例演示

#### 分析

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210918135043.png)

#### 实现

1. 将demo.war项目中的静态资源都删除掉，重新打包生成一个war包，在资料中有提供。

2. 将war包部署到tomcat中，把之前部署的内容删除掉

```
进入到tomcat的webapps目录下，将之前的内容删除掉
将新的war包复制到webapps下
将tomcat启动
```

3. 在Nginx所在服务器创建如下目录，并将对应的静态资源放入指定的位置

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210918135134.png)

其中index.html页面的内容如下:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="js/jquery.min.js"></script>
    <script>
        $(function(){
           $.get('http://192.168.200.133/demo/getAddress',function(data){
               $("#msg").html(data);
           });
        });
    </script>
</head>
<body>
    <img src="images/logo.png"/>
    <h1>Nginx如何将请求转发到后端服务器</h1>
    <h3 id="msg"></h3>
    <img src="images/mv.png"/>
</body>
</html>

```

4. 配置Nginx的静态资源与动态资源的访问

```conf
upstream webservice{
   server 192.168.200.146:8080;
}
server {
        listen       80;
        server_name  localhost;

        #动态资源
        location /demo {
                proxy_pass http://webservice;
        }
        #静态资源
        location ~/.*\.(png|jpg|gif|js){
                root html/web;
                gzip on;
        }

        location / {
            root   html/web;
            index  index.html index.htm;
        }
}
```

5. 启动测试，访问http://192.168.200.133/index.html

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210918135213.png)

假如某个时间点，由于某个原因导致Tomcat后的服务器宕机了，我们再次访问Nginx,会得到如下效果，用户还是能看到页面，只是缺失了访问次数的统计，这就是前后端耦合度降低的效果，并且整个请求只和后的服务器交互了一次，js和images都直接从Nginx返回，提供了效率，降低了后的服务器的压力。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210918135231.png)

## Nginx实现Tomcat集群搭建

在使用Nginx和Tomcat部署项目的时候，我们使用的是一台Nginx服务器和一台Tomcat服务器，效果图如下:

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210918165106.png)

那么问题来了，如果Tomcat的真的宕机了，整个系统就会不完整，所以如何解决上述问题，一台服务器容易宕机，那就多搭建几台Tomcat服务器，这样的话就提升了后的服务器的可用性。这也就是我们常说的集群，搭建Tomcat的集群需要用到了Nginx的反向代理和赋值均衡的知识，具体如何来实现?我们先来分析下原理

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210918165138.png)

环境准备：

(1)准备3台tomcat,使用端口进行区分[实际环境应该是三台服务器]，修改server.ml，将端口修改分别修改为8080,8180,8280

(2)启动tomcat并访问测试，

```
http://192.168.200.146:8080/demo/getAddress
```

```
http://192.168.200.146:8180/demo/getAddress
```

```
http://192.168.200.146:8280/demo/getAddress
```

(3)在Nginx对应的配置文件中添加如下内容:

```
upstream webservice{
        server 192.168.200.146:8080;
        server 192.168.200.146:8180;
        server 192.168.200.146:8280;
    }

```

好了，完成了上述环境的部署，我们已经解决了Tomcat的高可用性，一台服务器宕机，还有其他两条对外提供服务，同时也可以实现后台服务器的不间断更新。但是新问题出现了，上述环境中，如果是Nginx宕机了呢，那么整套系统都将服务对外提供服务了，这个如何解决？

## Nginx高可用解决方案

针对于上面提到的问题，我们来分析下要想解决上述问题，需要面临哪些问题?

```
需要两台以上的Nginx服务器对外提供服务，这样的话就可以解决其中一台宕机了，另外一台还能对外提供服务，但是如果是两台Nginx服务器的话，会有两个IP地址，用户该访问哪台服务器，用户怎么知道哪台是好的，哪台是宕机了的?
```

### Keepalived

使用Keepalived来解决，Keepalived 软件由 C 编写的，最初是专为 LVS 负载均衡软件设计的，Keepalived 软件主要是通过 VRRP 协议实现高可用功能。

### VRRP介绍

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210918165348.png)

VRRP（Virtual Route Redundancy Protocol）协议，翻译过来为虚拟路由冗余协议。VRRP协议将两台或多台路由器设备虚拟成一个设备，对外提供虚拟路由器IP,而在路由器组内部，如果实际拥有这个对外IP的路由器如果工作正常的话就是MASTER,MASTER实现针对虚拟路由器IP的各种网络功能。其他设备不拥有该虚拟IP，状态为BACKUP,处了接收MASTER的VRRP状态通告信息以外，不执行对外的网络功能。当主机失效时，BACKUP将接管原先MASTER的网络功能。

从上面的介绍信息获取到的内容就是VRRP是一种协议，那这个协议是用来干什么的？

1. 选择协议

   VRRP可以把一个虚拟路由器的责任动态分配到局域网上的 VRRP 路由器中的一台。其中的虚拟路由即Virtual路由是由VRRP路由群组创建的一个不真实存在的路由，这个虚拟路由也是有对应的IP地址。而且VRRP路由1和VRRP路由2之间会有竞争选择，通过选择会产生一个Master路由和一个Backup路由。

2. 路由容错协议

   Master路由和Backup路由之间会有一个心跳检测，Master会定时告知Backup自己的状态，如果在指定的时间内，Backup没有接收到这个通知内容，Backup就会替代Master成为新的Master。Master路由有一个特权就是虚拟路由和后端服务器都是通过Master进行数据传递交互的，而备份节点则会直接丢弃这些请求和数据，不做处理，只是去监听Master的状态

用了Keepalived后，解决方案如下:

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210918165503.png)

### 环境搭建

环境准备

| VIP             | IP              | 主机名      | 主/从  |
| --------------- | --------------- | ----------- | ------ |
|                 | 192.168.200.133 | keepalived1 | Master |
| 192.168.200.222 |                 |             |        |
|                 | 192.168.200.122 | keepalived2 | Backup |

keepalived的安装

```
步骤1:从官方网站下载keepalived,官网地址https://keepalived.org/
步骤2:将下载的资源上传到服务器
	keepalived-2.0.20.tar.gz
步骤3:创建keepalived目录，方便管理资源
	mkdir keepalived
步骤4:将压缩文件进行解压缩，解压缩到指定的目录
	tar -zxf keepalived-2.0.20.tar.gz -C keepalived/
步骤5:对keepalived进行配置，编译和安装
	cd keepalived/keepalived-2.0.20
	./configure --sysconf=/etc --prefix=/usr/local
	make && make install
```

安装完成后，有两个文件需要我们认识下，一个是 `/etc/keepalived/keepalived.conf`(keepalived的系统配置文件，我们主要操作的就是该文件)，一个是/usr/local/sbin目录下的`keepalived`,是系统配置脚本，用来启动和关闭keepalived

### Keepalived配置文件介绍

打开keepalived.conf配置文件

这里面会分三部，第一部分是global全局配置、第二部分是vrrp相关配置、第三部分是LVS相关配置。
本次课程主要是使用keepalived实现高可用部署，没有用到LVS，所以我们重点关注的是前两部分

```
global全局部分：
global_defs {
   #通知邮件，当keepalived发送切换时需要发email给具体的邮箱地址
   notification_email {
     tom@itcast.cn
     jerry@itcast.cn
   }
   #设置发件人的邮箱信息
   notification_email_from zhaomin@itcast.cn
   #指定smpt服务地址
   smtp_server 192.168.200.1
   #指定smpt服务连接超时时间
   smtp_connect_timeout 30
   #运行keepalived服务器的一个标识，可以用作发送邮件的主题信息
   router_id LVS_DEVEL
   
   #默认是不跳过检查。检查收到的VRRP通告中的所有地址可能会比较耗时，设置此命令的意思是，如果通告与接收的上一个通告来自相同的master路由器，则不执行检查(跳过检查)
   vrrp_skip_check_adv_addr
   #严格遵守VRRP协议。
   vrrp_strict
   #在一个接口发送的两个免费ARP之间的延迟。可以精确到毫秒级。默认是0
   vrrp_garp_interval 0
   #在一个网卡上每组na消息之间的延迟时间，默认为0
   vrrp_gna_interval 0
}
```

```
VRRP部分，该部分可以包含以下四个子模块
1. vrrp_script
2. vrrp_sync_group
3. garp_group
4. vrrp_instance
我们会用到第一个和第四个，
#设置keepalived实例的相关信息，VI_1为VRRP实例名称
vrrp_instance VI_1 {
    state MASTER  		#有两个值可选MASTER主 BACKUP备
    interface ens33		#vrrp实例绑定的接口，用于发送VRRP包[当前服务器使用的网卡名称]
    virtual_router_id 51#指定VRRP实例ID，范围是0-255
    priority 100		#指定优先级，优先级高的将成为MASTER
    advert_int 1		#指定发送VRRP通告的间隔，单位是秒
    authentication {	#vrrp之间通信的认证信息
        auth_type PASS	#指定认证方式。PASS简单密码认证(推荐)
        auth_pass 1111	#指定认证使用的密码，最多8位
    }
    virtual_ipaddress { #虚拟IP地址设置虚拟IP地址，供用户访问使用，可设置多个，一行一个
        192.168.200.222
    }
}
```

配置内容如下:

服务器1

```
global_defs {
   notification_email {
        tom@itcast.cn
        jerry@itcast.cn
   }
   notification_email_from zhaomin@itcast.cn
   smtp_server 192.168.200.1
   smtp_connect_timeout 30
   router_id keepalived1
   vrrp_skip_check_adv_addr
   vrrp_strict
   vrrp_garp_interval 0
   vrrp_gna_interval 0
}

vrrp_instance VI_1 {
    state MASTER
    interface ens33
    virtual_router_id 51
    priority 100
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        192.168.200.222
    }
}
```

服务器2

```
! Configuration File for keepalived

global_defs {
   notification_email {
        tom@itcast.cn
        jerry@itcast.cn
   }
   notification_email_from zhaomin@itcast.cn
   smtp_server 192.168.200.1
   smtp_connect_timeout 30
   router_id keepalived2
   vrrp_skip_check_adv_addr
   vrrp_strict
   vrrp_garp_interval 0
   vrrp_gna_interval 0
}

vrrp_instance VI_1 {
    state BACKUP
    interface ens33
    virtual_router_id 51
    priority 90
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        192.168.200.222
    }
}
```

### 访问测试

1. 启动keepalived之前，咱们先使用命令 `ip a`,查看192.168.200.133和192.168.200.122这两台服务器的IP情况。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210918165745.png)

2. 分别启动两台服务器的keepalived

```
cd /usr/local/sbin
./keepalived
```

再次通过 `ip a`查看ip

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210918165757.png)

3. 当把192.168.200.133服务器上的keepalived关闭后，再次查看ip

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210918165821.png)

通过上述的测试，我们会发现，虚拟IP(VIP)会在MASTER节点上，当MASTER节点上的keepalived出问题以后，因为BACKUP无法收到MASTER发出的VRRP状态通过信息，就会直接升为MASTER。VIP也会"漂移"到新的MASTER。

上面测试和Nginx有什么关系?

我们把192.168.200.133服务器的keepalived再次启动下，由于它的优先级高于服务器192.168.200.122的，所有它会再次成为MASTER，VIP也会"漂移"过去，然后我们再次通过浏览器访问:

```
http://192.168.200.222/
```

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210918165845.png)

如果把192.168.200.133服务器的keepalived关闭掉，再次访问相同的地址

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210918165859.png)

效果实现了以后， 我们会发现要想让vip进行切换，就必须要把服务器上的keepalived进行关闭，而什么时候关闭keepalived呢?应该是在keepalived所在服务器的nginx出现问题后，把keepalived关闭掉，就可以让VIP执行另外一台服务器，但是现在这所有的操作都是通过手动来完成的，我们如何能让系统自动判断当前服务器的nginx是否正确启动，如果没有，要能让VIP自动进行"漂移"，这个问题该如何解决?



### keepalived之vrrp_script

keepalived只能做到对网络故障和keepalived本身的监控，即当出现网络故障或者keepalived本身出现问题时，进行切换。但是这些还不够，我们还需要监控keepalived所在服务器上的其他业务，比如Nginx,如果Nginx出现异常了，仅仅keepalived保持正常，是无法完成系统的正常工作的，因此需要根据业务进程的运行状态决定是否需要进行主备切换，这个时候，我们可以通过编写脚本对业务进程进行检测监控。

实现步骤:

1. 在keepalived配置文件中添加对应的配置像

```
vrrp_script 脚本名称
{
    script "脚本位置"
    interval 3 #执行时间间隔
    weight -20 #动态调整vrrp_instance的优先级
}
```

2. 编写脚本

ck_nginx.sh

```
#!/bin/bash
num=`ps -C nginx --no-header | wc -l`
if [ $num -eq 0 ];then
 /usr/local/nginx/sbin/nginx
 sleep 2
 if [ `ps -C nginx --no-header | wc -l` -eq 0 ]; then
  killall keepalived
 fi
fi
```

Linux ps命令用于显示当前进程 (process) 的状态。

-C(command) :指定命令的所有进程

--no-header 排除标题

3. 为脚本文件设置权限

```
chmod 755 ck_nginx.sh
```

4. 将脚本添加到

```
vrrp_script ck_nginx {
   script "/etc/keepalived/ck_nginx.sh" #执行脚本的位置
   interval 2		#执行脚本的周期，秒为单位
   weight -20		#权重的计算方式
}
vrrp_instance VI_1 {
    state MASTER
    interface ens33
    virtual_router_id 10
    priority 100
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        192.168.200.111
    }
    track_script {
      ck_nginx
    }
}
```

5. 如果效果没有出来，可以使用 `tail -f /var/log/messages`查看日志信息，找对应的错误信息。
6. 测试



问题思考:

通常如果master服务死掉后backup会变成master，但是当master服务又好了的时候 master此时会抢占VIP，这样就会发生两次切换对业务繁忙的网站来说是不好的。所以我们要在配置文件加入 nopreempt 非抢占，但是这个参数只能用于state 为backup，故我们在用HA的时候最好master 和backup的state都设置成backup 让其通过priority来竞争。

# Nginx制作下载站点

首先我们先要清楚什么是下载站点?

我们先来看一个网站`http://nginx.org/download/`这个我们刚开始学习Nginx的时候给大家看过这样的网站，该网站主要就是用来提供用户来下载相关资源的网站，就叫做下载网站。

如何制作一个下载站点:

nginx使用的是模块ngx_http_autoindex_module来实现的，该模块处理以斜杠("/")结尾的请求，并生成目录列表。

nginx编译的时候会自动加载该模块，但是该模块默认是关闭的，我们需要使用下来指令来完成对应的配置

（1）autoindex:启用或禁用目录列表输出

| 语法   | autoindex on\|off;     |
| ------ | ---------------------- |
| 默认值 | autoindex off;         |
| 位置   | http、server、location |

（2）autoindex_exact_size:对应HTLM格式，指定是否在目录列表展示文件的详细大小

默认为on，显示出文件的确切大小，单位是bytes。
改为off后，显示出文件的大概大小，单位是kB或者MB或者GB

| 语法   | autoindex_exact_size  on\|off; |
| ------ | ------------------------------ |
| 默认值 | autoindex_exact_size  on;      |
| 位置   | http、server、location         |

（3）autoindex_format：设置目录列表的格式

| 语法   | autoindex_format html\|xml\|json\|jsonp; |
| ------ | ---------------------------------------- |
| 默认值 | autoindex_format html;                   |
| 位置   | http、server、location                   |

注意:该指令在1.7.9及以后版本中出现

（4）autoindex_localtime:对应HTML格式，是否在目录列表上显示时间。

默认为off，显示的文件时间为GMT时间。
改为on后，显示的文件时间为文件的服务器时间

| 语法   | autoindex_localtime on \| off; |
| ------ | ------------------------------ |
| 默认值 | autoindex_localtime off;       |
| 位置   | http、server、location         |

配置方式如下:

```
location /download{
    root /usr/local;
    autoindex on;
    autoindex_exact_size on;
    autoindex_format html;
    autoindex_localtime on;
}

```

XML/JSON格式[一般不用这两种方式]

# Nginx的用户认证模块

对应系统资源的访问，我们往往需要限制谁能访问，谁不能访问。这块就是我们通常所说的认证部分，认证需要做的就是根据用户输入的用户名和密码来判定用户是否为合法用户，如果是则放行访问，如果不是则拒绝访问。

Nginx对应用户认证这块是通过ngx_http_auth_basic_module模块来实现的，它允许通过使用"HTTP基本身份验证"协议验证用户名和密码来限制对资源的访问。默认情况下nginx是已经安装了该模块，如果不需要则使用--without-http_auth_basic_module。

该模块的指令比较简单，

（1）auth_basic:使用“ HTTP基本认证”协议启用用户名和密码的验证

| 语法   | auth_basic string\|off;           |
| ------ | --------------------------------- |
| 默认值 | auth_basic off;                   |
| 位置   | http,server,location,limit_except |

开启后，服务端会返回401，指定的字符串会返回到客户端，给用户以提示信息，但是不同的浏览器对内容的展示不一致。

（2）auth_basic_user_file:指定用户名和密码所在文件

| 语法   | auth_basic_user_file file;        |
| ------ | --------------------------------- |
| 默认值 | —                                 |
| 位置   | http,server,location,limit_except |

指定文件路径，该文件中的用户名和密码的设置，密码需要进行加密。可以采用工具自动生成

实现步骤:

1.nginx.conf添加如下内容

```
location /download{
    root /usr/local;
    autoindex on;
    autoindex_exact_size on;
    autoindex_format html;
    autoindex_localtime on;
    auth_basic 'please input your auth';
    auth_basic_user_file htpasswd;
}
```

2.我们需要使用`htpasswd`工具生成

```
yum install -y httpd-tools
```

```
htpasswd -c /usr/local/nginx/conf/htpasswd username //创建一个新文件记录用户名和密码
htpasswd -b /usr/local/nginx/conf/htpasswd username password //在指定文件新增一个用户名和密码
htpasswd -D /usr/local/nginx/conf/htpasswd username //从指定文件删除一个用户信息
htpasswd -v /usr/local/nginx/conf/htpasswd username //验证用户名和密码是否正确
```

上述方式虽然能实现用户名和密码的验证，但是大家也看到了，所有的用户名和密码信息都记录在文件里面，如果用户量过大的话，这种方式就显得有点麻烦了，这时候我们就得通过后台业务代码来进行用户权限的校验了。