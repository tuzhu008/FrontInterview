# useCallback

`useCallback` 的第一个参数会被直接返回，而不是进行包装，更不会对其进行调用。

## useCallback

```js
export function useCallback<T>(
  callback: T,
  deps: Array<mixed> | void | null,
): T {
  const dispatcher = resolveDispatcher();
  return dispatcher.useCallback(callback, deps);
}
```

```js
const HooksDispatcherOnMount: Dispatcher = {
  useCallback: mountCallback,
};

const HooksDispatcherOnUpdate: Dispatcher = {
  useCallback: updateCallback,
};

const HooksDispatcherOnRerender: Dispatcher = {
  useCallback: updateCallback,
};
```

## mountCallback

```js
function mountCallback<T>(callback: T, deps: Array<mixed> | void | null): T {
  const hook = mountWorkInProgressHook();
  const nextDeps = deps === undefined ? null : deps;
  hook.memoizedState = [callback, nextDeps];
  return callback;
}
```

## updateCallback

```js
function updateCallback<T>(callback: T, deps: Array<mixed> | void | null): T {
  const hook = updateWorkInProgressHook();
  const nextDeps = deps === undefined ? null : deps;
  const prevState = hook.memoizedState;
  if (nextDeps !== null) {
    const prevDeps: Array<mixed> | null = prevState[1];
    if (areHookInputsEqual(nextDeps, prevDeps)) {
      return prevState[0];
    }
  }
  hook.memoizedState = [callback, nextDeps];
  return callback;
}
```