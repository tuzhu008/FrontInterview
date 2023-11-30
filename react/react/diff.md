# React diff

* diff 算法的核心——复用

* 把树形结构按照层级分解，只比较同级元素。

* 使用 `key` 和 `elementType` 作为最重要的对比条件。

  * React 只会匹配相同 class 的 component（这里面的 class 指的是组件的名字）

* 选择性子树渲染。开发人员可以重写 shouldComponentUpdate 提高 diff 的性能。


## 时机

调用 `render` 生成由 ReactElement 组成的 vdom 的树状结构。

将 vdom 转换为 fiber 的过程称为 reconcile。

diff 则发生在 reconcile 阶段。

diff 实际是将 vdom 与 currentFiber 做对比，最后生成 workInProgress Fiber 树。

## 数据关系

### fiber 与组件实例的关系

* ClassComponent

  类组件实例的 `_reactInternals` 属性中存放了该实例所对应的 fiber

  类组件 fiber 中 `stateNode` 属性存放了该类组件的实例。

* HostComponent

  HostComponent 组件fiber的 `stateNode` 属性存放了 Dom 节点


### `current` 树与 `workInProgree`

`current` 树与 `workInProgress` 的相关fiber节点通过 `alternate` 属性相关联。

在 update 阶段 `current` 中需要删除的节点会被添加到 `workInProgress` 中相应fiber节点的 `deletions` 属性。

## reconcileChildren

```js
function reconcileChildren(
  current: Fiber | null,
  workInProgress: Fiber,
  nextChildren: any,
  renderLanes: Lanes,
) {
  if (current === null) {
    // If this is a fresh new component that hasn't been rendered yet, we
    // won't update its child set by applying minimal side-effects. Instead,
    // we will add them all to the child before it gets rendered. That means
    // we can optimize this reconciliation pass by not tracking side-effects.
    // 如果这是一个尚未渲染的新组件，我们不会通过应用最小的副作用来更新它的子组件集。
    // 相反，我们将在渲染之前将它们全部添加到子对象中。这意味着我们可以通过不跟踪副作用来优化这个协调过程。
    workInProgress.child = mountChildFibers(
      workInProgress,
      null,
      nextChildren,
      renderLanes,
    );
  } else {
    // If the current child is the same as the work in progress, it means that
    // we haven't yet started any work on these children. Therefore, we use
    // the clone algorithm to create a copy of all the current children.
    //如果当前子进程与正在进行的工作相同，则意味着我们还没有开始对这些子进程进行任何工作。
    // 因此，我们使用克隆算法来创建当前所有子节点的副本。

    // If we had any progressed work already, that is invalid at this point so
    // let's throw it out.
    //如果我们已经有任何正在进行的工作，那么在这一点上是无效的，所以让我们把它扔掉。
    workInProgress.child = reconcileChildFibers(
      workInProgress,
      current.child,
      nextChildren,
      renderLanes,
    );
  }
}
```

`current` 为 `null` 则表示第一次挂载，然后调用 `mountChildFibers` 执行挂载的逻辑。

`current` 不为 `null` 则表示存在历史挂载，因此调用 `reconcileChildFibers` 对他们的后代进行协调。

## reconcileChildFibers

经过 `reconcileChildFibers` 后，就完成了新的 fiber 树的生成。

```js
function reconcileChildFibers(
  returnFiber: Fiber,
  currentFirstChild: Fiber | null,
  newChild: any,
  lanes: Lanes,
): Fiber | null {
  // This indirection only exists so we can reset `thenableState` at the end.
  // It should get inlined by Closure.
  thenableIndexCounter = 0;
  const firstChildFiber = reconcileChildFibersImpl(
    returnFiber,
    currentFirstChild,
    newChild,
    lanes,
  );
  thenableState = null;
  // Don't bother to reset `thenableIndexCounter` to 0 because it always gets
  // set at the beginning.
  return firstChildFiber;
}
```

## 

```js
// 此API将使用协调本身的副作用标记子对象。
// 当我们遍历子节点和父节点时，它们会被添加到副作用列表中。
/**
 * @param returnFiber 当前正在处理的 workInProgree fiber
 * @param currentFirstChild 当前正在处理的 workInProgree 对应的 current 节点的第一个子节点，也就是 currentFiber.child
 * @param newChild 组件 render 的返回值
 * @param lanes
 * @returns {Fiber|*}
 */
function reconcileChildFibersImpl(
  returnFiber: Fiber,
  currentFirstChild: Fiber | null,
  newChild: any,
  lanes: Lanes,
): Fiber | null {
  // This function is not recursive.
  // If the top level item is an array, we treat it as a set of children,
  // not as a fragment. Nested arrays on the other hand will be treated as
  // fragment nodes. Recursion happens at the normal flow.

  // Handle top level unkeyed fragments as if they were arrays.
  // This leads to an ambiguity between <>{[...]}</> and <>...</>.
  // We treat the ambiguous cases above the same.

  //这个函数不是递归的。
  // 如果顶层元素是一个数组，我们将其视为一组子元素，而不是一个片段。
  // 另一方面，嵌套数组将被视为片段节点。递归发生在正常流程中。

  // 像处理数组一样处理顶层的非键(unkeyed)片段。
  // 这会导致<>{[...]}</> 和 <>...</>含混不清.
  // 我们同样对待上述的歧义情况。
  // TODO: Let's use recursion like we do for Usable nodes?

  // 是否是未键化的顶级片段节点
  const isUnkeyedTopLevelFragment =
    typeof newChild === 'object' &&
    newChild !== null &&
    newChild.type === REACT_FRAGMENT_TYPE &&
    newChild.key === null;
  if (isUnkeyedTopLevelFragment) {
    newChild = newChild.props.children;
  }

  // Handle object types
  if (typeof newChild === 'object' && newChild !== null) {
    switch (newChild.$$typeof) {
      case REACT_ELEMENT_TYPE:
        return placeSingleChild(
          reconcileSingleElement(
            returnFiber,
            currentFirstChild,
            newChild,
            lanes,
          ),
        );
      case REACT_PORTAL_TYPE:
        return placeSingleChild(
          reconcileSinglePortal(
            returnFiber,
            currentFirstChild,
            newChild,
            lanes,
          ),
        );
      case REACT_LAZY_TYPE:
        const payload = newChild._payload;
        const init = newChild._init;
        // TODO: This function is supposed to be non-recursive.
        return reconcileChildFibers(
          returnFiber,
          currentFirstChild,
          init(payload),
          lanes,
        );
    }

    if (isArray(newChild)) {
      return reconcileChildrenArray(
        returnFiber,
        currentFirstChild,
        newChild,
        lanes,
      );
    }

    if (getIteratorFn(newChild)) {
      return reconcileChildrenIterator(
        returnFiber,
        currentFirstChild,
        newChild,
        lanes,
      );
    }

    // Usables are a valid React node type. When React encounters a Usable in
    // a child position, it unwraps it using the same algorithm as `use`. For
    // example, for promises, React will throw an exception to unwind the
    // stack, then replay the component once the promise resolves.
    //
    // A difference from `use` is that React will keep unwrapping the value
    // until it reaches a non-Usable type.
    //
    // e.g. Usable<Usable<Usable<T>>> should resolve to T
    //
    // The structure is a bit unfortunate. Ideally, we shouldn't need to
    // replay the entire begin phase of the parent fiber in order to reconcile
    // the children again. This would require a somewhat significant refactor,
    // because reconcilation happens deep within the begin phase, and
    // depending on the type of work, not always at the end. We should
    // consider as an future improvement.
    if (typeof newChild.then === 'function') {
      const thenable: Thenable<any> = (newChild: any);
      return reconcileChildFibersImpl(
        returnFiber,
        currentFirstChild,
        unwrapThenable(thenable),
        lanes,
      );
    }

    if (
      newChild.$$typeof === REACT_CONTEXT_TYPE ||
      newChild.$$typeof === REACT_SERVER_CONTEXT_TYPE
    ) {
      const context: ReactContext<mixed> = (newChild: any);
      return reconcileChildFibersImpl(
        returnFiber,
        currentFirstChild,
        readContextDuringReconcilation(returnFiber, context, lanes),
        lanes,
      );
    }

    throwOnInvalidObjectType(returnFiber, newChild);
  }

  if (
    (typeof newChild === 'string' && newChild !== '') ||
    typeof newChild === 'number'
  ) {
    return placeSingleChild(
      reconcileSingleTextNode(
        returnFiber,
        currentFirstChild,
        '' + newChild,
        lanes,
      ),
    );
  }

  // Remaining cases are all treated as empty.
  return deleteRemainingChildren(returnFiber, currentFirstChild);
}
```

### reconcileSingleElement

这个方法用来处理新的 `wordInProgress` 节点只有一个后代的情况。

不论新的子节点是否可以复用 fiber，最后会对旧的未使用的所有 fiber 节点进行删除。

删除 fiber 并不会直接从树删除，而是将其添加到当前处理的 `wordInProgress` 节点的 `deletions` 属性中。

```js
function reconcileSingleElement(
  returnFiber: Fiber,
  currentFirstChild: Fiber | null,
  element: ReactElement,
  lanes: Lanes,
): Fiber {
  const key = element.key;
  let child = currentFirstChild;
  // 遍历 current child 列表
  while (child !== null) {
    // TODO: If key === null and child.key === null, then this only applies to
    // the first item in the list.
    if (child.key === key) {
      const elementType = element.type;
      if (elementType === REACT_FRAGMENT_TYPE) {
        // 同为 Fragment
        if (child.tag === Fragment) {
          deleteRemainingChildren(returnFiber, child.sibling);
          // 复用 fiber
          const existing = useFiber(child, element.props.children);
          // 更改 return 指针
          existing.return = returnFiber;
          if (__DEV__) {
            existing._debugSource = element._source;
            existing._debugOwner = element._owner;
          }
          return existing;
        }
      } else {
        if (
          child.elementType === elementType ||
          // Keep this check inline so it only runs on the false path:
          (__DEV__
            ? isCompatibleFamilyForHotReloading(child, element)
            : false) ||
          // Lazy types should reconcile their resolved type.
          // We need to do this after the Hot Reloading check above,
          // because hot reloading has different semantics than prod because
          // it doesn't resuspend. So we can't let the call below suspend.
          (typeof elementType === 'object' &&
            elementType !== null &&
            elementType.$$typeof === REACT_LAZY_TYPE &&
            resolveLazy(elementType) === child.type)
        ) {
          deleteRemainingChildren(returnFiber, child.sibling);
          const existing = useFiber(child, element.props);
          existing.ref = coerceRef(returnFiber, child, element);
          existing.return = returnFiber;
          if (__DEV__) {
            existing._debugSource = element._source;
            existing._debugOwner = element._owner;
          }
          return existing;
        }
      }
      // Didn't match.
      // 这里假定 key 是唯一的，key 相同，其余条件匹配不上，则认为不再有能匹配上的，因为其余的 key 肯定不一样。
      deleteRemainingChildren(returnFiber, child);
      break;
    } else {
      deleteChild(returnFiber, child);
    }
    child = child.sibling;
  }

  if (element.type === REACT_FRAGMENT_TYPE) {
    const created = createFiberFromFragment(
      element.props.children,
      returnFiber.mode,
      lanes,
      element.key,
    );
    created.return = returnFiber;
    return created;
  } else {
    const created = createFiberFromElement(element, returnFiber.mode, lanes);
    created.ref = coerceRef(returnFiber, currentFirstChild, element);
    created.return = returnFiber;
    return created;
  }
}
```

### reconcileChildrenArray

`reconcileChildrenArray` 将原始的 fiber 链表和新的元素列表看成两条并行的列表。

对比时，从列表头开始一一对比，对比的条件为 `key` 的值。

`key` 值相同则会对当前位置进行更新，如果 `elementType` 相同，则会复用当前的 fiber，如果不同，则会创建新的 fiber。

`key` 值不同，则认为从此处开始，列表已然不再相同。


经过循环对比后，会出现如下情况：

1. 两个列表同时到达末尾，结束。

1. 旧列表达到末尾，新列表未到达，则认为新列表比旧列表长，因此直接为剩余新列表项创建的 fiber。

1. 旧列表未达到末尾，新列表到达末尾，则认为新列表比旧列表短，那么直接将旧列表的剩余项删除。

1. 旧列表与新列表均为达到末尾，则认为是发生了突变的情况(也就是对比中出现了 key 不相同的情况)。这时，会遍历剩余的新列表项。为了达到复用的目的，则会将会为剩余的旧列表项生成一个 Map，便于新列表使用 `key` 或者 `index` 进行查找是否可以复用的 fiber。当处理完这些乱序的 fiber 后，会将 map 中剩余的未使用到的 fiber 进行删除。

```js
function reconcileChildrenArray(
  returnFiber: Fiber,
  currentFirstChild: Fiber | null,
  newChildren: Array<any>,
  lanes: Lanes,
): Fiber | null {
  
  // This algorithm can't optimize by searching from both ends since we
  // don't have backpointers on fibers. I'm trying to see how far we can get
  // with that model. If it ends up not being worth the tradeoffs, we can
  // add it later.

  // Even with a two ended optimization, we'd want to optimize for the case
  // where there are few changes and brute force the comparison instead of
  // going for the Map. It'd like to explore hitting that path first in
  // forward-only mode and only go for the Map once we notice that we need
  // lots of look ahead. This doesn't handle reversal as well as two ended
  // search but that's unusual. Besides, for the two ended optimization to
  // work on Iterables, we'd need to copy the whole set.

  // In this first iteration, we'll just live with hitting the bad case
  // (adding everything to a Map) in for every insert/move.

  // If you change this code, also update reconcileChildrenIterator() which
  // uses the same algorithm.

  // 这个算法不能通过从两端搜索来优化，因为我们在纤维上没有反指针。
  // 我想看看这种模式能走多远。如果它最终不值得权衡，我们可以稍后添加它。

  // 即使有一个两端的优化，我们希望优化的情况下，有很少的变化和蛮力比较，而不是去映射。
  // 它想要在前进模式下先探索那条路径，只有当我们注意到我们需要大量向前看时才会去映射。
  // 这处理反转不如两端搜索好，但这很不寻常。此外，为了在Iterables上进行两端优化，我们需要复制整个集合。

  // 在第一次迭代中，我们将在每次插入/移动时遇到坏情况(将所有内容添加到Map中)。

  // 如果你改变这段代码，也更新reconcileChildrenIterator()，它使用相同的算法。

  let resultingFirstChild: Fiber | null = null;
  let previousNewFiber: Fiber | null = null;

  let oldFiber = currentFirstChild;
  let lastPlacedIndex = 0;
  let newIdx = 0;
  let nextOldFiber = null;

  // 此循环的作用是按数组顺序遍历 olderFiber 链表和 newChildren
  // 以 key 为唯一重用标志
  // 双条件循环，退出循环的条件有三个：
  // 1. oldFiber 为 null、
  // 或者 newIdx ===  newChildren.length
  // 遇到第一个 key 不相同的元素
  // 遍历 workInProgress 的 chilren React Element
  for (; oldFiber !== null && newIdx < newChildren.length; newIdx++) {
    // 老树的长度超过现有树
    if (oldFiber.index > newIdx) {
      nextOldFiber = oldFiber;
      // 用于跳出循环
      oldFiber = null;
    } else {
      nextOldFiber = oldFiber.sibling;
    }

    // 更新插槽
    // 遇到 key 不一样就直接返回 null
    // 在 key 相同情况系，再判断 elementType 是否相同，相同则重用当前 fiber，如果不同，则新创建 fiber
    const newFiber = updateSlot(
      returnFiber,
      oldFiber,
      newChildren[newIdx],
      lanes,
    );
    if (newFiber === null) {
      // TODO: This breaks on empty slots like null children. That's
      // unfortunate because it triggers the slow path all the time. We need
      // a better way to communicate whether this was a miss or null,
      // boolean, undefined, etc.
      // 它会在空槽上断裂，就像空子槽一样。
      // 这很不幸，因为它总是触发慢路径。
      // 我们需要一种更好的方式来传达这是miss还是null、boolean、undefined等。
      if (oldFiber === null) {
        oldFiber = nextOldFiber;
      }
      break;
    }
    if (shouldTrackSideEffects) {
      if (oldFiber && newFiber.alternate === null) {
        // We matched the slot, but we didn't reuse the existing fiber, so we
        // need to delete the existing child.
        // 我们匹配了插槽，但是我们没有重用现有的光纤，所以我们需要删除现有的子节点。
        deleteChild(returnFiber, oldFiber);
      }
    }
    // 计算 lastPlacedIndex
    lastPlacedIndex = placeChild(newFiber, lastPlacedIndex, newIdx);
    if (previousNewFiber === null) {
      // TODO: Move out of the loop. This only happens for the first run.
      resultingFirstChild = newFiber;
    } else {
      // TODO: Defer siblings if we're not at the right index for this slot.
      // I.e. if we had null values before, then we want to defer this
      // for each null value. However, we also don't want to call updateSlot
      // with the previous one.
      previousNewFiber.sibling = newFiber;
    }
    previousNewFiber = newFiber;
    oldFiber = nextOldFiber;
  }

  // 成功遍历完 newChildren
  if (newIdx === newChildren.length) {
    // We've reached the end of the new children. We can delete the rest.
    // 我们已经到达了新孩子的终点。我们可以把剩下的删掉。
    deleteRemainingChildren(returnFiber, oldFiber);
    if (getIsHydrating()) {
      const numberOfForks = newIdx;
      pushTreeFork(returnFiber, numberOfForks);
    }
    return resultingFirstChild;
  }

  // 循环结束，但还未到达 newChildren 重点，且 fiber 链表也到达末尾
  // 这表示新数组的长度已经超过旧数组
  // 接下来就直接为 newChildren 中剩余的元素创建新的 fiber 节点
  if (oldFiber === null) {
    // If we don't have any more existing children we can choose a fast path
    // since the rest will all be insertions.
    // 如果没有子节点，我们可以选择一条快速路径，因为剩下的都是插入。
    for (; newIdx < newChildren.length; newIdx++) {
      const newFiber = createChild(returnFiber, newChildren[newIdx], lanes);
      if (newFiber === null) {
        continue;
      }
      lastPlacedIndex = placeChild(newFiber, lastPlacedIndex, newIdx);
      if (previousNewFiber === null) {
        // TODO: Move out of the loop. This only happens for the first run.
        resultingFirstChild = newFiber;
      } else {
        previousNewFiber.sibling = newFiber;
      }
      previousNewFiber = newFiber;
    }
    if (getIsHydrating()) {
      const numberOfForks = newIdx;
      pushTreeFork(returnFiber, numberOfForks);
    }
    return resultingFirstChild;
  }

  // Add all children to a key map for quick lookups.
  // 将所有子节点添加到键映射中以便快速查找。
  // 以fiber 节点的 key 和 index 为键的 Map
  const existingChildren = mapRemainingChildren(returnFiber, oldFiber);

  // Keep scanning and use the map to restore deleted items as moves.
  // 继续扫描和使用 map，以恢复删除的项目作为移动。
  for (; newIdx < newChildren.length; newIdx++) {
    const newFiber = updateFromMap(
      existingChildren,
      returnFiber,
      newIdx,
      newChildren[newIdx],
      lanes,
    );
    if (newFiber !== null) {
      if (shouldTrackSideEffects) {
        if (newFiber.alternate !== null) {
          // The new fiber is a work in progress, but if there exists a
          // current, that means that we reused the fiber. We need to delete
          // it from the child list so that we don't add it to the deletion
          // list.
          existingChildren.delete(
            newFiber.key === null ? newIdx : newFiber.key,
          );
        }
      }
      lastPlacedIndex = placeChild(newFiber, lastPlacedIndex, newIdx);
      if (previousNewFiber === null) {
        resultingFirstChild = newFiber;
      } else {
        previousNewFiber.sibling = newFiber;
      }
      previousNewFiber = newFiber;
    }
  }

  if (shouldTrackSideEffects) {
    // Any existing children that weren't consumed above were deleted. We need
    // to add them to the deletion list.
    // 删除上面没有使用的所有现有子节点。我们需要将它们添加到删除列表中。
    existingChildren.forEach(child => deleteChild(returnFiber, child));
  }

  if (getIsHydrating()) {
    const numberOfForks = newIdx;
    pushTreeFork(returnFiber, numberOfForks);
  }
  return resultingFirstChild;
}
```

### reconcileSinglePortal

```js
function reconcileSinglePortal(
  returnFiber: Fiber,
  currentFirstChild: Fiber | null,
  portal: ReactPortal,
  lanes: Lanes,
): Fiber {
  const key = portal.key;
  let child = currentFirstChild;
  while (child !== null) {
    // TODO: If key === null and child.key === null, then this only applies to
    // the first item in the list.
    if (child.key === key) {
      if (
        child.tag === HostPortal &&
        child.stateNode.containerInfo === portal.containerInfo &&
        child.stateNode.implementation === portal.implementation
      ) {
        deleteRemainingChildren(returnFiber, child.sibling);
        const existing = useFiber(child, portal.children || []);
        existing.return = returnFiber;
        return existing;
      } else {
        deleteRemainingChildren(returnFiber, child);
        break;
      }
    } else {
      deleteChild(returnFiber, child);
    }
    child = child.sibling;
  }

  const created = createFiberFromPortal(portal, returnFiber.mode, lanes);
  created.return = returnFiber;
  return created;
}
```

### reconcileChildrenIterator

```js
function reconcileChildrenIterator(
  returnFiber: Fiber,
  currentFirstChild: Fiber | null,
  newChildrenIterable: Iterable<mixed>,
  lanes: Lanes,
): Fiber | null {
  // This is the same implementation as reconcileChildrenArray(),
  // but using the iterator instead.

  const iteratorFn = getIteratorFn(newChildrenIterable);

  if (typeof iteratorFn !== 'function') {
    throw new Error(
      'An object is not an iterable. This error is likely caused by a bug in ' +
        'React. Please file an issue.',
    );
  }

  const newChildren = iteratorFn.call(newChildrenIterable);

  if (newChildren == null) {
    throw new Error('An iterable object provided no iterator.');
  }

  let resultingFirstChild: Fiber | null = null;
  let previousNewFiber: Fiber | null = null;

  let oldFiber = currentFirstChild;
  let lastPlacedIndex = 0;
  let newIdx = 0;
  let nextOldFiber = null;

  let step = newChildren.next();
  for (
    ;
    oldFiber !== null && !step.done;
    newIdx++, step = newChildren.next()
  ) {
    if (oldFiber.index > newIdx) {
      nextOldFiber = oldFiber;
      oldFiber = null;
    } else {
      nextOldFiber = oldFiber.sibling;
    }
    const newFiber = updateSlot(returnFiber, oldFiber, step.value, lanes);
    if (newFiber === null) {
      // TODO: This breaks on empty slots like null children. That's
      // unfortunate because it triggers the slow path all the time. We need
      // a better way to communicate whether this was a miss or null,
      // boolean, undefined, etc.
      if (oldFiber === null) {
        oldFiber = nextOldFiber;
      }
      break;
    }
    if (shouldTrackSideEffects) {
      if (oldFiber && newFiber.alternate === null) {
        // We matched the slot, but we didn't reuse the existing fiber, so we
        // need to delete the existing child.
        deleteChild(returnFiber, oldFiber);
      }
    }
    lastPlacedIndex = placeChild(newFiber, lastPlacedIndex, newIdx);
    if (previousNewFiber === null) {
      // TODO: Move out of the loop. This only happens for the first run.
      resultingFirstChild = newFiber;
    } else {
      // TODO: Defer siblings if we're not at the right index for this slot.
      // I.e. if we had null values before, then we want to defer this
      // for each null value. However, we also don't want to call updateSlot
      // with the previous one.
      previousNewFiber.sibling = newFiber;
    }
    previousNewFiber = newFiber;
    oldFiber = nextOldFiber;
  }

  if (step.done) {
    // We've reached the end of the new children. We can delete the rest.
    deleteRemainingChildren(returnFiber, oldFiber);
    if (getIsHydrating()) {
      const numberOfForks = newIdx;
      pushTreeFork(returnFiber, numberOfForks);
    }
    return resultingFirstChild;
  }

  if (oldFiber === null) {
    // If we don't have any more existing children we can choose a fast path
    // since the rest will all be insertions.
    for (; !step.done; newIdx++, step = newChildren.next()) {
      const newFiber = createChild(returnFiber, step.value, lanes);
      if (newFiber === null) {
        continue;
      }
      lastPlacedIndex = placeChild(newFiber, lastPlacedIndex, newIdx);
      if (previousNewFiber === null) {
        // TODO: Move out of the loop. This only happens for the first run.
        resultingFirstChild = newFiber;
      } else {
        previousNewFiber.sibling = newFiber;
      }
      previousNewFiber = newFiber;
    }
    if (getIsHydrating()) {
      const numberOfForks = newIdx;
      pushTreeFork(returnFiber, numberOfForks);
    }
    return resultingFirstChild;
  }

  // Add all children to a key map for quick lookups.
  const existingChildren = mapRemainingChildren(returnFiber, oldFiber);

  // Keep scanning and use the map to restore deleted items as moves.
  for (; !step.done; newIdx++, step = newChildren.next()) {
    const newFiber = updateFromMap(
      existingChildren,
      returnFiber,
      newIdx,
      step.value,
      lanes,
    );
    if (newFiber !== null) {
      if (shouldTrackSideEffects) {
        if (newFiber.alternate !== null) {
          // The new fiber is a work in progress, but if there exists a
          // current, that means that we reused the fiber. We need to delete
          // it from the child list so that we don't add it to the deletion
          // list.
          existingChildren.delete(
            newFiber.key === null ? newIdx : newFiber.key,
          );
        }
      }
      lastPlacedIndex = placeChild(newFiber, lastPlacedIndex, newIdx);
      if (previousNewFiber === null) {
        resultingFirstChild = newFiber;
      } else {
        previousNewFiber.sibling = newFiber;
      }
      previousNewFiber = newFiber;
    }
  }

  if (shouldTrackSideEffects) {
    // Any existing children that weren't consumed above were deleted. We need
    // to add them to the deletion list.
    existingChildren.forEach(child => deleteChild(returnFiber, child));
  }

  if (getIsHydrating()) {
    const numberOfForks = newIdx;
    pushTreeFork(returnFiber, numberOfForks);
  }
  return resultingFirstChild;
}
```

### reconcileSingleTextNode

```js
function reconcileSingleTextNode(
  returnFiber: Fiber,
  currentFirstChild: Fiber | null,
  textContent: string,
  lanes: Lanes,
): Fiber {
  // There's no need to check for keys on text nodes since we don't have a
  // way to define them.
  if (currentFirstChild !== null && currentFirstChild.tag === HostText) {
    // We already have an existing node so let's just update it and delete
    // the rest.
    deleteRemainingChildren(returnFiber, currentFirstChild.sibling);
    const existing = useFiber(currentFirstChild, textContent);
    existing.return = returnFiber;
    return existing;
  }
  // The existing first child is not a text node so we need to create one
  // and delete the existing ones.
  deleteRemainingChildren(returnFiber, currentFirstChild);
  const created = createFiberFromText(textContent, returnFiber.mode, lanes);
  created.return = returnFiber;
  return created;
}
```