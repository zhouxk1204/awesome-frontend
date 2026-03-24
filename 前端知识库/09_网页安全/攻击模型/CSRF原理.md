# CSRF 原理（Cross-Site Request Forgery）

## 📝 概要

CSRF 利用浏览器 **“向目标域发请求时自动携带 Cookie”** 的特性，在用户不知情的情况下，伪造用户身份完成敏感操作。

---

## 📌 核心结论（一句话）
>
> **Cookie 是否携带，只和“请求目标域”有关，与是否跨站无关。**

---

## 🧠 CSRF 成立的三要素

1. 使用 Cookie 维持登录态
2. 浏览器会自动携带 Cookie
3. 服务端未校验请求是否为“用户真实意图”

---

## 📌 CSRF 的本质

- 攻击者 **无法读取 Cookie**
- 但可以 **诱导浏览器发请求**
- 浏览器会 **自动附带 Cookie**
- 服务端误以为是合法用户请求

---

## 🚨 为什么浏览器不拦？

浏览器的安全边界是：

- ✅ 禁止跨站 **读取**
- ❌ 不禁止跨站 **发送**

➡️ 发请求是“合法行为”，浏览器无法判断意图。

---

## 💥 CSRF 攻击完整流程（简述）

```

用户已登录 bank.com（Cookie 已存在）  
↓  
用户访问 evil.com  
↓  
evil.com 构造请求指向 bank.com  
↓  
浏览器自动携带 bank.com 的 Cookie  
↓  
bank.com 误判为合法请求

````

---

## 🧨 常见 CSRF 触发方式（重点）

### 方式一：URL 型请求（IMG / SCRIPT / LINK）

#### 示例

```html
<img src="https://bank.com/transfer?to=attacker&amount=10000">
````

#### 特点

- 请求方式：`GET`

- Content-Type：**无（浏览器自动）**

- 不触发 CORS 预检

- 浏览器一定会发

- Cookie 一定会带（SameSite=None/Lax）

#### 能力范围

- ❌ 不能发 POST

- ❌ 不能自定义 Header

- ❌ 只能用于「危险 GET 接口」

#### 典型攻击

- 余额变更

- 状态切换

- 错误设计的 GET 写接口

---

### 方式二：隐藏表单自动提交（最危险）

#### 示例

```html
<form action="https://bank.com/transfer" method="POST">
  <input name="to" value="attacker">
  <input name="amount" value="10000">
</form>
<script>
  document.forms[0].submit();
</script>
```

---

## 🔍 表单 CSRF 的关键细节（非常重要）

### ✅ Content-Type 是什么？

**浏览器原生表单只能是以下三种之一：**

|Content-Type|是否常见|说明|
|---|---|---|
|application/x-www-form-urlencoded|⭐⭐⭐⭐⭐|默认|
|multipart/form-data|⭐⭐⭐⭐|文件上传|
|text/plain|⭐|极少|

👉 **都属于 Simple Request**

---

### ✅ 为什么不会触发 CORS 预检？

因为满足：

- 方法：GET / POST

- Header：浏览器默认

- Content-Type：Simple 类型

➡️ **浏览器直接发，不问服务器**

---

### ✅ Cookie 会不会带？

✔️ 会  
只要目标是 `bank.com`，Cookie 就会自动附带

---

### ❌ 表单 CSRF 的限制

- 不能设置自定义 Header

- 不能设置 JSON

- 不能携带 CSRF Token（除非写死）

---

## 🆚 两种 CSRF 方式对比（重点表）

|对比项|IMG / URL|隐藏表单|
|---|---|---|
|HTTP 方法|GET|POST|
|Content-Type|无|表单类型|
|是否预检|否|否|
|是否带 Cookie|是|是|
|是否可写操作|❌（设计错误才行）|✅|
|危险等级|⭐⭐|⭐⭐⭐⭐⭐|

---

## ❗ 为什么 JSON 请求几乎不会被 CSRF？

```js
fetch("https://bank.com/api/transfer", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    "X-CSRF-Token": "xxx"
  },
  credentials: "include",
  body: JSON.stringify(data)
});
```

### 原因

- `application/json` ❌ 非 Simple Request

- 会触发 **CORS 预检**

- 攻击者域名拿不到预检通过

- 无法发送请求

➡️ **JSON + 自定义 Header = 天然 CSRF 防线**

---

## 🛡️ CSRF 防御核心思想

> **攻击者可以让你“发请求”，但必须让他“伪造不了请求”。**

---

## 🛡️ 常见防御方式对比

|方式|原理|评价|
|---|---|---|
|CSRF Token（Header）|攻击者无法读取 Token|⭐⭐⭐⭐⭐|
|SameSite Cookie|浏览器限制跨站携带|⭐⭐⭐⭐|
|Origin / Referer 校验|校验来源|⭐⭐⭐|
|仅 JSON + Header|利用预检机制|⭐⭐⭐⭐|

---

## ⚠️ 常见误区

- ❌ 跨站请求不会携带 Cookie

- ❌ CSRF 只能 GET

- ❌ SameSite 可以完全替代 Token

---

## 🎯 适用场景

- Cookie 登录系统

- 银行 / 金融 / 后台管理

- 前后端分离架构

- Web 安全面试

---

## 🔗 关联笔记

- [[Fetch credentials 参数]]

- [[SameSite Cookie 详解]]

- [[XSS vs CSRF 边界]]

- [[浏览器安全模型]]

---

## 🏷️ 标签

- #security #csrf #cookie #browser #cors
