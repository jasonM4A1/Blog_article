# 1. Docker概述

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210920120745.png)

## 1.1 基本介绍

Docker 是一个开源的应用容器引擎，基于 Go 语言 并遵从 Apache2.0 协议开源。

Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。

容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）,更重要的是容器性能开销极低。

Docker 从 17.03 版本之后分为 CE（Community Edition: 社区版） 和 EE（Enterprise Edition: 企业版），我们用社区版就可以了。官网：https://docs.docker.com/

## 1.2 应用场景

+ Web应用的自动化打包和发布
+ 自动化测试和持续集成、发布
+ 在服务型环境中部署和整体数据库或其他的后台应用
+ 从头编译或者扩展现有的`OpenShift`或`Cloud Foundry`平台来搭建自己的Paas环境

## 1.3 Docker的优势

Docker 是一个用于开发，交付和运行应用程序的开放平台。Docker 使您能够将应用程序与基础架构分开，从而可以快速交付软件。借助 Docker，您可以与管理应用程序相同的方式来管理基础架构。通过利用 Docker 的方法来快速交付，测试和部署代码，您可以大大减少编写代码和在生产环境中运行代码之间的延迟。

1. **快速，一致地交付您的应用程序**

   Docker 允许开发人员使用您提供的应用程序或服务的本地容器在标准化环境中工作，从而简化了开发的生命周期。

   容器非常适合持续集成和持续交付（CI / CD）工作流程，请考虑以下示例方案：

   您的开发人员在本地编写代码，并使用 Docker 容器与同事共享他们的工作。
   他们使用 Docker 将其应用程序推送到测试环境中，并执行自动或手动测试。
   当开发人员发现错误时，他们可以在开发环境中对其进行修复，然后将其重新部署到测试环境中，以进行测试和验证。
   测试完成后，将修补程序推送给生产环境，就像将更新的镜像推送到生产环境一样简单。

2. **响应式部署和扩展**

   Docker 是基于容器的平台，允许高度可移植的工作负载。Docker 容器可以在开发人员的本机上，数据中心的物理或虚拟机上，云服务上或混合环境中运行。

   Docker 的可移植性和轻量级的特性，还可以使您轻松地完成动态管理的工作负担，并根据业务需求指示，实时扩展或拆除应用程序和服务。

3. **在同一硬件上运行更多工作负载**

   Docker 轻巧快速。它为基于虚拟机管理程序的虚拟机提供了可行、经济、高效的替代方案，因此您可以利用更多的计算能力来实现业务目标。Docker 非常适合于高密度环境以及中小型部署，而您可以用更少的资源做更多的事情。

# 2. 虚拟化技术和容器化技术

**虚拟化技术：**资源占用多、冗余步骤多、启动很慢

**容器化技术**：容器化技术不是模拟的一个完整的操作系统

比较Docker和虚拟机的不同：

1. 传统虚拟机，虚拟出硬件，运行一个完整的操作系统，然后在这个系统上安装和运行软件。
2. Docker容器内的应用直接运行在宿主机的内容，容器是没有自己的内核的，也没有虚拟硬件。
3. 每个容器都是相互隔离的，每个容器都有属于自己的文件系统，互不影响。

# 3. Docker的基本组成

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210919114446.png)

+ **镜像（image）**

  docker镜像就好比是一个模版，可以通过这个模版来创建容器服务

+ **容器（container）**

  Docker利用容器技术，独立运行一个或者一组应用，通过镜像来创建（目前可以把一个容器简单的理解为一个简易的Linux系统）

+ **仓库（repository）**

  仓库就是存放镜像的地方，例如`Docker Hub`

# 4. 安装运行Docker

1. 确保系统为centos7，内核办法为3以上

   ```bash
   [root@VM-4-7-centos docker]# uname -r
   3.10.0-1160.11.1.el7.x86_64
   
   [root@VM-4-7-centos docker]# cat /etc/os-release
   NAME="CentOS Linux"
   VERSION="7 (Core)"
   ```

2. 卸载Docker旧版本（之前没有安装则可以跳过）

   ```bash
   yum remove docker \
                     docker-client \
                     docker-client-latest \
                     docker-common \
                     docker-latest \
                     docker-latest-logrotate \
                     docker-logrotate \
                     docker-engine
   ```

3. 下载docker所需的工具包

   ```bash
   sudo yum install -y yum-utils
   ```

4. 设置下载docker的镜像仓库为阿里云

   ```bash
   yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
   ```

5. 更新yum软件包索引

   ```bash
   yum makecache fast
   ```

6. 安装最新版的docker（docker-ce 是社区版，docker-ee 企业版）

   ```bash
   sudo yum install docker-ce docker-ce-cli containerd.io
   ```

7. 启动Docker

   ```bash
   systemctl start docker
   ```

8. 查看Docker版本号

   ```bash
   docker version
   ```

   出现版本信息则表明安装成功！

9. 设置Docker开机自启

   ```bash
   systemctl enable docker
   ```

10. 下载`hello-world`镜像进行测试

    ```bash
    docker run hello-world
    ```

11. 查看`hell0-world`镜像是否下载成功 

    ```bash
    docker images
    ```

12. 卸载Docker

    ```bash
    # 1. 卸载依赖
    yum remove docker-ce docker-ce-cli containerd.io
    
    # 2. 删除资源  . /var/lib/docker是docker的默认工作路径
    rm -rf /var/lib/docker
    ```

# 5. Docker镜像加速

1. 进入网址`https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors`获取阿里云镜像加速链接

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210919121444.png)

2. 依次执行下面四条命令

   ```bash
   #创建对应的目录（如果有就不需要创建）
   sudo mkdir -p /etc/docker
   
   #创建daemon.json文件，并写入镜像加速器地址（[]中替换为上面自己获取的加速器地址）
   sudo tee /etc/docker/daemon.json <<-'EOF'
   {
     "registry-mirrors": ["https://ppc7nwnq.mirror.aliyuncs.com"]
   }
   EOF
   
   #重启服务
   sudo systemctl daemon-reload
   sudo systemctl restart docker
   ```

# 6. Docker容器运行流程

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210919132902.png)

**Docker为什么比VM Ware快？**

1. Docker比虚拟机更少的抽象层
2. docker利用宿主机的内核，VM需要的是Guest OS

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210919133007.png)

Docker新建一个容器的时候，不需要像虚拟机一样重新加载一个操作系统内核，直接利用宿主机的操作系统，而虚拟机是需要加载Guest OS。Docker和VM的对比如下：

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210919133034.png)

# 7. Docker常用命令

## 7.1 基础命令

```bash
#查看docker版本信息
docker version

#查看docker的系统信息
docker info

#帮助命令
docker command --help
```

## 7.2 镜像命令

### 7.2.1 查看本地主机的所有镜像

**语法：`docker images`**

```bash
[root@iZwz99sm8v95sckz8bd2c4Z ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
hello-world   latest    bf756fb1ae65   11 months ago   13.3kB

#解释:
1.REPOSITORY  镜像的仓库源

2.TAG  镜像的标签

3.IMAGE ID 镜像的id

4.CREATED 镜像的创建时间

5.SIZE 镜像的大小

# 可选参数
-a/--all 列出所有镜像

-q/--quiet 只显示镜像的id
```

### 7.2.2 搜索镜像

**语法：`docker search`**

```bash
[root@iZwz99sm8v95sckz8bd2c4Z ~]# docker search mysql
NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   10308     [OK]
mariadb                           MariaDB is a community-developed fork of MyS…   3819      [OK]
mysql/mysql-server                Optimized MySQL Server Docker images. Create…   754                  [OK]
percona                           Percona Server is a fork of the MySQL relati…   517       [OK]
centos/mysql-57-centos7           MySQL 5.7 SQL database server                   86
mysql/mysql-cluster               Experimental MySQL Cluster Docker images. Cr…   79
centurylink/mysql                 Image containing mysql. Optimized to be link…   60                   [OK]


#可选参数
Search the Docker Hub for images

Options:
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print search using a Go template
      --limit int       Max number of search results (default 25)
      --no-trunc        Don't truncate output
      
      
#搜索收藏数大于3000的镜像
[root@iZwz99sm8v95sckz8bd2c4Z ~]# docker search mysql --filter=STARS=3000
NAME      DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql     MySQL is a widely used, open-source relation…   10308     [OK]
mariadb   MariaDB is a community-developed fordockerk of MyS…   3819      [OK]
```

### 7.2.3 下载镜像

**语法：`docker pull 镜像名[:tag]`**

```bash
[root@iZwz99sm8v95sckz8bd2c4Z ~]# docker pull mysql
Using default tag: latest            #如果不写tag默认就是latest
latest: Pulling from library/mysql
6ec7b7d162b2: Pull complete          #分层下载,docker image的核心-联合文件系统
fedd960d3481: Pull complete
7ab947313861: Pull complete
64f92f19e638: Pull complete
3e80b17bff96: Pull complete
014e976799f9: Pull complete
59ae84fee1b3: Pull complete
ffe10de703ea: Pull complete
657af6d90c83: Pull complete
98bfb480322c: Pull complete
6aa3859c4789: Pull complete
1ed875d851ef: Pull complete
Digest: sha256:78800e6d3f1b230e35275145e657b82c3fb02a27b2d8e76aac2f5e90c1c30873 #签名
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest  #下载来源的真实地址  #docker pull mysql等价于docker pull docker.io/library/mysql:latest

#指定版本下载
[root@iZwz99sm8v95sckz8bd2c4Z ~]# docker pull mysql:5.7
5.7: Pulling from library/mysql
6ec7b7d162b2: Already exists
fedd960d3481: Already exists
7ab947313861: Already exists
64f92f19e638: Already exists
3e80b17bff96: Already exists
014e976799f9: Already exists
59ae84fee1b3: Already exists
7d1da2a18e2e: Pull complete
301a28b700b9: Pull complete
529dc8dbeaf3: Pull complete
bc9d021dc13f: Pull complete
Digest: sha256:c3a567d3e3ad8b05dfce401ed08f0f6bf3f3b64cc17694979d5f2e5d78e10173
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7
```

### 7.2.4 删除指定镜像

**语法：`docker rmi 镜像id`**

```bash
#1.删除指定的镜像id
[root@iZwz99sm8v95sckz8bd2c4Z ~]# docker rmi -f  镜像id

#2.删除多个镜像id
[root@iZwz99sm8v95sckz8bd2c4Z ~]# docker rmi -f  镜像id 镜像id 镜像id

#3.删除全部的镜像id
[root@iZwz99sm8v95sckz8bd2c4Z ~]# docker rmi -f  $(docker images -aq)
```

### 7.2.5 打包镜像

**语法：`docker save 镜像名 -o 名称.tar`**

```bash
#打包mydocker镜像
[root@VM-4-7-centos temp]# docker save mydocker -o mydocker.tar

[root@VM-4-7-centos temp]# ll
总用量 232992
-rw-r--r-- 1 root root         0 9月  20 11:29 HelloWorld.java
-rw------- 1 root root 238581760 9月  20 11:36 mydocker.tar	#打包的镜像tar包
```

### 7.2.6 载入镜像

**语法：`docker load -i 名称.tar`**

```bash
[root@VM-4-7-centos temp]# docker load -i mydocker.tar
Loaded image: mydocker:latest
```

## 7.3 容器命令

### 7.3.1 拉取下载一个容器

**语法：`docker pull 容器名`**

```bash
#拉取下载centos容器
docker pull centos
```

### 7.3.2 运行容器

**语法：`docker run [可选参数] image`**

```bash
#后台运行centos容器
docker run -d centos

#交互方式运行centos容器
docker run -it centos /bin/bash

#停止退出交互容器
[root@bd1b8900c547 /]# exit

#退出但不停止交互容器
Ctrl+P+Q

#参数说明
--name="名字"           指定容器名字
-d                     后台方式运行
-it                    使用交互方式运行,进入容器查看内容
-p                     指定容器的端口
(
-p ip:主机端口:容器端口  配置主机端口映射到容器端口
-p 主机端口:容器端口
-p 容器端口
)
-P                     随机指定端口(大写的P)
```

### 7.3.3 查询容器

**语法：`docker ps`**

```bash
#docker ps 
     # 列出当前正在运行的容器
-a   # 列出所有容器的运行记录
-n=? # 显示最近创建的n个容器
-q   # 只显示容器的编号


[root@iZwz99sm8v95sckz8bd2c4Z ~]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[root@iZwz99sm8v95sckz8bd2c4Z ~]# docker ps -a
CONTAINER ID   IMAGE          COMMAND       CREATED         STATUS                     PORTS     NAMES
bca129320bb5   centos         "/bin/bash"   4 minutes ago   Exited (0) 3 minutes ago             optimistic_shtern
bd1b8900c547   centos         "/bin/bash"   6 minutes ago   Exited (0) 5 minutes ago             cool_tesla
cf6adbf1b506   bf756fb1ae65   "/hello"      5 hours ago     Exited (0) 5 hours ago               optimistic_darwin
```

### 7.3.4 启动和停止容器

**语法：`docker command 容器id`**

```bash
docker start 容器id          #启动容器
docker restart 容器id        #重启容器
docker stop 容器id           #停止当前运行的容器
docker kill 容器id           #强制停止当前容器
```

### 7.3.5 删除容器

**语法：`docker rm 容器id`**

```bash
docker rm 容器id                 #删除指定的容器,不能删除正在运行的容器,强制删除使用 rm -f
docker rm -f $(docker ps -aq)   #删除所有的容器
docker ps -a -q|xargs docker rm #删除所有的容器
```

### 7.3.6  查看容器日志

**语法： `docker logs [参数] 容器id`**

```bash
#持续带时间打印指定10条容器日志
docker logs -tf --tail 10 6a9fe6a924ea

#参数说明
-t 日志加时间
-f 保留打印窗口，持续打印
-tail number 显示最后的number行
```

### 7.3.7 查看容器中进程信息

**语法：`docker top 容器id`**

```bash
[root@VM-4-7-centos bin]# docker top 6a9fe6a924ea
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                12357               12337               0                   13:49               ?                   00:00:00            /bin/bash
```

### 7.3.8 查看容器的元数据

**语法：``docker inspect 容器id``**

```bash
[root@VM-4-7-centos bin]# docker inspect 6a9fe6a924ea
[
    {
        "Id": "6a9fe6a924eafffd7b71ff739a5cc24203d936d2a6459ddb8ce4166a003e3a5e",
        "Created": "2021-09-19T05:49:00.433068635Z",
        "Path": "/bin/bash",
        "Args": [],
        "State": {
        ......
```

### 7.3.9 进入当前正在运行的容器

**语法：`docker exec -it 容器id /bin/bash`或`docker attact 容器id`**

```bash
#方式一：docker exec -it 容器id /bin/bash
docker exec -it 6a9fe6a924ea /bin/bash

#方式二：docker attact 容器id
docker attach 6a9fe6a924ea

#两种方式区别
docker exec：进入容器后会开启一个新的终端，使用`exit`退出后，容器依旧会继续运行
docker attach: 进入容器正在执行的终端，使用`exit`退出后，容器也会被停止
```

### 7.3.10 容器与主机文件拷贝

**从容器拷贝到主机：`docker cp 容器id:容器文件路径 主机文件路径`**

**从主机拷贝到容器：`docker cp 主机文件路径 容器id:容器文件路径`**

> 文件的拷贝只要容器存在就可以，无论容器是否正在运行

```bash
#从容器中拷贝文件到主机
docker cp 6a9fe6a924ea:/root/Person.java /root/

#从主机拷贝文件到容器
docker cp 
```

### 7.3.11 制作提交自己的镜像

1. 启动镜像为容器

2. 修改容器，加入自己的东西

3. 执行下面命令将修改后的容器变为自己的镜像

   **语法：`docker commit -a="作者名" -m="备注" 容器id 新镜像名:版本号`**

   ```bash
   #-a为作者 -m为备注 
   docker commit -a="BernardoLi" -m="add webapps app" 292b149a9bcd mytomcat:1.0
   ```

## 7.4 Docker命令案例演示

### 7.4.1 部署nginx

```bash
#拉取最新版的nginx镜像
docker pull nginx

#运行nginx，用本机3380对应docker80端口，起名为nginx01
docker run -d --name nginx01 -p 3380:80 nginx

#访问nginx
curl localhost:3380
```

### 7.4.2 部署tomcat

```bash
#运行tomcat，用本机3366对应docker8080端口，起名为tomcat02
docker run -d --name tomcat02 -p 3366:8080 tomcat:9.0
```

# 8. Docker可视化面板Portainer

## 8.1 介绍

Portainer是一个可视化的容器镜像的图形管理工具，利用Portainer可以轻松构建，管理和维护Docker环境。 而且完全免费，基于容器化的安装方式，方便高效部署。

官网地址：https://www.portainer.io/

## 8.2 安装

```bash
#直接拉取运行protainer
docker run -d -p 9000:9000 --restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true  portainer/portainer
```

## 8.3 使用

浏览器输入：你的外网ip:9000 （前提是你的阿里云安全组的8088端口已经开放），如下是登录页面

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210919182513.png)

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210919182529.png)

# 9. Docker镜像详解

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210919183711.png)

## 9.1 UnionFS（联合文件系统）

联合文件系统（UnionFS）是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下。联合文件系统是 Docker 镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。

特性：一次同时加载多个文件系统，但从外面看起来只能看到一个文件系统。联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录。

## 9.2 镜像加载原理

Docker的镜像实际由一层一层的文件系统组成：

**bootfs（boot file system）**主要包含bootloader和kernel。bootloader主要是引导加载kernel，完成后整个内核就都在内存中了。此时内存的使用权已由bootfs转交给内核，系统卸载bootfs。可以被不同的Linux发行版公用。

**rootfs（root file system）**包含典型Linux系统中的/dev，/proc，/bin，/etc等标准目录和文件。rootfs就是各种不同操作系统发行版（Ubuntu，Centos等）。因为底层直接用Host的kernel，rootfs只包含最基本的命令，工具和程序就可以了。

所有的Docker镜像都起始于一个基础镜像层，当进行修改或增加新的内容时，就会在当前镜像层之上，创建新的容器层。容器在启动时会在镜像最外层上建立一层可读写的容器层（R/W），而镜像层是只读的（R/O）。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210919183933.png)

# 10. Docker容器数据卷

## 10.1 概述

**简单来说：用来实现容器于宿主机之间数据共享**

数据卷就是数据(一个文件或者文件夹)。数据卷是特殊的目录，可以绕过联合文件系统，为一个或多个容器提供访问。

数据卷设计的目的是数据的永久化，是完全独立于容器的生命周期，不会在容器删除时删除其挂载的数据卷，也不会存在类似垃圾收集机制，对容器引用的数据卷进行处理。

Docker容器产生的数据，如果不通过docker commit生成新的镜像，使得数据做为镜像的一部分保存下来，那么当容器删除后，数据自然也就没有了。为了能保存数据在docker中我们使用卷。

## 10.2 特点

1. 数据卷可在容器之间共享或重用数据
2. 卷中的更改可以直接生效
3. 数据卷中的更改不会包含在镜像的更新中
4. 数据卷即使容器被删除也是存在的

## 10.3 添加数据卷

### 10.3.1 全路径挂载

**语法：`docker run [参数] -v /宿主机绝对路径目录:/容器内目录 镜像名`**

默认权限是可读可写，即容器对于挂载的数据卷有读写的权限，这样就可能会产生数据安全问题。

我们可以只给容器只读的权限：`docker docker run [参数] -v /宿主机绝对路径目录:/容器内目录:ro 镜像名 `。

这样容器就只能读取主机的数据，而不能修改主机的数据。

如果指定挂载的容器目录不存在，会自动创建。

```bash
#默认是可读可写
docker run -id -v /root/test:/usr/local/tomcat/webapps tomcat

#ro只读
docker run -id -v /root/test:/usr/local/tomcat/webapps:ro tomcat
  
#rw可读可写
docker run -id -v /root/test:/usr/local/tomcat/webapps:rw tomcat
```

### 10.3.2 匿名挂载和具名挂载

所谓匿名挂载（匿名卷），即在进行数据卷挂载的时候不指定宿主机的数据卷目录，`-v`命令之后直接跟上`容器内数据卷所在的路径`。

而具名挂载（命名卷）即在进行数据卷挂载的时候既指定`宿主机数据卷所在路径`，又指定`容器数据卷所在路径`

> 具名挂载时，如果挂载的卷不存在，会自动创建

先通过下面这种命令的方式感受一下两者的区别：

```bash
#匿名挂载（匿名卷）
docker run -d -p 6379:6379 --name mycentos -v /src/volume01

#具名挂载（命名卷） -v 卷名：容器数据卷所在路径
docker run -d -p 6379:6379 --name mycentos -v volumename:/src/volume01
```

除此种方式之外，我们也可以在在dockerfile构建docker镜像的时候使用`VOLUME`保留字来对数据卷进行挂载，此种挂载方式是匿名挂载的，我们可以指定一个或多个数据卷，这样只要启动了该自定义容器镜像，则会自动进行数据挂载，不会出现`忘记挂载导致数据不安全`的情况。

```bash
VOLUME ["容器内数据卷路径1","容器内数据卷路径2"……]
```

由于匿名挂载的时候只是指定了`容器内数据卷的路径`，至于该`容器内数据卷的路径`到底和`宿主机`中的哪个文件进行数据挂载，可以使用下面命令进行查看：

```bash
#查看当前正在运行的镜像容器id
[root@privateCloud / ]# docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
38d2810685e1        fc67f9b77899        "/bin/sh -c /bin/bash"   2 hours ago         Up 2 hours          80/tcp              focused_williams

#使用 inspect 查看镜像信息
[root@privateCloud / ]# docker inspect 38d2810685e1(这是容器id)
#在弹出来的信息中找到下面的数据：
"Mounts": [
            {
                "Type": "volume",
                "Name": "040a163ac1eb50ebc53b9014f2438baf3583491bfc38c0ae47c9d08ec4b009f8",
                "Source": "/var/lib/docker/volumes/040a163ac1eb50ebc53b9014f2438baf3583491bfc38c0ae47c9d08ec4b009f8/_data",
                "Destination": "volume01",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            },
            {
                "Type": "volume",
                "Name": "b7e238d439bb63d681d0c962bf44632fc76f2e82e249964023842198bfb3c16c",
                "Source": "/var/lib/docker/volumes/b7e238d439bb63d681d0c962bf44632fc76f2e82e249964023842198bfb3c16c/_data",
                "Destination": "volume02",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ],
        ......
```

从上面的 Mounts 中可以看到 Destination 和 Source 分别就是 容器内的数据卷 和 宿主机内的容器卷

**匿名卷和命名卷的区别？**

命名卷在用过一次之后以后挂载容器的时候还是可以继续使用，所以一般在需要保存数据的时候使用`命名卷`的方式

匿名卷则是随着容器的建立而建立，随着容器的关闭而消亡。匿名卷一般用来存储无关痛痒的数据。

## 10.4 查看数据卷

### 10.4.1 查看数据卷列表

**语法：`docker volume ls`**

```bash
[root@VM-4-7-centos temp]# docker volume ls
DRIVER    VOLUME NAME
local     4e5460a6a07395bf7c405aa5bbfbab8a11569380505eab3bdfc61f11f5f8b7f4
local     08a0de95cb8e7f5f0e6c549ac1038049a283fd39d6084f0b5fce2a4b0f1085d0
local     64f7a5e1ca5aa70671db2bad270e436878d7905c35edbf11ab55e357d334808c
local     eb12d02c5b348a1b8ac64933e0801ca82f4313ed31bdb98d3591631c64e6001a
local     fb389ae87ffd75cdf9a2823d75ee1e6fddf66cbbe695fbd6113ec2e1b9b11a2f
```

### 10.4.2 创建指定数据卷

**语法：`docker volume create 数据卷名`**

```bash
[root@VM-4-7-centos temp]# docker volume create good
good
```

### 10.4.3 查看指定数据卷信息

**语法：`docker volume inspect 数据卷名`**

```bash
[root@VM-4-7-centos temp]# docker volume inspect 4e5460a6a07395bf7c405aa5bbfbab8a11569380505eab3bdfc61f11f5f8b7f4
[
    {
        "CreatedAt": "2021-09-19T17:27:03+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/4e5460a6a07395bf7c405aa5bbfbab8a11569380505eab3bdfc61f11f5f8b7f4/_data",
        "Name": "4e5460a6a07395bf7c405aa5bbfbab8a11569380505eab3bdfc61f11f5f8b7f4",
        "Options": null,
        "Scope": "local"
    }
]
```

### 10.4.4 删除所有未被使用的数据卷

**语法：`docker volume prune`**

```bash
[root@VM-4-7-centos temp]# docker volume prune
WARNING! This will remove all local volumes not used by at least one container.
Are you sure you want to continue? [y/N] y
Deleted Volumes:
64f7a5e1ca5aa70671db2bad270e436878d7905c35edbf11ab55e357d334808c
eb12d02c5b348a1b8ac64933e0801ca82f4313ed31bdb98d3591631c64e6001a
fb389ae87ffd75cdf9a2823d75ee1e6fddf66cbbe695fbd6113ec2e1b9b11a2f
temp
08a0de95cb8e7f5f0e6c549ac1038049a283fd39d6084f0b5fce2a4b0f1085d0
4e5460a6a07395bf7c405aa5bbfbab8a11569380505eab3bdfc61f11f5f8b7f4

Total reclaimed space: 420.2kB
```

# 11. Dockerfile

## 11.2 什么是Dockerfile？

Dockerfile 是一个用来构建镜像的文本文件，文本内容包含了一条条构建镜像所需的指令和说明。

## 11.3 快速入门

这里仅讲解如何运行 Dockerfile 文件来定制一个镜像，具体 Dockerfile 文件内指令详解，将在下一节中介绍，这里你只要知道构建的流程即可。

1. 创建并编写Dockerfile文件

   ```
   #来自centos
   FROM	centos	
   
   #进入/bin/bash目录
   CMD	/bin/bash
   
   #输出hello Dockerfile
   CMD	echo Hello Dockerfile	
   ```

2. 构建镜像

   ```bash
   docker build -f ./Dockerfile -t mydocker .
   ```

3. 运行镜像

   ```bash
   [root@VM-4-7-centos test]# docker run mydocker
   Hello Dockerfile
   ```

## 11.4 Dockerfile命令

| 保留字     | 作用                                                         |
| ---------- | ------------------------------------------------------------ |
| FROM       | 指定基础镜像                                                 |
| MAINTAINER | 镜像维护者姓名与邮箱地址（官方已不推荐使用）                 |
| RUN        | 构建镜像时需要运行的指令                                     |
| EXPOSE     | 暴露的端口号                                                 |
| WORKDIR    | 指定在创建容器后，终端默认登录进来的工作目录，一个落脚点     |
| ENV        | 用来在构建镜像过程中设置环境变量                             |
| ADD        | 将宿主机目录下的文件拷贝进镜像（且会自动处理URL和解压tar包） |
| COPY       | 类似于ADD，拷贝文件和目录到镜像中<br />将从构建上下目录中<原路径>的文件/目录复制到新的一层的镜像内的<目标路径>位置 |
| VOLUME     | 挂载匿名容器数据卷，所有启动此镜像的容器都会被自动挂载此数据卷 |
| CMD        | 指定一个容器启动时要运行的命令<br />Dockerfile中可以有多个CMD指令，但只有最后一个生效，CMD会被docker run之后的参数替换 |
| ENTRYPOINT | 指定一个容器启动时要运行的命令<br />效果与CMD差不多，区别是多个ENTRYPOINT会同时生效 |



## 11.5 构建实例演示（运行自己项目的springboot Jar包）

1. 将springboot项目的Jar包上传的与`dockerfile`文件同级目录中（目录如果有其他无关文件，使用`.dockerignore`文件来设置忽略）

2. 编写`dockerfile`文件

   ```dockerfile
   FROM openjdk:8-jdk 
   WORKDIR /app
   ADD 11-springboot-ssm-1.0.0.jar app.jar
   EXPOSE 8080
   ENTRYPOINT ["java","-jar"]
   CMD ["app.jar"]
   ```

3. 构建镜像

   ```bash
   docker build -t springboot:1.0 .
   ```

4. 运行镜像

   > 因为这个项目使用了mysql服务，所以我们需要提前在docker 容器中运行mysql服务

   

   ```bash
   #启动mysql容器
   docker run -itd --name mysql -p 3307:3306 -e MYSQL_ROOT_PASSWORD=PassWord -v mysqldata:/var/lib/mysql -v mysqlconfig:/etc/mysql/ mysql:5.7.35
   
   #启动springboot Jar包镜像
   docker run -d --name springboot -p 8081:8080 springboot:1.0
   ```

5. 远程访问springboot项目页面（打开防火墙端口），浏览器输入`http://81.68.182.114:8081/user/queryUserById`

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210921120612.png)

# 12. 容器网络配置

## 12.1 概述

当 Docker 启动时，会自动在主机上创建一个 `docker0` 虚拟网桥，实际上是 Linux 的一个 bridge，可以理解为一个软件交换机。它会在挂载到它的网口之间进行转发。

同时，Docker 随机分配一个本地未占用的私有网段（在 [RFC1918](https://datatracker.ietf.org/doc/html/rfc1918) 中定义）中的一个地址给 `docker0` 接口。比如典型的 `172.17.42.1`，掩码为 `255.255.0.0`。此后启动的容器内的网口也会自动分配一个同一网段（`172.17.0.0/16`）的地址。

当创建一个 Docker 容器的时候，同时会创建了一对 `veth pair` 接口（当数据包发送到一个接口时，另外一个接口也可以收到相同的数据包）。这对接口一端在容器内，即 `eth0`；另一端在本地并被挂载到 `docker0` 网桥，名称以 `veth` 开头（例如 `vethAQI2QT`）。通过这种方式，主机可以跟容器通信，容器之间也可以相互通信。Docker 就创建了在主机和所有容器之间一个虚拟共享网络。

## 12.2 命令

### 12.2.1 查看所有网桥

**语法：`docker network ls`**

```bash
[root@VM-4-7-centos test]# docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
7e968f81f5d8   bridge    bridge    local#容器默认指向的网桥
aea0bf59c9aa   host      host      local
1ef8c4f65dec   none      null      local
```

### 12.2.2 创建自定义网桥

**语法：`docker network create 网桥名称`**

相当于`docker network create -d bridge  网桥名称`

```bash
#创建名为jason的网桥
[root@VM-4-7-centos test]# docker network create jason
```

### 12.2.3 运行容器指定网桥

**语法： `docker run [参数] --network 网桥名 镜像名/镜像id`**

```bash
# 使用自定义网桥运行容器
[root@VM-4-7-centos test]#docker run -d --name tomcat01 -p 8081:8080 --network jason tomcat:9.0

# 使用默认网桥运行容器
[root@VM-4-7-centos test]#docker run -d --name tomcat02 -p 8082:8080 tomcat:9.0
```

> 指定网桥之前，需要确保网桥已经被创建

### 12.2.4 容器之间网络通信

1. 查看需要通信的容器的IPAddress地址

   ```bash
   [root@VM-4-7-centos test]# docker inspect tomcat01
   ......
   "Networks": {
       "jason": {
           "IPAMConfig": null,
           "Links": null,
           "Aliases": [
               "8ff441759295"
           ],
           "NetworkID": "a4a13e686497e64c5d248d964234d69a0646c40a4844ac8a6c84d9d1f1469013",
           "EndpointID": "d453ff59c434221c5e5d865482f17ad872c4a9a3773193c23eca16c3709c8902",
           "Gateway": "172.18.0.1",	#实际主机分配挂载的IP
           "IPAddress": "172.18.0.2",#容器内分配挂载的IP
   ......
   ```

2. 使用查询到的IPAddress地址于目标容器进行网络通信（前提要进去容器）

   ```bash
   root@881467d86ec2:/usr/local/tomcat# curl http://172.18.0.2:8080
   ```

   > 如果两个互相通信的容器在一个网桥中，则还可以直接使用容器名来代替IPAddress

### 12.2.5 删除指定网桥

**语法：`docker network rm 网桥名`**

```bash
#删除名为bernardo的网桥
[root@VM-4-7-centos test]# docker network rm bernardo
```

### 12.2.5 查看网桥配置信息

**语法：`docker inspect 网桥名`**

```bash
#查看名为jason的网桥配置信息
[root@VM-4-7-centos test]# docker inspect jason
```

# 13. 使用Docker安装常见服务

## 13.1 使用Docker安装MySQL

1. 进入[docker hub官网](https://hub.docker.com/)搜索MySQL，找到对应的版本

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210920123214.png)

2. 拉取MySQL镜像

   ```bash
   [root@VM-4-7-centos ~]# docker pull mysql:5.7.35
   5.7.35: Pulling from library/mysql
   a330b6cecb98: Pull complete
   ......
   51a9a9b4ef36: Pull complete
   Digest: sha256:d9b934cdf6826629f8d02ea01f28b2c4ddb1ae27c32664b14867324b3e5e1291
   Status: Downloaded newer image for mysql:5.7.35
   docker.io/library/mysql:5.7.35
   ```
   
3. 运行MySQL容器，后台运行、指定容器名字、映射端口、指定root用户密码、挂载数据卷、修改配置文件启动

   ```bash
   [root@VM-4-7-centos ~]# docker run -itd --name mysql -p 3307:3306 -e MYSQL_ROOT_PASSWORD=PassWord -v mysqldata:/var/lib/mysql -v mysqlconfig:/etc/mysql/ mysql:5.7.35
   5137dc9337af350c26e9605a7c07651e80eca7db3c4dd130e8bf8f17b4e5ceeb
   ```

   > 部分配置信息，需要在我们修改后，重启MySQL容器才可以生效
   >
   > 又有部分配置信息，如端口，需要我们重新创建一个容器来使其生效

4. 远程连接Docker中的MySQL服务

   > 需要提前打开安装Docker服务器的防火墙和安全策略组

   ```bash
   ❯ mysql -uroot -p -h81.68.182.114 -P3307
   Enter password:
   Welcome to the MySQL monitor.  Commands end with ; or \g.
   Your MySQL connection id is 5
   Server version: 5.7.35 MySQL Community Server (GPL)
   
   Copyright (c) 2000, 2021, Oracle and/or its affiliates.
   
   Oracle is a registered trademark of Oracle Corporation and/or its
   affiliates. Other names may be trademarks of their respective
   owners.
   
   Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
   
   mysql>
   ```

## 13.2 使用Docker安装Tomcat

1. 进入[docker hub官网](https://hub.docker.com/)搜索Tomcat，找到对应的版本

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210920143645.png)

2. 拉取Tomcat镜像

   ```bash
   [root@VM-4-7-centos ~]# docker pull tomcat:9.0-jre8
   9.0-jre8: Pulling from library/tomcat
   955615a668ce: Pull complete
   ......
   daf564b3b156: Pull complete
   Digest: sha256:10dcb321d65b8ab6d10a8e8b083392e04bcbfef1df1af15572f41c789c669e2a
   Status: Downloaded newer image for tomcat:9.0-jre8
   docker.io/library/tomcat:9.0-jre8
   ```

3. 运行Tomcat容器，后台运行、指定容器名字、映射端口、挂载数据卷、修改配置文件启动

   ```bash
   [root@VM-4-7-centos ~]# docker run -d --name tomcat -p 8081:8080 -v tomcatapps:/usr/local/tomcat/webapps -v tomcatconfs:/usr/local/tomcat/conf tomcat:9.0-jre8
   ```

4. 远程访问Docker中的Tomcat服务

   > 需要提前打开安装Docker服务器的防火墙和安全策略组

   在浏览器输入`http://81.68.182.114:8081/test/test.html`（页面是我提前加入的），查看效果

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210920152740.png)

## 13.3 使用Docker安装Redis

1. 进入[docker hub官网](https://hub.docker.com/)搜索Redis，找到对应的版本

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210920154253.png)

2. 拉取Redis镜像

   ```bash
   [root@VM-4-7-centos test]# docker pull redis:5.0.13
   5.0.13: Pulling from library/redis
   a330b6cecb98: Already exists
   ......
   fe842fc77a96: Pull complete
   Digest: sha256:6aa31467e60f57a15427ab847752cc075d3218f70ba5fb6c913c4b9c09b61c95
   Status: Downloaded newer image for redis:5.0.13
   docker.io/library/redis:5.0.13
   ```

3. 运行Redis容器，后台运行、映射端口、挂载数据卷、指定容器名字、持久化

   ```bash
   docker run -d -p 6380:6379 -v redisdata:/data --name redis redis:5.0.13 redis-server --appendonly yes
   ```

   > 注意点：
   >
   > + 在指定redis镜像名，需要加上需要启动的命令服务器`redis-server`，否则redis不算完全启动
   > + `--appendonly yes`表示开启持久化，默认持久化的文件存储到了容器中的`/data`目录中，所有我们可以挂载此目录，将数据同步到主机上。

   运行Redis容器，后台运行、映射端口、挂载数据卷、指定容器名字、持久化、**以指定配置文件启动**

   ```bash
   docker run -d -p 6380:6380 -v redisdata:/data -v /root/myredis/conf:/usr/local/etc/redis --name redis redis:5.0.13 redis-server /usr/local/etc/redis/redis.conf --appendonly yes 
   ```

   > 如果需要以自定义配置文件启动Redis，需要我们自己手动下载`redis.conf`文件到本机`/root/myredis/conf`路径中（与上面命令对应即可），并挂载到容器的`/usr/local/etc/redis/`目录中，因为Redis的Docker镜像是没有conf配置文件的。最后启动时，还要指定我们自己的配置文件路径`/usr/local/etc/redis/redis.conf`，来通过配置文件启动redis
   >
   >  这里为了测试，我们在配置文件中将redis启动端口改为了6380，并且修改`protected-mode no`、`bind 0.0.0.0`

4. 远程访问Docker中的Redis服务

   > 需要提前打开安装Docker服务器的防火墙和安全策略组

   ```bash
   ❯ redis-cli -h 81.68.182.114 -p 6380
   81.68.182.114:6380>
   ```

## 13.4 使用Docker安装Nginx

1. 进入[docker hub官网](https://hub.docker.com/)搜索Redis，找到对应的版本

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210920170902.png)

2. 拉取Nginx镜像

   ```bash
   [root@VM-4-7-centos conf]# docker pull nginx:1.12.1
   1.12.1: Pulling from library/nginx
   bc95e04b23c0: Pull complete
   37e52b416fab: Pull complete
   641ecf99554d: Pull complete
   Digest: sha256:d92bebac6f0fcf80804744aa2ce630bb293cbb8d25bfa4e8bf3d6de1a08166b7
   Status: Downloaded newer image for nginx:1.12.1
   docker.io/library/nginx:1.12.1
   ```

3. 运行Nginx容器，后台运行、映射端口、指定容器名字

   ```bash
   docker run -d --name nginx -p 81:80 nginx:1.12.1
   ```

4. 远程访问Docker中的Nginx服务

   > 需要提前打开安装Docker服务器的防火墙和安全策略组

   浏览器输入`http://81.68.182.114:81/`

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210920175415.png)

## 13.5 使用Docker安装Elasticsearch

1. 进入[docker hub官网](https://hub.docker.com/)搜索Elasticsearch，找到对应的版本

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210920211443.png)

2. 拉取Elasticsearch镜像

   ```bash
   [root@VM-4-7-centos ~]# docker pull elasticsearch:6.8.0
   ```

3. 运行Elasticsearch容器

   ```bash
   docker run -d --name elasticsearch \
   	-p 9200:9200 \
   	-p 9300:9300 \
   	-v esdata:/usr/share/elasticsearch/data \
   	-v esconfig:/usr/share/elasticsearch/config \
   	-v esplugins:/usr/share/elasticsearch/plugins \
   	--net es \
   	elasticsearch:6.8.0 
   ```

   > `-d ` # 后台运行
   >
   > `--name elasticsearch` #指定容器名字
   >
   > `-p 9200:9200` #映射web端口
   >
   > `-p 9300:9300` #映射Java端口
   >
   > `-v esdata:/usr/share/elasticsearch/data` #挂载持久化数据
   >
   > `-v esconfig:/usr/share/elasticsearch/config` #挂载配置文件
   >
   > `-v esplugins:/usr/share/elasticsearch/plugins` #挂载插件
   >
   > `--net es` #指定网桥 
   >
   > `elasticsearch:6.8.0` #选择镜像

   > **警告：WARNING: IPv4 forwarding is disabled. Networking will not work**
   >
   > 解决办法：
   >
   > 1. 修改`/etc/sysctl.conf`文件，文件最后添加下面内容
   >
   >    ```
   >    net.ipv4.ip_forward=1
   >    ```
   >
   > 2. 重启network服务
   >
   >    ```bash
   >    systemctl restart network
   >    ```
   >
   > 3. 查看是否修改成功
   >
   >    ```bash
   >    sysctl net.ipv4.ip_forward
   >    ```
   >
   >    若返回`net.ipv4.ip_forward = 1`，表示成功
   >
   > 4. 重启容器即可

4. 安装插件`elasticsearch-analysis-ik`（版本要与`elasticsearch`保持一致）

   ```bash
   1. 找到容器中es插件目录`/usr/share/elasticsearch/plugins`挂载的本机卷目录
       {
         "Type": "volume",
         "Name": "esplugins",
         "Source": "/var/lib/docker/volumes/esplugins/_data", #本机卷挂载目录
         "Destination": "/usr/share/elasticsearch/plugins",
         "Driver": "local",
         "Mode": "z",
         "RW": true,
         "Propagation": ""
     }
   2. 上传`elasticsearch-analysis-ik-6.8.0.zip`文件到`/var/lib/docker/volumes/esplugins/_data`目录中
   3. 进入`/var/lib/docker/volumes/esplugins/_data`目录，创建一个`ik`目录，将`elasticsearch-analysis-ik-6.8.0.zip`解压到`ik`目录中
   4. 重启启动es的docker容器，使用`docker logs -f elasticsearch`，可以看`loaded plugin [analysis-ik]`，插件即安装成功
   ```

5. 远程访问ES，浏览器输入`http://81.68.182.114:9200/`

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210921074314.png)

6. 安装`kibana`（版本要与`elasticsearch`保持一致）

   ```bash
   #拉取kibana镜像
   docker pull kibana:6.8.0
   
   #连接es，运行kibana
   docker run -d --name kibana -p 5601:5601 --net es -e ELASTICSEARCH_URL=http://172.23.0.2:9200 kibana:6.8.0
   
   > -e ELASTICSEARCH_URL值通过查看elasticsearch挂载的网桥的ipaddress地址得到（当然kibana也要与elasticsearch挂载到同一网桥上）
   > docker inspect elasticsearch
   > "Networks": {
                   "es": {
                       "IPAMConfig": null,
                       "Links": null,
                       "Aliases": [
                           "01c1faa01c17"
                       ],
                       "NetworkID": "b46c192a40eb534a59006ca5f2815bfc1220bd83a15c0cb0ac5554a8f19b09bc",
                       "EndpointID": "1297e13dce251b386a029dc003d01f15dc296e1956c03d141a16dc7c0c21d3ba",
                       "Gateway": "172.23.0.1",
                       "IPAddress": "172.23.0.2", #这里就是-e ELASTICSEARCH_URL的值
                       "IPPrefixLen": 16,
                       "IPv6Gateway": "",
                       "GlobalIPv6Address": "",
                       "GlobalIPv6PrefixLen": 0,
                       "MacAddress": "02:42:ac:17:00:02",
                       "DriverOpts": null
                   }
   
   #也可以通过配置文件批准连接es，运行kibana
   docker run -d --name kibana -p 5601:5601 --net es -v kibanaconf:/usr/share/kibana/config kibana:6.8.0
   
   > 首先运行一次容器，然后进入卷`kibanaconf`对应的目录中找到`kibana.yml`配置文件，将`elasticsearch.hosts`值为`http://172.23.0.2:9200`
   > 最后重新启动容器即可
   ```

7. 远程访问`kibana`，浏览器输入`http://81.68.182.114:5601`

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210921080109.png)

# 14. Docker-compose

## 14.1 下载Docker-compose

下载Docker-compose

```bash
wget -c https://github.com/docker/compose/releases/download/1.25.5/docker-compose-Linux-x86_64
```

## 14.2 安装Docker-compose

将`docker-compose-Linux-x86_64`移动到`usr/local/bin`目录下，改名，并赋予执行权限

```bash
cp docker-compose-Linux-x86_64 /usr/local/bin/

mv docker-compose-Linux-x86_64 docker-compose

chmod +x /usr/local/bin/docker-compose
```

测试是否安装成功

```bash
[root@VM-4-7-centos bin]# docker-compose -v
docker-compose version 1.25.5, build 8a1c60f6
```

## 14.3  运行Docker-compose

创建编写一个`docker-compose.yml`文件

```yaml
version: "3.0"

services:
  tomcat: #服务名唯一
    image: tomcat:9.0-jre8 #指定镜像
    ports: 
      - 8083:8080

  tomcat02: 
    image: tomcat:9.0-jre8 
    ports:
      - 8084:8080
```

在`docker-compose.yml`文件同级目录中执行下面命令启动Docker-compose

```bash
docker-compose up
```

## 14.4 模版指令

### 14.4.1 docker-compose.yml模版文件

下面的`docker-compose.yml`文件共启动了四个服务——两个tomcat、一个mysql、一个redis

```yaml
version: "3.8" #指定docker-compose版本

services:
  tomcat01: #服务名称
    container_name: tomcat01 #指定容器名称 --name
    image: tomcat:9.0-jre8 #指定使用的镜像
    ports: #映射端口 -p
      - "8084:8080"
    volumes: #挂载数据卷 -v
      - tomcatwebapps01:/usr/local/tomcat/webapps #这里的分号处不能用空格
    networks: #指定网桥 --network
      - hello
    depends_on: #指定此服务在哪些服务后启动
      - mysql #写的是服务名称
      - redis
    healthcheck: #心跳检查，防止服务假死（哪个服务需要，就添加此条目，不需要不添加）
      test: [ "CMD", "curl", "-f", "http://localhost" ]
      interval: 1m30s
      timeout: 10s
      retries: 3
      start_period: 40s
    #sysctls: #配置容器内核参数，在某些容器启动失败时使用进行配置（可选，因为tomcat没有这个参数，我就注释了）
      #- net.core.somaxconn=1024
      #- net.ipv4.tcp_syncookies=0
    ulimits: #修改容器的进程数限制，也是在某些容器启动失败时使用进行配置（可选）
      nproc: 65535
      nofile:
          soft: 20000
          hard: 40000


  tomcat02: #服务名称
    container_name: tomcat02 #指定容器名称 --name
    image: tomcat:9.0-jre8 #指定使用的镜像
    ports: #映射端口 -p
      - "8085:8080"
    volumes: #挂载数据卷 -v
      - tomcatwebapp02:/usr/local/tomcat/webapps #这里的分号处不能用空格
    networks: #指定网桥 --network
      - hello

  mysql: #服务名称
    container_name: mysql01 #容器名称 --name
    image: mysql:5.7.35 #指定使用的镜像
    ports: #映射端口 -p
      - "3308:3306"
    volumes: #挂载数据卷 -v
      - mysqldata:/var/lib/mysql #持久化数据库数据
      - mysqlconfig:/etc/mysql/ #挂载配置文件
    #environment: #配置环境变量
      #- MYSQL_ROOT_PASSWORD=PassWord #直接设置root用户密码
    env_file: #配置环境变量
      - /root/test/MYSQL_ROOT_PASSWORD.env #读取指定路径配置文件来设置root用户密码
    networks: #挂载网桥 --net
      - hello

  redis: #服务名称
    container_name: redis01 #容器名称 --name
    image: redis:5.0.13 #指定使用的镜像
    ports: #映射端口 -p
      - "6380:6379"
    volumes: #挂载数据卷 -v
      - redisdata:/data #持久化数据库数据
    networks:
      - hello
    command: "redis-server --appendonly yes" #启动容器后执行的命令

volumes: #创建上面volumes标签使用的数据卷
  tomcatwebapps01: #声明需要创建的数据卷的名称（compose会自动创建改名的数据卷，但会加入项目名）
    external: #使用自定义卷名，不使用上面自动创建的数据卷名
      true #true表示使用自定义的卷名，但是需要提前手动创建好
  tomcatwebapp02:
  mysqldata:
  mysqlconfig:
  redisdata:

networks: #创建上面networks标签使用到的网桥
  hello: #声明需要创建忙的网桥名称，默认创建的就是bridge
    external: #使用外部指定网桥（如果不需要使用指定网桥，则不需要写）
      false #true表示确定使用外部指定网桥，但网桥必须提前手动创建好
```

### 14.4.2 Image

**作用：**

指定为镜像名称或镜像ID。如果镜像在本地不存在，Compose将会尝试拉取这个镜像。

**示例：**

```yaml
version: "3.2"

services:
  tomcat:
    image: tomcat:8.0-jre8
```

### 14.4.3 build

**作用：**

该命令和image有异曲同工之妙，如果使用image，一般是先编辑Dockerfile，然后再将Dockerfile手动构建成镜像到本地库，最后再使用image引入本地库中的镜像。

使用了build，编辑完Dockfile之后就不需要手动构建成镜像了，Compose帮助我们参照Dockerfile自动构建镜像到本地库，再引入本地库中的镜像。

**示例：**

```yaml
version: "3.2"

services:
  postilhub:
    build:
      # 上下文目录(相对路径)
      context: ./postilhub
      # Dockerfile文件名(默认为Dockerfile)
      dockerfile: Dockerfile
    # 镜像打包后运行的容器名
    container_name: postilhub
    # 映射容器内9000端口
    ports: 
      - "9000:9000"
    # 该服务依赖于tomcat服务
    depends_on: "tomcat"
  tomcat:
	image: tomcat:8.0-jre8
```

### 14.4.4 ports

**作用：**

暴露端口信息。

同时指定宿主机端口和容器端口（HOST : CONTAINER格式），或者仅仅指定容器的端口（宿主将会随机选择端口）。

相当于 `-p`。

> 当使用HOST:CONTAINER格式来映射接口时，如果你使用的容器端口小于60并且没放到引号里，可能会得到错误结果，因为YAML会自动解析xx:yy这种数字格式为60进制。为避免出现这种问题，建议数字串都采用引号包起来的字符串格式.

**示例**

```yaml
version: "3.2"

services:
  tomcat01:
    image: tomcat:8.0-jre8
      ports:
	  - "8080:8080"
  tomcat02:
    image: tomcat:8.0-jre8
      ports:
	- "8081:8080"
	- "5672:5672"
```

### 14.4.5 volumes

**作用：**

设置数据卷挂载路径，" : "前可以设置为宿主机路径（HOT : CONTAINER）或者数据卷名称（VOLUME : CONTAINER），并且最后还可以设置访问模式（HOST : CONTAINER : ro），该指令中所有路径支持相对路径。

相当于 `-v`。

**示例：**

```yaml
version: "3.2"

services:
  tomcat:
    image: tomcat:8.0-jre8
    volumes:
      - /app/postilhub:/usr/local/tomcat/webapps
```

上面是使用宿主机路径来映射的，如果使用数据卷名称映射，如下

```yaml
version: "3.2"

services:
  tomcat:
    image: tomcat:8.0-jre8
    volumes:
      - postilhub:/usr/local/tomcat/webapps
	  
# 声明			
volumes:
  postilhub:
```

数据卷名称shampooApp必须要单独声明，但是数据卷名称和我们指定的数据卷名称并不一样，Compose首先会看 `docker_compose.yml` 文件在哪个路径中，比如在 `/home` 目录中，那么Compose就会 `docker_compose.yml` 所在目录名和自定义数据卷名称组合起来：shampoo_postilhub。

如果想不加目录名，可以添加如下配置：

```yaml
version: "3.2"

services:
  tomcat:
    image: tomcat:8.0-jre8
    volumes:
      - postilhub:/usr/local/tomcat/webapps
			
# 声明数据卷		
volumes:
  postilhub:
    # 是否使用自定义数据卷名
    external:
      true
```

但是这种设置Compose就不会给我们自动创建数据卷了，我们需要使用如下命令，手动创建数据卷，再去启动Project。

```bash
docker volume create postilhub
```

### 14.4.6 networks

**作用：**

配置容器连接网桥，不配置默认就是bridge网桥。

相当于 `--network`。

**示例：**

```yaml
version: "3.2"

services:
  tomcat01:
    image: tomcat:8.0-jre8
    # 指定tomcat01服务使用postilhub网桥
    networks:
      - postilhub
  tomcat02:
    image: tomcat:8.0-jre8
    # 指定tomcat02服务使用postilhub网桥
    networks:
      - postilhub
			
# 声明网桥
networks:
  postilhub:
```

该网桥创建时，网桥名也是`docker_compose.yml` 所在目录名和自定义网桥名称的组合。

如果想不加目录名，可以添加如下配置：

```yaml
version: "3.2"

services:
  tomcat01:
    image: tomcat:8.0-jre8
    # 指定tomcat01服务使用postilhub网桥
    networks:
      - postilhub
  tomcat02:
    image: tomcat:8.0-jre8
    # 指定tomcat02服务使用postilhub网桥
    networks:
      - postilhub
			
# 声明网桥
networks:
  postilhub:
    # 是否使用自定义数据卷名
    external:
      true
```

但是这种设置Compose就不会给我们自动创建网桥了，我们需要使用如下命令，手动创建网桥，再去启动Project。

```bash
docker network create -d bridge postilhub
```

### 14.4.7 container_name

**作用：**

指定容器名称。

相当于 `--name`。

**示例：**

```yaml
version: "3.2"

services:
  tomcat01:
    container_name: tomcat01
    image: tomcat:8.0-jre8
  tomcat02:
    container_name: tomcat02
    image: tomcat:8.0-jre8
			
# 声明网桥
networks:
  postilhub:
```

### 14.4.8 environment

**作用：**

设置环境变量。你可以使用数组或字典两种格式。

相当于 `-e`。

**示例：**

```yaml
version: "3.2"

services:
  mysql:
    container_name: mysql
    image: mysql:8.0
    environment:
      - MYSQL_ROOT_PASSWORD=123456
```

上面使用数组表示，也可以使用字典表示：

```yaml
version: "3.2"

services:
  mysql:
    container_name: mysql
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: 123456
```

### 14.4.9 env_file

**作用：**

从文件中获取环境变量，可以为单独的文件路径或文件路径列表。

**示例：**

按照之前 `environment` 命令的配置

```yaml
version: "3.2"

services:
  mysql:
    container_name: mysql
    image: mysql:8.0
    # environment:
    # - MYSQL_ROOT_PASSWORD=123456
    env_file:
      - ./mysql.env
```

这些密码都属于敏感信息，不应该放在 `docker-compose.yml` 中明文配置。

在 `docker-compose.yml` 目录中创建环境变量配置文件 `mysql.env` 。

> Docker Compose中创建的配置文件必须以 env 结尾，文件中必须遵循 key=value 的格式，同时支持 # 注释。

```properties
MYSQL_ROOT_PASSWORD=123456
```

### 14.4.10 command

**作用：**

覆盖容器启动后默认执行的命令。

**示例：**

```yaml
version: "3.2"

services:
  redis:
    image: redis:5.0.10
      command: "redis-server --appendonly yes"
```

等价于：

```bash
docker run redis:5.0.10 redis-server --appendonly yes
```

### 14.4.11 depends_on

**作用：**

解决容器的依赖、启动先后的问题。

**示例：**

```yaml
version: "3.2"

services:
  postilhub:
    image: postilhub:01
    depneds_on:
      - mysql
      - redis
  mysql:
    image: mysql:8.0
  redis:
    image: redis:5.0.10		
```

以上配置表示postilhub服务依赖于mysql和redis服务，postilhub会在mysql和redis项目启动到一定程度后再启动（不是在mysql和redis服务完全启动时才启动）。

> depends_on配置的是服务名，不是容器名（container_name）。
>
> 依赖可以有级联效应，比如A依赖于B，B依赖于C，那么A也依赖于C。

### 14.4.12 healthcheck

**作用：**

心跳检测，通过命令检查容器是否健康运行。

**示例：**

```yaml
version: "3.2"

services:
  postilhub:
    image: postilhub:1.0
    # 对postilhub服务做心跳检测
    healthcheck:
  	  # 向当前Docker引擎发curl请求
      test: [ "CMD", "curl", "-f", "http://localhost" ]
      # 规定响应时间
      interval: 1m30s
      # 超时时间
      timeout: 10s
      # 重试次数
      retries: 3
```

> 如果部署的是远程Docker，需要将localhost改为远程Docker的IP。

### 14.4.13 sysctls

**作用：**

配置容器内核参数。

**示例：**

并不是必须配置，有些服务启动受容器内操作系统参数限制可能会无法启动，必须通过修改容器中参数才能启动（例如ES）。

```yaml
version: "3.2"

services:
  postilhub:
    image: postilhub:1.0
    # 对运行postilhub服务的容器做系统内核配置
    sysctls:
      - net.core.somaxconn=1024 
      - net.ipv4.tcp_syncookies=0 
```

### 14.4.14 ulimits

**作用：**

指定容器运行的进程数。

**示例：**

并不是必须配置，根据某些特殊服务进行配置。

```yaml
version: "3.2"

services:
  postilhub:
    image: postilhub:1.0
    # 对运行postilhub服务的容器做进程数配置
    ulimits:
      nproc: 65535
      nofile:
        soft: 20000
        hard: 40000
```

### 14.4.15 文件开头版本定义的问题

`docker-compose`与`docker`的版本对应关系如下图：

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210921165208.png)

### 14.4.16 healthcheck报错问题

需要将`docker-compose.yml`文件开头的`version`指定为`3.8`

## 14.5 Compose指令

`docker-compose.yml` 文件编辑完毕后运行的指令，**作用对象是Compose中的Project**，也就是说Project中的所有Services都会受到影响。

### 14.5.1 up

**作用：** 启动Project的所有Service。

**示例：**

启动所有Service。

```bash
docker-compose up
```

该命令还可以启动若干个Service，必须传入Service名

```bash
docker-compose up 服务名1 服务名2 ...
```

默认情况，`docker-compose up` 启动的容器都在前台，如果想要后台启动（Ctrl + C 停止所有容器）

```bash
docker-compose up -d
```

### 14.5.2 down

**作用：** 停止Project的所有Service，同时移除由模板文件自动创建的网桥（external创建的外部网桥不会被移除），但是不会移除数据卷。

**示例：**

停止所有Service。

```bash
docker-compose down
```

### 14.5.3 exec

**作用：** 进入Project中某个Service所在的容器。

**示例：**

```bash
docker-compose exec 服务名
```

### 14.5.4 ps

**作用：** 列出Project中目前的所有容器。

**示例：**

```bash
docker-compose ps
```

### 14.5.5 restart

**作用：** 重启Project中的所有Service。

**示例：**

```bash
docker-compose restart
```

该命令还可以重启若干个Service，必须传入Service名

```bash
docker-compose restart 服务名1 服务名2 ...
```

### 14.5.6 rm

**作用：** 删除Project中的所有Service。

**示例：**

```bash
docker-compose rm
```

该命令还可以删除若干个Service，必须传入Service名

```bash
docker-compose rm 服务名1 服务名2 ...
```

如果需要强制删除

```bash
docker-compose rm -f 服务名1 服务名2 ...
```

如果需要连带数据卷一起删除

```bash
docker-compose rm -v 服务名1 服务名2 ...
```

### 14.5.7 stop

**作用：** 关闭Project中的所有Service，仅仅是关闭，不会删除Service或者网桥。

**示例：**

```bash
docker-compose stop
```

该命令还可以关闭若干个Service，必须传入Service名

```bash
docker-compose stop 服务名1 服务名2 ...
```

### 14.5.8 top

**作用：** 查看Project中各个Service容器内运行的进程。

**示例：**

```bash
docker-compose top
```

该命令还可以查看若干个Service容器，必须传入Service名

```bash
docker-compose top 服务名1 服务名2 ...
```

### 14.5.9 pause

**作用：** 暂停Project中所有Service，使用ps会查看到该Service的State是pause而不是up或者down。

**示例：**

```bash
docker-compose pause
```

该命令还可以暂停若干个Service，必须传入Service名

```bash
docker-compose pause 服务名1 服务名2 ...
```

### 14.5.10 unpause

**作用：** 恢复暂停Project中所有Service。

**示例：**

```bash
docker-compose unpause
```

该命令还可以暂停恢复若干个Service，必须传入Service名

```bash
docker-compose unpause 服务名1 服务名2 ...
```

### 14.5.11 logs

**作用：** 查看Project中所有Service的日志。

**示例：**

```bash
docker-compose logs
```

该命令还可以查看若干个Service的日志，必须传入Service名

```bash
docker-compose logs 服务名1 服务名2 ...
```