

React 基于浏览器的事件机制实现了一套自身的事件机制，它符合 W3C 规范，包括事件触发、事件冒泡、事件捕获、事件合成和事件派发等。

React 事件被称为合成事件（SyntheticEvent），它是浏览器原生事件的包装器。

它将事件派发器绑定在根容器中，实现了事件的委托优化，并且实现了自定义的事件流。

`SyntheticEvent` 实例将被传递给你的事件处理函数，它是浏览器的原生事件的跨浏览器包装器。除兼容所有浏览器外，它还拥有和浏览器原生事件相同的接口。

`SyntheticEvent` 实例中通过 `nativeEvent` 保存了浏览器的底层事件。

合成事件与浏览器的原生事件不同，不会直接映射到原生事件。

## 设计动机

- 兼容性

  在底层磨平不同浏览器的差异，React 实现了统一的事件机制，我们不再需要处理浏览器事件机制方面的兼容问题，在上层面向开发者暴露稳定、统一的、与原生事件相同的事件接口

- 性能优化

  使用委托的方式，减少了内存的开销。

- 中心化管控、事件池

  在 17 以前，合成事件会被放入事件池，以避免事件被垃圾回收。

  通过事件池，React 把握了事件机制的主动权，实现了对所有事件的中心化管控。

  在事件池中获取或释放事件对象，避免频繁的创建和销毁。但从 17 开始，web 端 React 不再事件池。

## 原生事件和合成事件

React 元素的事件处理和 DOM 元素的很相似，但是有一点语法上的不同：

- React 事件的命名采用小驼峰式（camelCase），而不是纯小写。
- 使用 JSX 语法时你需要传入一个函数作为事件处理函数，而不是一个字符串。
- 在 React 中另一个不同点是你不能通过返回 `false` 的方式阻止默认行为。必须显式地使用 preventDefault。而传统的 HTML 中阻止表单的默认提交行为

## 事件系统

```js
/**
 * React和事件系统概述:
 *
 * +------------+    .
 * |    DOM     |    .
 * +------------+    .
 *       |           .
 *       v           .
 * +------------+    .
 * | ReactEvent |    .
 * |  Listener  |    .
 * +------------+    .                         +-----------+
 *       |           .               +--------+|SimpleEvent|
 *       |           .               |         |Plugin     |
 * +-----|------+    .               v         +-----------+
 * |     |      |    .    +--------------+                    +------------+
 * |     +-----------.--->|EventPluginHub|                    |    Event   |
 * |            |    .    |              |     +-----------+  | Propagators|
 * | ReactEvent |    .    |              |     |TapEvent   |  |------------|
 * |  Emitter   |    .    |              |<---+|Plugin     |  |other plugin|
 * |            |    .    |              |     +-----------+  |  utilities |
 * |     +-----------.--->|              |                    +------------+
 * |     |      |    .    +--------------+
 * +-----|------+    .                ^        +-----------+
 *       |           .                |        |Enter/Leave|
 *       +           .                +-------+|Plugin     |
 * +-------------+   .                         +-----------+
 * | application |   .
 * |-------------|   .
 * |             |   .
 * |             |   .
 * +-------------+   .
 *                   .
 */
```

事件系统由以下几部分构成：

* ReactEventListener：负责事件的注册。
* ReactEventEmitter：负责事件的分发。
*  EventPluginHub：负责事件的存储及分发。
*  Plugin：根据不同的事件类型构造不同的合成事件。

## 设计

整个事件系统是基于事件的插件系统进行构建的，插件系统提供了不同类型事件的注册、提取、合成、处理都是在此处完成的。每个插件则提供了对事件的注册、提取、合成功能。


## 事件监听
### 事件注册

在 react 代码加载执行阶段，其会直接注册相关事件，但此时的注册并不是绑定到 dom 元素上，而是将这一系列的事件加入到相关的缓存变量中：

```js
SimpleEventPlugin.registerEvents();
EnterLeaveEventPlugin.registerEvents();
ChangeEventPlugin.registerEvents();
SelectEventPlugin.registerEvents();
BeforeInputEventPlugin.registerEvents();
```

相关缓存
```js
// dom 原生事件名到 react 事件的名称映射
export const topLevelEventsToReactNames: Map<DOMEventName, string | null> =
  new Map();

// 所有原生事件的集合
export const allNativeEvents: Set<DOMEventName> = new Set();
```

### 事件绑定

事件绑定分为两种，一种是简单事件，一种是不可委托事件。不可委托事件是简单事件的子集。

不可委托事件的冒泡阶段时间不能被委托，捕获可以委托。

* 绑定所有支持的事件

  在应用初始化阶段，react 会简单事件委托绑定到外部的提供的挂载容器上。

  ```js
  function createRoot() {

    ...
    listenToAllSupportedEvents(rootContainerElement);
    ...
  }
  ```

  绑定事件时，所有简单的事件的捕获阶段都会被注册，而冒泡阶段则会排除掉不能被委托的元素。

  ```js
  function listenToAllSupportedEvents(rootContainerElement: EventTarget) {
    if (!(rootContainerElement: any)[listeningMarker]) {
      (rootContainerElement: any)[listeningMarker] = true;
      allNativeEvents.forEach(domEventName => {
        // We handle selectionchange separately because it
        // doesn't bubble and needs to be on the document.
        if (domEventName !== 'selectionchange') {
          if (!nonDelegatedEvents.has(domEventName)) {
            listenToNativeEvent(domEventName, false, rootContainerElement);
          }
          listenToNativeEvent(domEventName, true, rootContainerElement);
        }
      });
      // ...
    }
  }
  ```

* 绑定不可委托的事件

  不可委托的事件会被绑定到相应的 dom 元素上。

### 事件分发

绑定到容器的事件监听器函数，都是对 `dispatchEvent` 的包装，如事件名称等。

因此，当事件被触发时，最终都会调用 `dispatchEvent` 进行事件的分发。

### 事件分类

* 简单事件

  简单事件会被委托到容器元素上

  ```js
  'abort',
  'auxClick',
  'cancel',
  'canPlay',
  'canPlayThrough',
  'click',
  'close',
  'contextMenu',
  'copy',
  'cut',
  'drag',
  'dragEnd',
  'dragEnter',
  'dragExit',
  'dragLeave',
  'dragOver',
  'dragStart',
  'drop',
  'durationChange',
  'emptied',
  'encrypted',
  'ended',
  'error',
  'gotPointerCapture',
  'input',
  'invalid',
  'keyDown',
  'keyPress',
  'keyUp',
  'load',
  'loadedData',
  'loadedMetadata',
  'loadStart',
  'lostPointerCapture',
  'mouseDown',
  'mouseMove',
  'mouseOut',
  'mouseOver',
  'mouseUp',
  'paste',
  'pause',
  'play',
  'playing',
  'pointerCancel',
  'pointerDown',
  'pointerMove',
  'pointerOut',
  'pointerOver',
  'pointerUp',
  'progress',
  'rateChange',
  'reset',
  'resize',
  'seeked',
  'seeking',
  'stalled',
  'submit',
  'suspend',
  'timeUpdate',
  'touchCancel',
  'touchEnd',
  'touchStart',
  'volumeChange',
  'scroll',
  'toggle',
  'touchMove',
  'waiting',
  'wheel',
  ```
* 媒体事件

  需要单独附加到媒体元素

  ```js
  'abort',
  'canplay',
  'canplaythrough',
  'durationchange',
  'emptied',
  'encrypted',
  'ended',
  'error',
  'loadeddata',
  'loadedmetadata',
  'loadstart',
  'pause',
  'play',
  'playing',
  'progress',
  'ratechange',
  'resize',
  'seeked',
  'seeking',
  'stalled',
  'suspend',
  'timeupdate',
  'volumechange',
  'waiting',
  ```

* 不能委托的事件

  ```js
  'cancel',
  'close',
  'invalid',
  'load',
  'scroll',
  'toggle',
  ...mediaEventTypes
  ```

### 事件绑定流程图

![事件绑定](/assets//react/Listen-Event.png)

## 事件处理

事件的处理是从 `dispatchEvent 开始的`，接下来事件的处理工作就会交由插件事件系统来完成。

所有的事件都会放在批量更新的回调中，用于批量的更新。

```js
function dispatchEventForPluginEventSystemdomEventName: DOMEventName,
  eventSystemFlags: EventSystemFlags,
  nativeEvent: AnyNativeEvent,
  targetInst: null | Fiber,
  targetContainer: EventTarget,
): void {
  // ...

  batchedUpdates(() =>
    dispatchEventsForPlugins(
      domEventName,
      eventSystemFlags,
      nativeEvent,
      ancestorInst,
      targetContainer,
    ),
  );
}
```

接下来， `dispatchEventsForPlugins` 会调用 `extractEvents` 来进行事件队列的提取。这个事件队列，是 react 自定义事件系统的关键。最后 `processDispatchQueue` 会遍历这个事件队列，将所有事件的所有事件监听器函数全部按序执行。

```js
// 使用插件用当前的原生 dom 事件构造出一个事件队列
// 并调用 processDispatchQueue 进行处理

function dispatchEventsForPlugins(
  domEventName: DOMEventName,
  eventSystemFlags: EventSystemFlags,
  nativeEvent: AnyNativeEvent,
  targetInst: null | Fiber,
  targetContainer: EventTarget,
): void {
  const nativeEventTarget = getEventTarget(nativeEvent);
  const dispatchQueue: DispatchQueue = [];
  extractEvents(
    dispatchQueue,
    domEventName,
    targetInst,
    nativeEvent,
    nativeEventTarget,
    eventSystemFlags,
    targetContainer,
  );
  processDispatchQueue(dispatchQueue, eventSystemFlags);
}
```

### 构建事件队列

在事件提取阶段，`extractEvents` 会调用各个事件插件的 `extractEvents` 的方法进行不同事件的提取。事件提取的目标在于，构建一个 `dispatchQueue` 队列。

为了实现不同的行为，这些事件插件间的事件可能有重合。

事件插件会根据 `domEventName` 构造不同的合成事件，以 `SimpleEventPlugin` 为例：

```js
function extractEvents(
  dispatchQueue: DispatchQueue,
  domEventName: DOMEventName,
  targetInst: null | Fiber,
  nativeEvent: AnyNativeEvent,
  nativeEventTarget: null | EventTarget,
  eventSystemFlags: EventSystemFlags,
  targetContainer: EventTarget,
): void {
  const reactName = topLevelEventsToReactNames.get(domEventName);
  if (reactName === undefined) {
    return;
  }
  let SyntheticEventCtor = SyntheticEvent;
  let reactEventType: string = domEventName;
  switch (domEventName) {
    case 'keypress':
      // Firefox creates a keypress event for function keys too. This removes
      // the unwanted keypress events. Enter is however both printable and
      // non-printable. One would expect Tab to be as well (but it isn't).
      // TODO: Fixed in https://bugzilla.mozilla.org/show_bug.cgi?id=968056. Can
      // probably remove.
      if (getEventCharCode(((nativeEvent: any): KeyboardEvent)) === 0) {
        return;
      }
    /* falls through */
    case 'keydown':
    case 'keyup':
      SyntheticEventCtor = SyntheticKeyboardEvent;
      break;
    case 'focusin':
      reactEventType = 'focus';
      SyntheticEventCtor = SyntheticFocusEvent;
      break;
    case 'focusout':
      reactEventType = 'blur';
      SyntheticEventCtor = SyntheticFocusEvent;
      break;
    case 'beforeblur':
    case 'afterblur':
      SyntheticEventCtor = SyntheticFocusEvent;
      break;
    case 'click':
      // Firefox creates a click event on right mouse clicks. This removes the
      // unwanted click events.
      // TODO: Fixed in https://phabricator.services.mozilla.com/D26793. Can
      // probably remove.
      if (nativeEvent.button === 2) {
        return;
      }
    /* falls through */
    case 'auxclick':
    case 'dblclick':
    case 'mousedown':
    case 'mousemove':
    case 'mouseup':
    // TODO: Disabled elements should not respond to mouse events
    /* falls through */
    case 'mouseout':
    case 'mouseover':
    case 'contextmenu':
      SyntheticEventCtor = SyntheticMouseEvent;
      break;
    case 'drag':
    case 'dragend':
    case 'dragenter':
    case 'dragexit':
    case 'dragleave':
    case 'dragover':
    case 'dragstart':
    case 'drop':
      SyntheticEventCtor = SyntheticDragEvent;
      break;
    case 'touchcancel':
    case 'touchend':
    case 'touchmove':
    case 'touchstart':
      SyntheticEventCtor = SyntheticTouchEvent;
      break;
    case ANIMATION_END:
    case ANIMATION_ITERATION:
    case ANIMATION_START:
      SyntheticEventCtor = SyntheticAnimationEvent;
      break;
    case TRANSITION_END:
      SyntheticEventCtor = SyntheticTransitionEvent;
      break;
    case 'scroll':
      SyntheticEventCtor = SyntheticUIEvent;
      break;
    case 'wheel':
      SyntheticEventCtor = SyntheticWheelEvent;
      break;
    case 'copy':
    case 'cut':
    case 'paste':
      SyntheticEventCtor = SyntheticClipboardEvent;
      break;
    case 'gotpointercapture':
    case 'lostpointercapture':
    case 'pointercancel':
    case 'pointerdown':
    case 'pointermove':
    case 'pointerout':
    case 'pointerover':
    case 'pointerup':
      SyntheticEventCtor = SyntheticPointerEvent;
      break;
    default:
      // Unknown event. This is used by createEventHandle.
      break;
  }

  const inCapturePhase = (eventSystemFlags & IS_CAPTURE_PHASE) !== 0;
  if (
    enableCreateEventHandleAPI &&
    eventSystemFlags & IS_EVENT_HANDLE_NON_MANAGED_NODE
  ) {
    const listeners = accumulateEventHandleNonManagedNodeListeners(
      // TODO: this cast may not make sense for events like
      // "focus" where React listens to e.g. "focusin".
      ((reactEventType: any): DOMEventName),
      targetContainer,
      inCapturePhase,
    );
    if (listeners.length > 0) {
      // Intentionally create event lazily.
      const event: ReactSyntheticEvent = new SyntheticEventCtor(
        reactName,
        reactEventType,
        null,
        nativeEvent,
        nativeEventTarget,
      );
      dispatchQueue.push({event, listeners});
    }
  } else {
    // Some events don't bubble in the browser.
    // In the past, React has always bubbled them, but this can be surprising.
    // We're going to try aligning closer to the browser behavior by not bubbling
    // them in React either. We'll start by not bubbling onScroll, and then expand.
    const accumulateTargetOnly =
      !inCapturePhase &&
      // TODO: ideally, we'd eventually add all events from
      // nonDelegatedEvents list in DOMPluginEventSystem.
      // Then we can remove this special list.
      // This is a breaking change that can wait until React 18.
      domEventName === 'scroll';

    const listeners = accumulateSinglePhaseListeners(
      targetInst,
      reactName,
      nativeEvent.type,
      inCapturePhase,
      accumulateTargetOnly,
      nativeEvent,
    );
    if (listeners.length > 0) {
      // Intentionally create event lazily.
      const event: ReactSyntheticEvent = new SyntheticEventCtor(
        reactName,
        reactEventType,
        null,
        nativeEvent,
        nativeEventTarget,
      );
      dispatchQueue.push({event, listeners});
    }
  }
}
```

在提取事件的过程中，最核心的莫过于收集事件监听器。以 `accumulateSinglePhaseListeners` 为例：

```js
// 从 targetFiber 开始往上遍历，直到 fiber 树的根
// 从而构建出一个 listener 队列
export function accumulateSinglePhaseListeners(
  targetFiber: Fiber | null,
  reactName: string | null,
  nativeEventType: string,
  inCapturePhase: boolean,
  accumulateTargetOnly: boolean,
  nativeEvent: AnyNativeEvent,
): Array<DispatchListener> {
  const captureName = reactName !== null ? reactName + 'Capture' : null;
  const reactEventName = inCapturePhase ? captureName : reactName;
  let listeners: Array<DispatchListener> = [];

  let instance = targetFiber;
  let lastHostComponent = null;

  // Accumulate all instances and listeners via the target -> root path.
  while (instance !== null) {
    // Handle listeners that are on HostComponents (i.e. <div>)
    if (
      (tag === HostComponent ||
        (enableFloat ? tag === HostHoistable : false) ||
        (enableHostSingletons ? tag === HostSingleton : false)) &&
      stateNode !== null
    ) {
      lastHostComponent = stateNode;
      // Standard React on* listeners, i.e. onClick or onClickCapture
      if (reactEventName !== null) {
        const listener = getListener(instance, reactEventName);
        if (listener != null) {
          listeners.push(
            createDispatchListener(instance, listener, lastHostComponent),
          );
        }
      }
    }
    instance = instance.return;
  }
  return listeners;
}
```

此处略去很多代码，从剩余代码可以看到，该处从事件的发生节点开始，往上进行树的遍历，最后到达树的根节点。在此过程中，会依次在这条 fiber 链上的所有主机组件（dom 节点）的上使用 `getListener` 获取对应事件的监听器。最终构成了这个监听器的列表。

`getListener` 则很简单，获取当前 fiber 实例的最新 props，然后获取对应的事件属性。

```js
export default function getListener(
  inst: Fiber,
  registrationName: string,
): Function | null {
  const stateNode = inst.stateNode;
  if (stateNode === null) {
    // Work in progress (ex: onload events in incremental mode).
    return null;
  }
  const props = getFiberCurrentPropsFromNode(stateNode);
  if (props === null) {
    // Work in progress.
    return null;
  }
  const listener = props[registrationName];
  if (shouldPreventMouseEvent(registrationName, inst.type, props)) {
    return null;
  }

  if (listener && typeof listener !== 'function') {
    throw new Error(
      `Expected \`${registrationName}\` listener to be a function, instead got a value of \`${typeof listener}\` type.`,
    );
  }

  return listener;
}
```

在获取事件监听器的时候，需要剔除一些需要阻止的鼠标事件：

```js
function isInteractive(tag: string): boolean {
  return (
    tag === 'button' ||
    tag === 'input' ||
    tag === 'select' ||
    tag === 'textarea'
  );
}

function shouldPreventMouseEvent(
  name: string,
  type: string,
  props: Props,
): boolean {
  switch (name) {
    case 'onClick':
    case 'onClickCapture':
    case 'onDoubleClick':
    case 'onDoubleClickCapture':
    case 'onMouseDown':
    case 'onMouseDownCapture':
    case 'onMouseMove':
    case 'onMouseMoveCapture':
    case 'onMouseUp':
    case 'onMouseUpCapture':
    case 'onMouseEnter':
      return !!(props.disabled && isInteractive(type));
    default:
      return false;
  }
}
```

### 处理事件队列

在处理事件队列时，会根据事件所处阶段（捕获和冒泡）对时间队列进行有序（反向遍历和正向遍历）的处理。

```js

// 处理事件队列
// 遍历事件队列，对每个事件调用 processDispatchQueueItemsInOrder 进行处理
export function processDispatchQueue(
  dispatchQueue: DispatchQueue,
  eventSystemFlags: EventSystemFlags,
): void {
  const inCapturePhase = (eventSystemFlags & IS_CAPTURE_PHASE) !== 0;
  for (let i = 0; i < dispatchQueue.length; i++) {
    const {event, listeners} = dispatchQueue[i];
    processDispatchQueueItemsInOrder(event, listeners, inCapturePhase);
    //  event system doesn't use pooling.
  }
  // This would be a good time to rethrow if any of the event handlers threw.
  rethrowCaughtError();
}

// 处理某个事件的监听器列表
// 遍历监听器列表，调用 executeDispatch 处理每个监听器。
// 遍历时根据冒泡或者捕获阶段进行正序还是逆序遍历
function processDispatchQueueItemsInOrder(
  event: ReactSyntheticEvent,
  dispatchListeners: Array<DispatchListener>,
  inCapturePhase: boolean,
): void {
  let previousInstance;
  if (inCapturePhase) {
    for (let i = dispatchListeners.length - 1; i >= 0; i--) {
      const {instance, currentTarget, listener} = dispatchListeners[i];
      if (instance !== previousInstance && event.isPropagationStopped()) {
        return;
      }
      executeDispatch(event, listener, currentTarget);
      previousInstance = instance;
    }
  } else {
    for (let i = 0; i < dispatchListeners.length; i++) {
      const {instance, currentTarget, listener} = dispatchListeners[i];
      if (instance !== previousInstance && event.isPropagationStopped()) {
        return;
      }
      executeDispatch(event, listener, currentTarget);
      previousInstance = instance;
    }
  }
}

function executeDispatch(
  event: ReactSyntheticEvent,
  listener: Function,
  currentTarget: EventTarget,
): void {
  const type = event.type || 'unknown-event';
  event.currentTarget = currentTarget;
  invokeGuardedCallbackAndCatchFirstError(type, listener, undefined, event);
  event.currentTarget = null;
}

```

### 事件处理流程图

![事件处理](/assets//react/Handle-Event.png)

## 事件流

个页面往往会绑定多个事件，页面接收事件的顺序叫事件流。

W3C 标准事件的传播过程：

- 事件捕获
- 处于目标
- 事件冒泡

常用的事件处理性能优化手段：事件委托。把多个子元素同一类型的监听函数合并到父元素上，通过一个函数监听的行为叫事件委托。

### react 合成事件

React16 的事件绑定在 document 上， React17 以后事件绑定在 `container` 上。`ReactDOM.render(app,container)`。

事件绑定 事件触发：

- React 所有的事件绑定在 `container` 上(react17 以后)，而不是绑定在 DOM 元素上（作用：减少内存开销，所有的事件处理都在 container 上，其他节点没有绑定事件）
- React 自身实现了一套冒泡机制，不能通过 `return false` 阻止冒泡
- React 通过 `SytheticEvent` 实现了事件合成

![image.png](/assets/react/event.jpeg)

## question

### 为什么针对同一个事件，即使可能存在多次回调，document（container）也只需要注册一次监听

因为 React 注册到 document(container) 上的并不是一个某个 DOM 节点具体的回调逻辑，而是一个统一的事件分发函数 dispatchEvent - > 事件委托思想

### dispatchEvent 是怎么实现事件分发的

事件触发的本质是对 dispatchEvent 函数的调用

![image.png](/assets/react/event-flow.jpeg)

### React 事件处理为什么要手动绑定 this

react 组件会被编译为 `React.createElement`，在 `createElement` 中，它的 `this` 丢失了，并不是由组件实例调用的，因此需要手动绑定 `this`。

### 为什么不能通过 `return false` 阻止事件的默认行为

因为 React 基于浏览器的事件机制实现了一套自己的事件机制，和原生 DOM 事件不同，它采用了事件委托的思想，通过`dispatch` 统一分发事件处理函数

### React 怎么阻止事件冒泡

- 阻止合成事件的冒泡用 `e.stopPropagation()`
- 阻止合成事件和最外层 document 事件冒泡，使用 `e.nativeEvent.stopImmediatePropogation()`
- 阻止合成事件和除了最外层 document 事件冒泡，通过判断 e.target 避免

## 类型定义

```ts
/** An event which takes place in the DOM. */
interface Event {
  /**
   * Returns true or false depending on how event was initialized. True if event goes through its target's ancestors in reverse tree order, and false otherwise.
   */
  readonly bubbles: boolean;
  cancelBubble: boolean;
  /**
   * Returns true or false depending on how event was initialized. Its return value does not always carry meaning, but true can indicate that part of the operation during which event was dispatched, can be canceled by invoking the preventDefault() method.
   */
  readonly cancelable: boolean;
  /**
   * Returns true or false depending on how event was initialized. True if event invokes listeners past a ShadowRoot node that is the root of its target, and false otherwise.
   */
  readonly composed: boolean;
  /**
   * Returns the object whose event listener's callback is currently being invoked.
   */
  readonly currentTarget: EventTarget | null;
  /**
   * Returns true if preventDefault() was invoked successfully to indicate cancelation, and false otherwise.
   */
  readonly defaultPrevented: boolean;
  /**
   * Returns the event's phase, which is one of NONE, CAPTURING_PHASE, AT_TARGET, and BUBBLING_PHASE.
   */
  readonly eventPhase: number;
  /**
   * Returns true if event was dispatched by the user agent, and false otherwise.
   */
  readonly isTrusted: boolean;
  returnValue: boolean;
  /** @deprecated */
  readonly srcElement: EventTarget | null;
  /**
   * Returns the object to which event is dispatched (its target).
   */
  readonly target: EventTarget | null;
  /**
   * Returns the event's timestamp as the number of milliseconds measured relative to the time origin.
   */
  readonly timeStamp: number;
  /**
   * Returns the type of event, e.g. "click", "hashchange", or "submit".
   */
  readonly type: string;
  /**
   * Returns the invocation target objects of event's path (objects on which listeners will be invoked), except for any nodes in shadow trees of which the shadow root's mode is "closed" that are not reachable from event's currentTarget.
   */
  composedPath(): EventTarget[];
  initEvent(type: string, bubbles?: boolean, cancelable?: boolean): void;
  /**
   * If invoked when the cancelable attribute value is true, and while executing a listener for the event with passive set to false, signals to the operation that caused event to be dispatched that it needs to be canceled.
   */
  preventDefault(): void;
  /**
   * Invoking this method prevents event from reaching any registered event listeners after the current one finishes running and, when dispatched in a tree, also prevents event from reaching any other objects.
   */
  stopImmediatePropagation(): void;
  /**
   * When dispatched in a tree, invoking this method prevents event from reaching any objects other than the current object.
   */
  stopPropagation(): void;
  readonly AT_TARGET: number;
  readonly BUBBLING_PHASE: number;
  readonly CAPTURING_PHASE: number;
  readonly NONE: number;
}
```
