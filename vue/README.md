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
| .stop | 阻止事件继续传播，作用同 `event.stopPropagation()` |
| .prevent | 阻止默认行为，作用同 `event.preventDefault()` |
| .capture | 添加事件监听器时使用事件捕获模式 |
| .self | 只当在 `event.target` 是当前元素自身时触发处理函数 |
| .once | 事件只会触发一次 |
| .passive | 指示 `addEventListener` 添加的处理函数永远也不会调用 `event.preventDefault`，就算调用，也会失效   |

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
| .keyCode |  |
| .sync |  |

## vue事件中如何使用event对象？

* 作为处理函数默认参数

  ```html
  <div id="example-2">
    <!-- `greet` 是在下面定义的方法名 -->
    <button v-on:click="greet">Greet</button>
  </div>
  ```

  ```js
  methods: {
    greet: function (event) {
      alert('Hello ' + this.name + '!')
      if (event) {
        alert(event.target.tagName)
      }
    }
  }
  ```

* 显示传递

  ```html
  <button v-on:click="warn('Form cannot be submitted yet.', $event)">
    Submit
  </button>
  ```

  ```js
  methods: {
    warn: function (message, event) {
      // 现在我们可以访问原生事件对象
      if (event) {
        event.preventDefault()
      }
      alert(message)
    }
  }
  ```

## Vue 组件中 data 为什么必须是函数

确保每个组件实例都有一个不同的数据源，使用对象的形式的，所有的实例会共享同一数据源。

## v-for 与 v-if 的优先级

当 `v-if` 与 `v-for` 一起使用时，`v-for` 具有比 `v-if` 更高的优先级。

## vue中子组件调用父组件的方法

子组件实例含有一个指向父组件实例的属性 `$parent`

## vue中如何编写可复用的组件？

* 全局

  `Vue.component`

* 局部

  ```js
  var ComponentOne = {};
  ```

## vue中对象更改检测的注意事项

* 数组

  数组方法中，只有 `push`、`pop`、`shift`、`unshift`、`splice`、`sort`、`reverse` 这些方法修改数组时，能有响应效果。

  使用索引修改、修改数组长度等都无效，因此需要改用其他方式实现。

* 对象

  Vue 无法检测到对象属性的添加和删除。

  使用 `Vue.set` 设置响应属性。 

## 解决非工程化项目初始化页面闪动问题

这个闪动问题是由于实例还没准备完毕导致， 使用 `v-cloak` 指令和 CSS 规则如 `[v-cloak] { display: none }` 一起用时，这个指令可以隐藏未编译的 Mustache 标签直到实例准备完毕。

## v-model语法糖的组件中的使用

`v-model` 是 `v-bind:value` 和 `v-on:input` 的语法糖

## vue等单页面应用及其优缺点

优点：

1. 分离前后端关注点，前端负责界面显示，后端负责数据存储和计算，各司其职，不会把前后端的逻辑混杂在一起；

2. 减轻服务器压力，服务器只用出数据就可以，不用管展示逻辑和页面合成，吞吐能力会提高几倍；

3. 效果类似桌面应用，用户体验好。

缺点：

* 不利于 SEO

* 首次加载慢

## 什么是 vue 的计算属性？

vue 的三大数据来源之一，用以对数据进行加工，具有响应式的特点，惰性求值，只有在其它依赖数据变化时才进行计算，计算结果会进行缓存。

## vue父组件如何向子组件中传递数据？

`props`、`Vuex`、`provide/inject`

## vue 弹窗后如何禁止滚动条滚动？

* 阻止所有滚动

  在弹窗组件上使用 `@touchmove.prevent`

* 阻止文档滚动

  ```js
  const scrollHandler = (() => {
    let mo;
    return {
      stop () {
        mo = function (e) {
          e.preventDefault();
        };
        document.body.style.overflow = 'hidden';
        document.addEventListener("touchmove", mo);
      },
      /***取消滑动限制***/
      resume () {
        document.body.style.overflow = ''; //出现滚动条
        document.removeEventListener("touchmove", mo);
      }
    }
  })()
  ```

## vue 如何优化首屏加载速度？

* 异步路由加载

* 不打包库文件

* 关闭 sourcemap

* 开启 gzip 压缩

  * webpack gzip

  * 后端 gzip

* loading

* 服务端渲染

## 服务端渲染
