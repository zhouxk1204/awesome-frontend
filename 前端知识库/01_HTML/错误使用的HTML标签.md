# 10 个你仍在错误使用的 HTML 标签

## 📝 概要

HTML 标签不仅是结构，更是语义。误用标签会导致：

- 页面结构混乱

- 可访问性（a11y）变差

- SEO 得分下降

- 页面渲染性能变差

本卡片总结前端最常见的 **10 个 HTML 标签误用场景**，并给出正确替代方案与实战建议。

---

## 📌 核心知识点

### 1. `<b>` vs `<strong>`：视觉 vs 语义强调

- `<b>`：只有样式，没有语义

- `<strong>`：表示语义上的重点

  - 屏幕阅读器会加强语调

  - 搜索引擎识别为权重较高内容

**结论：视觉加粗 → CSS；语义强调 → strong。**

---

### 2. `<i>` vs `<em>`：斜体不是语气

- `<i>`：纯样式倾斜

- `<em>`：表达语气强调（语调会变化）

**结论：语气强调 → em。**

---

### 3. `<u>` vs `<ins>`：下划线不代表“新增内容”

- `<u>`：只是画了一条线（容易误认为链接）

- `<ins>`：表示新增内容、插入文本

**结论：样式 → CSS；语义 → ins。**

---

### 4. 滥用 `<br>`：不要用它堆空白

禁止用 `<br><br><br>` 做间距。  
`<br>` 仅适用于：

- 诗歌、歌词

- 地址换行

- 内容内部的自然换行

块间距应该使用 `margin/padding`。

---

### 5. 滥用 `<hr>` 作“视觉分割线”

`<hr>` 的语义：

- 表示“主题内容的语义分隔”

- 不是简单的设计线

想画横线 → 应使用 CSS border。

---

### 6. `<h1>~<h6>` 不是“字体大小工具”

- `<h1>` 全页面应唯一

- 按层级递进 …

- 不跳级

- 不用来凹样式

结构正确 = SEO + 可访问性更好。

---

### 7. `<li>` 不要包 `<p>`

错误：

```html
<li><p>Item</p></li>
```

正确：

```html
<li>Item</li>
```

减少无意义嵌套，让结构更干净。

---

### 8. `<img>` 必须写 alt

不写 alt 的后果：

- 搜索引擎不知道这张图是什么

- 屏幕阅读器无法朗读内容

- 图挂掉时没有替代文字

装饰性图像可写：

```html
<img src="..." alt="">
```

---

### 9. `<meta>` 标签写不全：SEO 基础直接丢分

> 以下是前端最关键的 meta 标签及其作用（已按你的要求补充）：

#### ✔ `meta name="description"`

**网页内容摘要**

- 直接影响搜索引擎展示的摘要信息

- 决定点击率（CTR）

- 帮助搜索引擎判断页面主题

示例：

```html
<meta name="description" content="Best online shop for electronics in Canada.">
```

---

#### ✔ `meta name="viewport"`

**移动端适配关键标签**

- 控制页面宽度与缩放行为

- 决定移动端是否正常显示

- 缺失会导致页面缩成一条“迷你版网页”

示例：

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

---

#### ✔ `meta charset="UTF-8"`

**页面编码方式**

- 告诉浏览器用 UTF-8 解析页面

- 避免中文字符出现乱码

- 搜索引擎读取内容更准确

示例：

```html
<meta charset="UTF-8">
```

---

#### ✔ `meta name="keywords"`（作用弱化但仍可写）

**页面关键词列表**

- 老 SEO 用法

- Google 作用已弱化，但部分搜索引擎仍参考

- 用于强化内容主题关联度

示例：

```html
<meta name="keywords" content="HTML, SEO, meta tags, web development">
```

---

**总结：meta 是 SEO 的“基础配置”，缺一项都可能让搜索引擎误解你的网页信息。**

---

### 10. `<script>` 放错位置拖慢首屏

禁止：

```html
<head>
  <script src="main.js"></script>
</head>
```

因为会阻塞渲染。

正确：

```html
<script src="main.js" defer></script>
```

- `defer`：等待 DOM 解析完成再执行（推荐）

- `async`：适合独立脚本

- 或放在 `body` 最底部

**参考：[[script标签中defer和async的区别]]**

---

## 💻 示例代码

```html
<!-- strong vs b -->
<p>This article is <strong>very important</strong> for HTML writers</p>

<!-- em vs i -->
<p>This is <em>really</em> important to note.</p>

<!-- ins vs u -->
<p><ins>Added in latest revision</ins></p>

<!-- CSS 替代 br 堆空白 -->
<p class="a">First line</p>
<p class="b">Second line</p>

<style>
  .a { margin-bottom: 40px; }
</style>

<!-- 正确标题层级 -->
<h1>Title</h1>
<h2>Section</h2>
<h3>Sub Section</h3>

<!-- 正确的图片 alt -->
<img src="cow.jpg" alt="A cow standing on a mountain">

<!-- 正确的 meta -->
<meta name="description" content="High quality e-commerce website.">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta charset="UTF-8">
<meta name="keywords" content="HTML, SEO, web development">

<!-- 正确 script -->
<script src="main.js" defer></script>
```

---

## ⚠️ 常见误区

- 用 `<b>` `<i>` `<u>` 处理样式

- 用 `<br>` 堆空白

- 滥用 `<hr>` 做视觉分割

- 用标题标签调字体

- `<li>` 套 `<p>`

- 图片不写 alt

- meta 缺失

- script 阻塞在 head

---

## 🎯 适用场景

- SEO 重要的网页

- 博客、文档、公司官网

- 高可访问性（a11y）要求

- 前端基础体系学习

---

## 🔗 关联笔记

- [[HTML_语义化最佳实践]]

- [[Web_可访问性指南]]

- [[如何正确使用_heading_标题结构]]

- [[前端_SEO_基础知识]]

---

## 🏷️ 标签

- #HTML #语义化 #SEO #可访问性 #最佳实践
