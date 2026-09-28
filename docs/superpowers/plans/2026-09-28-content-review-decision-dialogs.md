# Content Review Decision Dialogs Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 统一信息内容审核的通过与驳回操作名称和确认弹窗。

**Architecture:** 复用现有 `.content-approval-*` 结构生成两种决策弹窗，仅调整展示模板和按钮文案，保留原 action 和数据逻辑。

**Tech Stack:** 原生 JavaScript 模板、HTML、CSS、Playwright CLI

## Global Constraints

- 不改变审核通过、驳回及通知逻辑。
- 不提交 Git commit。

---

### Task 1: 统一审核决策界面

**Files:**
- Modify: `管理端原型设计/workflow.js`

**Interfaces:**
- Consumes: `form('post-decision')`、`contentReviewDetail()`、现有审核 actions。
- Produces: 一致的通过与驳回确认结构。

- [x] **Step 1: 统一详情页通过按钮名称**
- [x] **Step 2: 重构驳回确认弹窗**
- [x] **Step 3: 运行静态检查和浏览器验证**
