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

* 如果你不想改URL，就不用改。相反，设置 window.location = "#foo";在当前哈希不是 #foo 时， 才能创建新的历史记录项。

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

$route 表示当前激活的路由的状态信息，包含了当前 URL 解析得到的信息，还有 URL 匹配到的路由记录 (route records)。

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

* history

  history 模式利用了的 history 的两个接口。这两个接口为 HTML5 新增：

  ```
  window.history.pushState(stateObject, title, URL)
  window.history.replaceState(stateObject, title, URL)
  ```




## vue-router 实现路由懒加载（ 动态加载路由 ）

## active-class 是哪个组件的属性？

设置链接激活时使用的 CSS 类名。默认值可以通过路由的构造选项 `linkActiveClass` 来全局配置。

**怎么定义 vue-router 的动态路由? 怎么获取传过来的值**

答：在 router 目录下的 index.js 文件中，对 path 属性加上 /:id，使用 router 对象的 params.id 获取。

