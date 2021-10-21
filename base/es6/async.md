# async

`async` 函数是 `Generator` 函数的语法糖。

async/await 的本身是基于Promise的。

实际上 async 函数对 Generator 函数的改进，体现在一下四点：

* async 函数自带执行器，所以执行方式和普通函数的执行方式一样。
* async 和 await 比起 `*`和 `yield` 在语义上更清楚。
* co模块约定，yield命令后面只能是 Thunk 函数或 Promise 对象，而async函数的await命令后面，可以是 Promise 对象和原始类型的值（数值、字符串和布尔值，但这时会自动转成立即 resolved 的 Promise 对象）。

* async函数的返回值是 Promise 对象，这比 Generator 函数的返回值是 Iterator 对象方便多了。你可以用then方法指定下一步的操作。

示例：

```js
function *gen(){
  yield 1;
  yield 2;
  yield 3;
}

// generator 版 gen 的 async/await 模式
async function gen() {
  await 1;
  await 2;
  await 3;
}
```

经过 Babel 编译后：

```js
function asyncGeneratorStep(gen, resolve, reject, _next, _throw, key, arg) {
  try {
    var info = gen[key](arg);
    var value = info.value;
  } catch (error) {
    reject(error);
    return;
  }
  if (info.done) {
    resolve(value);
  } else {
    Promise.resolve(value).then(_next, _throw);
  }
}

function _asyncToGenerator(fn) {
  return function() {
    var self = this,
      args = arguments;
    return new Promise(function(resolve, reject) {
      var gen = fn.apply(self, args);

      function _next(value) {
        asyncGeneratorStep(gen, resolve, reject, _next, _throw, "next", value);
      }
      function _throw(err) {
        asyncGeneratorStep(gen, resolve, reject, _next, _throw, "throw", err);
      }
      _next(undefined);
    });
  };
}

function gen() {
  return _gen.apply(this, arguments);
}

function _gen() {
  _gen = _asyncToGenerator( /*#__PURE__*/regeneratorRuntime.mark(function _callee() {
    return regeneratorRuntime.wrap(function _callee$(_context) {
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
    }, _callee);
  }));
  return _gen.apply(this, arguments);
}
```



