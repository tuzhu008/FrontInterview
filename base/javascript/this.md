# this

this 用来在函数体内部指代函数当前的运行环境。

this 对象是在运行时基于函数的执行环境绑定的：

* 在全局函数中，this 等于 window
* 而当函数被作为某个对象的方法调用时，this 等于那个对象

**匿名函数的执行环境具有全局性，因此 this 通常指向 window**

```js
var name = 'The window';

var object = {
  name: 'My Object',
  getNameFunc: function () {
    return function () {
      return this.name;
    }
  }
};

alert(object.getNameFunc()()); // 非严格模式下弹出 The window
```

每个函数在被调用时，其活动对象会自动取得两个特殊变量，`this` 和 `arguments`。因此内部函数在搜索这两个变量时，只会搜索到其活动对象，因此永远不可能直接访问外部函数中的这两个变量。因为在搜索其活动对象的时候就已经找到了。

```js
var name = 'The window';

var object = {
  name: 'My Object',
  getNameFunc: function () {
    var that = this;
    return function () {
      return that.name;
    }
  }
};

alert(object.getNameFunc()()); // 非严格模式下弹出 The window
```

再来看下面几种情况：

```js
var name = 'The window';

var object = {
  name: 'My Object',
  getName: function () {
    return that.name;
  }
};

object.getName(); // My Object
(object.getName)(); // My Object
(object.getName = object.getName)(); // My Object
```

`(object.getName)()` 虽然加上了括号之后就好像在引用一个函数，但是 `this` 得到了维持，因为 `object.getName` 和 `(object.getName)` 的定义是相同的。

`(object.getName = object.getName)()` 先执行了一条赋值语句，然后再调用赋值后的结果。因为这个表达式的值是函数本身，所以 `this` 的值不能得到维持。

