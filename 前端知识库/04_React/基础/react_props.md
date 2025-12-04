# react_props（组件之间的通信方式）

## 📝 概要

Props（Properties）是 React 中父组件向子组件传递数据和回调的机制。Props 保持单向数据流、不可变（对接收方而言），用于组件配置、行为注入与组合。

---

## 📌 核心知识点

- **单向数据流**：Props 由父传子，子不能直接修改父传入的 props。

- **不可变（只读）**：子组件应把 props 视为只读值；需要修改时通过回调通知父组件。

- **任意类型**：可以传入原始值、对象、数组、函数、React 元素、组件、甚至 JSX。

- **children 特殊字段**：`props.children` 用于接收嵌套的子节点（可能为单个节点、数组、null、字符串等）。

- **展开（spread）语法**：`<Comp {...props} />` 可批量传递 props，但要注意覆盖与安全。

- **默认值与类型检查**：可以使用 `defaultProps`（类组件 / 兼容写法）或 TypeScript/PropTypes 做类型与默认值约束。

- **性能关注点**：传入新引用（对象/函数）会触发浅比较失效，影响 `React.memo` / `PureComponent` 的缓存效果。

- **组合优于继承**：通过 props 组合子组件功能（render props、children、高阶组件、hooks）是推荐模式。

---

## 💻 示例代码（详尽）

### 基本用法

```jsx
function Child({ name }) {
  return <p>Hello, {name}</p>;
}

function Parent() {
  return <Child name="John" />;
}
```

### 传入函数（父传达行为）

```jsx
function Child({ onClick }) {
  return <button onClick={() => onClick('from child')}>Click</button>;
}

function Parent() {
  const handle = (msg) => console.log(msg);
  return <Child onClick={handle} />;
}
```

### children 用法（嵌套内容）

```jsx
function Card({ children }) {
  return <div className="card">{children}</div>;
}

<Card>
  <h3>Title</h3>
  <p>Content</p>
</Card>
```

### props spread（注意覆盖风险）

```jsx
const extra = { id: 'u1', title: 'hello' };
<Component {...extra} title="override" /> // title 会被覆盖为 "override"
```

### 默认值（函数组件 + ES6 解构）

```jsx
function Button({ color = 'blue', children }) {
  return <button style={{ color }}>{children}</button>;
}
```

### PropTypes（运行时类型检查 - 可选）

```jsx
import PropTypes from 'prop-types';

function Avatar({ src, alt }) { ... }

Avatar.propTypes = {
  src: PropTypes.string.isRequired,
  alt: PropTypes.string,
};
```

### TypeScript（静态类型示例）

```tsx
type Props = { id: number; name: string; onClick?: () => void; };

function Item({ id, name, onClick }: Props) {
  return <div onClick={onClick}>{name} - {id}</div>;
}
```

### 避免不必要的重新渲染（memo + useCallback）

```jsx
const Child = React.memo(function Child({ onClick }) {
  console.log('Child render');
  return <button onClick={onClick}>Click</button>;
});

function Parent() {
  const [count, setCount] = useState(0);
  const handle = useCallback(() => setCount(c => c + 1), []);
  return <Child onClick={handle} />;
}
```

### Forwarding refs（组合场景）

```jsx
const Input = React.forwardRef(function Input(props, ref) {
  return <input ref={ref} {...props} />;
});
```

### Render Props（动态渲染）

```jsx
function DataFetcher({ children }) {
  const data = useData();
  return children(data);
}

// 使用
<DataFetcher>
  {data => <List items={data} />}
</DataFetcher>
```

---

## ⚠️ 常见误区

- ❌ **Prop 会双向绑定**：不是的，props 单向从父到子。要改变数据必须由父改变 state 并重新传入。

- ❌ **直接修改 props**：不要 `props.x = ...`，这会破坏 React 的单向数据流和不可变约束。

- ❌ **传入函数不安全**：函数可以安全传入，但每次重新创建会改变引用，可能导致子组件不必要重渲染。最佳做法用 `useCallback` 或在父中稳定函数引用。

- ❌ **spread 总是安全**：使用 `{...props}` 方便，但可能无意中泄露不该传的内部字段或覆盖显式传入的 props。

- ❌ **props.children 是字符串**：children 是任意 ReactNode，可能是数组、元素、null、字符串等，使用时考虑类型和 `React.Children` API。

---

## 🎯 适用场景（最佳实践）

- **配置组件**：通过 props 传入展示数据、主题样式、开关标志等。

- **注入行为**：父组件将回调函数传给子组件（事件处理、回调通知）。

- **组合 UI**：`children`、render props、slot-like patterns 提供高度可组合的组件。

- **高复用组件**：通过 props 提供不同配置以复用同一组件实现不同 UI。

- **控制 / 非控制模式**：表单组件可通过 `value` + `onChange` 做受控组件，也支持 `defaultValue` + ref 做非受控。

---

## ✅ 进阶技巧与注意事项

- **性能**：避免把大型对象/数组或内联函数直接作为 props（会破坏浅比较）。使用 `useMemo` / `useCallback` 或将数据提升到父外层并保持引用稳定。

- **不可变数据**：鼓励使用不可变操作（复制/创建新对象）来更新 state，这与 props 的浅比较更友好。

- **键 key 的意义**：在列表渲染时为子项提供稳定的 `key`，避免使用索引（除非列表不可变）。

- **默认值策略**：对可选 props 在参数解构中设默认值，或在 TypeScript 中用 `?` 且处理 undefined 情况。

- **API 设计**：组件 prop 接口应简洁、明确，优先考虑可组合性而非复杂的开关。

- **文档化 props**：为公共组件写好 prop 文档与示例，方便复用与维护。

---

## 🔗 关联笔记

- [[react_state]]（State 与 props 的关系）

- [[react_受控与非受控组件]]（表单组件模式）

- [[react_JSX_语法]]（props 在 JSX 中的写法）

- [[react_memo_与性能优化]]（props 引用稳定性与缓存）

- [[react_forwardRef_与组合模式]]

---

## 🏷️ 标签

# react #props #组件设计 #前端最佳实践
