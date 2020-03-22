# React

React 特性

单向绑定和双向绑定

## 组件生命周期

**挂载阶段：**

当组件实例被创建并插入 DOM 中时，其生命周期调用顺序如下：

* `constructor()`：获取实例的默认属性

* `static getDerivedStateFromProps()`

  会在调用 `render` 方法之前调用，并且在初始挂载及后续更新时都会被调用。它应返回一个对象来更新 `state`，如果返回 `null` 则不更新任何内容。

  此方法适用于罕见的用例，即 `state` 的值在任何时候都取决于 `props`。

* `componentWillMount`：组件即将被装载、渲染到页面上

  现在名字也叫 `UNSAFE_componentWillMount()`，该方法虽然在 17 上仍然可以使用，但已过时，在未来会被删除。

  有一个常见的误解是，在 `componentWillMount` 中获取数据可以避免第一次渲染为空的状态。实际上，这是不对的，因为 React 总是在 `componentWillMount` 之后立即执行 render。如果在 `componentWillMount` 触发时数据不可用，那么第一次 `render` 仍然会显示加载的状态，而不管你在哪里初始化获取数据。这就是为什么在绝大多数情况下，将获取数据移到 `componentDidMount` 没有明显效果的原因。

* `render`：组件在这里生成虚拟的 DOM 节点

* `componentDidMount`：组件真正在被装载之后

  会在组件挂载后（插入 DOM 树中）立即调用。

  依赖于 DOM 节点的初始化应该放在这里。如需通过网络请求获取数据，此处是实例化请求的好地方。

  这个方法是比较适合添加订阅的地方。如果添加了订阅，请不要忘记在 `componentWillUnmount()` 里取消订阅

**更新：**

当组件的 props 或 state 发生变化时会触发更新。组件更新的生命周期调用顺序如下：

* `static getDerivedStateFromProps()`

* `shouldComponentUpdate`

  组件接受到新属性或者新状态的时候（可以返回 false，接收数据后不更新，阻止 render 调用，后面的函数不会被继续执行了）

* `componentWillUpdate`

  组件即将更新不能修改属性和状态

  现名为 `UNSAFE_componentWillUpdate`，已过时，不该使用

* `render`：组件重新描绘

* `getSnapshotBeforeUpdate`

  `getSnapshotBeforeUpdate()` 在最近一次渲染输出（提交到 DOM 节点）之前调用。
  它使得组件能在发生更改之前从 DOM 中捕获一些信息（例如，滚动位置）。
  此生命周期的任何返回值将作为参数传递给 `componentDidUpdate()`。

* `componentDidUpdate`：组件已经更新

**销毁阶段：**

* componentWillUnmount：组件即将销毁

虚拟 DOM，虚拟 DOM 的 diff 算法

找出当前节点的所有父节点，打印他们的类

单向数据流

Vue 和 React 比较？原理，区别，亮点，作用

## `key` 的作用

Keys 是 React 用于追踪哪些列表中元素被修改、被添加或者被移除的辅助标识。

在开发过程中，我们需要保证某个元素的 key 在其同级元素中具有唯一性。在 React Diff 算法中 React 会借助元素的 Key 值来判断该元素是新近创建的还是被移动而来的元素，从而减少不必要的元素重渲染。此外，React 还需要借助 Key 值来判断元素与本地状态的关联关系，因此我们绝不可忽视转换函数中 Key 的重要性。

## 调用 `setState` 之后发生了什么？

在代码中调用 `setState` 函数之后，React 会将传入的参数对象与组件当前的状态合并，然后触发所谓的调和过程（Reconciliation）。经过调和过程，React 会以相对高效的方式根据新的状态构建 React 元素树并且着手重新渲染整个 UI 界面。在 React 得到元素树之后，React 会自动计算出新的树与老树的节点差异，然后根据差异对界面进行最小化重渲染。在差异计算算法中，React 能够相对精确地知道哪些位置发生了改变以及应该如何改变，这就保证了按需更新，而不是全部重新渲染。