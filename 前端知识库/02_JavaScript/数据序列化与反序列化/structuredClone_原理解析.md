# structuredClone 原理解析

## 📝 概要

`structuredClone()` 是浏览器内置的深拷贝算法，基于 **结构化克隆算法（Structured Clone Algorithm）**，能安全复制多数复杂类型（Map、Set、Date、RegExp…），并支持循环引用。

## 📌 核心知识点

- `structuredClone()` 基于 **浏览器底层的结构化克隆算法**。

- 能深拷贝多数常见内置对象（Array、Object、Map、Set、Date、RegExp…）。

- **支持循环引用**（JSON 无法支持）。

- 不会保留原型链（prototype 不复制）。

- 不能复制函数、DOM 节点、类实例的方法等不可克隆类型。

## 💻 示例代码

```js
const original = {
  name: "小明",
  date: new Date(),
  set: new Set([1, 2]),
  map: new Map([["x", 10]]),
};

original.self = original; // 循环引用

const copy = structuredClone(original);

console.log(copy);
console.log(copy.self === copy); // true
console.log(copy.date instanceof Date); // true
```

## 🧠 structuredClone 的底层原理（图解版）

### 🔍 1. 结构化克隆算法核心步骤

```
        ▼ 用户传入数据
 [Prepare Input Value]
        ▼
 [Check object type 是否可克隆]
        ▼
 [创建新对象，但不复制内部属性]
        ▼
 [递归克隆属性/键值]
        ▼
 [处理循环引用（使用 map 记录访问过的对象）]
        ▼
 [返回完整拷贝]
```

---

### 🔍 2. 可克隆 vs 不可克隆类型

#### ✅ **可克隆（structured-cloneable）**

- Object / Array

- Map / Set

- Date

- RegExp

- ArrayBuffer / TypedArray

- Blob / File

- ImageBitmap

- Error（部分类型）

#### ❌ **不可克隆**

- Function

- Symbol（作为 key 可以，但 value 不行）

- WeakMap / WeakSet

- DOM 元素

- 原型链（prototype 不会被复制）

- 带方法的 class 实例（方法被丢弃）

---

### 🔍 3. 循环引用处理机制

structuredClone 内部维护一个 `clonedObjects` 映射表：

```
original ──► copy
   ▲          ▲
   └── self ──┘
```

当再次遇到已拷贝过的对象时：

- 不重新克隆

- 而是直接引用已创建的副本

从而避免 **爆栈** 或 **无限递归**。

---

### 🔍 4. 为什么比 JSON 方法更强？

|特性|JSON.stringify|structuredClone|
|---|---|---|
|循环引用|❌ 报错|✔ 支持|
|Date|转成字符串|✔ 保留|
|RegExp|转成空对象|✔ 保留|
|Map/Set|丢失|✔ 保留|
|NaN/Infinity|变 null|✔ 保留|
|原型链|❌ 丢失|❌ 也丢失|
|函数|❌ 丢失|❌ 不支持|

**能力远强于 JSON，性能更好且更安全。**

---

## ⚠️ 常见误区

- ❌ **结构化克隆 = 完美深拷贝**  
    → 错，不能复制函数 / DOM / WeakMap 等。

- ❌ 认为 structuredClone 会保留原型链  
    → 实际是 **创建普通对象** 作为复制结果。

## 🎯 适用场景

- 深拷贝复杂结构数据（Map、Set、Date、循环引用）

- Web Worker 线程间的数据传递

- 大规模对象快照

- 对象可安全序列化的场景

## 🔗 关联笔记

- [[深拷贝_vs_浅拷贝]]

- [[JSON_stringify_深拷贝的缺陷]]

- [[structuredClone_最佳实践]]

## 🏷️ 标签

#javascript #deepclone #browserAPI #序列化
