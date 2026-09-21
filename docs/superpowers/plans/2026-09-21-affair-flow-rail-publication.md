# Affair Flow Rail And Publication Gate Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Give all four affair states one narrow, reverse-order vertical flow rail and prevent unfinished affairs from appearing published.

**Architecture:** Add shared workflow helpers in `workflow.js` so assignment and affair-detail modals render the same status rail. Use a three-column modal grid on desktop and normalize process-post publication state in `prototype-data.js`.

**Tech Stack:** Static JavaScript templates, CSS Grid, localStorage-backed prototype data, Playwright CLI.

## Global Constraints

- Flow order is `已办结 → 答复审核 → 办理中 → 待分办` from top to bottom.
- `内容审核通过` is context, not a flow node.
- Unfinished affairs always display and persist `未发布`.
- Existing form field IDs and action names remain unchanged.
- No new dependencies or commits.

---

### Task 1: Shared flow and publication helpers

**Files:**
- Modify: `管理端原型设计/workflow.js`
- Modify: `prototype-data.js`

**Interfaces:**
- Produces: `affairFlowStatus(affair)`, `affairPublicationStatus(affair, post)`, and `affairFlowRail(affair, post)`.

- [x] Add one status mapper for the four affair states.
- [x] Add one reverse-order vertical rail renderer.
- [x] Normalize open process affairs to `未发布` and block publish actions before closure.
- [x] Set the final publication mode when answer review closes an affair.

### Task 2: Unified three-column modals

**Files:**
- Modify: `管理端原型设计/workflow.js`
- Modify: `管理端原型设计/styles.css`
- Modify: `管理端原型设计/index.html`

**Interfaces:**
- Consumes: shared flow helpers from Task 1.
- Produces: source, work area, and narrow flow rail columns for all four states.

- [x] Move the pending-assignment rail to the right of the assignment form.
- [x] Move the handling, review, and closed rail to the right of their detail content.
- [x] Keep the rail narrow and vertical at desktop widths.
- [x] Stack source, work area, and rail below 700px.
- [x] Update asset cache keys.

### Task 3: Browser verification

**Files:**
- Verify: `管理端原型设计/workflow.js`
- Verify: `管理端原型设计/styles.css`
- Verify: `prototype-data.js`

**Interfaces:**
- Consumes: the completed modal and publication behavior.
- Produces: evidence that all four states share the requested layout.

- [x] Verify pending, handling, answer review, and closed detail modals.
- [x] Verify unfinished affairs show `未发布`.
- [x] Verify desktop and 736x929 layouts have no document overflow.
- [x] Run JavaScript syntax checks, `git diff --check`, and browser console checks.
