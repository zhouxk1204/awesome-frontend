# SameSite Cookie 详解

## 📝 概要

`SameSite` 是 Cookie 的安全属性，用于 **限制跨站请求时浏览器是否携带 Cookie**，主要用于缓解 CSRF 攻击。

---

## 📌 一句话结论
>
> **SameSite 决定：跨站请求时，Cookie“带不带”。**

---

## 🧠 Cookie 的自动携带规则（前置认知）

浏览器判断是否携带 Cookie，只看：

- 请求 **目标域**
- Cookie 的 **SameSite 属性**

❌ 不看请求来源是否恶意  
❌ 不看是不是用户主动操作

---

## 🧩 SameSite 的三个取值

```http
Set-Cookie: session=xxx; SameSite=Strict
Set-Cookie: session=xxx; SameSite=Lax
Set-Cookie: session=xxx; SameSite=None; Secure
````

---

## 1️⃣ SameSite=Strict（最严格）

### 行为规则

- **任何跨站请求都不携带 Cookie**

- 只有「当前页面域名 == Cookie 域名」才携带

### 示例

```text
用户在 evil.com 页面
↓
访问 https://bank.com
↓
❌ 不携带 bank.com Cookie
```

### 优点

- CSRF 防护最强

- 实现简单

### 缺点

- 用户体验差

- 登录态在外链、第三方跳转中丢失

### 适用场景

- 内部后台

- 管理系统

- 不需要第三方跳转的系统

---

## 2️⃣ SameSite=Lax（默认 & 平衡）

### 行为规则

- **跨站“顶级导航 GET”请求会携带 Cookie**

- 其余跨站请求不携带

### 能携带的场景

```text
用户点击链接跳转（GET）
evil.com → bank.com
✔️ Cookie 会带
```

### 不携带的场景

```html
<img src="https://bank.com/transfer">
<form method="POST" action="https://bank.com/transfer">
```

➡️ ❌ Cookie 不带

---

### 为什么设计 Lax？

- 保证基本 CSRF 防护

- 同时保证「点击链接还能保持登录态」

---

### 优点

- 防御大部分 CSRF

- 用户体验好

- **现代浏览器默认值**

### 缺点

- 顶级 GET 请求仍可能被滥用（若 GET 有副作用）

---

## 3️⃣ SameSite=None（完全放开）

### 行为规则

- **所有跨站请求都会携带 Cookie**

- 必须同时设置 `Secure`

```http
Set-Cookie: session=xxx; SameSite=None; Secure
```

❌ 否则 Cookie 会被浏览器拒绝

---

### 使用场景

- 前后端分离（跨域）

- 第三方登录

- iframe 嵌入

- SSO / 微前端

---

### 风险

- 完全暴露 CSRF 风险

- **必须配合 CSRF Token**

---

## 🧨 SameSite 与 CSRF 的关系（重点）

|SameSite|CSRF 风险|说明|
|---|---|---|
|Strict|极低|几乎杜绝|
|Lax|中|防 POST / 表单|
|None|高|必须额外防护|

---

## 🧠 常见误解澄清

### ❌ SameSite 可以完全防 CSRF

> 错

- Lax 仍允许 GET

- None 完全不防

- Strict 有 UX 问题

---

### ❌ SameSite 和 CORS 是一回事

> 错

|项目|SameSite|CORS|
|---|---|---|
|控制点|Cookie 携带|JS 访问|
|生效层|浏览器网络层|JS 层|
|防御|CSRF|数据窃取|

---

## 🛡️ 推荐安全组合（实践）

### ⭐ 推荐配置（最常见）

```http
Set-Cookie: session=xxx;
HttpOnly;
Secure;
SameSite=Lax
```

配合：

- CSRF Token（Header）

- JSON 接口

- 禁止 GET 写操作

---

### 前后端分离（跨域）

```http
Set-Cookie: session=xxx;
HttpOnly;
Secure;
SameSite=None
```

前端：

```js
fetch(url, {
  credentials: 'include'
})
```

后端：

- 校验 CSRF Token

- 校验 Origin

---

## ⚠️ 浏览器默认行为（重要）

|浏览器|默认 SameSite|
|---|---|
|Chrome 80+|Lax|
|Edge|Lax|
|Firefox|Lax|
|Safari|Lax（策略更严格）|

---

## 🎯 适用场景总结

|场景|推荐|
|---|---|
|普通 Web|Lax|
|后台系统|Strict|
|跨域登录|None + Token|

---

## 🔗 关联笔记

- [[CSRF 原理]]

- [[HttpOnly Cookie]]

- [[Fetch credentials 参数]]

- [[CORS 与安全边界]]

---

## 🏷️ 标签

- #security #cookie #samesite #csrf #browser
