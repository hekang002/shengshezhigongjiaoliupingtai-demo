# Workbench Top Layout Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Merge quick entrances into the identity card and place all four overview metrics to its right.

**Architecture:** Reuse and move the existing quick-entry DOM node in `refineOperationsWorkbench()` so navigation stays connected to its current events. Use CSS grid areas for the two-column top row and a 2×2 metric group on the right, then bump asset versions in `index.html` to prevent stale in-app browser resources.

**Tech Stack:** Static HTML, CSS Grid, vanilla JavaScript, Lucide icons.

## Global Constraints

- Preserve unrelated uncommitted work.
- Do not duplicate or recalculate any metric.
- Keep all existing navigation actions and tooltip behavior.

---

### Task 1: Recompose the workbench top area

**Files:**
- Modify: `管理端原型设计/app.js`
- Modify: `管理端原型设计/styles.css`
- Modify: `管理端原型设计/index.html`
- Test: browser snapshot and screenshot of the platform workbench

**Interfaces:**
- Consumes: `.wb-profile`, `.wb-profile-scope`, `.wb-side-section`, `.wb-summary`, and `.wb-center`.
- Produces: `.wb-profile-quick` and `.wb-summary-results`.

- [x] **Step 1: Move existing DOM nodes**

Append the quick-entry section to `.wb-profile` and retain all four existing metrics inside `.wb-summary`.

- [x] **Step 2: Implement the two-level grid**

Use `profile / summary` for the first row and arrange the four metrics as a 2×2 group. Remove the obsolete independent `quick` grid area and the separate metric row.

- [x] **Step 3: Update asset versions**

Set `styles.css`, `app.js`, and `workflow.js` resource queries to `20261006-workbench-metrics-right`.

- [x] **Step 4: Run static verification**

Run `node --check 管理端原型设计/app.js`, `node --check 管理端原型设计/workflow.js`, and `git diff --check`. All commands must exit with status 0.

- [x] **Step 5: Verify in the browser**

Confirm the quick entrances are below data scope inside the identity card, all four metrics are in the right-side 2×2 group, and the tooltip remains accessible.
