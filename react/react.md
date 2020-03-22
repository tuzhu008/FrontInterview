# React

## 什么是React？

* React 是 Facebook 在 2011 年开发的前端 JavaScript 库。

* 它遵循基于组件的方法，有助于构建可重用的UI组件。

* 它用于开发复杂和交互式的 Web 和移动 UI。

* 尽管它仅在 2015 年开源，但有一个很大的支持社区

React 特性

单向绑定和双向绑定

## 组件生命周期

**挂载阶段：**

当组件实例被创建并插入 DOM 中时，其生命周期调用顺序如下：

* `constructor()`：获取实例的默认属性

* `static getDerivedStateFromProps()`

  会在调用 `render` 方法之前调用，并且在初始挂载及后续更新时都会被调用。它应返回一个对象来更新 `state`，如果返回 `null` 则不更新任何内容。

  此方法适用于罕见的用例，即 `state` 的值在任何时候都取决于 `props`。

* `componentWillMount`：组件即将被装载、渲染到页面上

  现在名字也叫 `UNSAFE_componentWillMount()`，该方法虽然在 17 上仍然可以使用，但已过时，在未来会被删除。

  有一个常见的误解是，在 `componentWillMount` 中获取数据可以避免第一次渲染为空的状态。实际上，这是不对的，因为 React 总是在 `componentWillMount` 之后立即执行 render。如果在 `componentWillMount` 触发时数据不可用，那么第一次 `render` 仍然会显示加载的状态，而不管你在哪里初始化获取数据。这就是为什么在绝大多数情况下，将获取数据移到 `componentDidMount` 没有明显效果的原因。

* `render`：组件在这里生成虚拟的 DOM 节点

* `componentDidMount`：组件真正在被装载之后

  会在组件挂载后（插入 DOM 树中）立即调用。

  依赖于 DOM 节点的初始化应该放在这里。如需通过网络请求获取数据，此处是实例化请求的好地方。

  这个方法是比较适合添加订阅的地方。如果添加了订阅，请不要忘记在 `componentWillUnmount()` 里取消订阅

**更新：**

当组件的 props 或 state 发生变化时会触发更新。组件更新的生命周期调用顺序如下：

* `static getDerivedStateFromProps()`

* `shouldComponentUpdate`

  组件接受到新属性或者新状态的时候（可以返回 false，接收数据后不更新，阻止 render 调用，后面的函数不会被继续执行了）

* `componentWillUpdate`

  组件即将更新不能修改属性和状态

  现名为 `UNSAFE_componentWillUpdate`，已过时，不该使用

* `render`：组件重新描绘

* `getSnapshotBeforeUpdate`

  `getSnapshotBeforeUpdate()` 在最近一次渲染输出（提交到 DOM 节点）之前调用。它使得组件能在发生更改之前从 DOM 中捕获一些信息（例如，滚动位置）。此生命周期的任何返回值将作为参数传递给 `componentDidUpdate()`。

* `componentDidUpdate`

  `componentDidUpdate()` 会在更新后会被立即调用。首次渲染不会执行此方法。

**卸载阶段：**

* `componentWillUnmount`

  会在组件卸载及销毁之前直接调用。

**错误处理：**

* `static getDerivedStateFromError`

  此生命周期会在后代组件抛出错误后被调用。 它将抛出的错误作为参数，并返回一个值以更新 `state`

* `componentDidCatch`

  此生命周期在后代组件抛出错误后被调用。

  `componentDidCatch()` 会在“提交”阶段被调用，因此允许执行副作用。

[组件生命周期图谱](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

虚拟 DOM，虚拟 DOM 的 diff 算法

找出当前节点的所有父节点，打印他们的类

单向数据流

Vue 和 React 比较？原理，区别，亮点，作用

## `key` 的作用

Keys 是 React 用于追踪哪些列表中元素被修改、被添加或者被移除的辅助标识。

在开发过程中，我们需要保证某个元素的 key 在其同级元素中具有唯一性。在 React Diff 算法中 React 会借助元素的 Key 值来判断该元素是新近创建的还是被移动而来的元素，从而减少不必要的元素重渲染。此外，React 还需要借助 Key 值来判断元素与本地状态的关联关系，因此我们绝不可忽视转换函数中 Key 的重要性。

## 调用 `setState` 之后发生了什么？

在代码中调用 `setState` 函数之后，React 会将传入的参数对象与组件当前的状态合并，然后触发所谓的调和过程（Reconciliation）。经过调和过程，React 会以相对高效的方式根据新的状态构建 React 元素树并且着手重新渲染整个 UI 界面。在 React 得到元素树之后，React 会自动计算出新的树与老树的节点差异，然后根据差异对界面进行最小化重渲染。在差异计算算法中，React 能够相对精确地知道哪些位置发生了改变以及应该如何改变，这就保证了按需更新，而不是全部重新渲染。

## 为什么虚拟 dom 会提高性能?

虚拟 dom 相当于在 js 和真实 dom 中间加了一个缓存，利用 dom diff 算法避免了没有必要的 dom 操作，从而提高性能。

用 JavaScript 对象结构表示 DOM 树的结构；然后用这个树构建一个真正的 DOM 树，插到文档当中。

当状态变更的时候，重新构造一棵新的对象树。然后用新的树和旧的树进行比较，记录两棵树差异把 2 所记录的差异应用到步骤 1 所构建的真正的 DOM 树上，视图就更新了。

## react diff 原理

* 把树形结构按照层级分解，只比较同级元素。

* 给列表结构的每个单元添加唯一的 key 属性，方便比较。

* React 只会匹配相同 class 的 component（这里面的 class 指的是组件的名字）

* 合并操作，调用 component 的 setState 方法的时候, React 将其标记为 dirty.到每一个事件循环结束, React 检查所有标记 dirty 的 component 重新绘制.

* 选择性子树渲染。开发人员可以重写 shouldComponentUpdate 提高 diff 的性能。

## React 中 refs 的作用是什么？

Refs 是 React 提供给我们的安全访问 DOM 元素或者某个组件实例的句柄。

* createRef

  ```js
  class MyComponent extends React.Component { 
    constructor(props) {
      super(props);
      this.myRef = React.createRef();
    }
    render() {
      return <div ref={ this.myRef } />;
    }
  }
  ```

* 回调

  ```js
  class CustomTextInput extends React.Component {
    constructor(props) {
      super(props);

      this.textInput = null;
    }

    render () {
      return (<input type="text" ref={ el => this.textInput = el } />);
    }
  }
  ```

  ```js
  function CustomTextInput(props) {
    return (
      <div>
        <input ref={props.inputRef} />
      </div>
    );
  }

  class Parent extends React.Component {
    render() {
      return (
        <CustomTextInput
          inputRef={el => this.inputElement = el}
        />
      );
    }
  }
  ```

* useRef

  该方式只能在函数式组件中使用

  ```js
  function MyInput () {
    const inputRef = React.useRef(null);
    return (<input type="text" ref={ inputRef } />);
  }
  ```

* forwardRef

  ```js
  const CustomTextInput = React.forwardRef(props, ref) {
    return (
      <div>
        <input ref={ ref } />
      </div>
    );
  }

  class Parent extends React.Component {
    constructor (props) {
      super(props);

      this.ref = React.createRef();
    }
    render() {
      return (
        <CustomTextInput ref={ this.ref } />
      );
    }
  }
  ```

## 类组件(Class component)和函数式组件(Functional component)之间有何不同

* 类组件不仅允许你使用更多额外的功能，如组件自身的状态和生命周期钩子，也能使组件直接访问 store 并维持状态。

* 当组件仅是接收 props，并将组件自身渲染到页面时，该组件就是一个 '无状态组件(stateless component)'，可以使用一个纯函数来创建这样的组件。这种组件也被称为哑组件(dumb components)或展示组件

* 函数式组件没有实例

但现在可以使用各种 Hook 突破这些限制，函数式组件可以拥有状态，拥有 ref 等等

## 何为受控组件(controlled component)

在 HTML 中，表单元素（如`<input>`、 `<textarea>` 和 `<select>`）之类的表单元素通常自己维护 `state`，并根据用户输入进行更新。而在 React 中，可变状态（mutable state）通常保存在组件的 `state` 属性中，并且只能通过使用 `setState()` 来更新。

我们可以把两者结合起来，使 React 的 `state` 成为“唯一数据源”。渲染表单的 React 组件还控制着用户输入过程中表单发生的操作。被 React 以这种方式控制取值的表单输入元素就叫做“受控组件”。

## 何为高阶组件(higher order component)

高阶组件本质上是一个函数，它接收其他组件作为参数，通过组合的方式，返回一个新的组件。

原理上用了闭包。

## 描述事件在 React 中的处理方式。

为了解决跨浏览器兼容性问题，React 根据 W3C 规范来定义了合成事件。

React 中的事件处理程序将传递 `SyntheticEvent` 的实例，它是浏览器的原生事件的跨浏览器包装器。

除兼容所有浏览器外，它还拥有和浏览器原生事件相同的接口，包括 `stopPropagation()` 和 `preventDefault()`。

可以使用 `nativeEvent` 属性来获取浏览器的底层事件。

有趣的是，React 实际上并没有将事件附加到子节点本身。React 将使用单个事件监听器监听顶层的所有事件。这对于性能是有好处的，这也意味着在更新 DOM 时，React 不需要担心跟踪事件监听器。

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