# HTML5

## HTML5 优缺点？

**优点：**

* 网络标准统一、HTML5 本身是由 W3C 推荐出来的
* 多设备，跨平台
* 即时更新
* 提高了可用性和改进了用户的体验
* 新增语义化的标签，有助于页面结构的清晰，便于维护，利于 SEO
* 新增了原生的多媒体元素
* 更便捷的客户端存储

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

## HTML 行内元素有哪些，块级元素有哪些，空元素有哪些？

行内元素：

* b, big, i, small, tt
* abbr, acronym, cite, code, dfn, em, kbd, strong, samp, var
* a, bdo, br, img, map, object, q, script, span, sub, sup
* button, input, label, select, textarea\* 

块级元素：

| 元素 | 版本 |
| :--- | :---: |
| `<address>` |  |
| `<blockquote>` |  |
| `<dd>` |  |
| `<div>` |  |
| `<dl>` |  |
| `<fieldset>` |  |
| `<form>` |  |
| `<h1>, <h2>, <h3>, <h4>, <h5>, <h6>` |  |
| `<hr>` |  |
| `<noscript>` |  |
| `<ol>` |  |
| `<p>` |  |
| `<pre>` |  |
| `<table>` |  |
| `<tfoot>` |  |
| `<ul>` |  |
| `<footer>` | 5 |
| `<header>` | 5 |
| `<aside>` | 5 |
| `<section>` | 5 |
| `<article>` | 5 |
| `<video>` | 5 |
| `<audio>` | 5 |
| `<hgroup>` | 5 |
| `<output>` | 5 |
| `<canvas>` | 5 |
| `<figcaption>` | 5 |
| `<figure>` | 5 |


空元素：

| 元素 | 版本 |
| :--- | :---: |
| `<area>` |  | 
| `<base>` |  | 
| `<br>` |  | 
| `<col>` |  | 
| `<colgroup>` |  | 
| `<command>` |  | 
| `<embed>` |  | 
| `<hr>` |  | 
| `<img>` |  | 
| `<input>` |  | 
| `<keygen>` |  | 
| `<link>` |  | 
| `<meta>` |  | 
| `<param>` |  | 
| `<source>` |  | 
| `<track>` |  | 
| `<wbr>` |  | 



