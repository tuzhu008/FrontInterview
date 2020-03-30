  


源码剖析

1、vue内部与运行机制：

Vue.js 全局运行机制

响应式系统的基本原理

什么是 Virtual DOM？

如何编译template 模板？

## diff算法

* 比较只会在同层级进行, 不会跨层级比较

diff 算法的本质是找出两个对象之间的差异
diff 算法的核心是子节点数组对比,思路是通过 首尾两端对比
key 的作用 主要是
决定节点是否可以复用
建立key-index的索引,主要是替代遍历，提升性能

vdom是通过snabbdom.js库实现的,大概过程有以下三步：

* compile（把真实DOM编译成Vnode）
* diff（利用diff算法，比较oldVnode和newVnode之间有什么变化）
* patch（把这些变化用打补丁的方式更新到真实dom上去）

proxy代理？

2、vuex工作原理详解

