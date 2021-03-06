# Proxy

`Proxy` 对象用于定义基本操作的自定义行为（如属性查找，赋值，枚举，函数调用等）。

Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。

## 可以设置的值

* get

  方法用于拦截某个属性的读取操作

* set

  用来拦截某个属性的赋值操作。

  利用 `set` 方法，还可以数据绑定，即每当对象发生变化时，会自动更新 DOM。

* apply

  用于拦截函数的调用、`call` 和 `apply` 操作

* has

  用来拦截 `HasProperty` 操作，即判断对象是否具有某个属性时，这个方法会生效。

  比如 `in` 操作符。`for ... in` 的 `in` 无效。

* construct

  用于拦截 `new` 命令

  `construct` 方法返回的必须是一个对象，否则会报错。

* deleteProperty

  用于拦截 `delete` 操作，如果这个方法抛出错误或者返回 `false`，当前属性就无法被 `delete` 命令删除。

* ownKeys

  用来拦截对象自身属性的读取操作。具体来说，拦截以下操作：

  * `Object.getOwnPropertyNames()`

  * `Object.getOwnPropertySymbols()`

  * `Object.keys()`

  * `for...in`

  注意，使用 `Object.keys` 方法时，有三类属性会被 `ownKeys` 方法自动过滤，不会返回。

  * 目标对象上不存在的属性
  
  * 属性名为 Symbol 值
  
  * 不可遍历（enumerable）的属性

  如果目标对象自身包含不可配置的属性，则该属性必须被 `ownKeys` 方法返回，否则报错。

  如果目标对象是不可扩展的（non-extensible），这时 `ownKeys` 方法返回的数组之中，必须包含原对象的所有属性，且不能包含多余的属性，否则报错。

* getOwnPropertyDescriptor

  拦截 `Object.getOwnPropertyDescriptor()`，返回一个属性描述对象或者 `undefined`。

* defineProperty

  用于拦截 `Object.defineProperty` 操作。

* preventExtensions

  拦截 `Object.preventExtensions()`。
  
  该方法必须返回一个布尔值，否则会被自动转为布尔值。

  只有目标对象**不可**扩展时（即 `Object.isExtensible(proxy)` 为 `false`），`proxy.preventExtensions` 才能返回 `true`，否则会报错。

* getPrototypeOf

  主要用来拦截获取对象原型。具体来说，拦截下面这些操作：

  * `Object.prototype.__proto__`

  * `Object.prototype.isPrototypeOf()`

  * `Object.getPrototypeOf()`

  * `Reflect.getPrototypeOf()`

  * `instanceof`

  `getPrototypeOf` 方法的返回值必须是对象或者null，否则报错。

  另外，如果目标对象不可扩展（non-extensible）， `getPrototypeOf` 方法必须返回目标对象的原型对象。

* isExtensible

  拦截 `Object.isExtensible` 操作。

  该方法只能返回布尔值，否则返回值会被自动转为布尔值。

  它的返回值必须与目标对象的 `isExtensible` 属性保持一致，否则就会抛出错误。

* setPrototypeOf

  主要用来拦截 `Object.setPrototypeOf` 方法。

  该方法只能返回布尔值，否则会被自动转为布尔值。

  如果目标对象不可扩展（non-extensible），`setPrototypeOf` 方法不得改变目标对象的原型。

