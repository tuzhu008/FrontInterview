# BFC、IFC

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

## IFC



