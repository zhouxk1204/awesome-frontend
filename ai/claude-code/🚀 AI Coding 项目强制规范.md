# 🚀 AI Coding 项目强制规范（Claude Code版）

> **核心理念：AI 是执行者，人类负责规划与控制。**

---

# 🧠 一、核心原则（必须遵守）

## 1️⃣ 规划优先（Planning First）

* ❌ 禁止 AI 自主规划整个项目
* ✅ 所有开发必须基于已确认文档执行
* ✅ 编码前必须进行 Planning Pass

---

## 2️⃣ 文档驱动开发（Document-Driven Development）

项目必须包含以下结构：

```
/project
  ├── CLAUDE.md
  ├── PROMPTS.md
  └── /memory-bank
        ├── prd.md / gdd.md
        ├── tech-stack.md
        ├── implementation-plan.md
        ├── architecture.md
        └── progress.md
```

---

## 3️⃣ 三层架构（非常关键）

| 层级     | 文件          | 作用      |
| ------ | ----------- | ------- |
| 🟢 规则层 | CLAUDE.md   | AI 行为约束 |
| 🟡 数据层 | memory-bank | 项目事实    |
| 🔵 操作层 | PROMPTS.md  | 执行步骤    |

---

# 📁 二、文件职责说明

## 🟢 CLAUDE.md（规则）

用于约束 AI 行为：

* 必须读取哪些文件
* 是否允许跳步骤
* 编码规范

---

## 🟡 memory-bank（项目状态）

### prd.md / gdd.md

* 产品需求 / 游戏设计

### tech-stack.md

* 技术选型（优先简单）

### implementation-plan.md

* 分步骤执行计划（无代码）

### architecture.md

* 模块结构与职责

### progress.md

* 当前开发进度

---

## 🔵 PROMPTS.md（执行手册🔥）

用于驱动 AI：

* 初始化 Prompt
* 开发 Prompt
* Bug Prompt
* Feature Prompt

👉 用户按步骤复制执行

---

# 🔄 三、标准开发流程（Workflow）

```
需求 → PRD → 技术栈 → Implementation Plan
   ↓
Step 1 → 测试 → Step 2 → 测试 → ...
   ↓
记录 progress + 更新 architecture
```

---

# ⚙️ 四、Implementation Plan 规范

每一步必须包含：

```
## Step X

### Objective
目标

### Tasks
- 具体任务

### Test
验证方式
```

---

# 🧠 五、多 Agent 协同模式（进阶）

## 🎭 角色划分

| Agent     | 职责   |
| --------- | ---- |
| Product   | PRD  |
| Architect | 技术架构 |
| Planner   | 拆步骤  |
| Developer | 写代码  |

---

## 🔄 工作流

```
需求 → Product
     → Architect
     → Planner
     → Developer（循环执行）
```

---

# ⚠️ 六、常见错误（必须避免）

## ❌ AI 自主规划

👉 项目失控

## ❌ 跳过 implementation plan

👉 结构崩溃

## ❌ 一次生成大量代码

👉 不可维护

## ❌ 不清理上下文

👉 输出质量下降

---

# 💡 七、最佳实践

## ✅ 小步迭代

一次只做一个 Step

## ✅ 高频验证

每步必须测试

## ✅ 上下文管理

定期清空对话

## ✅ Git提交

每步完成后提交

---

# 🎯 八、一句话总结

> **你设计流程，AI执行步骤。**

---

# 🧩 九、进阶扩展（可选）

* Feature-level implementation plan
* 自动生成 Prompt Pack
* 多Agent自动协作
* 项目模板生成器

---

# 📚 相关文件

- [📘 PROMPTS 模板](📘%20PROMPTS%20模板.md) - 所有 Prompt 模板集合
- [📦 项目模板](项目模板/) - 可直接复用的项目模板文件
