# JSON.stringify 深拷贝的缺陷

## 📝 概要

使用 `JSON.parse(JSON.stringify(obj))` 实现深拷贝虽然简单，但会导致数据丢失、类型错误、循环引用失败等问题，因此不适合在真实项目使用。

---

## 📌 核心知识点

- JSON 序列化只支持 **可 JSON 化的数据类型**，会丢失函数、Symbol、undefined 等内容。

- 无法处理 **循环引用**，会直接抛出错误导致程序崩溃。

- 不会保留 **原型链**，class 实例会变成普通对象。

- 无法正确处理 Date、RegExp、Map、Set、NaN、Infinity 等特殊类型。

---

## 💻 示例代码

### ❌ 1. 循环引用会报错

```js
const obj = { name: "对象" };
obj.self = obj;

JSON.stringify(obj); 
// ❌ TypeError: Converting circular structure to JSON
```

---

### ❌ 2. 丢失特殊类型

```js
const origin = {
  func: () => {},
  symbol: Symbol('x'),
  undef: undefined,
  date: new Date(),
  reg: /abc/gi,
  nan: NaN,
  inf: Infinity
};

const copy = JSON.parse(JSON.stringify(origin));
console.log(copy);

// {
//   date: "2025-02-01T00:00:00.000Z", // 转成字符串
//   reg: {},                          // 变为空对象
//   nan: null,                        // NaN → null
//   inf: null                         // Infinity → null
//   // func、symbol、undefined 消失
// }
```

---

### ❌ 3. 原型链丢失

```js
class Person {
  constructor(name) {
    this.name = name;
  }
  sayHi() { return "Hi!"; }
}

const p = new Person("张三");
const copy = JSON.parse(JSON.stringify(p));

console.log(copy.sayHi);         // undefined
console.log(copy instanceof Person); // false
```

---

### ❌ 4. Map / Set / WeakMap / WeakSet 变成空对象

```js
const obj = {
  map: new Map([["a", 1]]),
  set: new Set([1, 2, 3]),
};

console.log(JSON.parse(JSON.stringify(obj))); 
// { map: {}, set: {} }
```

---

## ⚠️ 常见误区

- **误区 1：JSON.stringify 是一种“简单深拷贝方案”**  
    → 实际上它丢失大量数据类型，只适合非常简单的纯对象。

- **误区 2：项目里小对象用 JSON 深拷贝没问题**  
    → 一旦对象结构变化或后期加入特殊字段，就可能产生隐蔽 bug。

- **误区 3：JSON 性能更好**  
    → 对一些结构化对象，JSON 的序列化反而比 structuredClone 更慢。

---

## 🎯 适用场景

- 仅适用于 **简单配置对象/纯数据对象**，数据类型包含：

  - 数字 / 字符串

  - 普通对象 / 数组

  - 布尔值、null

❗ **只要有特殊类型、循环引用、class 实例就不能用 JSON 深拷贝。**

---

## 🔗 关联笔记

- [[structuredClone最佳实践]]

- [[structuredClone原理解析]]

- [[深拷贝与浅拷贝]

---

## 🏷️ 标签

#javascript #深拷贝 #JSON #序列化 #数据类型
