> Fiber is the new reconciliation engine in React 16. Its main goal is to enable incremental rendering of the virtual DOM

Fiber 是 React 16 中的新协调引擎。它的主要目标是支持虚拟 DOM 的增量呈现。

在光纤树的情况下，React 不执行递归遍历。相反，它会创建一个单向链表，并执行父级优先、深度优先遍历。

## 概念

* 协调（reconciliation）

  React 是一个用于构建用户界面的 JavaScript 库。其核心是跟踪组件状态更改并将更新状态投影到屏幕的机制。在 React 中，我们将这个过程称为和解。

  协调是 React 用来将一棵树与另一棵树进行比较以确定哪些部分需要更改的算法。

  协调是通常理解为“虚拟 DOM”背后的算法。高级描述是这样的：当你渲染一个 React 应用程序时，会生成一个描述应用程序的节点树并保存在内存中。然后将此树刷新到渲染环境 — 例如，对于浏览器应用程序，它被转换为一组 DOM 操作。当应用程序更新时（通常通过 setState ），会生成一个新树。新树与上一个树不同，以计算更新呈现的应用所需的操作。

  update，更新用于呈现 React 应用程序的数据的更改。

* 调度（scheduling）

  调度是确定何时应执行工作的过程。

* 工作（work）

  工作是必须执行的任何计算。如：更新 state、检索和比较 children 和 `props`、调用生命周期方法、更新 `ref` 等。所有这些活动在光纤架构中统称为“工作”。

  工作通常是更新的结果（例如 setState）。

  工作类型通常取决于 React 元素的类型。例如，对于类组件，React 需要创建一个实例，而它不会为函数式组件执行此操作。


## Fiber

### 目标

> * Ability to split interruptible work in chunks.
> * Ability to prioritize, rebase and reuse work in progress.
> * Ability to yield back and forth between parents and children to support layout in React.
> * Ability to return multiple elements from render().
> * Better support for error boundaries.

* 能够将可中断的工作分成块。

* 能够确定正在进行的工作的优先级、变基和重用。

* 能够在父母和孩子之间来回让步以支持 React 中的布局。

* 能够从 render() 返回多个元素。

* 更好地支持错误边界。

简单来讲，Fiber 的主要目标是使 React 能够利用调度，以更好的利用浏览器的更新频率，使 UI 动画、手势更流程。

- 增量更新
- 暂停、中止、重用、分配优先级

### 数据结构

* 架构角度

  从架构上来讲，Fiber 是对 React 旧的协调器的重写，用来管理 React 内部实例树的新实现。
  其引入了新的协调器算法，此协调器称为 Fiber 协调器，基于此的 react 架构成为 Fiber 架构。

  Fiber 遍历算法从过去的依赖内置堆栈的同步递归模型，变成了具有链表和指针的异步模型。
  因为不依赖堆栈，遍历可以在任意地方中断，然后再恢复，只需要记录中断的位置即可。

  Fiber 是堆栈的重新实现，专门用于 React 组件。您可以将单个光纤视为虚拟堆栈帧。
  重新实现堆栈的优势在于，可以将堆栈帧保存在内存中，并随时随地执行它们。这对于实现我们的日程安排目标至关重要。
  除了调度之外，手动处理堆栈帧还可以释放并发性和错误边界等功能的潜力

  PS：一个堆栈帧存储了一个堆内存地址，也就保存了一组数据。就如内置的堆栈帧，在 JS 同步递归时，保存了所有函数的执行环境。

* 职责角度

  从功能来讲，fiber 是 react 调度的一个工作单元。其存储了执行 React 工作的所有信息。

* 数据结构

  从数据结构来讲，fiber 是一个普通的 javascript 对象，它用以表示一个React 元素。光纤节点有效地保存组件的状态、props 和它呈现到的底层 DOM 元素。

  可以将其视作工作单元，表示要执行的一些工作。因为所有的工作都是通过对比 fiber 中的数据而进行的的。fiber 是更新的最小单元。

  而所有的 fiber 组合起来，又形成了一棵 fiber 树。该树保存了用于渲染 UI 的应用程序的状态。

  每个 fiber 节点都有 `child`、`sibling`、`return ` 三个指针，由此整个 fiber 树又是一个单向的链表。

  树有两种：

    * `current` 当前述与当前的 UI 相对应

    * `workInProgress` 工作树与当前进行的工作对应，所有的工作都是在这个树上进行的。

  Fiber 架构中，React 创建了一个可以变异的光纤节点树。而之前的版本中，此树不可变异。

  React 元素有很多类型：clsss、function、host component、portal、fragement???


### 第一次渲染

React Fiber 遍历每个组件的渲染函数的输出，并在树中为每个 React 元素创建一个 Fiber 节点。它使用 ·、`createFiberFromTypeAndProps` 将 React 元素转换为 fiber。React 元素可以是类组件或主机组件，如 div 或 span。对于类组件，它创建一个实例，对于主机组件，它从 React 元素获取数据/属性。

以此，将会构建起一棵由 fiber 组成的树。

这棵 fiber 树一个链表结构，每个 fiber 具有：child、sbling、return 指针。

### 更新

对于每次更新，它都会构建一个工作进行中树。它从根 fiber 开始，遍历树，直到叶节点。与初始渲染阶段不同，它不会为每个 React 元素创建一个新的 fiber。它只是使用该 React 元素的预先存在的 fiber，并在更新阶段合并来自更新元素的新数据/道具。

React Fiber 将更新划分为工作单元。它可以为每个工作单元分配优先级，并且能够在不需要时暂停、重用或中止工作单元。React **Fiber 将工作划分为多个工作单元**，这就是 fiber。它在多个帧中安排工作，并使用 `requestIdleCallback` 中的截止时间。每个更新都有其优先级定义，如动画，或者用户输入的优先级高于从获取的数据中呈现项目列表。Fiber 使用 `requestAnimationFrame` 进行优先级较高的更新，使用 `requestIdleCallback` 进行优先级较低的更新。因此，在安排工作时，Fiber 会检查当前更新的优先级和截止日期（帧结束后的空闲时间）。

如果优先级高于待处理的工作，或者没有截止日期或尚未达到截止日期，则 Fiber 可以在单个帧之后安排多个工作单元。下一组工作单元将延续到其他帧上。这就是 Fiber 可以暂停、重用和中止工作单元的原因。


这样实现了对渲染更新更细粒度的控制，能够释放更多的潜力。比如返回多元素、更好的错误处理。

- requestAnimationFrame

- requestIdleCallback

## 阶段

完成工作有两个阶段：渲染和提交。

### 渲染阶段

渲染阶段是异步的，此阶段执行的工作不会导致任何用户可见的更改。

实际的树遍历和截止时间的使用发生在此阶段。这是 Fiber 的内部逻辑，因此在此阶段对 Fiber 树所做的更改对用户不可见。因此，Fiber 可以暂停、中止或划分多个帧的工作。

我们可以把这个阶段称为协调阶段。Fiber 从 fiber 树的根部穿过并处理每个 fiber。为每个工作单元调用 [workLoop][workLoop] 函数以执行工作。我们可以将这项工作的处理分为两个步骤：开始和完成。

- 开始步骤

  如果你从 React 代码库中找到 `workLoop` 函数，它会调用 `performUnitOfWork`，它将 `nextUnitOfWork` 作为参数。它只不过是将要执行的工作单元。`performUnitOfWork` 函数在内部调用 `beginWork` 函数。这是在 fiber 上进行实际工作的地方，而执行工作单元正是迭代发生的地方。

  在 [`beginWork`][beginWork] 函数中，如果 fiber 没有任何待处理的工作，程序将会跳过此 fiber，因此不会进入开始阶段。这就是在遍历树时，Fiber 跳过已经处理过的 fiber 并直接跳到正在等待工作的 fiber 上的方式。如果您看到大的 `beginWork` 函数代码块，我们将找到一个调用相应 fiber 更新函数的 `switch` 块，具体取决于 fiber `tag`。就像主机组件的 updateHostComponent 一样。这些函数更新 fiber。

  `beginWork` 函数返回子 fiber（如果有）或 null（如果没有子 fiber）。`performUnitOfWork` 函数保持迭代并调用子 fiber，直到到达叶节点。对于叶节点，`beginWork` 返回 `null`，因为没有任何子节点，并且 `performUnitOfWork` 函数调用 `completeUnitOfWork` 函数。

- 完成步骤

  在此步骤中，`completeUnitOfWork` 函数通过调用 `completeWork` 函数来完成当前工作单元。`completeUnitOfWork` 返回同级 fiber（如果有要执行下一个工作单元），如果没有工作，则完成 `return`（父）fiber。这一直持续到返回值为 `null`，即直到它到达根节点。与 `beginWork` 一样，`completeWork` 也是一个实际工作发生的函数，而 `completeUnitOfWork` 用于迭代。

渲染阶段的结果会创建一个 `effect` 列表（副作用）。这些效果类似于插入、更新或删除主机组件的节点，或者调用类组件节点的生命周期方法。fiber 标有相应的 effect tag。

在渲染阶段之后，Fiber 将准备好提交更新。

执行的生命周期方法：

* getDerivedStateFromProps

* shouldComponentUpdate

* render

### 提交阶段

提交阶段是同步的，它会导致用户可见的更改，例如 DOM。

这是完成的工作将用于在 UI 上呈现它的阶段。由于此阶段的结果对用户可见，因此不能将其划分为部分渲染。此阶段是同步阶段。

在此阶段开始时，Fiber 具有已在 UI 上渲染的当前树、已完成的工作(`finishedWork`)或`workInProgress`树，该树是在渲染阶段和效果列表中构建的。

效果列表是 fiber 的链表，它有副作用。因此，它是渲染阶段的`workInProgress`树节点的子集，具有副作用（更新）。效果列表节点使用 `nextEffect` 指针进行链接。

在此阶段调用的函数是 [`completeRoot`](https://github.com/facebook/react/blob/95a313ec0b957f71798a69d8e83408f40e76765b/packages/react-reconciler/src/ReactFiberScheduler.js#L2306)。

在这里，workInProgress 树成为当前树，因为它用于呈现UI。实际的 DOM 更新（如插入、更新、删除和对生命周期方法的调用）或与 ref 相关的更新发生在效果列表中存在的节点上。

提交阶段又被划分为几个子阶段：
1. before mutation phase
  * getSnapshotBeforeUpdate
1. mutation phase 再次阶段更改 host tree
  * componentWillUnmount
1. layout phase
  * componentDidMount
  * componentDidUpdate

这就是光纤协调器的工作方式。

这就是 React fiber 调和器如何将工作划分为多个工作单元的方式。它设置每个工作的优先级，并可以暂停、重用和中止工作单元。在fiber树中，各个节点跟踪使上述事情成为可能所需的节点。每个fiber都是链表的一个节点，链表通过child、sibling和return引用进行连接。

提交阶段 `recursivelyTraverseMutationEffects` 执行深度优先遍历，在其中先执行 child 删除操作、然后再递归，将子孙递归完成再执行本身的插入，替换操作。

因此执行完 `recursivelyTraverseMutationEffects` 深度递归后，会将所有待删除的 fiber 进行删除，然后再从最末枝的节点开始返回执行 `commitReconciliationEffects`。`commitReconciliationEffects` 执行的就是插入、替换操作。

## Fiber 数据结构

下面是 React 代码库中定义的类型定义。删除了一些额外的道具并保留了一些评论以了解属性的含义。你可以在 [React 代码库](https://github.com/facebook/react/blob/master/packages/react-reconciler/src/ReactInternalTypes.js#L49)中找到详细的结构。

```ts
// A Fiber is work on a Component that needs to be done or was done. There can
// be more than one per component.
export type Fiber = {
  // These first fields are conceptually members of an Instance. This used to
  // be split into a separate type and intersected with the other Fiber fields,
  // but until Flow fixes its intersection bugs, we've merged them into a
  // single type.

  // An Instance is shared between all versions of a component. We can easily
  // break this out into a separate object to avoid copying so much to the
  // alternate versions of the tree. We put this on a single object for now to
  // minimize the number of objects created during the initial render.

  // Tag identifying the type of fiber.
  tag: WorkTag,

  // Unique identifier of this child.
  key: null | string,

  // The value of element.type which is used to preserve the identity during
  // reconciliation of this child.
  elementType: any,

  // The resolved function/class/ associated with this fiber.
  type: any,

  // The local state associated with this fiber.
  // 与该光纤相关联的本地状态。
  stateNode: any,

  // Conceptual aliases
  // parent : Instance -> return The parent happens to be the same as the
  // return fiber since we've merged the fiber and instance.

  // Remaining fields belong to Fiber

  // The Fiber to return to after finishing processing this one.
  // This is effectively the parent, but there can be multiple parents (two)
  // so this is only the parent of the thing we're currently processing.
  // It is conceptually the same as the return address of a stack frame.
  return: Fiber | null,

  // Singly Linked List Tree Structure.
  child: Fiber | null,
  sibling: Fiber | null,
  index: number,

  // The ref last used to attach this node.
  // I'll avoid adding an owner field for prod and model that as functions.
  ref:
    | null
    | (((handle: mixed) => void) & {_stringRef: ?string, ...})
    | RefObject,

  refCleanup: null | (() => void),

  // Input is the data coming into process this fiber. Arguments. Props.
  pendingProps: any, // This type will be more specific once we overload the tag.
  memoizedProps: any, // The props used to create the output.

  // A queue of state updates and callbacks.
  updateQueue: mixed,

  // The state used to create the output
  memoizedState: any,

  // Dependencies (contexts, events) for this fiber, if it has any
  dependencies: Dependencies | null,

  // Bitfield that describes properties about the fiber and its subtree. E.g.
  // the ConcurrentMode flag indicates whether the subtree should be async-by-
  // default. When a fiber is created, it inherits the mode of its
  // parent. Additional flags can be set at creation time, but after that the
  // value should remain unchanged throughout the fiber's lifetime, particularly
  // before its child fibers are created.
  mode: TypeOfMode,

  // Effect
  flags: Flags,
  subtreeFlags: Flags,
  deletions: Array<Fiber> | null,

  // Singly linked list fast path to the next fiber with side-effects.
  // 单链表的快速路径到下一个光纤的副作用。
  nextEffect: Fiber | null,

  // The first and last fiber with side-effect within this subtree. This allows
  // us to reuse a slice of the linked list when we reuse the work done within
  // this fiber.
  firstEffect: Fiber | null,
  lastEffect: Fiber | null,

  lanes: Lanes,
  childLanes: Lanes,

  // This is a pooled version of a Fiber. Every fiber that gets updated will
  // eventually have a pair. There are cases when we can clean up pairs to save
  // memory if we need to.
  alternate: Fiber | null,

  // Time spent rendering this Fiber and its descendants for the current update.
  // This tells us how well the tree makes use of sCU for memoization.
  // It is reset to 0 each time we render and only updated when we don't bailout.
  // This field is only set when the enableProfilerTimer flag is enabled.
  actualDuration?: number,

  // If the Fiber is currently active in the "render" phase,
  // This marks the time at which the work began.
  // This field is only set when the enableProfilerTimer flag is enabled.
  actualStartTime?: number,

  // Duration of the most recent render time for this Fiber.
  // This value is not updated when we bailout for memoization purposes.
  // This field is only set when the enableProfilerTimer flag is enabled.
  selfBaseDuration?: number,

  // Sum of base times for all descendants of this Fiber.
  // This value bubbles up during the "complete" phase.
  // This field is only set when the enableProfilerTimer flag is enabled.
  treeBaseDuration?: number,

  // Conceptual aliases
  // workInProgress : Fiber ->  alternate The alternate used for reuse happens
  // to be the same as work in progress.
  // __DEV__ only

  _debugSource?: Source | null,
  _debugOwner?: Fiber | null,
  _debugIsCurrentlyTiming?: boolean,
  _debugNeedsRemount?: boolean,

  // Used to verify that the order of hooks does not change between renders.
  _debugHookTypes?: Array<HookType> | null,
};
```

## 参考文档

首读：[Inside Fiber: in-depth overview of the new reconciliation algorithm in React](https://indepth.dev/posts/1008/inside-fiber-in-depth-overview-of-the-new-reconciliation-algorithm-in-react)

[react-fiber-architecture](https://github.com/acdlite/react-fiber-architecture)

[deep dive react fiber](https://blog.logrocket.com/deep-dive-react-fiber/)

[An Introduction to React Fiber ](https://www.velotio.com/engineering-blog/react-fiber-algorithm)

[Here is a well documented list of resources ](https://github.com/koba04/react-fiber-resources)

[react-fiber-resources](https://github.com/koba04/react-fiber-resources)

[React Fiber单向链表架构](https://github.com/hushicai/hushicai.github.io/issues/41)

[The how and why on React’s usage of linked list in Fiber to walk the component’s tree](https://indepth.dev/posts/1007/the-how-and-why-on-reacts-usage-of-linked-list-in-fiber-to-walk-the-components-tree)

[beginWork]: https://github.com/facebook/react/blob/f765f022534958bcf49120bf23bc1aa665e8f651/packages/react-reconciler/src/ReactFiberBeginWork.js#L1076
[workLoop]: https://github.com/facebook/react/blob/f765f022534958bcf49120bf23bc1aa665e8f651/packages/react-reconciler/src/ReactFiberScheduler.js#L1136
