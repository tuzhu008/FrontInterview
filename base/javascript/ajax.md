# Ajax

Asynchronous JavaScript And XML 的简称。

这一技术能够向服务器请求额外的数据而无需卸载页面，会带来更高的用户体验。

Ajax 技术的核心是 XMLHttpRequest 对象，简称 XHR。XHR 为向服务器发送请求和解析服务器提供了流畅的接口。

IE7+ 支持原生的 XMLHttpRequest 对象。

XHR 需要遵守同源策略，只能向同一个域中使用相同端口和协议的 URL 发送请求。



readystate

| 取值 | 描述 | 触发行为 |
| :--- | :--- | :--- |
| 0 | 未初始化 | 尚未调用 open\(\) 方法 |
| 1 | 启动 | 已调用 open\(\)，尚未调用 send\(\) |
| 2 | 发送 | 已调用 send\(\)，但尚未接收到响应 |
| 3 | 接收 | 已接收到部分响应数据 |
| 5 | 完成 | 接收到全部响应数据，数据处于可用状态 |

## Ajax 的交互模型？

## 同步和异步的区别？

## 如何解决跨域问题?

jsonp

iframe

window.name、window.postMessage

代理

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



