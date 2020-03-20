# Vue

## mvvm 框架是什么？

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
|  |  |

## vue-loader 是什么？使用它的用途有哪些？

## 为什么使用key?

## v-on可以监听多个方法吗？

## 单页面应用和多页面应用区别及优缺点

## vue和jQuery的区别

## delete和Vue.delete删除数组的区别

## SPA首屏加载慢如何解决

## Vue-router 跳转和 location.href 有什么区别

## vue slot

## 你们vue项目是打包了一个js文件，一个css文件，还是有多个文件？

## Vue里面router-link在电脑上有用，在安卓上没反应怎么解决？

## Vue2中注册在router-link上事件无效解决方法

## RouterLink在IE和Firefox中不起作用（路由不跳转）的问题

## params和query的区别

## vue更新数组时触发视图更新的方法

## vue-router 是什么?它有哪些组件

## active-class 是哪个组件的属性？

## 怎么定义 vue-router 的动态路由? 怎么获取传过来的值？

## $route 和 $router 的区别

## vue-router的两种模式

## vue-router实现路由懒加载（ 动态加载路由 ）

## vuex

## 双向绑定原理

`Object.defineProperty`

* 这里重点解释 虚拟 dom 和效率问题

* 说说 vue 的生命周期

* vue 的虚拟 dom 是什么？有什么影响？说说你在项目中比较你使用的 vue / javascript/jquery 的区别

* vue 比较 jquery 有什么优势和劣势？

* 说说你对 reatc 和 angular 的了解和看法？



