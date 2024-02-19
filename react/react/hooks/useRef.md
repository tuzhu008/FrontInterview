# useRef

```js
export function useRef<T>(initialValue: T): {current: T} {
  const dispatcher = resolveDispatcher();
  return dispatcher.useRef(initialValue);
}
```

```js
const HooksDispatcherOnMount: Dispatcher = {
  useRef: mountRef,
};

const HooksDispatcherOnUpdate: Dispatcher = {
  useRef: updateRef,
};

const HooksDispatcherOnRerender: Dispatcher = {
  useRef: updateRef,
};
```

## mountRef

```js
function mountRef<T>(initialValue: T): {current: T} {
  const hook = mountWorkInProgressHook();
  if (enableUseRefAccessWarning) {
    if (__DEV__) {
      //...
    } else {
      const ref = {current: initialValue};
      hook.memoizedState = ref;
      return ref;
    }
  } else {
    const ref = {current: initialValue};
    hook.memoizedState = ref;
    return ref;
  }
}
```



## updateRef

```js
function updateRef<T>(initialValue: T): {current: T} {
  const hook = updateWorkInProgressHook();
  return hook.memoizedState;
}
```