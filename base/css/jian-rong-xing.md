# 兼容性

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

## click 在 ios 上有300ms 的延迟，原因及如何解决？

原因：ios 上有双击缩放的功能，点击屏幕浏览器无法判断用户是想要进行单击操作还是双击操作，所以要等待 300ms 内是否有第二次点击。

解决办法：

* 第一种：禁止缩放

  ```html
  <meta name="viewport" content="width=device-width, user-scalable=no">
  ```

* 第二种：[fastclick](https://github.com/ftlabs/fastclick)

  原理：检测到 touchend 事件后，立刻触发模拟 click 事件，并且把浏览器 300ms 之后真正触发的事件给组断掉

## 说说你对 retina屏，pc，ipad，手机端的适配经验和方案？
