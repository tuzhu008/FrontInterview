## constructor

## getDerivedStateFromProps

会在调用 `render` 方法之前调用，并且在初始挂载及后续更新时都会被调用。它应返回一个对象来更新 `state`，如果返回 `null` 则不更新任何内容。

此方法适用于罕见的用例，即 `state` 的值在任何时候都取决于 `props`。

## render

组件重新描绘

## componentDidMount

会在组件挂载后（插入 DOM 树中）立即调用。

依赖于 DOM 节点的初始化应该放在这里。如需通过网络请求获取数据，此处是实例化请求的好地方。

这个方法是比较适合添加订阅的地方。如果添加了订阅，请不要忘记在 `componentWillUnmount()` 里取消订阅

## componentShouldUpdate

组件接受到新属性或者新状态的时候（可以返回 false，接收数据后不更新，阻止 render 调用，后面的函数不会被继续执行了）

## getSnapshotBeforeUpdate

`getSnapshotBeforeUpdate()` 在最近一次渲染输出（提交到 DOM 节点）之前调用。它使得组件能在发生更改之前从 DOM 中捕获一些信息（例如，滚动位置）。此生命周期的任何返回值将作为参数传递给 `componentDidUpdate()`。

## componentDidUpdate

`componentDidUpdate()` 会在更新后会被立即调用。首次渲染不会执行此方法。

## componentWillUnmount

会在组件卸载及销毁之前直接调用。

## getDerivedStateFromError

此生命周期会在后代组件抛出错误后被调用。 它将抛出的错误作为参数，并返回一个值以更新 `state`

## componentDidCatch

此生命周期在后代组件抛出错误后被调用。

`componentDidCatch()` 会在“提交”阶段被调用，因此允许执行副作用。

## 流程图

![生命周期](/assets/react/lifecycle.png)

### 挂载

constructor

getDerivedStateFromProps

render

componentDidMount

### 更新

getDerivedStateFromProps

componentShouldUpdate

render

getSnapshotBeforeUpdate

componentDidUpdate

### 卸载

componentWillUnmount

### 错误捕获

static getDerivedStateFromError

componentDidCatch()