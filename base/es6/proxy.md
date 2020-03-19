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


* deleteProperty


* ownKeys


* getOwnPropertyDescriptor


* defineProperty


* preventExtensions


* getPrototypeOf


* isExtensible


* setPrototypeOf


* apply


* construct

