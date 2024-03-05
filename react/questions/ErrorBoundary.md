# ErrorBoundary

错误边界是一种 React 组件，这种组件可以捕获发生在其子组件树任何位置的 JavaScript 错误，并打印这些错误，同时展示降级 UI，而并不会渲染那些发生崩溃的子组件树。
错误边界在渲染期间、生命周期方法和整个组件树的构造函数中捕获错误

## 使用场景

## 错误边界的注意点

错误边界无法捕获一下场景中出现的错误：

* 组件外的报错

* 错误边界的父组件

* 自身的错误

  错误边界本身产生的错误，需要由它的父级错误边界来捕获。

* 异步的错误

* 事件中的错误

  错误边界无法捕获事件处理器内部的错误。

  React 不需要错误边界来捕获事件处理器中的错误。与 `render` 方法和生命周期方法不同，事件处理器不会在渲染期间触发。
  因此，如果它们抛出异常，React 仍然能够知道需要在屏幕上显示什么。

* 服务端渲染的错误

不适用的场景除了最后一个，其他都是因为没有被catch到错误。

总结：仅处理渲染子组件期间的同步错误

## 使用

* Class 组件
* 静态 `getDerivedStateFromError` 生命周期方法用于监听错误
* 实例 `componentDidCatch` 生命周期方法 用于捕获错误信息

```js
import React, { PureComponent } from  react ;
import ErrorPage from  ../ErrorPage ;

export default class ErrorBound extends PureComponent {
  constructor(props) {
    super(props);
    this.state = {
      hasError: false,
    };
  }

  static getDerivedStateFromError(error) {
    // 更新 state 使下一次渲染能够显示降级后的 UI
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    // 1、错误信息（error）
    // 2、错误堆栈（errorInfo)
    console.log(error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return <ErrorPage />;
    }
    return this.props.children;
  }
}
```