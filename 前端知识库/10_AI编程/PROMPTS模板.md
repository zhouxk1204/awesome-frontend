# 📘 PROMPTS 模板

> 用于驱动 AI 执行的 Prompt 模板集合

---

## 🟢 STEP 0：初始化项目

### 🎯 目标

生成 PRD / 技术栈 / 实施计划

### ▶️ Prompt

```
我想做一个项目：

【项目描述】
{填写}

请帮我：

1. 生成 PRD（简洁）
2. 生成 Tech Stack（最简单方案）
3. 生成 Implementation Plan

要求：
- 分步骤
- 每步包含 Objective / Tasks / Test
- 不要写代码
- 不要过度设计
```

---

## 🟡 STEP 1：计划校验

### ▶️ Prompt

```
Read all files in /memory-bank.

Is implementation-plan.md clear?

Ask all necessary questions.

Do NOT assume anything.
```

---

## 🟠 STEP 2：执行开发

### ▶️ Prompt

```
Read all files in /memory-bank.

Proceed with Step 1.

Rules:
- Start with planning pass
- Do NOT proceed further
- Wait for validation
```

---

## 🧪 STEP 3：测试后更新

### ▶️ Prompt

```
Update progress.md

Update architecture.md
```

---

## 🔁 STEP 4：循环执行

### ▶️ Prompt

```
Read progress.md

Proceed with next step

Do NOT skip steps
```

---

## 🐞 Bug 修复

### ▶️ Prompt

```
Here is the error:
[ERROR]

Tasks:
1. Analyze root cause
2. Propose minimal fix

Rules:
- Do NOT rewrite everything
```

---

## 🧩 新功能扩展

### ▶️ Prompt

```
We want to add a feature:

[描述]

Tasks:
1. Create feature-implementation.md
2. Break into small steps
3. Each step must include test

Do NOT implement yet
```

---

## 🔄 代码审查

### ▶️ Prompt

```
Review the following code:

[代码片段]

Check:
1. Code quality
2. Security issues
3. Performance concerns
4. Best practices

Provide specific improvement suggestions.
```

---

## 📝 文档生成

### ▶️ Prompt

```
Generate documentation for:

[模块/功能描述]

Include:
1. Overview
2. Usage examples
3. API reference
4. Configuration options
```

---

## 🧪 测试用例生成

### ▶️ Prompt

```
Generate test cases for:

[功能描述]

Include:
1. Unit tests
2. Edge cases
3. Error handling tests
4. Integration tests (if applicable)
```

---

## 🔧 重构建议

### ▶️ Prompt

```
Analyze and suggest refactoring for:

[代码片段]

Focus on:
1. Code organization
2. Reusability
3. Maintainability
4. Performance

Do NOT implement, just suggest.
```

---

# 📚 使用说明

1. 根据当前开发阶段选择对应 Prompt
2. 复制 Prompt 内容
3. 替换 `[占位符]` 为实际内容
4. 发送给 AI 执行
5. 验证结果后继续下一步
