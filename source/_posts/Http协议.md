---
title: Http协议
date: 2019-08-30 18:20:15
tags: http
categories: 学习笔记
---
### what is
- HTTP是Hyper Text Transfer Protocol（超文本传输协议）的缩写,是用于从万维网服务器传输超文本到本地浏览器的传送协议；
- HTTP是一个基于TCP/IP通信协议来传递数据（HTML 文件, 图片文件, 查询结果等）；
- 连接时需要3次握手、断开时需要4次挥手；

### 工作原理
- HTTP是无连接：
	无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间；
- HTTP是媒体独立的：
	只要客户端和服务器知道如何处理的数据内容，任何类型的数据都可以通过HTTP发送。客户端以及服务器指定使用适合的MIME-type内容类型；
- HTTP是无状态：
	无状态是指协议对于事务处理没有记忆能力；
	缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大；
	在服务器不需要先前信息时它的应答就较快；
 
### 消息结构
- Http消息包括请求消息和响应消息两部分；
- HTTP请求报文由请求行（request line）、请求头（header）、空行和请求数据4个部分组成；
	- 请求行由请求方法字段、URL字段和HTTP协议版本字段3个字段组成；
		HTTP1.0定义了三种请求方法： GET, POST 和 HEAD方法；
		HTTP1.1新增了五种请求方法：OPTIONS, PUT, DELETE, TRACE 和 CONNECT 方法；
	- 请求头部由关键字/值对组成，每行一对，关键字和值用英文冒号“:”分隔；
	- 请求数据POST方法中使用。POST方法适用于需要客户填写表单的场合。与请求数据相关的最常使用的请求头是Content-Type和Content-Length；
- HTTP响应也由三个部分组成，分别是：状态行、响应头、空行、响应正文；
	- 在响应中唯一真正的区别在于第一行中用状态信息代替了请求信息，状态行通过提供一个状态码来说明所请求的资源情况。
		- 状态代码由三位数字组成，第一个数字定义了响应的类别；
			1xx：指示信息--表示请求已接收，继续处理。
			2xx：成功--表示请求已被成功接收、理解、接受。
			3xx：重定向--要完成请求必须进行更进一步的操作。
			4xx：客户端错误--请求有语法错误或请求无法实现。
			5xx：服务器端错误--服务器未能实现合法的请求。
		- 常见的状态码：
			200 OK：客户端请求成功。
			400 Bad Request：客户端请求有语法错误，不能被服务器所理解。
			401 Unauthorized：请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用。
			403 Forbidden：服务器收到请求，但是拒绝提供服务。
			404 Not Found：请求资源不存在，举个例子：输入了错误的URL。
			500 Internal Server Error：服务器发生不可预期的错误。
			503 Server Unavailable：服务器当前不能处理客户端的请求，一段时间后可能恢复正常，举个例子：HTTP/1.1 200 OK（CRLF）。
#### 通用头部
- Request URL:请求的URL地址；
- Request Method: 请求方法，get/post/put/delete……；
- Status Code：状态码，200 为请求成功；
- Remote Address：路由地址；
#### 请求头部
- Accept：告诉WEB服务器自己接受什么介质类型，*/* 表示任何类型，type/* 表示该类型下的所有子类型；
- Accept-Charset：浏览器申明自己接收的字符集；
- Accept-Encoding：浏览器申明自己接收的编码方法，通常指定压缩方法，是否支持压缩，支持什么压缩方法 （gzip，deflate）；
- Accept-Language：浏览器申明自己接收的语言。语言跟字符集的区别：中文是语言，中文有多种字符集，比如big5，gb2312，gbk等等；
- Authorization：当客户端接收到来自WEB服务器的 WWW-Authenticate 响应时，该头部来回应自己的身份验证信息给WEB服务器；
- Connection：表示是否需要持久连接（close、keep-alive）；
- Referer：发送请求页面URL。浏览器向 WEB 服务器表明自己是从哪个 网页/URL 获得/点击 当前请求中的网址/URL；
- User-Agent: 浏览器表明自己的身份（是哪种浏览器）；
- Host： 发送请求页面所在域；
- Cache-Control：浏览器应遵循的缓存机制：
	no-cache（不要缓存的实体，要求现在从WEB服务器去取）；
	max-age：（只接受 Age 值小于 max-age 值，并且没有过期的对象）； 
	max-stale：（可以接受过去的对象，但是过期时间必须小于 max-stale 值）；  
	min-fresh：（接受其新鲜生命期大于其当前 Age 跟 min-fresh 值之和的缓存对象）；
- Pramga：主要使用 Pramga: no-cache，相当于 Cache-Control： no-cache；
- Range：浏览器（比如 Flashget 多线程下载时）告诉 WEB 服务器自己想取对象的哪部分；
- Form：一种请求头标，给定控制用户代理的人工用户的电子邮件地址；
- Cookie：这是最重要的请求头信息之一；

#### 响应头部
- Age：当代理服务器用自己缓存的实体去响应请求时，用该头部表明该实体从产生到现在经过多长时间了。
- Accept-Ranges：WEB服务器表明自己是否接受获取其某个实体的一部分（比如文件的一部分）的请求。bytes：表示接受，none：表示不接受。
-Cache-Control：服务器应遵循的缓存机制。
	public(可以用 Cached 内容回应任何用户)
	private（只能用缓存内容回应先前请求该内容的那个用户）
	no-cache（可以缓存，但是只有在跟WEB服务器验证了其有效后，才能返回给客户端） 
	max-age：（本响应包含的对象的过期时间）  
	ALL:  no-store（不允许缓存）  
- Connection： 是否需要持久连接
	close（连接已经关闭）。
	keepalive（连接保持着，在等待本次连接的后续请求）。
	Keep-Alive：如果浏览器请求保持连接，则该头部表明希望 WEB 服务器保持连接多长时间（秒）。例如：Keep-Alive：300
- Content-Encoding：WEB服务器表明自己使用了什么压缩方法（gzip，deflate）压缩响应中的对象。 例如：Content-Encoding：gzip 
- Content-Language：WEB 服务器告诉浏览器自己响应的对象的语言。
- Content-Length：WEB 服务器告诉浏览器自己响应的对象的长度。例如：Content-Length: 26012
- Content-Range：WEB 服务器表明该响应包含的部分对象为整个对象的哪个部分。例如：Content-Range: bytes 21010-47021/47022
- Content-Type：WEB 服务器告诉浏览器自己响应的对象的类型。例如：Content-Type：application/xml
- Expired：WEB服务器表明该实体将在什么时候过期，对于过期了的对象，只有在跟WEB服务器验证了其有效性后，才能用来响应客户请求。
- Last-Modified：WEB 服务器认为对象的最后修改时间，比如文件的最后修改时间，动态页面的最后产生时间等等。
- Location：WEB 服务器告诉浏览器，试图访问的对象已经被移到别的位置了，到该头部指定的位置去取。
- Proxy-Authenticate： 代理服务器响应浏览器，要求其提供代理身份验证信息。
- Server: WEB 服务器表明自己是什么软件及版本等信息。
- Refresh：表示浏览器应该在多少时间之后刷新文档，以秒计。

### 附录
{% asset_link HTTP协议.pdf %}
