# JavaScript

## Cookie

HTTP 协议本身是无状态的。什么是无状态呢，即服务器无法判断用户身份。Cookie 实际上是一小段的 key-value 格式的文本信息。客户端向服务器发起请求，如果服务器需要记录该用户状态，就使用 `response` 向客户端浏览器颁发一个 Cookie。客户端浏览器会把 Cookie 保存起来。当浏览器再请求该网站时，浏览器把请求的网址连同该 Cookie 一同提交给服务器。服务器检查该 Cookie，以此来辨认用户状态。

优点：

* 数据持久性
* 不需要任何服务器资源，因为 Cookie 是存储在客户端并发送给服务器读取
* 可配置到期规则，控制 Cookie 的生命周期，使之不会永远有效，偷盗者可能拿到的是一个过期的 Cookie
* 简单性，基于文件的轻量结构
* 通过良好的编程，控制保存在 Cookie 中的 Session 对象的大小
* 通过加密和安全传输技术（ssl），减少 Cookie 被破解的可能性
* 只要cookie中不存放敏感的数据，即使被盗也不会有重大损失

缺点：

* Cookie 的数量和长度都有限制

  * IE6 及以下的版本最多20个

  * IE7 以后的可以有50个

  * Firefox 可以有50个

  * chrome 和 Safari 没有限制

  * 每个 cookie 的长度不超过4k，否则会被截掉

* 潜在的安全风险：

  * 明文存储，可修改，易被截取篡改，如果 cookie 被拦截，就可能会获取到所有的 Session 信息
  * 服务器如果未设置 `http-only` 时，客户端可以使用 JS 对其进行访问，进而可以修改。

* 受限于用户配置，有的用户禁用了浏览器或者客户端设备接受 cookie 的能力，因此限制了这一功能

* 使用场景有限：例如，有些状态不可能保存在客户端，为了防止重复提交表达，需要在服务器端保存一个计时器，如果把这个计时器保存在客户端，它将不起作用。

* 增加网络开销：由于每次都会随请求一起发送到服务器，一些不必要的信息也会一起发送，造成带宽的浪费。

## 本地存储

### Cookie

### localStorage

用于持久化的本地存储，除非主动删除数据，否则数据永远不会过期。

访问 localStorage 具有同源策略。

优点：

* 容量大，可以达到 5M
* 易用，强大，原生支持

缺点：

* 兼容性差，高级版本的浏览器才支持，这点其实可以忽略，目前业界支持最低的 ie8 已经支持

  ![](/assets/localStorage浏览器兼容性.png)

* 存储内容类型限制：只能为 String 类型

* 隐私模式下不可读取：可读写，但不可永远保存；正常模式和隐私模式是隔离的，隐私模式无法访问正常模式存储的东西；。

* 不能被爬虫提取

### sessionStorage

用于本地存储一个会话中的数据，这些数据只有在同一个会话中的页面才能访问，会话结束后数据随之销毁。sessionStorage 不是一种持久化的本地存储，仅仅是会话级别的存储。

访问 sessionStorage 具有同源策略。

### localStorage与sessionStroage区别

* localStroage 是将信息存储在硬件设备中的，关闭浏览器或网页也不会消失；
* sessionStroage 有效期只是网页在浏览器打开到关闭的时间段

### IndexedDB

IndexedDB 是一种低级 API，用于客户端存储大量结构化数据\(包括, 文件/ blobs\)。该 API 使用索引来实现对该数据的高性能搜索。

IndexedDB 是一个事务型数据库系统，是一个基于 JavaScript 的面向对象的数据库。IndexedDB 允许您存储和检索用键索引的对象；可以存储结构化克隆算法支持的任何对象。

优点：

* 存储内容类型多样
* 可以存储大量数据

缺点:

* 兼容性差，IE10

### WebSQL

WebSQL 是前端的一个独立模块，是 web 存储方式的一种。

WebSQL 是一个在浏览器客户端的结构关系数据库，这是浏览器内的本地RDBMS\(关系型数据库系统\)，你可以使用 SQL 查询

缺点：

* 兼容性差，当前只有谷歌支持，ie 和火狐均不支持。

## null 和 undefined 的区别

* 意义不同

  `null` 是一个表示空对象

  `undefined` 是一个表示无的原始值

* 数值转换

  `null` 可以转为数值 0

  `undefined` 可以转为数值 NaN

* `null` 是一个 JavaScript 保留关键字，`undefined` 不是

## new 操作符都干了什么？

* 创建了一个空对象，
* 链接到原型：将该对象的 `__proto__` 属性指向构造函数的 `prototype` 属性
* 绑定 this 指向：将 this 变量引用指向该对象
* 执行构造函数，返回对象

### 实现一个 new

```js
function _new (fn, ...args) {
  const obj = Object.create(fn.prototype);
  const result = fn.apply(obj, args);
  return result instanceOf Object ? result : obj;
}
```

## 对 JSON 的了解

JSON 是一种轻量级的数据交换格式，它是基于 JavaSript 的一个子集。数据格式简单，易于读写，占用带宽小。

## JS 延迟加载的方式有哪些？

1. defer：开启新县城下载脚本，并使脚本在文档解析完成后执行
2. async：HTML5 新增属性，用于异步下载脚本文件，下载完成立即解释执行代码
3. 动态创建 DOM 方式
4. 按需异步载入

## 如何解决跨域问题

* jsonp：其原理是利用带有 `src` 的元素具有跨域的性质，以动态插入 script 标签的方式进行请求
* document.domain + iframe
* window.name、window.postMessage
* 服务器设置代理
* CORS：设置 http 头，Access-Control-Allow-Origin

## 哪些操作会造成内存泄漏？

内存泄漏是指任何对象在您不再拥有或者需要它之后仍然存在。

垃圾回收期定期扫描对象，并计算每个对象被其他对象引用的数量。如果一个对象的被引用数量为 0，或者对该对象的唯一引用是循环的，那么该对象的内存即可回收。

1. setTimout 的第一个参数使用字符串而非函数的话，会引发内存泄漏
2. 闭包
3. 控制台日志
4. 循环引用

## 如何判断当前脚本运行在浏览器还是 node 环境中？

`global === window`

## 对 Node 的优点和缺点提出自己的看法？

优点：

* 由于 Node 是基于事件驱动和无阻塞的，所以非常适合处理并发请求，因此构建在 Node 上的代理服务器相比起亚技术实现的服务器表现要好得多
* 与 Node 代理服务器交互的客户端代码是由 JavaScript 语言编写的，因为客户端和服务器端都用同一种语言编写。

缺点：

* 新东西，稳定性有待提供，资料不完整
* 缺少足够的三方库支持

## Javscript 对象的几种创建方式？

* 字面量
* new Object
* Object.create

## Ajax 的过程是怎样的

1. 创建 XMLHttpRequest 对象，也就是创建一个异步调用对象
2. 创建一个新的 HTTP 请求，并指定该 HTTP 请求的方法，URL 以及验证信息
3. 设置 响应 HTTP 请求状态变化的函数
4. 发送 HTTP 请求
5. 获取异步调用返回的数据
6. 使用 JavaScript 和 DOM 实现局部刷新

```js
const xhr = new XMLHttpRequest();

// 请求成功回调函数
xhr.onload = (e) => {
  console.log(request success);
}

// 请求结束
xhr.onloaded = (e) => {
  console.log(request loadend);
}

// 请求超时
xhr.ontimeout = (e) => {
    console.log('request timeout');
};


xhr.timeout = 0; // 设置超时时间,0表示永不超时
// 初始化请求
xhr.open('GET/POST/DELETE/...', '/url', true || false);
// 设置期望的返回数据类型 'json' 'text' 'document' ...
xhr.responseType = '';
// 设置请求头
xhr.setRequestHeader('', '');
// 发送请求
xhr.send(null || new FormData || 'a=1&b=2' || 'json字符串');
```

## 异步加载和延迟加载

异步：

* 动态插入 `script` 标签
* ajax 获取 js 代码，使用 `eval` 执行
* `script` 标签加上 `defer` 属性或者 `async` 属性：`defer` 是推迟执行，`async` 是加载完就执行
* 创建并插入 iframe

延迟加载：

有些代码在初始化页面的时候并不需要，因此可以延迟加载它

## Flash 和 Ajax 各自的优缺点，在使用中如何取舍？

Flash：

* Flash 适合处理多媒体，矢量图像、访问机器
* 对 CSS、处理文本上不足，不利于 SEO

ajax：

* Ajax 对 CSS，文本支持很好，支持搜索
* 多媒体，矢量图形，机器访问不足

共同点：

 * 与服务器交互，无刷新传递消息
 * 可以检测用户连线和在线状态
 * 操作 DOM

## 请解释一下 js 的同源策略？

同源策略是客户端脚本的重要安全度量标准。它最早出自 Netscape Navigator2.0，其目的是防止某个文档或脚本从多个不同源装载。

这里的同源策略指的是：协议、域名、端口号相同。

同源策略是一种安全协议，指一段脚本只能读取来自同一来源的窗口和文档的属性，执行同源窗口的代码。

为什么要有同源策略：

同源策略的目的，是为了保证用户信息的安全，防止恶意的网站窃取数据。

## 什么是 'use strict;'？使用它的好处和坏处分别是什么？

ES5 添加了第二种运行模式，严格模式，这种模式使得 Javascript 在更严格的条件下运行。

优点：

* 效率更高
* 语义更明确
* 更稳定、更安全
* 为未来增加更多可能性，提前做好向后兼容

缺点：

* 兼容，IE6/7/8/9 均不支持严格模式
* 严格模式改变了语义，依赖这些改变可能会导致没有实现严格模式的浏览器中出现问题或者错误
* 压缩

### 严格模式中的变化

严格模式同时改变了语法及运行时行为。变化通常分为这几类：

* 将问题直接转化为错误（如语法错误或运行时错误）
  * 严格模式下无法再意外创建全局变量。
  * 严格模式会使引起静默失败(silently fail,注:不报错也没有任何效果)的赋值操作抛出异常. 例如, NaN 是一个不可写的全局变量. 在正常模式下, 给 NaN 赋值不会产生任何作用; 开发者也不会受到任何错误反馈. 但在严格模式下, 给 NaN 赋值会抛出一个异常. 任何在正常模式下引起静默失败的赋值操作 (给不可写属性赋值, 给只读属性(getter-only)赋值, 给不可扩展对象(non-extensible object)的新属性赋值) 都会抛出异常:
  * 在严格模式下, 试图删除不可删除的属性时会抛出异常(之前这种操作不会产生任何效果)
  * 在Gecko版本34之前，严格模式要求一个对象内的所有属性名在对象内必须唯一。正常模式下重名属性是允许的，最后一个重名的属性决定其属性值。因为只有最后一个属性起作用，当代码要去改变属性值而不是修改最后一个重名属性的时候，复制这个对象就产生一连串的bug。在严格模式下，重名属性被认为是语法错误。
  * 严格模式要求函数的参数名唯一. 在正常模式下, 最后一个重名参数名会掩盖之前的重名参数. 之前的参数仍然可以通过 arguments[i] 来访问, 还不是完全无法访问. 然而, 这种隐藏毫无意义而且可能是意料之外的 (比如它可能本来是打错了), 所以在严格模式下重名参数被认为是语法错误。
  * 严格模式禁止八进制数字语法. ECMAScript并不包含八进制语法, 但所有的浏览器都支持这种以零(0)开头的八进制语法: 0644 === 420 还有 "\045" === "%".在ECMAScript 6中支持为一个数字加"0o"的前缀来表示八进制数.
  * ECMAScript 6中的严格模式禁止设置primitive值的属性.不采用严格模式,设置属性将会简单忽略(no-op),采用严格模式,将抛出TypeError错误
  
* 简化了如何为给定名称的特定变量计算
  * 严格模式禁用 `with`
  * 严格模式下的 `eval` 不再为上层范围(surrounding scope,注:包围eval代码块的范围)引入新变量
  * 严格模式禁止删除声明变量

* 简化了 `eval` 以及 `arguments`
  * 名称 eval 和 arguments 不能通过程序语法被绑定(be bound)或赋值. 
  * 严格模式下，参数的值不会随 arguments 对象的值的改变而变化。
  * 不再支持 arguments.callee

* 将写"安全“JavaScript的步骤变得更简单
  * 在严格模式下通过 `this` 传递给一个函数的值不会被强制转换为一个对象
  * 在严格模式中再也不能通过广泛实现的ECMAScript扩展“游走于”JavaScript的栈中
  * 严格模式下的 `arguments` 不会再提供访问与调用这个函数相关的变量的途径。

* 以及改变了预测未来 ECMAScript 行为的方式。
  * 在严格模式中一部分字符变成了保留的关键字。这些字符包括 `implements`, `interface`, `let`, `package`, `private`, `protected`, `public`, `static` 和 `yield`。
  * 严格模式禁止了不在脚本或者函数层面上的函数声明。

## GET 和 POST 的区别，何时使用 POST?

* 语义：GET 用于信息获取，POST 则用于信息的提交。
* 参数传递：GET 使用 URL 传递传输，POST 使用请求体传递参数。这也导致服务器获取参数的方式不同。
* 参数长度：GET 有限制，IE为2083byte，Chrome 为 8182，服务器对此也都有各自限制；POST 没有限制。
* 安全性：由于 GET 使用 URL 传输参数，参数更易被获取，因此安全性 GET < POST

上面的差异就决定了 POST 的使用场景：更安全、参数长度、保存或者修改数据

## 哪些地方会出现 CSS 阻塞，哪些地方会出现 js 阻塞？

浏览器有并行下载数量限制

* CSS：生成 render 树的时候
* JS：下载的时候会阻塞其他资源下载、执行都会影响

## eval 是做什么的？

`eval()` 函数会将传入的字符串当做 JavaScript 代码进行执行。

在代码中绝不应该使用该函数，非常不安全，也很耗时，因为要调用 js 解释器

## 写一个通用的事件监听函数？


```js
MyEvent = {
	// 页面加载完成后
	readyEvent: function(fn) {
		if (fn == null) {
			fn = document;
		}

		if (typeof window.onload !== 'function') {
			window.onload = fn;
		} else {
		  var oldonload = window.onload;
			window.onload = function() {
				oldonload();
				fn();
			};
		}
	},
	// 视能力分别使用dom0||dom2||IE方式 来绑定事件
	// 参数： 操作的元素,事件名称 ,事件处理程序
	addEvent: function(element, type, handler) {
		if (element.addEventListener) {
			//事件类型、需要执行的函数、是否捕捉
			element.addEventListener(type, handler, false);
		} else if (element.attachEvent) {
			element.attachEvent('on' + type, function() {
				handler.call(element);
			});
		} else {
			element['on' + type] = handler;
		}
	},
	// 移除事件
	removeEvent: function(element, type, handler) {
		if (element.removeEnentListener) {
			element.removeEnentListener(type, handler, false);
		} else if (element.datachEvent) {
			element.detachEvent('on' + type, handler);
		} else {
			element['on' + type] = null;
		}
	},
	// 阻止事件 (主要是事件冒泡，因为IE不支持事件捕获)
	stopPropagation: function(ev) {
		if (ev.stopPropagation) {
			ev.stopPropagation();
		} else {
			ev.cancelBubble = true;
		}
	},
	// 取消事件的默认行为
	preventDefault: function(event) {
		if (event.preventDefault) {
			event.preventDefault();
		} else {
			event.returnValue = false;
		}
	},
	// 获取事件目标
	getTarget: function(event) {
		return event.target || event.srcElement;
	},
	// 获取event对象的引用，取到事件的所有信息，确保随时能使用event；
	getEvent: function(e) {
		var ev = e || window.event;
		if (!ev) {
			var c = this.getEvent.caller;
			while (c) {
				ev = c.arguments[0];
				if (ev && Event === ev.constructor) {
					break;
				}
				c = c.caller;
			}
		}
		return ev;
	}
};
```

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

## 深度克隆

## typeof 和 instanceof 区别？举例说明

## 说说 Object.assign\(\) 和 Object.create\(\)

## Object.prototype.toString.call\(\) 具体做了什么

## javascript 类型哪几种？es6 新增的 symbol 对象，有什么作用？

### 基本类型：

基本类型（基本数值、基本数据类型）是一种既非对象也无方法的数据。在 JavaScript 中，共有7种基本类型：string，number，bigint，boolean，null，undefined，symbol (ECMAScript 2016新增)。

所有基本类型的值都是不可改变的。但需要注意的是，基本类型本身和一个赋值为基本类型的变量的区别。变量会被赋予一个新值，而原值不能像数组、对象以及函数那样被改变。

### 基本类型包装对象

* String 为字符串基本类型。
* Number 为数值基本类型。
* BigInt 为大整数基本类型。
* Boolean 为布尔基本类型。
* Symbol 为字面量基本类型。

## 怎么检查对象类型？

## es6 的解构赋值和深度 clone 了解嘛？

## 了解 作用域和原型链嘛？实现一个原型链

## 闭包的问题？es6闭包的问题？

## javascript为什么是单线程？知道event loop吗？实现一个



