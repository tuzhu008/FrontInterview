# useMemoCache

```js
export function useMemoCache(size: number): Array<any> {
  const dispatcher = resolveDispatcher();
  // $FlowFixMe[not-a-function] This is unstable, thus optional
  return dispatcher.useMemoCache(size);
}
```