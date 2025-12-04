# **React 受控与非受控组件**

## 📝 概要

React 表单输入有两种模式：受控组件（state 管控）与非受控组件（DOM 管控）。

## 📌 核心知识点

### 受控组件

- value 由 state 控制

- onChange 更新 state

- 可做即时校验

### 非受控组件

- value 由 DOM 管理

- 使用 ref 获取值

- 适合简单场景

## 💻 示例代码

### 受控组件

```jsx
const [value, setValue] = useState("");
<input value={value} onChange={e => setValue(e.target.value)} />;
```

### 非受控组件

```jsx
const ref = useRef();
<input ref={ref} defaultValue="abc" />;
```

## ⚠️ 常见误区

- 非受控并不是“更差”；在性能敏感场景更适合。

- 受控组件不是“双向绑定”，更新必须显式触发。

## 🎯 适用场景

- 受控：需要校验、格式化、实时提示

- 非受控：一次性提交、性能优化

## 🔗 关联笔记

- [[react_state]]

## 🏷️ 标签

# react #表单
