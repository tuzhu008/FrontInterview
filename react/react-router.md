# React Router

## 什么是React 路由？

React 路由是一个构建在 React 之上的强大的路由库，它有助于向应用程序添加新的屏幕和流。这使 URL 与网页上显示的数据保持同步。它负责维护标准化的结构和行为，并用于开发单页 Web 应用。 React 路由有一个简单的API。

## 为什么React Router v4中使用 switch 关键字 ？

虽然 `<div>` 用于封装 Router 中的多个路由，当你想要仅显示要在多个定义的路线中呈现的单个路线时，可以使用 “switch” 关键字。使用时，`<switch>` 标记会按顺序将已定义的 URL 与已定义的路由进行匹配。找到第一个匹配项后，它将渲染指定的路径。从而绕过其它路线。

## 为什么需要 React 中的路由？

Router 用于定义多个路由，当用户定义特定的 URL 时，如果此 URL 与 Router 内定义的任何 “路由” 的路径匹配，则用户将重定向到该特定路由。所以基本上我们需要在自己的应用中添加一个 Router 库，允许创建多个路由，每个路由都会向我们提供一个独特的视图

```html
<switch>
    <route exact path=’/’ component={Home}/>
    <route path=’/posts/:id’ component={Newpost}/>
    <route path=’/posts’   component={Post}/>
</switch>
```

## 列出 React Router 的优点。

* 像 React 基于组件一样，在 React Router v4 中，API 是 'All About Components'。可以将 Router 可视化为单个根组件（`<BrowserRouter>`），其中我们将特定的子路由（`<route>`）包起来。

* 无需手动设置历史值：在 React Router v4 中，我们要做的就是将路由包装在 `<BrowserRouter>` 组件中。

* 包是分开的：共有三个包，分别用于 Web、Native 和 Core。这使我们应用更加紧凑。基于类似的编码风格很容易进行切换。

## React Router与常规路由有何不同？

| 主题 | 常规路由 | React 路由 |
| :--- | :--- | :--- |
| 参与的页面 | 每个视图对应一个新文件 | 只涉及单个HTML页面 |
| URL 更改 | HTTP 请求被发送到服务器并且接收相应的 HTML 页面 | 仅更改历史记录属性 |
| 体验 | 用户实际在每个视图的不同页面切换 | 用户认为自己正在不同的页面间切换 |
