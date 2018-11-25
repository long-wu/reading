第2章 简单的HTTP协议
=======================


## 1、HTTP协议的作用与HTTP协议相关内容
### HTTP协议用于客户端和服务器之间的通信
	HTTP协议和TCP/IP协议族内的其他众多协议相同，用于客户端和服务器之间的通信
	HTTP协议是通过请求和响应的交换达成通信

### 请求和响应简介
	请求由客户端发出，然后由服务器回复响应
	请求访问文本或图像等资源的一端被称为客户端，而提供资源响应的一端成为服务器端
	HTTP协议规定请求从客户端发出，最后服务器端应该请求并返回响应
	示例:
		发送请求(客户端):
			GET /index.html HTTP/1.1 			
			Host: hacker.jp
		发送响应(服务器):
			HTTP/1.1 200 OK
			Date: Tue, 10 Jul 2012 05:23:23 GMT
			Content-Length: 333
			Content-Type: text/html
			...

### HTTP不保存状态
* HTTP是不保存状态即无状态的协议(自身不对请求和响应之前的通信状态进行保存)
* 自身不具备保存之前发送过的请求或响应的功能
* 虽然HTTP协议是无状态的协议，但为了实现期望的保持状态的功能，引入了COOKIE技术


## 2、HTTP协议请求报文详解
### 请求报文组成
	请求报文由请求方法、请求URL、协议版本、可选的首部字段和内容实体构成

### 请求报文详解
	GET /index.html HTTP/1.1 			
	Host: hacker.jp
	Connection: keep-alive
	Content-Type: application/x-www-form-urlencoded
	Content-Length: 33
		
	name=wyb&age=21
	--> GET: 请求类型(method) /index.html: URL, 指明访问的对象 HTTP1.1: HTTP版本号
	--> Host到Content-Length: 请求首部字段
	--> name一行: 请求的内容实体

### HTTP方法(请求类型、method)的类型
* GET: 获取资源
* POST: 传输实体主体
* PUT: 传输文件
* HEAD: 获得报文首部
* DELETE: 删除资源
* OPTIONS: 询问支持的方法
* TRACE: 追踪路径
* CONNECT: 要求用隧道协议连接代理
* 以上请求方法中常用的类型: GET POST PUT DELETE

### 请求URI定位资源
	HTTP协议使用URI来定位互联网上的资源，因为URI的特定功能，在互联网上任意位置的资源都能被访问到
	当客户端请求访问资源而发送请求时，URI需要将作为请求报文中的请求URL包含在内


## 3、HTTP协议响应报文详解
### 响应报文组成
	响应报文由协议版本、状态码(表示请求成功或失败的数字代码)、用以解释状态码的原因短语、可选的首部字段以及实体主体构成构成

### 响应报文详解
	HTTP/1.1 200 OK
	Date: Tue, 10 Jul 2012 05:23:23 GMT
	Content-Length: 333
	Content-Type: text/html
		
	<html>
		...
	</html>
	--> HTTP1.1: HTTP版本号 200 OK: 请求的处理结果的状态码和原因短语
	--> Date到Content-Type: 响应首部字段
	--> <html>到</html>: 响应主体内容


## 4、HTTP协议实现持久连接节省通信量
### 为什么要持久连接
	HTTP协议的初始版本中，每进行一次HTTP通信就要断开一次TCP连接
	以当年的通信情况来看，都是些很小的文件传输，所以即使这样也没什么大不了的
	但是随着HTTP的普及，文档中包含大量图片的情况多了起来
	比如，请求一个包含多张图片的HTML页面时，在请求HTML的同时也会去请求这些图片
	每次的请求都会造成无谓的TCP连接建立和断开，增加通信量的开销

### 什么是持久连接
	为了解决上述的问题 HTTP1.1和部分HTTP1.0提出了keep-alive技术，这项技术的特点是只要任意一方没有明确提出断开
	连接，则保持TCP连接状态

## 5、Cookie技术
### Cookie技术实现
	Cookie技术通过在请求和响应报文中写入Cookie信息来控制客户端的状态
	Cookie会根据从服务器端发送的响应报文内的一个叫Set-Cookie的首部字段信息，通知客户端保存Cookie，
	当下一次客户端再往该服务器发送请求时，客户端会自动在请求报文中加入Cookie值然后发出去。
	服务端接收到客户端发送过来的cookie后会检查究竟是从哪一个客户端发来的连接请求，然后
	对比服务器上的记录，最后得到之前的状态信息