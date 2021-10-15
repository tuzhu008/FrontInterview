# Promise

## promise原理

promise 的核心原理其实就是一个发布订阅模式，通过两个队列来缓存成功的回调（`onResolve`）和失败的回调（`onReject`）。

promise 有三种状态：

* `pending`：进行中

* `fulfilled`：已成功

* `rejected`：已失败

一旦状态改变，就不会再变，任何时候都可以得到这个结果。

promise 提供了一个 `then` 方法用来访问其当前或最终值或理由。`then` 也会返回一个 promise，因此可以进行链式调用。

## 实现一个

以下示例已通过所有测试用例：

```js
// 先定义三个常量表示状态
var PENDING = 'pending';
var FULFILLED = 'fulfilled';
var REJECTED = 'rejected';

function resolvePromise(promise, x, resolve, reject) {
  // 如果 promise 和 x 指向同一对象，以 TypeError 为据因拒绝执行 promise
  // 这是为了防止死循环
  if (promise === x) {
    return reject(new TypeError('The promise and the return value are the same'));
  }

  if (x instanceof MyPromise) {
    // 如果 x 为 Promise ，则使 promise 接受 x 的状态
    // 也就是继续执行x，如果执行的时候拿到一个y，还要继续解析y
    // 这个if跟下面判断then然后拿到执行其实重复了，可有可无
    x.then(function (y) {
      resolvePromise(promise, y, resolve, reject);
    }, reject);
  }
  // 如果 x 为对象或者函数
  else if (typeof x === 'object' || typeof x === 'function') {
    // 这个坑是跑测试的时候发现的，如果x是null，应该直接resolve
    if (x === null) {
      return resolve(x);
    }

    try {
      // 把 x.then 赋值给 then 
      var then = x.then;
    } catch (error) {
      // 如果取 x.then 的值时抛出错误 e ，则以 e 为据因拒绝 promise
      return reject(error);
    }

    // 如果 then 是函数
    if (typeof then === 'function') {
      var called = false;
      // 将 x 作为函数的作用域 this 调用之
      // 传递两个回调函数作为参数，第一个参数叫做 resolvePromise ，第二个参数叫做 rejectPromise
      // 名字重名了，我直接用匿名函数了
      try {
        then.call(
          x,
          // 如果 resolvePromise 以值 y 为参数被调用，则运行 [[Resolve]](promise, y)
          function (y) {
            // 如果 resolvePromise 和 rejectPromise 均被调用，
            // 或者被同一参数调用了多次，则优先采用首次调用并忽略剩下的调用
            // 实现这条需要前面加一个变量called
            if (called) return;
            called = true;
            resolvePromise(promise, y, resolve, reject);
          },
          // 如果 rejectPromise 以据因 r 为参数被调用，则以据因 r 拒绝 promise
          function (r) {
            if (called) return;
            called = true;
            reject(r);
          });
      } catch (error) {
        // 如果调用 then 方法抛出了异常 e：
        // 如果 resolvePromise 或 rejectPromise 已经被调用，则忽略之
        if (called) return;

        // 否则以 e 为据因拒绝 promise
        reject(error);
      }
    } else {
      // 如果 then 不是函数，以 x 为参数执行 promise
      resolve(x);
    }
  } else {
    // 如果 x 不为对象或者函数，以 x 为参数执行 promise
    resolve(x);
  }
}

class MyPromise {

  static resolve = (param) => {
    if (param instanceof MyPromise) {
      return param;
    }

    return new MyPromise((resolve) => {
      resolve(param);
    });
  }

  static reject = (reason) => {
    return new MyPromise((resolve, reject) => {
      reject(reason);
    });
  }

  static all =  (promiseList) => {
    const resPromise = new MyPromise(function (resolve, reject) {
      const result = [];
      const length = promiseList.length;

      let count = 0;

      if (length === 0) {
        return resolve(result);
      }

      promiseList.forEach(function (promise, index) {
        MyPromise.resolve(promise).then(function (value) {
          count++;
          result[index] = value;
          if (count === length) {
            resolve(result);
          }
        }, function (reason) {
          reject(reason);
        });
      });
    });

    return resPromise;
  }

  static race = (promiseList) => {
    return new MyPromise((resolve, reject) => {
      const length = promiseList.length;

      if (length === 0) {
        return resolve();
      } else {
        for (let i = 0; i < length; i++) {
          // 可以兼容 promiseList 的内容不为 promise 的情况
          // return 可以阻断 for
          MyPromise.resolve(promiseList[i]).then((value) => {
            return resolve(value);
          }, (reason) => {
            return reject(reason);
          });
        }
      }
    });
  }

  status = PENDING;

  value;

  reason;

  onFulfilledCallbacks = [];

  onRejectedCallbacks = [];

  constructor(fn) {
    try {
      fn(this.resolve, this.reject);
    } catch (err) {
      this.reject(err);
    }
  }

  resolve = (value) => {
    if (this.status === PENDING) {
      this.status = FULFILLED;
      this.value = value;

      // resolve里面将所有成功的回调拿出来执行
      this.onFulfilledCallbacks.forEach(callback => {
        callback(this.value);
      });
    }
  }

  reject = (reason) => {
    if (this.status === PENDING) {
      this.status = REJECTED;
      this.reason = reason;

      // resolve里面将所有失败的回调拿出来执行
      this.onRejectedCallbacks.forEach(callback => {
        callback(this.reason);
      });
    }
  }

  then = (onFulfilled, onRejected) => {
    const promiseIns = new MyPromise((resolve, reject) => {
      const resolveCallback = () => {
        setTimeout(() => {
          try {
            if (typeof onFulfilled !== 'function') {
              resolve(this.value);
            } else {
              const result = onFulfilled(this.value);
              resolvePromise(promiseIns, result, resolve, reject);
            }
          } catch (error) {
            reject(error);
          }
        });
      }

      const rejectCallback = () => {
        setTimeout(() => {
          try {
            if (typeof onRejected !== 'function') {
              reject(this.reason);
            } else {
              var result = onRejected(this.reason);
              resolvePromise(promiseIns, result, resolve, reject); 
            }
          } catch (error) {
            reject(error);
          }
        });
      }

      if (this.status === FULFILLED) {
        resolveCallback();
      } else if (this.status === REJECTED) {
        rejectCallback();
      } else if (this.status === PENDING) {
        this.onFulfilledCallbacks.push(resolveCallback);
        this.onRejectedCallbacks.push(rejectCallback);
      }
    });

    return promiseIns;
  }

  catch = (onRejected) => {
    this.then(null, onRejected);
  }

  finally = (fn) => {
    return this.then((value) => {
      return MyPromise.resolve(fn()).then(() => value);
    }, () => {
      return MyPromise.resolve(fn()).then(() => {
        throw error;
      });
    });
  }
}

MyPromise.deferred = function () {
  var result = {};
  result.promise = new MyPromise(function (resolve, reject) {
    result.resolve = resolve;
    result.reject = reject;
  });

  return result;
}

module.exports = MyPromise;
```

## 参考

1、[https://zhuanlan.zhihu.com/p/58428287](https://zhuanlan.zhihu.com/p/58428287)

