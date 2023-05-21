## mvvm 框架是什么？

MVVM 是 Model-View-ViewModel 的缩写。

* Model

  Model 代表数据模型，也可以在 Model 中定义数据修改和操作的业务逻辑。

* View

  View 代表 UI 组件，它负责将数据模型转化成 UI 展现出来。

* ViewModel

  MVVM 的核心是数据驱动即 ViewModel，ViewModel 是 View 和 Model 的关系映射。

  ViewModel 类似中转站(Value Converter)，负责转换 Model 中的数据对象，使得数据变得更加易于管理和使用。

  MVVM 本质就是基于操作数据来操作视图进而操作 DOM，借助于 MVVM 无需直接操作 DOM，开发者只需完成包含声明绑定的视图模板，编写 ViewModel 中有业务，使得 View 完全实现自动化。

  ViewModel 监听模型数据的改变和控制视图行为、处理用户交互，简单理解就是一个同步 View 和 Model 的对象，连接 Model 和 View。

  在 MVVM 架构下，View 和 Model 之间并没有直接的联系，而是通过 ViewModel 进行交互，Model 和 ViewModel 之间的交互是双向的， 因此 View 数据的变化会同步到 Model 中，而 Model 数据的变化也会立即反应到 View 上。

  ViewModel 通过双向数据绑定把 View 层和 Model 层连接了起来，而View 和 Model 之间的同步工作完全是自动的，无需人为干涉，因此开发者只需关注业务逻辑，不需要手动操作 DOM, 不需要关注数据状态的同步问题，复杂的数据状态维护完全由 MVVM 来统一管理。