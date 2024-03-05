# Node 基础

* 在 Node 应用中如何利用多核心CPU的优势
* Node 中 cluster 的原理是什么
* Node 中如何判断一个路径是文件还是文件夹
* Node 如何进行进程间通信
* 在 node 中如何监听异步资源的生命周期
* node --max-old-space-size=4096 是什么意思
* 如何理解 Node 的异步非阻塞IO
* 当 Node 应用发生 gc 时，如何监控
* Node 应用中如何查看 gc 的日志
* 如何获取你们 Node 项目的 cpu profile 快照
* Node 中循环引用会发生什么
* Node 中 require 时发生了什么
* 简述 node/v8 中的垃圾回收机制
* node 中 exec，fork 与 spawn 有何区别
* node 中 dns.resolve 及 dns.lookup 有什么区别
* node 中如何查看函数异步调用栈
* 有没有接触过 fs-extra，它是解决什么问题的
* 在 node 中如何开启 https
* node 中 module.exports 与 exports 有什么区别

## 如何判断当前脚本运行在浏览器还是 node 环境中？

`global === window`

## 对 Node 的优点和缺点提出自己的看法？

优点：

* 由于 Node 是基于事件驱动和无阻塞的，所以非常适合处理并发请求，因此构建在 Node 上的代理服务器相比起亚技术实现的服务器表现要好得多
* 与 Node 代理服务器交互的客户端代码是由 JavaScript 语言编写的，因为客户端和服务器端都用同一种语言编写。

缺点：

* 新东西，稳定性有待提供，资料不完整
* 缺少足够的三方库支持

## Node.js 的适用场景？

### 特点

* 它是一个Javascript运行环境
* 依赖于Chrome V8引擎进行代码解释
* 事件驱动
* 非阻塞I/O
* 轻量、可伸缩，适于实时数据交互应用
* 单进程，单线程

### 解决问题

* 并发连接
* I/O阻塞

### NodeJS的优缺点

**优点**

* 高并发（最重要的优点）
* 适合I/O密集型应用

**缺点**

* 单线程，不适合CPU密集型应用，不能充分利用CPU
* 可靠性低，一旦代码某个环节崩溃，整个系统都崩溃

  解决方案：

  * Nginx反向代理，负载均衡，开多个进程，绑定多个端口；
  * 开多个进程监听同一个端口，使用 cluster 模块；

* 开源组件库质量参差不齐，更新快，向下不兼容
* Debug不方便，错误没有stack trace

### 适合NodeJS的场景

* 高并发：RESTful API、大量Ajax请求的应用
* 统一Web应用的UI层
* 聊天、实时推送

## Node 事件方法





