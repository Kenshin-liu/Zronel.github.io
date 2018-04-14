---
layout: post
title: http协议
date: 2017/01/10
---

之前在校招中经常会被问道http协议有关的内容，在前端工作中http协议也是很重要的一项技能。
本文介绍一些http协议中常用的一些知识。倘若很是感兴趣的话可以自行阅读《http协议详解》一书。

## 概念

http协议：超文本传输协议(Hypertext transfer protocol)。是一种详细规定了浏览器和万维网(WWW = World Wide Web)服务器之间互相通信的规则，通过因特网传送万维网文档的数据传送协议。

HTTP是一个应用层协议，由请求和响应构成，是一个标准的客户端服务器模型。HTTP是一个无状态的协议。

HTTP默认的端口号为80，HTTPS的端口号为443。

![image](http://ojg2nfova.bkt.clouddn.com/111703047392802.png)

## http请求

http请求由三部分组成，分别是：请求行、消息报头、请求正文
请求行以一个方法符号开头，以空格分开，后面跟着请求的URI和协议的版本。
格式如下：Method Request-URI HTTP-Version CRLF
其中 Method表示请求方法；Request-URI是一个统一资源标识符；HTTP-Version表示请求的HTTP协议版本；CRLF表示回车和换行（除了作为结尾的CRLF外，不允许出现单独的CR或LF字符）
请求方法有：

| 方法    | 作用                    |
| ------------- |:-------------:| 
| GET    | 请求获取Request-URI所标识的资源|
| POST   | 在Request-URI所标识的资源后附加新的数据|
| HEAD   | 请求获取由Request-URI所标识的资源的响应消息报头|
| PUT    | 请求服务器存储一个资源，并用Request-URI作为其标识|
| DELETE | 请求服务器删除Request-URI所标识的资源|
| TRACE  | 请求服务器回送收到的请求信息，主要用于测试或诊断|
| OPTIONS| 请求查询服务器的性能，或者查询与资源相关的选项和需求|

一个简单的get请求例子：<br>Request URL:https://clients5.google.com/pagead/drt/dn/dn.js<br>
Request Method:GET<br>
Status Code:200<br>
Remote Address:61.91.161.217:443<br>
age:42232<br>
alt-svc:quic=":443"; ma=2592000; v="35,34"<br>
cache-control:public, max-age=86400<br>
content-encoding:gzip<br>
content-length:11504<br>
content-type:text/javascript<br>
date:Mon, 09 Jan 2017 14:05:23 GMT<br>
expires:Tue, 10 Jan 2017 14:05:23 GMT<br>
last-modified:Thu, 08 Dec 2016 01:00:57 GMT<br>
server:sffe<br>
status:200<br>
vary:Accept-Encoding<br>
x-content-type-options:nosniff<br>
x-xss-protection:1; mode=block<br>

常用的请求报头
常用的请求报头
*Accept*<br>
Accept请求报头域用于指定客户端接受哪些类型的信息。eg：Accept：image/gif，表明客户端希望接受GIF图象格式的资源；Accept：text/html，表明客户端希望接受html文本。

*Accept-Charset*<br>
Accept-Charset请求报头域用于指定客户端接受的字符集。eg：Accept-Charset:iso-8859-1,gb2312.如果在请求消息中没有设置这个域，缺省是任何字符集都可以接受。

*Accept-Encoding*<br>
Accept-Encoding请求报头域类似于Accept，但是它是用于指定可接受的内容编码。eg：Accept-Encoding:gzip.deflate.如果请求消息中没有设置这个域服务器假定客户端对各种内容编码都可以接受。

*Accept-Language*<br>
Accept-Language请求报头域类似于Accept，但是它是用于指定一种自然语言。eg：Accept-Language:zh-cn.如果请求消息中没有设置这个报头域，服务器假定客户端对各种语言都可以接受。

*Authorization*<br>
Authorization请求报头域主要用于证明客户端有权查看某个资源。当浏览器访问一个页面时，如果收到服务器的响应代码为401（未授权），可以发送一个包含Authorization请求报头域的请求，要求服务器对其进行验证。

*Host*<br>
Host请求报头域主要用于指定被请求资源的Internet主机和端口号。

*User-Agent*<br>
这个请求头不是必须的，保存的是一些用户浏览器以及操作系统等信息。

## http响应

在接收和解释请求消息后，服务器返回一个HTTP响应消息。

HTTP响应也是由三个部分组成，分别是：状态行、消息报头、响应正文

状态行格式如下：
HTTP-Version Status-Code Reason-Phrase CRLF
其中，
*HTTP-Version表示服务器HTTP协议的版本；*
*Status-Code表示服务器发回的响应状态代码；*
*Reason-Phrase表示状态代码的文本描述。*

状态码一般有如下情况：
100  Continue   继续，一般在发送post请求时，已发送了http header之后服务端将返回此信息，表示确认，之后发送具体参数信息<br>
200  OK         正常返回信息<br>
201  Created    请求成功并且服务器创建了新的资源<br>
202  Accepted   服务器已接受请求，但尚未处理<br>

300  请求的资源可在多处得到<br>
301  Moved Permanently  请求的网页已永久移动到新位置。<br>
302 Found       临时性重定向。<br>
303 See Other   临时性重定向，且总是使用 GET 请求新的 URI。<br>
304  Not Modified 自从上次请求后，请求的网页未修改过。<br>

400 Bad Request  服务器无法理解请求的格式，客户端不应当尝试再次使用相同的内容发起请求。<br>
401 Unauthorized 请求未授权。<br>
403 Forbidden   禁止访问。<br>
404 Not Found   找不到如何与 URI 相匹配的资源。<br>

500 Internal Server Error  服务器遇到错误，无法完成请求。<br>
502 - 网关错误<br>
503 Service Unavailable 服务器端暂时无法处理请求（可能是过载或维护）。<br>


    
    
## 关于post与get的区别

 
1. GET提交，请求的数据会附在URL之后（就是把数据放置在HTTP协议头＜request-line＞中），以?分割URL和传输数据，多个参数用&连接;例如：login.action?name=hyddd&password=idontknow&verify=%E4%BD%A0 %E5%A5%BD。如果数据是英文字母/数字，原样发送，如果是空格，转换为+，如果是中文/其他字符，则直接把字符串用BASE64加密，得出如： %E4%BD%A0%E5%A5%BD，其中％XX中的XX为该符号以16进制表示的ASCII。

  POST提交：把提交的数据放置在是HTTP包的包体＜request-body＞中。上文示例中红色字体标明的就是实际的传输数据

  因此，GET提交的数据会在地址栏中显示出来，而POST提交，地址栏不会改变

 

2. 传输数据的大小：

   首先声明,HTTP协议没有对传输的数据大小进行限制，HTTP协议规范也没有对URL长度进行限制。 而在实际开发中存在的限制主要有：

   GET:特定浏览器和服务器对URL长度有限制，例如IE对URL长度的限制是2083字节(2K+35)。对于其他浏览器，如Netscape、FireFox等，理论上没有长度限制，其限制取决于操作系统的支持。

   因此对于GET提交时，传输数据就会受到URL长度的限制。

   POST:由于不是通过URL传值，理论上数据不受限。但实际各个WEB服务器会规定对post提交数据大小进行限制，Apache、IIS6都有各自的配置。

 

3. 安全性：

POST的安全性要比GET的安全性高。注意：这里所说的安全性和上面GET提到的“安全”不是同个概念。上面“安全”的含义仅仅是不作数据修改，而这里安全的含义是真正的Security的含义，比如：通过GET提交数据，用户名和密码将明文出现在URL上，因为(1)登录页面有可能被浏览器缓存， (2)其他人查看浏览器的历史纪录，那么别人就可以拿到你的账号和密码了，

