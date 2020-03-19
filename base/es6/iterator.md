# Iterator

`Iterator` 是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署 `Iterator` 接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）。

`Iterator` 的作用有三个：

* 为各种数据结构，提供一个统一的、简便的访问接口；

* 二是使得数据结构的成员能够按某种次序排列；

* ES6 创造了一种新的遍历命令 `for...of` 循环，`Iterator` 接口主要供 `for...of` 消费。

Iterator 的遍历过程：

1. 创建一个指针对象，指向当前数据结构的起始位置。也就是说，遍历器对象本质上，就是一个指针对象。

2. 第一次调用指针对象的 `next` 方法，可以将指针指向数据结构的第一个成员。

3. 第二次调用指针对象的 `next` 方法，指针就指向数据结构的第二个成员。

4. 不断调用指针对象的 `next` 方法，直到它指向数据结构的结束位置。

一种数据结构只要部署了 `Iterator` 接口，我们就称这种数据结构是“可遍历的”（iterable）。

ES6 规定，默认的 `Iterator` 接口部署在数据结构的 `Symbol.iterator` 属性，或者说，一个数据结构只要具有 `Symbol.iterator` 属性，就可以认为是“可遍历的”（iterable）。

`Symbol.iterator` 属性本身是一个函数，就是当前数据结构默认的遍历器生成函数。执行这个函数，就会返回一个遍历器。

原生具备 Iterator 接口的数据结构如下。

* Array

* Map

* Set

* String

* TypedArray

* 函数的 arguments 对象

* NodeList 对象

## 调用 Iterator 接口

有一些场合会默认调用 Iterator 接口：

* 解构赋值

* 扩展运算符

* `yield*`

* `for...of`

* `Array.from()`

* `Map(), Set(), WeakMap(), WeakSet()（比如new Map([['a',1],['b',2]])）`

* `Promise.all()`

* `Promise.race()`
