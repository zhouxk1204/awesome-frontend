# **react_JSX_转译机制（React.createElement 与自动模式）**

## 📝 概要

JSX 并不是浏览器支持的语法，需要被 Babel 转译为 JavScript 函数调用。

## 📌 核心知识点

- **传统模式 (classic)**：JSX 转为 `React.createElement`

- **自动模式 (automatic)**：使用 `jsx()` / `jsxs()`，无需 import React

- JSX 编译步骤：类型识别 → props 收集 → children 递归处理

## 💻 示例代码

### JSX

```jsx
const element = <div className="app">Hello</div>;
```

### classic 模式转换后

```js
React.createElement("div", { className: "app" }, "Hello");
```

### automatic 模式转换后

```js
import { jsx } from "react/jsx-runtime";
jsx("div", { className: "app", children: "Hello" });
```

## ⚠️ 常见误区

- 自动模式并不是 React 的“魔法”，只是不同的编译产物。

- `React.createElement` 的返回值不是 DOM，而是虚拟 DOM 对象。

## 🎯 适用场景

- 理解虚拟 DOM 的结构与渲染流程

- 手写 React 渲染器或调试低层代码

## 🔗 关联笔记

- [[React JSX语法]]

## 🏷️ 标签

# react #JSX #babel
