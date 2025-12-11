# 现代视口单位：`dvh` / `svh` / `lvh` 全解析（移动端 `vh` 问题的根本解决方案）

## 📝 概要

为了解决移动端 `100vh` 因地址栏/工具栏与软键盘导致的不稳定问题，规范新增了 `lvh`（Large Viewport）、`svh`（Small Viewport）与 `dvh`（Dynamic Viewport）三类视口单位，推荐在移动端用 `dvh`（配合降级方案）替代传统 `vh`。

## 📌 核心知识点

- `vh/vw/vmin/vmax` 在移动端可能不稳定：浏览器地址栏、工具栏及软键盘会影响可视高度，而 `vh` 的计算并不总是动态更新。

- **`lvh`（Large Viewport）**：假定 UA 接口（地址栏/工具栏）全部收起，代表**最大视口**。

- **`svh`（Small Viewport）**：假定 UA 接口全部展开，代表**最小视口**（稳定、不跳动）。

- **`dvh`（Dynamic Viewport）**：随 UA 接口展开/收起**动态变化**：展开时等于 `svh`，收起时等于 `lvh`，是最佳实践（但浏览器实现存在差异）。

- 浏览器兼容性：Chrome（Android/PC）与 Firefox 对 `dvh` 支持较好；iOS Safari 对 `dvh` 的实现并不完全动态（常见行为：初始 `dvh ≈ svh`，只有用户交互后才更新）。

- 实战补救：在需要稳健的全屏布局时，可用 `100dvh` + `100svh` 回退，或使用 JS 动态计算 `--vh` CSS 变量（`window.innerHeight`/`visualViewport`）以兼容 iOS Safari。

## 💻 示例代码

### CSS：推荐用法 + 回退

```css
/* 优先使用 dvh，兼容时可自动生效；对 Safari 做回退到 svh */
.fullscreen {
  height: 100dvh;
  height: 100svh; /* Safari fallback */
  /* 进一步保险：JS 动态变量 fallback（见下） */
}
```

### JS：industry-standard 动态 vh 回退（兼容 iOS Safari）

```js
// 计算并设置 --vh，兼容 iOS Safari 的动态高度问题
function setVhVar() {
  const vh = window.innerHeight * 0.01;
  document.documentElement.style.setProperty('--vh', `${vh}px`);
}
setVhVar();
window.addEventListener('resize', setVhVar);
window.visualViewport && visualViewport.addEventListener('resize', setVhVar);

// CSS 使用
// .full { height: calc(var(--vh) * 100); }
```

### Mermaid（简要示意 d/ l/ s 视口关系）

```mermaid
flowchart TB
  A[svh (small) = 工具栏展开时的高度]
  B[dvh (dynamic) = 随 UI 变化（展开→svh, 收起→lvh）]
  C[lvh (large) = 工具栏收起时的高度]
  A --> B
  C --> B
```

## ⚠️ 常见误区

- 误区：`100vh` 在移动端总是“屏幕高度” —— 错误，浏览器 UI（地址栏/工具栏）会影响其值并导致布局异常。

- 误区：`dvh` 在所有浏览器都立即动态更新 —— 部分浏览器（尤其 iOS Safari）暂时未完全实现动态更新，`100dvh` 初始可能等同 `100svh`。

- 误区：用 `dvh` 就能强制浏览器收起地址栏 —— `dvh` **反应**视口变化，但**不能触发或控制**浏览器 UI 的收起/展开（用户交互控制）。

- 误区：不用回退即可覆盖所有设备 —— 仍需对旧浏览器或 Safari 做回退（`svh`、JS 变量或混合策略）。

## 🎯 适用场景

- `100dvh`：适用于大多数移动端全屏布局（hero、模态、全屏视频/游戏），前提为目标浏览器已支持或能接受回退策略。

- `100svh`：适用于需要视觉稳定、不随地址栏变化跳动的场景（聊天、表单、输入密集页面）。

- `100lvh`：适用于沉浸式体验或希望占满最大可视区域的场景（PWA、全屏游戏），但在初始加载不一定可见，取决于浏览器 UI 状态。

- JS fallback（`--vh`）：当需兼容 iOS Safari 的真实动态高度或避免键盘/地址栏引起的布局错位时使用（最稳健）。

## 🔗 关联笔记

- [[为什么 iOS Safari上看到100dvh与100svh一样？]]

## 🏷️ 标签

- #css #viewport #mobile #前端工程 #dvh #svh #lvh
