# Ajax

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
