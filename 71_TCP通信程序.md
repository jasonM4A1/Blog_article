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

### 案例要求

运用前面所学的`ServerSocket`类和`Socket`类及他们的常用方法，来实现一个`客户端`与`服务端`进行数据交互的程序。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201030150315.jpg)

### 案例实现

**服务端**

~~~java
import java.net.ServerSocket;
import java.net.Socket;
import java.io.InputStream;
import java.io.OutputStream;

public class ServerTCP {
    public static void main(String[] args) throws Exception {
        System.out.println("服务端启动，等待连接...");
        // 1.创建ServerSocket对象，绑定端口，开始等待连接
        ServerSocket ss = new ServerSocket(6666);
        // 2.获取socket对象
        Socket server = ss.accept();
        // 3.获取InputStream对象
        InputStream is = server.getInputStream();
        // 4.读取客户端发送来的数据
        byte[] bytes = new byte[1024];
        int len = is.read(bytes);
        // 5.打印读取到的数据
        String mgs = new String(bytes, 0, len);
        System.out.println(mgs);
        // 6.获取OutputStream对象
        OutputStream os = server.getOutputStream();
        // 7.给客户端回写数据
        os.write("我已经收到了您发送的数据，多些您呢~".getBytes());
        // 8.关闭资源
        os.close();
        is.close();
        server.close();
    }
}
//结果：
//服务端启动，等待连接...
//服务器你在干什么呢？
~~~

**客户端**

~~~java
import java.net.Socket;
import java.io.InputStream;
import java.io.OutputStream;

public class ClientTCP {
    public static void main(String[] args) throws Exception {
        System.out.println("客户端开始发送数据了....");
        // 1.创建一个Socket对象，并绑定主机名和端口号
        Socket client = new Socket("localhost",6666);
        // 2.获取OutputStream对象
        OutputStream os = client.getOutputStream();
        // 3.向服务端发送数据
        os.write("服务器你在干什么呢？".getBytes());
        // 4.获取InputStream对象
        InputStream is = client.getInputStream();
        // 5.读取服务器回写来的数据
        byte[] bytes = new byte[1024];
        int len = is.read(bytes);
        // 6.打印读取到的数据
        String mgs = new String(bytes,0,len);
        System.out.println(mgs);
        // 7.关闭资源
        is.close();
        os.close();
        client.close();
    }
}
//结果：
//客户端开始发送数据了....
//我已经收到了您发送的数据，多些您呢~
~~~

## 课后练习一：文件上传

### 案例要求

将文件从客户端上传到服务端。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201030181159.jpg)

### 案例实现

+ 服务器端

~~~java
import java.net.ServerSocket;
import java.net.Socket;
import java.io.*;

public class FileUpload_Server {
    public static void main(String[] args) throws Exception {
        System.out.println("服务端启动，等待连接...");
        // 1.创建ServerSocket对象，绑定端口号
        ServerSocket ss = new ServerSocket(6666);
        // 循环，确保能一直连接到客户端
        while (true) {
            // 2.获取Socket对象
            Socket server = ss.accept();
            // 采用多线程，提高效率
            new Thread(() -> {
                try (
                        // 3.创建输入流，用于接收客户端发送的数据
                        BufferedInputStream bis = new BufferedInputStream(server.getInputStream());
                        // 4.创建输出流，用于将接收到数据写入硬盘
                        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(new File("D:\\pictures\\压缩后", "top.liboshuai"+System.currentTimeMillis()+".jpg")))
                ) {
                    // 5.进行数据的读写
                    int len = 0;
                    byte[] bytes = new byte[1024];
                    while ((len = bis.read(bytes)) != -1) {
                        bos.write(bytes);
                    }
                    System.out.println("上传成功，等待下一次文件长传");
                    // 6.创建输出流，用于给客户端回写数据
                    OutputStream os = server.getOutputStream();
                    os.write("服务端已经接受到文件了".getBytes());
                    os.close();
                } catch (IOException e) {
                    e.printStackTrace();
                } finally {
                    try {
                        if (server != null) {
                            server.close();
                        }
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }).start();
        }
    }
}
~~~

+ 客户端

~~~java
import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.FileInputStream;
import java.io.InputStream;
import java.net.Socket;

public class FileUpload_Client {
    public static void main(String[] args) throws Exception {
        System.out.println("客户端启动，开始上传文件...");
        // 1.创建Socket对象
        Socket client = new Socket("localhost",6666);
        // 2.获取输入流，将本地文件写入流
        BufferedInputStream bis = new BufferedInputStream(new FileInputStream("D:\\pictures\\压缩后\\one.jpg"));
        // 3.获取输出流，将文件数据发给服务端
        BufferedOutputStream bos = new BufferedOutputStream(client.getOutputStream());
        // 4.获取输出流，接收服务端回写来的数据
        InputStream is = client.getInputStream();
        // 5.进行数据读写
        int len = 0;
        byte[] bytes = new byte[1024];
        while ((len = bis.read(bytes)) != -1) {
            bos.write(bytes,0,len);
        }
        client.shutdownOutput();
        // 6.接收服务端回写的数据
        byte[] back = new byte[50];
        is.read(back);
        System.out.println(new String(back));;

        is.close();
        client.close();
        bis.close();
    }
}
~~~

## 课后练习二：模拟B\S服务器

### 案例要求

模拟网站服务器，使用浏览器访问自己编写的服务端程序，查看网页效果。

### 案例分析

1. 准备页面数据，web文件夹，复制到我们`Module`中。

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201106011931.png)

2. 编写代码

3. 使用浏览器访问`localhost:8888/web/index.html`。

   ![](https://gitee.com/jasonM4A1/pictureHost/raw/master/20201106040517.png)

### 案例实现

~~~java
import java.net.ServerSocket;
import java.net.Socket;
import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException {
        System.out.println("服务端启动，等待连接...");
        // 1.创建ServerSocket对象，绑定端口号
        ServerSocket ss = new ServerSocket(8888);
        // 2.创建多线程使浏览器可以访问所有的图片
        while (true) {
            Socket socket = ss.accept();
            new Thread(new Web(socket)).start();
        }
    }
    static class Web implements Runnable {
        private Socket socket;

        public Web(Socket socket) {
            this.socket = socket;
        }

        @Override
        public void run() {
            try {
                // 3.读取浏览器请求的资源路径
                BufferedReader br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
                // 3.1读取资源路径所在文本的第一行
                String request = br.readLine();
                // 3.2用空格分隔文本
                String[] strArr = request.split(" ");
                // 3.3去除/，得到资源路径
                String path = strArr[1].substring(1);
                // 4.读取资源文件到流中
                BufferedInputStream bis = new BufferedInputStream(new FileInputStream(path));
                // 5.将资源文件发送给浏览器
                BufferedOutputStream bos = new BufferedOutputStream(socket.getOutputStream());
                // 6.进行文件读写
                bos.write("HTTP/1.1 200 OK\r\n".getBytes());
                bos.write("Content-Type:text/html\r\n".getBytes());
                bos.write("\r\n".getBytes());
                int len = 0;
                byte[] bytes = new byte[1024];
                while ((len = bis.read(bytes)) != -1) {
                    bos.write(bytes,0,len);
                }
                // 7.关闭资源
                bos.close();
                bis.close();
                br.close();
                socket.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
~~~

