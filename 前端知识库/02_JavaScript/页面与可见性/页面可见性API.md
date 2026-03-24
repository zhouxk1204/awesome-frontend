# 页面可见性 API

## 📝 概要
Page Visibility API 用于检测页面是否对用户可见，适合处理切换标签页、最小化窗口等场景。

## 📌 核心知识点
- document.hidden：页面不可见时为 true
- visibilitychange 事件：页面可见性变化时触发
- 可用于暂停视频、动画或轮询请求

## 💻 示例代码
```js
document.addEventListener('visibilitychange', () => {
  if (document.hidden) {
    console.log('用户离开了当前页面');
    pauseMyVideo();
  } else {
    console.log('用户回来了');
    playMyVideo();
  }
});
```

## ⚠️ 常见误区
- 无法区分标签页切换和页面关闭
- 不适合发送异步数据上报

## 🎯 适用场景
- 暂停/恢复动画或轮播
- 节省轮询请求资源
- 控制媒体播放

## 🔗 关联笔记
- [[beforeunload与unload]]
- [[navigator.sendBeacon]]
- [[pagehide与pageshow]]

## 🏷️ 标签
#javascript #页面可见性 #前端API
