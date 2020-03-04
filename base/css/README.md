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

## CSS3 新特性

* 新增结构伪类元素
* 多栏布局、grid 布局、flex 布局
* 阴影：box-shadow
* 圆角：border-radius
* 边框图案：border-image
* 文字特效：text-shadow
* 变化：transform
* 过渡：transition
* 动画：animation
* 媒体查询：@media
* 字体：@font-face
* 关键帧：@keyframes
* 多背景
* rgba
* 线性渐变：gradient

## 为什么要初始化 CSS 样式

因为浏览器的兼容问题，不同的浏览器对有些标签的默认值是不同的，这会导致浏览器之间的页面显示差异。

当然，初始化样式会对 SEO 有一定的影响，但鱼和熊掌不可兼得，但力求影响最小的情况下进行。

## 对 BFC 规范的理解？

BFC，Block Formatting Context，块级格式化上下文。BFC 是 Web 页面的可视化 CSS 渲染的一部分，是布局过程中生成块级盒子的区域，也是浮动元素与其他元素的交互限定区域。

* BFC 是一个独立的布局环境，可以理解为一个容器，在这个容器中按照一定规则进行物品摆放，并且不会影响其他环境中的物品。
* 如果一个元素符合触发 BFC 的条件，则 BFC 中的元素布局不受外部影响。
* 浮动元素会创建 BFC，则读懂元素内部子元素主要受该浮动元素影响，所以两个浮动元素之间是互不影响的。

### 创建 BFC

* 根元素或包含根元素的元素
* 浮动元素 float = left \| right 或 inherit\(≠null\)
* 绝对定位元素 position = absolute 或 fixed
* display = inline-block \| flex \| inline-flex \| table-cell 或 table-caption
* overflow = hidden \| auto 或 scroll\(≠visible\)

### BFC 的特性

* BFC 是一个读；i的容器，容器内子元素不会影响容器外的元素。反之亦如此。
* 盒子从顶端开始垂直地一个接一个地排列，盒子之间垂直地间距是由 margin 决定的。
* 在同一个 BFC 中，两个相邻的处置外边距会发生重叠
* BFC 区域不会和 float box 发生重叠
* BFC 能够识别并包含浮动元素，当计算其区域的高度时，浮动元素也可以参考参与计算

### BFC 的作用

* 包含浮动元素（清除浮动）
* 导致外边距折叠
* 避免外边距折叠

## 解释下 CSS sprite，以及要如何在页面或网站中使用它

CSS Sprite 俗称精灵图，或者雪碧图。来自于动漫，我们将很多小精灵的图片集合到一张卡片上。CSS 也是如此，我们将网页中一些背景图片、icon 整合到一张图片文件中，再利用 CSS 的 backgroun-position 参数对图片进行定位，初始时元素的坐上角和图片坐上角是重合的位置为（0, 0），想要目标图片时，我们需要移动图片，由于元素已经在文档中定好位，移动的是背景图，背景图相较于原始位置进行左移和下移，因此得到的定位值为负。通过定位我们能准确获取到目标图片的位置。

优点：减少很多图片请求的开销，浏览器虽然能够并发，但也存在并发数限制，一般为 6 个。

## 说说你对语义化的理解？

* 用正确的标签做正确的事，有利于在浏览器中显示清晰的结构
* 有利于 SEO：与搜索引起建立良好的沟通，有助于爬虫抓取更多有效信息
* 有助于无障碍模式
* 便于团队开发和维护，语义化使代码更具可读性。

## Doctype 的作用？严格模式和混杂模式如何区分？它们有何意义？

`<!Doctype>` 用来进行文档声明，处于 `<html>` 标签之前，用来告知浏览器以何种模式来渲染文档。

严格模式的排版和 JS 运作模式是以该浏览器支持的最高标准执行的，而混杂模式则以宽容的向后兼容的方式显示，它会模拟老式浏览器的行为，以防止站点无法工作。

`<!Doctype>` 不存在或格式不正确会导致文档以混杂模式呈现。

## 你知道几种 Doctype 文档声明？

Doctype 可以声明三种 DTD 类型，分别表示严格版本、过渡版本以及基于框架的 HTML 文档。

* HTML 4.01 规定了三种文档类型：

  * Stric
  * Transitional
  * Frameset

* HTML 1.0 规定了三种文档类型：

  * Stric
  * Transitional
  * Frameset

Standards 模式用于呈现遵循最新标准的网页，而 Quirks 模式用于呈现为传统浏览器而设计的网页

* html5

  ```html
  <!DOCTYPE html>
  ```

* html4.01

  ```html
  <!-- Strict -->
  <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">

  <!-- Transitional -->
  <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">

  <!-- Frameset -->
  <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN" "http://www.w3.org/TR/html4/frameset.dtd">
  ```

* XHTML 1.0

  ```html
  <!-- Strict -->
  <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">

  <!-- Transitional -->
  <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

  <!-- Frameset -->
  <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Frameset//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd">
  ```

* XHTML 1.0

  ```html
  <!-- Strict -->
  <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">
  ```

## HTML 和 XHTML 有什么区别？

XHTML 元素必须被正确的嵌套

XHTML 元素必须被关闭

XHTML 标签必须用小写字母

XHTML 文档必须拥有根元素

## html 常见兼容性问题？

* 浏览器默认的 `margin` 和 `padding` 不同

* 获取属性方式：IE 可以使用获取常规属性方式获取自定义属性，也可以使用 `getAttribute()` 方法，但 firefox 下，只能使用 `getAttribute()`。

* IE 下，event 对象有 x、y 属性，但是没有 pageX、pageY firefox 有 pageX、pageY 属性，没有  x、y 属性。

  解决方案：使用条件注释，引入对应的代码。

* Chrome 中文见面下默认会将小雨 12px 的文本强制按照 12 px 显示

  解决方案：

  ```css
  html{ -webkit-text-size-adjust: none; }
  ```

* 超链接样式顺序导致效果

  解决方案：按一下顺序书写

  ```css
  a:link {}
  a:visited {}
  a:hover {}
  a:active {}
  ```

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

## DOM 操作——怎样添加、移动、移除、复制、创建和查找节点

* 创建节点

  ```js
  document.createElement() // 创建一个具体的元素
  document.createTextNode() // 创建一个文本节点
  document.createDocumentFragement() // 创建一个 DOM 片段
  ```

* 添加、移除、替换、插入、复制

  ```js
  ParentNode.append()
  ParentNode.appendChild()

  ParentNode.replaceChild()

  ParentNode.insertBefore()

  Node.cloneNode()

  ```

* 查找

  ```js
  element.getElementsByTagName()
  element.getElementsByClass()
  element.getElementById()

  element.querySelector()
  element.querySelectorAll()
  ```

## HTML5 有哪些新特性、移除了哪些元素？如何处理 HTML5 新标签的浏览器兼容问题？如何区分 HTML 和 HTML5？

新特性：

HTML5 现在已经不是 SGML 的子集，主要是关于图像，位置，存储，多任务等功能的增加

* 拖拽释放 API
* 语义化更好的内容标签：`<header>`、`<footer>`、`<nav>`、`<aside>`、`<article>`、`<section>`
* 音频、视频 API
* 画布：Canvas API
* 地址位置：Geolocation API
* localStorage、sessionStorage
* 表单控件：calendar、date、time、email、url、search
* 新的技术：webworker、websocket、Geolocation

移除的元素：

* 纯表现的元素：basefont、big、center、font、s、sticke、tt、u
* 对可用性产生负面影响的元素：frame、frameset、noframes

支持 HTML 新标签：

IE8/7/6支持通过 `document.createElement` 方法产生的新标签，可以利用这一特性让这些浏览器支持 HTML5 新标签，浏览器支持新标签后，还需要添加默认的样式。

有现成插件可用：`html5shiv`

如何区分：

从 DOCTYPE 文档声明、新增的结构元素、功能元素都可以区分

## iframe 的优缺点

优点：

* 解决加载缓慢的第三方内容，如图标和广告的加载问题
* Security sandbox(安全沙箱)
* 并行加载脚本

缺点：

* iframe 会阻塞主页面的 onload 事件
* 即使内容为空，加载也需要时间
* 没有语义
* 产生很多页面，不易管理
* 不容易打印
* 浏览器的后退按钮无效
* 代码复杂，不利于代码 SEO
* 增加不必要的 http 请求

## 如何实现浏览器多个标签页之间的通信？

localStorage（storage 事件）、cookie

## webSocket 如何兼容低浏览器？

* Adobe Flash Socket
* ActiveX HTMLFile（IE)
* 轮询

## 线程与进程的区别

* 进程是资源分配的最小单位，线程是程序执行的最小单位
* 进程有自己的独立地址空间，没启动一个进程，系统就会为它分配地址空间，建立数据表来维护代码段、堆栈段和数据段，这种操作非常昂贵。而线程是共享进程中的数据的，使用相同的地址空间，因此 CPU 切换一个线程的花费远比进程小很多，同时创建一个线程的开销也比进程小很多
* 线程之间的通信更方便，统一进程下的线程共享全局变量、静态变量等数据，而进程之间的通信则需要以通信的方式（IPC）进行。不过如何处理好同步与互斥是便携多线程程序的难点。
* 但是多进程程序更健壮，多线程程序只需要有一个线程死掉，整个进程也死掉了，而一个进程死掉并不会对另一个进程造成影响，因为进程有自己独立的地址空间。
* 从逻辑上来说，多线程的意义在于一个应用程序中，有多个执行部分可以同时执行。但是操作系统并没有将多个线程看作多个独立的应用，来实现进程调度和管理以及资源分配。这就是进程与线程的重要区别。

## 如何对网站的文件和资源进行优化？

* 文件合并，减少 http 请求次数
* 压缩：文件压缩、响应压缩
* CDN 托管，降低通信距离
* 缓存
* CSS 放最上面，script 放最上面
* 避免在 CSS 使用表达式
* 结构、样式、行为分离
* 减少 DNS 查询
* 避免重定向
* 正确选用图片格式

## 你都使用哪些工具来测试代码的性能？

* Profile
* JSPerf
* Dromaeo

没做过

## 什么是 FOUC（无样式内容闪烁）？如何来避免？

FOUC：Flash Of Unstyled Content 无样式内容闪烁

引起该问题的罪魁祸首就是 `@import`，它会在文档加载完成后再去加载，将其替换为 `link` 标签

## document.write 和 innerHTML 的区别？

document.write 只能重绘整个页面，innerHTML 可以重绘页面的一部分

## .call 和 .apply 的作用？

动态改变某个类的某个方法的运行环境

## 什么叫优雅降级和渐进增强？

* 优雅降级：Web 站点在所有心事浏览器中都能正常工作，如果用户使用的是老式浏览器，则代码会检查确认它们是否能正常工作。由于 IE 独特的盒模型布局问题，针对不同版本的 IE 的 hack 时间过优雅降级了，为那么无法支持新功能的浏览器增加候选方案，使之在旧式浏览器上以某种形式降级体验却不至于完全无效。

* 渐进增强：从被所有浏览器支持的基本功能开始，逐步地添加哪些只有新式浏览器才支持的功能，向页面增加无害于基础浏览器的额外样式和功能。当浏览器支持时，他们会自动地呈现出来并发挥作用。

## 性能优化方法？

* 减少 http 请求数量和数据量：压缩代码、合并代码、合并图片、合理选用图片格式、网页 Gzip 压缩、 Ajax 数据缓存
* CDN、图片服务器
* 减少 DOM 操作次数：innerHTML 代替 DOM 操作
* 尽量使用 className 而不是 style
* 优化 js 性能
* 减少全局变量，缓存 DOM 节点查找结果
* 避免使用 CSS Expression
* css 网顶部，脚本放底部

## 一个页面从输入 URL 到页面加载显示完成，这个过程中都发生了什么？


