# 函数式编程

## 高阶函数

高阶函数是指接收函数作为参数或者返回值是函数的函数。

## 柯里化

柯里化（Currying），又称部分求值（Partial Evaluation），是把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术。核心思想是把多参数传入的函数拆成单参数（或部分）函数，内部再返回调用下一个单参数（或部分）函数，依次处理剩余的参数。

js 中的柯里化需要依赖函数的 `length` 属性，该属性指明函数需要的参数个数。

```js
function curry(fn) {
  if (fn.length === 0) {
    return fn;
  }

  function _curry(fn, args = []) {
    return function (..._args) {
      // 退出条件
      if (args.length + _args.length === fn.length) {
        return fn(...args.concat(_args));
      }

      return _curry(fn, args.concat(_args))
    }
  }

  return _curry(fn)
}
```