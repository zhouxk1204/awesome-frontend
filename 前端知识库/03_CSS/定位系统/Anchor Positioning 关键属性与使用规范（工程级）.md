# Anchor Positioning 关键属性与使用规范（工程级）

## 📝 使用前说明

> 本文为**工程使用规范**，不讲原理。  
> 如需理解定位模型，请先阅读：  
> [[CSS Anchor Positioning（概念与定位模型）]]

---

## 1️⃣ anchor-name —— 锚点命名规范

### 基本作用

为被锚定的元素声明一个**可被引用的锚点名称**。

```css
.button {
  anchor-name: --button-primary;
}
```

---

### ✅ 命名规范（强烈建议）

```txt
--<角色>-<用途>
```

**推荐示例：**

- `--trigger`

- `--button-primary`

- `--avatar`

- `--menu-anchor`

- `--card-header`

**不推荐：**

- ❌ `--a`

- ❌ `--tmp`

- ❌ `--123`

> Anchor 名称本质是「空间参考点」，  
> 应体现**语义角色**而非样式。

---

## 2️⃣ position-anchor —— 绑定锚点目标

### 基本作用

指定当前元素要**相对于哪个锚点元素定位**。

```css
.tooltip {
  position: absolute;
  position-anchor: --button-primary;
}
```

### 特性说明

- 可跨 DOM 层级

- 不要求父子关系

- 多个元素可共享一个 anchor

---

## 3️⃣ position-area —— 语义化位置声明（重点）

### 设计目的

用**语义位置**描述相对关系，  
避免手动计算 `top / left / transform`。

---

### 🧭 可用语义关键字全集

```txt
top
right
bottom
left
center
```

---

### 🧩 合法组合方式

|写法|含义|
|---|---|
|`top`|锚点正上方|
|`bottom`|锚点正下方|
|`left`|锚点左侧|
|`right`|锚点右侧|
|`top left`|左上|
|`top right`|右上|
|`bottom left`|左下|
|`bottom right`|右下|
|`center`|正中|
|`center right`|中右|
|`center left`|中左|
|`top center`|上中|
|`bottom center`|下中|

> **顺序不重要，但最多两个值**

---

### 🚫 不合法组合

- ❌ `top bottom`

- ❌ `left right`

- ❌ 超过两个值

---

## 4️⃣ anchor() —— 精确边界引用

### 基本作用

从锚点元素中**读取具体边界值**。

```css
.tooltip {
  top: anchor(bottom);
  left: anchor(right);
}
```

---

### 常用 anchor 函数

|写法|含义|
|---|---|
|`anchor(top)`|锚点上边|
|`anchor(right)`|锚点右边|
|`anchor(bottom)`|锚点下边|
|`anchor(left)`|锚点左边|
|`anchor(center)`|锚点中心|

---

## 5️⃣ position-area vs anchor() 使用选择

|场景|推荐|
|---|---|
|Tooltip / Menu|`position-area`|
|角标 / Badge|`position-area`|
|像素级对齐|`anchor()`|
|自定义偏移|`anchor()` + `margin`|

---

## 6️⃣ 典型使用场景对照表

|场景|推荐写法|
|---|---|
|右上角角标|`top right`|
|下拉菜单|`bottom left`|
|Tooltip|`top center`|
|Popover|`bottom center`|
|装饰性伪元素|`top right`|

---

## 7️⃣ 组合最佳实践（非常重要）

### ✅ 推荐

```css
.badge {
  position: absolute;
  position-anchor: --avatar;
  position-area: top right;
}
```

---

### ❌ 不推荐

```css
.badge {
  top: anchor(top);
  left: anchor(left);
  transform: translate(50%, -50%);
}
```

> **语义能表达的，就不要数学计算**

---

## 8️⃣ 多 Anchor 的设计建议

- 一个组件可暴露多个 anchor

- 不同用途，不同命名

```css
.card {
  anchor-name: --card --card-header;
}
```

---

## 9️⃣ 与伪元素结合（推荐模式）

```css
.avatar {
  anchor-name: --avatar;
}

.avatar::after {
  position: absolute;
  position-anchor: --avatar;
  position-area: top right;
}
```

📌 这是 **Anchor Positioning 的推荐用法之一**。

---

## 🔑 一句话规范总结

> **Anchor 名字表达“角色”，  
> position-area 表达“位置语义”，  
> anchor() 只在语义不够时使用。**

---

## 🏷️ 标签

- #css #anchor-positioning #position-anchor #ui-pattern
