## call

**`call` 的 ts 描述**：

```ts
/**
 * Calls a method of an object, substituting another object for the current object.
 * @param thisArg The object to be used as the current object.
 * @param argArray A list of arguments to be passed to the method.
 */
call(this: Function, thisArg: any, ...argArray: any[]): any;
```

**实现**：

```js
Function.prototype.myCall = function (thisArg) {
  const property = Symbol("func");
  const context = thisArg || window;
  const args = [...arguments].slice(1);

  Object.defineProperty(context, property, {
    value: this,
    writable: true,
    configurable: true,
  });

  context[property](...args);
  delete context[property];
};
```


## apply

**`apply` 的 ts 描述**：

```ts
/**
 * Calls the function, substituting the specified object for the this value of the function, and the specified array for the arguments of the function.
 * @param thisArg The object to be used as the this object.
 * @param argArray A set of arguments to be passed to the function.
 */
apply(this: Function, thisArg: any, argArray?: any): any;
```

**实现**：

```js
Function.prototype.myApply = function (thisArg, args = []) {
  const property = Symbol("func");
  const context = thisArg || window;

  Object.defineProperty(context, property, {
    value: this,
    writable: true,
    configurable: true,
  });

  context[property](...args);
  delete context[property];
};
```

## bind

**`bind` 的 ts 描述**：

```ts
/**
 * For a given function, creates a bound function that has the same body as the original function.
 * The this object of the bound function is associated with the specified object, and has the specified initial parameters.
 * @param thisArg An object to which the this keyword can refer inside the new function.
 * @param argArray A list of arguments to be passed to the new function.
 */
bind(this: Function, thisArg: any, ...argArray: any[]): any;
```

**实现**：

```js
Function.prototype.myBind = function (thisArg) {
  const context = thisArg || window;
  const boundArgs = [...arguments].slice(1);

  const self =  this;

  return function () {
    const newArg = [...arguments];
    return self.apply(context, boundArgs.concat(newArg));
  }
};
```