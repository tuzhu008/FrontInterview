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

## CSS 的选择符有哪些？优先级算法如何计算？CSS3 新增伪类有哪些？

| 选择符 | 描述 | 选择器类型 | 权重 | 版本 |
| :--- | :--- | :--- | :--- | :--- |
| \* | 通配符 | 其他 | 0 | 2 |
| .class | 类 | 类选择器 | 10 | 1 |
| \#id | id | id 选择器 | 100 | 1 |
| element | 元素 | 元素选择器 | 1 | 1 |
| element, element | 多目标 | 其他 | 0 | 1 |
| element element | 后代 | 其他 | 0 | 1 |
| element &gt; element | 子 | 其他 | 0 | 2 |
| element + element | 相邻 | 其他 | 0 | 2 |
| element1 ~ element2 | 选择前面有 element1 的每个 element2 元素 | 其他 | 0 | 3 |
| :first-letter | 首字母 | 伪元素选择器 | 1 | 1 |
| :first-line | 首行 | 伪元素选择器 | 1 | 1 |
| :before | 内容之前的内容 | 伪元素选择器 | 1 | 2 |
| :after | 内容之后的内容 | 伪元素选择器 | 1 | 2 |
| :root | 文档的根元素 | 伪元素选择器 | 1 | 3 |
| ::selection | 选择被用户选取的元素部分。 | 伪元素选择器 | 1 | 3 |
| \[attribute\] | 属性 | 属性选择器 | 10 | 2 |
| \[attribute=value\] | 属性值相等 | 属性选择器 | 10 | 2 |
| \[attribute~=value\] | 属性值包含.. | 属性选择器 | 10 | 2 |
| \[attribute\|=value\] | 属性值以..开头 | 属性选择器 | 10 | 2 |
| \[attribute^=value\] | 属性值以..开头 | 属性选择器 | 10 | 3 |
| \[attribute$=value\] | 属性值以..结尾 | 属性选择器 | 10 | 3 |
| \[attribute$=value\] | 属性值以..结尾 | 属性选择器 | 10 | 3 |
| :link | 违背访问的链接 | 动态伪类选择器 | 10 | 1 |
| :visited | 访问过的链接 | 动态伪类选择器 | 10 | 1 |
| :active | 活动的链接 | 动态伪类选择器 | 10 | 1 |
| :hover | 鼠标位于其上的链接 | 动态伪类选择器 | 10 | 1 |
| :focus | 获得焦点的元素 | 动态伪类选择器 | 10 | 2 |
| :lang\(language\) | lang 属性以..开头 | 动态伪类选择器 | 10 | 2 |
| :first-child | 第一个子元素 | 结构伪类选择器 | 10 | 2 |
| :last-child | 最后一个 | 结构伪类选择器 | 10 | 3 |
| :first-of-type | 类型的第一个 | 结构伪类选择器 | 10 | 3 |
| :last-of-type | 类型的最后一个 | 结构伪类选择器 | 10 | 3 |
| :only-of-type | 仅出现一次的类型 | 结构伪类选择器 | 10 | 3 |
| :only-child | 仅有一个子元素 | 结构伪类选择器 | 10 | 3 |
| :nth-child\(n\) | 第几个子元素 | 结构伪类选择器 | 10 | 3 |
| :nth-last-child\(n\) | 倒数第几个子元素 | 结构伪类选择器 | 10 | 3 |
| :nth-of-type\(n\) | 类型的第几个 | 结构伪类选择器 | 10 | 3 |
| :nth-last-of-type\(n\) | 类型的倒数几个 | 结构伪类选择器 | 10 | 3 |
| :empty | 空元素 | 结构伪类选择器 | 10 | 3 |
| :enabled | 启用的 | 状态伪类选择器 | 10 | 3 |
| :disabled | 禁用的 | 状态伪类选择器 | 10 | 3 |
| :checked | 选择的 | 状态伪类选择器 | 10 | 3 |
| :not\(selector\) | 非 |  | 10 | 3 |
| :target | 当前活动的 |  | 10 | 3 |

权重规则：

* 代表内联样式，如: style=””，权值为1000。
* 代表ID选择器，如：\#content，权值为0100。
* 代表类，伪类和属性选择器，如.content，权值为0010。
* 代表类型选择器和伪元素选择器，如div p，权值为0001。
* 通配符、子选择器、相邻选择器等的。如\*、&gt;、+,权值为0000。

## 哪些属性可以继承？

* 字体系列属性

  | 属性 | 描述 |
  | :--- | :--- |
  | font |  |
  | font-family |  |
  | font-weight |  |
  | font-size |  |
  | font-style |  |
  | font-variant |  |
  | font-stretch |  |
  | font-size-adjust |  |

* 文本系列属性

  | 属性 | 描述 |
  | :--- | :--- |
  | text-indent |  |
  | text-align |  |
  | line-height |  |
  | word-spacing |  |
  | letter-spacing |  |
  | text-transform |  |
  | direction |  |
  | color |  |

* 元素可见性：visibility

* 表格布局属性

  | 属性 | 描述 |
  | :--- | :--- |
  | caption-side |  |
  | border-collapse |  |
  | border-spacing |  |
  | empty-cells |  |
  | table-layout |  |

* 列表属性

  | 属性 | 描述 |
  | :--- | :--- |
  | list-style-type |  |
  | list-style-image |  |
  | list-style-position |  |
  | list-style |  |

* 生成内容属性：quotes

* 光标属性：cursor

* 页面样式属性

  | 属性 | 描述 |
  | :--- | :--- |
  | page |  |
  | page-break-inside |  |
  | windows |  |
  | orphans |  |

* 声音样式属性

  | 属性 | 描述 |
  | :--- | :--- |
  | speak |  |
  | speak-punctuation |  |
  | speak-numeral |  |
  | speak-header |  |
  | speech-rate |  |
  | volume |  |
  | voice-family |  |
  | pitch |  |
  | pitch-range |  |
  | stress |  |
  | richness |  |
  | azimuth |  |
  | elevation |  |

### 所有元素可以继承的属性：

1、元素可见性：visibility、opacity  
2、光标属性：cursor

### 内联元素可以继承的属性:

1、字体系列属性  
2、除text-indent、text-align之外的文本系列属性

### 块级元素可以继承的属性:

1、text-indent、text-align

### 无继承性的属性

* \*display

* 文本系列属性

  | 属性 | 描述 |
  | :--- | :--- |
  | vertical-align |  |
  | text-decoration |  |
  | text-shadow |  |
  | white-space |  |
  | unicode-bidi |  |

* 盒子模型的属性：宽度、高度、内外边距、边框等

* 背景属性：背景图片、颜色、位置等

* 定位属性：浮动、清除浮动、定位position等

* 生成内容属性：content、counter-reset、counter-increment

* 轮廓样式属性：utline-style、outline-width、outline-color、outline

* 页面样式属性：size、page-break-before、page-break-after

### 继承中比较特殊的几点

* a 标签的字体颜色不能被继承

* h1-h6 标签字体的大小也是不能被继承的

因为它们都有一个默认值

## 列出 display 的值，说明他们的作用？

| 取值 | 描述 |
| :--- | :--- |
| none | 此元素不会被显示。 |
| block | 此元素将显示为块级元素，此元素前后会带有换行符。 |
| inline | 默认。此元素会被显示为内联元素，元素前后没有换行符。 |
| inline-block | 行内块元素。（CSS2.1 新增的值） |
| flex |  |
| grid |  |
| list-item | 此元素会作为列表显示。 |
| run-in | 此元素会根据上下文作为块级元素或内联元素显示。 |
| table | 此元素会作为块级表格来显示（类似 `<table>`），表格前后带有换行符。 |
| inline-table | 此元素会作为内联表格来显示（类似 `<table>`），表格前后没有换行符。 |
| table-row-group | 此元素会作为一个或多个行的分组来显示（类似 `<tbody>`）。 |
| table-header-group | 此元素会作为一个或多个行的分组来显示（类似 `<thead>`）。 |
| table-footer-group | 此元素会作为一个或多个行的分组来显示（类似 `<tfoot>`）。 |
| table-row | 此元素会作为一个表格行显示（类似 `<tr>`）。 |
| table-column-group | 此元素会作为一个或多个列的分组来显示（类似 `<colgroup>`）。 |
| table-column | 此元素会作为一个单元格列显示（类似 `<col>`） |
| table-cell | 此元素会作为一个表格单元格显示（类似 `<td>` 和 `<th>`） |
| table-caption | 此元素会作为一个表格标题显示（类似 `<caption>`） |
| inherit | 规定应该从父元素继承 display 属性的值。 |

## position 的 relative 和 absolute fen 分别是对于谁定位的？



