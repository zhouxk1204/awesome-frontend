# pagehide 与 pageshow

## 📝 概要
pagehide 和 pageshow 事件用于应对浏览器的往返缓存（bfcache），比 unload 更可靠。

## 📌 核心知识点
- pagehide：页面离开时触发，无论是否存入 bfcache
- event.persisted：页面是否被缓存
- 可与 navigator.sendBeacon 结合发送数据

## 💻 示例代码
```js
window.addEventListener('pagehide', (event) => {
  if (event.persisted) {
    console.log('页面进入 bfcache');
  } else {
    console.log('页面正常卸载');
  }
  navigator.sendBeacon('/log', getAnalyticsData());
});
```

## ⚠️ 常见误区
- unload 事件可能不触发
- 不要在 pagehide 中做耗时操作

## 🎯 适用场景
- 移动端页面卸载数据上报
- 与 bfcache 兼容的数据处理

## 🔗 关联笔记
- [[navigator.sendBeacon]]
- [[beforeunload与unload]]
- [[页面可见性API]]

## 🏷️ 标签
#javascript #页面卸载 #bfcache
