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
2. 有两种：IE 盒子模型、标准 W3C 盒子模型；IE 的 content 部分包括了 border 和 padding。CSS3 中可以通过 `box-sizing` 进行设置

## CSS 的选择符有哪些？哪些属性可以继承？优先级算法如何计算？CSS3 新增伪类有哪些？

| 选择符 | 描述 | 选择器类型 | 权重 | 版本 |
| :--- | :--- | :--- | :--- | :--- |
| \*  | 通配符 |  |  | 2 |
| .class | 类 |  | 10 | 1 |
| \#id | id |  | 100 | 1 |
| element | 元素 |  | 1 | 1 |
| element, element | 多目标 |  |  | 1 |
| element element | 后代 |  |  | 1 |
| element &gt; element | 子 |  |  | 2 |
| element + element | 相邻 |  |  | 2 |
| element1 ~ element2 | 选择前面有 element1 的每个 element2 元素 |  |  | 3 |
| \[attribute\] | 属性 |  |  | 2 |
| \[attribute=value\] | 属性值相等 | 属性选择器 | 10 | 2 |
| \[attribute~=value\] | 属性值包含.. | 属性选择器 | 10 | 2 |
| \[attribute\|=value\] | 属性值以..开头 | 属性选择器 | 10 | 2 |
| \[attribute^=value\] | 属性值以..开头 | 属性选择器 | 10 | 3 |
| \[attribute$=value\] | 属性值以..结尾 | 属性选择器 | 10 | 3 |
| \[attribute$=value\] | 属性值以..结尾 | 属性选择器 | 10 | 3 |
| :link | 违背访问的链接 |  | 10 | 1 |
| :visited | 访问过的链接 |  | 10 | 1 |
| :active | 活动的链接 |  | 10 | 1 |
| :hover | 鼠标位于其上的链接 |  | 10 | 1 |
| :focus | 获得焦点的元素 |  | 10 | 2 |
| :first-letter | 首字母 |  | 10 | 1 |
| :first-line | 首行 |  | 10 | 1 |
| :first-child | 第一个子元素 |  | 10 | 2 |
| :before | 内容之前的内容 |  | 10 | 2 |
| :after | 内容之后的内容 |  | 10 | 2 |
| :lang\(language\) | lang 属性以..开头 |  | 10 | 2 |
| :first-of-type | 类型的第一个 |  | 10 | 3 |
| :last-of-type | 类型的最后一个 |  | 10 | 3 |
| :only-of-type | 仅出现一次的类型 |  | 10 | 3 |
| :only-child | 仅有一个子元素 |  | 10 | 3 |
| :nth-child\(n\) | 第几个子元素 |  | 10 | 3 |
| :nth-last-child\(n\) | 倒数第几个子元素 |  | 10 | 3 |
| :nth-of-type\(n\) | 类型的第几个 |  | 10 | 3 |
| :nth-last-of-type\(n\) | 类型的倒数几个 |  | 10 | 3 |
| :last-child | 最后一个 |  | 10 | 3 |
| :root | 文档的根元素 |  | 10 | 3 |
| :empty | 空元素 |  | 10 | 3 |
| :target | 当前活动的 |  | 10 | 3 |
| :enabled | 启用的 |  | 10 | 3 |
| :disabled | 禁用的 |  | 10 | 3 |
| :checked | 选择的 |  | 10 | 3 |
| :not\(selector\) | 非 |  | 10 | 3 |
| ::selection | 选择被用户选取的元素部分。 |  | 10 | 3 |



权重规则：







