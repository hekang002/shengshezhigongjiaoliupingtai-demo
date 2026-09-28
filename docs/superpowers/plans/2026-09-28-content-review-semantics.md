# Content Review Semantics Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 明确信息内容审核页面的风险提示、操作边界和审核记录语义。

**Architecture:** 在现有审核详情模板中增加轻量提示组件，限制审核模块的操作集合，并在展示层格式化既有审计数据，不改动数据结构。

**Tech Stack:** 原生 JavaScript 模板、HTML、CSS、Playwright CLI

## Global Constraints

- 只调整信息内容审核模块。
- 不改变现有审核提交和审计写入逻辑。
- 不提交 Git commit。

---

### Task 1: 调整审核详情与操作

**Files:**
- Modify: `管理端原型设计/workflow.js`
- Modify: `管理端原型设计/styles.css`

**Interfaces:**
- Consumes: `contentReviewDetail()`、`contentReviewTable()`、现有 `data.audit` 记录。
- Produces: `.content-help-tip` 悬浮说明和结构化审核记录。

- [x] **Step 1: 将辅助说明改成问号悬浮提示**
- [x] **Step 2: 移除审核模块的发布管理操作**
- [x] **Step 3: 格式化审核结果、意见、人员和时间**
- [x] **Step 4: 运行静态检查和浏览器验证**
