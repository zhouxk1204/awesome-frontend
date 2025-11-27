# vw 和 clamp 流体布局

## 📝 概要
在现代前端开发中，px 和 rem 的使用场景正在被大幅压缩，取而代之的是 vw 和 clamp() 为核心的流体布局方案。

## 📌 核心知识点

### 1️⃣ vw (视口宽度单位)
- 1vw = 视口宽度的 1%
- 元素尺寸会随浏览器窗口实时缩放

示例：
```css
.title {
  font-size: 5vw;
}
```

### 2️⃣ clamp(): 优雅的边界控制
- clamp(MIN, PREFERRED, MAX) 限制最小值和最大值

示例：
```css
.title {
  font-size: clamp(16px, 5vw, 32px);
}
```

### 3️⃣ px 与 rem 的适用场景
- px：用于绝对尺寸，如 border-width、box-shadow  
- rem：后台系统或文档型网站  
- vw + clamp()：To C 产品、高要求视觉体验

## ✅ 总结
- clamp() + vw 提供无级流体体验  
- 保留 px 与 rem 用于特定场景

## 🔗 关联笔记
- [[vw_布局优缺点]]

## 🏷️ 标签
#css #响应式 #流体布局 #vw #clamp #前端性能 #前端设计
