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

[https://www.runoob.com/http/http-status-codes.html](https://www.runoob.com/http/http-status-codes.html)

## Http协议由什么组成?

请求报文包括三部分:

* 请求行:包含请求方法,URI,HTTP版本协议

* 请求首部字段

* 请求内容实体

响应报文包含三部分:

* 状态行:包含HTTP版本,状态码,状态码原因短语

* 响应首部字段

* 响应内容实体

https://www.jianshu.com/p/9fa58c432e08

## http2 和 http 有什么区别？

## cache-control

## Http 方法

| 方法 | 描述 |
| :--- | :--- |
| GET | 用于请求访问已经被 URI 识别的资源 |
| POST | 用于传输信息给服务器 |
| PUT | 传输文件 |
| HEAD | 获得报文首部，与GET方法类似，只是不返回报文主体，一般用于验证URI是否有效。 |
| DELETE | 删除文件，与PUT方法相反，删除对应URI位置的文件 |
| OPTIONS | 查询相应URI支持的HTTP方法 |

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

## 301 和 302 的区别?

301 Moved Permanently 被请求的资源已永久移动到新位置，并且将来任何对此资源的引用都应该使用本响应返回的若干个 URI 之一。如果可能，拥有链接编辑功能的客户端应当自动把请求的地址修改为从服务器反馈回来的地址。除非额外指定，否则这个响应也是可缓存的。

302 Found 请求的资源现在临时从不同的 URI 响应请求。由于这样的重定向是临时的，客户端应当继续向原有地址发送以后的请求。只有在 Cache-Control 或 Expired 中指定的情况下，这个响应才是可缓存的。字面上区别就是 301 是永久重定向，而 302 是临时重定向。

301 比较常用的场景是使用域名跳转。 302 用来做临时跳转，比如未登录的用户访问用户中心重定向到登录页。

## 304 和 200

200：请求已成功，请求所希望的响应头或数据体将随此响应返回。即返回的数据为全量的数据，如果文件不通过过 GZip 压缩的话，文件是多大，则需要多大的传输量。

304：如果客户端发送一个带条件的 GET 的请求且该请求已被允许，而文档的内容（自上次访问以来或根据请求条件）并没有改变，则服务器应当返回这个状态码。即客户端和服务器端只需要传输少量的数据来做文件的校验，如果文件没有修改过，则不需要返回全量的数据。

## 首部

a、通用首部字段（请求报文与响应报文都会使用的首部字段）

Date：创建报文时间
Connection：连接的管理
Cache-Control：缓存的控制
Transfer-Encoding：报文主体的传输编码方式

b、请求首部字段（请求报文会使用的首部字段）
Host：请求资源所在服务器
Accept：可处理的媒体类型
Accept-Charset：可接收的字符集
Accept-Encoding：可接受的内容编码
Accept-Language：可接受的自然语言

c、响应首部字段（响应报文会使用的首部字段）
Accept-Ranges：可接受的字节范围
Location：令客户端重新定向到的URI
Server：HTTP服务器的安装信息

d、实体首部字段（请求报文与响应报文的的实体部分使用的首部字段）
Allow：资源可支持的HTTP方法
Content-Type：实体主类的类型
Content-Encoding：实体主体适用的编码方式
Content-Language：实体主体的自然语言
Content-Length：实体主体的的字节数
Content-Range：实体主体的位置范围，一般用于发出部分请求时使用


与HTTP 1.1相比，主要区别包括
HTTP/2采用二进制格式而非文本格式
HTTP/2是完全多路复用的，而非有序并阻塞的——只需一个连接即可实现并行
使用报头压缩，HTTP/2降低了开销
HTTP/2让服务器可以将响应主动“推送”到客户端缓存中