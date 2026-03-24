# Token 存储方式安全对比

## 📝 概要

不同 Token 存储位置，在 XSS 与 CSRF 防御上存在根本差异。

## 📌 核心知识点

- LocalStorage 易受 XSS
- Cookie 自动携带易受 CSRF
- HttpOnly Cookie 无法被 JS 读取

## 💻 示例代码

```js
localStorage.setItem("token", jwt);

Set-Cookie: token=xxx; HttpOnly; SameSite=Lax
```

## ⚠️ 常见误区

- 认为 HttpOnly Cookie 是“绝对安全”

- 混淆 XSS 与 CSRF 的威胁模型

- 忽略 SameSite 的重要性

## 🎯 适用场景

- 登录态设计

- 前端鉴权方案选型

- 安全架构评估

## 🔗 关联笔记

- [[HttpOnly Cookie]]

- [[XSS 攻击模型]]

## 🏷️ 标签

- #token #安全设计 #auth
