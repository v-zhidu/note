# Servlet 简介

## Servlet 是什么？

Java Servlet是运行在Web服务器或应用程序上的程序，它是作为来自Web浏览器和其他HTTP客户端的请求和服务器上的数据库和应用程序的中间层。

## Servlet 架构

![Servlet架构](http://www.runoob.com/wp-content/uploads/2014/07/servlet-arch.jpg)

## Servlet 任务

Servlet执行以下主要任务:

* 读取客户端发送的显式的数据。包括网页上的HTML表单，或者来自其它HTTP客户端的表单。
* 读取客户端发送的隐式的数据。包括cookies、媒体类型和浏览器能理解的压缩格式等等。
* 处理数据并生成结果。这个过程可能需要访问数据库，执行RMI或CORBA，调用API服务，或者直接计算得出对应的响应。
* 发送显式的数据（即文档）到客户端。HTML,XML等格式。
* 发送隐式的数据（HTTP响应）到客户端。设置cookies和缓存参数，以及其他类似的任务。


## 相比于CGI，Servlet有以下几点优势：

* 性能明显更好 - Servlet在Web服务器的地址空间内执行，这样它就没有必要再创建一个单独的进程来处理每个客户的请求。
* 跨平台特性
* Servlet是可信的 - 服务器上Java安全管理执行力一系列的限制，以保护服务器计算机上的资源。
* Java类库支持 - 它可以通过sockets和RMI机制与applets、数据库或其他软件进行交互。

## Servlet容器 - Tomcat, Jetty

## 其他语言平台实现的CGI

[资料](https://www.biaodianfu.com/cgi-fastcgi-wsgi.html)

### FastCGI

### WSGI