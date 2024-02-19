# useCacheRefresh

```js
export function useCacheRefresh(): <T>(?() => T, ?T) => void {
  const dispatcher = resolveDispatcher();
  // $FlowFixMe[not-a-function] This is unstable, thus optional
  return dispatcher.useCacheRefresh();
}
```

```js
const HooksDispatcherOnMount: Dispatcher = {};

if (enableCache) {
  (HooksDispatcherOnMount: Dispatcher).useCacheRefresh = mountRefresh;
}

const HooksDispatcherOnUpdate: Dispatcher = {};
if (enableCache) {
  (HooksDispatcherOnUpdate: Dispatcher).useCacheRefresh = updateRefresh;
}

const HooksDispatcherOnRerender: Dispatcher = {};
if (enableCache) {
  (HooksDispatcherOnRerender: Dispatcher).useCacheRefresh = updateRefresh;
}
```

## mountRefresh

```js
function mountRefresh(): any {
  const hook = mountWorkInProgressHook();
  const refresh = (hook.memoizedState = refreshCache.bind(
    null,
    currentlyRenderingFiber,
  ));
  return refresh;
}
```

## updateRefresh

```js
function updateRefresh(): any {
  const hook = updateWorkInProgressHook();
  return hook.memoizedState;
}
```