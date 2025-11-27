# async/await 错误处理优化

## 📝 概要
在 JavaScript 中，async/await 提高了异步代码的可读性，但传统的 try...catch 会导致嵌套地狱和样板代码。
本卡片介绍一种更优雅的 async/await 错误处理模式，借鉴 Go 语言风格，通过 `to` 函数统一管理 Promise 错误。

## 📌 核心知识点

### 问题
- 嵌套 try...catch 层层增加缩进
- 样板代码冗余
- 错误处理与核心逻辑混合，关注点不单一

### 解决方案
- 封装 `to` 辅助函数，返回 [error, data]  
- 避免业务代码中重复 try...catch  
- 采用卫语句（Guard Clause）处理错误

### to 函数示例
```js
async function to(promise) {
  try {
    const data = await promise;
    return [null, data];
  } catch (error) {
    return [error, null];
  }
}
```

### 使用示例

#### 单请求
```js
async function displayUser(userId) {
  const [error, user] = await to(fetchUserById(userId));
  if (error) {
    console.error('获取用户失败:', error);
    return;
  }
  console.log('用户信息:', user.name);
}
```

#### 多请求（嵌套消除）
```js
async function loadPageData(userId) {
  const [userError, user] = await to(fetchUserById(userId));
  if (userError) return console.error('获取用户失败:', userError);

  const [postsError, posts] = await to(fetchPostsByUserId(user.id));
  if (postsError) return console.error('获取文章失败:', postsError);

  const [commentsError, comments] = await to(fetchCommentsForPosts(posts[0].id));
  if (commentsError) return console.error('获取评论失败:', commentsError);

  console.log('用户信息:', user.name);
  console.log('用户文章:', posts);
  console.log('文章评论:', comments);
}
```

#### 并发请求
```js
async function loadDashboard(userId) {
  const [
    [userError, userData],
    [settingsError, settingsData]
  ] = await Promise.all([
    to(fetchUser(userId)),
    to(fetchUserSettings(userId))
  ]);

  if (userError) console.error('加载用户数据失败');
  if (settingsError) console.error('加载用户设置失败');

  if (userData) console.log('用户数据:', userData);
  if (settingsData) console.log('用户设置:', settingsData);
}
```

## ✅ 优势总结
- 核心逻辑扁平化，易读易维护  
- 减少重复 try...catch 样板代码  
- 错误处理集中，关注点分离  
- 强制开发者处理 error，避免遗漏  

## 🔗 关联笔记
<!-- 可根据未来相关知识点补充 -->

## 🏷️ 标签
#javascript #async #await #错误处理 #前端API
