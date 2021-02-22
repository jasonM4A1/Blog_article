---
title: >-
  Tomcat报错：严重 [RMI TCP Connection(3)-127.0.0.1]
  org.apache.catalina.core.ContainerBase.addChildInternal
  ContainerBase.addChild: start:
date: 2021-02-09 12:09:37
tags:
	- JavaWeb
	- Tomcat
	- 报错
categories:
	- 踩坑记录
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210209121103.jpg
---

# 问题描述

在启动Tomcat时，遇到下面的错误。

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210209121210.png)

```
09-Feb-2021 12:08:53.832 严重 [RMI TCP Connection(3)-127.0.0.1] org.apache.catalina.core.ContainerBase.addChildInternal ContainerBase.addChild: start: 
	org.apache.catalina.LifecycleException: 无法启动组件[StandardEngine[Catalina].StandardHost[localhost].StandardContext[/Servlet]]
		at org.apache.catalina.util.LifecycleBase.handleSubClassException(LifecycleBase.java:440)
		at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:198)
		at org.apache.catalina.core.ContainerBase.addChildInternal(ContainerBase.java:743)
		at org.apache.catalina.core.ContainerBase.addChild(ContainerBase.java:719)
		at org.apache.catalina.core.StandardHost.addChild(StandardHost.java:705)
		at org.apache.catalina.startup.HostConfig.manageApp(HostConfig.java:1719)
		at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
		at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
		at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
		at java.lang.reflect.Method.invoke(Method.java:498)
		at org.apache.tomcat.util.modeler.BaseModelMBean.invoke(BaseModelMBean.java:286)
		at com.sun.jmx.interceptor.DefaultMBeanServerInterceptor.invoke(DefaultMBeanServerInterceptor.java:819)
		at com.sun.jmx.mbeanserver.JmxMBeanServer.invoke(JmxMBeanServer.java:801)
		at org.apache.catalina.mbeans.MBeanFactory.createStandardContext(MBeanFactory.java:482)
		at org.apache.catalina.mbeans.MBeanFactory.createStandardContext(MBeanFactory.java:431)
		at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
		at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
		at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
		at java.lang.reflect.Method.invoke(Method.java:498)
		at org.apache.tomcat.util.modeler.BaseModelMBean.invoke(BaseModelMBean.java:286)
		at com.sun.jmx.interceptor.DefaultMBeanServerInterceptor.invoke(DefaultMBeanServerInterceptor.java:819)
		at com.sun.jmx.mbeanserver.JmxMBeanServer.invoke(JmxMBeanServer.java:801)
		at com.sun.jmx.remote.security.MBeanServerAccessController.invoke(MBeanServerAccessController.java:468)
		at javax.management.remote.rmi.RMIConnectionImpl.doOperation(RMIConnectionImpl.java:1468)
		at javax.management.remote.rmi.RMIConnectionImpl.access$300(RMIConnectionImpl.java:76)
		at javax.management.remote.rmi.RMIConnectionImpl$PrivilegedOperation.run(RMIConnectionImpl.java:1309)
		at java.security.AccessController.doPrivileged(Native Method)
		at javax.management.remote.rmi.RMIConnectionImpl.doPrivilegedOperation(RMIConnectionImpl.java:1408)
		at javax.management.remote.rmi.RMIConnectionImpl.invoke(RMIConnectionImpl.java:829)
		at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
		at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
		at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
		at java.lang.reflect.Method.invoke(Method.java:498)
		at sun.rmi.server.UnicastServerRef.dispatch(UnicastServerRef.java:357)
		at sun.rmi.transport.Transport$1.run(Transport.java:200)
		at sun.rmi.transport.Transport$1.run(Transport.java:197)
		at java.security.AccessController.doPrivileged(Native Method)
		at sun.rmi.transport.Transport.serviceCall(Transport.java:196)
		at sun.rmi.transport.tcp.TCPTransport.handleMessages(TCPTransport.java:573)
		at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run0(TCPTransport.java:834)
		at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.lambda$run$0(TCPTransport.java:688)
		at java.security.AccessController.doPrivileged(Native Method)
		at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run(TCPTransport.java:687)
		at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
		at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
		at java.lang.Thread.run(Thread.java:748)
	Caused by: java.lang.IllegalArgumentException: servlet映射中的<url pattern>[Servlet01]无效
		at org.apache.catalina.core.StandardContext.addServletMappingDecoded(StandardContext.java:3173)
		at org.apache.catalina.core.StandardContext.addServletMappingDecoded(StandardContext.java:3160)
		at org.apache.catalina.startup.ContextConfig.configureContext(ContextConfig.java:1337)
		at org.apache.catalina.startup.ContextConfig.webConfig(ContextConfig.java:1114)
		at org.apache.catalina.startup.ContextConfig.configureStart(ContextConfig.java:778)
		at org.apache.catalina.startup.ContextConfig.lifecycleEvent(ContextConfig.java:299)
		at org.apache.catalina.util.LifecycleBase.fireLifecycleEvent(LifecycleBase.java:123)
		at org.apache.catalina.core.StandardContext.startInternal(StandardContext.java:5053)
		at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:183)
		... 44 more
09-Feb-2021 12:08:53.834 严重 [RMI TCP Connection(3)-127.0.0.1] org.apache.tomcat.util.modeler.BaseModelMBean.invoke Exception invoking method manageApp
	java.lang.IllegalStateException: ContainerBase.addChild: start: org.apache.catalina.LifecycleException: 无法启动组件[StandardEngine[Catalina].StandardHost[localhost].StandardContext[/Servlet]]
		at org.apache.catalina.core.ContainerBase.addChildInternal(ContainerBase.java:747)
		at org.apache.catalina.core.ContainerBase.addChild(ContainerBase.java:719)
		at org.apache.catalina.core.StandardHost.addChild(StandardHost.java:705)
		at org.apache.catalina.startup.HostConfig.manageApp(HostConfig.java:1719)
		at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
		at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
		at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
		at java.lang.reflect.Method.invoke(Method.java:498)
		at org.apache.tomcat.util.modeler.BaseModelMBean.invoke(BaseModelMBean.java:286)
		at com.sun.jmx.interceptor.DefaultMBeanServerInterceptor.invoke(DefaultMBeanServerInterceptor.java:819)
		at com.sun.jmx.mbeanserver.JmxMBeanServer.invoke(JmxMBeanServer.java:801)
		at org.apache.catalina.mbeans.MBeanFactory.createStandardContext(MBeanFactory.java:482)
		at org.apache.catalina.mbeans.MBeanFactory.createStandardContext(MBeanFactory.java:431)
		at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
		at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
		at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
		at java.lang.reflect.Method.invoke(Method.java:498)
		at org.apache.tomcat.util.modeler.BaseModelMBean.invoke(BaseModelMBean.java:286)
		at com.sun.jmx.interceptor.DefaultMBeanServerInterceptor.invoke(DefaultMBeanServerInterceptor.java:819)
		at com.sun.jmx.mbeanserver.JmxMBeanServer.invoke(JmxMBeanServer.java:801)
		at com.sun.jmx.remote.security.MBeanServerAccessController.invoke(MBeanServerAccessController.java:468)
		at javax.management.remote.rmi.RMIConnectionImpl.doOperation(RMIConnectionImpl.java:1468)
		at javax.management.remote.rmi.RMIConnectionImpl.access$300(RMIConnectionImpl.java:76)
		at javax.management.remote.rmi.RMIConnectionImpl$PrivilegedOperation.run(RMIConnectionImpl.java:1309)
		at java.security.AccessController.doPrivileged(Native Method)
		at javax.management.remote.rmi.RMIConnectionImpl.doPrivilegedOperation(RMIConnectionImpl.java:1408)
		at javax.management.remote.rmi.RMIConnectionImpl.invoke(RMIConnectionImpl.java:829)
		at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
		at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
		at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
		at java.lang.reflect.Method.invoke(Method.java:498)
		at sun.rmi.server.UnicastServerRef.dispatch(UnicastServerRef.java:357)
		at sun.rmi.transport.Transport$1.run(Transport.java:200)
		at sun.rmi.transport.Transport$1.run(Transport.java:197)
		at java.security.AccessController.doPrivileged(Native Method)
		at sun.rmi.transport.Transport.serviceCall(Transport.java:196)
		at sun.rmi.transport.tcp.TCPTransport.handleMessages(TCPTransport.java:573)
		at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run0(TCPTransport.java:834)
		at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.lambda$run$0(TCPTransport.java:688)
		at java.security.AccessController.doPrivileged(Native Method)
		at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run(TCPTransport.java:687)
		at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
		at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
		at java.lang.Thread.run(Thread.java:748)
09-Feb-2021 12:08:53.834 严重 [RMI TCP Connection(3)-127.0.0.1] org.apache.tomcat.util.modeler.BaseModelMBean.invoke Exception invoking method createStandardContext
	javax.management.RuntimeOperationsException: Exception invoking method manageApp
		at org.apache.tomcat.util.modeler.BaseModelMBean.invoke(BaseModelMBean.java:294)
		at com.sun.jmx.interceptor.DefaultMBeanServerInterceptor.invoke(DefaultMBeanServerInterceptor.java:819)
		at com.sun.jmx.mbeanserver.JmxMBeanServer.invoke(JmxMBeanServer.java:801)
		at org.apache.catalina.mbeans.MBeanFactory.createStandardContext(MBeanFactory.java:482)
[2021-02-09 12:08:53,839] Artifact Servlet:war exploded: Error during artifact deployment. See server log for details.
		at org.apache.catalina.mbeans.MBeanFactory.createStandardContext(MBeanFactory.java:431)
		at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
		at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
		at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
		at java.lang.reflect.Method.invoke(Method.java:498)
		at org.apache.tomcat.util.modeler.BaseModelMBean.invoke(BaseModelMBean.java:286)
		at com.sun.jmx.interceptor.DefaultMBeanServerInterceptor.invoke(DefaultMBeanServerInterceptor.java:819)
		at com.sun.jmx.mbeanserver.JmxMBeanServer.invoke(JmxMBeanServer.java:801)
		at com.sun.jmx.remote.security.MBeanServerAccessController.invoke(MBeanServerAccessController.java:468)
		at javax.management.remote.rmi.RMIConnectionImpl.doOperation(RMIConnectionImpl.java:1468)
		at javax.management.remote.rmi.RMIConnectionImpl.access$300(RMIConnectionImpl.java:76)
		at javax.management.remote.rmi.RMIConnectionImpl$PrivilegedOperation.run(RMIConnectionImpl.java:1309)
		at java.security.AccessController.doPrivileged(Native Method)
		at javax.management.remote.rmi.RMIConnectionImpl.doPrivilegedOperation(RMIConnectionImpl.java:1408)
		at javax.management.remote.rmi.RMIConnectionImpl.invoke(RMIConnectionImpl.java:829)
		at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
		at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
		at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
		at java.lang.reflect.Method.invoke(Method.java:498)
		at sun.rmi.server.UnicastServerRef.dispatch(UnicastServerRef.java:357)
		at sun.rmi.transport.Transport$1.run(Transport.java:200)
		at sun.rmi.transport.Transport$1.run(Transport.java:197)
		at java.security.AccessController.doPrivileged(Native Method)
		at sun.rmi.transport.Transport.serviceCall(Transport.java:196)
		at sun.rmi.transport.tcp.TCPTransport.handleMessages(TCPTransport.java:573)
		at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run0(TCPTransport.java:834)
		at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.lambda$run$0(TCPTransport.java:688)
		at java.security.AccessController.doPrivileged(Native Method)
		at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run(TCPTransport.java:687)
		at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
		at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
		at java.lang.Thread.run(Thread.java:748)
	Caused by: java.lang.IllegalStateException: ContainerBase.addChild: start: org.apache.catalina.LifecycleException: 无法启动组件[StandardEngine[Catalina].StandardHost[localhost].StandardContext[/Servlet]]
		at org.apache.catalina.core.ContainerBase.addChildInternal(ContainerBase.java:747)
		at org.apache.catalina.core.ContainerBase.addChild(ContainerBase.java:719)
		at org.apache.catalina.core.StandardHost.addChild(StandardHost.java:705)
		at org.apache.catalina.startup.HostConfig.manageApp(HostConfig.java:1719)
		at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
		at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
		at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
		at java.lang.reflect.Method.invoke(Method.java:498)
		at org.apache.tomcat.util.modeler.BaseModelMBean.invoke(BaseModelMBean.java:286)
		... 35 more
09-Feb-2021 12:09:03.340 信息 [localhost-startStop-1] org.apache.catalina.startup.HostConfig.deployDirectory 把web 应用程序部署到目录 [D:\JavaSoftware\apache-tomcat-8.5.61\webapps\manager]
09-Feb-2021 12:09:03.399 信息 [localhost-startStop-1] org.apache.catalina.startup.HostConfig.deployDirectory Web应用程序目录[D:\JavaSoftware\apache-tomcat-8.5.61\webapps\manager]的部署已在[58]毫秒内完成
```

# 解决方法

![](https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210209121328.png)

web.xml文件的url-pattern标签的的路径设置不正确，加上/ 即可。