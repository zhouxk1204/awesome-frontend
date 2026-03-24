# **react_state（组件内部可变数据）**

## 📝 概要

State 是 React 用来管理组件 **内部可变数据** 的机制。当 state 变化时，React 会触发组件重新渲染，从而更新 UI。

在函数组件中使用 `useState` 管理状态；在类组件中使用 `this.setState`。

---

## 📌 核心知识点

### **1. useState 的基础使用**

```jsx
const [count, setCount] = useState(0);
```

- `count`：当前状态值

- `setCount`：更新状态的函数

- `0`：初始值，仅首次渲染生效

---

### **2. 状态更新是异步 & 批处理的**

- 多次 `setState` / `setCount` 会被 React **合并（batching）**

- 更新不是立即生效，而是在事件结束后统一更新

- 在 React 18 中，**批处理变成默认行为**

---

### **3. 函数式更新（推荐）**

当新的 state 依赖旧的 state 时：

```jsx
setCount(prev => prev + 1);
```

可以避免闭包陷阱，也确保拿到最新值。

---

### **4. React state 必须保持不可变性**

❌ 直接修改状态不会触发渲染：

```jsx
state.count++
```

✔️ 正确方式（返回新对象/新数组）：

```jsx
setList(prev => [...prev, 1]);
```

---

### **5. 初始化函数只执行一次**

```jsx
const [data] = useState(() => expensiveCompute());
```

传入函数可避免每次渲染都执行 `expensiveCompute()`。

---

### **6. 数组、对象必须返回新引用**

对象更新：

```jsx
setUser(prev => ({ ...prev, age: 20 }));
```

数组更新：

```jsx
setArr(prev => prev.filter(v => v !== 1));
```

React 通过 **引用变化** 判断是否需要重新渲染。

---

### **7. setState 不会自动合并对象**

与类组件不同，函数组件不会合并对象：

```jsx
setState({ a: 1 });  // 覆盖整个 state 对象，不是合并
```

---

### **8. 类组件的 this.setState**

```jsx
this.setState({ count: this.state.count + 1 });
```

类组件的 setState 会自动浅合并对象。

函数式更新：

```jsx
this.setState(prev => ({ count: prev.count + 1 }));
```

---

### **9. State 只在下一次渲染中生效**

```jsx
setCount(5);
console.log(count); // 还是旧值
```

因为渲染还未发生。

---

### **10. 状态不会跨组件共享**

要跨组件共享数据：

- Context

- Redux / Zustand / Jotai

- 通过 props 向下传递回调向上传递等

---

## 💻 示例代码

### ✔️ 基础示例

```jsx
const [count, setCount] = useState(0);

function increment() {
  setCount(prev => prev + 1);
}
```

### ✔️ 更新对象

```jsx
const [user, setUser] = useState({ name: 'Tom', age: 18 });

setUser(prev => ({ ...prev, age: prev.age + 1 }));
```

### ✔️ 更新数组

```jsx
const [list, setList] = useState([]);

setList(prev => [...prev, 'new item']);
```

---

## ⚠️ 常见误区

### **❌ 1. 多次 setState 会逐次更新**

实际上是 **批处理**

```jsx
setCount(count + 1);
setCount(count + 1);
```

最终只加 +1。

✔️ 正确：

```jsx
setCount(prev => prev + 1);
setCount(prev => prev + 1);
```

最终 +2。

---

### **❌ 2. state 不是立即更新的**

不能在 setState 后立即使用新值。

---

### **❌ 3. 修改引用类型不会触发更新**

```jsx
list.push(1); // ❌ 不会触发更新
setList(list);
```

必须返回新数组。

---

### **❌ 4. useState 初始值每次渲染都会执行**

传函数即可避免

```jsx
useState(() => heavyCompute());
```

---

## 🎯 适用场景

- 管理 UI 的本地状态：输入框、打开/关闭、计数器

- 接收异步请求结果并渲染 UI

- 需要响应用户交互并动态更新内容

- 管理表单、选项、切换状态等

---

## 🔗 关联笔记

- [[react_受控与非受控组件]]
- [[react_state_批处理机制]]
- [[react_渲染流程]]

---

## 🏷️ 标签

# react #state #hooks #组件状态 #不可变数据 #批处理机制
