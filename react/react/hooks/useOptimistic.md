# useOptimistic

```js
export function useOptimistic<S, A>(
  passthrough: S,
  reducer: ?(S, A) => S,
): [S, (A) => void] {
  const dispatcher = resolveDispatcher();
  // $FlowFixMe[not-a-function] This is unstable, thus optional
  return dispatcher.useOptimistic(passthrough, reducer);
}
```

```js
const HooksDispatcherOnMount: Dispatcher = {};
if (enableAsyncActions) {
  (HooksDispatcherOnMount: Dispatcher).useOptimistic = mountOptimistic;
}

const HooksDispatcherOnUpdate: Dispatcher = {};
if (enableAsyncActions) {
  (HooksDispatcherOnUpdate: Dispatcher).useOptimistic = updateOptimistic;
}

const HooksDispatcherOnRerender: Dispatcher = {};
if (enableAsyncActions) {
  (HooksDispatcherOnRerender: Dispatcher).useOptimistic = rerenderOptimistic;
}
```

## mountOptimistic

```js
function mountOptimistic<S, A>(
  passthrough: S,
  reducer: ?(S, A) => S,
): [S, (A) => void] {
  const hook = mountWorkInProgressHook();
  hook.memoizedState = hook.baseState = passthrough;
  const queue: UpdateQueue<S, A> = {
    pending: null,
    lanes: NoLanes,
    dispatch: null,
    // Optimistic state does not use the eager update optimization.
    lastRenderedReducer: null,
    lastRenderedState: null,
  };
  hook.queue = queue;
  // This is different than the normal setState function.
  const dispatch: A => void = (dispatchOptimisticSetState.bind(
    null,
    currentlyRenderingFiber,
    true,
    queue,
  ): any);
  queue.dispatch = dispatch;
  return [passthrough, dispatch];
}
```

## updateOptimistic

```js
function updateOptimistic<S, A>(
  passthrough: S,
  reducer: ?(S, A) => S,
): [S, (A) => void] {
  const hook = updateWorkInProgressHook();
  return updateOptimisticImpl(
    hook,
    ((currentHook: any): Hook),
    passthrough,
    reducer,
  );
}
```

```js
function updateOptimisticImpl<S, A>(
  hook: Hook,
  current: Hook | null,
  passthrough: S,
  reducer: ?(S, A) => S,
): [S, (A) => void] {
  // Optimistic updates are always rebased on top of the latest value passed in
  // as an argument. It's called a passthrough because if there are no pending
  // updates, it will be returned as-is.
  //
  // Reset the base state to the passthrough. Future updates will be applied
  // on top of this.
  hook.baseState = passthrough;

  // If a reducer is not provided, default to the same one used by useState.
  const resolvedReducer: (S, A) => S =
    typeof reducer === 'function' ? reducer : (basicStateReducer: any);

  return updateReducerImpl(hook, ((currentHook: any): Hook), resolvedReducer);
}
```

## rerenderOptimistic

```js
function rerenderOptimistic<S, A>(
  passthrough: S,
  reducer: ?(S, A) => S,
): [S, (A) => void] {
  // Unlike useState, useOptimistic doesn't support render phase updates.
  // Also unlike useState, we need to replay all pending updates again in case
  // the passthrough value changed.
  //
  // So instead of a forked re-render implementation that knows how to handle
  // render phase udpates, we can use the same implementation as during a
  // regular mount or update.
  const hook = updateWorkInProgressHook();

  if (currentHook !== null) {
    // This is an update. Process the update queue.
    return updateOptimisticImpl(
      hook,
      ((currentHook: any): Hook),
      passthrough,
      reducer,
    );
  }

  // This is a mount. No updates to process.

  // Reset the base state to the passthrough. Future updates will be applied
  // on top of this.
  hook.baseState = passthrough;
  const dispatch = hook.queue.dispatch;
  return [passthrough, dispatch];
}
```