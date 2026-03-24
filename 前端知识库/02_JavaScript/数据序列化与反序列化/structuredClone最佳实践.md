# structuredClone 最佳实践

## 📝 概要

如何在真实项目中安全、高效、正确地使用 `structuredClone()` 完成深拷贝。

## 📌 核心知识点

- `structuredClone()` 是前端最推荐的深拷贝 API（安全、性能好、支持复杂类型）。

- 适用于 **可序列化的数据结构**，但不适用于函数、DOM、WeakMap 等不可克隆类型。

- 在状态管理、Web Worker、缓存快照中应用广泛。

- 使用前应考虑成本、数据规模和数据结构类型。

## 💻 示例代码

```js
// 基础使用
const original = {
  name: "小明",
  date: new Date(),
  nested: { x: 1 },
  set: new Set([1, 2]),
  map: new Map([["a", 100]]),
};

const copy = structuredClone(original);

console.log(copy.date instanceof Date); // true
console.log(copy.set instanceof Set);   // true
console.log(copy.map instanceof Map);   // true
console.log(copy !== original);         // true
```

---

## 🔍 图解 structuredClone 最佳实践

---

### ✅ 场景 1：深拷贝大型对象（比 JSON 更稳定）

适合：

- 数十 KB ~ 几 MB 的对象

- 存在 Set/Map/Date/RegExp/循环引用

```
复杂对象 ──► structuredClone ──► 安全稳定
```

示例：

```js
const snapshot = structuredClone(largeState);
```

避免使用 JSON：

```js
// ❌ JSON 会丢失类型、死循环报错
JSON.parse(JSON.stringify(largeState));
```

---

### ✅ 场景 2：创建应用快照 —— “时间旅行调试”/撤销操作

```
状态树（State Tree）
      ▼
 structuredClone
      ▼
    历史栈（History Stack）
```

示例：

```js
history.push(structuredClone(currentState));
```

适用：

- Redux-like 状态快照

- 撤销/恢复（Undo/Redo）

- Debugger 工具

---

### ✅ 场景 3：Web Worker 消息传递（最佳实践！）

浏览器 Worker 自动使用 **结构化克隆算法**。

手动深拷贝后再发送，安全性更高：

```js
worker.postMessage(structuredClone(data));
```

优势：

- Map/Set 不会被破坏

- 循环引用自动处理

- 避免 JSON 丢失结构

---

### ✅ 场景 4：避免意外修改原对象（不可变数据设计）

当你不希望方法内部修改入参：

```js
function safeProcess(data) {
  const cloned = structuredClone(data);
  // …安全处理
  return cloned;
}
```

非常适合：

- 工具函数

- 数据转换器

- API 管道

---

## ❗ 不推荐使用 structuredClone 的场景

### ❌ 1. 含有函数、class 实例的方法

```js
const obj = { fn() {}, classInstance: new CustomClass() };

structuredClone(obj); 
// fn 消失，classInstance 变成普通对象
```

### ❌ 2. WeakMap、WeakSet

它们内部的键是弱引用，无法被克隆。

### ❌ 3. 需要保留原型链的对象

structuredClone 不复制 prototype：

```js
class A { }
const a = new A();
const b = structuredClone(a);

a instanceof A // true
b instanceof A // false ❌
```

---

## ⚠️ 常见误区

- 误区 1：structuredClone 能复制所有东西  
    → ❌ 函数、DOM、WeakMap、原型链都无法复制

- 误区 2：structuredClone 是性能最强的深拷贝  
    → ❌ 只有在“纯数据场景”性能最佳

---

## 🎯 适用场景

- 状态快照 / Undo-Redo 功能

- Web Worker 大数据传输

- 大型对象深拷贝（包括 Map、Set、Date、循环引用）

- 避免副作用的不可变数据模式

- 表单与配置数据克隆

---

## 🔗 关联笔记

- [[structuredClone原理解析]]

- [[深拷贝与浅拷贝]]

- [[JSON stringify深拷贝的缺陷]]

---

## 🏷️ 标签

#javascript #deepclone #最佳实践 #浏览器API #序列化
