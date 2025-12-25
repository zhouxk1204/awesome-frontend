# OKLCH 颜色管理

## 📝 概要

OKLCH 是一种以人眼感知一致性为核心的现代颜色模型。与 RGB、HSL 相比，它在 UI 状态变化、色阶生成和设计系统中具有更高的稳定性和可预测性。

---

## 📌 核心知识点

### 1️⃣ 主流颜色模型对比（RGB / HSL / OKLCH）

#### RGB（Red / Green / Blue）

**优点**

- 硬件与浏览器底层模型
- 性能好、兼容性最好
- 所有颜色最终都会转为 RGB 显示

**缺点**

- 不符合人眼感知
- 数值变化 ≠ 视觉变化
- 无法直接表达「亮 / 暗 / 强 / 弱」

**适用场景**

- 底层渲染
- 图片、Canvas、视频
- 非人工调色场景

---

#### HSL（Hue / Saturation / Lightness）

**优点**

- 概念直观，容易理解
- 适合简单的颜色选择器
- 比 RGB 更接近人类直觉

**缺点**

- Lightness ≠ 感知亮度
- 调亮 / 调暗会偏灰或偏色
- 不适合生成稳定的 UI 色阶

**典型问题**
> Hover 后颜色发灰、炸色

**适用场景**

- 低复杂度页面
- 简单主题切换
- 非设计系统级项目

---

#### OKLCH（Lightness / Chroma / Hue）

**优点**

- 基于人眼感知一致性（OKLab）
- 亮度变化线性、可预测
- 非常适合 UI 状态和色阶
- 语义色可保持视觉权重一致

**缺点**

- 学习成本略高
- 老浏览器支持有限
- 需要建立数值规范

**适用场景**

- Design System
- Design Token
- Tailwind / 现代 CSS
- 中大型前端项目

---

### 2️⃣ 为什么 UI 最终应选择 OKLCH

| 需求 | RGB | HSL | OKLCH |
|----|----|----|----|
| 状态变化 | ❌ | ⚠️ | ✅ |
| 色阶生成 | ❌ | ❌ | ✅ |
| 语义一致性 | ❌ | ❌ | ✅ |
| 可预测性 | ❌ | ⚠️ | ✅ |

> **当项目进入「组件复用 / 状态复杂 / 暗黑模式」阶段，  
RGB 和 HSL 的问题会被无限放大。**

---

### 3️⃣ OKLCH 的三通道模型

- **L（Lightness）**：感知亮度（0%–100%）
- **C（Chroma）**：色度 / 鲜艳程度
- **H（Hue）**：色相角度（0–360）

---

### 4️⃣ 颜色管理三条铁律

1. **状态变化（hover / active） → 只改 L**
2. **弱化 / 强化 → 只改 C**
3. **语义换色 → 只改 H**

---

### 5️⃣ UI 推荐亮度阶梯（黄金模板）

```txt
L: 95  90  85  75  65  56  48  40
C: 固定
H: 固定
````

|L 值|用途|
|---|---|
|95|页面背景|
|90|次级背景|
|85|分隔区域|
|75|浅填充|
|65|中强调|
|56|主色|
|48|Active|
|40|深色强调|

---

### 6️⃣ 色度（C）的经验控制

- UI 主色：`0.15 – 0.22`

- 次级 / Disabled：`≤ 0.10`

- `L < 40` 时，`C ≤ 0.08`

---

### 7️⃣ OKLCH from 语法（现代 CSS）

```css
oklch(from <color> <L> <C> <H>)
```

---

## 💻 示例代码

```css
--primary: oklch(56% 0.20 265);

--primary-hover:  oklch(from var(--primary) calc(l + 6%) c h);
--primary-active: oklch(from var(--primary) calc(l - 8%) c h);
--primary-muted:  oklch(from var(--primary) l calc(c * 0.4) h);
```

---

## ⚠️ 常见误区

- ❌ 用 HSL 思维调 OKLCH

- ❌ 低亮度 + 高色度

- ❌ 同时改 L 和 C

- ❌ `calc(l*.5)`（缺少空格）

---

## 🎯 适用场景

- UI / Web / App

- Design System

- Design Token

- 暗色模式

- Tailwind 颜色体系

---

## 🔗 关联笔记

- [[RGB 与设备色彩]]

- [[HSL 的局限性]]

- [[OKLab 与感知一致颜色空间]]

- [[Tailwind 色彩管理]]

---

## 🏷️ 标签

- #css #oklch #颜色管理 #design-system #ui-design
