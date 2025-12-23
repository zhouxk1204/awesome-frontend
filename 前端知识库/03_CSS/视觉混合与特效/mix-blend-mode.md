# mix-blend-mode

## 📝 概要

`mix-blend-mode` 用于定义**元素内容与其背后背景内容的颜色混合方式**，类似 Photoshop 的图层混合模式，常用于图片叠加、自动反色文字、高光与质感效果等视觉设计。

---

## 📌 核心知识点

- `mix-blend-mode` **始终作用于当前元素与其背景**
- **不能控制透明度**，只影响颜色混合计算
- 必须有可见背景，否则效果不明显
- 常与 `opacity` / `rgba` 一起使用
- 适用于文字、图片、背景色等元素

常见模式：

- `multiply`
- `screen`
- `overlay`
- `difference`
- `exclusion`
- `lighten / darken`

---

## 💻 示例代码

### 1️⃣ 正片叠底（multiply）— 图片压暗

```html
<div class="relative">
  <img src="/photo.jpg" class="w-full" />
  <div class="absolute inset-0 bg-black/40 mix-blend-multiply"></div>
</div>
```

---

### 2️⃣ 滤色（screen）— 发光效果

```html
<div class="bg-black p-10">
  <div class="bg-purple-500/60 mix-blend-screen p-6 text-white">
    Glow Effect
  </div>
</div>
```

---

### 3️⃣ 叠加（overlay）— 提升对比度

```html
<div class="bg-gray-200 p-10">
  <div class="bg-[url('/texture.png')] bg-cover mix-blend-overlay opacity-50 p-6">
    Texture Overlay
  </div>
</div>
```

---

### 4️⃣ 差值（difference）— 自动反色文字

```html
<div class="bg-gradient-to-r from-blue-500 to-purple-600 p-10">
  <span class="text-white mix-blend-difference text-xl font-bold">
    Auto Invert Text
  </span>
</div>
```

---

### 5️⃣ 排除（exclusion）— 柔和反色

```html
<div class="bg-orange-400 p-10">
  <span class="text-white mix-blend-exclusion text-xl">
    Soft Invert
  </span>
</div>
```

---

### 6️⃣ 取亮 / 取暗（lighten / darken）

```html
<div class="flex gap-6">
  <div class="bg-green-500 p-6 text-white mix-blend-lighten">
    Lighten
  </div>
  <div class="bg-green-500 p-6 text-white mix-blend-darken">
    Darken
  </div>
</div>
```

---

## ⚠️ 常见误区

- ❌ 认为 `mix-blend-mode` 可以控制透明度

- ❌ 背景是透明时仍期待混合效果

- ❌ 元素被放入 `isolation` 环境导致混合失效

- ❌ 未在 Safari / iOS 上测试

---

## 🎯 适用场景

- 图片暗化 / 遮罩层

- 渐变或纹理叠加

- 自动反色文字（适配任意背景）

- Hover 高光 / 特效层

- 高级 UI 视觉设计

---

## 🔗 关联笔记

- [[background-blend-mode]]

---

## 🏷️ 标签

- #tailwindcss #css #视觉效果 #mix-blend-mode #前端进阶
