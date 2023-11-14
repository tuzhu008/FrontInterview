# 数据结构

## ReactDOMRoot

当调用 `ReactDOM.createRoot` 方法时，会创建并返回 `ReactDOMRoot` 的实例，该实例具有以下成员：

| key | type |
|:---:|:----:|
| render | Function |
| unmount | Function |
| _internalRoot | FiberRootNode(FiberRoot in TS) |

## FiberRoot

当调用 ReactDOM.render 方法时，会创建一个 FiberRoot。
该 FiberRoot 实际是 `FiberRootNode` 的一个实例，其核心成员如下：

| key | type | desc |
|:---:|:----:|:---:|
| current | FiberNode | 该 Fiber 的 type 为 `HostRoot` |
| containerInfo | DOMContainer | |
| finishedWork | (HostRoot)FiberNode | null | |

其中 `current` 属性是一个类型为 HostRoot 的特殊 FiberNode，它是 Fiber Tree 的根节点，充当最顶层组件（如App）的父级，如下：

```js
const domContainer = document.querySelector('#container');
const root = ReactDOM.createRoot(domContainer);
root.render(<App />);
```

当 FiberRoot 创建成功后，会将其绑定到 container 的 `'__reactContainer$' + randomKey` 中：

```js
const randomKey = Math.random().toString(36).slice(2);
const internalContainerInstanceKey = '__reactContainer$' + randomKey;

function markContainerAsRoot(hostRoot: Fiber, node: Container): void {
  // $FlowFixMe[prop-missing]
  node[internalContainerInstanceKey] = hostRoot;
}

markContainerAsRoot(root.current, container);
```

绑定成功后就可以通过 `contariner[internalContainerInstanceKey]` 来访问 HostRoot Fiber 了。
自此，contariner 所代表的 dom 树与 HostRoot 所代表的 Fiber 树建立上链接。

## FiberRootNode

```js
function FiberRootNode(
  this: $FlowFixMe,
  containerInfo: any,
  // $FlowFixMe[missing-local-annot]
  tag,
  hydrate: any,
  identifierPrefix: any,
  onRecoverableError: any,
) {
  this.tag = tag;
  this.containerInfo = containerInfo;
  this.pendingChildren = null;
  this.current = null;
  this.pingCache = null;
  this.finishedWork = null;
  this.timeoutHandle = noTimeout;
  this.cancelPendingCommit = null;
  this.context = null;
  this.pendingContext = null;
  this.next = null;
  this.callbackNode = null;
  this.callbackPriority = NoLane;
  this.expirationTimes = createLaneMap(NoTimestamp);

  this.pendingLanes = NoLanes;
  this.suspendedLanes = NoLanes;
  this.pingedLanes = NoLanes;
  this.expiredLanes = NoLanes;
  this.mutableReadLanes = NoLanes;
  this.finishedLanes = NoLanes;
  this.errorRecoveryDisabledLanes = NoLanes;

  this.entangledLanes = NoLanes;
  this.entanglements = createLaneMap(NoLanes);

  this.hiddenUpdates = createLaneMap(null);

  this.identifierPrefix = identifierPrefix;
  this.onRecoverableError = onRecoverableError;

  if (enableCache) {
    this.pooledCache = null;
    this.pooledCacheLanes = NoLanes;
  }

  if (supportsHydration) {
    this.mutableSourceEagerHydrationData = null;
  }

  if (enableSuspenseCallback) {
    this.hydrationCallbacks = null;
  }

  this.incompleteTransitions = new Map();
  if (enableTransitionTracing) {
    this.transitionCallbacks = null;
    const transitionLanesMap = (this.transitionLanes = []);
    for (let i = 0; i < TotalLanes; i++) {
      transitionLanesMap.push(null);
    }
  }

  if (enableProfilerTimer && enableProfilerCommitHooks) {
    this.effectDuration = 0;
    this.passiveEffectDuration = 0;
  }

  if (enableUpdaterTracking) {
    this.memoizedUpdaters = new Set();
    const pendingUpdatersLaneMap = (this.pendingUpdatersLaneMap = []);
    for (let i = 0; i < TotalLanes; i++) {
      pendingUpdatersLaneMap.push(new Set());
    }
  }
}
```

## Fiber

```js
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

## Update

React 使用 Update 来表示一次更新或渲染，其数据结构如下：

| key | type | desc |
|:---:|:----:|:---:|
| tag | 0|1|2|3 | UpdateState = 0、ReplaceState = 1、ForceUpdate = 2、CaptureUpdate = 3 |
| payload | any | |
| callback | Function|null | 更新后的回调函数 |
| next | Update|null | 下一个Update对象 |
| nextEffect | Update|null	 | |

## UpdateQueue

`UpdateQueue` 表示在 FiberNode 中的更新队列，其核心数据结构如下：

| key | type | desc |
|:---:|:----:|:---:|
| baseState	| Object	|
| firstUpdate	| `Update\|null`	| 第一个Update
| lastUpdate	| `Update\|null`	| 最后一个Update
| firstCapturedUpdate	| `Update\|null`		|
| lastCapturedUpdate	| `Update\|null`		|
| firstEffect	| `Update\|null`		|
| lastEffect	| `Update\|null`		|

## Effects List

在18以前的 fiber 架构中，React通过FiberNode中的firstEffect、nextEffect、lastEffect字段构建了一个链表。

在18新的架构中，已经将 Effects List 删除，代以在 commit 的递归。

## 数据结构关系图

![react数据关系](/assets/react/react-data-structure.png)

## 数据关系

### fiber 与组件实例的关系

* ClassComponent

  类组件实例的 `_reactInternals` 属性中存放了该实例所对应的 fiber

  类组件 fiber 中 `stateNode` 属性存放了该类组件的实例。

* HostComponent

  HostComponent 组件fiber的 `stateNode` 属性存放了 Dom 节点

* HostRoot

  HostRoot fiber 的 `stateNode` 属性存放了 FiberRoot

### `current` 树与 `workInProgree`

`current` 树与 `workInProgress` 的相关fiber节点通过 `alternate` 属性相关联。

在 update 阶段 `current` 中需要删除的节点会被添加到 `workInProgress` 中相应fiber节点的 `deletions` 属性。