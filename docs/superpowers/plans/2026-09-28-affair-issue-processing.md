# Affair Issue Processing Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 将事项处理改为问题标记、讨论结论记录和统一回复闭环。

**Architecture:** 保留现有三状态和批量选择机制，调整事项生成默认值、列表字段、弹窗字段和回复写入逻辑；不引入会议实体或承办流程。

**Tech Stack:** 原生 JavaScript 模板、HTML、CSS、Playwright CLI

## Global Constraints

- 不建设会议管理功能。
- 回复后直接进入已回复。
- 回音壁发布保持独立操作。
- 不提交 Git commit。

---

### Task 1: 调整事项生成与列表

**Files:**
- Modify: `管理端原型设计/workflow.js`

- [x] **Step 1: 新事项默认待分类并纳入三类来源**
- [x] **Step 2: 调整列表字段和操作名称**

### Task 2: 调整标记、结论与回复

**Files:**
- Modify: `管理端原型设计/workflow.js`

- [x] **Step 1: 标记问题不再按来源预选分类**
- [x] **Step 2: 回复弹窗增加讨论结论并移除回复方式**
- [x] **Step 3: 保存结论、回复人和回复时间**
- [x] **Step 4: 更新已回复详情**

### Task 3: 验证

**Files:**
- Test: `管理端原型设计/workflow.js`

- [x] **Step 1: 运行语法和差异检查**
- [x] **Step 2: 验证三状态页面和单条/批量回复流程**
