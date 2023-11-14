## 异步与同步

legacy模式下：命中 batchedUpdates 时是异步 未命中 batchedUpdates 时是同步的。

concurrent 模式下：都是异步的。

在 18 以前的版本中，会根据 `executionContext` 标志来判断需要走的流程，如果 `executionContext` 不存在，则会进行同步更新。

React 应用被分为了三个阶段，挂载、更新、卸载。所有的更新，本质都是通过 `setState` 来触发，子组件的 `props` 也是某个祖先组件的 `state`。这是因为，只有调用 React 的内部 api 才能触发 React 内部的。

当 React 应用完成挂载后，有两种行为可以触发应用的更新：一是程序自动触发，如 setTimeout、Promise 等等；二是用户触发的更新，如点击事件。程序自动触发的更新，React 对其不能有完全的控制权，因此在 18 以下的版本中，由程序触发的更新不会设置 `executionContext`，因此会走同步更新的逻辑。而用户触发的更新是通过事件系统进行响应的，React 基于原生的 DOM 事件实现了自己的事件系统，因此其对用户触发的更新具有完全的控制权，在处理由用户触发的更新时，`executionContext` 会被设置为 `EventContext`。

从 18 开始，`setState` 触发的逻辑始终都会被放到一个异步的回调中。

## 流程概述

* 排队更新。当调用 `setState` 进行更新时，首先会将本次更新进行排队缓存。
* 调度更新。每次排队更新后，都会根据调度标记检查是否有已经被调度的更新存在。若存在则忽略，若不存在则安排一次调度。在 `concurrent` 模式下，被调度的更新操作都包装在一个异步的回调中。
* 处理调度。当开始处理调度，调度标记则又会被置为 `false`，因此，当有新的 `setState` 调用时，又会安排一次调度。React 使用内部的 `Scheduler` 进行调度任务的控制。当一个调度任务完成，则执行调度的回调，也就是执行更新操作。
* 更新。更新可分为两个阶段：渲染和提交。

  * 渲染。渲染过程就是构造新的 dom 树的过程，这个过程是递归的。在这个过程中会进行新老 fiber 的对比。

  * 提交。提交阶段则是将新的 dom 树进行挂载的过程，并且会执行一些副作用。


## 流程

以类组件的 `setState` 方法为例。`setState` 定义如下：

```js
// packages/react/src/ReactBaseClasses.js
Component.prototype.setState = function (partialState, callback) {
  //...
  this.updater.enqueueSetState(this, partialState, callback, 'setState');
};
```

当调用 `instance.setState` 时，实际实在调用 `updater.enqueueSetState` 方法。

在 fiber 架构下，`enqueueSetState` 的定义如下：

```js
// packages/react-reconciler/src/ReactFiberClassComponent.js
const classComponentUpdater = {
  //...
  // $FlowFixMe[missing-local-annot]
  // payload 是调用 setState 传入的参数
  enqueueSetState(inst: any, payload: any, callback) {
    // 类实例上有一个 _reactInternals 属性，用于存储 fiber
    const fiber = getInstance(inst);
    const lane = requestUpdateLane(fiber);

    // 创建更新对象
    const update = createUpdate(lane);
    update.payload = payload;
    if (callback !== undefined && callback !== null) {
      update.callback = callback;
    }

    // 递归查找 FiberRoots
    const root = enqueueUpdate(fiber, update, lane);
    if (root !== null) {
      scheduleUpdateOnFiber(root, fiber, lane);
      entangleTransitions(root, fiber, lane);
    }

    if (enableSchedulingProfiler) {
      markStateUpdateScheduled(fiber, lane);
    }
  },
};

```

每一次 `setState` 调用都会创建一个 `Update` 实例，
然后调用 `enqueueUpdate` 将这个 `Update` 实例添加到全局的缓存中，查找并返回到 fiber 树的根，
然后调用 `scheduleUpdateOnFiber` 调度本次更新。

`enqueueUpdate` 的定义如下：

```js
// packages/react-reconciler/src/ReactFiberClassUpdateQueue.js
function enqueueUpdate<State>(
  fiber: Fiber,
  update: Update<State>,
  lane: Lane,
): FiberRoot | null {
  //...
  const sharedQueue: SharedQueue<State> = (updateQueue: any).shared;
  return enqueueConcurrentClassUpdate(fiber, sharedQueue, update, lane);
}
```
`enqueueConcurrentClassUpdate` 定义如下：

```js
function enqueueConcurrentClassUpdate<State>(
  fiber: Fiber,
  queue: ClassQueue<State>,
  update: ClassUpdate<State>,
  lane: Lane,
): FiberRoot | null {
  const concurrentQueue: ConcurrentQueue = (queue: any);
  const concurrentUpdate: ConcurrentUpdate = (update: any);
  enqueueUpdate(fiber, concurrentQueue, concurrentUpdate, lane);
  return getRootForUpdatedFiber(fiber);
}
```
`enqueueConcurrentClassUpdate` 调用 `enqueueUpdate` 将本次更新加入到全局的更新队列中，
调用 `getRootForUpdatedFiber` 以获取当前 fiber 的 FiberRoot。


```js
// packages/react-reconciler/src/ReactFiberConcurrentUpdates.js
function enqueueUpdate(
  fiber: Fiber,
  queue: ConcurrentQueue | null,
  update: ConcurrentUpdate | null,
  lane: Lane,
) {
  // Don't update the `childLanes` on the return path yet. If we already in
  // the middle of rendering, wait until after it has completed.
  // 暂时不要更新返回路径上的 childLanes。如果我们已经在渲染的中间，等待，直到它已经完成。
  concurrentQueues[concurrentQueuesIndex++] = fiber;
  concurrentQueues[concurrentQueuesIndex++] = queue;
  concurrentQueues[concurrentQueuesIndex++] = update;
  concurrentQueues[concurrentQueuesIndex++] = lane;

  concurrentlyUpdatedLanes = mergeLanes(concurrentlyUpdatedLanes, lane);

  // The fiber's `lane` field is used in some places to check if any work is
  // scheduled, to perform an eager bailout, so we need to update it immediately.
  // TODO: We should probably move this to the "shared" queue instead.
  // 光纤的 `lane` 字段在某些地方用于检查是否有工作被安排，以执行紧急退出，因此我们需要立即更新它。
  // TODO: 我们可能应该将其移到“共享”队列中。
  fiber.lanes = mergeLanes(fiber.lanes, lane);
  const alternate = fiber.alternate;
  if (alternate !== null) {
    alternate.lanes = mergeLanes(alternate.lanes, lane);
  }
}
```

其中 `concurrentQueues` 是一个全局的缓存。
```js
// packages/react-reconciler/src/ReactFiberConcurrentUpdates.js

// If a render is in progress, and we receive an update from a concurrent event,
// we wait until the current render is over (either finished or interrupted)
// before adding it to the fiber/hook queue. Push to this array so we can
// access the queue, fiber, update, et al later.
// 如果渲染正在进行中，并且我们从并发事件接收到更新，
// 我们等待直到当前渲染结束(完成或中断)，然后将其添加到 fiber/hook 队列中。
// Push 到这个数组，这样我们就可以稍后访问队列、光纤、更新等。
const concurrentQueues: Array<any> = [];
```

```js
function getRootForUpdatedFiber(sourceFiber: Fiber): FiberRoot | null {
  // TODO: We will detect and infinite update loop and throw even if this fiber
  // has already unmounted. This isn't really necessary but it happens to be the
  // current behavior we've used for several release cycles. Consider not
  // performing this check if the updated fiber already unmounted, since it's
  // not possible for that to cause an infinite update loop.
  throwIfInfiniteUpdateLoopDetected();

  // When a setState happens, we must ensure the root is scheduled. Because
  // update queues do not have a backpointer to the root, the only way to do
  // this currently is to walk up the return path. This used to not be a big
  // deal because we would have to walk up the return path to set
  // the `childLanes`, anyway, but now those two traversals happen at
  // different times.
  // TODO: Consider adding a `root` backpointer on the update queue.
  // 当setState发生时，我们必须确保root被调度。
  // 因为更新队列没有指向 root 的反向指针，所以目前唯一的方法是沿着返回路径向上走。
  // 这在过去并不是什么大问题，因为我们必须沿着返回路径去设置 `childLanes`，但现在这两次遍历发生在不同的时间。
  // TODO:考虑在更新队列上添加一个 `root` 反向指针。
  detectUpdateOnUnmountedFiber(sourceFiber, sourceFiber);
  let node = sourceFiber;
  let parent = node.return;
  while (parent !== null) {
    detectUpdateOnUnmountedFiber(sourceFiber, node);
    node = parent;
    parent = node.return;
  }
  return node.tag === HostRoot ? (node.stateNode: FiberRoot) : null;
}
```

`scheduleUpdateOnFiber` 被调用，用以调度这些更新。

```js
function scheduleUpdateOnFiber(
  root: FiberRoot,
  fiber: Fiber,
  lane: Lane,
) {
  //...

  ensureRootIsScheduled(root);
}
```

```js
function ensureRootIsScheduled(root: FiberRoot): void {
  //...

  if (!didScheduleMicrotask) {
    didScheduleMicrotask = true;
    scheduleImmediateTask(processRootScheduleInMicrotask);
  }
}
```

```js
function scheduleImmediateTask(cb: () => mixed) {
  // ...
  if (supportsMicrotasks) {
    scheduleMicrotask(() => {
      // In Safari, appending an iframe forces microtasks to run.
      // https://github.com/facebook/react/issues/22459
      // We don't support running callbacks in the middle of render
      // or commit so we need to check against that.
      const executionContext = getExecutionContext();
      if ((executionContext & (RenderContext | CommitContext)) !== NoContext) {
        // Note that this would still prematurely flush the callbacks
        // if this happens outside render or commit phase (e.g. in an event).

        // Intentionally using a macrotask instead of a microtask here. This is
        // wrong semantically but it prevents an infinite loop. The bug is
        // Safari's, not ours, so we just do our best to not crash even though
        // the behavior isn't completely correct.
        Scheduler_scheduleCallback(ImmediateSchedulerPriority, cb);
        return;
      }
      cb();
    });
  } else {
    // If microtasks are not supported, use Scheduler.
    Scheduler_scheduleCallback(ImmediateSchedulerPriority, cb);
  }
}
```

在这里 `scheduleImmediateTask` 调用 `scheduleMicrotask` 将任务进行异步的调度。

这里就是为何在 concurrent 模式下 `setState` 一直为异步的原因。


```js
const scheduleTimeout: any =
  typeof setTimeout === 'function' ? setTimeout : (undefined: any);
const localPromise = typeof Promise === 'function' ? Promise : undefined;

const scheduleMicrotask: any =
  typeof queueMicrotask === 'function'
    ? queueMicrotask
    : typeof localPromise !== 'undefined'
    ? callback =>
        localPromise.resolve(null).then(callback).catch(handleErrorInNextTick)
    : scheduleTimeout;
```

当异步任务达到触发条件，`processRootScheduleInMicrotask` 将被调用。

```js
function processRootScheduleInMicrotask() {
  // This function is always called inside a microtask. It should never be
  // called synchronously.
  didScheduleMicrotask = false;
  if (__DEV__) {
    didScheduleMicrotask_act = false;
  }

  // We'll recompute this as we iterate through all the roots and schedule them.
  mightHavePendingSyncWork = false;

  const currentTime = now();

  let prev = null;
  let root = firstScheduledRoot;
  while (root !== null) {
    const next = root.next;

    if (
      currentEventTransitionLane !== NoLane &&
      shouldAttemptEagerTransition()
    ) {
      markRootEntangled(root, mergeLanes(currentEventTransitionLane, SyncLane));
    }

    const nextLanes = scheduleTaskForRootDuringMicrotask(root, currentTime);
    if (nextLanes === NoLane) {
      // This root has no more pending work. Remove it from the schedule. To
      // guard against subtle reentrancy bugs, this microtask is the only place
      // we do this — you can add roots to the schedule whenever, but you can
      // only remove them here.

      // Null this out so we know it's been removed from the schedule.
      root.next = null;
      if (prev === null) {
        // This is the new head of the list
        firstScheduledRoot = next;
      } else {
        prev.next = next;
      }
      if (next === null) {
        // This is the new tail of the list
        lastScheduledRoot = prev;
      }
    } else {
      // This root still has work. Keep it in the list.
      prev = root;
      if (includesSyncLane(nextLanes)) {
        mightHavePendingSyncWork = true;
      }
    }
    root = next;
  }

  currentEventTransitionLane = NoLane;

  // At the end of the microtask, flush any pending synchronous work. This has
  // to come at the end, because it does actual rendering work that might throw.
  flushSyncWorkOnAllRoots();
}
```

上面代码的关键所在就是 `scheduleTaskForRootDuringMicrotask`：

```js
function scheduleTaskForRootDuringMicrotask(
  root: FiberRoot,
  currentTime: number,
): Lane {
  // This function is always called inside a microtask, or at the very end of a
  // rendering task right before we yield to the main thread. It should never be
  // called synchronously.
  //这个函数总是在微任务中被调用，或者在渲染任务的最后，在我们屈服于主线程之前被调用。
  // 它永远不应该被同步调用。
  //
  // TODO: Unless enableDeferRootSchedulingToMicrotask is off. We need to land
  // that ASAP to unblock additional features we have planned.
  //
  // This function also never performs React work synchronously; it should
  // only schedule work to be performed later, in a separate task or microtask.

  // Check if any lanes are being starved by other work. If so, mark them as
  // expired so we know to work on those next.
  markStarvedLanesAsExpired(root, currentTime);

  // Determine the next lanes to work on, and their priority.
  const workInProgressRoot = getWorkInProgressRoot();
  const workInProgressRootRenderLanes = getWorkInProgressRootRenderLanes();
  const nextLanes = getNextLanes(
    root,
    root === workInProgressRoot ? workInProgressRootRenderLanes : NoLanes,
  );

  const existingCallbackNode = root.callbackNode;
  if (
    // Check if there's nothing to work on
    nextLanes === NoLanes ||
    // If this root is currently suspended and waiting for data to resolve, don't
    // schedule a task to render it. We'll either wait for a ping, or wait to
    // receive an update.
    //
    // Suspended render phase
    (root === workInProgressRoot && isWorkLoopSuspendedOnData()) ||
    // Suspended commit phase
    root.cancelPendingCommit !== null
  ) {
    // Fast path: There's nothing to work on.
    if (existingCallbackNode !== null) {
      cancelCallback(existingCallbackNode);
    }
    root.callbackNode = null;
    root.callbackPriority = NoLane;
    return NoLane;
  }

  // Schedule a new callback in the host environment.
  if (includesSyncLane(nextLanes)) {
    // Synchronous work is always flushed at the end of the microtask, so we
    // don't need to schedule an additional task.
    if (existingCallbackNode !== null) {
      cancelCallback(existingCallbackNode);
    }
    root.callbackPriority = SyncLane;
    root.callbackNode = null;
    return SyncLane;
  } else {
    // We use the highest priority lane to represent the priority of the callback.
    const existingCallbackPriority = root.callbackPriority;
    const newCallbackPriority = getHighestPriorityLane(nextLanes);

    if (
      newCallbackPriority === existingCallbackPriority &&
      // Special case related to `act`. If the currently scheduled task is a
      // Scheduler task, rather than an `act` task, cancel it and re-schedule
      // on the `act` queue.
      !(
        __DEV__ &&
        ReactCurrentActQueue.current !== null &&
        existingCallbackNode !== fakeActCallbackNode
      )
    ) {
      // The priority hasn't changed. We can reuse the existing task.
      return newCallbackPriority;
    } else {
      // Cancel the existing callback. We'll schedule a new one below.
      cancelCallback(existingCallbackNode);
    }

    let schedulerPriorityLevel;
    switch (lanesToEventPriority(nextLanes)) {
      case DiscreteEventPriority:
        schedulerPriorityLevel = ImmediateSchedulerPriority;
        break;
      case ContinuousEventPriority:
        schedulerPriorityLevel = UserBlockingSchedulerPriority;
        break;
      case DefaultEventPriority:
        schedulerPriorityLevel = NormalSchedulerPriority;
        break;
      case IdleEventPriority:
        schedulerPriorityLevel = IdleSchedulerPriority;
        break;
      default:
        schedulerPriorityLevel = NormalSchedulerPriority;
        break;
    }

    const newCallbackNode = scheduleCallback(
      schedulerPriorityLevel,
      performConcurrentWorkOnRoot.bind(null, root),
    );

    root.callbackPriority = newCallbackPriority;
    root.callbackNode = newCallbackNode;
    return newCallbackPriority;
  }
}
```

其中

```js
const newCallbackNode = scheduleCallback(
  schedulerPriorityLevel,
  performConcurrentWorkOnRoot.bind(null, root),
);
```


```js
function scheduleCallback(
  priorityLevel: PriorityLevel,
  callback: RenderTaskFn,
) {
   //...
  return Scheduler_scheduleCallback(priorityLevel, callback);
}
```

而

```js
import * as Scheduler from 'scheduler';
const scheduleCallback = Scheduler.unstable_scheduleCallback;
```

`unstable_scheduleCallback` 定义如下：

```js
function unstable_scheduleCallback(
  priorityLevel: PriorityLevel,
  callback: Callback,
  options?: {delay: number},
): Task {
  var currentTime = getCurrentTime();

  var startTime;
  if (typeof options === 'object' && options !== null) {
    var delay = options.delay;
    if (typeof delay === 'number' && delay > 0) {
      startTime = currentTime + delay;
    } else {
      startTime = currentTime;
    }
  } else {
    startTime = currentTime;
  }

  var timeout;
  switch (priorityLevel) {
    case ImmediatePriority:
      timeout = IMMEDIATE_PRIORITY_TIMEOUT;
      break;
    case UserBlockingPriority:
      timeout = USER_BLOCKING_PRIORITY_TIMEOUT;
      break;
    case IdlePriority:
      timeout = IDLE_PRIORITY_TIMEOUT;
      break;
    case LowPriority:
      timeout = LOW_PRIORITY_TIMEOUT;
      break;
    case NormalPriority:
    default:
      timeout = NORMAL_PRIORITY_TIMEOUT;
      break;
  }

  var expirationTime = startTime + timeout;

  var newTask: Task = {
    id: taskIdCounter++,
    callback,
    priorityLevel,
    startTime,
    expirationTime,
    sortIndex: -1,
  };
  if (enableProfiling) {
    newTask.isQueued = false;
  }

  if (startTime > currentTime) {
    // This is a delayed task.
    newTask.sortIndex = startTime;
    push(timerQueue, newTask);
    if (peek(taskQueue) === null && newTask === peek(timerQueue)) {
      // All tasks are delayed, and this is the task with the earliest delay.
      if (isHostTimeoutScheduled) {
        // Cancel an existing timeout.
        cancelHostTimeout();
      } else {
        isHostTimeoutScheduled = true;
      }
      // Schedule a timeout.
      requestHostTimeout(handleTimeout, startTime - currentTime);
    }
  } else {
    newTask.sortIndex = expirationTime;
    push(taskQueue, newTask);
    if (enableProfiling) {
      markTaskStart(newTask, currentTime);
      newTask.isQueued = true;
    }
    // Schedule a host callback, if needed. If we're already performing work,
    // wait until the next time we yield.
    if (!isHostCallbackScheduled && !isPerformingWork) {
      isHostCallbackScheduled = true;
      requestHostCallback(flushWork);
    }
  }

  return newTask;
}
```

在上面的方法中，创建了一个新的任务对象 `newTask`。
如果任务是一个异步任务，那就将其添加到 `timerQueue` 队列中。
如果任务是一个同步任务，那就将其添加到 `taskQueue` 队列中。

此方法会调用 `requestHostTimeout` 和 `requestHostCallback`，以完成对 `timerQueue` 和 `taskQueue` 的调度。
它们整体形成一个调度异步任务和同步任务的循环，
异步任务调度的结果最终是将到期的异步任务添加到 `taskQueue` 队列，
然后从 `taskQueue` 中取出最近的一个任务，然后执行。
其执行的结果就是调用 `performConcurrentWorkOnRoot`。

```js
function performConcurrentWorkOnRoot(
  root: FiberRoot,
  didTimeout: boolean,
): RenderTaskFn | null {
  if (enableProfilerTimer && enableProfilerNestedUpdatePhase) {
    resetNestedUpdateFlag();
  }

  if ((executionContext & (RenderContext | CommitContext)) !== NoContext) {
    throw new Error('Should not already be working.');
  }

  // Flush any pending passive effects before deciding which lanes to work on,
  // in case they schedule additional work.
  const originalCallbackNode = root.callbackNode;
  const didFlushPassiveEffects = flushPassiveEffects();
  if (didFlushPassiveEffects) {
    // Something in the passive effect phase may have canceled the current task.
    // Check if the task node for this root was changed.
    if (root.callbackNode !== originalCallbackNode) {
      // The current task was canceled. Exit. We don't need to call
      // `ensureRootIsScheduled` because the check above implies either that
      // there's a new task, or that there's no remaining work on this root.
      return null;
    } else {
      // Current task was not canceled. Continue.
    }
  }

  // Determine the next lanes to work on, using the fields stored
  // on the root.
  // TODO: This was already computed in the caller. Pass it as an argument.
  let lanes = getNextLanes(
    root,
    root === workInProgressRoot ? workInProgressRootRenderLanes : NoLanes,
  );
  if (lanes === NoLanes) {
    // Defensive coding. This is never expected to happen.
    return null;
  }

  // We disable time-slicing in some cases: if the work has been CPU-bound
  // for too long ("expired" work, to prevent starvation), or we're in
  // sync-updates-by-default mode.
  // TODO: We only check `didTimeout` defensively, to account for a Scheduler
  // bug we're still investigating. Once the bug in Scheduler is fixed,
  // we can remove this, since we track expiration ourselves.
  const shouldTimeSlice =
    !includesBlockingLane(root, lanes) &&
    !includesExpiredLane(root, lanes) &&
    (disableSchedulerTimeoutInWorkLoop || !didTimeout);
  let exitStatus = shouldTimeSlice
    ? renderRootConcurrent(root, lanes)
    : renderRootSync(root, lanes);
  if (exitStatus !== RootInProgress) {
    if (exitStatus === RootErrored) {
      // If something threw an error, try rendering one more time. We'll
      // render synchronously to block concurrent data mutations, and we'll
      // includes all pending updates are included. If it still fails after
      // the second attempt, we'll give up and commit the resulting tree.
      const originallyAttemptedLanes = lanes;
      const errorRetryLanes = getLanesToRetrySynchronouslyOnError(
        root,
        originallyAttemptedLanes,
      );
      if (errorRetryLanes !== NoLanes) {
        lanes = errorRetryLanes;
        exitStatus = recoverFromConcurrentError(
          root,
          originallyAttemptedLanes,
          errorRetryLanes,
        );
      }
    }
    if (exitStatus === RootFatalErrored) {
      const fatalError = workInProgressRootFatalError;
      prepareFreshStack(root, NoLanes);
      markRootSuspended(root, lanes);
      ensureRootIsScheduled(root);
      throw fatalError;
    }

    if (exitStatus === RootDidNotComplete) {
      // The render unwound without completing the tree. This happens in special
      // cases where need to exit the current render without producing a
      // consistent tree or committing.
      markRootSuspended(root, lanes);
    } else {
      // The render completed.

      // Check if this render may have yielded to a concurrent event, and if so,
      // confirm that any newly rendered stores are consistent.
      // TODO: It's possible that even a concurrent render may never have yielded
      // to the main thread, if it was fast enough, or if it expired. We could
      // skip the consistency check in that case, too.
      const renderWasConcurrent = !includesBlockingLane(root, lanes);
      const finishedWork: Fiber = (root.current.alternate: any);
      if (
        renderWasConcurrent &&
        !isRenderConsistentWithExternalStores(finishedWork)
      ) {
        // A store was mutated in an interleaved event. Render again,
        // synchronously, to block further mutations.
        exitStatus = renderRootSync(root, lanes);

        // We need to check again if something threw
        if (exitStatus === RootErrored) {
          const originallyAttemptedLanes = lanes;
          const errorRetryLanes = getLanesToRetrySynchronouslyOnError(
            root,
            originallyAttemptedLanes,
          );
          if (errorRetryLanes !== NoLanes) {
            lanes = errorRetryLanes;
            exitStatus = recoverFromConcurrentError(
              root,
              originallyAttemptedLanes,
              errorRetryLanes,
            );
            // We assume the tree is now consistent because we didn't yield to any
            // concurrent events.
          }
        }
        if (exitStatus === RootFatalErrored) {
          const fatalError = workInProgressRootFatalError;
          prepareFreshStack(root, NoLanes);
          markRootSuspended(root, lanes);
          ensureRootIsScheduled(root);
          throw fatalError;
        }

        // FIXME: Need to check for RootDidNotComplete again. The factoring here
        // isn't ideal.
      }

      // We now have a consistent tree. The next step is either to commit it,
      // or, if something suspended, wait to commit it after a timeout.
      root.finishedWork = finishedWork;
      root.finishedLanes = lanes;
      finishConcurrentRender(root, exitStatus, finishedWork, lanes);
    }
  }

  ensureRootIsScheduled(root);
  return getContinuationForRoot(root, originalCallbackNode);
}
```

这段代码调用了  `renderRootConcurrent` 来开启更新的[渲染阶段](/react/core/render-phase.md)，
当渲染完成后，代码又调用 `finishConcurrentRender` 来开启更新的[提交阶段](/react/core/commit-phase.md)。