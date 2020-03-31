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

  DefinePlugin 允许创建一个在编译时可以配置的全局常量。

* webpack.HotModuleReplacementPlugin

  启用热替换模块(Hot Module Replacement)，也被称为 HMR。

* webpack.NamedModulesPlugin

  当开启 HMR 的时候使用该插件会显示模块的相对路径，建议用于开发环境。

* webpack.NoEmitOnErrorsPlugin

  在编译出现错误时，使用 NoEmitOnErrorsPlugin 来跳过输出阶段。这样可以确保输出资源不会包含错误。对于所有资源，统计资料(stat)的 emitted 标识都是 false。

* HtmlWebpackPlugin

  HtmlWebpackPlugin简化了HTML文件的创建，以便为你的webpack包提供服务。这对于在文件名中包含每次会随着编译而发生变化哈希的 webpack bundle 尤其有用。 你可以让插件为你生成一个HTML文件，使用lodash模板提供你自己的模板，或使用你自己的loader。

* SkeletonWebpackPlugin

  骨架屏占位

prod:

* webpack.DefinePlugin

* UglifyJsPlugin

* ExtractTextPlugin

  extract-text-webpack-plugin

  抽取 css 到单独的文件中

* optimize-css-assets-webpack-plugin

* HtmlWebpackPlugin

  html-webpack-plugin

* webpack.HashedModuleIdsPlugin

  该插件会根据模块的相对路径生成一个四位数的hash作为模块id, 建议用于生产环境。

* webpack.optimize.ModuleConcatenationPlugin

* webpack.optimize.CommonsChunkPlugin

  CommonsChunkPlugin 插件，是一个可选的用于建立一个独立文件(又称作 chunk)的功能，这个文件包括多个入口 chunk 的公共模块。

* CopyWebpackPlugin

  文件赋值

* CompressionWebpackPlugin

  提供带 Content-Encoding 编码的压缩版的资源

* BundleAnalyzerPlugin

  模块分析器

## Loader和Plugin的不同？

loader 用于对模块的源代码进行转换。loader 可以使你在 import 或"加载"模块时预处理文件。因此，loader 类似于其他构建工具中“任务(task)”，并提供了处理前端构建步骤的强大方法。loader 可以将文件从不同的语言（如 TypeScript）转换为 JavaScript，或将内联图像转换为 data URL。loader 甚至允许你直接在 JavaScript 模块中 import CSS文件！

插件是 webpack 的支柱功能。webpack 自身也是构建于，你在 webpack 配置中用到的相同的插件系统之上！

Plugins 目的在于解决 loader 无法实现的其他事。从打包优化和压缩，到重新定义环境变量，功能强大到可以用来处理各种各样的任务。webpack提供了很多开箱即用的插件：CommonChunkPlugin主要用于提取第三方库和公共模块，避免首屏加载的bundle文件，或者按需加载的bundle文件体积过大，导致加载时间过长，是一把优化的利器。而在多页面应用中，更是能够为每个页面间的应用程序共享代码创建bundle。


## webpack的构建流程是什么?从读取配置到输出文件这个过程尽量说全
Webpack 的运行流程是一个串行的过程，从启动到结束会依次执行以下流程：


* 初始化参数：从配置文件和 Shell 语句中读取与合并参数，得出最终的参数；

* 开始编译：用上一步得到的参数初始化 Compiler 对象，加载所有配置的插件，执行对象的 run 方法开始执行编译；

* 确定入口：根据配置中的 entry 找出所有的入口文件；

* 编译模块：从入口文件出发，调用所有配置的 Loader 对模块进行翻译，再找出该模块依赖的模块，再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理；

* 完成模块编译：在经过第4步使用 Loader 翻译完所有模块后，得到了每个模块被翻译后的最终内容以及它们之间的依赖关系；

* 输出资源：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 Chunk，再把每个 Chunk 转换成一个单独的文件加入到输出列表，这步是可以修改输出内容的最后机会；

* 输出完成：在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统。

* 在以上过程中，Webpack 会在特定的时间点广播出特定的事件，插件在监听到感兴趣的事件后会执行特定的逻辑，并且插件可以调用 Webpack 提供的 API 改变 Webpack 的运行结果。


是否写过Loader和Plugin？描述一下编写loader或plugin的思路？
webpack的热更新是如何做到的？说明其原理？
如何利用webpack来优化前端性能？（提高性能和体验）
如何提高webpack的构建速度？
怎么配置单页应用？怎么配置多页应用？
npm打包时需要注意哪些？如何利用webpack来更好的构建？
如何在vue项目中实现按需加载？