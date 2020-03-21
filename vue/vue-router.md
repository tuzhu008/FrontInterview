# Vue-Router

* 导航被触发。

* 在失活的组件里调用离开守卫。

* 调用全局的 `beforeEach` 守卫。

* 在重用的组件里调用 `beforeRouteUpdate` 守卫 \(2.2+\)。

* 在路由配置里调用 `beforeEnter`。

* 解析异步路由组件。

* 在被激活的组件里调用 `beforeRouteEnter`。

* 调用全局的 `beforeResolve` 守卫 \(2.5+\)。

* 导航被确认。

* 调用全局的 `afterEach` 钩子。

* 触发 DOM 更新。

* 用创建好的实例调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数。

## 钩子函数 {#item-7}

* beforeEach

* beforeRouteUpdate

* beforeEnter

* beforeRouteEnter

* beforeResolve

* afterEach

* beforeRouteLeave

## Vue-router 跳转和 location.href 有什么区别

## Vue里面router-link在电脑上有用，在安卓上没反应怎么解决？

Vue 路由在 Android 机上有问题，babel 问题，安装 babel polypill 插件解决。

## Vue2中注册在router-link上事件无效解决方法

## RouterLink在IE和Firefox中不起作用（路由不跳转）的问题

## vue-router 是什么?它有哪些组件

## 怎么定义 vue-router 的动态路由? 怎么获取传过来的值？

## $route 和 $router 的区别

## vue-router的两种模式

## vue-router实现路由懒加载（ 动态加载路由 ）

## params和query的区别

## active-class 是哪个组件的属性？

## vue-loader 是什么？使用它的用途有哪些？

**怎么定义 vue-router 的动态路由? 怎么获取传过来的值**

答：在 router 目录下的 index.js 文件中，对 path 属性加上 /:id，使用 router 对象的 params.id 获取。

