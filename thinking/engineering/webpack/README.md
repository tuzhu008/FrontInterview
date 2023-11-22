
# Webpack

webpack是一个前端打包器，帮助开发者将js模块（各种类型的模块化规范）打包成一个或多个js脚本。webpack的工作过程可以分为依赖解析过程和代码打包过程，首先执行对应的build命令，webpack首先分析入口文件，会递归解析AST获取对应依赖，得到一个依赖图。然后为每一个模块添加包裹函数（webpack的模块化），从入口文件为起点，递归执行模块，进行拼接IIFE（立即调用函数表达式：保证了模块变量不会影响全局作用域），产出对应的bundle。

## webpack中plugin和loader分别做什么？它们之间的执行顺序？

* loader：用于将不同类型的文件转换成webpack可以识别的文件（webpack只认识js和json）。
* plugin：存在于webpack整个生命周期中，是一种基于事件机制工作的模式，可以在webpck打包过程对某些节点做某些定制化处理。同时plugin可以对loader解析过程中做一些处理，协同处理文件。
执行顺序：两者不存在明显的先后顺序，不过webpack在初始化处理时，会优先识别到plugin中的内容。

## webpack常见的优化方案

* 基于esm的tree shaking
* 对balel设置缓存，缩小babel-loader的处理范围,及精准指定要处理的目录。
* 压缩资源（mini-css-extract-plugin，compression-webpack-plugin）
* 配置资源的按需引入（第三方组件库）
* 配置splitChunks来进行按需加载（根据）
* 设置CDN优化

## webpack 的打包流程是什么样的

## webpack 是用来做什么的，原理是什么

## webpack 中的 loader 的作用是什么

## 有没有自己写过一个webpack的loader

## webpack 中plugin的作用是什么，有没有自己写过

## 使用 webpack 时如何优化项目体积

## webpack 中什么是 HMR，原理是什么

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