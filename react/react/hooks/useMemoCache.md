# useMemoCache

```js
export function useMemoCache(size: number): Array<any> {
  const dispatcher = resolveDispatcher();
  // $FlowFixMe[not-a-function] This is unstable, thus optional
  return dispatcher.useMemoCache(size);
}
```

```js
const HooksDispatcherOnMount: Dispatcher = {};
if (enableUseMemoCacheHook) {
  (HooksDispatcherOnMount: Dispatcher).useMemoCache = useMemoCache;
}

const HooksDispatcherOnUpdate: Dispatcher = {};
if (enableUseMemoCacheHook) {
  (HooksDispatcherOnUpdate: Dispatcher).useMemoCache = useMemoCache;
}

const HooksDispatcherOnRerender: Dispatcher = {};
if (enableUseMemoCacheHook) {
  (HooksDispatcherOnRerender: Dispatcher).useMemoCache = useMemoCache;
}
```

## useMemoCache

```js
function useMemoCache(size: number): Array<any> {
  let memoCache = null;
  // Fast-path, load memo cache from wip fiber if already prepared
  let updateQueue: FunctionComponentUpdateQueue | null =
    (currentlyRenderingFiber.updateQueue: any);
  if (updateQueue !== null) {
    memoCache = updateQueue.memoCache;
  }
  // Otherwise clone from the current fiber
  if (memoCache == null) {
    const current: Fiber | null = currentlyRenderingFiber.alternate;
    if (current !== null) {
      const currentUpdateQueue: FunctionComponentUpdateQueue | null =
        (current.updateQueue: any);
      if (currentUpdateQueue !== null) {
        const currentMemoCache: ?MemoCache = currentUpdateQueue.memoCache;
        if (currentMemoCache != null) {
          memoCache = {
            data: currentMemoCache.data.map(array => array.slice()),
            index: 0,
          };
        }
      }
    }
  }
  // Finally fall back to allocating a fresh instance of the cache
  if (memoCache == null) {
    memoCache = {
      data: [],
      index: 0,
    };
  }
  if (updateQueue === null) {
    updateQueue = createFunctionComponentUpdateQueue();
    currentlyRenderingFiber.updateQueue = updateQueue;
  }
  updateQueue.memoCache = memoCache;

  let data = memoCache.data[memoCache.index];
  if (data === undefined) {
    data = memoCache.data[memoCache.index] = new Array(size);
    for (let i = 0; i < size; i++) {
      data[i] = REACT_MEMO_CACHE_SENTINEL;
    }
  } else if (data.length !== size) {
    // TODO: consider warning or throwing here
    if (__DEV__) {
      //...
    }
  }
  memoCache.index++;
  return data;
}
```
