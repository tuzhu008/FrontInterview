# Vue 3

* 修改 diff 算法

* 将数据绑定的实现由 `Object.defineProperty` 调整为 `Proxy`

* 函数式 API

* Tree-shaking友好

* 增强了 TypeScript 支持


## Vue3.0 为什么要用 proxy？

在 Vue2 中， `0bject.defineProperty` 会改变原始数据，其需要遍历对象，为每个属性调用 `0bject.defineProperty` 来达到数据劫持的目的；并且，`0bject.defineProperty` 不支持 `delete` 操作的响应式。

Proxy 是创建对象的虚拟表示，并提供 `set` 、`get` 和 `deleteProperty` 等处理器，这些处理器可在访问或修改原始对象上的属性时进行拦截，有以下特点∶

* 不需用使用 `Vue.$set` 或 `Vue.$delete` 触发响应式。
* 全方位的数组变化检测，消除了 Vue2 无效的边界情况。
* 支持 `Map`，`Set`，`WeakMap` 和 `WeakSet`。

Proxy 实现的响应式原理与 Vue2的实现原理相同，实现方式大同小异∶

* get 收集依赖
* set、delete 等触发依赖的响应式
* 对于集合类型，就是对集合对象的方法做一层包装：原方法执行后执行依赖相关的收集或触发逻辑。