# Metrics

## TTFB

https://web.dev/articles/ttfb

https://web.dev/articles/optimize-ttfb

## FCP

https://web.dev/articles/fcp

## LCP

https://web.dev/articles/lcp

https://web.dev/articles/optimize-lcp

## FID

https://web.dev/articles/fid

https://web.dev/articles/optimize-fid

## TTI

https://web.dev/articles/tti

## TBT

https://web.dev/articles/tbt

## CLS

https://web.dev/articles/cls

https://web.dev/articles/optimize-cls

https://twitter.com/anniesullie/status/1491399685961293828?s=20&t=Qj_nzSRZD0_c-HaAnfr98Q

## INP

## SI


## 测量工具

| Field Tool | TTFB | FCP | LCP | FID | TTI | TBT | CLS | INP | SI |
|:----|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| Chrome User Experience Report | ✅ | ✅ | ✅ | ✅ |  |  |  | ✅ |  |  |
| PageSpeed Insights |  | ✅ | ✅ | ✅ |  |  |  | ✅ |  |  |
| Search Console (Core Web Vitals report) |  | ✅ | ✅ | ✅ |  |  |  | ✅ |  |  |
| web-vitals JavaScript library | ✅ | ✅ | ✅ | ✅ |  |  |  | ✅  |  |  |

| Lab Tool | TTFB | FCP | LCP | FID | TTI | TBT | CLS | INP | SI |
|:----|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| Chrome DevTools | ✅ | ✅ | ✅ |  |  |  | ✅ |  |  |
| Lighthouse |  | ✅ | ✅ |  | ✅ | ✅ | ✅ |  |  |
| PageSpeed Insights |  | ✅ | ✅ |  |  |  | ✅ |  |  |
| WebPageTest | ✅ |  | ✅ |  | ✅ | ✅ | ✅ |  |  |

| JavaScript API | TTFB | FCP | LCP | FID | TTI | TBT | CLS | INP | SI |
|:----|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| Event Timing API |  |  | ✅ | ✅ |  |  |  |  |  |  |
| Largest Contentful Paint API |  |  | ✅ |  |  |  |  |  |  |  |
| Layout Instability API |  |  |  |  |  |  |  | ✅ |  |  |

## 量化指标

| Metric | Good | Need Improvement | Poor | Unit |
|:----|:----|:----|:----|:----|
| TTFB |  |  |  |
| FCP |  |  |  |  |
| LCP |  |  |  |  |
| FID |  |  |  |  |
| TTI | 0–3.8 | 3.9–7.3 | > 7.3 | s |
| TBT | 0-200 | 200-600 | >600 | ms |
| CLS | 0-0.1 | 0.1-0.25 | >0.25 | fraction |
| INP |  |  |  |  |
| SI |  |  |  |  |

## 优化手段

| 优化手段 | TTFB | FCP | LCP | FID | TTI | TBT | CLS | INP | SI |
|:----|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| Eliminate render-blocking resources |  | ✅ |  |  |  |  |  |  |  |
| Minify JavaScript |  |  |  |  | ✅ |  |  |  |  |
| Minify CSS |  | ✅ |  |  |  |  |  |  |  |
| Remove unused CSS |  | ✅ |  |  |  |  |  |  |  |
| Remove unused JavaScript |  | ✅ |  |  |  |  |  |  |  |
| Preconnect to required origins |  | ✅ |  |  | ✅ |  |  |  |  |
| Reduce server response times (TTFB) |  | ✅ |  |  |  |  |  |  |  |
| Avoid multiple page redirects | ✅ | ✅ |  |  |  |  |  |  |  |
| Preload key requests |  | ✅ |  |  | ✅ |  |  |  |  |
| Avoid enormous network payloads |  | ✅ |  |  |  |  |  |  |  |
| Serve static assets with an efficient cache policy |  | ✅ |  |  |  |  |  |  |  |
| Avoid an excessive DOM size |  | ✅ |  |  |  |  |  |  |  |
| Minimize critical request depth |  | ✅ |  |  | ✅ |  |  |  |  |
| Ensure text remains visible during webfont load |  | ✅ |  |  |  |  |  |  |  |
| Keep request counts low and transfer sizes small |  | ✅ |  |  | ✅ | ✅ |  |  |  |
| Break up Long Tasks |  |  |  | ✅ |  |  |  |  |  |
| Optimize your page for interaction readiness |  |  |  | ✅ |  |  |  |  |  |
| Use a web worker |  |  |  | ✅ |  |  |  |  |  |
| Reduce JavaScript execution time |  |  |  | ✅ | ✅ | ✅ |  |  |  |
| Reduce the impact of third-party code |  |  |  |  | ✅ | ✅ |  |  |  |
| Minimize main thread work |  |  |  |  | ✅ | ✅ |  |  |  |
| Use a Content Delivery Network (CDN) | ✅ |  |  |  |  |  |  |  |  |
| Used cached content where possible | ✅ |  |  |  |  |  |  |  |  |
| Use a service worker | ✅ |  |  |  |  |  |  |  |  |
| Use 103 Early Hints for render-critical resources | ✅ |  |  |  |  |  |  |  |  |
| Platform-specific guidance | ✅ |  |  |  |  |  |  |  |  |
| Hosting, hosting, hosting | ✅ |  |  |  |  |  |  |  |  |
| HAlways include `width` and `height size attributes on your images and video elements |  |  |  |  |  |  | ✅ |  |  |
| Statically reserve space for late-loading content |  |  |  |  |  |  | ✅ |  |  |
| Avoid placing late-loading content near the top of the viewport  |  |  |  |  |  |  | ✅ |  |  |
| Avoid inserting new content without a user interaction  |  |  |  |  |  |  | ✅ |  |  |
| Apply Composited animation by use some CSS properties, such as `transform`   |  |  |  |  |  |  | ✅ |  |  |
| [Handle fonts properly，such as fallback font, preload...][Fonts] |  |  |  |  |  |  | ✅ |  |  |
| Ensuring pages are eligible for the [bfcache][bfcache] |  |  |  |  |  |  | ✅ |  |  |


[Fonts]: https://web.dev/articles/font-best-practices
[bfcache]: https://web.dev/articles/bfcache