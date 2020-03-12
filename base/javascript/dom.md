# DOM

DOM 即文档对象模型，是 Document Object Model 的简称。它是针对 XML ，但经过扩展用于 HTML 的应用程序编程接口。

DOM 把整个页面映射为一个多层节点结构。

## DOM 级别

* **DOM1**

  DOM1 的目标主要是映射文档的结构，它由两个模块组成：

  * DOM Core

    DOM 核心规定的是如何映射基于 XML 的文档结构，以便简化对我恩当中任意部分的访问和操作。

  * Dom HTML

    DOM HTML 模块则在 DOM 核心的基础上加以扩展，添加了针对 HTML 的对象和方法。

* **DOM2**

  DOM2 在原来 DOM 基础上又扩充了鼠标和用户事件、范围、遍历（迭代文档的方法）等细分模块，而且通过对象接口增加了对 CSS 的支持。

  DOM1 中的 DOM 核心模块也经过扩展开始支持 XML 命名空间。

  DOM2 新增了下列新模块，也给出了众多新类型和新接口的定义：

  * DOM 视图
  * DOM 事件
  * DOM 样式
  * DOM 遍历和范围

* **DOM3**

  * 引入了统一方式加载和保存文档

    在 DOM 加载和保存模块中定义。

  * 新增了验证文档的方法

    在 DOM 验证模块中定义

  * DOM3 也对 DOM 核心进行了扩展，开始支持 XML 1.0 规范。


## 其他

在全局环境中声明的变量和直接在 window 对象上定义属性有一点区别：全局变量不能通过 delete 删除，而直接在 window 对象上定义的属性可以。

```js
var age = 10;
window.name = 'wahaha';

delete window.age; // < ie9 报错、false
delete window.name; // true
```

获取窗口位置

```js
function getScreenPos () {
  return {
    left: typeof window.screenLeft === 'number' ? window.screenLeft : window.screenX, 
    top: typeof window.screenTop === 'number' ? window.screenTop : window.screenY
  };
}
```

* `screenLeft` 和 `screenTop` 在 IE、Safari、Opera、Chrome 中有效

* Firefox 支持 `screenX` 和 `screenY` 中有效，同时 Safari、Chrome 也支持
