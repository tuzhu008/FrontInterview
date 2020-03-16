# Ajax

Asynchronous JavaScript And XML 的简称。

这一技术能够向服务器请求额外的数据而无需卸载页面，会带来更高的用户体验。

Ajax 技术的核心是 XMLHttpRequest 对象，简称 XHR。XHR 为向服务器发送请求和解析服务器提供了流畅的接口。

IE7+ 支持原生的 XMLHttpRequest 对象。

XHR 需要遵守同源策略，只能向同一个域中使用相同端口和协议的 URL 发送请求。

## API

* open

* send

* abort

* setRequestHeader

  必须在调用 `open()` 方法之后且调用 `send()` 方法之前调用

* getResponseHeader

* getAllResponseHeaders

readystate

| 取值 | 描述 | 触发行为 |
| :--- | :--- | :--- |
| 0 | 未初始化 | 尚未调用 `open()` 方法 |
| 1 | 启动 | 已调用 `open()`，尚未调用 `send()` |
| 2 | 发送 | 已调用 `send()`，但尚未接收到响应 |
| 3 | 接收 | 已接收到部分响应数据 |
| 5 | 完成 | 接收到全部响应数据，数据处于可用状态 |


## 事件

* readystatechange
* timeout
* load

其他事件：

* loadstart
* progress
* error
* abort
* loadend

## Ajax 的交互模型？

## 如何解决跨域问题?

jsonp

iframe

window.name、window.postMessage

代理

### CORS

CORS 是 Cross-Origin Resource Sharing 的简称。它定义了在必须访问跨源资源时，浏览器与服务器应该如何沟通。

CORS 背后的基本思想，就是使用自定义的 HTTP 头部让浏览器与服务器进行沟通，从而决定请求或者响应是应该成功，还是应该失败。

* 为请求添加 `Origin` 头部，其中包含请求页面的源信息

  ```
  Origin: https://www.test.com
  ```
* 服务器如果认为这个请求可以接受，就在 `Access-Control-Allow-Origin` 响应头部中慧发相同源信息

  ```
  Access-Control-Allow-Orig: https://www.test.com
  ```

  如果没有这个响应头，或者这个头部的源信息不匹配，浏览器就会驳回请求。

请求和响应都不包含 cookie 信息。

## Ajax 的过程是怎样的

1. 创建 XMLHttpRequest 对象，也就是创建一个异步调用对象
2. 创建一个新的 HTTP 请求，并指定该 HTTP 请求的方法，URL 以及验证信息
3. 设置 响应 HTTP 请求状态变化的函数
4. 发送 HTTP 请求
5. 获取异步调用返回的数据
6. 使用 JavaScript 和 DOM 实现局部刷新

```js
const xhr = new XMLHttpRequest();

// 请求成功回调函数
xhr.onload = (e) => {
  console.log(request success);
}

// 请求结束
xhr.onloaded = (e) => {
  console.log(request loadend);
}

// 请求超时
xhr.ontimeout = (e) => {
    console.log('request timeout');
};


xhr.timeout = 0; // 设置超时时间,0表示永不超时
// 初始化请求
xhr.open('GET/POST/DELETE/...', '/url', true || false);
// 设置期望的返回数据类型 'json' 'text' 'document' ...
xhr.responseType = '';
// 设置请求头
xhr.setRequestHeader('', '');
// 发送请求
xhr.send(null || new FormData || 'a=1&b=2' || 'json字符串');
```

## GET 请求传参长度的误区

Http 协议并未规定 GET 和 POST 的长度限制

GET 长度限制是由浏览器和web 服务器做出的限制，且各有区别。

## 解决 ajax 缓存问题

* 设置Header：`If-Modified-Since: 0`
* 设置Header：`Cache-Control: no-cache`
* URL 拼接随机参数

## urlEndoce

```js
function urlEncode(data) {
  var _urlEncode = function(param, key, encode) {
    if (param == null) {
      return '';
    }
    var paramStr = '';
    var type = typeof param;
    if (type == 'string' || type == 'number' || type == 'boolean') {
      paramStr += '&' + key + '=' + ((encode == null || encode) ? encodeURIComponent(param) : param);
    } else {
      for (var i in param) {
        var k = key == null ? i: key + (param instanceof Array ? '[' + i + ']' : '.' + i);
        paramStr += _urlEncode(param[i], k, encode);
      }
    }
    return paramStr;
  }

  return _urlEncode(data).slice(1);
}
```
