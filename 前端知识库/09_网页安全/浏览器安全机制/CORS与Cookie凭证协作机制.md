# CORS 与 Cookie 凭证协作机制

## 📝 概要

跨域请求中，Cookie 是否发送和是否被接收，必须由浏览器和服务端共同允许。

## 📌 核心知识点

- credentials 决定是否携带 Cookie
- 服务端必须显式允许凭证
- Access-Control-Allow-Origin 不能为 *

## 💻 示例代码

```http
Access-Control-Allow-Origin: https://frontend.com
Access-Control-Allow-Credentials: true
```

```js
fetch(url, { credentials: "include" })
```

## ⚠️ 常见误区

- 以为设置 credentials 就够了

- 使用 `*` 作为 Allow-Origin

- 忽略 preflight 导致失败

## 🎯 适用场景

- 前后端分离

- 跨域登录态

- Cookie Session 架构

## 🔗 关联笔记

- [[Fetch credentials 参数]]

- [[同源策略（SOP）详解]]

## 🏷️ 标签

- #cors #cookie #跨域
