# 为什么在 iOS Safari 上看到 `100dvh` 与 `100svh` 一样？

## 📝 概要

iOS Safari 对 `dvh` 的实现并不完全符合规范，初始渲染阶段通常把 `100dvh` 设为与 `100svh` 相同，因此你会看到两者“没有差别”。

## 📌 核心知识点

- `dvh` 按规范应随地址栏 UI 展开/收起动态变化（展开=svh，收起=lvh）。

- **iOS Safari 并未完全实现该行为**：

  - 初次加载页面时，iOS Safari 仍按 **Small Viewport**（svh）作为 dvh 的值。

  - 只有在用户滚动、触摸、打开/关闭键盘后，`dvh` 才会被更新。

- 这导致开发者在初始页面看到：  
    **100dvh ≈ 100svh，而非预期的 100lvh 或动态区域高度。**

## 💻 示例代码

### 实际测试中你会看到

```css
.box {
  height: 100dvh; /* iOS Safari 初始：等同 100svh */
  background: lightblue;
}
```

### 视觉表现（iOS Safari 初始加载）

```
可视区（small viewport）
------------------------
|                      |
|      100dvh          |  <- 实际与 svh 相同
|                      |
------------------------
地址栏仍然占空间
```

### 只有在交互后才可能变成这样

```
可视区（large viewport）
------------------------
|                      |
|      100dvh          |  <- dvh 更新，接近 lvh
|                      |
------------------------
地址栏折叠
```

## ⚠️ 常见误区

- ❌ **误区：`dvh` 可以强制收起地址栏**

  - 不能，只能“反映”浏览器 UI 的状态，不会“控制”浏览器 UI。

- ❌ **误区：所有浏览器的 `dvh` 都是实时动态的**

  - 真实情况：iOS Safari 动态更新滞后，初始为 `svh`。

- ❌ **误区：`dvh` 不需要回退策略**

  - 在 iOS Safari 上，使用 `svh`、JS fallback 是必要的。

## 🎯 适用场景

- 分析移动端布局异常时用于判断：为什么“全屏布局没有真正全屏”。

- 需要决定是否使用回退策略，例如：

```css
height: 100dvh;
height: 100svh; /* iOS Safari fallback */
```

或使用 JS：

```js
--vh = window.innerHeight * 0.01
```

## 🔗 关联笔记

- [[现代视口单位dvh_svh_lvh 全解析_移动端 vh 问题的根本解决方案]]

## 🏷️ 标签

- #css #viewport #mobile #safari #dvh
