# XSS vs CSRF vs SameSite vs HttpOnly  

## 攻击 / 防御对照图（总览）

## 📝 概要

XSS、CSRF 是两种**完全不同方向**的攻击；  
HttpOnly、SameSite 是针对 **“浏览器自动行为”** 的防御手段。  
它们**互相配合，而不是互相替代**。

---

## 🧠 一句话全局认知

> **XSS 是“偷执行权”**  
> **CSRF 是“偷请求权”**  
> **HttpOnly 防偷看**  
> **SameSite 防乱带**

---

## 🧭 攻击 & 防御全景时序图（文字版）

```text
┌────────────┐
│   攻击者   │
└─────┬──────┘
      │
      │（1）构造输入 / 链接 / 页面
      ▼
┌──────────────────────────────┐
│           浏览器               │
│                              │
│  ┌───────────────┐           │
│  │ Cookie 存储区 │◄──────┐   │
│  └───────────────┘       │   │
│           ▲               │   │
│           │ 自动携带       │   │
│           │               │   │
│   ┌───────┴────────┐      │   │
│   │ 目标站点页面   │──────┘   │
│   │（bank.com）    │          │
│   └───────┬────────┘          │
│           │                   │
│           ▼                   │
│   JS 执行环境（是否被污染？） │
└──────────────────────────────┘
````

---

## 🔥 攻击模型对照表（核心）

|维度|XSS|CSRF|
|---|---|---|
|攻击目标|JS 执行环境|请求发送机制|
|利用点|浏览器信任脚本|浏览器自动带 Cookie|
|是否跨站|不一定|**一定跨站**|
|是否需要用户操作|有时|通常不需要|
|是否能读 Cookie|可以（非 HttpOnly）|❌ 不行|
|是否能带 Cookie|✅|✅|
|危害级别|极高|高|

---

## 🧨 XSS 攻击模型（简化）

```text
攻击者
  ↓ 注入恶意 JS
站点页面
  ↓ 浏览器信任
JS 执行
  ↓
读取 Cookie / 发请求 / 劫持账号
```

### 🛡️ 关键防御

- 输出编码

- CSP

- HttpOnly Cookie

---

## 🧨 CSRF 攻击模型（关键理解）

```text
攻击者页面 (evil.com)
  ↓ 自动提交请求
浏览器
  ↓ 自动携带 Cookie
目标站点 (bank.com)
  ↓
误以为是用户操作
```

⚠️ **重点**

> 是否携带 Cookie，只看「请求目标域」，**与是否跨站无关**

---

## 🍪 HttpOnly 的定位

### 它防什么？

```text
JS ❌ document.cookie
```

➡️ 防止 **XSS 直接偷会话**

### 它防不了什么？

- CSRF

- XSS 发请求

📌 **定位**

> HttpOnly 是 **“降低 XSS 后果”**，不是消灭 XSS

---

## 🍪 SameSite 的定位

### SameSite=Strict

```text
跨站请求 ❌ 不带 Cookie
```

### SameSite=Lax

```text
跨站 GET（导航）✅
跨站 POST ❌
```

### SameSite=None

```text
全部跨站请求 ✅（必须 Secure）
```

📌 **定位**

> SameSite 是 **“限制 Cookie 自动外借”**

---

## 🧩 SameSite vs CSRF 的关系

|SameSite|CSRF 风险|
|---|---|
|Strict|几乎 0|
|Lax|中（可被 GET 利用）|
|None|高（必须配合 CSRF Token）|

---

## 🔐 HttpOnly + SameSite 联合效果

|攻击类型|是否防住|
|---|---|
|XSS 偷 Cookie|✅（HttpOnly）|
|XSS 发请求|❌|
|CSRF 表单|✅（SameSite=Lax/Strict）|
|CSRF 图片|✅|
|CSRF fetch|取决于 SameSite|

---

## 🧠 四者关系总结（非常重要）

```text
XSS      → 执行权问题
CSRF     → 请求权问题
HttpOnly → 看不看得到 Cookie
SameSite → 带不带 Cookie
```

> **没有任何一个机制是“万能的”**  
> **它们是分工协作的**

---

## 🎯 推荐安全组合（现代 Web）

```text
Cookie:
  HttpOnly
  Secure
  SameSite=Lax

接口:
  CSRF Token（Header）

前端:
  不用 innerHTML
  开 CSP
```

---

## 🔗 关联笔记

- [[XSS攻击模型]]

- [[CSRF原理]]

- [[HttpOnly Cookie]]

- [[SameSite Cookie详解]]

---

## 🏷️ 标签

- #security #xss #csrf #cookie #browser #web-security
