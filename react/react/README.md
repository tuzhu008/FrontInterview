# React



* [了解 React 中的 ErrorBoundary 吗，它有那些使用场景](../questions/ErrorBoundary.md)

【Q021】React 中，cloneElement 与 createElement 各是什么，有什么区别
【Q038】使用 react 实现一个通用的 message 组件
【Q067】react 如何使用 render prop component 请求数据
【Q068】React Portal 有哪些使用场景
【Q071】react 与 vue 数组中 key 的作用是什么
【Q092】react 中 ref 是干什么用的，有哪些使用场景
【Q100】如何使用 react/vue 实现一个 message API
【Q146】如果使用 SSR，可以在 created/componentWillMount 中访问 localStorage 吗
【Q152】如何实现一个 react hook，你有没有自己写过一个
【Q154】在 react/vue 中数组是否可以以在数组中的次序为 key
【Q164】React 中 fiber 是用来做什么的
【Q211】React hooks 中 useCallback 的使用场景是什么
【Q235】useEffect 中如何使用 async/await
【Q277】redux 解决什么问题，还有什么其他方案
【Q367】redux 和 mobx 有什么不同
【Q369】在 React 项目中 immutable 是优化性能的
【Q371】在 redux 中如何发送请求
【Q375】在 redux 中如何写一个记录状态变更的日志插件
【Q378】React 在 setState 时发生了什么
【Q380】如何设计一个UI组件库
【Q403】React 中的 dom diff 算法如何从 O(n3) 优化到 O(n) 的
【Q404】在 React 应用中如何排查性能问题
【Q408】React 17.0 有什么变化
【Q452】现代框架如 React、Vue 相比原生开发有什么优势
【Q455】React/Vue 中的 router 实现原理如何
【Q466】在 SSR 项目中如何判断当前环境时服务器端还是浏览器端
【Q497】React.setState 是同步还是异步的
【Q498】什么是服务器渲染 (SSR)
【Q499】在 React 中如何实现代码分割 (code splitting)
【Q500】在 React 中如何做好性能优化
【Q501】在 React 中发现状态更新时卡顿，此时应该如何定位及优化
【Q502】当多次重复点击按钮时，以下三个 Heading 是如何渲染的
【Q539】Javascript 数组中有那些方法可以改变自身，那些不可以
【Q552】关于 setState 以下代码的输出
【Q590】React 中什么是合成事件
【Q592】前端项目中有哪些副作用
【Q593】React/Vue 中受控组件与不受控组件的区别
【Q595】React 中监听 input 的 onChange 事件的原生事件是什么
【Q611】React/Vue 中兄弟组件如何进行通信
【Q612】React.memo 中是如何实现性能优化的
【Q614】immer 的原理是什么，为什么它的性能更高
【Q615】React.useMemo 与 React.useCallback 是如何进行性能优化的
【Q624】同一页面三个组件请求同一个 API 发送了三次请求，如何优化
【Q627】如何优化 React 项目的性能
【Q653】useLayoutEffect 和 useEffect 有什么区别
【Q659】在 React Hooks 中实现 usePreviouseValue 取上次渲染的值
【Q699】在虚拟 DOM 中进行 diff 算法时，介绍当根据 key 对数组进行重用时的算法

## 什么是 virtual DOM，它的引入带了什么好处

* 跨平台。于抽象了原本的渲染过程，实现了跨平台的能力，而不仅仅局限于浏览器的 DOM，可以是安卓和 IOS 的原生组件，可以是近期很火热的小程序，也可以是各种 GUI。

* 维护性。Virtual DOM 在牺牲(牺牲很关键)部分性能的前提下，增加了可维护性，这也是很多框架的通性。 
* 效率。
  * 实现了对 DOM 的集中化操作，在数据改变时先对虚拟 DOM 进行修改，再批量反映到真实的 DOM中
  * 用最小的代价来更新DOM，提高效率。


## react 中 ref 是干什么用的，有哪些使用场景

* 引用 React 组件实例，dom 实例
* 一些值需要随时间变化，却不需要响应式的数据存储。

## 什么是 React？

* React 是 Facebook 在 2011 年开发的前端 JavaScript 库。

* 它遵循基于组件的方法，有助于构建可重用的UI组件。

* 它用于开发复杂和交互式的 Web 和移动 UI。

* 尽管它仅在 2015 年开源，但有一个很大的支持社区

## React 有什么特点？

* 它使用虚拟 DOM 而不是真正的DOM，提升页面的渲染性能。

* 它遵循单向数据流或数据绑定。

* 它可以进行服务器端渲染。

* 以写 js 方式构建应用

## React 的一些主要优点

* 现代化的前端框架，磨平了很多浏览器差异，使开发变得简单。

* 采用 MVVM 的架构模式，使前端开发可以专注于业务本身。

* 它提高了应用的性能

* 可以方便地在客户端和服务器端使用

* 由于 JSX，代码的可读性很好

* React 很容易与 Meteor，Angular 等其他框架集成

* 使用 React，编写 UI 测试用例变得非常容易

## React 有哪些限制？

* 兼容性。随着 react 的不断演进，它也抛弃了一些对老旧浏览器的支持。以 React 18为例，它已经放弃了对 ie11 的支持，如需兼容，需要回退到 React 17 版本。

* 与以前提倡的结构、样式、行为相分离的原则有所违背。但无对错。

## React 中 render\(\) 的作用

每个 React 组件强制要求必须有一个 `render()`。它返回一个 React 元素，是原生 DOM 组件的表示。如果需要渲染多个 HTML 元素，则必须将它们组合在一个封闭标记内，例如 `<form>`、`<group>`、`<div>` 等。此函数必须保持纯净，即必须每次调用时都返回相同的结果。

## 什么是 Props?

Props 是 properties 的索性，表示 React 中的属性。

它们是只读组件，必须保持纯，即不可变。它们总是在整个应用中从父组件传递到子组件。子组件永远不能将 prop 送回父组件。这有助于维护单向数据流，通常用于呈现动态生成的数据。

React 特性

单向绑定和双向绑定

## React element

React element 是用来表示 react 节点的一个普通对象。

## 组件是什么？

组件是一个独立的、可复用的代码块，它将用户界面分成更小的部分，而不是在单个文件中构建整个UI。

React 中有两种组件：函数式组件和class组件。

## 什么是class组件（类组件）？

class 组件是返回 JSX 的 ES6 类，必须从 React 继承。因为在 React 的早期版本（16.8）中不能在函数组件内使用状态，所以函数组件只用于渲染 UI。

## 什么是函数组件？

一个返回React元素的 JavaScript/ES6 函数被称为函数组件（JSX）。

自从引入 React Hooks 以来，我们已经能够在函数式组件中使用状态，因为语法更简洁，很多人都采用这个方法。

## 如何在React中使用CSS

使用CSS设置React应用程序样式的方法有3种：

* 内联样式
* 外部样式
* CSS in jS

## 什么是Props？

Props也被称作属性。它们将数据从一个组件传递到另一个组件（从父组件到子组件）。它们通常被用来呈现动态数据。

注意：子组件永远不能将props发送到父组件，因为此流程是单向的（父到子）。

## React中的State是什么？

State是一个React的内置对象，用于在组件中创建和管理数据。它与props的不同之处在于它用于存储数据而不是传递数据。

State是可变的（数据可以更改）并且可以通过 `this.state()` 访问。

## React 中 `render()` 的目的

每个React组件必须强制具有 `render`。它使用 `props` 和 `state` 返回 React 元素。

React 通过调用嵌套的 `render` 来构建了一个 由 React 元素构成的树，称为虚拟 DOM。

找出当前节点的所有父节点，打印他们的类

Vue 和 React 比较？原理，区别，亮点，作用

## `key` 的作用

key 用于唯一标识在相同父元素下的同级元素。
以追踪哪些列表中元素被修改、被添加或者被移除的辅助标识。

在开发过程中，我们需要保证某个元素的 key 在其同级元素中具有唯一性。在 React Diff 算法中 React 会借助元素的 Key 值来判断该元素是新近创建的还是被移动而来的元素，从而减少不必要的元素重渲染。此外，React 还需要借助 Key 值来判断元素与本地状态的关联关系，因此我们绝不可忽视转换函数中 Key 的重要性。

## 调用 `setState` 之后发生了什么？

在代码中调用 `setState` 函数之后，React 会将传入的参数对象与组件当前的状态合并，然后触发所谓的调和过程（Reconciliation）。经过调和过程，React 会以相对高效的方式根据新的状态构建 React 元素树并且着手重新渲染整个 UI 界面。在 React 得到元素树之后，React 会自动计算出新的树与老树的节点差异，然后根据差异对界面进行最小化重渲染。在差异计算算法中，React 能够相对精确地知道哪些位置发生了改变以及应该如何改变，这就保证了按需更新，而不是全部重新渲染。

## 状态\(`state`\)和属性\(`props`\)之间有何区别

`state` 译为状态，用于存储组件内部的一些数据。既为状态，也就有时效性，它可随时间而变化。

`props` 译为属性，是外部传入组件的数据。此 `props` 是外部对组件的配置。由于其数据来源不是组件内部，因此组件不应该直接改变其值。就 React 官方定义来讲，`props` 应是不可变的（immutable）。

## 为什么虚拟 dom 会提高性能?

虚拟 dom 相当于在 js 和真实 dom 中间加了一个缓存，利用 dom diff 算法避免了没有必要的 dom 操作，从而提高性能。

用 JavaScript 对象结构表示 DOM 树的结构；然后用这个树构建一个真正的 DOM 树，插到文档当中。

当状态变更的时候，重新构造一棵新的对象树。然后用新的树和旧的树进行比较，记录两棵树差异把 2 所记录的差异应用到步骤 1 所构建的真正的 DOM 树上，视图就更新了。

## React 中 refs 的作用是什么？

Refs 是 React 提供给我们的安全访问 DOM 元素或者某个组件实例的句柄。

* createRef。

* 回调。回调的唯一参数为组件实例或者 Dom 元素。

* useRef。 一个 React Hook，该方式只能在函数式组件中使用。

* forwardRef。转发 Ref。

## 类组件\(Class component\)和函数式组件\(Functional component\)之间有何不同

* 类组件不仅允许你使用更多额外的功能，如组件自身的状态和生命周期钩子，也能使组件直接访问 store 并维持状态。

* 当组件仅是接收 props，并将组件自身渲染到页面时，该组件就是一个 '无状态组件\(stateless component\)'，可以使用一个纯函数来创建这样的组件。这种组件也被称为哑组件\(dumb components\)或展示组件

* 函数式组件没有实例

但现在可以使用各种 Hook 突破这些限制，函数式组件可以拥有状态，拥有 ref 等等

## 何为受控组件\(controlled component\)

数据和行为收到 React 控制的组件。React 的 state 作为其唯一的数据来源。

> 在 HTML 中，表单元素（如`<input>`、 `<textarea>` 和 `<select>`）之类的表单元素通常自己维护 `state`，并根据用户输入进行更新。而在 React 中，可变状态（mutable state）通常保存在组件的 `state` 属性中，并且只能通过使用 `setState()` 来更新。

> 我们可以把两者结合起来，使 React 的 `state` 成为“唯一数据源”。渲染表单的 React 组件还控制着用户输入过程中表单发生的操作。被 React 以这种方式控制取值的表单输入元素就叫做“受控组件”。

## 描述事件在 React 中的处理方式。

为了解决跨浏览器兼容性问题，React 根据 W3C 规范来定义了合成事件。

React 中的事件处理程序将传递 `SyntheticEvent` 的实例，它是浏览器的原生事件的跨浏览器包装器。

除兼容所有浏览器外，它还拥有和浏览器原生事件相同的接口，包括 `stopPropagation()` 和 `preventDefault()`。

可以使用 `nativeEvent` 属性来获取浏览器的底层事件。

有趣的是，React 实际上并没有将事件附加到子节点本身。React 使用委托机制将使用单个事件监听器监听顶层的所有事件。这对于性能是有好处的，这也意味着在更新 DOM 时，React 不需要担心跟踪事件监听器。

`SyntheticEvent` 是合并而来。这意味着 `SyntheticEvent` 对象可能会被重用，而且在事件回调函数被调用后，所有的属性都会无效。出于性能考虑，你不能通过异步访问事件。

## createElement 和 cloneElement 有什么区别？

* `React.createElement()`

  ```js
  React.cloneElement(
    type,
    [props],
    [...children]
  );
  ```

  JSX 语法就是用 `React.createElement()` 来构建 React 元素的。

  它接受三个参数，第一个参数可以是一个标签名。如 div、span，或者 React 组件。第二个参数为传入的属性。第三个以及之后的参数，皆作为组件的子组件。

* cloneElement

  ```js
  React.cloneElement(
    element,
    [props],
    [...children]
  );
  ```

  以 element 元素为样板克隆并返回新的 React 元素。返回元素的 props 是将新的 props 与原始元素的 props 浅层合并后的结果。新的子元素将取代现有的子元素，而来自原始元素的 key 和 ref 将被保留。

## React 创建组件的方式

* 类组件

* 函数式组件

## 何为高阶组件\(higher order component\)

高阶组件本质上是一个函数，它接收其他组件作为参数，通过组合的方式，返回一个新的组件。

原理上用了闭包。

高阶组件也是包装器模式的应用。

## 用 HOC 做什么

* 代码重用，逻辑和引导抽象

* 渲染劫持

* 状态抽象和控制

* Props 控制

## 什么是纯组件

对于同样的输入，有相同输出的组件。

纯组件可以替换只有 render 功能的组件。纯（Pure） 组件是可以编写的最简单、最快的组件。这些组件增强了代码的简单性和应用的性能。

## MVC 框架的主要问题是什么？

* 对 DOM 操作的代价非常高

* 程序运行缓慢且效率低下

* 内存浪费严重

* 由于循环依赖性，组件模型需要围绕 models 和 views 进行创建

## React 如何管理事件

* React事件使用了事件委托的机制，一般事件委托的作用都是为了减少页面的注册事件数量，减少内存开销，优化浏览器性能，React这么做也是有这么一个目的，除此之外，也是为了能够更好的管理事件，实际上，React中所有的事件最后都是被委托到了 document这个顶级DOM上

* 既然所有的事件都被委托到了 document上，那么肯定有一套管理机制，所有的事件都是以一种先进先出的队列方式进行触发与回调

* 既然都已经接管事件了，那么不对事件做些额外的事情未免有些浪费，于是 React中就存在了自己的 合成事件\(SyntheticEvent\)，合成事件由对应的 EventPlugin负责合成，不同类型的事件由不同的 plugin合成，例如 SimpleEvent Plugin、TapEvent Plugin等

## component 和 PureComponent区别？

Component 是类组件的基类， 在 React.Component 的子类中有个必须定义的 render\(\) 函数。

`React.PureComponent` 继承自 `React.Component` ，并实现了其`shouldComponentUpdate` 方法，该方法对比 `prop` 和 `state` 进行浅层的对比方以决定是否更新。而 `React.Component`并未实现 该方法。

如果赋予 React 组件相同的 `props` 和 `state`，`render()` 函数会渲染相同的内容，那么在某些情况下使用 `React.PureComponent` 可提高性能。

> 注意
>
> `React.PureComponent` 中的 `shouldComponentUpdate()` 仅作对象的浅层比较。如果对象中包含复杂的数据结构，则有可能因为无法检查深层的差别，产生错误的比对结果。仅在你的 `props` 和 `state` 较为简单时，才使用 `React.PureComponent`，或者在深层数据结构发生变化时调用 `forceUpdate()` 来确保组件被正确地更新。你也可以考虑使用 immutable 对象加速嵌套数据的比较。
>
> 此外，`React.PureComponent` 中的 `shouldComponentUpdate()` 将跳过所有子组件树的 `prop` 更新。因此，请确保所有子组件也都是“纯”的组件。

## setState 是异步还是同步？

有时表现出异步，有时表现出同步：

* `setState` 只在合成事件和钩子函数中是“异步”的，在原生事件和 `setTimeout` 中都是同步的。
* `setState` 的“异步”并不是说内部由异步代码实现，其实本身执行的过程和代码都是同步的，只是合成事件和钩子函数的调用顺序在更新之前，导致在合成事件和钩子函数中没法立马拿到更新后的值，形成了所谓的“异步”，当然可以通过第二个参数 `setState(partialState, callback)` 中的 `callback` 拿到更新后的结果。
* `setState` 的批量更新优化也是建立在“异步”（合成事件、钩子函数）之上的，在原生事件和 `setTimeout` 中不会批量更新，在“异步”中如果对同一个值进行多次`setState`，`setState` 的批量更新策略会对其进行覆盖，取最后一次的执行，如果是同时 `setState` 多个不同的值，在更新时会对其进行合并批量更新。

## React 中在哪捕获错误？

react 中可以使用 `getDerivedStateFromError` 生命周期方法捕捉到子组件的错误。一次可以将其需要错误的边界的组件作为父组件将其包装起来。

## 参考

[https://segmentfault.com/a/1190000018604138](https://segmentfault.com/a/1190000018604138)

