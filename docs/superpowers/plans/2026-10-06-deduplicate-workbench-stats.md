# Deduplicate Workbench Statistics Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Remove duplicate workbench statistics and make the remaining total explicit and explainable.

**Architecture:** Refine the rendered platform workbench after its HTML is mounted, using the existing unique workbench selectors. Remove the duplicate mini-stat node, relabel the summary total, add the established `metric-help` tooltip, and rebalance the identity card with scoped CSS.

**Tech Stack:** Static HTML, CSS, vanilla JavaScript, Lucide icons.

## Global Constraints

- Preserve all unrelated existing and uncommitted changes.
- Keep the current total formula unchanged: pending review + pending handling + pending publication.
- Keep the detailed work queue unchanged.

---

### Task 1: Deduplicate and clarify the workbench statistics

**Files:**
- Modify: `管理端原型设计/app.js`
- Modify: `管理端原型设计/styles.css`
- Test: browser snapshot and screenshot of the platform dashboard

**Interfaces:**
- Consumes: `.wb-profile .wb-mini-stats`, `.wb-summary > div:first-child`, and the existing `.metric-help` tooltip style.
- Produces: `refineOperationsWorkbench()` and a clarified `.wb-summary-label`.

- [x] **Step 1: Add the render refinement**

Remove `.wb-mini-stats`; change the first summary label to `当前待办总数`; add the tooltip text `当前待办总数 = 待审核 + 待处理 + 待公开；处理中事项已进入办理流程，不重复计入。` and change the note to `待审核 + 待处理 + 待公开`.

- [x] **Step 2: Rebalance the identity card**

Make `.wb-profile` a two-row grid so the identity block fills available space and the data-scope row stays at the bottom. Add scoped tooltip positioning for `.wb-summary-label`.

- [x] **Step 3: Run static checks**

Run `node --check 管理端原型设计/app.js`, `node --check 管理端原型设计/workflow.js`, and `git diff --check`. All commands must exit with status 0.

- [x] **Step 4: Verify the browser result**

Confirm the four duplicate stats are absent, the first summary label and note are updated, the tooltip is present with the exact formula, and the identity/quick-entry row remains visually balanced.
