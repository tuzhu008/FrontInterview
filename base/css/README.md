# CSS

## `display: none;` 和 `visibility: hidden` 的区别?

`display: none;`

* 隐层对应元素，在文档布局中不再给它分配空间，它各边的元素会合拢，就当它从来不存在。
* 会将元素从可访问性树中移除。这会导致该元素及其后续元素不再被屏幕阅读技术访问。
* 子元素会随之不可见
* 会导致重新渲染

`visibility: hidden`

* 隐藏对应元素，但是在文档布局中仍然保留原来的空间。
* 子元素如果设置 `visibility: visible`，子元素可见 
* 只会导致重绘

## `link` 和 `@import` 的区别？

* `link` 属于 HTML 标签，而 `@import`  是 CSS 提供的。
* 页面被加载时，`link` 会同时被加载，而 `@import` 引用的 CSS 会等到页面加载完成再加载
* `@import` 只在 IE5 以上才能识别，而 link 是 HTML 标签，无兼容问题。

## `position` 的 `absolute` 与 `fixed` 共同点与不同点？

共同点：

* 改变元素的呈现方式，让元素以 `inline-block` 方法呈现
* 让元素脱离文档流，不占据空间
* 默认会覆盖到非定位元素上

不同点：

* absolute 是可以进行设置的，而 fixed 的定位元素固定为浏览器窗口
* 页面滚动时，fixed 元素与浏览器窗口之间的距离是不变的

## 介绍一下盒子模型？

1. 盒子模型：margin、border、padding、content
2. 有两种：IE 盒子模型、标准 W3C 盒子模型；IE 的 content 部分包括了 border 和 padding。



