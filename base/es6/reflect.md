# Reflect

`Reflect` 是一个内置的对象，它提供拦截 JavaScript 操作的方法。

这些方法与 proxy handlers 的方法相同。Reflect 不是一个函数对象，因此它是不可构造的。

`Reflect` 的所有属性和方法都是静态的。

## 设计目的

`Reflect` 的很多方法与 `Object` 的方法重合，如此设计有以下几点：

* 将Object对象的一些明显属于语言内部的方法（比如 `Object.defineProperty` ），放到 `s` 对象上。

* 修改某些Object方法的返回结果，让其变得更合理。

* 让 `Object` 操作都变成函数行为

* `Reflect` 对象的方法与Proxy对象的方法一一对应，只要是 `Proxy` 对象的方法，就能在 `Reflect` 对象上找到对应的方法。

## API

* `Reflect.apply()`

  对一个函数进行调用操作，同时可以传入一个数组作为调用参数。

* `Reflect.construct()`

  对构造函数进行 `new` 操作，相当于执行 `new target(...args)`。

* `Reflect.get()`

  获取对象身上某个属性的值，类似于 `target[name]`。

* `Reflect.set()`

  将值分配给属性的函数。返回一个 Boolean，如果更新成功，则返回 `true`。

* `Reflect.has()`

  判断一个对象是否存在某个属性，和 `in` 运算符 的功能完全相同。

* `Reflect.ownKeys()`

  返回一个包含所有自身属性（不包含继承属性）的数组。(类似于 `Object.keys()`, 但不会受 `enumerable` 影响).

* `Reflect.defineProperty()`

  `Object.defineProperty()` 类似。

* `Reflect.deleteProperty()`

  作为函数的 `delete` 操作符，相当于执行 `delete target[name]`

* `Reflect.getPrototypeOf()`

  类似于 `Object.getPrototypeOf()`。

* `Reflect.setPrototypeOf()`

  类似于 `Object.setPrototypeOf()`。

* `Reflect.getOwnPropertyDescriptor()`

  类似于 `Object.getOwnPropertyDescriptor()`。

* `Reflect.isExtensible()`

  类似于 `Object.isExtensible()`.

* `Reflect.preventExtensions()`

  类似于 `Object.preventExtensions()`。返回一个 Boolean。
