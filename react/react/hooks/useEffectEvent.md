# useEffectEvent

```js
export function useEffectEvent<Args, F: (...Array<Args>) => mixed>(
  callback: F,
): F {
  const dispatcher = resolveDispatcher();
  // $FlowFixMe[not-a-function] This is unstable, thus optional
  return dispatcher.useEffectEvent(callback);
}
```

```js
const HooksDispatcherOnMount: Dispatcher = {};
if (enableUseEffectEventHook) {
  (HooksDispatcherOnMount: Dispatcher).useEffectEvent = mountEvent;
}

const HooksDispatcherOnUpdate: Dispatcher = {};
if (enableUseEffectEventHook) {
  (HooksDispatcherOnUpdate: Dispatcher).useEffectEvent = updateEvent;
}

const HooksDispatcherOnRerender: Dispatcher = {};
if (enableUseEffectEventHook) {
  (HooksDispatcherOnRerender: Dispatcher).useEffectEvent = updateEvent;
}
```

## mountEvent

```js
function mountEvent<Args, Return, F: (...Array<Args>) => Return>(
  callback: F,
): F {
  const hook = mountWorkInProgressHook();
  const ref = {impl: callback};
  hook.memoizedState = ref;
  // $FlowIgnore[incompatible-return]
  return function eventFn() {
    if (isInvalidExecutionContextForEventFunction()) {
      throw new Error(
        "A function wrapped in useEffectEvent can't be called during rendering.",
      );
    }
    return ref.impl.apply(undefined, arguments);
  };
}
```

## updateEvent

```js
function updateEvent<Args, Return, F: (...Array<Args>) => Return>(
  callback: F,
): F {
  const hook = updateWorkInProgressHook();
  const ref = hook.memoizedState;
  useEffectEventImpl({ref, nextImpl: callback});
  // $FlowIgnore[incompatible-return]
  return function eventFn() {
    if (isInvalidExecutionContextForEventFunction()) {
      throw new Error(
        "A function wrapped in useEffectEvent can't be called during rendering.",
      );
    }
    return ref.impl.apply(undefined, arguments);
  };
}
```

```js
function useEffectEventImpl<Args, Return, F: (...Array<Args>) => Return>(
  payload: EventFunctionPayload<Args, Return, F>,
) {
  currentlyRenderingFiber.flags |= UpdateEffect;
  let componentUpdateQueue: null | FunctionComponentUpdateQueue =
    (currentlyRenderingFiber.updateQueue: any);
  if (componentUpdateQueue === null) {
    componentUpdateQueue = createFunctionComponentUpdateQueue();
    currentlyRenderingFiber.updateQueue = (componentUpdateQueue: any);
    componentUpdateQueue.events = [payload];
  } else {
    const events = componentUpdateQueue.events;
    if (events === null) {
      componentUpdateQueue.events = [payload];
    } else {
      events.push(payload);
    }
  }
}
```