---
layout: post
title: "HTTP Cache"
categories: HTTP
---

## HTTP Cache
1. 缓存的目的

2. HTTP1.1缓存类型

    1. 过期模型(Expiration Model)

    2. 验证模型(Validation Model)
3. 什么响应可缓存

4.从缓存里构建响应

    hop-by-hop头域

* Connection
* Keep-Alive
* Transfer-Encoding
* Proxy-Authenticate
* Proxy-Authorization
* TE
* Trailers
* Upgrade


    不可更改的头域

- Contents-location
- Content-MD5
- ETag
- Last-Modified

    联合头域
* 304
* 206


    联合字节范围
* Range
