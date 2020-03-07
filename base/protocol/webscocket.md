# WebSocket

WebSocket 是 HTML5 开始提供的一种浏览器与服务器间进行全双工通讯的通信技术。依靠这种技术可以实现客户端和服务端的长连接，双向实时通信。

特点：

* 事件驱动
* 异步
* 使用 ws 或者 wss 协议进行加密的 socket
* 能够实现真正意义上的推送功能

## webSocket 如何兼容低浏览器？

* Adobe Flash Socket
* ActiveX HTMLFile（IE\)
* 轮询