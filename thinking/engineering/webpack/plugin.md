# plugin


## Plugin List

| 名字 | 描述 |
|:--|:--|

| clean-webpack-plugin | 清除指定路径的文件或者文件夹 |
| HtmlWebpackPlugin |	简单创建 HTML 文件，用于服务器访问 |
| CopyWebpackPlugin |	将单个文件或整个目录复制到构建目录 |
| MinChunkSizePlugin |	确保 chunk 大小超过指定限制 |
| MiniCssExtractPlugin |	为每个引入 CSS 的 JS 文件创建一个 CSS 文件 |
| LimitChunkCountPlugin |	设置 chunk 的最小/最大限制，以微调和控制 chunk |
| DefinePlugin |	允许在编译时(compile time)配置的全局常量 |
| uglifyjs-webpack-plugin | 用于压缩JS文件 |
| BannerPlugin |	使用 babel-minify进行压缩 |
| CommonsChunkPlugin |	提取 chunks 之间共享的通用模块 |
| CompressionWebpackPlugin |	预先准备的资源压缩版本，使用 Content-Encoding 提供访问服务 |
| ContextReplacementPlugin |	重写 require 表达式的推断上下文 |
| DllPlugin |	为了极大减少构建时间，进行分离打包 |
| EnvironmentPlugin |	DefinePlugin 中 process.env 键的简写方式。 |
| HotModuleReplacementPlugin |	启用模块热替换(Enable Hot Module Replacement - HMR) |
| I18nWebpackPlugin |	为 bundle 增加国际化支持 |
| IgnorePlugin |	从 bundle 中排除某些模块 |
| NoEmitOnErrorsPlugin |	在输出阶段时，遇到编译错误跳过 |
| NormalModuleReplacementPlugin |	替换与正则表达式匹配的资源 |
| NpmInstallWebpackPlugin |	在开发环境下自动安装缺少的依赖 |
| ProgressPlugin |	报告编译进度 |
| ProvidePlugin |	不必通过 import/require 使用模块 |
| SourceMapDevToolPlugin |	对 source map 进行更细粒度的控制 |
| EvalSourceMapDevToolPlugin |	对 eval source map 进行更细粒度的控制 |
| UglifyjsWebpackPlugin |	可以控制项目中 UglifyJS 的版本 |
| TerserPlugin |	允许控制项目中 Terser 的版本 |
| ZopfliWebpackPlugin |	通过 node-zopfli 将资源预先压缩的版本 |