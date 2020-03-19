# Proxy

`Proxy` 对象用于定义基本操作的自定义行为（如属性查找，赋值，枚举，函数调用等）。

Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。

## 可以设置的值

* get
* set
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