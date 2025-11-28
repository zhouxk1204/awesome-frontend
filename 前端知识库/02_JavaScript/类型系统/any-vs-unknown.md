# TypeScript：any-vs-unknown
## 🧩 any 与 unknown 的核心区别

| 特性      | any             | unknown           |
| ------- | --------------- | ----------------- |
| 类型安全    | ❌ 跳过检查          | ✅ 强制类型检查          |
| 可赋值性    | 可赋值给任意类型        | 只能赋值给 any/unknown |
| 属性/方法访问 | 可直接访问（但运行时可能报错） | ❌访问前必须类型收窄        |
| 使用场景    | JS迁移、快速原型、第三方库  | API 数据、来源不明数据     |
| 官方态度    | 不推荐 ⚠️          | 推荐优先使用 ⭐          |

## 🎯 面试官最想听的回答（高级版本）

> **any = 逃生舱（放弃类型检查）**  
> **unknown = 安全顶级类型（类型收窄后才能使用）**

## 关键示例对比

### ❌ any —— 编译通过，但运行时可能崩

```ts
function test(v: any) {
  v.foo.bar();  // 编译通过，运行时炸
}
````

### ✅ unknown —— 强制类型检查，避免崩

```ts
function test(v: unknown) {
  if (typeof v === 'object' && v !== null && 'foo' in v) {
    (v as any).foo; // 类型收窄后安全
  }
}
```

## 🔐 类型系统层级位置

```
never → 具体类型 → 联合类型 → unknown
                        ↑
                      any （绕过类型系统）
```

## ✔️ 最佳实践总结

|建议|场景|
|---|---|
|默认使用 `unknown`|未知来源数据、API响应、工具函数|
|谨慎使用 `any`|JS迁移、复杂第三方库、POC快速开发|
|配置 ESLint 禁止 `any`|@typescript-eslint/no-explicit-any|

> unknown = 更安全的 any 替代方案

## 🗣️ 一句话总结（面试背诵版）

> any 绕过类型系统，编译器完全不管你；  
> unknown 仍需遵守类型检查，通过类型守卫才能安全使用。  
> **unknown 是现代 TS 的最佳实践。**

## 🔗 关联笔记
[[类型守卫]]

## 🏷️ 标签
#TypeScript #类型系统 #any #unknown #类型安全