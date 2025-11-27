# beforeunload 与 unload

## 📝 概要
beforeunload 和 unload 事件用于监听页面即将离开（关闭、刷新、导航）的行为。

## 📌 核心知识点
- beforeunload：可提示用户是否离开，防止数据丢失
- unload：页面卸载时触发，但异步请求可能被中断

## 💻 示例代码
```js
window.addEventListener('beforeunload', (e) => {
  e.returnValue = ''; // 显示浏览器默认确认提示
});

window.addEventListener('unload', () => {
  console.log('用户正在离开页面');
});
```

## ⚠️ 常见误区
- unload 不可靠，异步请求可能失败
- 浏览器限制自定义提示文本

## 🎯 适用场景
- 提示用户保存数据
- 简单同步清理操作（不适合发送异步请求）

## 🔗 关联笔记
- [[页面可见性 API]]
- [[navigator.sendBeacon]]
- [[pagehide 与 pageshow]]

## 🏷️ 标签
#javascript #页面卸载 #前端API
