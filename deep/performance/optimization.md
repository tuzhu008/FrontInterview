# 性能优化

在讨论性能时，必须要做到精确，并且要根据可量化衡量的客观标准来指代性能。这些条件称为“指标”。

## 如何进行性能优化

## 性能分析

* performace

* Lighthouse


## 指标类型

还有其他几种类型的指标，与用户对性能的感受相关。

* 感知的加载速度：网页加载并将其所有视觉元素呈现到屏幕上的速度。
* 加载响应速度：网页加载和执行任何必需的 JavaScript 代码的速度，以便让组件快速响应用户互动
* 运行时响应速度：网页加载后，网页能够以多快的速度响应用户互动。
* 视觉稳定性：网页上的元素是否会以用户意想不到且可能会干扰互动的方式发生变化？
* 流畅性：过渡和动画是否以一致的帧速率渲染并从一种状态流畅地流向另一种状态？

鉴于上述所有类型的性能指标，很明显，单个指标不足以捕获网页的所有性能特征。

## 性能指标


* FCP（First Contentful Paint）白屏时间，值越低越好；
* SI（Speed Index）页面渲染时间，值越低越好；
* LCP（Largest Contentful Paint）可视窗口最大内容渲染时间，值越低越好
* TTI（Time to Interactive）用户可交互时间，值越低越好；
* TBT（Total Blocking Time）用户行为阻塞时间，值越低越好；
* CLS（Cumulative Layout Shift）可视窗口中累计可见元素布局偏移；
* FID（First Input Delay）用户首次交互时间，值越低越好

### FCP(First Contentful Paint)

首次内容绘制 (FCP) 指标用于测量从网页开始加载到网页任何一部分内容呈现在屏幕上的时间。对于此指标，“内容”是指文本、图片（包括背景图片）、`<svg>` 元素或非白色 `<canvas>` 元素，不包括 iframe 中的元素。

FCP优化策略：

* 消除阻塞渲染的资源
* 预先连接到所需的源
* 避免多页重定向
* 预加载密钥请求
* 避免网络负载过大
* 最大限度地降低关键请求深度
* 尽量减少请求数和传输大小
  * 缩减 CSS 大小
  * 移除未使用的 CSS
  * 移除未使用的 JavaScript
* 缓存优化
  * 利用高效的缓存政策提供静态资源
* 缩短服务器响应时间 (TTFB)
  * DNS解析优化。DNS缓存优化、DNS预加载策略、稳定可靠的DNS服务器。
  * 服务端处理优化。Redis缓存、数据库存储优化或是系统内的各种中间件以及Gzip压缩等...
  * CDN加速
* 渲染优化：
  * 避免 DOM 规模过大
  * 字体加载优化。确保文本在网页字体加载期间保持可见。`font-display: swap;`

## LCP(Largest Contentful Paint)

Largest Contentful Paint (LCP) 指标会报告视口内可见的最大图片或文本块的呈现时间（相对于网页首次开始加载的时间）。

根据[Largest Contentful Paint API][w3c-LCP] 中当前的规定，Largest Contentful Paint 考虑的元素类型包括：

* `<img>` 元素
* `<svg>` 元素内的 `<image>` 元素
* 包含海报图片的 `<video>` 元素（系统会使用海报图片加载时间）
* 一个元素，带有通过 `url()` 函数（而不是 CSS 渐变）加载的背景图片
* 包含文本节点或其他内嵌级别文本元素的子项的块级元素。
* 为自动播放 `<video>` 元素而绘制的第一帧（截至 2023 年 8 月）
* 动画图片格式（例如 GIF 动画）的第一帧（截至 2023 年 8 月）

除了考虑某些元素之外，系统还会应用某些启发法来排除某些可能被视为“非内容”的元素。对于基于 Chromium 的浏览器，包括：

* 不透明度为 0 且对用户不可见的元素
* 覆盖整个视口的元素，很可能被视为背景而非内容
* 占位符图片或其他低熵的图片，可能无法反映网页真实内容

浏览器可能会继续改进这些启发法，以确保匹配用户对于最大内容元素的预期。


## SI(Speed Index)页面内容呈现的速度

工具：Lighthouse

SI 测量在页面加载期间内容视觉显示的速度。

Lighthouse 会先捕捉页面加载的视频并计算帧与帧之间的视觉进度。
然后在通过 [Speedline Node.js module](https://github.com/paulirish/speedline)计算 SI 评分。

优化策略

任何提升网页加载速度的方式都会提升 SI。

* 最小化主线程的任务
* 减小 JavaScript 执行时间
* 确保文字在字体加载期间一直显示

SI优化策略
1、优化主进程（renderer process），包括解析HTML，创建DOM树，解析CSS，执行JavaScript

优化第三方JavaScript（First Load Time）
利用防抖优化用户输入事件
利用web worker
减少复杂的样式计算和嵌套
避免大的、复杂的布局和布局回流；
简化页面绘制和减少绘制区域
减少冗余代码
8、异步加载非必须的css

## TBT(Total Blocking Time)

TBT 衡量的是从响应到用户输入之间页面被阻塞的总时长，比如鼠标点击，屏幕触摸，键盘输入等。
该时长是通过将 FCP 和 TTI 之间的所有长时任务的阻塞部分相加来计算的。
执行超过 50ms 的任务被认定为长时任务。超过 50ms 的部分被认为是阻塞部分。
例如，70ms 的长时任务的阻塞部分为 20ms。

通常，常见的长时任务有：

* 非必要的 JavaScript 加载，解析或执行。

* 低效的 JavScript 语句。

优化策略：

* 减少 JavaScript 有效负载
  * 代码分割
  * 删除无用代码
  * 高校加载三方 JavaScript

* 优化低效代码

## CLS(Cumulative Layout Shift)

页面整个生命周期所有元素发生的非预期的布局累计偏移值。

如何减少CLS
给image，video或者其他具有长宽比的元素设置长宽值；
尽量不要在已经渲染的内容前面插入内容；
尽量选择transform animations属性去触发布局变动；

## TTI(用户可交互速度)
页面呈现的可用内容速度，比如最大内容渲染速度；
大多数页面元素注册的事件，并且用户交互响应速度在50毫秒以内；

TTI优化策略
1、加载采用PRPL模式

Push (or preload) the most important resources.
Render the initial route as soon as possible.
Pre-cache remaining assets.
Lazy load other routes and non-critical assets.

2、减少因代码切割导致的依赖

3、去除无用代码
可以通过eslint，webpack等工具发现和去除无用的冗余代码。

4、压缩代码和数据
无论是早期的grunt，gulp，还是现在流行的webpack，都提供了快捷方便的代码压缩工具。


[w3c-LCP]:https://w3c.github.io/largest-contentful-paint/