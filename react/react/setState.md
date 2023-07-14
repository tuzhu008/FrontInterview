legacy模式下：命中batchedUpdates时是异步 未命中batchedUpdates时是同步的

concurrent模式下：都是异步的

在 18 以前的版本中，会根据 `executionContext` 标志来判断需要走的流程，如果 `executionContext` 不存在，则会进行同步更新。

React 应用被分为了三个阶段，挂载、更新、卸载。所有的更新，本质都是通过 `setState` 来触发，子组件的 `props` 也是某个祖先组件的 `state`。这是因为，只有调用 React 的内部 api 才能触发 React 内部的。

当 React 应用完成挂载后，有两种行为可以触发应用的更新：一是程序自动触发，如 setTimeout、Promise 等等；二是用户触发的更新，如点击事件。程序自动触发的更新，React 对其不能有完全的控制权，因此在 18 以下的版本中，由程序触发的更新不会设置 `executionContext`，因此会走同步更新的逻辑。而用户触发的更新是通过事件系统进行响应的，React 基于原生的 DOM 事件实现了自己的事件系统，因此其对用户触发的更新具有完全的控制权，在处理由用户触发的更新时，`executionContext` 会被设置为 `EventContext`。

从 18 开始，`setState` 触发的逻辑始终都会被放到一个异步的回调中。