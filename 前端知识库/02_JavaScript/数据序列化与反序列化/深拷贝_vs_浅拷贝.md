# 深拷贝 vs 浅拷贝

## 📝 概要

浅拷贝只复制对象的第一层；深拷贝会完全复制所有层级的值，使拷贝后的对象与源对象互不影响。

---

## 📌 核心知识点

- **浅拷贝（Shallow Copy）**  
    仅复制第一层属性；引用类型的子对象仍指向同一内存地址。

- **深拷贝（Deep Copy）**  
    递归复制所有层级；所有引用类型数据都会重新创建。

- **核心区别**：是否真正“完全独立”复制嵌套对象。

---

## 💻 示例代码

### 📌 浅拷贝示例（Object.assign / 展开运算符）

```js
const obj = { a: 1, b: { c: 2 } };

const shallow = { ...obj };
// 或：const shallow = Object.assign({}, obj);

shallow.b.c = 100;
console.log(obj.b.c); // ❗ 100 —— 被修改
```

### 📌 深拷贝示例（structuredClone）

```js
const obj = { a: 1, b: { c: 2 } };

const deep = structuredClone(obj);

deep.b.c = 100;
console.log(obj.b.c); // ✔️ 2 —— 不受影响
```

---

## ⚠️ 常见误区

- 误区 1：**展开运算符 {...obj} 就是深拷贝**  
    → 只复制第一层，是典型的浅拷贝。

- 误区 2：**JSON 深拷贝适用于所有场景**  
    → 无法处理循环引用、特殊类型（Date、RegExp、Map、Set 等）。

- 误区 3：**深拷贝一定比浅拷贝更好**  
    → 深拷贝会消耗更多性能，不应滥用。

---

## 🎯 适用场景

### ✔️ 浅拷贝适用场景

- 对象层级浅

- 不需要复制复杂结构

- React setState 更新对象（推荐浅拷贝 + 局部替换）

### ✔️ 深拷贝适用场景

- 大型对象的逻辑隔离

- 配置项克隆（form schema、深层 UI 配置）

- 多层嵌套对象的数据处理

- 需要避免引用共享带来的隐式修改风险

---

## 🔗 关联笔记

- [[JSON_stringify_深拷贝的缺陷]]

- [[structuredClone_最佳实践]]

- [[structuredClone_原理解析]]

---

## 🏷️ 标签

#javascript #深拷贝 #浅拷贝 #数据结构 #前端基础
