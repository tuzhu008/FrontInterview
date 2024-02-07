# useContext

```js
export function useContext<T>(Context: ReactContext<T>): T {
  const dispatcher = resolveDispatcher();
  return dispatcher.useContext(Context);
}
```