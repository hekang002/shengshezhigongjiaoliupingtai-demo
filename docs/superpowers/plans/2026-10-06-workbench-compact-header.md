# Workbench Compact Header Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the oversized identity-and-shortcuts block with a compact identity strip and one-row operational metrics.

**Architecture:** Keep the existing `board()` data sources and `refineOperationsWorkbench()` metric calculations. Remove the duplicate quick-entry markup at its source, simplify the refinement hook so it only removes legacy mini-stats and populates metrics, then restyle the existing workbench grid without introducing new components or data.

**Tech Stack:** Static HTML, CSS Grid, vanilla JavaScript, Lucide icons.

## Global Constraints

- Preserve the four existing metric definitions, tooltip copy, values, and click behavior.
- Remove only the duplicated workbench quick entrances; keep the left application navigation unchanged.
- Keep the desktop workbench minimum canvas width at `980px`.
- Use the existing red, gold, amber, and green semantic colors.
- Do not introduce new dependencies.

---

### Task 1: Remove duplicate quick entrances

**Files:**
- Modify: `管理端原型设计/workflow.js`
- Modify: `管理端原型设计/app.js`

**Interfaces:**
- Consumes: `.wb-profile`, `.wb-mini-stats`, and `.wb-summary` rendered by `board()`.
- Produces: a profile containing only `.wb-profile-top` and `.wb-profile-scope`, plus the unchanged `.wb-summary` target.

- [x] **Step 1: Remove the quick-link data and markup**

Delete the `quickLinks` array and the `wb-side-section` containing “常用入口” from the platform workbench template.

- [x] **Step 2: Simplify the refinement preconditions**

Remove the `quickEntrances` query, class mutation, and `appendChild` call from `refineOperationsWorkbench()`. Keep removal of `.wb-mini-stats`, require only `.wb-profile`, `.wb-summary`, and `PrototypeData`, then populate metrics as before.

- [x] **Step 3: Verify removed copy**

Run:

```bash
rg -n "常用入口|查看驾驶舱|wb-profile-quick|quickEntrances|quickLinks" 管理端原型设计/app.js 管理端原型设计/workflow.js
```

Expected: no matches.

### Task 2: Build the compact visual hierarchy

**Files:**
- Modify: `管理端原型设计/app.js`
- Modify: `管理端原型设计/styles.css`

**Interfaces:**
- Consumes: `.wb-profile`, `.wb-profile-top`, `.wb-profile-scope`, `.wb-summary-card`, and semantic card classes.
- Produces: a one-line identity strip and a four-column metric strip.

- [x] **Step 1: Add the long-running attention marker**

Add `<em class="wb-summary-flag">需关注</em>` inside the “长时间未回复” metric label. Keep its tooltip icon and business copy unchanged.

- [x] **Step 2: Change the workbench grid**

Set `.wb-layout` to one content column with grid areas `profile`, `summary`, `todo`, and `focus`. Set `.wb-profile` to two columns and `.wb-summary` to four equal columns.

- [x] **Step 3: Refine spacing and semantic colors**

Reduce avatar and identity padding, align the data scope at the right edge, remove the 2x2 metric borders, and style `.wb-summary-flag` plus the subtle amber long-running state.

- [x] **Step 4: Verify visual states**

Use Playwright at `1536x960` and `753x919`. Confirm the identity strip is one row, all four metrics align, labels do not overlap, and the task queue moves upward.

### Task 3: Bust caches and run final checks

**Files:**
- Modify: `管理端原型设计/index.html`

**Interfaces:**
- Consumes: updated CSS and JavaScript assets.
- Produces: prototype URL version `20261006-workbench-compact-v4`.

- [x] **Step 1: Update resource versions**

Set `styles.css`, `app.js`, and `workflow.js` query strings to `20261006-workbench-compact-v4`.

- [x] **Step 2: Run static checks**

Run:

```bash
node --check 管理端原型设计/app.js
node --check 管理端原型设计/workflow.js
git diff --check
```

Expected: all commands exit with status `0`.

- [x] **Step 3: Verify the live prototype**

Open the versioned platform workbench, confirm there are no browser console errors, and refresh the user's in-app browser tab to `v=20261006-workbench-compact-v4&role=platform`.
