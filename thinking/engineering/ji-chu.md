# 工程化面试题

### webpack与grunt、gulp的不同

gulp 是基于任务和流的工程化工具，适用于流程化的场景。

webpack 主要是基于入口的模块化打包器，通过各种 loader 和插件来完成复杂的任务，在模块化上更具优势。

## 有哪些常见的Loader？他们是解决什么问题的？

* vue-loader

  加载和转译 Vue 组件

* url-loader

  像 file loader 一样工作，但如果文件小于限制，可以返回 data URL

  比如 图片、音频、视频、字体文件

* babel-loader

  加载 ES2015+ 代码，然后使用 Babel 转译为 ES5

* ts-loader

* scss-loader

* vue-style-loader

* raw-loader 加载文件原始内容（utf-8）

* file-loader 将文件发送到输出文件夹，并返回（相对）URL

https://www.webpackjs.com/loaders/

## 有哪些常见的Plugin？他们是解决什么问题的？

dev:

* webpack.DefinePlugin

* webpack.HotModuleReplacementPlugin

* webpack.NamedModulesPlugin

* webpack.NoEmitOnErrorsPlugin

* HtmlWebpackPlugin

* SkeletonWebpackPlugin

prod:

* webpack.DefinePlugin

* UglifyJsPlugin

* ExtractTextPlugin

* OptimizeCSSPlugin

* HtmlWebpackPlugin

* webpack.HashedModuleIdsPlugin

* webpack.optimize.ModuleConcatenationPlugin

* webpack.optimize.CommonsChunkPlugin

* CopyWebpackPlugin

* CompressionWebpackPlugin

* BundleAnalyzerPlugin

Loader和Plugin的不同？
webpack的构建流程是什么?从读取配置到输出文件这个过程尽量说全
是否写过Loader和Plugin？描述一下编写loader或plugin的思路？
webpack的热更新是如何做到的？说明其原理？
如何利用webpack来优化前端性能？（提高性能和体验）
如何提高webpack的构建速度？
怎么配置单页应用？怎么配置多页应用？
npm打包时需要注意哪些？如何利用webpack来更好的构建？
如何在vue项目中实现按需加载？