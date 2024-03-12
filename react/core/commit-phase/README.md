# 提交阶段


提交阶段又被划分为了三个子阶段：

* before mutation。
* mutation phase。
* layout phase。

提交阶段以 `finishConcurrentRender` 为入口，

```js
function finishConcurrentRender(
  root: FiberRoot,
  exitStatus: RootExitStatus,
  finishedWork: Fiber,
  lanes: Lanes,
) {
  // TODO: The fact that most of these branches are identical suggests that some
  // of the exit statuses are not best modeled as exit statuses and should be
  // tracked orthogonally.
  switch (exitStatus) {
    case RootInProgress:
    case RootFatalErrored: {
      throw new Error('Root did not complete. This is a bug in React.');
    }
    case RootSuspendedWithDelay: {
      if (includesOnlyTransitions(lanes)) {
        // This is a transition, so we should exit without committing a
        // placeholder and without scheduling a timeout. Delay indefinitely
        // until we receive more data.
        markRootSuspended(root, lanes);
        return;
      }
      // Commit the placeholder.
      break;
    }
    case RootErrored:
    case RootSuspended:
    case RootCompleted: {
      break;
    }
    default: {
      throw new Error('Unknown root exit status.');
    }
  }

  if (shouldForceFlushFallbacksInDEV()) {
    // We're inside an `act` scope. Commit immediately.
    //...
  } else {
    if (
      includesOnlyRetries(lanes) &&
      (alwaysThrottleRetries || exitStatus === RootSuspended)
    ) {
      // This render only included retries, no updates. Throttle committing
      // retries so that we don't show too many loading states too quickly.
      const msUntilTimeout =
        globalMostRecentFallbackTime + FALLBACK_THROTTLE_MS - now();

      // Don't bother with a very short suspense time.
      if (msUntilTimeout > 10) {
        markRootSuspended(root, lanes);

        const nextLanes = getNextLanes(root, NoLanes);
        if (nextLanes !== NoLanes) {
          // There's additional work we can do on this root. We might as well
          // attempt to work on that while we're suspended.
          return;
        }

        // The render is suspended, it hasn't timed out, and there's no
        // lower priority work to do. Instead of committing the fallback
        // immediately, wait for more data to arrive.
        // TODO: Combine retry throttling with Suspensey commits. Right now they
        // run one after the other.
        root.timeoutHandle = scheduleTimeout(
          commitRootWhenReady.bind(
            null,
            root,
            finishedWork,
            workInProgressRootRecoverableErrors,
            workInProgressTransitions,
            lanes,
          ),
          msUntilTimeout,
        );
        return;
      }
    }
    commitRootWhenReady(
      root,
      finishedWork,
      workInProgressRootRecoverableErrors,
      workInProgressTransitions,
      lanes,
    );
  }
}
```

接下来则是调用 `commitRootWhenReady` 进行实际的处理：

```js
function commitRootWhenReady(
  root: FiberRoot,
  finishedWork: Fiber,
  recoverableErrors: Array<CapturedValue<mixed>> | null,
  transitions: Array<Transition> | null,
  lanes: Lanes,
) {
  // TODO: Combine retry throttling with Suspensey commits. Right now they run
  // one after the other.
  if (includesOnlyNonUrgentLanes(lanes)) {
    // Before committing, ask the renderer whether the host tree is ready.
    // If it's not, we'll wait until it notifies us.
    startSuspendingCommit();
    // This will walk the completed fiber tree and attach listeners to all
    // the suspensey resources. The renderer is responsible for accumulating
    // all the load events. This all happens in a single synchronous
    // transaction, so it track state in its own module scope.
    accumulateSuspenseyCommit(finishedWork);
    // At the end, ask the renderer if it's ready to commit, or if we should
    // suspend. If it's not ready, it will return a callback to subscribe to
    // a ready event.
    const schedulePendingCommit = waitForCommitToBeReady();
    if (schedulePendingCommit !== null) {
      // NOTE: waitForCommitToBeReady returns a subscribe function so that we
      // only allocate a function if the commit isn't ready yet. The other
      // pattern would be to always pass a callback to waitForCommitToBeReady.

      // Not yet ready to commit. Delay the commit until the renderer notifies
      // us that it's ready. This will be canceled if we start work on the
      // root again.
      root.cancelPendingCommit = schedulePendingCommit(
        commitRoot.bind(null, root, recoverableErrors, transitions),
      );
      markRootSuspended(root, lanes);
      return;
    }
  }

  // Otherwise, commit immediately.
  commitRoot(root, recoverableErrors, transitions);
}
```

```js
function commitRoot(
  root: FiberRoot,
  recoverableErrors: null | Array<CapturedValue<mixed>>,
  transitions: Array<Transition> | null,
) {
  // TODO: This no longer makes any sense. We already wrap the mutation and
  // layout phases. Should be able to remove.
  const previousUpdateLanePriority = getCurrentUpdatePriority();
  const prevTransition = ReactCurrentBatchConfig.transition;

  try {
    ReactCurrentBatchConfig.transition = null;
    setCurrentUpdatePriority(DiscreteEventPriority);
    commitRootImpl(
      root,
      recoverableErrors,
      transitions,
      previousUpdateLanePriority,
    );
  } finally {
    ReactCurrentBatchConfig.transition = prevTransition;
    setCurrentUpdatePriority(previousUpdateLanePriority);
  }

  return null;
}
```

```js

function commitRootImpl(
  root: FiberRoot,
  recoverableErrors: null | Array<CapturedValue<mixed>>,
  transitions: Array<Transition> | null,
  renderPriorityLevel: EventPriority,
) {
  do {
    // `flushPassiveEffects` will call `flushSyncUpdateQueue` at the end, which
    // means `flushPassiveEffects` will sometimes result in additional
    // passive effects. So we need to keep flushing in a loop until there are
    // no more pending effects.
    // TODO: Might be better if `flushPassiveEffects` did not automatically
    // flush synchronous work at the end, to avoid factoring hazards like this.

    // ' flushPassiveEffects '将在最后调用' flushSyncUpdateQueue '，
    // 这意味着' flushPassiveEffects '有时会导致额外的被动效果。
    // 所以我们需要在循环中不断刷新，直到没有更多的挂起的效果。
    // TODO:如果'flushPassiveEffects'在结束时不自动刷新同步工作可能会更好，以避免这样的风险分解。
    flushPassiveEffects();
  } while (rootWithPendingPassiveEffects !== null);
  flushRenderPhaseStrictModeWarningsInDEV();

  if ((executionContext & (RenderContext | CommitContext)) !== NoContext) {
    throw new Error('Should not already be working.');
  }

  const finishedWork = root.finishedWork;
  const lanes = root.finishedLanes;

  if (enableSchedulingProfiler) {
    markCommitStarted(lanes);
  }

  if (finishedWork === null) {
    if (enableSchedulingProfiler) {
      markCommitStopped();
    }

    return null;
  } else {
    //...
  }
  root.finishedWork = null;
  root.finishedLanes = NoLanes;

  if (finishedWork === root.current) {
    throw new Error(
      'Cannot commit the same tree as before. This error is likely caused by ' +
        'a bug in React. Please file an issue.',
    );
  }

  // commitRoot never returns a continuation; it always finishes synchronously.
  // So we can clear these now to allow a new callback to be scheduled.

  // commitRoot从不返回continuation;它总是同步完成。
  // 我们现在可以清除这些来允许一个新的回调被安排。
  root.callbackNode = null;
  root.callbackPriority = NoLane;
  root.cancelPendingCommit = null;

  // Check which lanes no longer have any work scheduled on them, and mark
  // those as finished.
  // 检查哪些车道不再安排任何工作，并标记这些都完成了。
  let remainingLanes = mergeLanes(finishedWork.lanes, finishedWork.childLanes);

  // Make sure to account for lanes that were updated by a concurrent event
  // during the render phase; don't mark them as finished.
  // 确保考虑在渲染阶段被并发事件更新的车道;不要把它们标记为完成。
  const concurrentlyUpdatedLanes = getConcurrentlyUpdatedLanes();
  remainingLanes = mergeLanes(remainingLanes, concurrentlyUpdatedLanes);

  markRootFinished(root, remainingLanes);

  if (root === workInProgressRoot) {
    // We can reset these now that they are finished.
    // 现在它们已经完成了，我们可以重置它们。
    workInProgressRoot = null;
    workInProgress = null;
    workInProgressRootRenderLanes = NoLanes;
  } else {
    // This indicates that the last root we worked on is not the same one that
    // we're committing now. This most commonly happens when a suspended root
    // times out.
    //这表明我们最后处理的根不是我们现在提交的根。这通常发生在挂起的根超时时。
  }

  // If there are pending passive effects, schedule a callback to process them.
  // Do this as early as possible, so it is queued before anything else that
  // might get scheduled in the commit phase. (See #16714.)
  // TODO: Delete all other places that schedule the passive effect callback
  // They're redundant.

  //  如果有等待的被动效果，调度回调来处理它们。
  // 尽可能早地执行此操作，以便在提交阶段可能安排的其他事项之前将其排队。(见# 16714)。
  // TODO:删除所有其他安排被动效果回调的地方，它们是多余的。
  if (
    (finishedWork.subtreeFlags & PassiveMask) !== NoFlags ||
    (finishedWork.flags & PassiveMask) !== NoFlags
  ) {
    if (!rootDoesHavePassiveEffects) {
      rootDoesHavePassiveEffects = true;
      pendingPassiveEffectsRemainingLanes = remainingLanes;
      // workInProgressTransitions might be overwritten, so we want
      // to store it in pendingPassiveTransitions until they get processed
      // We need to pass this through as an argument to commitRoot
      // because workInProgressTransitions might have changed between
      // the previous render and commit if we throttle the commit
      // with setTimeout
      pendingPassiveTransitions = transitions;
      scheduleCallback(NormalSchedulerPriority, () => {
        flushPassiveEffects();
        // This render triggered passive effects: release the root cache pool
        // *after* passive effects fire to avoid freeing a cache pool that may
        // be referenced by a node in the tree (HostRoot, Cache boundary etc)
        return null;
      });
    }
  }

  // Check if there are any effects in the whole tree.
  // TODO: This is left over from the effect list implementation, where we had
  // to check for the existence of `firstEffect` to satisfy Flow. I think the
  // only other reason this optimization exists is because it affects profiling.
  // Reconsider whether this is necessary.

  // 检查整个树是否有任何效果。
  // TODO:这是从效果列表实现中遗留下来的，我们必须检查“firstEffect”是否存在以满足流。
  // 我认为这种优化存在的另一个原因是它影响了性能分析。重新考虑这是否必要
  const subtreeHasEffects =
    (finishedWork.subtreeFlags &
      (BeforeMutationMask | MutationMask | LayoutMask | PassiveMask)) !==
    NoFlags;
  const rootHasEffect =
    (finishedWork.flags &
      (BeforeMutationMask | MutationMask | LayoutMask | PassiveMask)) !==
    NoFlags;

  if (subtreeHasEffects || rootHasEffect) {
    const prevTransition = ReactCurrentBatchConfig.transition;
    ReactCurrentBatchConfig.transition = null;
    const previousPriority = getCurrentUpdatePriority();
    setCurrentUpdatePriority(DiscreteEventPriority);

    const prevExecutionContext = executionContext;
    executionContext |= CommitContext;

    // Reset this to null before calling lifecycles
    ReactCurrentOwner.current = null;

    // The commit phase is broken into several sub-phases. We do a separate pass
    // of the effect list for each phase: all mutation effects come before all
    // layout effects, and so on.

    // The first phase a "before mutation" phase. We use this phase to read the
    // state of the host tree right before we mutate it. This is where
    // getSnapshotBeforeUpdate is called.

    // 提交阶段被分成几个子阶段。
    // 我们对每个阶段的副作用列表进行单独的传递：所有突变副作用都在所有布局效果之前，依此类推。

    // 第一阶段是“突变前”阶段。
    // 在对宿主树进行变异之前，我们使用这个阶段来读取它的状态。
    // 这就是 getSnapshotBeforeUpdate 被调用的地方。
    const shouldFireAfterActiveInstanceBlur = commitBeforeMutationEffects(
      root,
      finishedWork,
    );

    if (enableProfilerTimer) {
      // Mark the current commit time to be shared by all Profilers in this
      // batch. This enables them to be grouped later.
      recordCommitTime();
    }

    if (enableProfilerTimer && enableProfilerNestedUpdateScheduledHook) {
      // Track the root here, rather than in commitLayoutEffects(), because of ref setters.
      // Updates scheduled during ref detachment should also be flagged.
      rootCommittingMutationOrLayoutEffects = root;
    }

    // The next phase is the mutation phase, where we mutate the host tree.
    // 下一个阶段是突变阶段，我们使宿主树发生突变。
    commitMutationEffects(root, finishedWork, lanes);

    if (enableCreateEventHandleAPI) {
      if (shouldFireAfterActiveInstanceBlur) {
        afterActiveInstanceBlur();
      }
    }
    resetAfterCommit(root.containerInfo);

    // The work-in-progress tree is now the current tree. This must come after
    // the mutation phase, so that the previous tree is still current during
    // componentWillUnmount, but before the layout phase, so that the finished
    // work is current during componentDidMount/Update.
    root.current = finishedWork;

    // The next phase is the layout phase, where we call effects that read
    // the host tree after it's been mutated. The idiomatic use case for this is
    // layout, but class component lifecycles also fire here for legacy reasons.
    if (enableSchedulingProfiler) {
      markLayoutEffectsStarted(lanes);
    }
    commitLayoutEffects(finishedWork, root, lanes);

    if (enableSchedulingProfiler) {
      markLayoutEffectsStopped();
    }

    if (enableProfilerTimer && enableProfilerNestedUpdateScheduledHook) {
      rootCommittingMutationOrLayoutEffects = null;
    }

    // Tell Scheduler to yield at the end of the frame, so the browser has an
    // opportunity to paint.
    requestPaint();

    executionContext = prevExecutionContext;

    // Reset the priority to the previous non-sync value.
    setCurrentUpdatePriority(previousPriority);
    ReactCurrentBatchConfig.transition = prevTransition;
  } else {
    // No effects.
    root.current = finishedWork;
    // Measure these anyway so the flamegraph explicitly shows that there were
    // no effects.
    // TODO: Maybe there's a better way to report this.
    if (enableProfilerTimer) {
      recordCommitTime();
    }
  }

  const rootDidHavePassiveEffects = rootDoesHavePassiveEffects;

  if (rootDoesHavePassiveEffects) {
    // This commit has passive effects. Stash a reference to them. But don't
    // schedule a callback until after flushing layout work.
    rootDoesHavePassiveEffects = false;
    rootWithPendingPassiveEffects = root;
    pendingPassiveEffectsLanes = lanes;
  } else {
    // There were no passive effects, so we can immediately release the cache
    // pool for this render.
    releaseRootPooledCache(root, remainingLanes);
  }

  // Read this again, since an effect might have updated it
  remainingLanes = root.pendingLanes;

  // Check if there's remaining work on this root
  // TODO: This is part of the `componentDidCatch` implementation. Its purpose
  // is to detect whether something might have called setState inside
  // `componentDidCatch`. The mechanism is known to be flawed because `setState`
  // inside `componentDidCatch` is itself flawed — that's why we recommend
  // `getDerivedStateFromError` instead. However, it could be improved by
  // checking if remainingLanes includes Sync work, instead of whether there's
  // any work remaining at all (which would also include stuff like Suspense
  // retries or transitions). It's been like this for a while, though, so fixing
  // it probably isn't that urgent.
  if (remainingLanes === NoLanes) {
    // If there's no remaining work, we can clear the set of already failed
    // error boundaries.
    legacyErrorBoundariesThatAlreadyFailed = null;
  }

  onCommitRootDevTools(finishedWork.stateNode, renderPriorityLevel);

  if (enableUpdaterTracking) {
    if (isDevToolsPresent) {
      root.memoizedUpdaters.clear();
    }
  }

  // Always call this before exiting `commitRoot`, to ensure that any
  // additional work on this root is scheduled.
  ensureRootIsScheduled(root);

  if (recoverableErrors !== null) {
    // There were errors during this render, but recovered from them without
    // needing to surface it to the UI. We log them here.
    const onRecoverableError = root.onRecoverableError;
    for (let i = 0; i < recoverableErrors.length; i++) {
      const recoverableError = recoverableErrors[i];
      const errorInfo = makeErrorInfo(
        recoverableError.digest,
        recoverableError.stack,
      );
      onRecoverableError(recoverableError.value, errorInfo);
    }
  }

  if (hasUncaughtError) {
    hasUncaughtError = false;
    const error = firstUncaughtError;
    firstUncaughtError = null;
    throw error;
  }

  // If the passive effects are the result of a discrete render, flush them
  // synchronously at the end of the current task so that the result is
  // immediately observable. Otherwise, we assume that they are not
  // order-dependent and do not need to be observed by external systems, so we
  // can wait until after paint.
  // TODO: We can optimize this by not scheduling the callback earlier. Since we
  // currently schedule the callback in multiple places, will wait until those
  // are consolidated.
  if (includesSyncLane(pendingPassiveEffectsLanes) && root.tag !== LegacyRoot) {
    flushPassiveEffects();
  }

  // Read this again, since a passive effect might have updated it
  remainingLanes = root.pendingLanes;
  if (includesSyncLane(remainingLanes)) {
    if (enableProfilerTimer && enableProfilerNestedUpdatePhase) {
      markNestedUpdateScheduled();
    }

    // Count the number of times the root synchronously re-renders without
    // finishing. If there are too many, it indicates an infinite update loop.
    if (root === rootWithNestedUpdates) {
      nestedUpdateCount++;
    } else {
      nestedUpdateCount = 0;
      rootWithNestedUpdates = root;
    }
  } else {
    nestedUpdateCount = 0;
  }

  // If layout work was scheduled, flush it now.
  flushSyncWorkOnAllRoots();

  if (enableSchedulingProfiler) {
    markCommitStopped();
  }

  if (enableTransitionTracing) {
    // We process transitions during passive effects. However, passive effects can be
    // processed synchronously during the commit phase as well as asynchronously after
    // paint. At the end of the commit phase, we schedule a callback that will be called
    // after the next paint. If the transitions have already been processed (passive
    // effect phase happened synchronously), we will schedule a callback to process
    // the transitions. However, if we don't have any pending transition callbacks, this
    // means that the transitions have yet to be processed (passive effects processed after paint)
    // so we will store the end time of paint so that we can process the transitions
    // and then call the callback via the correct end time.
    const prevRootTransitionCallbacks = root.transitionCallbacks;
    if (prevRootTransitionCallbacks !== null) {
      schedulePostPaintCallback(endTime => {
        const prevPendingTransitionCallbacks =
          currentPendingTransitionCallbacks;
        if (prevPendingTransitionCallbacks !== null) {
          currentPendingTransitionCallbacks = null;
          scheduleCallback(IdleSchedulerPriority, () => {
            processTransitionCallbacks(
              prevPendingTransitionCallbacks,
              endTime,
              prevRootTransitionCallbacks,
            );
          });
        } else {
          currentEndTime = endTime;
        }
      });
    }
  }

  return null;
}
```

提交阶段被分成几个子阶段：

  * 第一阶段，“突变前”阶段。

  * 第二阶段，突变阶段。

  * 第三阶段，布局阶段。


## 突变前阶段

突变前阶段对整个 fiber 树进行深度优先遍历，对需要进行更新的 fiber 根据组件类型进行不同的处理：

这个阶段就做一些突变前的准备工作：

* 调用函数式组件的部分hooks

* 调用类组件的 `getSnapshotBeforeUpdate` 生命周期方法

* 清理 `containerInfo`

## 突变阶段

突变阶段是递归进行的，采用深度优先的规则。

删除 fiber 的突变操作是父级优先的，即，每一次递归都会先删除父级的 `deletions` 中待删除的 fiber。

其余的突变操作是子级优先的。

在此阶段会正式修改 dom 结构。

先执行删除操作，然后是插入和排序，再进行修改操作。

* 类组件的 `componentWillUnmount` 在这里调用

[详解](./commitMutationEffects.md)

**在突变阶段执行完成后，会将 `finishedWork` 变成 `current` 树。**

```js
root.current = finishedWork;
```

## 布局阶段

React 在提交的布局阶段采用递归的方式工作，并且采用深度优先的策略。

在此阶段，在每个 fiber 上的工作始终会在其子 fiber 的工作完成够才被执行。也就是说，会首先完成 child，然后再进行 parent 的操作。

例如类组件，会先调用子组件的 `componentDidMount`，后调用父组件的 `componentDidMount`。

[详解](./commitLayoutEffects.md)