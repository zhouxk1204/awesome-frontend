# 一次跨域请求完整时序图（CORS / Cookie / CSRF）

## 📝 概要

本笔记描述：**当用户在浏览器中访问一个跨域接口时，从触发请求到 JavaScript 成功（或失败）读取响应的完整过程**，并标注 CORS、Cookie、CSRF 在各阶段的真实作用点。

---

## 🧠 场景设定

- 页面来源（Origin）：<https://frontend.com>
- 接口地址：<https://api.backend.com>
- 登录态：HttpOnly Cookie（Session / JWT）
- CSRF 防护：CSRF Token（Header 校验）
- 请求方式：POST + application/json

---

## ⏱️ 完整时序图

```text
┌────────────────────┐
│ 用户访问前端页面    │
│ https://frontend.com│
└─────────┬──────────┘
          │
          │ JS 发起 fetch / axios 请求
          │
          ▼
┌──────────────────────────────┐
│ 浏览器准备发送请求            │
│ 判断：是否跨域？ → 是         │
│ 判断：credentials 是否 include│
└─────────┬────────────────────┘
          │
          │（若非简单请求）
          │ 发送 OPTIONS 预检请求
          ▼
┌──────────────────────────────┐
│ OPTIONS /api                  │
│ Origin: frontend.com          │
│ Access-Control-Request-Method │
│ Access-Control-Request-Headers│
└─────────┬────────────────────┘
          │
          │ 服务端返回 CORS 授权
          ▼
┌──────────────────────────────┐
│ 200 OK                         │
│ Access-Control-Allow-Origin    │
│ Access-Control-Allow-Headers   │
│ Access-Control-Allow-Credentials│
└─────────┬────────────────────┘
          │
          │ 预检通过
          │ 浏览器发送正式请求
          ▼
┌──────────────────────────────┐
│ POST /api                     │
│ Cookie: session=xxx           │ ← 自动携带
│ X-CSRF-Token: yyy             │ ← JS 手动加
│ Origin: frontend.com          │
└─────────┬────────────────────┘
          │
          │ 后端逻辑
          │ 1️⃣ 验证登录态（Cookie）
          │ 2️⃣ 校验 CSRF Token
          ▼
┌──────────────────────────────┐
│ 200 OK / 403                  │
│ Access-Control-Allow-Origin   │
│ Access-Control-Allow-Credentials│
└─────────┬────────────────────┘
          │
          │ 浏览器检查 CORS 响应头
          │
          ▼
┌──────────────────────────────┐
│ JS 是否能读取 response？       │
│ ✔ CORS 允许 → JS 拿到数据      │
│ ✖ CORS 拒绝 → JS 报错          │
└──────────────────────────────┘

```

## 🔐 Cookie 在哪里“自动发送”？

- **只与请求目标域有关**

- 与是否跨站页面 **无关**

- 满足以下条件就会自动携带：

  - domain / path 匹配

  - Secure（https）

  - SameSite 允许（Lax / None）

👉 **这正是 CSRF 成立的根本原因**

---

## 🛡️ CSRF 防护发生在哪？

- CSRF Token：

  - 存在 Cookie / meta / JS 内存

  - JS 主动写入 Header

- 后端校验：

  - Cookie 中的 session

  - Header 中的 csrf token

  - **二者必须匹配**

👉 攻击者页面：

- 能触发请求

- 能携带 Cookie

- ❌ 无法读取 Cookie

- ❌ 无法构造正确 CSRF Header

---

## 🚨 常见失败点总结

|现象|原因|
|---|---|
|请求发出但 JS 拿不到数据|CORS 响应头缺失|
|OPTIONS 403|后端没处理预检|
|Cookie 没带|credentials 未开启|
|跨站请求被拒|SameSite=Strict|
|登录态正常但 403|CSRF 校验失败|

---

## 🎯 一句话理解

> **跨域不是“发不出请求”，而是“读不读得到响应”**  
> **CSRF 不是“能不能发请求”，而是“能不能伪造身份”**

---

## 🔗 关联笔记

- [[CORS 跨域机制]]

- [[CSRF 原理]]

- [[Fetch credentials 参数]]

- [[SameSite Cookie 详解]]

- [[HttpOnly Cookie]]

## 🏷️ 标签

- #web安全 #cors #csrf #cookie #浏览器
