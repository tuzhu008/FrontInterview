# 回流、重绘

## 回流

通过构造渲染树，将可见 DOM 节点以及它对应的样式结合起来，还需要计算它们在设备视口(viewport)内的确切位置和大小，这个计算的阶段就是回流。

## 重绘

最终，通过构造渲染树和回流阶段，知道了哪些节点是可见的，以及可见节点的样式和具体的几何信息(位置、大小)，那么我们就可以将渲染树的每个节点都转换为屏幕上的实际像素，这个阶段就叫做重绘节点。

重绘是一个元素外观的改变所触发的浏览器行为，例如改变visibility、outline、背景色等属性。浏览器会根据元素的新属性重新绘制，使元素呈现新的外观。

## 何时发生回流重绘

我们前面知道了，回流这一阶段主要是计算节点的位置和几何信息，那么当页面布局和几何信息发生变化的时候，就需要回流。比如以下情况：

* 添加或删除可见的 DOM 元素

* 元素的位置发生变化

* 元素的尺寸发生变化（包括外边距、内边框、边框大小、高度和宽度等）

* 内容发生变化，比如文本变化或图片被另一个不同尺寸的图片所替代。

* 页面一开始渲染的时候（这肯定避免不了）

* 浏览器的窗口尺寸变化（因为回流是根据视口的大小来计算元素的位置和大小的）

**注意：回流一定会触发重绘，而重绘不一定会回流**

根据改变的范围和程度，渲染树中或大或小的部分需要重新计算，有些改变会触发整个页面的重排，比如，滚动条出现的时候或者修改了根节点。

## 减少回流和重绘

### 最小化重绘和重排

由于重绘和重排可能代价比较昂贵，因此最好就是可以减少它的发生次数。为了减少发生次数，我们可以合并多次对DOM和样式的修改，然后一次处理掉。考虑这个例子：

```js
const el = document.getElementById('test');
el.style.padding = '5px';
el.style.borderLeft = '1px';
el.style.borderRight = '2px';
```

例子中，有三个样式属性被修改了，每一个都会影响元素的几何结构，引起回流。当然，大部分现代浏览器都对其做了优化，因此，只会触发一次重排。但是如果在旧版的浏览器或者在上面代码执行的时候，有其他代码访问了布局信息(上文中的会触发回流的布局信息)，那么就会导致三次重排。

因此，我们可以合并所有的改变然后依次处理，比如我们可以采取以下的方式：

* 使用cssText

  ```js
  const el = document.getElementById('test');
  el.style.cssText += 'border-left: 1px; border-right: 2px; padding: 5px;';
  ```

* 修改CSS的class

  ```js
  const el = document.getElementById('test');
  el.className += ' active';
  ```

### 批量修改 DOM

当我们需要对DOM对一系列修改的时候，可以通过以下步骤减少回流重绘次数：

* 使元素脱离文档流

* 对其进行多次修改

* 将元素带回到文档中。

该过程的第一步和第三步可能会引起回流，但是经过第一步之后，对DOM的所有修改都不会引起回流，因为它已经不在渲染树了。

有三种方式可以让DOM脱离文档流：

* 隐藏元素，应用修改，重新显示

* 使用文档片段(document fragment)在当前DOM之外构建一个子树，再把它拷贝回文档。

* 将原始元素拷贝到一个脱离文档的节点中，修改节点后，再替换原始的元素。

### 避免触发同步布局事件

### 对于复杂动画效果,使用绝对定位让其脱离文档流

### css3硬件加速（GPU加速）

比起考虑如何减少回流重绘，我们更期望的是，根本不要回流重绘。这个时候，css3硬件加速就闪亮登场啦！！

划重点：使用css3硬件加速，可以让transform、opacity、filters这些动画不会引起回流重绘 。但是对于动画的其它属性，比如background-color这些，还是会引起回流重绘的，不过它还是可以提升这些动画的性能。

常见的触发硬件加速的css属性：

* transform

* opacity

* filters

* Will-change

css3硬件加速的坑：

* 如果你为太多元素使用css3硬件加速，会导致内存占用较大，会有性能问题。

* 在GPU渲染字体会导致抗锯齿无效。这是因为GPU和CPU的算法不同。因此如果你不在动画结束的时候关闭硬件加速，会产生字体模糊。

## 参考

https://segmentfault.com/a/1190000017329980