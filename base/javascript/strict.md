# Strict

* 数字的八进制字面量在严格模式下无效，会导致支持的 JavaScript 引擎抛出错误。

* 严格模式对如何使用 arguments 对象做出了一些限制。

  * 使用 `arguments[index]` 的方式赋值会变得无效

    ```js
    function doAdd (num1, num2) {
      "use strict";
      arguments[1] = 10;
      console.log(arguments[1] + num2);
    }

    doAdd(1); // NaN 
    ```
  
  * 重写 `arguments` 的值会导致语法错误

* `setTimeout` 调用的代码都是在全局作用域中执行的，因此函数中 `this` 的值在非严格模式下为 `window`，在严格模式下为 `undefined`

## 什么是 'use strict;'？使用它的好处和坏处分别是什么？

ES5 添加了第二种运行模式，严格模式，这种模式使得 Javascript 在更严格的条件下运行。

优点：

* 效率更高
* 语义更明确
* 更稳定、更安全
* 为未来增加更多可能性，提前做好向后兼容

缺点：

* 兼容，IE6/7/8/9 均不支持严格模式
* 严格模式改变了语义，依赖这些改变可能会导致没有实现严格模式的浏览器中出现问题或者错误
* 压缩

### 严格模式中的变化

严格模式同时改变了语法及运行时行为。变化通常分为这几类：

* 将问题直接转化为错误（如语法错误或运行时错误）

  * 严格模式下无法再意外创建全局变量。
  * 严格模式会使引起静默失败\(silently fail,注:不报错也没有任何效果\)的赋值操作抛出异常. 例如, NaN 是一个不可写的全局变量. 在正常模式下, 给 NaN 赋值不会产生任何作用; 开发者也不会受到任何错误反馈. 但在严格模式下, 给 NaN 赋值会抛出一个异常. 任何在正常模式下引起静默失败的赋值操作 \(给不可写属性赋值, 给只读属性\(getter-only\)赋值, 给不可扩展对象\(non-extensible object\)的新属性赋值\) 都会抛出异常:
  * 在严格模式下, 试图删除不可删除的属性时会抛出异常\(之前这种操作不会产生任何效果\)
  * 在Gecko版本34之前，严格模式要求一个对象内的所有属性名在对象内必须唯一。正常模式下重名属性是允许的，最后一个重名的属性决定其属性值。因为只有最后一个属性起作用，当代码要去改变属性值而不是修改最后一个重名属性的时候，复制这个对象就产生一连串的bug。在严格模式下，重名属性被认为是语法错误。
  * 严格模式要求函数的参数名唯一. 在正常模式下, 最后一个重名参数名会掩盖之前的重名参数. 之前的参数仍然可以通过 arguments\[i\] 来访问, 还不是完全无法访问. 然而, 这种隐藏毫无意义而且可能是意料之外的 \(比如它可能本来是打错了\), 所以在严格模式下重名参数被认为是语法错误。
  * 严格模式禁止八进制数字语法. ECMAScript并不包含八进制语法, 但所有的浏览器都支持这种以零\(0\)开头的八进制语法: 0644 === 420 还有 "\045" === "%".在ECMAScript 6中支持为一个数字加"0o"的前缀来表示八进制数.
  * ECMAScript 6中的严格模式禁止设置primitive值的属性.不采用严格模式,设置属性将会简单忽略\(no-op\),采用严格模式,将抛出TypeError错误

* 简化了如何为给定名称的特定变量计算

  * 严格模式禁用 `with`
  * 严格模式下的 `eval` 不再为上层范围\(surrounding scope,注:包围eval代码块的范围\)引入新变量
  * 严格模式禁止删除声明变量

* 简化了 `eval` 以及 `arguments`

  * 名称 eval 和 arguments 不能通过程序语法被绑定\(be bound\)或赋值. 
  * 严格模式下，参数的值不会随 arguments 对象的值的改变而变化。
  * 不再支持 arguments.callee

* 将写"安全“JavaScript的步骤变得更简单

  * 在严格模式下通过 `this` 传递给一个函数的值不会被强制转换为一个对象
  * 在严格模式中再也不能通过广泛实现的ECMAScript扩展“游走于”JavaScript的栈中
  * 严格模式下的 `arguments` 不会再提供访问与调用这个函数相关的变量的途径。

* 以及改变了预测未来 ECMAScript 行为的方式。

  * 在严格模式中一部分字符变成了保留的关键字。这些字符包括 `implements`, `interface`, `let`, `package`, `private`, `protected`, `public`, `static` 和 `yield`。
  * 严格模式禁止了不在脚本或者函数层面上的函数声明。
