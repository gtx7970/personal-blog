---
title: "Http"
metaTitle: "This is the title tag of this page"
metaDescription: "This is the vue description"
---


### 状态码

```js
```

### 缓存

1. 按缓存位置
内存缓存
硬盘缓存
service worker

2. 按缓存策略

强缓存
expire
cache-control

协商缓存
etag （if-none-match）
      (etag)
last-modify (last-modified)
            (if-modified-since)

```js
Cache-Control: no-cache, no-store, must-revalidate
```

### HTTP 1.0
TCP连接无法复用, 每次请求都需要重新建立TCP通道， 这就需要重复进行三次握手和四次挥手

队头阻塞

### HTTP 1.1
长连接(默认开启，Connection: keep-alive) 一个TCP连接上可以传送多个HTTP请求和响应

可以并行发出请求，但是对请求的响应仍要遵循先进先出的原则，第一个请求处理的结果返回后，第二个请求才会得到响应

没有真正解决队头阻塞


### HTTP 2

请求/响应复用： 完全双向的请求和响应复用

报头压缩

服务端推送
