# **React 组件基础（函数组件 / 类组件）**

## 📝 概要

React 的 UI 单元由组件构成，分为 **函数组件** 与 **类组件**。组件可管理状态、响应事件、组合 UI，并通过 Props 和 State 进行数据流管理。

---

## 📌 核心知识点

### **1. 函数组件（Function Component）**

- 使用普通函数或箭头函数定义

- 支持 **Hooks**（useState, useEffect, useRef 等）

- 更轻量、易于测试、性能优化空间大

- 无 `this`，避免类组件的绑定问题

```jsx
function Welcome({ name }) {
  return <h1>Hello, {name}</h1>;
}

// 或箭头函数
const Welcome = ({ name }) => <h1>Hello, {name}</h1>;
```

---

### **2. 类组件（Class Component）**

- 继承自 `React.Component`

- 拥有生命周期方法（componentDidMount、componentDidUpdate、componentWillUnmount 等）

- 通过 `this.state` 管理状态，通过 `this.setState` 更新

- 适合实现 **错误边界** 或老项目迁移

```jsx
class Welcome extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }

  increment = () => this.setState({ count: this.state.count + 1 });

  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

---

### **3. 组件命名规则**

- **首字母大写**：区分 HTML 标签

- 小写表示原生 DOM 元素

- 组件名称尽量语义化（如 `Card`、`Modal`、`UserProfile`）

---

### **4. Props 与 State 的关系**

- **Props**：父组件传入，单向数据流，只读

- **State**：组件内部管理，可变，触发渲染

- 函数组件通过 Hooks 管理状态，类组件通过 `this.state` 和 `this.setState`

---

### **5. 生命周期 / 副作用处理**

#### 函数组件

- `useEffect` 替代生命周期

- 支持多次注册 effect，返回函数做清理

```jsx
useEffect(() => {
  console.log('Mounted or updated');
  return () => console.log('Cleanup');
}, [deps]);
```

#### 类组件

- `componentDidMount`：组件挂载完成

- `componentDidUpdate`：更新后执行

- `componentWillUnmount`：卸载前清理

---

### **6. 受控组件 / 非受控组件**

- **受控**：表单值由 state 管理，变化通过事件同步

- **非受控**：表单值由 DOM 自己管理，通过 ref 获取

---

### **7. 性能与优化**

- 使用 `React.memo` 避免函数组件不必要渲染

- useCallback / useMemo 优化回调和复杂计算

- 类组件使用 `PureComponent` 或 `shouldComponentUpdate`

---

### **8. 组合优于继承**

- 组件通过 **props.children、render props、HOC、Hooks** 组合功能

- 避免类继承层次复杂化

---

## 💻 示例代码

```jsx
// 函数组件 + useState + useEffect
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log('Count changed:', count);
  }, [count]);

  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}

// 类组件 + state + 生命周期
class CounterClass extends React.Component {
  state = { count: 0 };

  componentDidMount() { console.log('Mounted'); }
  componentDidUpdate() { console.log('Updated'); }
  componentWillUnmount() { console.log('Unmount'); }

  increment = () => this.setState({ count: this.state.count + 1 });

  render() {
    return <button onClick={this.increment}>{this.state.count}</button>;
  }
}
```

---

## ⚠️ 常见误区

- ❌ 函数组件不是无状态组件

- ❌ 类组件不是过时，只是在部分新场景减少使用

- ❌ Hooks 不能在循环、条件或嵌套函数中调用

- ❌ props 不是可变数据，不能直接修改

- ❌ 使用 state 需要返回新对象/数组（不可变性）

---

## 🎯 适用场景

- **函数组件**：业务页面、UI 组件、列表、表单

- **类组件**：错误边界、生命周期复杂逻辑、老项目迁移

- **组合与复用**：通过 props.children、render props、HOC、Hooks 构建可复用组件

---

## 🔗 关联笔记

- [[react_JSX_语法]]

- [[react_props]]

- [[react_state]]

- [[react_受控与非受控组件]]

- [[react_hooks_基础]]

---

## 🏷️ 标签

#react #组件 #函数组件 #类组件 #生命周期 #hooks #组件组合
