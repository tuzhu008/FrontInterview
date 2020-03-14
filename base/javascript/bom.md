# BOM

BOM 即浏览器对象模型。

从根本上上讲，BOM 只处理浏览器窗口和框架；但习惯也把所有针对浏览器的 JavaScript 扩展算作 BOM 的一部分。下面是一些扩展：

* 弹出新浏览器窗口的功能
* 移动、缩放和关闭浏览器窗口的功能
* 提供浏览器详情信息的 `navigator` 对象
* 提供浏览器所加载页面的详细信息的 `location` 对象
* 提供用户显示器分辨率详细信息的 `screen` 对象
* 对 cookie 的支持
* 像 XMLHttpRequest 和 IE 的 ActiveXobject 这样的自定义对象。

## location 对象

location 对象提供了与当前窗口中加载的文档有关的信息，还提供了一些导航信息。

location 对象既是 window 对象的属性，也是 document 对象的属性。

```js
window.location === document.location // true
```

location 对象将 URL 解析为独立的片段，让开发人员可以通过不同的属性访问这些片段。

## history 对象

## navigator 对象

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