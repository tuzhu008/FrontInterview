# CSS

## CSS 有什么特殊性？（优先级、计算特殊值）

优先级：

* 同类型，同级别的样式后者先于前者
* 根据权重计算
* 具体的 > 泛化的
* 近的 > 远的（内联样式 > 内部样式表 > 外联样式表）

  * 内联样式：`style` 属性
  * 内部样式表：`link` 标签
  * 外联样式表）：在外部样式表中 `@import` 的样式，因为 `@import` 总是像代码加在样式表头部，按照声明顺序

* 内联 `!important` > `!important` > 内联


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

display属性有两个作用：

* 一是定义元素的类型（块级元素或行内元素），规定元素的流式布局；
* 二是控制其子元素的布局（`grid` 或 `flex`）。

grid：网格布局

flex：弹性布局

```css
/* <display-outside> values
外部显示类型 */
display: block;
display: inline;
display: run-in;

/* <display-inside> values
内部显示类型 */
display: flow;
display: flow-root;
display: table;
display: flex;
display: grid;
display: ruby;

/* <display-outside> plus <display-inside> values */
display: block flow;
display: inline table;
display: flex run-in;

/* <display-listitem> values */
display: list-item;
display: list-item block;
display: list-item inline;
display: list-item flow;
display: list-item flow-root;
display: list-item block flow;
display: list-item block flow-root;
display: flow list-item block;

/* <display-internal> values */
display: table-row-group;
display: table-header-group;
display: table-footer-group;
display: table-row;
display: table-cell;
display: table-column-group;
display: table-column;
display: table-caption;
display: ruby-base;
display: ruby-text;
display: ruby-base-container;
display: ruby-text-container;

/* <display-box> values */
display: contents;
display: none; /* 此元素不会被显示 */

/* <display-legacy> values */
display: inline-block;
display: inline-table;
display: inline-flex;
display: inline-grid;

/* Global values */
display: inherit;
display: initial;
display: unset;
```

## position 的 relative 和 absolute 分别是对于谁定位的？

relative：相对于在标准文档流中的位置定位，标准文档流。

absolute：绝对定位，最近非 static 的祖先元素

fixed：绝对定位，窗口

static：没有定位，标准文档流。



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

## 为什么要初始化 CSS 样式

因为浏览器的兼容问题，不同的浏览器对有些标签的默认值是不同的，这会导致浏览器之间的页面显示差异。

当然，初始化样式会对 SEO 有一定的影响，但鱼和熊掌不可兼得，但力求影响最小的情况下进行。


## 解释下 CSS sprite，以及要如何在页面或网站中使用它

CSS Sprite 俗称精灵图，或者雪碧图。来自于动漫，我们将很多小精灵的图片集合到一张卡片上。CSS 也是如此，我们将网页中一些背景图片、icon 整合到一张图片文件中，再利用 CSS 的 backgroun-position 参数对图片进行定位，初始时元素的坐上角和图片坐上角是重合的位置为（0, 0），想要目标图片时，我们需要移动图片，由于元素已经在文档中定好位，移动的是背景图，背景图相较于原始位置进行左移和下移，因此得到的定位值为负。通过定位我们能准确获取到目标图片的位置。

优点：减少很多图片请求的开销，浏览器虽然能够并发，但也存在并发数限制，一般为 6 个。


## 解释下浮动和它的工作原理？清除浮动的技巧？

使用浮动的元素会脱离标准文档流，不占据空间。浮动元素碰到包含它的边框或者浮动元素的边框停留。

清除浮动：

* 使用 `clear: both|left|right`

  此方法是通过自动为清除元素加上外边距实现的

* 使用空标签清除

* 使用 overflow

* 使用 after 伪元素

后面这三种清除的本质都是使用 BFC。

## 浮动元素引起的问题和解决办法？

* 父元素高度无法被撑开，影响父级元素同级的元素

* 与浮动元素同级的非浮动元素位置改变

* 同级元素布局改变

清除浮动
