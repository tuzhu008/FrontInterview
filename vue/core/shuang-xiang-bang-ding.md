# 双向绑定的原理

Vue 数据双向绑定原理是通过：数据劫持 + 发布者-订阅者模式 的方式来实现的。

Vue首先劫持（监听）各属性的 getter、setter，并在当监听的属性发生变动时通知订阅者，是否需要更新，若更新就会执行对应的更新函数。

常见的基于数据劫持的双向绑定有两种实现:

* `Object.defineProperty`。Vue2.0 使用。

* `Proxy`。ES2015 新特性，在 Vue3.0 版本开始使用 `Proxy` 代替 `Object.defineProperty`。