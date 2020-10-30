---
title: TCP通信程序
date: 2020-10-30 14:15:10
tags:
	- Java
	- 网络
	- 基础知识
categories:
	- Java
	- 复习
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201030141627.jpg
---

# TCP通信程序

## 概述

TCP通信能实现两台计算机之间的数据交互，通信的两端，要严格区分为客户端（Client）与服务端（Server）。

**两端通信时步骤：**

1. 服务端程序，需要事先启动，等待客户端的连接。
2. 客户端主动连接服务器端，连接成功才能通信。服务端不可以主动连接客户端。

**在Java中，提供了两个类用于实现TCP通信程序：**

1. **客户端：**`java.net.Socket`类。创建`Socket`对象，向服务器发出连接请求，服务端响应请求，两者建立连接开始通信。
2. **服务端：**`java.net.ServerSocket`类。创建`ServerSocket`对象，相当于开启一个服务，并等待客户端的连接。

## Socket类

### 概述

`java.net.Socket`该类实现客户端套接字，套接字指的是两台设备之间通讯的端点。

### 构造方法

+ `Socket(String host, int port)`：创建一个流套接字，并将其连接到指定主机上的指定端口号。如果指定的host是null ，则相当于指定地址为回送地址。  

  > 小贴士：回送地址(127.x.x.x) 是本机回送地址（Loopback Address），主要用于网络软件测试以及本地机进程间通信，无论什么程序，一旦使用回送地址发送数据，立即返回，不进行任何网络传输。

**代码演示**

~~~java
import java.io.IOException;
import java.net.Socket;

public class Test {
    public static void main(String[] args) {
        try {
            Socket client = new Socket("127.0.0.1", 6666);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
~~~

### 常用方法

1. `InputStream getInputStream()`：返回此套接字的输入流。
   + 如果此Scoket具有相关联的通道，则生成的InputStream 的所有操作也关联该通道。
   + 关闭生成的InputStream也将关闭相关的Socket。
2. `OutputStream getOutputStream()`：返回此套接字的输出流。
   + 如果此Scoket具有相关联的通道，则生成的OutputStream 的所有操作也关联该通道。
   + 关闭生成的OutputStream也将关闭相关的Socket。
3. `void close()`：关闭套接字。
   + 一旦一个socket被关闭，它不可再使用。
   + 关闭此socket也将关闭相关的InputStream和OutputStream 。 
4. `void shutdownOutput()`：禁用此套接字的输出流。
   + 任何先前写出的数据将被发送，随后终止输出流。 

**代码演示**

~~~java
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.Socket;

public class Test {
    public static void main(String[] args) {
        try {
            //通过传入主机名称和端口号，创建一个Socket对象
            Socket client = new Socket("127.0.0.1", 6666);
            //获取此套接字的输入流
            InputStream is = client.getInputStream();
            //获取此套接字的输出流
            OutputStream os = client.getOutputStream();
            //禁用此套接字的输出流
            client.shutdownOutput();
            //关闭此套接字
            client.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
~~~

## ServerSocket类

### 概述

`java.net.ServerSocket`这个类实现了服务器套接字，该对象等待通过网络的请求。

### 构造方法

+ `ServerSocket(int port)`：创建一个服务器套接字，绑定到指定的端口。

**代码演示**

~~~java
import java.net.ServerSocket;
import java.io.IOException;

public class Test {
    public static void main(String[] args) {
        try {
            //创建一个服务器套接字，绑定到指定的端口。
            ServerSocket server = new ServerSocket(6666);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
~~~

### 常用方法

+ `Socket accept()`：侦听并接受连接，返回一个新的Socket对象，用于和客户端实现通信。该方法会一直阻塞直到建立连接。 

**代码演示**

~~~java
import java.net.ServerSocket;
import java.net.Socket;
import java.io.IOException;

public class Test {
    public static void main(String[] args) {
        try {
            //创建一个ServerSocket对象，并绑定到指定的端口上
            ServerSocket server = new ServerSocket(6666);
            //监听与此套接字建立的连接，并接受它
            Socket client = server.accept();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
~~~

## 简单的TCP网络程序

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201030150315.jpg)

### 程序实现步骤

1. 【服务端】启动，创建`ServerSocket`对象，等待连接。
2. 【客户端】启动，创建`Socket`对象，请求连接。
3. 【服务端】调用`accept()`方法，接收连接，并返回一个`Socket`对象。
4. 【客户端】通过调用`Socket`对象的`getOutputStream()`方法，来获取`OutputStream`，以便于向服务端写出数据。
5. 【服务端】通过调用`Socket`对象的`getInputStream()`方法，来获取`InputStream`，以便于向客户端发送数据。
6. 【服务端】

### 程序代码实现

**服务端**

~~~java

~~~

**客户端**

~~~java

~~~

