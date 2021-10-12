# JavaScript

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



