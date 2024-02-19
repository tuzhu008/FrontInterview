# use

```js
export function use<T>(usable: Usable<T>): T {
  const dispatcher = resolveDispatcher();
  return dispatcher.use(usable);
}
```

```js
const HooksDispatcherOnMount: Dispatcher = {
  use,
};

const HooksDispatcherOnUpdate: Dispatcher = {
  use,
};

const HooksDispatcherOnRerender: Dispatcher = {
  use,
};
```

## use

```js
function use<T>(usable: Usable<T>): T {
  if (usable !== null && typeof usable === 'object') {
    // $FlowFixMe[method-unbinding]
    if (typeof usable.then === 'function') {
      // This is a thenable.
      const thenable: Thenable<T> = (usable: any);
      return useThenable(thenable);
    } else if (usable.$$typeof === REACT_CONTEXT_TYPE) {
      const context: ReactContext<T> = (usable: any);
      return readContext(context);
    }
  }

  // eslint-disable-next-line react-internal/safe-string-coercion
  throw new Error('An unsupported type was passed to use(): ' + String(usable));
}
```