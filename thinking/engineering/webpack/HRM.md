# HRM

HMR（Hot Module Replacement）是webpack一个重要的特性，当代码文件修改并保存之后，webapck通过watch监听到文件发生变化，会对代码文件重新打包生成两个模块补丁文件manifest（js） 和一个（或多个）updated chunk（js），将结果存储在内存文件系统中，通过websocket通信机制将重新打包的模块发送到浏览器端，浏览器动态的获取新的模块补丁替换旧的模块，浏览器不需要刷新页面就可以实现应用的更新。


## 原理

关键点：

* webpack dev server

* websocket channel

* mainfest 文件 和 updated chunk 文件

* HRM runtime

webpack dev server 与 浏览器 HRM runtime 之间通过建立 websocket 连接进行通信

webpack dev server 监听文件的更改，然后生成 mainfest 文件 和 updated chunk 文件，并使用 websocket 将本次编译的 hash 推送给浏览器

浏览器使用收到的 hash 获取生成的 mainfest 文件

浏览器根据获取到的 mainfest，使用 ajax 请求对应的更新 chunk

然后浏览器动态替换这些 chunk，并触发更新。