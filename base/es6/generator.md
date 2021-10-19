# Generator

Generator 函数是 ES6 提供的一种异步编程解决方案，语法行为与传统函数完全不同。

执行 Generator 函数会返回一个遍历器对象。

```js
function* myGenerator () {
  yield 'my';
  yield 'name';
  yield 'is';
  return 'wahaha';
}

const mg = myGenerator();
mg.next();
```

`yield` 表达式就是暂停标志。

`yield` 表达式只能用在 `Generator` 函数里面，用在其他地方都会报错。

`yield` 表达式如果用在另一个表达式之中，必须放在圆括号里面。

```js
function* demo() {
  console.log('Hello' + yield); // SyntaxError
  console.log('Hello' + yield 123); // SyntaxError

  console.log('Hello' + (yield)); // OK
  console.log('Hello' + (yield 123)); // OK
}
```

由于 `next` 方法的参数表示上一个 `yield` 表达式的返回值。

调用 Generator 函数和调用普通函数一样，在函数名后面加上`()`即可，但是 Generator 函数不会像普通函数一样立即执行，而是返回一个指向内部状态对象的指针，所以要调用遍历器对象Iterator 的 `next` 方法，指针就会从函数头部或者上一次停下来的地方开始执行。

