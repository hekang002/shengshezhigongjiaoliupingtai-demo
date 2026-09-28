# Content Review Approval Dialog Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 优化内容审核通过确认弹窗的信息层级和视觉表现。

**Architecture:** 保留现有 `form('post-decision')` 的业务分支和提交动作，只替换批准场景的展示模板，并用独立 CSS 类限定样式影响范围。

**Tech Stack:** 原生 JavaScript 模板、HTML、CSS、Playwright CLI

## Global Constraints

- 不改变审核通过后的发布和事项生成逻辑。
- 适配 736×929 视口且不得横向溢出。
- 不提交 Git commit。

---

### Task 1: 重构确认弹窗

**Files:**
- Modify: `管理端原型设计/workflow.js`
- Modify: `管理端原型设计/styles.css`

**Interfaces:**
- Consumes: `form('post-decision', id)`、`sensitiveWordHits()`、现有审核 action。
- Produces: `.content-approval-*` 弹窗结构和样式。

- [x] **Step 1: 调整批准场景模板**

  将标题改为“确认审核通过”，并输出审核对象摘要、敏感词标签、处理意见和操作结果说明。

- [x] **Step 2: 增加限定样式**

  使用 `.content-approval-*` 类建立紧凑层级，并添加窄屏换行规则。

- [x] **Step 3: 静态检查**

  运行 `node --check 管理端原型设计/workflow.js` 和 `git diff --check`，预期退出码为 0。

- [x] **Step 4: 浏览器验证**

  在 736×929 视口打开审核通过弹窗，确认内容完整、无横向溢出、控制台无错误。
