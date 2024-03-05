# React Hook

## react hooks，它带来了那些便利

* React hooks 解决了状态及逻辑复用的问题，将 React 复用的最小单位从组件降为了逻辑。
* React hooks 使函数式组件有了状态，可以完全替代 Class 组件，模板代码也随之变少。
* 使用 React hooks，可以避免一些由类组件导致的问题，如测试，tree shaking，逻辑耦合。
* 更符合 React 本身的思想，函数式、组件化。

## react hooks 的原理是什么

* React Hooks 的实现原理是基于函数式编程的思想来实现的，通过函数的调用和闭包的机制来管理状态
* 每个 Hook 都是一个函数，它可以对组件的状态进行操作或者访问 React 的其他功能。
* 当组件渲染时，React 会根据每个 Hook 调用的顺序来维护内部的状态并执行相应的操作。
* 组件的所有 Hook，在内部以单向链表的方式进行存储。更新时会检查依赖，并根据变化来更新状态链中的对应状态。

## react hooks 中如何模拟 componentDidMount

```js
useEffect(callback, []);
```

## react hooks 如何替代或部分替代 redux 功能

redux 在 react 中主要作为中心存储使用，用与在组件间共享数据。

* redux 的 reducer 通过 `useReducer` 模拟
* 父子组件间的状态共享可以使用 `useContext` 模拟
* 异步数据，可以通过将请求封装为 hook 来完成，还可以配以缓存。或者再进一步，使用 `useSyncExternalStore` 结合外部存储。

## React hooks 中 useCallback 的使用场景是什么

作为props传递的函数，为了提供稳定的 props，以避免不必要的重复渲染。

## useEffect 中如何使用 async/await

```js
async function fetchData() {
  let response = await fetch("api/data");
  response = await res.json();
  dataSet(response);
}
 
useEffect(() => {
  fetchData();
}, []);
```

## 为什么不能在条件表达式或者循环里面定义 react hooks

React 依靠顺序来判断状态与hook 的对应关系。

## 在 React hooks 中如何模拟 forceUpdate

```js
const [ignored, forceUpdate] = useReducer((x) => x + 1, 0);
 
function handleClick() {
  forceUpdate();
}
```


```js
import {useState,useCallback} from 'react';
const [state,setState] = useState({})
const handleForceUpdate = useCallback(() => {
    setState({})
},[])
```

## React.useMemo 与 React.useCallback 是如何进行性能优化的

## 在 React Hooks 中实现 usePreviouseValue 取上次渲染的值

```js
import { useRef } from "react";
 
type ShouldUpdateFunc<T> = (prev: T | undefined, next: T) => boolean;
 
const defalutShouldUpdate = <T>(prev?: T, next?: T) => prev !== next;
 
function usePrevious<T>(
  state: T,
  shouldUpdateFun: ShouldUpdateFunc<T> = defalutShouldUpdate,
): T | undefined {
  const prev = useRef<T>();
  const cur = useRef<T>();
 
  if (shouldUpdateFun(cur.current, state)) {
    prev.current = cur.current;
    cur.current = state;
  }
  return prev.current;
}
 
export default usePrevious;
```

## 在 react 中，以下父子组件的 useEffect/useLayoutEffect 顺序如何

```js
import { useEffect, useLayoutEffect } from "react";
import "./App.css";
 
function Child() {
  console.log("Child: Render");
 
  useEffect(() => {
    console.log("Child: useEffect");
  });
 
  useLayoutEffect(() => {
    console.log("Child: useLayoutEffect");
  });
 
  return <div className="App">Child</div>;
}
 
function App() {
  console.log("App: render");
 
  useEffect(() => {
    console.log("App: useEffect");
  });
 
  useLayoutEffect(() => {
    console.log("App: useLayoutEffect");
  });
 
  return (
    <div className="App">
      App
      <Child />
    </div>
  );
}
 
export default App;
```

```js
App: render
Child: Render
Child: useLayoutEffect
App: useLayoutEffect
Child: useEffect
App: useEffect
```