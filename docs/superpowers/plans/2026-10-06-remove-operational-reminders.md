# Remove Operational Reminders Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Remove the redundant operational-reminders card from the platform operations workbench.

**Architecture:** Suppress the reminder card at the component class boundary and change the first workbench row from three columns to two. The identity card and quick-entry card fill the row without adding a replacement component.

**Tech Stack:** Static HTML, CSS, vanilla JavaScript.

## Global Constraints

- Preserve existing uncommitted prototype changes.
- Do not change dashboard counts or queue calculation rules.
- Keep the existing visibility rules for all unrelated cards.

---

### Task 1: Remove the reminder card from presentation

**Files:**
- Modify: `管理端原型设计/styles.css`
- Test: `管理端原型设计/workflow.js`

**Interfaces:**
- Consumes: `.wb-reminders` card class emitted by `board()`.
- Produces: A two-column top row where the card has no visual box or layout footprint.

- [x] **Step 1: Add the presentation rule**

```css
.wb-reminders {
  display: none !important;
}

.wb-layout {
  grid-template-columns: minmax(280px, .9fr) minmax(440px, 1.8fr);
  grid-template-areas: "profile quick" "summary summary" "todo todo" "focus focus";
}
```

- [x] **Step 2: Run JavaScript syntax verification**

Run: `node --check 管理端原型设计/workflow.js`

Expected: command exits with status 0 and no diagnostics.

- [x] **Step 3: Run whitespace verification**

Run: `git diff --check`

Expected: command exits with status 0 and no whitespace errors.

- [x] **Step 4: Verify the rendered workbench**

Open the platform operations workbench and confirm that `REMINDERS` and `运营提醒` are absent while the identity card and quick-entry card fill the top row.
