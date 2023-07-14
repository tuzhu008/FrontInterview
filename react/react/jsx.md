JSX 是 Javascript XML 的简称。

JSX 是一个 JavaScript 的语法扩展，其具有 JavaScript 的全部功能，用于创建 React 元素。

JSX 以声明式的方式描述了视图。

在编译之后，JSX 表达式会被转为普通 JavaScript 函数调用，并且对其取值后得到 JavaScript 对象。

## 优点

* JSX 是一种语法糖，有助于保持代码更具可读性和简洁性。

* 在 JSX 中更高效地报告错误和警告。

## 编译

Babel 会把 JSX 转译成一个名为 `React.createElement()` 函数调用。


## ​ React.createElement

* 提取保留的 props

* 提取剩余的 props

* 处理 children，并将 `children` 赋值给 `props.children`

* 使用处理好的参数调用 `ReactElement` 创建一个 react 元素。

```js
/**
 * 创建并返回给定类型的新ReactElement。
 * 参见 https://reactjs.org/docs/react-api.html#createelement
 */
export function createElement(type, config, children) {
  let propName;

  // Reserved names are extracted
  // 提取保留名称
  const props = {};

  let key = null;
  let ref = null;
  let self = null;
  let source = null;

  if (config != null) {
    if (hasValidRef(config)) {
      ref = config.ref;
    }
    if (hasValidKey(config)) {
      key = '' + config.key;
    }

    self = config.__self === undefined ? null : config.__self;
    source = config.__source === undefined ? null : config.__source;
    // Remaining properties are added to a new props object
    // 剩余属性被添加到 props 对象中
    for (propName in config) {
      if (
        hasOwnProperty.call(config, propName) &&
        !RESERVED_PROPS.hasOwnProperty(propName)
      ) {
        props[propName] = config[propName];
      }
    }
  }

  // Children can be more than one argument, and those are transferred onto
  // the newly allocated props object.
  // 子对象可以有多个参数，这些参数会被转移到新分配的 props 对象上。
  const childrenLength = arguments.length - 2;
  if (childrenLength === 1) {
    props.children = children;
  } else if (childrenLength > 1) {
    const childArray = Array(childrenLength);
    for (let i = 0; i < childrenLength; i++) {
      childArray[i] = arguments[i + 2];
    }
    props.children = childArray;
  }

  // Resolve default props
  // 解析默认 props
  if (type && type.defaultProps) {
    const defaultProps = type.defaultProps;
    for (propName in defaultProps) {
      if (props[propName] === undefined) {
        props[propName] = defaultProps[propName];
      }
    }
  }

  return ReactElement(
    type,
    key,
    ref,
    self,
    source,
    ReactCurrentOwner.current,
    props,
  );
}
```

```js
/**
 * Factory method to create a new React element. This no longer adheres to
 * the class pattern, so do not use new to call it. Also, instanceof check
 * will not work. Instead test $$typeof field against Symbol.for('react.element') to check
 * if something is a React Element.
 * 创建 React 元素的工厂方法。这不再遵循类模式，所以不要使用new来调用它。
 * 同样，instanceof检查将不起作用。
 * 相反，在 Symbol.for('react.element') 上测试$$typeof字段来检查某些东西是否为React元素
 *
 * @param {*} type
 * @param {*} props
 * @param {*} key
 * @param {string|object} ref
 * @param {*} owner
 * @param {*} self A *temporary* helper to detect places where `this` is
 * different from the `owner` when React.createElement is called, so that we
 * can warn. We want to get rid of owner and replace string `ref`s with arrow
 * functions, and as long as `this` and owner are the same, there will be no
 * change in behavior.
 * @param {*} source An annotation object (added by a transpiler or otherwise)
 * indicating filename, line number, and/or other information.
 * @internal
 */
function ReactElement(type, key, ref, self, source, owner, props) {
  const element = {
    // This tag allows us to uniquely identify this as a React Element
    // 这个标签允许我们唯一地将其标识为React元素
    $$typeof: REACT_ELEMENT_TYPE,

    // Built-in properties that belong on the element
    // 属于元素的内置属性
    type: type,
    key: key,
    ref: ref,
    props: props,

    // Record the component responsible for creating this element.
    // 记录负责创建该元素的组件。
    _owner: owner,
  };

  return element;
}
```

如果组件是 ClassComponent 则 type 是 class 本身，如果组件是 FunctionComponent 创建的，则 type 是这个function，源码中用  `ClassComponent.prototype.isReactComponent` 来区别二者。注意 class 或者 function 创建的组件一定要首字母大写，不然后被当成普通节点，type 就是字符串。

jsx 对象上没有优先级、状态、effectTag 等标记，这些标记在 Fiber 对象上，在 mount 时 Fiber 根据 jsx 对象来构建，在 update 时根据最新状态的 jsx 和 current Fiber 对比，形成新的 workInProgress Fiber，最后 workInProgress Fibe 切换成 current Fiber。

## 总结

jsx 是 `React.createElement` 的语法糖，jsx 通过 babel 转化成 `React.createElement` 函数，`React.createElement` 执行之后返回 jsx 对象，也叫virtual-dom，也可成为 React 元素。

在初次渲染时，React 利用 `createFiberFromTypeAndProps` 将 React 元素转换为 fiber 和current Fiber进行对比形成workInProgress Fiber

## 问题

### 什么是 JSX？

JSX 是一种 JavaScript 的语法扩展，运用于 React 架构中，其格式比较像是模版语言，但事实上完全是在 JavaScript 内部实现的。元素是构成 React 应用的最小单位，JSX 就是用来声明 React 当中的元素，React 使用 JSX 来描述用户界面。

### 为什么浏览器无法读取JSX？

JSX 并不是有效的 JavaScript 代码，浏览器没有内置读取和理解 JSX 的功能。

需要将 JSX 编译成浏览器可以理解的 JavaScript 代码。



