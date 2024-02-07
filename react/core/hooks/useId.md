# useId

```js
export function useId(): string {
  const dispatcher = resolveDispatcher();
  return dispatcher.useId();
}
```