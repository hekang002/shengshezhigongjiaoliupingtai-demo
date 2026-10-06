# Task-Driven Workbench Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Turn the platform work overview into an operational task page with consistent counts, separated work states, and status-duration-based actions.

**Architecture:** Keep `board()` as the source of queue counts and navigation targets, but make each actionable category a distinct queue row. Use `refineOperationsWorkbench()` to derive the four work metrics from current prototype data, clarify copy, add risk metadata, and retain the approved top layout without rewriting the large HTML template.

**Tech Stack:** Static HTML, CSS Grid, vanilla JavaScript, Lucide icons, localStorage-backed prototype data.

## Global Constraints

- Preserve all unrelated uncommitted changes.
- The top pending count must equal the sum of visible actionable queue rows.
- Processing items must not be counted as pending work.
- Metrics and status-duration metadata must update on every render.
- The workbench must not derive urgency from deadline fields that cannot be set in the current item-processing workflow.

---

### Task 1: Align the actionable queue

**Files:**
- Modify: `管理端原型设计/workflow.js`

**Interfaces:**
- Consumes: `pending`, `waiting`, `pendingPublications`, `pendingComments`, `pendingReports`, and `pendingUsers`.
- Produces: six queue rows with direct navigation targets.

- [ ] **Step 1: Replace the queue categories**

Remove `处理中事项` and the combined `互动核查` row. Add separate `评论审核`, `举报核查`, and `用户审核` rows so every count has a correct destination.

### Task 2: Reframe metrics and risk items

**Files:**
- Modify: `管理端原型设计/app.js`
- Modify: `管理端原型设计/styles.css`

**Interfaces:**
- Consumes: `.wb-summary`, `.wb-queue`, `.wb-focus-list`, and `PrototypeData.read()`.
- Produces: `待我处理`, `处理中事项`, `长时间未回复`, `已回复事项`, and duration-enriched focus rows.

- [ ] **Step 1: Calculate consistent work metrics**

Set pending work from the sum of visible queue counts. Calculate processing, long-running open items, and replied counts from prototype data using explicit definitions. Long-running means a pending or processing item has remained in its current state for at least 7 natural days.

- [ ] **Step 2: Clarify workbench copy and actions**

Rename the two main sections, remove English kickers, update page supporting text, and make the metric cards navigable.

- [ ] **Step 3: Enrich focus rows**

Add current-status duration, its start date, and next-action text to each visible affair. Pending duration starts at item creation; processing duration starts at classification or entry into processing. Missing timestamps display as unrecorded and are excluded from the long-running count. Remove non-functional status tabs and add a processing-priority note.

- [ ] **Step 4: Apply operational visual hierarchy**

Use restrained red for pending work and amber for long-running items, keep result metrics neutral, and ensure all labels fit within the existing desktop canvas.

### Task 3: Prevent stale assets and verify

**Files:**
- Modify: `管理端原型设计/index.html`
- Test: browser snapshot, metric formula tooltip, and first-screen screenshot

**Interfaces:**
- Consumes: updated `app.js`, `workflow.js`, and `styles.css`.
- Produces: cache-busted prototype URL `20261006-task-duration-v3`.

- [ ] **Step 1: Update resource versions**

Set `styles.css`, `app.js`, and `workflow.js` queries to `20261006-task-duration-v3`.

- [ ] **Step 2: Run static checks**

Run `node --check 管理端原型设计/app.js`, `node --check 管理端原型设计/workflow.js`, and `git diff --check`. All commands must exit with status 0.

- [ ] **Step 3: Verify business consistency in the browser**

Confirm the top pending metric equals the six queue counts, processing does not appear in the queue, long-running items are visually identified, no deadline or overdue wording remains on the workbench, and no English kickers remain.
