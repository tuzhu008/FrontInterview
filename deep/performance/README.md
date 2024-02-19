# 前端性能

前端性能，可以分为量化性能和感知性能。

量化性能通常使用性能指标来衡量，不同的性能测量工具定义了不同的性能指标，相同的性能指标的计算规则也不尽相同。

## 感知性能

* Loading
* Transition
* Animation

## Metics

* TTFB
* FCP
* LCP
* FID
* TTI
* TBT
* CLS
* INP
* SI

## References

[MDN Web Performance](https://developer.mozilla.org/en-US/docs/Web/Performance)

[MDN Web performance guide](https://developer.mozilla.org/en-US/docs/Learn/Performance)

[Rendering performance](https://web.dev/articles/rendering-performance)

[Measure performance with the RAIL model](https://web.dev/articles/rail)

### 用户体验

* Respond to user input in under 100 ms. process events in under 50ms

* Produce a frame in under 10 ms when animating or scrolling.

* Maximize main thread idle time.

* Load interactive content in under 5000 ms.

参考 [UX research on how users perceive delays](https://www.nngroup.com/articles/response-times-3-important-limits/)

### 指标

[Metrics](https://web.dev/explore/metrics)

### discrete actions And continuous ations

discrete actions，离散事件。如 clicks, taps, and key presses。

continuous ations，连续事件。如 scrolling，zooming。


https://godbasin.github.io/front-end-playground/front-end-basic/performance/front-end-performance-optimization.html