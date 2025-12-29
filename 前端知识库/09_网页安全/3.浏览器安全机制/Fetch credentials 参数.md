# Fetch credentials 参数

## 📝 概要

`credentials`（fetch）和 `withCredentials`（axios）用于控制 **请求是否自动携带 Cookie / 认证信息**，是登录态维持、跨域请求和 CSRF 防护的关键配置。

---

## 📌 核心知识点

- fetch 使用 `credentials`
- axios 使用 `withCredentials`
- 跨域携带 Cookie：**前端 + 后端 CORS 都必须配置**
- Cookie 自动发送 ≠ 前端可读取 Cookie

---

## 💻 示例代码

### ✅ fetch 配置方式

```js
// 默认：same-origin（同源才带 Cookie）
fetch('/api/user');

// 明确同源
fetch('/api/user', {
  credentials: 'same-origin'
});

// 跨域并携带 Cookie（最常用）
fetch('https://api.example.com/user', {
  method: 'GET',
  credentials: 'include'
});

// 明确不携带 Cookie
fetch('/api/user', {
  credentials: 'omit'
});
````

#### fetch 的 credentials 取值说明

|值|行为|
|---|---|
|`same-origin`|仅同源请求携带 Cookie（默认）|
|`include`|同源 + 跨域都携带 Cookie|
|`omit`|完全不携带 Cookie|

---

### ✅ axios 配置方式

#### 单个请求

```js
axios.get('/api/user', {
  withCredentials: true
});
```

#### 全局默认配置（推荐）

```js
axios.defaults.withCredentials = true;
```

#### axios 实例配置

```js
const api = axios.create({
  baseURL: 'https://api.example.com',
  withCredentials: true
});

api.get('/user');
```

📌 **注意**

- axios **没有** `same-origin / omit` 选项

- `withCredentials: true` ≈ fetch 的 `credentials: 'include'`

---

## ⚠️ 常见误区

- 误区 1：只在前端开启 `withCredentials`

```
❌ 少了后端：
Access-Control-Allow-Credentials: true
```

- 误区 2：认为 Cookie 能跨域携带但无法控制  
    → 实际上 **fetch / axios 默认都不会跨域带 Cookie**

- 误区 3：Cookie 自动携带 = 不安全  
    → 风险来自 **CSRF**，不是 Cookie 本身

---

## 🎯 适用场景

- 前后端分离（不同域）

- HttpOnly Cookie 登录态

- CSRF Token 校验（Cookie + Header 双重验证）

---

## 🔗 关联笔记

- [[CSRF 原理]]

- [[CORS 跨域机制]]

- [[HttpOnly Cookie]]

- [[SameSite Cookie 详解]]

---

## 🏷️ 标签

- #javascript #fetch #axios #http #credentials #安全 #CSRF #CORS
