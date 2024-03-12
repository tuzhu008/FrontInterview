# gzip 的原理是什么，如何配置

# 可以对图片开启 gzip 压缩吗，为什么

http 的请求报文与响应报文的格式是什么

http 响应头中的 ETag 值是如何生成的

如果 http 响应头中 ETag 值改变了，是否意味着文件内容一定已经更改

http 服务中静态文件的 Last-Modified 是根据什么生成的

既然 http 是无状态协议，那它是如何保持登录状态


https 是如何保证报文安全的

我们如何从 http 的报文中得知该服务使用的技术栈

在发送 http 请求报文时，Host 是必要的吗

localhost:3000 与 localhost:5000 的 cookie 信息是否共享


http 响应头中如果 content-type 为 application/octet-stream，则代表什么意思
http 向 https 做重定向应该使用哪个状态码
http 响应头中的 Date 与 Last-Modified 有什么不同，网站部署时需要注意什么
http 1.1 中的 keep-alive 有什么作用
当在浏览器中看到某资源使用了 http2 后，使用 curl 为什么看到的仍是 http 1.1
https 中如何保证证书是可信任的
http2 中的首部压缩的实现原理是什么
http 请求头中的 X-Forwarded-For 代表什么意思

no-cache 与 no-store 的区别是什么

什么是跨域，如何解决跨域问题

主域名的 SEO 是否比二级域名要更好

有没有用过 continuous local storage，用在了哪里

cookie 有哪些字段

图片防盗链原理是什么

fetch 中 credentials 指什么意思，可以取什么值

当 cookie 没有设置 maxage 时，cookie 会存在多久

在浏览器中如何获取剪切板中内容

关于 cors 的响应头有哪些

## 浏览器 Tab 间通信

**同源Tab**

* BroadCast Channel
* localstorage
* Service Worker
* Shared Worker
* IndexedDB
* window.open + window.opener

**不同源Tab**

* 通过同源的 iframe 作为桥接层
