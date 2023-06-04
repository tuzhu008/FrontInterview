# Babel

babel是一个工具链，主要用于将ES2015+代码转换为当前和旧浏览器或环境中向后兼容的Js版本。这句话比较官方，其实babel就是一个语法转换工具链，它会将我们书写的代码（vue或react）通过相关的解析（对应的Preset），主要是词法解析和语法解析，通过babel-parser转换成对应的AST树，再对得到的抽象语法树根据相关的规则配置，转换成最终需要的目标平台识别的AST树，再得到目标代码。
在日程的Webpack使用主要有三个插件：babel-loader、babel-core、babel-preset-env。
babel本质上会运行babel-loader一个函数，在运行时会匹配到对应的文件，根据babel.config.js（.balelrc）的配置（这里会配置相关的babel-preset-env,它会告诉babel用什么规则去进行代码转换）去将代码进行一个解析和转换（转换依靠的是babel-core），最终得到目标平台的代码。