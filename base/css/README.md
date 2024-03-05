# CSS/HTML

* 如何使用 css 写一个魔方
* 如何使用 css 写一个有 3D 效果的立方体
* 有没有使用过 css variable，它解决了哪些问题
* 谈谈你对 styled-component 的看法
* 使用 CSS 如何画一个三角形
* display: inline 的元素设置 margin 和 padding 会生效吗
* html 的默认 display 属性是多少
* 响应式布局需要注意哪一些
* 对一个非定长宽的块状元素如何做垂直水平居中
* 如何实现左右固定，中间自适应布局
* 如何实现表格单双行条纹样式
* 简述下 css specificity
* Node 中 require json 文件数据时，如何当文件更新时，重新 require
* 有哪些 css 属性不能展示动画效果
* css 动画与 js 动画哪个性能更好
* css 中属性选择器及类选择器的权重哪个高
* 为什么会发生样式抖动
* position: sticky 如何工作，适用于哪些场景
* 什么是层叠上下文 (stacking contect)，谈谈对它的理解
* 你用 css 实现过什么不错的效果
* 你做前端有多少时间花在写 css 上
* 伪类与伪元素有什么区别

## 对 Web 标准以及 W3C 的理解与认识？

Web 标准：它不是某一个标准，而是一系列标准的集合。网页主要由三部分组成：结构、表现、行为。对应的标准也分三方面，结构化标准语言主要包括 XHTML 和 XML，表现标准语言主要包括 CSS，行为标准语言主要包括对象模型、ECMAScript 等等。这些标准大部分由万维网联盟起草和发布，也有一些是其他标准组织制定的。比如 ECMA 的 ECMAScript 标准。

## DOM 操作——怎样添加、移动、移除、复制、创建和查找节点

* 创建节点

  ```js
  document.createElement() // 创建一个具体的元素
  document.createTextNode() // 创建一个文本节点
  document.createDocumentFragement() // 创建一个 DOM 片段
  ```

* 添加、移除、替换、插入、复制

  ```js
  ParentNode.append()
  ParentNode.appendChild()

  ParentNode.replaceChild()

  ParentNode.insertBefore()

  Node.cloneNode()
  ```

* 查找

  ```js
  element.getElementsByTagName()
  element.getElementsByClass()
  element.getElementById()

  element.querySelector()
  element.querySelectorAll()
  ```

## 如何实现浏览器多个标签页之间的通信？

localStorage（storage 事件）、cookie

## 线程与进程的区别

* 进程是资源分配的最小单位，线程是程序执行的最小单位
* 进程有自己的独立地址空间，没启动一个进程，系统就会为它分配地址空间，建立数据表来维护代码段、堆栈段和数据段，这种操作非常昂贵。而线程是共享进程中的数据的，使用相同的地址空间，因此 CPU 切换一个线程的花费远比进程小很多，同时创建一个线程的开销也比进程小很多
* 线程之间的通信更方便，统一进程下的线程共享全局变量、静态变量等数据，而进程之间的通信则需要以通信的方式（IPC）进行。不过如何处理好同步与互斥是便携多线程程序的难点。
* 但是多进程程序更健壮，多线程程序只需要有一个线程死掉，整个进程也死掉了，而一个进程死掉并不会对另一个进程造成影响，因为进程有自己独立的地址空间。
* 从逻辑上来说，多线程的意义在于一个应用程序中，有多个执行部分可以同时执行。但是操作系统并没有将多个线程看作多个独立的应用，来实现进程调度和管理以及资源分配。这就是进程与线程的重要区别。

## 如何对网站的文件和资源进行优化？

* 文件合并，减少 http 请求次数
* 压缩：文件压缩、响应压缩
* CDN 托管，降低通信距离
* 缓存
* CSS 放最上面，script 放最上面
* 避免在 CSS 使用表达式
* 结构、样式、行为分离
* 减少 DNS 查询
* 避免重定向
* 正确选用图片格式

## 你都使用哪些工具来测试代码的性能？

* Profile
* JSPerf
* Dromaeo

没做过

## 什么是 FOUC（无样式内容闪烁）？如何来避免？

FOUC：Flash Of Unstyled Content 无样式内容闪烁

引起该问题的罪魁祸首就是 `@import`，它会在文档加载完成后再去加载，将其替换为 `link` 标签

## document.write 和 innerHTML 的区别？

document.write 只能重绘整个页面，innerHTML 可以重绘页面的一部分

## 什么叫优雅降级和渐进增强？

* 优雅降级：Web 站点在所有心事浏览器中都能正常工作，如果用户使用的是老式浏览器，则代码会检查确认它们是否能正常工作。由于 IE 独特的盒模型布局问题，针对不同版本的 IE 的 hack 时间过优雅降级了，为那么无法支持新功能的浏览器增加候选方案，使之在旧式浏览器上以某种形式降级体验却不至于完全无效。

* 渐进增强：从被所有浏览器支持的基本功能开始，逐步地添加哪些只有新式浏览器才支持的功能，向页面增加无害于基础浏览器的额外样式和功能。当浏览器支持时，他们会自动地呈现出来并发挥作用。

## 性能优化方法？

* 减少 http 请求数量和数据量：压缩代码、合并代码、合并图片、合理选用图片格式、网页 Gzip 压缩、 Ajax 数据缓存
* CDN、图片服务器
* 减少 DOM 操作次数：innerHTML 代替 DOM 操作
* 尽量使用 className 而不是 style
* 优化 js 性能
* 减少全局变量，缓存 DOM 节点查找结果
* 避免使用 CSS Expression
* css 网顶部，脚本放底部

## 一个页面从输入 URL 到页面加载显示完成，这个过程中都发生了什么？

* 当发送一个 URL 请求时，浏览器会开启一个线程来处理这个请求。
* DNS 解析，获取到资源目标主机的 IP 地址
* 三次握手建立 TCP/IP 链接
* 一旦 TCP/IP 连接建立，浏览器会通过该连接向远程服务器发送 HTTP 请求。
* 远程服务器找到资源并使用 HTTP 响应返回资源
* 此时，Web 服务器提供资源服务，客户端开始下载资源
* 浏览器解析返回的 HTML 文档，生成一个 DOM 树，根据下载的 CSS 生成 CSS 规则树，然后浏览器经过计算将 DOM 树和 CSS 树以一定规则生成 render 树。DOM 树和 CSS 树是不会相互阻塞的。

## 如何进行页面重构？

页面重构的概念：根据原有内容和结构的基础上，通过 div + css 写出更满足产品需求，更符合 web 标准的页面结构。

要解决的问题：

* 业务需求，设计还原度
* SEO
* 性能
* 可维护性
* 向前兼容

重构涉及内容：

* 设计稿的重构：设计师的设计稿可能不是特别符合页面效果，当拿到设计稿时需要通过二次重构和修改达到预期效果。
* 功能不全页面的重构：页面功能不符合用户体验、用户交互。
* 过时页面的重构：使用的是过时的代码和标签，跟不上时代的发展。
* 代码重构：代码质量、SEO优化、页面性能、更好的语义化、浏览器兼容、CSS优化。

## web 应用从服务器主动推送 Data 到客户端的方式有哪些？

* html5 websocket
* XHR 长连接
* XHR Mutilpart Streaming
* iframe
* Script 长连接

## 事件、IE 与火狐的事件机制有什么区别？如何阻止冒泡？

事件：用户在网页元素进行的操作，导致发生的事情。  
处理机制：IE 是事件冒泡，firefox 同事支持冒泡和捕获。

阻止冒泡：

* `e.stopPropagation()`
* `e.cancelBubble = true;`

## 如何获取 UA？

使用 `navigator` 对象

## 浏览器页面有那三层结构，分别是什么，作用是什么？

三层结构：结构层、表示层、行为层

分别是：HTML、CSS、javaScript

作用：HTML 实现页面结构，CSS 完成页面的表现与风格，JavaScript 实现一些客户端的功能与业务

**缺点：**

* 该标准并未能很好的被浏览器所支持。


## 如何实现一个原型的可点击区域？

* map + area、SVG
* `border-radius`
* 纯 js 实现，根据点击点的坐标来计算

## 要动态改变层中内容可使用的方法？

`innerHTML`，`innerText`

## 一个图片 url 访问后直接下载怎样实现？

请求的返回头里面，用于浏览器解析的虫咬参数就是 OSS 的 API 文档里面返回的 http 头，决定用户下载行为的参数。

下载的情况：

```
x-oss-object-type: Normal
x-oss-request-id: 1ejdhdusydahd
x-oss-storage-class: Standard
```

## web Quality (无障碍)

能够被残障人士使用的网站才能称得上一个易用的（易访问的）网站。

残障人士指的是那些带有残疾或者身体不健康的用户。

## fetch 发送 2次请求的原因？

fetch 发送 post 请求的时候，总是发送两次，第一次状态是204，第二次才成功。

原因很简单，因为用 fetch 的 post 请求的时候导致 fetch 第一次发送一个 Options 请求，询问服务器是否支持修改的请求头，如果服务器支持，则在第二次中发送真正的请求。

## 说一下 web woker

在 HTML 页面中，如果在执行脚本时，页面的状态是不可响应的，知道脚本执行完成后，页面才变成可响应的。因为 js 是单线程的，执行会阻塞后面代码的执行。

通过使用 Web Workers，Web应用程序可以在独立于主线程的后台线程中，运行一个脚本操作。这样做的好处是可以在独立线程中执行费时的处理任务，从而允许主线程（通常是UI线程）不会因此被阻塞/放慢。

主线程和 worker 线程之间通过 `postMessage` 传递消息。

## 讲讲 viewport 和移动端布局？

TODO:

## 怎么看网站的性能如何？

检测页面加载时间一般有两种方式，一种是被动去测：就是在被检测的页面植入脚本或探针，当影虎访问页面时，探针自动采集数据并传回数据库进行分析，另一种主动监测的方式，即主动的搭建分布式受控环境，模拟用户发起页面访问请求，主动采集性能数据并分析，在检测的精准度上，专业的第三方工具效果更加，比如性能极客。

## 事件代理在捕获阶段的实际应用？

可以在父元素层面阻止时间向子元素传播，可以代替子元素执行某些操作。

## 事件模型是什么，DOM0 和 DOM2 级有什么区别，DOM 的分级是什么？

// TODO:

## flex 和 grid 的区别

flex 是一维布局系统，适合做局部布局，比如导航栏组件。

grid 是二维布局系统，通常用于整个页面的规划。

## 移动端 meta viewport=device

设置视口的宽度为设备的宽度，并可以指定初始的缩放比例，以及是否允许用户缩放等。


```html
<meta
  name="viewport"
  content="
    width=device-width,  // 设置viewport的宽等于屏幕宽
    initial-scale=1.0,  // 初始缩放为1
    maximum-scale=1.0,
    user-scalable=no,  // 不允许用户手动缩放
    viewport-fit=cover // 缩放以填充满屏幕
    "
>
```

## 移动端 1px

| 方案 | 优点 | 缺点 |
|:---- | :---- | :---- |
| 直接写0.5px | 代码简单 | IOS及Android老设备不支持 |
| 用图片代替边框 | 全机型兼容 | 修改颜色及不支持圆角 |
| background渐变 | 全机型兼容 | 代码多及不支持圆角 |
| box-shadow模拟边框实现 |  全机型兼容 | 有边框和虚影无法实现 |
| 伪元素先放大后缩小 | 简单实用  | 缺点不明显 |
| 设置viewport解决问题  | 一套代码适用所有页面 | 缺点不明显 |

## 参考文档

[MDN: HTML 元素参考](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element)


https://tsejx.github.io/css-guidebook/layout/centered/center-horizontally/