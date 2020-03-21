# Vue

## mvvm 框架是什么？

MVVM 是 Model-View-ViewModel 的缩写。

* Model

  Model 代表数据模型，也可以在 Model 中定义数据修改和操作的业务逻辑。

* View

  View 代表 UI 组件，它负责将数据模型转化成 UI 展现出来。

* ViewModel

  MVVM的核心是数据驱动即ViewModel，ViewModel是View和Model的关系映射。ViewModel类似中转站(Value Converter)，负责转换Model中的数据对象，使得数据变得更加易于管理和使用。MVVM本质就是基于操作数据来操作视图进而操作DOM，借助于MVVM无需直接操作DOM，开发者只需完成包含声明绑定的视图模板，编写ViewModel中有业务，使得View完全实现自动化。

  ViewModel 监听模型数据的改变和控制视图行为、处理用户交互，简单理解就是一个同步 View 和 Model 的对象，连接 Model 和 View。

在 MVVM 架构下，View 和 Model 之间并没有直接的联系，而是通过 ViewModel 进行交互，Model 和 ViewModel 之间的交互是双向的， 因此 View 数据的变化会同步到 Model 中，而 Model 数据的变化也会立即反应到 View 上。

ViewModel 通过双向数据绑定把 View 层和 Model 层连接了起来，而View 和 Model 之间的同步工作完全是自动的，无需人为干涉，因此开发者只需关注业务逻辑，不需要手动操作 DOM, 不需要关注数据状态的同步问题，复杂的数据状态维护完全由 MVVM 来统一管理。

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

## vue 优点

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



