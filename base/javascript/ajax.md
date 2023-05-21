# Ajax

Asynchronous JavaScript And XML 的简称。

这一技术能够向服务器请求额外的数据而无需卸载页面，会带来更高的用户体验。

Ajax 技术的核心是 XMLHttpRequest 对象，简称 XHR。XHR 为向服务器发送请求和解析服务器提供了流畅的接口。

IE7+ 支持原生的 XMLHttpRequest 对象。

XHR 需要遵守同源策略，只能向同一个域中使用相同端口和协议的 URL 发送请求。

## API

* open

  初始化一个请求。

* send

  发送请求。如果请求是异步的（默认），那么该方法将在请求发送后立即返回。

* abort

  如果请求已被发出，则立刻中止请求。

* setRequestHeader

  必须在调用 `open()` 方法之后且调用 `send()` 方法之前调用

* getResponseHeader

  返回包含指定响应头的字符串，如果响应尚未收到或响应中不存在该报头，则返回 null。

* getAllResponseHeaders

  以字符串的形式返回所有用 CRLF 分隔的响应头，如果没有收到响应，则返回 null。

readystate

| 取值 | 描述 | 触发行为 |
| :--- | :--- | :--- |
| 0 | `UNSENT` | 代理被创建，尚未调用 `open()` 方法 |
| 1 | `OPENED` | 已调用 `open()`，尚未调用 `send()` |
| 2 | `HEADERS_RECEIVED` | `send()` 方法已经被调用，并且响应头部和状态已经可获得。 |
| 3 | `LOADING` | 已接收到部分响应数据 |
| 4 | `DONE` | 接收到全部响应数据，数据处于可用状态 |

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

## Ajax 的交互模型？

## 如何解决跨域问题?

jsonp

iframe

window.name、window.postMessage

window.domain

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
  Access-Control-Allow-Origin: https://www.test.com
  ```

  如果没有这个响应头，或者这个头部的源信息不匹配，浏览器就会驳回请求。

请求和响应都不包含 cookie 信息。

默认情况喜爱，跨源请求不提供凭据。跨源请求设置 `withCreDentials` 为 `true`，可以指定某个请求应该发送凭证。如何服务器接受带凭据的请求，会用下面的 HTTP 头部来响应。

```
Access-Control-Allow-Credentials: true
```

### 图像 Ping

图像 Ping 是与服务器进行简单、单向的跨域服务通信的一种方式。

**原理：**利用 `<img>` 能从任何网页中加载图像，且不用担心跨域的特性。

```js
var img = new Image();
img.onload = img.onload = function () {
  alert('onload');
}
img.src = 'http://www.examole.com/1e332132';
```

**缺点：**

* 只能发送 GET 请求

* 无法访问服务器的响应文本

### JSONP

JSONP 是 JSON with padding 的简写。

**原理：**利用 `<script>` 能从任何网页中加载脚本，且不用担心跨域的特性。

JSONP 看起来与 JSON 差不多，只不过是被包含在函数调用中的 JSON：

```js
callback({ "name": "wahaha" });
```

JSONP 由两部分组成：回调函数和数据。

* 回调函数：当响应到来时应该在页面中调用的函数，回调函数的名字一般是在请求中指定的

* 数据：传入回调函数的 JSON 数据

```js
var script = document.createElement('script');
script.src = 'http://www.examole.com/dasda?callback=handleRequst';
document.body.appendChild(script);
```

**优点：**能够直接访问响应文本，直接在浏览器和服务器间进行双向通信。

**缺点：**

* 安全：JSONP 是从其他域中加载代码执行，有可能含有恶意代码。

* 不易确认请求状态

### Comet

Comet 是服务器向页面推送数据的技术。

实现 Comet 的方式：**长轮询**和**流**

### WebSocket

同源策略对 WebSocket 不适用。至于是否会与某个域中的页面通信，则完全取决于服务器。

WebSocket 只能通过连接发送纯文本数据，所以对于复杂的数据结构，在通过连接发送之前必须进行序列化。

## 将原生的 ajax 封装成 promise

```js
function ajax (options) {
    const { method = 'GET', url, headers = {}, timeout = 20000, data } = options;
    return new Promise(function (resolve, reject) {
        const xhr = new XMLHttpRequest();
        const headerEntries = Object.entries(headers);
        let requestURL = url;
        let requestBody = data;

        if (method === 'GET' || method === 'HEAD') {
            requestURL = encode(url, data);
            requestBody = null;
        } else {
            requestBody = JSON.parse(data);
        }

        xhr.open(method, url);

        for (let [key, value] of headerEntries) {
            xhr.setRequestHeader(key, value);
        }

        xhr.onreadystatechange = function () {
            if (xhr.readyState === XMLHttpRequest.DONE) {
                if (xhr.status === 200) {
                    resolve(JSON.parse(xhr.responseText));
                } else {
                    reject();
                }

            }
        }

        xhr.timeout = function () {
            reject('超时');
        }

        xhr.send(requestBody);
    });
}
```



