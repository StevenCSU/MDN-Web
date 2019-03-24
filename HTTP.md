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

---

## HTTP缓存

### 各种类型的缓存

缓存是一种保存资源副本并在下次请求时直接使用该副本的技术。当 web 缓存发现请求的资源已经被存储，它会拦截请求，返回该资源的拷贝，而不会去源服务器重新下载。这样带来的好处有：缓解服务器端压力，提升性能(获取资源的耗时更短了)。对于网站来说，缓存是达到高性能的重要组成部分。缓存需要合理配置，因为并不是所有资源都是永久不变的：重要的是对一个资源的缓存应截止到其下一次发生改变（即不能缓存过期的资源）。

大致分为两类：私有缓存和共享缓存。

### 缓存操作的目标
虽然 HTTP 缓存不是必须的，但重用缓存的资源通常是必要的。然而常见的 HTTP 缓存只能存储 GET 响应，对于其他类型的响应则无能为力。缓存的关键主要包括request method和目标URI（一般只有GET请求才会被缓存）。 普遍的缓存案例:

- 一个检索请求的成功响应: 对于 GET请求，响应状态码为：200，则表示为成功。一个包含例如HTML文档，图片，或者文件的响应。
- 永久重定向: 响应状态码：301。
- 错误响应: 响应状态码：404 的一个页面。
- 不完全的响应: 响应状态码 206，只返回局部的信息。
- 除了 GET 请求外，如果匹配到作为一个已被定义的cache键名的响应。

### 缓存控制

#### Cache-control 头
- 禁止进行缓存
```http
Cache-Control: no-store
```

- 强制确认缓存
```http
Cache-Control: no-cache
```

- 私有缓存和公共缓存
```http
Cache-Control: private
Cache-Control: public
```

- 缓存过期机制
```http
Cache-Control: max-age=31536000
```

- 缓存验证确认
```http
Cache-Control: must-revalidate
```

#### Pragma头

### 新鲜度

expirationTime = responseTime + freshnessLifetime - currentAge

#### 加速资源

### 缓存验证

#### ETags

### 带vary头的响应

---

## HTTP cookies

HTTP Cookie（也叫Web Cookie或浏览器Cookie）是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上。通常，它用于告知服务端两个请求是否来自同一浏览器，如保持用户的登录状态。Cookie使基于无状态的HTTP协议记录稳定的状态信息成为了可能。

Cookie主要用于以下三个方面：

- 会话状态管理（如用户登录状态、购物车、游戏分数或其它需要记录的信息）
- 个性化设置（如用户自定义设置、主题等）
- 浏览器行为跟踪（如跟踪分析用户行为等）

### 创建Cookie

服务器在响应头里面添加一个Set-Cookie选项。

#### Set-Cookie响应头部和Cookie请求头部

服务器发送该头部告知客户端保存cookie信息。
```http
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: yummy_cookie=choco
Set-Cookie: tasty_cookie=strawberry

[页面内容]
```

现在，对该服务器发起的每一次新请求，浏览器都会将之前保存的Cookie信息通过Cookie请求头部再发送给服务器。
```http
GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: yummy_cookie=choco; tasty_cookie=strawberry
```

#### 会话期Cookie
浏览器关闭后它会被自动删除。

#### 持久性Cookie
和关闭浏览器便失效的会话期Cookie不同，持久性Cookie可以指定一个特定的过期时间（Expires）或有效期（Max-Age）。
```http
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT;
```

#### Cookie中的Secure和HttpOnly标记
标记为 Secure 的Cookie只应通过被HTTPS协议加密过的请求发送给服务端。

为避免跨域脚本 (XSS) 攻击，通过JavaScript的 Document.cookie API无法访问带有 HttpOnly 标记的Cookie，它们只应该发送给服务端。如果包含服务端 Session 信息的 Cookie 不想被客户端 JavaScript 脚本调用，那么就应该为其设置 HttpOnly 标记。

```http
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT; Secure; HttpOnly
```

#### Cookie的作用域
Domain 和 Path 标识定义了Cookie的作用域：即Cookie应该发送给哪些URL。

#### SameSite Cookies

#### JavaScript通过Document.cookies访问Cookie
```JavaScript
document.cookie = "yummy_cookie=choco"; 
document.cookie = "tasty_cookie=strawberry"; 
console.log(document.cookie); 
// logs "yummy_cookie=choco; tasty_cookie=strawberry"
```

### 安全
**当机器处于不安全环境时，切记不能通过HTTP Cookie存储、传输敏感信息。**

#### 会话劫持和XSS

#### 跨站请求伪造CSRF

### 追踪和隐私

- 第三方Cookie
- 禁止追踪Do-Not-Track
- 欧盟Cookie指令
- 僵尸Cookie和删不掉的Cookie