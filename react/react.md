# React

React 特性

单向绑定和双向绑定

## 组件生命周期

**初始化阶段：**

* getDefaultProps：获取实例的默认属性

* getInitialState：获取每个实例的初始化状态

* componentWillMount：组件即将被装载、渲染到页面上

* render：组件在这里生成虚拟的 DOM 节点

* componentDidMount：组件真正在被装载之后

**运行中状态：**

* componentWillReceiveProps：组件将要接收到属性的时候调用

* shouldComponentUpdate：组件接受到新属性或者新状态的时候（可以返回 false，接收数据后不更新，阻止 render 调用，后面的函数不会被继续执行了）

* componentWillUpdate：组件即将更新不能修改属性和状态

* render：组件重新描绘

* componentDidUpdate：组件已经更新

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