# react_stopPropagation_与_isPropagationStopped_源码解析

## 📝 概要

在 React 中，`stopPropagation()` 会**真正阻止原生 DOM 事件冒泡**（如阻止 body、document 上的事件），但 React 自己的事件冒泡由内部机制控制，因此是否继续执行 React 组件树中的事件，需要通过 `isPropagationStopped()` 判断。React 使用此标记决定是否继续执行后续监听器。

- 官方文档描述：[[https://zh-hans.react.dev/reference/react-dom/components/common]]

---

## 📌 核心知识点

- **React SyntheticEvent.stopPropagation() 会调用原生 `event.stopPropagation()`，因此真正阻止 DOM 冒泡。**

- React 的事件系统是模拟的（不是依赖 DOM 冒泡），派发流程由 `processDispatchQueueItemsInOrder` 控制。

- React 会检查 `event.isPropagationStopped()` 以决定是否继续触发下一层组件的事件回调。

- 因此：

  - `stopPropagation()`：阻止 **DOM 冒泡 + React 监听器**

  - `isPropagationStopped()`：只控制 **React 自己的事件队列是否继续执行**，但不影响 DOM

---

## 💻 示例代码（含源码）

### 🔹 React SyntheticEvent.stopPropagation 源码

（来自：[https://github.com/facebook/react/blob/main/packages/react-native-renderer/src/legacy-events/SyntheticEvent.js#L131](https://github.com/facebook/react/blob/main/packages/react-native-renderer/src/legacy-events/SyntheticEvent.js#L131) ）

```js
stopPropagation: function () {
  const event = this.nativeEvent;
  if (!event) return;

  if (event.stopPropagation) {
    event.stopPropagation(); // ← 真正阻止 DOM 冒泡
  } else if (typeof event.cancelBubble !== 'unknown') {
    event.cancelBubble = true;
  }

  // React 内部：标记已停止冒泡
  this.isPropagationStopped = functionThatReturnsTrue;
},
```

---

### 🔹 React 内部事件派发：processDispatchQueueItemsInOrder

（来自：[https://github.com/facebook/react/blob/main/packages/react-dom-bindings/src/events/DOMPluginEventSystem.js#L266](https://github.com/facebook/react/blob/main/packages/react-dom-bindings/src/events/DOMPluginEventSystem.js#L266) ）

```js
function processDispatchQueueItemsInOrder(
  event,
  dispatchListeners,
  inCapturePhase,
) {
  let previousInstance;

  if (inCapturePhase) {
    for (let i = dispatchListeners.length - 1; i >= 0; i--) {
      const {instance, currentTarget, listener} = dispatchListeners[i];

      // 若 isPropagationStopped() 返回 true，则停止 React 自己的冒泡
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
```

### 🧩 重点解释

- 浏览器冒泡：`div → body → document`

- React 的监听器统一注册在 document 上，因此 **DOM 冒泡到 document 时才触发 React**

- 因此：

  - React 的 `stopPropagation()` 会阻止 DOM 后续传播（如 body 不会触发）

  - React 组件之间的“冒泡”完全由 `processDispatchQueueItemsInOrder` 决定，**与 DOM 无关**

  - `isPropagationStopped()` 控制的是组件树事件是否继续向上执行

---

## ⚠️ 常见误区

### ❌ 误区 1：React 的 stopPropagation 不会阻止 DOM 冒泡

**错误。**React 内部调用了原生 `event.stopPropagation()`，因此 body / document 上绑定的事件都会被阻止。

### ❌ 误区 2：isPropagationStopped 会阻止 DOM 冒泡

**错误。**它只阻止 React 自己的事件派发流程，与 DOM 冒泡无关。

### ❌ 误区 3：React 冒泡依赖 DOM 冒泡

React 事件系统的组件树冒泡是**模拟的、手动控制的**，不依赖真实 DOM 冒泡机制。

---

## 🎯 适用场景

- 需要阻止 body / document 上注册的 DOM 监听器（stopPropagation）。

- 需要阻止父组件的 React 事件执行（isPropagationStopped）。

- 需要同时阻止 DOM 和 React 冒泡（调用 stopPropagation）。

- 仅希望阻止 React 冒泡但不阻止 DOM（**需要手动 hack：不要触发原生 stopPropagation，仅改变内部标记**）。

---

## 🔗 关联笔记

- [[事件委托原理]]

- [[浏览器事件模型]]

- [[React 合成事件机制]]

---

## 🏷️ 标签

#javascript #React #事件系统 #合成事件 #前端知识库
