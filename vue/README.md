# Vue

## 什么是 vue 生命周期？有什么作用？

Vue 实例从创建到销毁的过程，就是生命周期。

生命周期中有多个事件钩子，让我们在控制整个 Vue 实例的过程时更容易形成好的逻辑。

Vue 生命周期：

| 生命周期 | 描述 |
| :--- | :--- |
| beforeCreate | Vue 实例已创建，初始化事件和生命周期之后、在注入和校验之前。 |
| created | 在注入和校验之后。这时候数据、数据等都已注入实例。实例初始化完成。 |
| beforeMount | 编译模板之后，将其挂载到 DOM 之前。 |
| mounted | 执行 render 函数，将渲染出来的内容，将其挂载到 DOM 之后。 |
| beforeUpdate | 更新之前。 |
| updated | 更新之后。 |
| beforeDestroy | 销毁之前。 |
| destroyed | 销毁之后。 |
| activated | 被 `keep-alive` 缓存的组件激活时调用。 |
| deactivated | 被 `keep-alive` 缓存的组件停用时调用。 |

![Vue 生命周期.png](/assets/VueLifecycle.png)

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

## Vue 组件间通信有哪几种方式？

Vue 组件间通信是面试常考的知识点之一，这题有点类似于开放题，你回答出越多方法当然越加分，表明你对 Vue 掌握的越熟练。Vue 组件间通信只要指以下 3 类通信：父子组件通信、隔代组件通信、兄弟组件通信，下面我们分别介绍每种通信方式且会说明此种方法可适用于哪类组件间通信。

* props / $emit  适用 父子组件通信

  这种方法是 Vue 组件的基础，相信大部分同学耳闻能详，所以此处就不举例展开介绍。

* ref 与 $parent / $children 适用 父子组件通信

  ref：如果在普通的 DOM 元素上使用，引用指向的就是 DOM 元素；如果用在子组件上，引用就指向组件实例

  $parent / $children：访问父 / 子实例

* EventBus （$emit / $on）  适用于 父子、隔代、兄弟组件通信

  这种方法通过一个空的 Vue 实例作为中央事件总线（事件中心），用它来触发事件和监听事件，从而实现任何组件间的通信，包括父子、隔代、兄弟组件。

* $attrs/$listeners 适用于 隔代组件通信

  $attrs：包含了父作用域中不被 prop 所识别 (且获取) 的特性绑定 ( class 和 style 除外 )。当一个组件没有声明任何 prop 时，这里会包含所有父作用域的绑定 ( class 和 style 除外 )，并且可以通过 v-bind="$attrs" 传入内部组件。通常配合 inheritAttrs 选项一起使用。

  $listeners：包含了父作用域中的 (不含 .native 修饰器的)  v-on 事件监听器。它可以通过 v-on="$listeners" 传入内部组件

* provide / inject 适用于 隔代组件通信

  祖先组件中通过 provider 来提供变量，然后在子孙组件中通过 inject 来注入变量。 provide / inject API 主要解决了跨级组件间的通信问题，不过它的使用场景，主要是子组件获取上级组件的状态，跨级组件间建立了一种主动提供与依赖注入的关系。

* Vuex  适用于 父子、隔代、兄弟组件通信
  
  Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。每一个 Vuex 应用的核心就是 store（仓库）。“store” 基本上就是一个容器，它包含着你的应用中大部分的状态 ( state )。

  Vuex 的状态存储是响应式的。当 Vue 组件从 store 中读取状态的时候，若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新。

  改变 store 中的状态的唯一途径就是显式地提交  (commit) mutation。这样使得我们可以方便地跟踪每一个状态的变化。

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
| .passive | 指示 `addEventListener` 添加的处理函数永远也不会调用 `event.preventDefault`，就算调用，也会失效 |

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
| .right |  |

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

当 `v-if` 与 `v-for` 一起使用时，具有下列两种情况：

v2: `v-for` 具有比 `v-if` 更高的优先级。

v3: `v-if` 具有比 `v-for` 更高的优先级。

为避免产生歧义，因此应该避免将 `v-if` 和 `v-for` 放在一起使用。


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



## Vue 中 computed 和 watch 有什么区别？

最大的却别是设计理念，适用场景上的区别。 从设计模式的角度来讲，computed 是适配器模式，watch 则是观察者模式。

计算属性 computed：

* 支持缓存，只有依赖数据发生变化时，才会重新进行计算函数；
* 计算属性内不支持异步操作；
* 计算属性的函数中都有一个 get(默认具有，获取计算属性)和 set(手动添加，设置计算属性)方法；
* 计算属性是自动监听依赖值的变化，从而动态返回内容。

侦听属性 watch：

* 不支持缓存，只要数据发生变化，就会执行侦听函数；
* 侦听属性内支持异步操作；
* 侦听属性的值可以是一个对象，接收 handler 回调，deep，immediate 三个属性；
* 监听是一个过程，在监听的值变化时，可以触发一个回调，并做一些其他事情。

## 参考

[vue-interview](https://github.com/57code/vue-interview)