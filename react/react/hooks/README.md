# Hook 运行机制


## hooks 解决什么问题？

React 需要为共享状态逻辑提供更好的原生途径。

向着函数组件继续迈进，以更多的替代来组件。以解决类组件存在的问题。如让一些优化方案失效、不能很好的被压缩。

在 hooks 出现前，react 拆分以及复用的最小粒度是组件，一个组件可能慢慢变大，逻辑变得越来越复杂，使得这个组件难以理解、维护和测试。


## 类组件生命周期与 hooks

* getDerivedStateFromProps

* componentDidMount

* shouldComponentUpdate

* componentDidUpdate

* componentWillUnmount

## 内置 hooks

* useState
* useReducer
* useContext
* useRef
* useEffect
* useLayoutEffect
* useInsertionEffect
* useMemo
* useCallback
* useTransition
* useDeferredValue
* useDebugValue
* useId
* useSyncExternalStore
* useImperativeHandle
* use

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



所有的 Hook 只能在 React 的函数组件中调用。不要在其他 JavaScript 函数中调用。

## Hook 的分类

* Insertion

  * useInsertionEffect

* Layout

  * useImperativeHandle

  * useLayoutEffect

* Passive

  * useEffect

  * useSyncExternalStore

## 所有 Hook

* [useState](./useState.md)

## `useEffect`

`useEffect` 第一个参数能不能是 `async` 参数呢？ 


## 源码参考

所有的 hooks 都必须在函数组件中运行，以下是函数式组件更新的入口：

```js
function updateFunctionComponent(
  current: null | Fiber,
  workInProgress: Fiber,
  Component: any,
  nextProps: any,
  renderLanes: Lanes,
) {
  if (__DEV__) {
    //...
  }

  let context;
  if (!disableLegacyContext) {
    const unmaskedContext = getUnmaskedContext(workInProgress, Component, true);
    context = getMaskedContext(workInProgress, unmaskedContext);
  }

  let nextChildren;
  let hasId;
  prepareToReadContext(workInProgress, renderLanes);
  if (enableSchedulingProfiler) {
    markComponentRenderStarted(workInProgress);
  }
  if (__DEV__) {
    //...
  } else {
    nextChildren = renderWithHooks(
      current,
      workInProgress,
      Component,
      nextProps,
      context,
      renderLanes,
    );
    hasId = checkDidRenderIdHook();
  }
  if (enableSchedulingProfiler) {
    markComponentRenderStopped();
  }

  if (current !== null && !didReceiveUpdate) {
    bailoutHooks(current, workInProgress, renderLanes);
    return bailoutOnAlreadyFinishedWork(current, workInProgress, renderLanes);
  }

  if (getIsHydrating() && hasId) {
    pushMaterializedTreeId(workInProgress);
  }

  // React DevTools reads this flag.
  workInProgress.flags |= PerformedWork;
  reconcileChildren(current, workInProgress, nextChildren, renderLanes);
  return workInProgress.child;
}

```

`renderWithHooks` 函数在每次更新时都会执行，也就是说函数组件在更新时每次都会执行。

```js
export function renderWithHooks<Props, SecondArg>(
  current: Fiber | null,
  workInProgress: Fiber,
  Component: (p: Props, arg: SecondArg) => any,
  props: Props,
  secondArg: SecondArg,
  nextRenderLanes: Lanes,
): any {
  renderLanes = nextRenderLanes;
  currentlyRenderingFiber = workInProgress;

  if (__DEV__) {
    //...
  }

  workInProgress.memoizedState = null;
  workInProgress.updateQueue = null;
  workInProgress.lanes = NoLanes;

  // The following should have already been reset
  // currentHook = null;
  // workInProgressHook = null;

  // didScheduleRenderPhaseUpdate = false;
  // localIdCounter = 0;
  // thenableIndexCounter = 0;
  // thenableState = null;

  // TODO Warn if no hooks are used at all during mount, then some are used during update.
  // Currently we will identify the update render as a mount because memoizedState === null.
  // This is tricky because it's valid for certain types of components (e.g. React.lazy)

  // Using memoizedState to differentiate between mount/update only works if at least one stateful hook is used.
  // Non-stateful hooks (e.g. context) don't get added to memoizedState,
  // so memoizedState would be null during updates and mounts.
  if (__DEV__) {
    //...
  } else {
    ReactCurrentDispatcher.current =
      current === null || current.memoizedState === null
        ? HooksDispatcherOnMount
        : HooksDispatcherOnUpdate;
  }

  // In Strict Mode, during development, user functions are double invoked to
  // help detect side effects. The logic for how this is implemented for in
  // hook components is a bit complex so let's break it down.
  //
  // We will invoke the entire component function twice. However, during the
  // second invocation of the component, the hook state from the first
  // invocation will be reused. That means things like `useMemo` functions won't
  // run again, because the deps will match and the memoized result will
  // be reused.
  //
  // We want memoized functions to run twice, too, so account for this, user
  // functions are double invoked during the *first* invocation of the component
  // function, and are *not* double invoked during the second incovation:
  //
  // - First execution of component function: user functions are double invoked
  // - Second execution of component function (in Strict Mode, during
  //   development): user functions are not double invoked.
  //
  // This is intentional for a few reasons; most importantly, it's because of
  // how `use` works when something suspends: it reuses the promise that was
  // passed during the first attempt. This is itself a form of memoization.
  // We need to be able to memoize the reactive inputs to the `use` call using
  // a hook (i.e. `useMemo`), which means, the reactive inputs to `use` must
  // come from the same component invocation as the output.
  //
  // There are plenty of tests to ensure this behavior is correct.
  const shouldDoubleRenderDEV =
    __DEV__ &&
    debugRenderPhaseSideEffectsForStrictMode &&
    (workInProgress.mode & StrictLegacyMode) !== NoMode;

  shouldDoubleInvokeUserFnsInHooksDEV = shouldDoubleRenderDEV;
  let children = Component(props, secondArg);
  shouldDoubleInvokeUserFnsInHooksDEV = false;

  // Check if there was a render phase update
  if (didScheduleRenderPhaseUpdateDuringThisPass) {
    // Keep rendering until the component stabilizes (there are no more render
    // phase updates).
    children = renderWithHooksAgain(
      workInProgress,
      Component,
      props,
      secondArg,
    );
  }

  if (shouldDoubleRenderDEV) {
    //...
    } finally {
      setIsStrictModeForDevtools(false);
    }
  }

  finishRenderingHooks(current, workInProgress, Component);

  return children;
}
```

在 `renderWithHooks` 函数中，会根据当前是首次挂载还是更新来设置 `ReactCurrentDispatcher.current`。

然后此函数则会调用 `Component` 来执行函数组件。

```js
const HooksDispatcherOnMount: Dispatcher = {
  readContext,

  use,
  useCallback: mountCallback,
  useContext: readContext,
  useEffect: mountEffect,
  useImperativeHandle: mountImperativeHandle,
  useLayoutEffect: mountLayoutEffect,
  useInsertionEffect: mountInsertionEffect,
  useMemo: mountMemo,
  useReducer: mountReducer,
  useRef: mountRef,
  useState: mountState,
  useDebugValue: mountDebugValue,
  useDeferredValue: mountDeferredValue,
  useTransition: mountTransition,
  useSyncExternalStore: mountSyncExternalStore,
  useId: mountId,
};

if (enableCache) {
  (HooksDispatcherOnMount: Dispatcher).useCacheRefresh = mountRefresh;
}
if (enableUseMemoCacheHook) {
  (HooksDispatcherOnMount: Dispatcher).useMemoCache = useMemoCache;
}
if (enableUseEffectEventHook) {
  (HooksDispatcherOnMount: Dispatcher).useEffectEvent = mountEvent;
}
if (enableFormActions && enableAsyncActions) {
  (HooksDispatcherOnMount: Dispatcher).useHostTransitionStatus =
    useHostTransitionStatus;
  (HooksDispatcherOnMount: Dispatcher).useFormState = mountFormState;
}
if (enableAsyncActions) {
  (HooksDispatcherOnMount: Dispatcher).useOptimistic = mountOptimistic;
}

const HooksDispatcherOnUpdate: Dispatcher = {
  readContext,

  use,
  useCallback: updateCallback,
  useContext: readContext,
  useEffect: updateEffect,
  useImperativeHandle: updateImperativeHandle,
  useInsertionEffect: updateInsertionEffect,
  useLayoutEffect: updateLayoutEffect,
  useMemo: updateMemo,
  useReducer: updateReducer,
  useRef: updateRef,
  useState: updateState,
  useDebugValue: updateDebugValue,
  useDeferredValue: updateDeferredValue,
  useTransition: updateTransition,
  useSyncExternalStore: updateSyncExternalStore,
  useId: updateId,
};
if (enableCache) {
  (HooksDispatcherOnUpdate: Dispatcher).useCacheRefresh = updateRefresh;
}
if (enableUseMemoCacheHook) {
  (HooksDispatcherOnUpdate: Dispatcher).useMemoCache = useMemoCache;
}
if (enableUseEffectEventHook) {
  (HooksDispatcherOnUpdate: Dispatcher).useEffectEvent = updateEvent;
}
if (enableFormActions && enableAsyncActions) {
  (HooksDispatcherOnUpdate: Dispatcher).useHostTransitionStatus =
    useHostTransitionStatus;
  (HooksDispatcherOnUpdate: Dispatcher).useFormState = updateFormState;
}
if (enableAsyncActions) {
  (HooksDispatcherOnUpdate: Dispatcher).useOptimistic = updateOptimistic;
}

const HooksDispatcherOnRerender: Dispatcher = {
  readContext,

  use,
  useCallback: updateCallback,
  useContext: readContext,
  useEffect: updateEffect,
  useImperativeHandle: updateImperativeHandle,
  useInsertionEffect: updateInsertionEffect,
  useLayoutEffect: updateLayoutEffect,
  useMemo: updateMemo,
  useReducer: rerenderReducer,
  useRef: updateRef,
  useState: rerenderState,
  useDebugValue: updateDebugValue,
  useDeferredValue: rerenderDeferredValue,
  useTransition: rerenderTransition,
  useSyncExternalStore: updateSyncExternalStore,
  useId: updateId,
};
if (enableCache) {
  (HooksDispatcherOnRerender: Dispatcher).useCacheRefresh = updateRefresh;
}
if (enableUseMemoCacheHook) {
  (HooksDispatcherOnRerender: Dispatcher).useMemoCache = useMemoCache;
}
if (enableUseEffectEventHook) {
  (HooksDispatcherOnRerender: Dispatcher).useEffectEvent = updateEvent;
}
if (enableFormActions && enableAsyncActions) {
  (HooksDispatcherOnRerender: Dispatcher).useHostTransitionStatus =
    useHostTransitionStatus;
  (HooksDispatcherOnRerender: Dispatcher).useFormState = rerenderFormState;
}
if (enableAsyncActions) {
  (HooksDispatcherOnRerender: Dispatcher).useOptimistic = rerenderOptimistic;
}
```
