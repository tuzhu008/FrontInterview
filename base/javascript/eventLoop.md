# Event Loop

JavaScript 有一个**基于事件循环**的并发模型，事件循环负责执行代码、收集和处理事件以及执行队列中的子任务。

![](/assets/the_javascript_runtime_environment_example.svg)

事件循环

JavaScript 是单线程的语言。为了实现”非阻塞“，调度各种任务而引入了事件循环。

JavaScript 的 所有任务可以分成两种，一种是**同步任务**（synchronous），另一种是**异步任务**（asynchronous）。

* 同步任务指的是，在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务；
* 异步任务指的是，不进入主线程、而进入"任务队列"（task queue）的任务，只有"任务队列"通知主线程，某个异步任务可以执行了，该任务才会进入主线程执行。

  常见的异步任务有 Ajax 操作、定时器（`setTimeout`/`setInterval`）、UI事件（load(图片js文件的加载等)、resize、scroll、click等）。这些异步由相应的线程处理后，会将其回调添加到事件任务队列等待主线程执行。

JS 主线程会先执行执行栈中的代码，直到执行栈中的任务全部执行完。这时候 JS 主线程会去查找事件队列中的任务。

## 堆

**堆**是堆内存的简称。

heap 是没有结构的，数据可以任意存放。heap 用于复杂数据类型（引用类型）分配空间，例如数组对象、object对象。

javascript 中其他类型的数据被称为引用类型的数据 : 如对象(Object)、数组(Array)、函数(Function) …，它们是通过拷贝和new出来的，这样的数据存储于堆中。其实，说存储于堆中，也不太准确，因为，引用类型的数据的地址指针是存储于栈中的，当我们想要访问引用类型的值的时候，需要先从栈中获得对象的地址指针，然后，在通过地址指针找到堆中的所需要的数据。


## 栈

**栈**是栈内存的简称。

stack是有结构的，每个区块按照一定次序存放（后进先出），stack中主要存放一些基本类型的变量和对象的引用，存在栈中的数据大小与生存期必须是确定的。可以明确知道每个区块的大小，因此，stack的寻址速度要快于heap。

javascript的基本类型就5种: Undefined、Null、Boolean、Number 和 String，它们都是直接按值存储在栈中的，每种类型的数据占用的内存空间的大小是确定的，并由系统自动分配和自动释放。这样带来的好处就是，内存可以及时得到回收，相对于堆来说，更加容易管理内存空间。

## 执行栈

使用栈的特性对函数执行进行管理。

## 事件任务队列

一个 JavaScript 运行时包含了一个待处理消息的消息队列。每一个消息都关联着一个用以处理这个消息的回调函数。

在事件循环期间的某个时刻，运行时会从最先进入队列的消息开始处理队列中的消息。被处理的消息会被移出队列，并作为输入参数来调用与之关联的函数。正如前面所提到的，调用一个函数总是会为其创造一个新的栈帧。

函数的处理会一直进行到执行栈再次为空为止；然后事件循环将会处理队列中的下一个消息（如果还有的话）。

## 图解

图2：

![](/assets/eventloop.png)

图1：

![](/assets/eventloop2.image)

## 任务类型

JS 任务类型有两种：**宏任务**和**微任务**。

### 宏任务
macrotask，又称为 task。

在以下时机，任务会被添加到任务队列：

* 一段新程序或子程序被直接执行时（比如从一个控制台，或在一个 `<script>` 元素中运行代码）。
* 触发了一个事件，将其回调函数添加到任务队列时。
* 执行到一个由 setTimeout() 或 setInterval() 创建的 timeout 或 interval，以致相应的回调函数被添加到任务队列时。

事件循环驱动你的代码按照这些任务排队的顺序，一个接一个地处理它们。在当前迭代轮次中，只有那些当事件循环过程开始时 已经处于任务队列中的任务会被执行。其余的任务不得不等待到下一次迭代。

### 微任务

起初微任务和任务之间的差异看起来不大。它们很相似；都由位于某个队列的 JavaScript 代码组成并在合适的时候运行。但是，只有在迭代开始时队列中存在的任务才会被事件循环一个接一个地运行，这和处理微任务队列是殊为不同的。

有两点关键的区别。

首先，每当一个任务存在，事件循环都会检查该任务是否正把控制权交给其他 JavaScript 代码。如若不然，事件循环就会运行微任务队列中的所有微任务。接下来微任务循环会在事件循环的每次迭代中被处理多次，包括处理完事件和其他回调之后。

其次，如果一个微任务通过调用 queueMicrotask(), 向队列中加入了更多的微任务，则那些新加入的微任务 会早于下一个任务运行。这是因为事件循环会持续调用微任务直至队列中没有留存的，即使是在有更多微任务持续被加入的情况下。

包括：
* `Promise.then`、`catch`、`finally`
* `await` 后面代码
* `process.nextTick`（仅Node）
* `MutationObserver`（仅浏览器）


### 执行过程

事件循环的过程如下：

1. JS引擎（唯一主线程）按顺序解析代码，遇到函数声明，直接跳过，遇到函数调用，入栈；
1. 如果是同步函数调用，直接执行得到结果，同步函数弹出栈，继续下一个函数调用；
1. 如果是异步函数调用，分发给Web API（多个辅助线程），异步函数弹出栈，继续下一个函数调用；
1. Web API中，异步函数在相应辅助线程中处理完成后，即异步函数达到触发条件了（比如setTimeout设置的10s后），如果异步函数是宏任务，则入宏任务消息队列，如果是微任务，则入微任务消息队列；
1. Event Loop不停地检查主线程的调用栈与回调队列，当调用栈空时，就把微任务消息队列中的第一个任务推入栈中执行，执行完成后，再取第二个微任务，直到微任务消息队列为空；然后去宏任务消息队列中取第一个宏任务推入栈中执行，当该宏任务执行完成后，在下一个宏任务执行前，再依次取出微任务消息队列中的所有微任务入栈执行。
1. 上述过程不断循环，每当微任务队列清空，可作为本轮事件循环的结束。

有几个关键点如下：

* 所有微任务总会在下一个宏任务之前全部执行完毕，宏任务必然是在微任务之后才执行的（因为微任务实际上是宏任务的其中一个步骤）。
* 宏任务按顺序执行，且浏览器在每个宏任务之间渲染页面
* 所有微任务也按顺序执行，且在以下场景会立即执行所有微任务

## 参考文档

[掘金：浏览器多线程和js单线程](https://juejin.cn/post/6844903812642111501)