# Blob URL 原理解析

## 📝 概要

Blob URL 是一种指向浏览器内存中二进制数据的临时地址，用于在前端展示或下载数据，而无需依赖远程服务器。

---

## 📌 核心知识点

- **Blob 对象**表示浏览器内存中的类文件数据（不可变）

- **URL.createObjectURL(blob)** 用于创建“指向该 Blob 的本地 URL”

- **该 URL 不存在于网络中，只存在于浏览器内部**

- **必须使用 URL.revokeObjectURL(url)** 手动释放内存（避免内存泄漏）

---

## 💻 示例代码

### ① 创建 Blob URL

```js
const blob = new Blob(["Hello Blob URL"], { type: "text/plain" });
const url = URL.createObjectURL(blob);

console.log(url); 
// 例如 blob:https://example.com/7d93-43b0-88e2
```

### ② 使用 Blob URL 下载文件

```js
const blob = new Blob(["测试内容"], { type: "text/plain" });
const url = URL.createObjectURL(blob);

const a = document.createElement("a");
a.href = url;
a.download = "demo.txt";
a.click();

URL.revokeObjectURL(url);
```

### ③ 将图片 Blob 显示为 img

```js
const imgBlob = await fetch("image.png").then(r => r.blob());
const imgURL = URL.createObjectURL(imgBlob);

document.querySelector("img").src = imgURL;
```

---

## ⚠️ 常见误区

### ❌ 误区 1：Blob URL 是真实网络 URL

Blob URL 不存在于互联网，也无法被别人访问。  
属于浏览器内部对象的“引用地址”。

### ❌ 误区 2：不用 revokeObjectURL 也没问题

实际上：

- Blob URL 占用内存

- 长时间不释放会导致内存泄漏（尤其是大量文件预览时）

---

## 🎯 适用场景

- 下载后端返回的二进制数据（PDF、Excel、图片…）

- 下载前端生成的内容（Canvas 图片、文本、JSON）

- 文件预览（图片预览、PDF 预览）

- 避免必须将文件上传到服务器才能展示

---

## 🔗 关联笔记

- [[前端文件下载的N种姿势]]
- [[Blob_vs_File_区别解析]]

---

## 🏷️ 标签

- #JavaScript #Blob #URL #浏览器API #前端下载
