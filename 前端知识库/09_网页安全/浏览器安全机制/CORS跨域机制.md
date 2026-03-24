# CORS 跨域机制

## 📝 概要

CORS（Cross-Origin Resource Sharing）是浏览器在同源策略（SOP）之上，引入的一套**“受控跨域访问”授权机制**，用于决定：跨域请求发出后，响应是否允许前端 JavaScript 读取。

## 📌 核心知识点

- **跨域请求本身是可以发送的**，CORS 只控制「能不能读响应」
- CORS 是 **浏览器行为**，不是前端或后端单方面决定
- 服务端通过响应头明确授权哪些源可以访问资源
- Cookie / 凭证跨域必须额外开启 credentials 支持

## 💻 示例代码

### 服务端响应头示例

```http
Access-Control-Allow-Origin: https://frontend.example.com
Access-Control-Allow-Methods: GET, POST, PUT
Access-Control-Allow-Headers: Content-Type, Authorization
Access-Control-Allow-Credentials: true
````

### 前端 fetch 示例

```js
fetch("https://api.example.com/user", {
  method: "GET",
  credentials: "include" // 允许携带 Cookie
});
```

---

## 📌 简单请求 vs 非简单请求

### ✅ 简单请求（不会触发 Preflight）

满足 **全部** 条件：

- Method：GET / POST / HEAD

- Content-Type：

  - text/plain

  - application/x-www-form-urlencoded

  - multipart/form-data

- 不包含自定义 Header

👉 浏览器直接发请求

---

### 🚨 非简单请求（触发 Preflight）

例如：

```http
POST /api
Content-Type: application/json
Authorization: Bearer xxx
```

浏览器流程：

```text
OPTIONS（预检）
  ↓
服务器允许？
  ↓
正式请求
```

---

## ⚠️ 常见误区

- ❌ 认为「跨域请求发不出去」

- ❌ 以为 CORS 能防 CSRF（不能）

- ❌ 使用 `Access-Control-Allow-Origin: *` 同时开启 credentials

- ❌ 忽略 OPTIONS 请求导致接口 403

## 🎯 适用场景

- 前后端分离架构

- 跨域登录态（Cookie Session）

- API 网关 / BFF 架构

- CSRF / Cookie 行为分析

## 🔗 关联笔记

- [[同源策略（SOP）详解]]

- [[Fetch credentials 参数]]

- [[CSRF 原理]]

- [[SameSite Cookie 详解]]

## 🏷️ 标签

- #cors #跨域 #浏览器安全 #http
