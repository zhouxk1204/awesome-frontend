# Preflight OPTIONS 请求原理

## 📝 概要

浏览器在发送“非简单请求”前，会先发 OPTIONS 请求确认服务器是否允许。

## 📌 核心知识点

- Simple Request 不触发 preflight
- Content-Type 决定是否安全
- OPTIONS 本身不携带业务数据

## 💻 示例代码

```http
OPTIONS /api/user
Access-Control-Request-Method: POST
Access-Control-Request-Headers: Authorization
```

## ⚠️ 常见误区

- 认为 OPTIONS 是后端业务逻辑

- 忽略 OPTIONS 导致请求失败

- 误解 CSRF 与 preflight 的关系

## 🎯 适用场景

- 跨域 API 设计

- CSRF 攻击理解

- 请求失败排查

## 🔗 关联笔记

- [[CSRF 原理]]

- [[CORS 跨域机制]]

## 🏷️ 标签

- #preflight #cors #http
