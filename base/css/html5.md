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

### 行内元素：

* b, big, i, small, tt
* abbr, acronym, cite, code, dfn, em, kbd, strong, samp, var
* a, bdo, br, img, map, object, q, script, span, sub, sup
* button, input, label, select, textarea\* 

### 块级元素：

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


### 空元素：

| 元素 | 版本 |
| :--- | :---: |
| `<img>` |  | 
| `<input>` |  | 
| `<hr>` |  | 
| `<br>` |  | 
| `<col>` |  | 
| `<link>` |  | 
| `<meta>` |  | 
| `<colgroup>` |  | 
| `<area>` |  | 
| `<base>` |  | 
| `<command>` |  | 
| `<keygen>`（废弃） |  | 
| `<param>` |  | 
| `<wbr>` |  | 
| `<embed>` | 5 | 
| `<source>` | 5 | 
| `<track>` | 5 | 

## HTML5 标准提供了哪些新的 API?

* Media API
* Text Track API
* Application Cache API
* User Interaction
* Data Transfer API
* Command API
* Constraint Validation API
* History API

TODO:展开

## HTML5 应用程序缓存和浏览器缓存有什么区别？

应用程序缓存是 HTML5 的重要特性之一，提供了离线使用的功能，让应用程序可以获取本地的网站内容，
如 HTML，CSS，图片以及 JavaScript。这个特性可以提高网站性能，它的实现借助于 mainfest 文件。

与传统的浏览器缓存相比，比不强制缓存用户访问的网站内容。

应用程序缓存的优点：

* 离线缓存：用户可在离线时使用它们
* 速度：加载速度更快
* 减少服务器负载：浏览器只从服务器下载更新的或者更改过的资源


## audio 和 video 支持的文件格式？

* audio：Mp3、Wav、Ogg
* video：MP4、WebM、Ogg

## HTML 文档类型和字符集是？

```html
<!-- 文档类型 -->
<!doctype html>
<html lang="en">
<head>
    <!-- 字符集 -->
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>

</body>
</html>
```

## html5 标签的作用？

* 使 web 页面的内容更加有序和规范
* 使搜索引起更加容易按照 HTML5 规则识别出有效的内容
* 使 web 页面更接近一种数据字段和表。