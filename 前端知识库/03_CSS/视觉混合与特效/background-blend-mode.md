# background-blend-mode

## 📝 概要

`background-blend-mode` 用于**控制同一个元素的多个背景图层之间的混合方式**。  
它只作用于 `background-image / background-color / gradient` 之间，**不会影响元素内容（文字、子元素）**。

---

## 📌 核心知识点

- 只对 **同一元素的多个 background 层** 生效
- 不影响文字、子元素
- 类似 Photoshop 的「背景图层混合」
- 必须有 **两个及以上背景层**
- 背景顺序：**后写的在最上层**
- 常与图片 + 颜色 / 渐变搭配

支持的模式：
`multiply / screen / overlay / difference / exclusion / lighten / darken`

---

## 💻 示例代码（Tailwind）

### 1️⃣ 图片 + 半透明颜色遮罩（最常用）

```html
<div
  class="h-64
         bg-[url('/photo.jpg')]
         bg-black/50
         bg-blend-multiply
         bg-cover bg-center">
</div>
````

📌 用途：Banner、卡片背景压暗

---

### 2️⃣ 图片 + 渐变叠加（高级视觉）

```html
<div
  class="h-64
         bg-[linear-gradient(115deg,#4F48E7,#8A43E1)]
         bg-[url('/photo.jpg')]
         bg-blend-overlay
         bg-cover bg-center">
</div>
```

📌 用途：Hero 区域、品牌主视觉

---

### 3️⃣ 发光 / 高亮背景（screen）

```html
<div
  class="h-64
         bg-[url('/dark-bg.jpg')]
         bg-purple-500/60
         bg-blend-screen
         bg-cover bg-center">
</div>
```

📌 用途：霓虹、科技感背景

---

### 4️⃣ 纹理叠加（texture overlay）

```html
<div
  class="h-64
         bg-[url('/texture.png')]
         bg-gray-300
         bg-blend-overlay
         bg-cover">
</div>
```

📌 用途：纸张感、噪点质感

---

### 5️⃣ 自动反色背景（difference）

```html
<div
  class="h-64
         bg-[url('/pattern.png')]
         bg-white
         bg-blend-difference
         bg-cover">
</div>
```

📌 用途：艺术 / 实验性页面

---

### 6️⃣ 柔和反色（exclusion）

```html
<div
  class="h-64
         bg-[url('/pattern.png')]
         bg-white
         bg-blend-exclusion
         bg-cover">
</div>
```

📌 用途：低对比度反转效果

---

### 7️⃣ 取亮（lighten）

```html
<div
  class="h-64
         bg-[url('/photo.jpg')]
         bg-yellow-300
         bg-blend-lighten
         bg-cover bg-center">
</div>
```

📌 用途：高亮区域强调

---

### 8️⃣ 取暗（darken）

```html
<div
  class="h-64
         bg-[url('/photo.jpg')]
         bg-blue-900
         bg-blend-darken
         bg-cover bg-center">
</div>
```

📌 用途：暗色主题 / 夜间风格

---

### 9️⃣ 多背景层（图片 + 渐变 + 颜色）

```html
<div
  class="h-64
         bg-[linear-gradient(115deg,#4F48E7,#8A43E1)]
         bg-[url('/photo.jpg')]
         bg-black/40
         bg-blend-overlay
         bg-cover bg-center">
</div>
```

📌 用途：复杂视觉合成（品牌页）

---

## ⚠️ 常见误区

- ❌ 只设置一个背景却期待混合

- ❌ 以为能影响文字颜色

- ❌ 忘记背景层顺序

- ❌ 与 `mix-blend-mode` 场景混淆

---

## 🎯 适用场景

- Banner / Hero 背景设计

- 图片暗化但不影响文字

- 统一品牌色调

- 渐变 + 图片叠加

- 设计系统背景方案

---

## 🔍 与 mix-blend-mode 对比

|项目|background-blend-mode|mix-blend-mode|
|---|---|---|
|作用对象|同一元素背景|元素与背景|
|是否影响内容|❌|✅|
|推荐程度|⭐⭐⭐⭐⭐|⭐⭐⭐|

---

## 🔗 关联笔记

- [[mix-blend-mode]]

- [[Tailwind 背景技巧]]

- [[CSS 渐变]]

- [[高级视觉效果]]

---

## 🏷️ 标签

- #tailwindcss #css #background-blend-mode #视觉效果
