# navigator.sendBeacon

## 📝 概要
sendBeacon() 用于在页面卸载时可靠地发送少量数据到服务器，保证即使页面关闭也会发送。

## 📌 核心知识点
- 异步发送，不阻塞页面卸载
- 常与 unload 或 pagehide 配合使用
- 适合发送日志、统计、分析数据

## 💻 示例代码
```js
window.addEventListener('pagehide', () => {
  navigator.sendBeacon('/log', getAnalyticsData());
});
```

## ⚠️ 常见误区
- 不适合发送大量数据
- 不会阻止页面卸载

## 🎯 适用场景
- 用户离开页面时发送统计数据
- 后台日志上报

## 🔗 关联笔记
- [[beforeunload与unload]]
- [[pagehide与pageshow]]

## 🏷️ 标签
#javascript #页面卸载 #数据上报
