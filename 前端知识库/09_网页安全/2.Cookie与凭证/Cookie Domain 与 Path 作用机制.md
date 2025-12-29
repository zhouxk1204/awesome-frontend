# Cookie Domain 与 Path 作用机制

## 📝 概要

Cookie 是否会随请求发送，不只与 SameSite 有关，还严格受 Domain 和 Path 匹配规则控制。

## 📌 核心知识点

- Cookie 只有在 **Domain 匹配** 时才会发送
- Path 决定 Cookie 的生效路径范围
- 子域与父域的 Cookie 继承规则是单向的

## 💻 示例代码

```http
Set-Cookie: session=abc;
  Domain=.example.com;
  Path=/admin;
```

```text

✅ https://api.example.com/admin/users
❌ https://api.example.com/profile
❌ https://other.com/admin
```

## ⚠️ 常见误区

- 误以为“同一个站点一定能读到 Cookie”

- 忽略 Path 导致 Cookie 未携带

- 误把 Domain 当成 URL 前缀

## 🎯 适用场景

- 登录态丢失排查

- CSRF / 认证问题调试

- 多子域系统设计

## 🔗 关联笔记

- [[SameSite Cookie 详解]]

- [[CSRF 原理]]

## 🏷️ 标签

- #cookie #web安全 #浏览器机制
