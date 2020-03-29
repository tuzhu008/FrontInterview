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

在某种意义上，调用 `pushState()` 与 设置 `window.location = "#foo"` 类似，二者都会在当前页面创建并激活新的历史记录。但 `pushState()` 具有如下几条优点：

* 新的 URL 可以是与当前URL同源的任意URL 。相反，只有在修改哈希时，设置 window.location 才能是同一个 document。

* 如果你不想改URL，就不用改。相反，设置 window.location = "\#foo";在当前哈希不是 \#foo 时， 才能创建新的历史记录项。

* 你可以将任意数据和新的历史记录项相关联。而基于哈希的方式，要把所有相关数据编码为短字符串。

* 如果 标题 随后还会被浏览器所用到，那么这个数据是可以被使用的（哈希则不是）。

注意 `pushState()` 绝对不会触发 `hashchange` 事件，即使新的 URL 与旧的 URL 仅哈希不同也是如此。

## Vue 里面 router-link 在电脑上有用，在安卓上没反应怎么解决？

Vue 路由在 Android 机上有问题，babel 问题，安装 babel polypill 插件解决。

## Vue2 中注册在 router-link 上事件无效解决方法

需要在 `@click` 后面加上 `native` 就可以了

## vue-router 是什么?它有哪些组件

Vue Router 是 Vue.js 官方的路由管理器。它和 Vue.js 的核心深度集成，让构建单页面应用变得易如反掌。

组件：

* router-link

* router-view

## 怎么定义 vue-router 的动态路由? 怎么获取传过来的值？

```js
const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头
    { path: '/user/:id', component: User }
  ]
})
```

`$route.params.id` 访问

## $route 和 $router 的区别

$router 是路由实例

$route 表示当前激活的路由的状态信息，包含了当前 URL 解析得到的信息，还有 URL 匹配到的路由记录 \(route records\)。

## vue-router 的两种模式

“更新视图但不重新请求页面”是前端路由原理的核心之一，目前在浏览器环境中这一功能的实现主要有两种方式：

* hash

  URL 中 `#` 后面的部分称为 hash，可通过window.location.hash属性读取。

  * hash 虽然出现在 URL 中，但不会被包括在 HTTP 请求中。它是用来指导浏览器动作的，对服务器端完全无用，因此，改变 hash 不会重新加载页面

  * 可以为hash的改变添加监听事件：

    ```js
    window.addEventListener("hashchange", funcRef, false)
    ```

  * 每一次改变hash（window.location.hash），都会在浏览器的访问历史中增加一个记录

  路由的改变主要由两个构成

  * push

    ```
    window.location.hash = newHash;
    ```

  * replace

    ```
    window.location.replace(newURL);
    ```

* history

  history 模式利用了的 history 的两个接口。这两个接口为 HTML5 新增：

  ```
  window.history.pushState(stateObject, title, URL)
  window.history.replaceState(stateObject, title, URL)
  ```

  * `stateObject`: 当浏览器跳转到新的状态时，将触发popState事件，该事件将携带这个stateObject参数的副本

  * `title`: 所添加记录的标题

  * `URL`: 所添加记录的URL

  调用这两个方法，会改变地址栏 URL，并且会修改浏览器历史记录栈，但浏览器不会立即发送请求该 URL。

  路由的改变主要由两个构成

  * push

    ```
    history.pushState();
    ```

  * replace

    ```
    history.replaceState();
    ```

  **比较：**

  * `pushState` 设置的新 URL 可以是与当前 URL 同源的任意 URL；而 hash 只可修改\#后面的部分，故只可设置与当前同文档的 URL

  * `pushState` 设置的新URL可以与当前 URL 一模一样，这样也会把记录添加到栈中；而 hash 设置的新值必须与原来不一样才会触发记录添加到栈中

  * `pushState` 通过 stateObject 可以添加任意类型的数据到记录中；而 hash 只可添加短字符串

  * `pushState` 可额外设置 title 属性供后续使用

## vue-router 实现路由懒加载（ 动态加载路由 ）

实现主要是通过 webpack 的动态 import 来实现，webpack 会进行代码分割，将动态加载的组件打入单独的包中，我们可以在 import 的时候指定分割到的包名：

```js
const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
const Bar = () => import(/* webpackChunkName: "group-foo" */ './Bar.vue')
const Baz = () => import(/* webpackChunkName: "group-foo" */ './Baz.vue')
```

## active-class 是哪个组件的属性？

设置链接激活时使用的 CSS 类名。默认值可以通过路由的构造选项 `linkActiveClass` 来全局配置。

## 怎么定义 vue-router 的动态路由? 怎么获取传过来的值

答：在 router 目录下的 index.js 文件中，对 path 属性加上 /:id，使用 router 对象的 params.id 获取。

## history 和 hash 模式的区别

* history 模式利用了 html5 的新 API `pushState` 和 `replaceState`，这两个参数会修改浏览器的访问栈，只会改变 URL，但并不会引起页面刷新。

* hash 模式使用浏览器修改 hash 不用导致页面刷新的原理，通过监听 `hashChange` 事件作出响应。

## 页面如何随路由变化

1、vue-router如何响应 路由参数 的变化？

2、完整的 vue-router 导航解析流程

3、vue-router有哪几种导航钩子（ 导航守卫 ）？

## vue-router 的几种实例方法以及参数传递

* router.beforeEach

  ```js
  router.beforeEach((to, from, next) => {
    /* 必须调用 `next` */
  })
  ```

* router.beforeResolve

  ```js
  router.beforeResolve((to, from, next) => {
    /* 必须调用 `next` */
  })
  ```

* router.afterEach

  ```js
  router.afterEach((to, from) => {})
  ```

* router.push

* router.replace

* router.go

* router.back

* router.forward

* router.getMatchedComponents

* router.resolve

* router.addRoutes

* router.onReady

* router.onError

## `<router-link></router-link>`组件及其属性

  * to

  * replace

  * tag

  * append

  * exact

  * active-class

  * event

  * exact-active-class

## vue-router实现路由懒加载（ 动态加载路由 ）

使用 Vue 的异步组件和 Webpack 的代码分割功能实现路由的懒加载

## 参考

[https://zhuanlan.zhihu.com/p/27588422](https://zhuanlan.zhihu.com/p/27588422)

