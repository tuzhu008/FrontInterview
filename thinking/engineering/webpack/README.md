
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