# 📘 PROMPTS 执行手册

> 此文件用于驱动 AI 执行，按步骤复制使用

---

## 🚀 快速开始

### STEP 0：初始化项目

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

### STEP 1：计划校验

```
Read all files in /memory-bank.

Is implementation-plan.md clear?

Ask all necessary questions.

Do NOT assume anything.
```

---

### STEP 2：执行开发

```
Read all files in /memory-bank.

Proceed with Step 1.

Rules:
- Start with planning pass
- Do NOT proceed further
- Wait for validation
```

---

### STEP 3：测试后更新

```
Update progress.md

Update architecture.md
```

---

### STEP 4：循环执行

```
Read progress.md

Proceed with next step

Do NOT skip steps
```

---

## 🐞 Bug 修复

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

## 📝 使用流程

1. **初始化** → 运行 STEP 0
2. **校验** → 运行 STEP 1
3. **开发** → 循环运行 STEP 2-4
4. **修复/扩展** → 按需使用对应 Prompt
