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

| 类型 | nodeValue | nodeType |
| :--- | :--- | :--- |
| Node |  |  |
| Document |  |  |
| ELement |  |  |
| Text |  |  |
| Comment |  |  |
| CDATASection |  |  |
| DocumentType |  |  |
| DocumentFragment |  |  |

## Node

```js
someNode.childNodes
someNode.parentNode
someNode.previousNode
someNode.nextNode
someNode.firstChild
someNode.lastChild
someNode.ownDocument

someNode.hasChildNodes()

// 支持子节点的节点类型，操作子节点
someNode.appendChild()
someNode.insertBefore()
someNode.replaceChild()
someNode.removeChild()

// 所有节点类型都支持
someNode.cloneNode(deep: boolean)
someNode.normalize()
```

## Document 类型

JavaScript 通过 Document 类型来表示文档。

`document` 是 HTMLDocument 的实例，HTMLDocument 继承自 Document

`document` 是 `window` 对象的属性

```js
document.constructor === HTMLDocument // true

document instanceof Document // true

document === window.document // true
```

**访问 **`<html>`** 元素：**

```js
ducoment.documentElement // 指向 html 元素

ducoment.documentElement === document.childNodes[0] // true
ducoment.documentElement === document.firstChild // true
```

**访问 **`<body>`** 元素：**

```js
document.body
```

**访问 DocumentType：**

浏览器实现不一

```js
document.doctype
```

**访问 title**

```js
document.title
```

```js
document.URL
document.domain
document.referrer
```

这三个中只有 `domain` 是可以修改的，但出于安全考虑，这个值只允许设置为 `URL` 中出现的域

**查找元素：**

```js
document.getElementById()
document.getElementsByClass()
document.getElementsByTagName() // 返回一个 HTMLCollection 对象
document.getElementsByName() // 返回一个 HTMLCollection 对象
document.createElement()
```

```js
document.links
document.images
document.anchors
document.forms
```

## Element 类型

Element 类型用于表现 XML 和 HTML 元素。

所有的 HTML 元素都是 HTMLElement 的实例或子实例，HTMLElement 继承自 Element。

```js
element.id
element.className
element.title
element.lang
element.dir

element.getAtrribute()
element.setAtrribute()
element.removeAtrribute()
```

## 



