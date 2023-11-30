
# Webpack

webpack是一个前端打包器，帮助开发者将js模块（各种类型的模块化规范）打包成一个或多个js脚本。webpack的工作过程可以分为依赖解析过程和代码打包过程，首先执行对应的build命令，webpack首先分析入口文件，会递归解析AST获取对应依赖，得到一个依赖图。然后为每一个模块添加包裹函数（webpack的模块化），从入口文件为起点，递归执行模块，进行拼接IIFE（立即调用函数表达式：保证了模块变量不会影响全局作用域），产出对应的bundle。

## 构建流程

![webpack 构建流程](/assets/webpack-flow.webp)

1. 初始化参数。从配置文件和命令行读取与合并参数。
1. 开始编译。用最终的配置参数初始化  Compiler 实例，加载所有配置的插件，通过执行编译器实例的 `run` 方法开始执行编译。
1. 确定入口。根据配置中的 entry 找出所有的入口文件。
1. 编译模块。从入口文件出发，调用配置的 loader 对模块进行翻译，找出其所有依赖的模块，然后递归对这些依赖模块进行处理。
1. 完成模块编译。经过编译后，得到每个模块被翻译后的内容，以及他们之间的依赖关系。这种依赖关系形成了一种树状结构。
1. 输出资源。根据入口和模块之间的关系，组装成一个个包含多个模块的 chunk，再将每个 chunk 转换成一个个单独的文件加入到输出列表，这是可以修改输出内容的最后机会。
1. 输出完成。在确定好输出内容后，根据配置确定输出的路径和文件名，将文件内容写入到文件系统中。

## loader

在功能角度讲，其目的是为了让 webpack 拥有加载和解析非 JavaScript 文件的能力。

在工程化的角度讲，其目的是为了支持模块化的开发，允许在程序中直接以 `import` 的模式导入 CSS，image 等文件。

Loader 本质就是一个函数。对函数中接收到的内容进行转换，返回转换后的结果。

## 插件

插件目的在于解决 loader 无法实现的其他事。

webpack 插件是一个具有 `apply` 方法的 JavaScript 对象。`apply` 方法会被 webpack compiler 调用，并且在**整个**编译生命周期都可以访问 compiler 对象。

插件存在于webpack整个生命周期中，是一种基于事件机制工作的模式，可以在webpck打包过程对某些节点做某些定制化处理。

同时plugin可以对loader解析过程中做一些处理，协同处理文件。

```js
const pluginName = 'ConsoleLogOnBuildWebpackPlugin';

class ConsoleLogOnBuildWebpackPlugin {
  apply(compiler) {
    compiler.hooks.run.tap(pluginName, (compilation) => {
      console.log('webpack 构建正在启动！');
    });
  }
}

module.exports = ConsoleLogOnBuildWebpackPlugin;
```

## webpack中plugin和loader分别做什么？它们之间的执行顺序？

执行顺序：两者不存在明显的先后顺序，不过webpack在初始化处理时，会优先识别到plugin中的内容。

运行时机不一样

Loader 运行在打包文件之前（loader为在模块加载时的预处理文件）。plugins 在整个编译周期都起作用


## tree shaking

## webpack 如何优化构建体积跟速度
## webpack常见的优化方案

速度：
  对balel设置缓存，缩小 babel-loader 的处理范围，及精准指定要处理的目录。
  * 启用一些 loader 的缓存。
  * 精准配置 loader 的生效文件夹，`include` 和 `exclude`。
  * cache-loader 、HardSourceWebpackPlugin？
  * sourcemap。根据需要配置合理的 sourcemap。
  * 针对不同的环境采用不同的配置
    * sourcemap。根据需要配置合理的 sourcemap。
    * 代码压缩 `sourcemap。根据需要配置合理的 sourcemap。`
    * CSS 提取
    * 加速依赖下载速度
  <!-- * 加速依赖下载速度 -->

  * TODO:ignorePlugin、noParse

  * 多进程。尽可能利用硬件资源，多线程方式来编译代码。

* 基于 esm 的tree shaking
* 压缩资源（mini-css-extract-plugin，compression-webpack-plugin）
* 提取 CSS、图片
* 配置资源的按需引入（第三方组件库）
* 配置splitChunks来进行按需加载（根据）。按名称、位置等等。
  * 按大小、名称、位置等等。
  * 懒加载
* 设置CDN优化
* externals

## webpack 的打包流程是什么样的

Webpack 的运行流程是一个串行的过程,从启动到结束会依次执行以下流程 :

1. 初始化参数：从配置文件和 Shell 语句中读取与合并参数,得出最终的参数。
1. 开始编译：用上一步得到的参数初始化 Compiler 对象,加载所有配置的插件,执行对象的 run 方法开始执行编译。
1. 确定入口：根据配置中的 entry 找出所有的入口文件。
1. 编译模块：从入口文件出发,调用所有配置的 Loader 对模块进行翻译,再找出该模块依赖的模块,再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理。
1. 完成模块编译：在经过第 4 步使用 Loader 翻译完所有模块后,得到了每个模块被翻译后的最终内容以及它们之间的依赖关系。
1. 输出资源：根据入口和模块之间的依赖关系,组装成一个个包含多个模块的 Chunk,再把每个 Chunk 转换成一个单独的文件加入到输出列表,这步是可以修改输出内容的最后机会。
1. 输出完成：在确定好输出内容后,根据配置确定输出的路径和文件名,把文件内容写入到文件系统。

在以上过程中,Webpack 会在特定的时间点广播出特定的事件,插件在监听到感兴趣的事件后会执行特定的逻辑,并且插件可以调用 Webpack 提供的 API 改变 Webpack 的运行结果。


## babel-loader、babel/preset-env、babel-polyfill、babel-transform-runtime 什么区别

* babel-loader 把babel跟webpack连接上。相当于是桥梁。实际上还不能把es6转成es5

* babel/preset-env 预设，能把let语句等翻译成es5（但是promise、generator等语法不行）

* babel-polyfill 才真正的支持Promise等语法

* @babel-polyfill包 = corejs包 + regenerator包

## webpack 是用来做什么的，原理是什么

## webpack 中的 loader 的作用是什么

## 有没有自己写过一个webpack的loader

## webpack 中plugin的作用是什么，有没有自己写过

## 使用 webpack 时如何优化项目体积

## webpack 中什么是 HMR，原理是什么


Webpack 的热更新又称热替换（Hot Module Replacement），缩写为 HMR。 可以做到无需完全刷新整个页面的同时，保留页面状态并且更新模块。提到热更新我们不得不知道另一个概念——WDS。webpack-dev-server(WDS)是一个基于express的web server，server内部调用webpack，这样的好处是提供了热加载和热替换的功能

热更新的核心就是客户端从服务端拉取更新后的文件，准确的说是 chunk diff (chunk 需要更新的部分)，实际上 WDS 与浏览器之间维护了一个Websocket，当本地资源发生变化时，WDS 会向浏览器推送更新，并带上构建时的 hash，让客户端与上一次资源进行对比。客户端对比出差异后会向 WDS 发起Ajax请求来获取更改内容(文件列表、hash)，这样客户端就可以再借助这些信息继续向 WDS 发起jsonp请求获取该chunk的增量更新。

## 使用 webpack 打包时，如何更好地利用 long term cache

## 随着 http2 的发展，webpack 有没有更好的打包方案

## 对于已经 import 但未实际使用的模块使用 webpack 还会对它打包吗？

## 使用 webpack 如何分包

## webpack 的 runtime 做了什么事情

## webpack 中的 code spliting 是如何动态加载 chunk 的？

## 打包器(webpack/rollup) 如何将打包后的 js 资源注入 html 中

## 打包器(webpack/rollup) 如何加载 json、image 等非 Javascript 资源

## 打包器(webpack/rollup) 如何加载 style 样式资源

## 如何提升 webpack 构建资源的速度

## webpack 中是如何处理 new URL 资源的