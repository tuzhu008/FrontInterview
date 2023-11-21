# commitMutationEffects

突变阶段是递归进行的，采用深度优先的规则。

删除 fiber 的突变操作是父级优先的，即，每一次递归都会先删除父级的 `deletions` 中待删除的 fiber。

其余的突变操作是子级优先的。

在此阶段会正式修改 dom 结构。

先执行删除操作，然后是插入和排序，再进行修改操作。

在调用 `commitDeletionEffects` 时，类组件的 `componentWillUnmount` 以及函数组件的hook清理函数在这里调用。删除操作[详见](./commitDeletionEffects.md)

```js
function commitMutationEffects(
  root: FiberRoot,
  finishedWork: Fiber,
  committedLanes: Lanes,
) {
  inProgressLanes = committedLanes;
  inProgressRoot = root;

  commitMutationEffectsOnFiber(finishedWork, root, committedLanes);

  inProgressLanes = null;
  inProgressRoot = null;
}

// 递归地遍历突变
function recursivelyTraverseMutationEffects(
  root: FiberRoot,
  parentFiber: Fiber,
  lanes: Lanes,
) {
  // Deletions effects can be scheduled on any fiber type. They need to happen
  // before the children effects hae fired.
  // 删除效果可以安排在任何光纤类型上。它们需要在子节点副作用发生之前发生。
  const deletions = parentFiber.deletions;
  if (deletions !== null) {
    for (let i = 0; i < deletions.length; i++) {
      const childToDelete = deletions[i];
      try {
        commitDeletionEffects(root, parentFiber, childToDelete);
      } catch (error) {
        captureCommitPhaseError(childToDelete, parentFiber, error);
      }
    }
  }

  if (parentFiber.subtreeFlags & MutationMask) {
    let child = parentFiber.child;
    while (child !== null) {
      commitMutationEffectsOnFiber(child, root, lanes);
      child = child.sibling;
    }
  }
}

// 提交 fiber 上的突变副作用
function commitMutationEffectsOnFiber(
  finishedWork: Fiber,
  root: FiberRoot,
  lanes: Lanes,
) {
  const current = finishedWork.alternate;
  const flags = finishedWork.flags;

  // The effect flag should be checked *after* we refine the type of fiber,
  // because the fiber tag is more specific. An exception is any flag related
  // to reconciliation, because those can be set on all fiber types.

  // 效果标志应该在我们细化纤维类型后检查，因为纤维标签更具体。
  // 任何与协调相关的标志都是例外，因为它们可以在所有光纤类型上设置。

  switch (finishedWork.tag) {
    case FunctionComponent:
    case ForwardRef:
    case MemoComponent:
    case SimpleMemoComponent: {
      recursivelyTraverseMutationEffects(root, finishedWork, lanes);
      commitReconciliationEffects(finishedWork);

      if (flags & Update) {
        try {
          // 注意 HookInsertion | HookHasEffect，
          commitHookEffectListUnmount(
            HookInsertion | HookHasEffect,
            finishedWork,
            finishedWork.return,
          );
          commitHookEffectListMount(
            HookInsertion | HookHasEffect,
            finishedWork,
          );
        } catch (error) {
          captureCommitPhaseError(finishedWork, finishedWork.return, error);
        }
        // Layout effects are destroyed during the mutation phase so that all
        // destroy functions for all fibers are called before any create functions.
        // This prevents sibling component effects from interfering with each other,
        // e.g. a destroy function in one component should never override a ref set
        // by a create function in another component during the same commit.
        try {
          // 注意 HookLayout | HookHasEffect，
          commitHookEffectListUnmount(
            HookLayout | HookHasEffect,
            finishedWork,
            finishedWork.return,
          );
        } catch (error) {
          captureCommitPhaseError(finishedWork, finishedWork.return, error);
        }
      }
      return;
    }
    case ClassComponent: {
      recursivelyTraverseMutationEffects(root, finishedWork, lanes);
      commitReconciliationEffects(finishedWork);

      if (flags & Ref) {
        if (current !== null) {
          safelyDetachRef(current, current.return);
        }
      }

      if (flags & Callback && offscreenSubtreeIsHidden) {
        const updateQueue: UpdateQueue<mixed> | null =
          (finishedWork.updateQueue: any);
        if (updateQueue !== null) {
          deferHiddenCallbacks(updateQueue);
        }
      }
      return;
    }
    case HostHoistable: {
      if (enableFloat && supportsResources) {
        // We cast because we always set the root at the React root and so it cannot be
        // null while we are processing mutation effects
        const hoistableRoot: HoistableRoot = (currentHoistableRoot: any);
        recursivelyTraverseMutationEffects(root, finishedWork, lanes);
        commitReconciliationEffects(finishedWork);

        if (flags & Ref) {
          if (current !== null) {
            safelyDetachRef(current, current.return);
          }
        }

        if (flags & Update) {
          const currentResource =
            current !== null ? current.memoizedState : null;
          const newResource = finishedWork.memoizedState;
          if (current === null) {
            // We are mounting a new HostHoistable Fiber. We fork the mount
            // behavior based on whether this instance is a Hoistable Instance
            // or a Hoistable Resource
            if (newResource === null) {
              if (finishedWork.stateNode === null) {
                finishedWork.stateNode = hydrateHoistable(
                  hoistableRoot,
                  finishedWork.type,
                  finishedWork.memoizedProps,
                  finishedWork,
                );
              } else {
                mountHoistable(
                  hoistableRoot,
                  finishedWork.type,
                  finishedWork.stateNode,
                );
              }
            } else {
              finishedWork.stateNode = acquireResource(
                hoistableRoot,
                newResource,
                finishedWork.memoizedProps,
              );
            }
          } else if (currentResource !== newResource) {
            // We are moving to or from Hoistable Resource, or between different Hoistable Resources
            if (currentResource === null) {
              if (current.stateNode !== null) {
                unmountHoistable(current.stateNode);
              }
            } else {
              releaseResource(currentResource);
            }
            if (newResource === null) {
              mountHoistable(
                hoistableRoot,
                finishedWork.type,
                finishedWork.stateNode,
              );
            } else {
              acquireResource(
                hoistableRoot,
                newResource,
                finishedWork.memoizedProps,
              );
            }
          } else if (newResource === null && finishedWork.stateNode !== null) {
            // We may have an update on a Hoistable element
            const updatePayload: null | UpdatePayload =
              (finishedWork.updateQueue: any);
            finishedWork.updateQueue = null;
            if (updatePayload !== null) {
              try {
                commitUpdate(
                  finishedWork.stateNode,
                  updatePayload,
                  finishedWork.type,
                  current.memoizedProps,
                  finishedWork.memoizedProps,
                  finishedWork,
                );
              } catch (error) {
                captureCommitPhaseError(
                  finishedWork,
                  finishedWork.return,
                  error,
                );
              }
            }
          }
        }
        return;
      }
      // Fall through
    }
    case HostSingleton: {
      if (enableHostSingletons && supportsSingletons) {
        if (flags & Update) {
          const previousWork = finishedWork.alternate;
          if (previousWork === null) {
            const singleton = finishedWork.stateNode;
            const props = finishedWork.memoizedProps;
            // This was a new mount, we need to clear and set initial properties
            clearSingleton(singleton);
            acquireSingletonInstance(
              finishedWork.type,
              props,
              singleton,
              finishedWork,
            );
          }
        }
      }
      // Fall through
    }
    case HostComponent: {
      recursivelyTraverseMutationEffects(root, finishedWork, lanes);
      commitReconciliationEffects(finishedWork);

      if (flags & Ref) {
        if (current !== null) {
          safelyDetachRef(current, current.return);
        }
      }
      if (supportsMutation) {
        // TODO: ContentReset gets cleared by the children during the commit
        // phase. This is a refactor hazard because it means we must read
        // flags the flags after `commitReconciliationEffects` has already run;
        // the order matters. We should refactor so that ContentReset does not
        // rely on mutating the flag during commit. Like by setting a flag
        // during the render phase instead.
        if (finishedWork.flags & ContentReset) {
          const instance: Instance = finishedWork.stateNode;
          try {
            resetTextContent(instance);
          } catch (error) {
            captureCommitPhaseError(finishedWork, finishedWork.return, error);
          }
        }

        if (flags & Update) {
          const instance: Instance = finishedWork.stateNode;
          if (instance != null) {
            // Commit the work prepared earlier.
            const newProps = finishedWork.memoizedProps;
            // For hydration we reuse the update path but we treat the oldProps
            // as the newProps. The updatePayload will contain the real change in
            // this case.
            const oldProps =
              current !== null ? current.memoizedProps : newProps;
            const type = finishedWork.type;
            // TODO: Type the updateQueue to be specific to host components.
            const updatePayload: null | UpdatePayload =
              (finishedWork.updateQueue: any);
            finishedWork.updateQueue = null;
            if (updatePayload !== null || diffInCommitPhase) {
              try {
                commitUpdate(
                  instance,
                  updatePayload,
                  type,
                  oldProps,
                  newProps,
                  finishedWork,
                );
              } catch (error) {
                captureCommitPhaseError(
                  finishedWork,
                  finishedWork.return,
                  error,
                );
              }
            }
          }
        }
      }
      return;
    }
    case HostText: {
      recursivelyTraverseMutationEffects(root, finishedWork, lanes);
      commitReconciliationEffects(finishedWork);

      if (flags & Update) {
        if (supportsMutation) {
          if (finishedWork.stateNode === null) {
            throw new Error(
              'This should have a text node initialized. This error is likely ' +
                'caused by a bug in React. Please file an issue.',
            );
          }

          const textInstance: TextInstance = finishedWork.stateNode;
          const newText: string = finishedWork.memoizedProps;
          // For hydration we reuse the update path but we treat the oldProps
          // as the newProps. The updatePayload will contain the real change in
          // this case.
          const oldText: string =
            current !== null ? current.memoizedProps : newText;

          try {
            commitTextUpdate(textInstance, oldText, newText);
          } catch (error) {
            captureCommitPhaseError(finishedWork, finishedWork.return, error);
          }
        }
      }
      return;
    }
    case HostRoot: {
      if (enableFloat && supportsResources) {
        prepareToCommitHoistables();

        const previousHoistableRoot = currentHoistableRoot;
        currentHoistableRoot = getHoistableRoot(root.containerInfo);

        recursivelyTraverseMutationEffects(root, finishedWork, lanes);
        currentHoistableRoot = previousHoistableRoot;

        commitReconciliationEffects(finishedWork);
      } else {
        recursivelyTraverseMutationEffects(root, finishedWork, lanes);
        commitReconciliationEffects(finishedWork);
      }

      if (flags & Update) {
        if (supportsMutation && supportsHydration) {
          if (current !== null) {
            const prevRootState: RootState = current.memoizedState;
            if (prevRootState.isDehydrated) {
              try {
                commitHydratedContainer(root.containerInfo);
              } catch (error) {
                captureCommitPhaseError(
                  finishedWork,
                  finishedWork.return,
                  error,
                );
              }
            }
          }
        }
        if (supportsPersistence) {
          const containerInfo = root.containerInfo;
          const pendingChildren = root.pendingChildren;
          try {
            replaceContainerChildren(containerInfo, pendingChildren);
          } catch (error) {
            captureCommitPhaseError(finishedWork, finishedWork.return, error);
          }
        }
      }
      return;
    }
    case HostPortal: {
      if (enableFloat && supportsResources) {
        const previousHoistableRoot = currentHoistableRoot;
        currentHoistableRoot = getHoistableRoot(
          finishedWork.stateNode.containerInfo,
        );
        recursivelyTraverseMutationEffects(root, finishedWork, lanes);
        commitReconciliationEffects(finishedWork);
        currentHoistableRoot = previousHoistableRoot;
      } else {
        recursivelyTraverseMutationEffects(root, finishedWork, lanes);
        commitReconciliationEffects(finishedWork);
      }

      if (flags & Update) {
        if (supportsPersistence) {
          const portal = finishedWork.stateNode;
          const containerInfo = portal.containerInfo;
          const pendingChildren = portal.pendingChildren;
          try {
            replaceContainerChildren(containerInfo, pendingChildren);
          } catch (error) {
            captureCommitPhaseError(finishedWork, finishedWork.return, error);
          }
        }
      }
      return;
    }
    case SuspenseComponent: {
      recursivelyTraverseMutationEffects(root, finishedWork, lanes);
      commitReconciliationEffects(finishedWork);

      // TODO: We should mark a flag on the Suspense fiber itself, rather than
      // relying on the Offscreen fiber having a flag also being marked. The
      // reason is that this offscreen fiber might not be part of the work-in-
      // progress tree! It could have been reused from a previous render. This
      // doesn't lead to incorrect behavior because we don't rely on the flag
      // check alone; we also compare the states explicitly below. But for
      // modeling purposes, we _should_ be able to rely on the flag check alone.
      // So this is a bit fragile.
      //
      // Also, all this logic could/should move to the passive phase so it
      // doesn't block paint.
      const offscreenFiber: Fiber = (finishedWork.child: any);
      if (offscreenFiber.flags & Visibility) {
        // Throttle the appearance and disappearance of Suspense fallbacks.
        const isShowingFallback =
          (finishedWork.memoizedState: SuspenseState | null) !== null;
        const wasShowingFallback =
          current !== null &&
          (current.memoizedState: SuspenseState | null) !== null;

        if (alwaysThrottleRetries) {
          if (isShowingFallback !== wasShowingFallback) {
            // A fallback is either appearing or disappearing.
            markCommitTimeOfFallback();
          }
        } else {
          if (isShowingFallback && !wasShowingFallback) {
            // Old behavior. Only mark when a fallback appears, not when
            // it disappears.
            markCommitTimeOfFallback();
          }
        }
      }

      if (flags & Update) {
        try {
          commitSuspenseCallback(finishedWork);
        } catch (error) {
          captureCommitPhaseError(finishedWork, finishedWork.return, error);
        }
        const retryQueue: RetryQueue | null = (finishedWork.updateQueue: any);
        if (retryQueue !== null) {
          finishedWork.updateQueue = null;
          attachSuspenseRetryListeners(finishedWork, retryQueue);
        }
      }
      return;
    }
    case OffscreenComponent: {
      if (flags & Ref) {
        if (current !== null) {
          safelyDetachRef(current, current.return);
        }
      }

      const newState: OffscreenState | null = finishedWork.memoizedState;
      const isHidden = newState !== null;
      const wasHidden = current !== null && current.memoizedState !== null;

      if (finishedWork.mode & ConcurrentMode) {
        // Before committing the children, track on the stack whether this
        // offscreen subtree was already hidden, so that we don't unmount the
        // effects again.
        const prevOffscreenSubtreeIsHidden = offscreenSubtreeIsHidden;
        const prevOffscreenSubtreeWasHidden = offscreenSubtreeWasHidden;
        offscreenSubtreeIsHidden = prevOffscreenSubtreeIsHidden || isHidden;
        offscreenSubtreeWasHidden = prevOffscreenSubtreeWasHidden || wasHidden;
        recursivelyTraverseMutationEffects(root, finishedWork, lanes);
        offscreenSubtreeWasHidden = prevOffscreenSubtreeWasHidden;
        offscreenSubtreeIsHidden = prevOffscreenSubtreeIsHidden;
      } else {
        recursivelyTraverseMutationEffects(root, finishedWork, lanes);
      }

      commitReconciliationEffects(finishedWork);

      const offscreenInstance: OffscreenInstance = finishedWork.stateNode;

      // TODO: Add explicit effect flag to set _current.
      offscreenInstance._current = finishedWork;

      // Offscreen stores pending changes to visibility in `_pendingVisibility`. This is
      // to support batching of `attach` and `detach` calls.
      offscreenInstance._visibility &= ~OffscreenDetached;
      offscreenInstance._visibility |=
        offscreenInstance._pendingVisibility & OffscreenDetached;

      if (flags & Visibility) {
        // Track the current state on the Offscreen instance so we can
        // read it during an event
        if (isHidden) {
          offscreenInstance._visibility &= ~OffscreenVisible;
        } else {
          offscreenInstance._visibility |= OffscreenVisible;
        }

        if (isHidden) {
          const isUpdate = current !== null;
          const wasHiddenByAncestorOffscreen =
            offscreenSubtreeIsHidden || offscreenSubtreeWasHidden;
          // Only trigger disapper layout effects if:
          //   - This is an update, not first mount.
          //   - This Offscreen was not hidden before.
          //   - Ancestor Offscreen was not hidden in previous commit.
          if (isUpdate && !wasHidden && !wasHiddenByAncestorOffscreen) {
            if ((finishedWork.mode & ConcurrentMode) !== NoMode) {
              // Disappear the layout effects of all the children
              recursivelyTraverseDisappearLayoutEffects(finishedWork);
            }
          }
        } else {
          if (wasHidden) {
            // TODO: Move re-appear call here for symmetry?
          }
        }

        // Offscreen with manual mode manages visibility manually.
        if (supportsMutation && !isOffscreenManual(finishedWork)) {
          // TODO: This needs to run whenever there's an insertion or update
          // inside a hidden Offscreen tree.
          hideOrUnhideAllChildren(finishedWork, isHidden);
        }
      }

      // TODO: Move to passive phase
      if (flags & Update) {
        const offscreenQueue: OffscreenQueue | null =
          (finishedWork.updateQueue: any);
        if (offscreenQueue !== null) {
          const retryQueue = offscreenQueue.retryQueue;
          if (retryQueue !== null) {
            offscreenQueue.retryQueue = null;
            attachSuspenseRetryListeners(finishedWork, retryQueue);
          }
        }
      }
      return;
    }
    case SuspenseListComponent: {
      recursivelyTraverseMutationEffects(root, finishedWork, lanes);
      commitReconciliationEffects(finishedWork);

      if (flags & Update) {
        const retryQueue: Set<Wakeable> | null =
          (finishedWork.updateQueue: any);
        if (retryQueue !== null) {
          finishedWork.updateQueue = null;
          attachSuspenseRetryListeners(finishedWork, retryQueue);
        }
      }
      return;
    }
    case ScopeComponent: {
      if (enableScopeAPI) {
        recursivelyTraverseMutationEffects(root, finishedWork, lanes);
        commitReconciliationEffects(finishedWork);

        // TODO: This is a temporary solution that allowed us to transition away
        // from React Flare on www.
        if (flags & Ref) {
          if (current !== null) {
            safelyDetachRef(finishedWork, finishedWork.return);
          }
          safelyAttachRef(finishedWork, finishedWork.return);
        }
        if (flags & Update) {
          const scopeInstance = finishedWork.stateNode;
          prepareScopeUpdate(scopeInstance, finishedWork);
        }
      }
      return;
    }
    default: {
      recursivelyTraverseMutationEffects(root, finishedWork, lanes);
      commitReconciliationEffects(finishedWork);

      return;
    }
  }
}
```

## commitReconciliationEffects

在每个 fiber 上调用，以处理插入和排序操作。

[详解](./commitReconciliationEffects.md)

## 类组件

类组件在此过程中没有特别的工作要做。

## 函数组件

函数式组件在突变阶段会顺序调用 `commitHookEffectListUnmount`、`commitHookEffectListMount` 来处理 [`HookInsertion`](/react/core/hooks.md) 类型的 Hooks，先卸载，然后马上挂载。

* useInsertionEffect

并且会在进行下一个阶段（布局阶段）前调用 `commitHookEffectListUnmount` 对 `HookLayout` 类型的 hooks 执行卸载操作。

* useLayoutEffect

* useImperativeHandle

```js
// 注意 HookInsertion | HookHasEffect，
  commitHookEffectListUnmount(
    HookInsertion | HookHasEffect,
    finishedWork,
    finishedWork.return,
  );
  commitHookEffectListMount(
    HookInsertion | HookHasEffect,
    finishedWork,
  );

function commitHookEffectListUnmount(
  flags: HookFlags,
  finishedWork: Fiber,
  nearestMountedAncestor: Fiber | null,
) {
  const updateQueue: FunctionComponentUpdateQueue | null =
    (finishedWork.updateQueue: any);
  const lastEffect = updateQueue !== null ? updateQueue.lastEffect : null;
  if (lastEffect !== null) {
    const firstEffect = lastEffect.next;
    let effect = firstEffect;
    do {
      if ((effect.tag & flags) === flags) {
        // Unmount
        const inst = effect.inst;
        const destroy = inst.destroy;
        if (destroy !== undefined) {
          inst.destroy = undefined;
          safelyCallDestroy(finishedWork, nearestMountedAncestor, destroy);
        }
      }
      effect = effect.next;
    } while (effect !== firstEffect);
  }
}

function commitHookEffectListMount(flags: HookFlags, finishedWork: Fiber) {
  const updateQueue: FunctionComponentUpdateQueue | null =
    (finishedWork.updateQueue: any);
  const lastEffect = updateQueue !== null ? updateQueue.lastEffect : null;
  if (lastEffect !== null) {
    const firstEffect = lastEffect.next;
    let effect = firstEffect;
    do {
      if ((effect.tag & flags) === flags) {
        // Mount
        const create = effect.create;
       
        const inst = effect.inst;
        const destroy = create();
        inst.destroy = destroy;

      }
      effect = effect.next;
    } while (effect !== firstEffect);
  }
}
```

* 首先对待删除的 fiber 列表进行循环删除操作，在此会调用其销毁的生命周期方法，如：`componentWillUnmount`


## HostComponent

```js
function commitUpdate(
  domElement: Instance,
  updatePayload: any,
  type: string,
  oldProps: Props,
  newProps: Props,
  internalInstanceHandle: Object,
): void {
  if (diffInCommitPhase) {
    // Diff and update the properties.
    updateProperties(domElement, type, oldProps, newProps);
  } else {
    // Apply the diff to the DOM node.
    updatePropertiesWithDiff(
      domElement,
      updatePayload,
      type,
      oldProps,
      newProps,
    );
  }

  // Update the props handle so that we know which props are the ones with
  // with current event handlers.
  updateFiberProps(domElement, newProps);
}
```


## HostText

```js
function commitTextUpdate(
  textInstance: TextInstance,
  oldText: string,
  newText: string,
): void {
  textInstance.nodeValue = newText;
}
```

## HostRoot

```js
function commitHydratedContainer(container: Container): void {
  // Retry if any event replaying was blocked on this.
  retryIfBlockedOn(container);
}

function retryIfBlockedOn(
  unblocked: Container | SuspenseInstance,
): void {
  if (queuedFocus !== null) {
    scheduleCallbackIfUnblocked(queuedFocus, unblocked);
  }
  if (queuedDrag !== null) {
    scheduleCallbackIfUnblocked(queuedDrag, unblocked);
  }
  if (queuedMouse !== null) {
    scheduleCallbackIfUnblocked(queuedMouse, unblocked);
  }
  const unblock = (queuedEvent: QueuedReplayableEvent) =>
    scheduleCallbackIfUnblocked(queuedEvent, unblocked);
  queuedPointers.forEach(unblock);
  queuedPointerCaptures.forEach(unblock);

  for (let i = 0; i < queuedExplicitHydrationTargets.length; i++) {
    const queuedTarget = queuedExplicitHydrationTargets[i];
    if (queuedTarget.blockedOn === unblocked) {
      queuedTarget.blockedOn = null;
    }
  }

  while (queuedExplicitHydrationTargets.length > 0) {
    const nextExplicitTarget = queuedExplicitHydrationTargets[0];
    if (nextExplicitTarget.blockedOn !== null) {
      // We're still blocked.
      break;
    } else {
      attemptExplicitHydrationTarget(nextExplicitTarget);
      if (nextExplicitTarget.blockedOn === null) {
        // We're unblocked.
        queuedExplicitHydrationTargets.shift();
      }
    }
  }

  if (enableFormActions) {
    // Check the document if there are any queued form actions.
    const root = unblocked.getRootNode();
    const formReplayingQueue: void | FormReplayingQueue = (root: any)
      .$$reactFormReplay;
    if (formReplayingQueue != null) {
      for (let i = 0; i < formReplayingQueue.length; i += 3) {
        const form: HTMLFormElement = formReplayingQueue[i];
        const submitterOrAction:
          | null
          | HTMLInputElement
          | HTMLButtonElement
          | FormAction = formReplayingQueue[i + 1];
        const formProps = getFiberCurrentPropsFromNode(form);
        if (typeof submitterOrAction === 'function') {
          // This action has already resolved. We're just waiting to dispatch it.
          if (!formProps) {
            // This was not part of this React instance. It might have been recently
            // unblocking us from dispatching our events. So let's make sure we schedule
            // a retry.
            scheduleReplayQueueIfNeeded(formReplayingQueue);
          }
          continue;
        }
        let target: Node = form;
        if (formProps) {
          // This form belongs to this React instance but the submitter might
          // not be done yet.
          let action: null | FormAction = null;
          const submitter = submitterOrAction;
          if (submitter && submitter.hasAttribute('formAction')) {
            // The submitter is the one that is responsible for the action.
            target = submitter;
            const submitterProps = getFiberCurrentPropsFromNode(submitter);
            if (submitterProps) {
              // The submitter is part of this instance.
              action = (submitterProps: any).formAction;
            } else {
              const blockedOn = findInstanceBlockingTarget(target);
              if (blockedOn !== null) {
                // The submitter is not hydrated yet. We'll wait for it.
                continue;
              }
              // The submitter must have been a part of a different React instance.
              // Except the form isn't. We don't dispatch actions in this scenario.
            }
          } else {
            action = (formProps: any).action;
          }
          if (typeof action === 'function') {
            formReplayingQueue[i + 1] = action;
          } else {
            // Something went wrong so let's just delete this action.
            formReplayingQueue.splice(i, 3);
            i -= 3;
          }
          // Schedule a replay in case this unblocked something.
          scheduleReplayQueueIfNeeded(formReplayingQueue);
          continue;
        }
        // Something above this target is still blocked so we can't continue yet.
        // We're not sure if this target is actually part of this React instance
        // yet. It could be a different React as a child but at least some parent is.
        // We must continue for any further queued actions.
      }
    }
  }
}
```