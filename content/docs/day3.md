---
title: HTTP和HTTPS协议
categories:
  - web
  
abbrlink: 2701
date: 2023.11.20
tags: 
   - web

---

# http协议与https协议

## http简介

1.HTTP 协议是 Hyper Text Transfer Protocol（超文本传输协议）的缩写，是用于从万维网（ WWW:World Wide Web ）服务器传输超文本到本地浏览器的传送协议.

2.http协议是基于tcp/ip协议来传输数据（html文件、图片文件、查询结果等）

3.http的url是由http://起始，默认使用端口为80

## http工作原理

1.http协议工作于客户端-服务器架构上

2.浏览器作为http客户端通过url向http服务端即web服务器发送所有的请求。

3.web服务器接受到请求后，向客户端发送响应信息

4.http的默认端口号为80，但可以改为其他的端口

![cgiarch](https://www.runoob.com/wp-content/uploads/2013/11/cgiarch.gif)

http协议的工作原理示意图

# https简介

https经由http进行通信，但是利用ssl/tls来加密数据包，https的开发的主要目的是提供对网站服务器的身份认证，保护交换资料的隐私和完整性。

https的url是由https://起始，默认端口为443

## https的作用

信任的情况：

与一个网站之间的 HTTPS 连线仅在这些情况下可被信任：

- 浏览器正确地实现了 HTTPS 且操作系统中安装了正确且受信任的证书颁发机构；
- 证书颁发机构仅信任合法的网站；
- 被访问的网站提供了一个有效的证书，也就是说它是一个由操作系统信任的证书颁发机构签发的（大部分浏览器会对无效的证书发出警告）；（可以人为的添加证书，如bp的使用则需要人为的给浏览器添加证书）
- 该证书正确地验证了被访问的网站（例如，访问 [https://www.runoob.com](https://www.runoob.com/) 时收到了签发给 www.runoob.com 而不是其它域名的证书）；
- 此协议的加密层（SSL/TLS）能够有效地提供认证和高强度的加密。
