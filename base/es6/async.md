# async

`async` 函数是 `Generator` 函数的语法糖。

async/await 的本身是基于Promise的。

实际上 async 函数对 Generator 函数的改进，体现在一下四点：

* async 函数自带执行器，所以执行方式和普通函数的执行方式一样。
* async 和 await 比起 `*`和 `yield` 在语义上更清楚。
* co模块约定，yield命令后面只能是 Thunk 函数或 Promise 对象，而async函数的await命令后面，可以是 Promise 对象和原始类型的值（数值、字符串和布尔值，但这时会自动转成立即 resolved 的 Promise 对象）。

* async函数的返回值是 Promise 对象，这比 Generator 函数的返回值是 Iterator 对象方便多了。你可以用then方法指定下一步的操作。



