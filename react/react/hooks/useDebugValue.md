# useDebugValue

```js
export function useDebugValue<T>(
  value: T,
  formatterFn: ?(value: T) => mixed,
): void {
  if (__DEV__) {
    const dispatcher = resolveDispatcher();
    return dispatcher.useDebugValue(value, formatterFn);
  }
}
```

```js
const HooksDispatcherOnMount: Dispatcher = {
  useDebugValue: mountDebugValue,
};

const HooksDispatcherOnUpdate: Dispatcher = {
  useDebugValue: updateDebugValue,
};

const HooksDispatcherOnRerender: Dispatcher = {
  useDebugValue: updateDebugValue,
};
```

## mountDebugValue

```js
function mountDebugValue<T>(value: T, formatterFn: ?(value: T) => mixed): void {
  // This hook is normally a no-op.
  // The react-debug-hooks package injects its own implementation
  // so that e.g. DevTools can display custom hook values.
}
```

## updateDebugValue

```js
const updateDebugValue = mountDebugValue;
```