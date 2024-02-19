# useSyncExternalStore

```js
export function useSyncExternalStore<T>(
  subscribe: (() => void) => () => void,
  getSnapshot: () => T,
  getServerSnapshot?: () => T,
): T {
  const dispatcher = resolveDispatcher();
  return dispatcher.useSyncExternalStore(
    subscribe,
    getSnapshot,
    getServerSnapshot,
  );
}
```

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
