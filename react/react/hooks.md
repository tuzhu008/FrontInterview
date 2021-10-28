# Hooks

## useEffect

### 函数组件中如何执行异步请求操作？

在函数组件中执行异步请求很容易，异步请求要么获取数据，要么保存数据。获取数据意味着需要有地方接收数据，而保存数据，意味着要进行写库操作。老版的 React 的函数式组件也称为 Pure 组件，很纯粹，是一个纯函数，不带任何有副作用的操作。因此在老版 React 的函数式组件中执行异步请求是没有意义的，也影响性能。

但到了新版 React 中，可以使用 hooks 为函数式组件提供更强大的功能。比如状态存储，执行副作用。这里，接收数据成为可能，执行副作用的保存操作也称为可能。

```js
useEffect(() => {
  const fetchData = async () => {
    const result = await axios('请求地址');
    // 执行其他操作...
  };

  fetchData();
}, []);
```

### 如何用 useEffect 实现 class 组件的生命周期

1. useEffect实现 `componentDidMount` 的效果：

   ```js
    useEffect(() => {

    }, []);
   ```

2. useEffect 实现 `componentWillUnMount` 的效果：

   ```js
   useEffect(() => {
    // 该函数如 `componentWillUnMount` 在组件即将卸载时调用
    return () => {};
   }, []);
   ```

3. 实现 `componentDidUpdate`:

   ```js
   useEffect(() => {
    // user.id 变化时
   }, [user.id]);
   ```

## useMemo 与 useCallback 区别？

共同点：

* 接收参数相同。都接收两个参数，第一个函数，第二个为依赖数据组成的数组
* 懒计算。都只会在依赖数据发生变化时才重新计算。

不同点：

* 返回值不同。`useMemo` 的返回值是传入的回调函数运行后的返回值；而 `useCallback` 则是返回传入的回调函数。



