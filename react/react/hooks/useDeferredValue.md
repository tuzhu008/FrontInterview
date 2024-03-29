# useDeferredValue

```js
export function useDeferredValue<T>(value: T, initialValue?: T): T {
  const dispatcher = resolveDispatcher();
  return dispatcher.useDeferredValue(value, initialValue);
}
```

```js
const HooksDispatcherOnMount: Dispatcher = {
  useDeferredValue: mountDeferredValue,
};

const HooksDispatcherOnUpdate: Dispatcher = {
  useDeferredValue: updateDeferredValue,
};

const HooksDispatcherOnRerender: Dispatcher = {
  useDeferredValue: rerenderDeferredValue,
};
```

## mountDeferredValue

```js
function mountDeferredValue<T>(value: T, initialValue?: T): T {
  const hook = mountWorkInProgressHook();
  return mountDeferredValueImpl(hook, value, initialValue);
}
```

```js
function mountDeferredValueImpl<T>(hook: Hook, value: T, initialValue?: T): T {
  if (
    enableUseDeferredValueInitialArg &&
    // When `initialValue` is provided, we defer the initial render even if the
    // current render is not synchronous.
    initialValue !== undefined &&
    // However, to avoid waterfalls, we do not defer if this render
    // was itself spawned by an earlier useDeferredValue. Check if DeferredLane
    // is part of the render lanes.
    !includesSomeLane(renderLanes, DeferredLane)
  ) {
    // Render with the initial value
    hook.memoizedState = initialValue;

    // Schedule a deferred render to switch to the final value.
    const deferredLane = requestDeferredLane();
    currentlyRenderingFiber.lanes = mergeLanes(
      currentlyRenderingFiber.lanes,
      deferredLane,
    );
    markSkippedUpdateLanes(deferredLane);

    return initialValue;
  } else {
    hook.memoizedState = value;
    return value;
  }
}
```

## updateDeferredValue

```js
function updateDeferredValue<T>(value: T, initialValue?: T): T {
  const hook = updateWorkInProgressHook();
  const resolvedCurrentHook: Hook = (currentHook: any);
  const prevValue: T = resolvedCurrentHook.memoizedState;
  return updateDeferredValueImpl(hook, prevValue, value, initialValue);
}
```

```js
function updateDeferredValueImpl<T>(
  hook: Hook,
  prevValue: T,
  value: T,
  initialValue?: T,
): T {
  if (is(value, prevValue)) {
    // The incoming value is referentially identical to the currently rendered
    // value, so we can bail out quickly.
    return value;
  } else {
    // Received a new value that's different from the current value.

    // Check if we're inside a hidden tree
    if (isCurrentTreeHidden()) {
      // Revealing a prerendered tree is considered the same as mounting new
      // one, so we reuse the "mount" path in this case.
      const resultValue = mountDeferredValueImpl(hook, value, initialValue);
      // Unlike during an actual mount, we need to mark this as an update if
      // the value changed.
      if (!is(resultValue, prevValue)) {
        markWorkInProgressReceivedUpdate();
      }
      return resultValue;
    }

    const shouldDeferValue = !includesOnlyNonUrgentLanes(renderLanes);
    if (shouldDeferValue) {
      // This is an urgent update. Since the value has changed, keep using the
      // previous value and spawn a deferred render to update it later.

      // Schedule a deferred render
      const deferredLane = requestDeferredLane();
      currentlyRenderingFiber.lanes = mergeLanes(
        currentlyRenderingFiber.lanes,
        deferredLane,
      );
      markSkippedUpdateLanes(deferredLane);

      // Reuse the previous value. We do not need to mark this as an update,
      // because we did not render a new value.
      return prevValue;
    } else {
      // This is not an urgent update, so we can use the latest value regardless
      // of what it is. No need to defer it.

      // Mark this as an update to prevent the fiber from bailing out.
      markWorkInProgressReceivedUpdate();
      hook.memoizedState = value;
      return value;
    }
  }
}
```

## rerenderDeferredValue

```js
function rerenderDeferredValue<T>(value: T, initialValue?: T): T {
  const hook = updateWorkInProgressHook();
  if (currentHook === null) {
    // This is a rerender during a mount.
    return mountDeferredValueImpl(hook, value, initialValue);
  } else {
    // This is a rerender during an update.
    const prevValue: T = currentHook.memoizedState;
    return updateDeferredValueImpl(hook, prevValue, value, initialValue);
  }
}
```