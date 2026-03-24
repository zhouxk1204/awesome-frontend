# 前端文件下载的 N 种姿势（从简单到高级）

## 📝 概要

前端文件下载常见方式包括 `<a download>`、`window.open()`、Fetch/XHR + Blob 等。每种方式都有适用场景。理解其原理是实现安全、可控、可授权文件下载的关键。

---

## 📌 核心知识点

- 浏览器下载行为由**超链接**、**导航**或**响应头**触发。

- 浏览器无法直接“保存文件”，只能通过以下方式间接实现：

  - 导航到文件地址 (`location.href`)

  - 触发下载 (`download` 属性)

  - 解析二进制数据并创建 Blob 链接（Object URL）

- 需要**权限控制**时，应使用 Fetch/XHR 搭配 Blob 下载。

---

## 📊 图解（简化）

### 1️⃣ `<a download>` 工作机制图

```
点击 <a download> → 浏览器不导航 → 直接下载资源 → 文件名由 download 决定
```

### 2️⃣ `window.open()` / `location.href`

```
页面导航 → 浏览器收到 Content-Disposition: attachment → 触发下载
```

### 3️⃣ Fetch / XHR + Blob

```
Fetch(Request) → Response(ArrayBuffer) → Blob → Object URL → 模拟点击 <a> → 下载 → revoke()
```

### 4️⃣ 权限下载

```
Fetch(Request with Authorization) → 获取 Blob → 下载
```

---

## 💻 示例代码

### 1. `<a download>`（最简单）

```html
<a href="/static/report.pdf" download="报告.pdf">下载报告</a>
```

⚠️ 该方式不可附带认证头，且可能受跨域限制。

---

### 2. `window.open()` 或 `location.href`

```js
window.open("https://example.com/file/export?id=123");
```

依赖服务端设置：

```
Content-Disposition: attachment; filename="yourfile.xlsx"
```

---

### 3. Fetch + Blob + ObjectURL（万能方案）

```js
async function downloadFile() {
  const res = await fetch("/api/report", {
    method: "GET"
  });
  const blob = await res.blob();

  const url = URL.createObjectURL(blob);
  const a = document.createElement("a");

  a.href = url;
  a.download = "报告.xlsx";
  a.click();

  URL.revokeObjectURL(url);
}
```

---

### ⭐ 4.（新增）**带权限的文件下载（推荐）**

用于：

- Token 认证（JWT）

- 用户登录 Cookie 会话

- 需要自定义头部（Authorization）

**🔐 Fetch 示例：**

```js
async function secureDownload() {
  const res = await fetch("/api/secure/download", {
    method: "GET",
    headers: {
      Authorization: "Bearer " + localStorage.getItem("token")
    },
    credentials: "include" // 若使用 Cookie
  });

  if (!res.ok) {
    throw new Error("下载失败");
  }

  const blob = await res.blob();
  const url = URL.createObjectURL(blob);

  const a = document.createElement("a");
  a.href = url;
  a.download = decodeURIComponent(
    res.headers.get("x-file-name") || "download.bin"
  );
  a.click();

  URL.revokeObjectURL(url);
}
```

**⭐ 推荐后端设置文件名：**

```
Content-Disposition: attachment; filename*=utf-8''报告.xlsx
X-File-Name: 报告.xlsx
```

---

## ⚠️ 常见误区

- ❌ `a.download` 无法用于跨域受保护资源

- ❌ `location.href` 无法携带授权头部

- ❌ Blob URL 必须手动 `revokeObjectURL`，否则会内存泄漏

- ❌ window.open() 可能被浏览器拦截

- ❌ Fetch 默认不会附带请求 Cookie，需加 `credentials: "include"`

---

## 🎯 适用场景

|方式|场景|
|---|---|
|`<a download>`|静态文件、本地文件、不需要权限|
|`window.open()`|后端流式文件下载、跨域下载|
|Fetch/XHR+Blob|**需要控制权限、动态文件、带认证、带请求头**|
|Fetch Stream|大文件、断点、进度条下载|

---

## 🔐（新增）如何正确做“有权限的下载”

### 🔸 1. 使用需要授权的 API（例如 Bearer Token）

浏览器自身的下载导航（`href`/`open`）**无法加 header**，  
所以必须：

✔ 使用 Fetch/XHR  
✔ 带上 Authorization  
✔ 接收 Blob 再下载

---

### 🔸 2. 服务端必须允许 Token/Cookie

如果使用 Cookie 会话：

```
fetch(url, { credentials: "include" })
```

---

### 🔸 3. 文件名需要由后端返回

必须通过 headers，例如：

```
Content-Disposition: attachment; filename="xxx.xlsx"
X-File-Name: xxx.xlsx
```

---

### 🔸 4. 大文件用 Stream

Fetch + ReadableStream 实现流式读写，节省内存。

（如需，我可帮你生成“可暂停、可恢复的前端大文件下载”方案）

---

## 🔗 关联笔记

- [[Blob URL原理解析]]

- [[Fetch_与_XHR_区别]]

- [[前端文件流处理_Fetch_ReadableStream]]

- [[Token_鉴权机制]]

- [[前端权限控制方案]]

---

## 🏷️ 标签

- #javascript #download #blob #fetch #权限控制 #前端工程化
