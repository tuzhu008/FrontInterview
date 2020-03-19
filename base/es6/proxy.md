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


* getOwnPropertyDescriptor

  拦截 `Object.getOwnPropertyDescriptor()`，返回一个属性描述对象或者 `undefined`。

* defineProperty

  用于拦截 `Object.defineProperty` 操作。

* preventExtensions


* getPrototypeOf

  主要用来拦截获取对象原型。具体来说，拦截下面这些操作。

  * `Object.prototype.__proto__`

  * `Object.prototype.isPrototypeOf()`

  * `Object.getPrototypeOf()`

  * `Reflect.getPrototypeOf()`

  * `instanceof`

* isExtensible


* setPrototypeOf


* apply


* construct

