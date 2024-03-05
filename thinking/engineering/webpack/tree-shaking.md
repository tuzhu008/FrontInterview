# tree shaking

Tree Shaking 指基于 ES Module 进行静态分析，通过 AST 将用不到的函数进行移除，从而减小打包体积。

## 基本原理

* ES6 Module引入进行静态分析，故而编译的时候正确判断到底加载了那些模块
* 通过 AST 将用不到的函数进行移除

## tree shaking 原理

tree shaking的原理大致包括以下几个步骤：

* buildModule阶段：当源码解析成ast树后，分析export语法，转变为对应的dependency。
* `hooks.finishModules`钩子：通过FlagDependencyExportsPlugin插件遍历所有module的dependencies，找到export相关的dependencies，然后分析出导出的变量名称。最后根据每个导出的变量名称创建一个exportInfo，最后将exportInfo与当前的module建立联系：通过getExportsInfo可以访问当前module所有的exportInfo。
* `hooks.optimizeDependencies`钩子：通过FlagDependencyUsagePlugin插件，也是遍历dependencies。
  * 查找使用了模块变量的dependency，解析出具体使用了哪些变量。
  * 根据该dependency和moduleGraph，获取该dependency对应的module。
  * 获取module的exportsInfo，根据name获取对应的exportInfo。
  * 将exportInfo，标记为已使用。（可通过_usedInRuntime属性访问是否使用过）。
* seal阶段：在生成代码时，分析export相关的dependency，如果没有被使用，那么不生成相应的导出代码。
* assets阶段：生成的代码通过terser-webpack-plugin插件删除无用代码。


## 面试

* 基于 ES6 模块系统，使用经过编译生成的模块依赖树，从其中查找 export 相关的依赖，然后对每一个 export 变量进行记录，分析和标记。
* 然后通过 AST 将标记为未使用的变量进行删除。
