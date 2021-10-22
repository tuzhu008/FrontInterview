# Generator

Generator 函数是 ES6 提供的一种异步编程解决方案，语法行为与传统函数完全不同。

执行生成器函数会返回一个 Generator 对象。

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

示例：

```js
function *gen(){
  yield 1;
  yield 2;
  yield 3;
}
```

经过 Babel 编译后：

```js
var _marked = /*#__PURE__*/regeneratorRuntime.mark(gen);

function gen() {
  return regeneratorRuntime.wrap(function gen$(_context) {
    while (1) {
      switch (_context.prev = _context.next) {
        case 0:
          _context.next = 2;
          return 1;

        case 2:
          _context.next = 4;
          return 2;

        case 4:
          _context.next = 6;
          return 3;

        case 6:
        case "end":
          return _context.stop();
      }
    }
  }, _marked);
}
```

mark：

```js
exports.mark = function(genFun) {
  if (Object.setPrototypeOf) {
    Object.setPrototypeOf(genFun, GeneratorFunctionPrototype);
  } else {
    genFun.__proto__ = GeneratorFunctionPrototype;
    define(genFun, toStringTagSymbol, "GeneratorFunction");
  }
  genFun.prototype = Object.create(Gp);
  return genFun;
};
```

`mark` 方法向 `genFun` 绑定了一系列原型。

```js
function wrap(innerFn, outerFn, self, tryLocsList) {
  // If outerFn provided and outerFn.prototype is a Generator, then outerFn.prototype instanceof Generator.
  var protoGenerator = outerFn && outerFn.prototype instanceof Generator ? outerFn : Generator;
  var generator = Object.create(protoGenerator.prototype);
  var context = new Context(tryLocsList || []);

  // The ._invoke method unifies the implementations of the .next,
  // .throw, and .return methods.
  generator._invoke = makeInvokeMethod(innerFn, self, context);

  return generator;
}
```

从上面来看，`outerFn` 也就是 `_marked` ，即绑定了一系列原型后的 `genFun`。

generator 变量可以简化为：

```js
var generator = {
  __invoke(method, arg) {
    // do something

    var value = context.done ? undefined : gen$(context);

    return {
      value,
      done: context.done
    };
  },

  next(arg) {
    return this.__invoke('next', arg);
  }
}
```

context 变量可以简化为：

```js
var context = {
  next:0,
  prev: 0,
  done: false,
  stop: function stop () {
    this.done = true
  }
}
```

context 作为上下文对象，用于存储当前状态，并封装了操作这些状态的方法。

