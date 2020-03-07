# CSS/HTML

## 对 Web 标准以及 W3C 的理解与认识？

Web 标准：它不是某一个标准，而是一系列标准的集合。网页主要由三部分组成：结构、表现、行为。对应的标准也分三方面，结构化标准语言主要包括 XHTML 和 XML，表现标准语言主要包括 CSS，行为标准语言主要包括对象模型、ECMAScript 等等。这些标准大部分由万维网联盟起草和发布，也有一些是其他标准组织制定的。比如 ECMA 的 ECMAScript 标准。

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

## iframe 的优缺点

优点：

* 解决加载缓慢的第三方内容，如图标和广告的加载问题
* Security sandbox\(安全沙箱\)
* 并行加载脚本

缺点：

* 产生很多页面，不易管理
* iframe 会阻塞主页面的 onload 事件
* 即使内容为空，加载也需要时间
* 没有语义
* 不容易打印
* 浏览器的后退按钮无效
* 代码复杂，不利于代码 SEO
* 增加不必要的 http 请求
* iframe 和主页面共享连接池，而浏览器对连接数有限制，所以会影响页面并行加载。

## 如何实现浏览器多个标签页之间的通信？

localStorage（storage 事件）、cookie

## webSocket 如何兼容低浏览器？

* Adobe Flash Socket
* ActiveX HTMLFile（IE\)
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

* 当发送一个 URL 请求时，浏览器会开启一个线程来处理这个请求。
* DNS 解析，获取到资源目标主机的 IP 地址
* 三次握手建立 TCP/IP 链接
* 一旦 TCP/IP 连接建立，浏览器会通过该连接向远程服务器发送 HTTP 请求。
* 远程服务器找到资源并使用 HTTP 响应返回资源
* 此时，Web 服务器提供资源服务，客户端开始下载资源
* 浏览器解析返回的 HTML 文档，生成一个 DOM 树，根据下载的 CSS 生成 CSS 规则树，然后浏览器经过计算将 DOM 树和 CSS 树以一定规则生成 render 树。DOM 树和 CSS 树是不会相互阻塞的。

## 如何进行页面重构？

页面重构的概念：根据原有内容和结构的基础上，通过 div + css 写出更满足产品需求，更符合 web 标准的页面结构。

要解决的问题：

* 业务需求，设计还原度
* SEO
* 性能
* 可维护性
* 向前兼容

重构涉及内容：

* 设计稿的重构：设计师的设计稿可能不是特别符合页面效果，当拿到设计稿时需要通过二次重构和修改达到预期效果。
* 功能不全页面的重构：页面功能不符合用户体验、用户交互。
* 过时页面的重构：使用的是过时的代码和标签，跟不上时代的发展。
* 代码重构：代码质量、SEO优化、页面性能、更好的语义化、浏览器兼容、CSS优化。

## web 应用从服务器主动推送 Data 到客户端的方式有哪些？

* html5 websocket
* XHR 长连接
* XHR Mutilpart Streaming
* iframe
* Script 长连接

## 事件、IE 与火狐的事件机制有什么区别？如何阻止冒泡？

事件：用户在网页元素进行的操作，导致发生的事情。  
处理机制：IE 是事件冒泡，firefox 同事支持冒泡和捕获。

阻止冒泡：

* e.stopPropagation\(\)
* e.cancelBubble = true;

## 如何获取 UA？

使用 `navigator` 对象

## 浏览器页面有那三层结构，分别是什么，作用是什么？

三层结构：结构层、表示层、行为层

分别是：HTML、CSS、javaScript

作用：HTML 实现页面结构，CSS 完成页面的表现与风格，JavaScript 实现一些客户端的功能与业务

**缺点：**

* 该标准并未能很好的被浏览器所支持。

## 超链接 target 属性的取值和作用？

该属性指定在何处显示链接的资源。 取值为标签（tab），窗口（window），或框架（iframe）等浏览上下文的名称或其他关键词。以下关键字具有特殊的意义:

* `_self`

  当前页面加载，即当前的响应到同一HTML 4 frame（或HTML5浏览上下文）。此值是默认的，如果没有指定属性的话。

* `_blank`

  新窗口打开，即到一个新的未命名的HTML4窗口或HTML5浏览器上下文

* `_parent`

  加载响应到当前框架的HTML4父框架或当前的HTML5浏览上下文的父浏览上下文。如果没有parent框架或者浏览上下文，此选项的行为方式与 \_self 相同。

* `_top`

  IHTML4中：加载的响应成完整的，原来的窗口，取消所有其它frame。 HTML5中：加载响应进入顶层浏览上下文（即，浏览上下文，它是当前的一个的祖先，并且没有parent）。如果没有parent框架或者浏览上下文，此选项的行为方式相同\_self

* framename

  在指定的框架中打开被链接文档。

## `data-*` 属性的作用是什么？

`data-*` 全局属性 是一类被称为自定义数据属性的属性，它赋予我们在所有 HTML 元素上嵌入自定义数据属性的能力，并可以通过脚本(一般指JavaScript) 与 HTML 之间进行专有数据的交换。

JS 可以通过 `getAttribute` 和 `dataset` 获取

## Label 的作用是什么，是怎么用的？

`<label>` 元素（标签）表示用户界面中某个元素的说明。当用户选择该标签时，浏览器会自动将焦点转移到和标签和相关的表单控件上。

将一个 `<label>` 和一个 `<input>` 元素放在一起会有以下几点好处：

* 标签文本不仅与其相应的文本输入在视觉上相关联; 它也以编程方式与它相关联。 这意味着，当用户点击到表单输入时，屏幕阅读器可以读出标签，使在使用辅助技术的用户更容易理解应输入哪些数据.
* 你可以单击关联的标签来聚焦或者激活 input，以及 input 本身。这种增加的命中区域为激活 input 提供了方便，包括那些使用触摸屏设备的。

其他使用事项：

* 标签标记的表单控件称为标签元素的标签控件。一个 input 可以与多个标签相关联。
* 标签本身并不与表单直接相关。它们只通过与它们相关联的控件间接地与表单相关联。
* 当点击或者触碰（tap）一个与表单控件相关联的 `<label>` 时，关联的控件的 click 事件也会触发。
属性

## 如何实现一个原型的可点击区域？

* map + area、SVG
* `border-radius`
* 纯 js 实现，根据点击点的坐标来计算

## title 与 h3 、b 与 strong、i 与 em 的区别？

title 属性没有明确意义，只表示是个标题，H1则表示层次明确的标题，对页面信息的抓取也有很大影响

strong 是标明重点内容，有语气加强的含义，使用阅读设备阅读时 `<strong>` 内容会重读，而 b 只是展示强调内容

i 内容展示内容为斜体，em 表示强调的文本。

物理样式标签：b、i、u、s、pre

语义样式标签：strong、em、ins、del、code

## 实现不适用 border 画出 1px 高的线，在不同浏览器的标准模式与怪异模式下都能保持一致？

```html
<div style="width: 100%;height: 1px;background-color: black;"></div>
```

## html5 标签的作用？

* 使 web 页面的内容更加有序和规范
* 使搜索引起更加容易按照 HTML5 规则识别出有效的内容
* 使 web 页面更接近一种数据字段和表。

## `src` 和 `href` 的区别？

* `href`：Hypertext Reference 的缩写，超文本引用，它指向一些网络资源，建立和当前元素或者说是本文档的链接关系。在加载它的时候，不会停止对当前文档的处理，浏览器会继续往下走。常用在 `a`、`link` 等标签。

* `src`：source 的所写，表示的是对资源的引用，它指向的内容会嵌入到当前标签所在的位置。由于 `src` 的内容是页面必不可少的一部分，因此浏览器在解析 `src` 时会停下来对后续文档的处理，直到 `src` 的内容加载完毕。常用在 `script`、`img`、`iframe` 标签中，我们建议 js 文件放在 HTML 文档的最后面。

`href` 用于建立当前页面与引用资源之间的关系（链接），而 `src` 则会替换当前标签。遇到 `href`，页面会并行加载后续内容；而 `src` 则不同，浏览器需要加载完毕 `src` 的内容才会继续往下走。

## img 的 title 和 alt 有什么区别？

`alt` 屏幕阅读器会将这些描述读给需要使用阅读器的使用者听，让他们知道图像的含义。如果由于某种原因无法加载图像，普通浏览器也会在页面上显示 `alt` 属性中的备用文本。

`title` 元素的值一般作为提示条(tooltip)呈现给用户，在光标于图片上停下后显示出来。

## 表单的基本组成部分有哪些，表单的主要用途是什么？

**组成：**表单标签、表单域、表单按钮

表单标签：这里面包含了处理保单数据所用的 CGI 程序的 URL，以及数据提交到服务器的方法。

表单域：包含了文本框、密码框、隐藏域、多行文本、复选框、单选框、下拉选择框、文件上传框等表单控件

表单按钮：包括提交按钮，复位按钮和一般按钮。用于将数据传送到服务器上或取消输入，还可以用表单按钮来控制其他定义了处理脚本的处理工作。

**主要用于：**表单在网页中的中主要负责数据采集的功能，和向服务器传送数据。

## 说说你对 retina屏，pc，ipad，手机端的适配经验和方案？



## 参考文档

[MDN: HTML 元素参考](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element)