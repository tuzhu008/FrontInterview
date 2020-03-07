# Http

## Http  状态码

* 100 Continue  继续，一般在发送post请求时，已发送了http header之后服务端将返回此信息，表示确认，之后发送具体参数信息
* 200 OK   正常返回信息
* 201 Created  请求成功并且服务器创建了新的资源
* 202 Accepted  服务器已接受请求，但尚未处理
* 301 Moved Permanently  请求的网页已永久移动到新位置
* 302 Found  临时性重定向
* 303 See Other  临时性重定向，且总是使用 GET 请求新的 URI
* 304 Not Modified  自从上次请求后，请求的网页未修改过
* 400 Bad Request  服务器无法理解请求的格式，客户端不应当尝试再次使用相同的内容发起请求
* 401 Unauthorized  请求未授权
* 403 Forbidden  禁止访问
* 404 Not Found  找不到如何与 URI 相匹配的资源
* 500 Internal Server Error  最常见的服务器端错误
* 503 Service Unavailable 服务器端暂时无法处理请求（可能是过载或维护）

## http 状态码有哪些？分别代表什么意思？

* 1xx 信息性状态码，表示服务器接收到请求，正在处理
* 2xx 成功状态码，标示服务器正确处理完请求
* 3xx 重定向状态码，标示请求的资源位置发生改变，需要重新请求，301 永久重定向，302 临时重定向
* 4xx 客户端错误状态码，服务器无法处理该请求 404 not found
* 5xx 服务器内部错误状态码，服务器处理请求出错

## http2 和 http 有什么区别？

## cache-control

## Http 方法

| 方法 | 描述 |
| :--- | :--- |
|  |  |

TODO:

## Http2

## http 常用请求头

## 强缓存和协商缓存？

缓存分为两种：强缓存和协商缓存，根据响应的 header 内容来决定

| 类型 | 获取资源形式 | 状态码 | 发送请求到服务器 |
| :--- | :--- | :--- | :--- |
| 强缓存 | 从缓存获取 | 200（from cache\) | 否，直接从缓存读取 |
| 协商缓存 | 从缓存获取 | 304（not modified\) | 是，通过服务器来告知缓存是否可用 |



强缓存相关字段有：expired, cache-control。如果 cache-control 与 expired 同时存在的话，cache-control 优先级高于 expired

协商缓存相关字段有：Last-Modified/if-Modified-since, Etag/if-None-Match

