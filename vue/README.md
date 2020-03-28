# Vue

## 什么是 vue 生命周期？有什么作用？

Vue 实例从创建到销毁的过程，就是生命周期。

生命周期中有多个事件钩子，让我们在控制整个 Vue 实例的过程时更容易形成好的逻辑。

Vue 生命周期：

* beforeCreate

  Vue 实例已创建，初始化事件和生命周期之后、在注入和校验之前。

* created

  在注入和校验之后。这时候数据、数据等都已注入实例。实例初始化完成。

* beforeMount

  编译模板之后，将其挂载到 DOM 之前。

* mounted

  执行 render 函数，将渲染出来的内容，将其挂载到 DOM 之后。

* beforeUpdate

  更新之前。

* updated

  更新之后。

* beforeDestroy

  销毁之前。

* destroyed

  销毁之后。

* activated

  被 `keep-alive` 缓存的组件激活时调用。

* deactivated

  被 `keep-alive` 缓存的组件停用时调用。

[Vue 生命周期.png](/assets/VueLifecycle.png)

## 第一次页面加载会触发哪几个钩子？

* beforeCreate,

* created

* beforeMount

* mounted

## 简述每个周期具体适合哪些场景

* beforeCreate,

  loading

* created

  ajax

* mounted

  dom 操作

* beforeDestroy

  清理工作

## 虚拟 dom

浏览器 DOM 对象的 Vue 表示，将每一个节点使用 VNode 的对象进行表示，将 DOM 表现为 VNode 连接形成的树状结构。

Vue 采用高效的 diff 算法来比较虚拟 dom 的改变，将其生成真实的 DOM 树，然后插入文档中。

## vue 优点

## computed 实现原理

computed 的实现采用发布订阅模式。

computed 本质是一个惰性求值的观察者。

computed 内部实现了一个惰性的 watcher,也就是 computed watcher,computed watcher 不会立刻求值,同时持有一个 dep 实例。

其内部通过 `this.dirty` 属性标记计算属性是否需要重新求值。

当 computed 的依赖状态发生改变时,就会通知这个惰性的 watcher,

computed watcher 通过 this.dep.subs.length 判断有没有订阅者,

有的话,会重新计算,然后对比新旧值,如果变化了,会重新渲染。 \(Vue 想确保不仅仅是计算属性依赖的值发生变化，而是当计算属性最终计算的值发生变化时才会触发渲染 watcher 重新渲染，本质上是一种优化。\)

没有的话,仅仅把 this.dirty = true。 \(当计算属性依赖于其他数据时，属性并不会立即重新计算，只有之后其他地方需要读取属性的时候，它才会真正计算，即具备 lazy（懒计算）特性。\)

## 谈一谈 nextTick 的原理

* 线程

  GUI、JS 引擎线程、事件触发线程、定时线程、http 线程

* 执行栈

* 事件队列

* 事件循环

* 微任务

  `promise.then()`、`process.nextTick`

* 宏任务

  主代码块，setTimeout，setInterval 等

执行顺序：

* 执行一个宏任务（栈中没有就从事件队列中获取）

* 执行过程中如果遇到微任务，就将它添加到微任务的任务队列中

* 宏任务执行完毕后，立即执行当前微任务队列中的所有微任务（依次执行）

* 当前宏任务执行完毕，开始检查渲染，然后GUI线程接管渲染

* 渲染完毕后，JS线程继续接管，开始下一个宏任务（从事件队列中获取）

## computed 和 watch 有什么区别及运用场景?

computed 和 watch 实质都是去订阅某些状态的改变。但他们的设计思想有些不同。

computed 计算属性 : 依赖其它属性值，并且 computed 的值有缓存，只有它依赖的属性值发生改变，下一次获取 computed 的值时才会重新计算 computed 的值。这个过程应该是纯粹的数据处理，不能去执行一些副作用的任务。

watch 侦听器 : 更多的是「观察」的作用，无缓存性，类似于某些数据的监听回调，每当监听的数据变化时都会执行回调进行后续操作。

computed 主要用在需要通过对其他状态进行加工后得到结果的场景。比如数据过滤

watch 用于响应某些数据的变化，对此执行副作用。

## 为什么在 Vue3.0 采用了 Proxy，抛弃了 Object.defineProperty

`Object.defineProperty` 是为状态的每个属性去定义的，因此，需要去遍历状态对象。Proxy 是针对整个状态对象进行代理的。

Proxy 不仅可以定义 getter 和 setter，还可以针对其他一些操作进行代理，比如属性的 delete 操作，在2.0 时代，delete 属性无法触发状态更新，因此需要 Vue.delete。

## vue父组件向子组件传递数据？

* prop

* $emit

* .sync

* 事件中心

* 状态管理器

## v-show 和 v-if 指令的共同点和不同点？

`v-if` 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

`v-if` 也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

相比之下，`v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。

一般来说，`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好

## `<keep-alive></keep-alive>`的作用是什么?

`<keep-alive>` 包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们。

## vue 当中的指令

| 指令 | 作用 |
| :--- | :--- |
| `v-text` | 更新元素的 `textContent` |
| `v-html` | 更新元素的 `innerHTML` |
| `v-show` | 根据表达式之真假值，切换元素的 `display` CSS 属性 |
| `v-if` | 条件渲染 |
| `v-else` |  |
| `v-else-if` |  |
| `v-for` | 列表渲染 |
| `v-on` | 事件绑定 |
| `v-bind` | 数据绑定 |
| `v-model` | 数据双向绑定 |
| `v-slot` |  |
| `v-pre` | 跳过这个元素和它的子元素的编译过程 |
| `v-cloak` | 这个指令保持在元素上直到关联实例结束编译 |
| `v-once` | 绑定只执行一次的事件 |

## 为什么使用key?

key 是给每一个 vnode 的唯一 id，依靠 key,我们的 diff 操作可以更准确、更快速 \(对于简单列表页渲染来说 diff 节点也更快,但会产生一些隐藏的副作用,比如可能不会产生过渡效果,或者在某些节点有绑定数据（表单）状态，会出现状态错位。\)

`key` 的特殊属性主要用在 Vue 的虚拟 DOM 算法，在新旧 nodes 对比时辨识 VNodes。

如果不使用 `key`，Vue 会使用一种最大限度减少动态元素并且尽可能的尝试就地修改/复用相同类型元素的算法。

而使用 `key` 时，它会基于 `key` 的变化重新排列元素顺序，并且会移除 `key` 不存在的元素。

有相同父元素的子元素必须有独特的 `key`。重复的 `key` 会造成渲染错误。

## v-on可以监听多个方法吗？

```html
<!-- 对象语法 (2.4.0+) -->
<button v-on="{ mousedown: doThis, mouseup: doThat }"></button>
```

## 单页面应用和多页面应用区别及优缺点

## vue 和 jQuery 的区别

vue 操作数据，jQuery 操作 dom

## delete 和 Vue.delete 删除数组的区别

删除对象的属性。如果对象是响应式的，确保删除能触发更新视图。

这个方法主要用于避开 Vue 不能检测到属性被删除的限制，但是你应该很少会使用它。

## vue slot

`<slot>` 元素作为组件模板之中的内容分发插槽

## vue 更新数组时触发视图更新的方法

`push()` `pop()` `shift()` `unshift()` `splice()` `sort()` `reverse()`

## 双向绑定原理

`Object.defineProperty`

* 这里重点解释 虚拟 dom 和效率问题

* 说说 vue 的生命周期

* vue 的虚拟 dom 是什么？有什么影响？说说你在项目中比较你使用的 vue / javascript/jquery 的区别

* vue 比较 jquery 有什么优势和劣势？

* 说说你对 reatc 和 angular 的了解和看法？

## vue-loader 是什么？使用它的用途有哪些？

## 像vue-router，vuex他们都是作为vue插件，请说一下他们分别都是如何在vue中生效的？

通过 vue 的插件系统，用 `vue.mixin` 混入到全局，在每个组件的生命周期的某个阶段注入组件实例。

## 对于Vue是一套渐进式框架的理解

给多少材料干多少活儿

## vue.js 的两个核心是什么？

* 数据驱动

* 组件系统

## vue 常用的修饰符

**事件修饰符：**

| 修饰符 | 作用 |
| :--- | :--- |
| .stop |  |
| .prevent |  |
| .capture |  |
| .self |  |
| .once |  |
| .passive |  |

**按键修饰符：**

| 修饰符 | 作用 |
| :--- | :--- |
| .enter |  |
| .tab |  |
| .delete |  |
| .esc |  |
| .space |  |
| .up |  |
| .down |  |
| .left |  |
| .right|  |

**系统修饰符：**

| 修饰符 | 作用 |
| :--- | :--- |
| .ctrl |  |
| .alt |  |
| .shift |  |
| .meta |  |

**鼠标按钮修饰符：**

| 修饰符 | 作用 |
| :--- | :--- |
| .left |  |
| .right |  |
| .middle |  |

**其他修饰符：**

| 修饰符 | 作用 |
| :--- | :--- |
| .exact |  |



5、v-on可以监听多个方法吗？

6、vue中 key 值的作用

7、vue-cli工程升级vue版本

8、vue事件中如何使用event对象？

9、$nextTick的使用

10、Vue 组件中 data 为什么必须是函数

11、v-for 与 v-if 的优先级

12、vue中子组件调用父组件的方法

13、vue中 keep-alive 组件的作用

14、vue中如何编写可复用的组件？

15、什么是vue生命周期和生命周期钩子函数？

16、vue生命周期钩子函数有哪些？

17、vue如何监听键盘事件中的按键？

18、vue更新数组时触发视图更新的方法

19、vue中对象更改检测的注意事项

20、解决非工程化项目初始化页面闪动问题

21、v-for产生的列表，实现active的切换

22、v-model语法糖的组件中的使用

23、十个常用的自定义过滤器

24、vue等单页面应用及其优缺点

25、什么是vue的计算属性？

26、vue-cli提供的几种脚手架模板

27、vue父组件如何向子组件中传递数据？

28、vue-cli开发环境使用全局常量

29、vue-cli生产环境使用全局常量

30、vue弹窗后如何禁止滚动条滚动？

31、计算属性的缓存和方法调用的区别

32、vue-cli中自定义指令的使用

参考答案： [https://www.kancloud.cn/hanxu](https://www.kancloud.cn/hanxu)...

vue-router  
1、vue-router如何响应 路由参数 的变化？

2、完整的 vue-router 导航解析流程

3、vue-router有哪几种导航钩子（ 导航守卫 ）？

4、vue-router的几种实例方法以及参数传递

5、vue-router的动态路由匹配以及使用

6、vue-router如何定义嵌套路由？

7、`<router-link></router-link>`组件及其属性

8、vue-router实现路由懒加载（ 动态加载路由 ）

9、vue-router路由的两种模式

10、history路由模式与后台的配合

参考答案： [https://www.kancloud.cn/hanxu](https://www.kancloud.cn/hanxu)...

vuex  
1、什么是vuex？

2、使用vuex的核心概念

3、vuex在vue-cli中的应用

4、组件中使用 vuex 的值和修改值的地方？

5、在vuex中使用异步修改

6、pc端页面刷新时实现vuex缓存

参考答案： [https://www.kancloud.cn/hanxu](https://www.kancloud.cn/hanxu)...

http请求  
1、Promise对象是什么？

2、axios、fetch与ajax有什么区别？

3、什么是JS的同源策略和跨域问题？

4、如何解决跨域问题？

5、vue-cli中如何使用JSON数据模拟？

6、vue-cli中http请求的统一管理。

7、axios有什么特点？

参考答案： [https://www.kancloud.cn/hanxu](https://www.kancloud.cn/hanxu)...

UI样式  
1、.vue组件的scoped属性的作用

2、如何让CSS只在当前组件中起作用？

3、vue-cli中常用的UI组件库

4、如何适配移动端？【 经典 】

5、移动端常用媒体查询的使用

6、垂直居中对齐

7、vue-cli中如何使用背景图片？

8、使用表单禁用时移动端样式问题

9、多种类型文本超出隐藏问题

参考答案： [https://www.kancloud.cn/hanxu](https://www.kancloud.cn/hanxu)...

常用功能  
1、vue中如何实现tab切换功能？

2、vue中如何利用 keep-alive 标签实现某个组件缓存功能？

3、vue中实现切换页面时为左滑出效果

4、vue中父子组件如何相互调用方法？

5、vue中央事件总线的使用

参考答案： [https://www.kancloud.cn/hanxu](https://www.kancloud.cn/hanxu)...

混合开发  
1、vue如何调用 原生app 提供的方法？

2、原生app 调用 vue 提供的方法，并将值传递到 .vue 组件中

参考答案： [https://www.kancloud.cn/hanxu](https://www.kancloud.cn/hanxu)...

生产环境  
1、vue打包命令是什么？

2、vue打包后会生成哪些文件？

3、如何配置 vue 打包生成文件的路径？

4、vue如何优化首屏加载速度？

参考答案： [https://www.kancloud.cn/hanxu](https://www.kancloud.cn/hanxu)...

MVVM设计模式  
1、MVC、MVP与MVVM模式

2、MVC、MVP与MVVM的区别

3、常见的实现MVVM几种方式

4、Object.defineProperty\(\)方法

5、实现一个自己的MVVM（原理剖析）

6、 ES6中类和定义

7、JS中的文档碎片

8、解构赋值

9、Array.from与Array.reduce

10、递归的使用

11、Obj.keys\(\)与Obj.defineProperty

12、发布-订阅模式

13、实现MVVM的思路分析

参考答案： [https://www.kancloud.cn/hanxu](https://www.kancloud.cn/hanxu)...

源码剖析  
1、vue内部与运行机制：

Vue.js 全局运行机制  
响应式系统的基本原理  
什么是 Virtual DOM？  
如何编译template 模板？  
diff算法  
批量异步更新策略及 nextTick 原理？  
proxy代理？  
2、vuex工作原理详解

Vue.mixin  
Vue.use  
参考答案： [https://www.kancloud.cn/hanxu](https://www.kancloud.cn/hanxu)...

深入拓展  
1、vue开发命令 npm run dev 输入后的执行过程

2、vue的服务器端渲染

3、从零写一个npm安装包

4、vue-cli中常用到的加载器

5、webpack的特点

参考答案： [https://www.kancloud.cn/hanxu](https://www.kancloud.cn/hanxu)...

