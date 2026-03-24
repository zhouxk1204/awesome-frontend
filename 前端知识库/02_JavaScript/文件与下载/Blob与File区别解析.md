# Blob vs File 区别解析

## 📝 概要

Blob 与 File 都是浏览器中的“类文件对象”，但 File 是带文件名和来源信息的 Blob，本质上 File 继承自 Blob。

---

## 📌 核心知识点

### ✔️ 共同点

- 都代表 **不可变的二进制数据**

- 都可作为 `<input type="file">`、`FormData`、Fetch 上传的主体

- 都具有：

  - `size`

  - `type`

  - `slice()`

  - `stream()`

  - `arrayBuffer()`

  - `text()`

### ✔️ 核心区别点

|特性|Blob|File|
|---|---|---|
|文件名|❌ 没有|✔️ 有 `name`|
|最后修改时间|❌ 没有|✔️ 有 `lastModified`|
|来源|任意内存数据|通常来自用户本地文件|
|创建方式|`new Blob()`|`new File()` 或 文件选择器|
|继承关系|**基类**|**继承自 Blob**|
|用途|通用二进制容器|表示真实文件对象|

### ✔️ 本质一句话总结

**File = Blob + 文件名(name) + 修改时间(lastModified)**

---

## 💻 示例代码

### ① Blob 创建

```js
const blob = new Blob(["Hello World"], { type: "text/plain" });
console.log(blob.size);    // 11
console.log(blob.type);    // "text/plain"
```

### ② File 创建

```js
const file = new File(["Hello World"], "hello.txt", {
  type: "text/plain",
  lastModified: Date.now()
});

console.log(file.name);          // "hello.txt"
console.log(file.lastModified);  // 时间戳
```

### ③ Blob 转 File

```js
const blob = new Blob(["data"], { type: "text/plain" });

const file = new File([blob], "data.txt", {
  type: blob.type,
  lastModified: Date.now()
});
```

### ④ File 转 Blob（其实 File 本身就是 Blob）

```js
const file = document.querySelector("input").files[0];
const blob = file.slice(0, file.size);
```

---

## ⚠️ 常见误区

### ❌ 误区 1：Blob 和 File 是两个完全不同的对象

→ **错误**。  
File 实际上 _继承自_ Blob，是“带文件名的 Blob”。

### ❌ 误区 2：Blob 不能上传

→ **错误**。  
Blob、File 都能通过 Fetch 或 FormData 上传。

### ❌ 误区 3：File 只能从 `<input>` 得到

→ **错误**。  
你可以用 `new File([...])` 人工创建 File 对象（如 mock 文件、前端生成文件）。

---

## 🎯 适用场景

### ✔️ 使用 Blob 的场景

- 接收 API 返回的二进制数据

- 生成动态文件（JSON、CSV、Canvas 图像）

- Blob URL 预览 PDF、图片

- 作为 Web Worker 或 WebSocket 消息体

### ✔️ 使用 File 的场景

- 用户上传本地文件

- 保留文件名，用于：

  - 上传时的文件名展示

  - 下载保留原名

  - 业务层文件类型判断

- 文件选择器或 drag & drop 文件处理

---

## 🔗 关联笔记

- [[Blob_URL_原理解析]]

- [[前端文件下载的N种姿势]]

---

## 🏷️ 标签

- #JavaScript #Blob #File #浏览器API #二进制数据 #前端上传
