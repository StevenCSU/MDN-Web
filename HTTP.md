# 学习MDN web docs上的http教程的笔记

**超文本传输​​协议(HTTP)**是用于传输诸如HTML的超媒体文档的应用层协议。它被设计用于Web浏览器和Web服务器之间的通信，但它也可以用于其他目的。 HTTP遵循经典的客户端-服务端模型，客户端打开一个连接以发出请求，然后等待它收到服务器端响应。 HTTP是无状态协议，意味着服务器不会在两个请求之间保留任何数据（状态）。虽然通常基于TCP / IP层，但可以在任何可靠的传输层上使用; 也就是说，一个不会静默丢失消息的协议，如UDP。

---

## HTTP概述

*Protocol* A protocol is a system of rules that define how data is exchanged within or between computers. Communications between devices require that the devices agree on the format of the data that is being exchanged. The set of rules that defines a format is called a protocol.

客户端和服务端通过交换各自的消息（与数据流正好相反）进行交互。由像浏览器这样的客户端发出的消息叫做 requests，被服务端响应的消息叫做 responses。

### 基于HTTP的组件系统

#### 客户端：user-agent
user-agent 就是任何能够为用户发起行为的工具。这个角色通常都是由浏览器来扮演。一些例外情况，比如是工程师使用的程序，以及Web开发人员调试应用程序。

#### Web服务端
Server只是虚拟意义上代表一个机器：它可以是共享负载（负载均衡）的一组服务器组成的计算机集群，也可以是一种复杂的软件，通过向其他计算机（如缓存，数据库服务器，电子商务服务器 ...）发起请求来获取部分或全部资源。

#### 代理 Proxies
代理的作用
- 缓存（可以是公开的也可以是私有的，像浏览器的缓存）
- 过滤（像反病毒扫描，家长控制...）
- 负载均衡（让多个服务器服务不同的请求）
- 认证（对不同资源进行权限管理）
- 日志记录（允许存储历史信息）

### HTTP的基本性质
- HTTP是简单的
- HTTP是可扩展的
- HTTP是无状态，有会话的
    可以使用cookie创建有状态的对话
- HTTP和连接

### HTTP能控制什么
- 缓存
- 开放同源限制
- 认证
- 代理和隧道
- 会话

### HTTP流
1. 打开一个TCP连接
2. 发送一个HTTP报文
3. 读取客户端返回的报文信息
4. 关闭连接或者为后续请求重用连接

### HTTP报文
**请求**
```http
GET / HTTP/1.1
Host: developer.mozilla.org
Accept-Language: fr
```
请求由以下元素组成
- 一个HTTP的method
- 要获取的资源的路径
- HTTP协议版本号
- 为服务端表达其他信息的可选头部headers
- 对于一些像POST这样的方法，报文的body就包含了发送的资源，这与响应报文的body类似

**响应**
```http
HTTP/1.1 200 OK
Date: Sat, 09 Oct 2010 14:28:02 GMT
Server: Apache
Last-Modified: Tue, 01 Dec 2009 20:18:22 GMT
ETag: "51142bc1-7449-479b075b2891b"
Accept-Ranges: bytes
Content-Length: 29769
Content-Type: text/html

<!DOCTYPE html... (here comes the 29769 bytes of the requested web page)
```
响应报文包含了下面的元素
- HTTP协议版本号
- 一个状态码（status code），来告知对应请求执行成功或失败，以及失败的原因。
- 一个状态信息，这个信息是非权威的状态码描述信息，可以由服务端自行设定。
- HTTP headers，与请求头部类似。
- 可选项，比起请求报文，响应报文中更常见地包含获取的资源body。