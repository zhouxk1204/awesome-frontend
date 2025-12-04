# react_JSX_语法

## 📝 概要

JSX（JavaScript XML）是 React 用来描述 UI 的语法扩展。  
它让你可以在 JavaScript 中写出类似 HTML 的结构，同时拥有完整的 JS 表达式能力。

JSX 最终不会直接在浏览器中运行，而是通过 Babel 转译为 `React.createElement` 或 `jsx/jsxs` 调用。

---

## 📌 核心知识点

### **1. JSX 本质**

- 不是模板语言，是 JavaScript 的语法扩展。
- JSX 被编译为纯 JavaScript 表达式，因此 **所有 JSX 都是表达式**。

---

### **2. 属性（Props）规则**

#### ✔ 驼峰命名法

```jsx
<div className="box" tabIndex={0}></div>
```

#### ✔ 特殊属性映射

|HTML|JSX|
|---|---|
|class|className|
|for|htmlFor|
|tabindex|tabIndex|
|readonly|readOnly|

#### ✔ aria 与 data-* 保持原样

```jsx
<div aria-hidden="true" data-id="123"></div>
```

#### ✔ 布尔属性

```jsx
<input disabled />        // disabled={true}
<input disabled={false} />
```

#### ✔ 字符串 vs 表达式

```jsx
<div title="hello" />
<div title={name} />
```

---

### **3. 子元素（Children）**

#### ✔ 文本、表达式、嵌套均可

```jsx
<div>
  {user.name}
  {list.map(i => <li key={i.id}>{i.name}</li>)}
</div>
```

#### ✔ false/null/undefined 不会渲染

```jsx
<div>{false}</div>  // 不会显示
```

#### ✔ 数组可直接渲染

```jsx
<div>{['a','b','c']}</div>  // abc
```

---

### **4. 花括号 {} 的用法**

#### ✔ 可以放入任意 JS 表达式

```jsx
<p>{count + 1}</p>
```

#### ❌ 不能写语句（如 if、for）

```jsx
{ if (a) {} } // ❌错误
```

正确方式：

```jsx
{ a > 0 && <p>{a}</p> }
{ a > 0 ? <p>OK</p> : <p>NO</p> }
```

---

### **5. 必须只有一个根节点**

```jsx
// ❌
<div></div>
<p></p>

// ✔
<>
  <div></div>
  <p></p>
</>
```

---

### **6. Fragment 写法**

```jsx
<>
  <h1>Hello</h1>
</>
```

或带 key（必须使用显式写法）：

```jsx
<React.Fragment key={1}>
  <div />
</React.Fragment>
```

---

### **7. 自闭合标签**

```jsx
<input />
<img src="xx" />
```

---

### **8. JSX 中的注释**

```jsx
<div>
  {/* 这是注释 */}
</div>
```

---

### **9. 多行 JSX 必须加括号**

```jsx
return (
  <div>
    <p>Hello</p>
  </div>
);
```

---

### **10. JSX 中使用 JavaScript 对象**

#### ✔ style 传入对象（需要驼峰命名）

```jsx
<div style={{ backgroundColor: 'red', fontSize: 14 }} />
```

---

### **11. 动态属性名（ES6 展开）**

```jsx
const props = { id: 1, title: 'Hello' };

<div {...props} />
```

注意：后写的属性会覆盖前面的。

---

### **12. JSX 中使用函数**

```jsx
<button onClick={() => alert(1)}>Click</button>
```

---

### **13. JSX 中使用数组 map 渲染列表**

```jsx
<ul>
  {list.map(item => (
    <li key={item.id}>{item.name}</li>
  ))}
</ul>
```

---

### **14. JSX 与条件渲染**

```jsx
{loading && <Spinner />}
{isLogin ? <Home /> : <Login />}
```

---

### **15. JSX 与逻辑运算（短路规则）**

⚠ 注意 `0` 会被渲染

```jsx
{0 && <div>不会执行但 0 会显示</div>}
```

---

### **16. className 动态合并**

```jsx
<div className={`btn ${active ? 'active' : ''}`} />
```

---

### **17. dangerouslySetInnerHTML（插入 HTML）**

⚠️ 有安全风险 XSS

```jsx
<div dangerouslySetInnerHTML={{ __html: "<h1>Hello</h1>" }} />
```

---

## 🔍 JSX 转译机制（简要）

### classic 模式（旧）

```js
React.createElement("div", null, "Hello")
```

### automatic 模式（React 17+ 默认）

```js
import { jsx } from "react/jsx-runtime";
jsx("div", { children: "Hello" });
```

详解见：  
👉 [[react_JSX_转译机制]]

---

## ⚠️ 常见误区（扩展版）

### ❌ JSX 是模板

JSX 只是语法糖，底层仍然是 JS。

### ❌ {} 中可以写 if / for

NO！只能写表达式。

### ❌ style 是字符串

应写成对象。

### ❌ React 会自动防止 XSS

不是全自动，`dangerouslySetInnerHTML` 仍有风险。

---

## 🎯 适用场景

- 构建结构复杂的 UI

- 模板中插入任意 JS 运算

- 通过表达式动态渲染界面

- 与组件 props 结合创建灵活可复用 UI

---

## 🔗 关联笔记

- [[react_JSX_转译机制]]

- [[react_组件基础]]

---

## 🏷️ 标签

# react #JSX #syntax #transpiling
