# JavaScript 模块化进阶

## 📝 概述
随着应用规模增大，JavaScript 模块化（module）成为标准解决方案。ES6 引入模块系统，主流浏览器和 Node.js 已全面支持。

---

## 📌 模块化语法

### 默认导出
```js
// module.js
const foo = 123
export default foo

import foo from './module.js'
console.log(foo)  // 123
```

### 命名导出
```js
export const foo = 123
import { foo } from './module.js'
console.log(foo)  // 123
```

### 默认 + 命名导出
```js
export const foo = 123
const bar = 456
export default bar

import bar, { foo } from './module.js'
console.log(foo, bar)  // 123 456
```

### 导入别名
```js
import { foo as f } from './module.js'
import { default as foo } from './module.js'
```

### 星号/命名空间导入
```js
import * as obj from './module.js'
console.log(obj.foo, obj.bar, obj.default)
```

### 副作用导入
```js
import './module.js'  // 执行初始化逻辑
```

---

## ⚡ 模块化特性

- **严格模式**：模块默认严格模式  
- **加载一次**：模块只会加载一次，导入多次不重复执行  
- **单向绑定**：导入者不能修改导出值  
- **globalThis**：跨环境统一全局对象  
- **defer 延迟执行**：模块延迟执行，保证 DOM 可用  
- **外部脚本**：需 CORS 支持  

---

## 🔧 模块化进阶

- **重新导出**：export { foo } from './foo.js'  
- **裸模块**：Nodejs 支持裸模块，浏览器需构建工具（Vite、Webpack）处理  
- **非 JS 模块化**：构建工具可支持 CSS、JSON、图片等模块化  
- **import.meta**：模块运行时元信息，支持自定义属性、环境变量、热更新  
- **ESM vs CJS**：静态分析 + Tree Shaking，ESM 支持 top-level await  
- **动态导入**：import(url) 返回 Promise，可用于懒加载路由  

---

## 📦 发布模块示例

- **库形式**
```json
{
  "name": "axios",
  "version": "1.0.0",
  "main": "index.js"
}
```
- **CLI 形式**
```json
{
  "name": "tsc",
  "version": "1.0.0",
  "main": "index.js",
  "bin": { "tsc": "bin/index.js" }
}
```

---

## 🔗 关联笔记
<!-- JavaScript模块化、ESM、CJS、Tree Shaking、动态导入、模块发布 -->

## 🏷️ 标签
#javascript #模块化 #esm #cjs #import_meta #动态导入 #tree_shaking #前端优化
