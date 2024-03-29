# JavaScript

* 在 js 中如何把类数组转化为数组
* Array(100).map(x => 1) 结果是多少
* 如何实现 compose 函数，进行函数合成
* 如何实现类似 lodash.get 函数
* js 中什么是可选链操作符，如何访问数组
* 如何实现一个深拷贝 (cloneDeep)
* 如何实现一个 flatMap 函数 (头条)
* 如何实现一个 async/await
* 如何使用 async/await 实现 Promise.all 的效果
* 有没有遇到 js 捕捉不到异常堆栈信息的情况
* Promise 在异步资源的生命周期 (async_hooks) 中是如何被销毁的
* 有没有用过 Promise.allSettled() ，它是干什么的
* 使用 js 实现一个 lru cache
* 如何实现 Promise.race
* '+' 与 '~' 选择器有什么不同
* js 中在 new 的时候发生了什么

## null 和 undefined 的区别

* 意义不同

  `null` 是一个表示空对象

  `undefined` 是一个表示无的原始值

* 数值转换

  `null` 可以转为数值 0

  `undefined` 可以转为数值 NaN

* `null` 是一个 JavaScript 保留关键字，`undefined` 不是

## 为什么 `typeof null === 'object'`

在计算机中，所有数据在底层都是按照二进制来存储的。javascript 中 `typeof` 通过数据二进制的前三位来判断类型。
object 的二进制前三位为 `000`，而 `null` 的所有二进制位全部都为 0，所以 `null` 也会被判定为 object`。

## 为什么 `0.1 + 0.2 !== 0.3`

主要由于 0.1 和 0.2 转为二进制的时候为无限循环小数，而计算机的存储位置有限因此会做一定的截取舍入处理，再进行加减就有一定的误差了

## new 操作符都干了什么？

* 创建了一个空对象，
* 链接到原型：将该对象的 `__proto__` 属性指向构造函数的 `prototype` 属性
* 绑定 this 指向：将 this 变量引用指向该对象
* 执行构造函数，返回对象

**实现一个 new**

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

1. defer：开启新线程下载脚本，并使脚本在文档解析完成后执行
2. async：HTML5 新增属性，用于异步下载脚本文件，下载完成立即解释执行代码
3. 动态创建 DOM 方式
4. 按需异步载入

## 什么是跨域？如何解决跨域问题

跨域是由浏览器的同源策略造成的，是浏览器施加的安全限制。

同源策略是一个重要的安全策略，它用于限制一个源的文档或者它加载的脚本如何能与另一个源的资源进行交互。

同源是指两个 URL 的**协议**、**端口**（如果有指定的话）和**主机**都相同的话，则这两个 URL 是同源的。

目前，如果非同源，共有三种行为受到限制：

  * Cookie、LocalStorage 和 IndexDB 无法读取。

  * DOM 无法获得。

  * AJAX 请求不能发送。

`<script>` `<img>` `<iframe>` `<link>` 等带 `src` 属性的标签都可以跨域加载资源，而不受同源策略的限制。 

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

## Javscript 对象的几种创建方式？

* 字面量
* new Object
* Object.create

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

## GET 和 POST 的区别，何时使用 POST?

* 语义：GET 用于信息获取，POST 则用于信息的提交。
* 参数传递：GET 使用 URL 传递传输，POST 使用请求体传递参数。这也导致服务器获取参数的方式不同。
* 参数长度：GET 有限制，IE为2083byte，Chrome 为 8182，服务器对此也都有各自限制；POST 没有限制。
* 安全性：由于 GET 使用 URL 传输参数，参数更易被获取，因此安全性 GET &lt; POST

上面的差异就决定了 POST 的使用场景：更安全、参数长度、保存或者修改数据

## 哪些地方会出现 CSS 阻塞，哪些地方会出现 js 阻塞？

浏览器有并行下载数量限制

* CSS：生成 render 树的时候
* JS：下载的时候会阻塞其他资源下载、执行都会影响

## eval 是做什么的？

`eval()` 函数会将传入的字符串当做 JavaScript 代码进行执行。

在代码中绝不应该使用该函数，非常不安全，也很耗时，因为要调用 js 解释器

## .call 和 .apply 的作用？

动态改变某个类的某个方法的运行环境

## typeof 和 instanceof 区别？举例说明

## 说说 Object.assign\(\) 和 Object.create\(\)

## Object.prototype.toString.call\(\) 具体做了什么

## javascript 类型哪几种？es6 新增的 symbol 对象，有什么作用？

### 基本类型：

基本类型（基本数值、基本数据类型）是一种既非对象也无方法的数据。在 JavaScript 中，共有7种基本类型：string，number，bigint，boolean，null，undefined，symbol \(ECMAScript 2016新增\)。

所有基本类型的值都是不可改变的。但需要注意的是，基本类型本身和一个赋值为基本类型的变量的区别。变量会被赋予一个新值，而原值不能像数组、对象以及函数那样被改变。

### 基本类型包装对象

* String 为字符串基本类型。
* Number 为数值基本类型。
* BigInt 为大整数基本类型。
* Boolean 为布尔基本类型。
* Symbol 为字面量基本类型。

## 怎么检查对象类型？

## javascript为什么是单线程？知道event loop吗？实现一个

## 垃圾回收

JavaScript 具有自动垃圾收集机制，也就是说，执行环境会负责管理代码执行过程中使用的内存。

这种垃圾回收机制的原理很简单：找出那些不再继续使用的变量，然后释放其占用的内存。

垃圾收集器会按照固定的时间间隔周期性地执行这一操作。

垃圾收集器会跟踪哪个变量有用哪个变量没用，对于不再有用的变量打上标记，以备将来收回其占用的内存。

用于标识无用变量的策略可能会因实现而异。

### 回收机制

JS 的垃圾回收机制有两种：标记清除、引用计数

* **标记清除：**

  * 标记阶段：从根集合出发，将所有活动对象及其子对象打上标记
  * 清除阶段：遍历堆，将非活动对象（未打上标记）的连接到空闲链表上

  _优点：_

  * 实现简单， 容易和其他算法组合

  _缺点：_

  * 碎片化， 会导致无数小分块散落在堆的各处
  * 分配速度不理想，每次分配都需要遍历空闲列表找到足够大的分块
  * 与写时复制技术不兼容，因为每次都会在活动对象上打上标记

* **引用计数：**

  引用计数，就是记录每个对象被引用的次数，每次新建对象、赋值引用和删除引用的同时更新计数器，如果计数器值为 0 则直接回收内存。

  _优点：_

  * 可即刻回收垃圾
  * 最大暂停时间短
  * 没有必要沿指针查找， 不要和标记-清除算法一样沿着根集合开始查找

    _缺点：_

  * 计数器的增减处理繁重

  * 计数器需要占用很多位

  * 实现繁琐复杂， 每个赋值操作都得替换成引用更新操作

  * 循环引用无法回收

> [几种垃圾回收算法](https://www.jianshu.com/p/a8a04fd00c3c)  
> [V8 之旅： 垃圾回收器](http://newhtml.net/v8-garbage-collection/)

## setTimeout、setInterval 和 requetAnimationFrame 之间的区别

setTimeout 延迟执行。timeout 最小时间为 4ms，之后会将回调函数添加到事件队列中。

setInterval 延迟执行。

requetAnimationFrame 要求浏览器在下次重绘之前执行。回调函数执行次数通常是每秒60次，1000 / 60 ≈ 16.6 ms

## this 指向有哪几种？

默认绑定：全局环境中，this 默认绑定到 window。

隐式绑定：一般地，被直接对象所包含的函数调用时，也称为方法调用，this 隐式绑定到该直接对象。

隐式丢失：隐式丢失是指被隐式绑定的函数丢失绑定对象，从而默认绑定到 window。

显式绑定：通过 `call()`, `apply()`, `bind()` 方法把对象绑定到 this 上，叫显式绑定。

new 绑定：如果函数或者方法调用前带有换剪子 new, 它就构成构造函数调用，对于 this 绑定来说，称为 new 绑定。

* 构造函数通常不适用 `return` 语句，它们通常初始化新对象，当构造函数的函数体执行完毕时，他会显式返回。在这种情况下，构造函数调用表达式的计算结果就是这个新对象的值
* 如果构造函数使用 `return` 语句但并没有指定返回值，或者返回一个原始值，那么这是将忽略返回值，同事使用这个新对象作为调用结果。
* 如果构造函数显式地使用 `return` 语句返回一个对象，那么调用表达式的值就是这个对象。

## 暂时性死区

`let` `const` 关键字声明的变量不会有 `var` 的变量提升，必须先声明后使用。

## 判断一个变量是数组有几种方式？

* `instanceof`，原型判断，写法：`变量 instanceof Array`

* `__proto__`，原型判断，写法：`变量.__proto__ === Array.prototype`

* `constructor`，原型判断，写法：`变量.constructor === Array`

* `Object.prototype.toString`，通过object类型的副属性class去判断的其中函数的class是Function，结果是`[object Function]`， 普通的对象是Object，结果是`[object Object]`，写法：`Object.prototype.toString.call(变量) === '[object Array]'`

* `Array.isArray`，es6 新增的方法，写法：`Array.isArray(变量)`

## JS中如何区别Object 和 Array？

`typeof` 一般测试基本类型（Undefined、Null、Boolean、Number、String\)，对引用类型（数组，对象，函数），数组和对象返回object，函数引用类型返回 `Function`。`typeof` 对于区分数组和对象是没有用的。

* 通过 ES6 中的 `Array.isArray` 来识别

  ```
  Array.isArray([])  //true
  Array.isArray({})  //false
  ```

* 通过  `instanceof` 来识别

  ```
  [] instanceof Array   //true
  {} instanceof Array   //false
  ```

* 通过 `constructor` 来识别

  ```
  [].constructor === Array
  [].constructor === Object
  ```

* 通过  `Object.prototype.toString.call` 来识别

  ```
  Object.prototype.toString.call([])   //["object Array"]
  Object.prototype.toString.call({})   //["object Object"]
  ```

* 通过 `isPrototypeOf()` 函数来识别

  ```
  Array.prototype.isPrototypeOf(arr) //true表示是数组，false不是数组
  ```

## 事件代理、事件冒泡、事件捕获是什么？

**事件代理：**又称之为事件委托，是JavaScript中常用绑定事件的常用技巧。“事件代理”是把原本需要绑定在子元素的响应事件（click、keydown......）委托给父元素，让父元素担当事件监听的职务。事件代理的原理是DOM元素的事件冒泡。

**事件代理优点：**

1. 可以大量节省内存占用，减少事件注册；
2. 可以实现当新增子对象时无需再次对其绑定（动态绑定事件）。

一个事件触发后，会在子元素和父元素之间传播（propagation），这种传播分成三个阶段：

* **捕获阶段：**
  从window对象传导到目标节点（上层传到底层）称为“捕获阶段”（capture phase），捕获阶段不会响应任何事件；
* **目标阶段：**
  在目标节点上触发，称为“目标阶段”
* **冒泡阶段：**
  从目标节点传导回window对象（从底层传回上层），称为“冒泡阶段”（bubbling phase）。事件代理即是利用事件冒泡的机制把里层所需要响应的事件绑定到外层。

## TODO: axios 原理

axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。axios可以用在浏览器和 node.js 中是因为，它会自动判断当前环境是什么，如果是浏览器，就会基于XMLHttpRequests实现axios。如果是node.js环境，就会基于node内置核心模块http实现axios。

* XMLHttpRequest 是浏览器内置的一个对象，它为客户端提供了在客户端和服务器之间传输数据的功能。
* process 对象是node内置的一个全局变量，提供有关信息，控制当前 Node.js 进程。通过判断process是否存在，来判断是否是node环境。

**特征：**

1. Make XMLHttpRequests from the browser（从浏览器发起XMLHttpRequests请求）
2. Make http requests from node.js（从node.js发起http请求）
3. Supports the Promise API（支持PromiseAPI）
4. Intercept request and response（拦截请求和响应）
5. Transform request and response data（转换请求和响应数据）
6. Cancel requests（取消请求）
7. Automatic transforms for JSON data（自动转换json数据）
8. Client side support for protecting against XSRF（客户端支持自动防止XSRF）

## 什么是跨域？如何处理？

当一个请求url的**协议、域名、端口**三者之间任意一个与当前页面url不同即为跨域。

> 出于浏览器的同源策略限制。同源策略（Sameoriginpolicy）是一种约定，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，则浏览器的正常功能可能都会受到影响。可以说Web是构建在同源策略基础之上的，浏览器只是针对同源策略的一种实现。同源策略会阻止一个域的javascript脚本和另外一个域的内容进行交互。所谓同源（即指在同一个域）就是两个页面具有相同的协议（protocol），主机（host）和端口号（port）。

**非同源限制：**

1. 无法读取非同源网页的 Cookie、LocalStorage 和 IndexedDB
2. 无法接触非同源网页的 DOM
3. 无法向非同源地址发送 AJAX 请求

**解决方法：**

**1、设置 document.domain 解决无法读取非同源网页的 Cookie问题**

> 因为浏览器是通过 document.domain 属性来检查两个页面是否同源，因此只要通过设置相同的document.domain，两个页面就可以共享Cookie（此方案仅限主域相同，子域不同的跨域应用场景。）

**2、跨文档通信 API：window.postMessage\(\)**

> 调用postMessage方法实现父窗口 [http://test1.com](https://link.zhihu.com/?target=http%3A//test1.com) 向子窗口 [http://test2.com](https://link.zhihu.com/?target=http%3A//test2.com) 发消息（子窗口同样可以通过该方法发送消息给父窗口），它可用于解决以下方面的问题：  
> 1.页面和其打开的新窗口的数据传递  
> 2.多窗口之间消息传递  
> 3.页面与嵌套的iframe消息传递  
> 4.上面三个场景的跨域数据传递

```js
// 父窗口打开一个子窗口
var openWindow = window.open('http://test2.com', 'title');

// 父窗口向子窗口发消息(第一个参数代表发送的内容，第二个参数代表接收消息窗口的url)
openWindow.postMessage('Nice to meet you!', 'http://test2.com');
```

调用message事件，监听对方发送的消息

```js
// 监听 message 消息
window.addEventListener('message', function (e) {
  console.log(e.source); // e.source 发送消息的窗口
  console.log(e.origin); // e.origin 消息发向的网址
  console.log(e.data);   // e.data   发送的消息
},false);
```

**3、JSONP**

> JSONP 是服务器与客户端跨源通信的常用方法。最大特点就是简单适用，兼容性好（兼容低版本IE），缺点是只支持get请求，不支持post请求。
>
> 核心思想：网页通过添加一个&lt;script&gt;元素，向服务器请求 JSON 数据，服务器收到请求后，将数据放在一个指定名字的回调函数的参数位置传回来。

**4、CORS**

> CORS 是跨域资源分享（Cross-Origin Resource Sharing）的缩写。它是 W3C 标准，属于跨源 AJAX 请求的根本解决方法。
>
> 普通跨域请求：只需服务器端设置 Access-Control-Allow-Origin
>
> 带cookie跨域请求：前后端都需要进行设置（根据xhr.withCredentials字段判断是否带有cookie）



