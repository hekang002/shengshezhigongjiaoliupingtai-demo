# Centralized Affair Processing Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace department assignment and handling with a three-state, management-operated classification and unified reply workflow.

**Architecture:** Normalize legacy affair records at the shared data layer, then render one management-side affair workspace over the normalized model. Keep user notification and echo-wall publication as separate side effects of reply and explicit publication actions.

**Tech Stack:** Static JavaScript templates, CSS Grid, localStorage-backed prototype data, Playwright CLI.

## Global Constraints

- Affair states are exactly `待处理`, `处理中`, and `已回复`.
- Only sensitive-word hits require content review.
- Business exchange posts never generate affairs.
- Replies do not automatically publish to the echo wall.
- No assignee, department assignment, deadline, extension, reminder, transfer, answer review, questionnaire, or closure workflow remains visible.
- Preserve unrelated dirty-worktree changes and add no dependencies or commits.

---

### Task 1: Normalize content and affair data

**Files:**
- Modify: `prototype-data.js`
- Modify: `app.js`

**Interfaces:**
- Produces: normalized affair `status`, `category`, `topicTags`, `isKey`, `isCommon`, `internalNote`, `draft`, `feedback`, and `repliedAt` fields.
- Produces: staff progress stages `['待处理', '处理中', '已回复']`.

- [x] Map legacy affair statuses to the three approved states during data reconciliation.
- [x] Create new affairs in `待处理` for published suggestions and voice requests only.
- [x] Normalize non-sensitive posts to `审核通过 + 已发布`; retain review for sensitive hits.
- [x] Replace staff-side assignment and closure wording with the approved three-state language.
- [x] Run `node --check prototype-data.js` and `node --check app.js`.

### Task 2: Replace management navigation and workspace

**Files:**
- Modify: `管理端原型设计/app.js`
- Modify: `管理端原型设计/workflow.js`

**Interfaces:**
- Consumes: normalized affairs from Task 1.
- Produces: `affair-workspace` route, three status tabs, classification actions, reply actions, and selection state.

- [x] Remove the visible contractor-management navigation group and rename the affair route to `事项处理`.
- [x] Replace assignment filters, summary, table fields, and actions with classification-oriented equivalents.
- [x] Add a classification detail action that saves category, tags, key/common flags, and internal note before moving an affair to `处理中`.
- [x] Add row selection for processing affairs and a unified-reply command for one or more selected records.
- [x] Write one reply to every selected affair, notify each author, and move each affair to `已回复`.
- [x] Keep echo-wall publication as a separate action available only for replied affairs.
- [x] Remove visible deadline, reminder, extension, transfer, answer-review, and closed-state actions from this workspace.
- [x] Run `node --check 管理端原型设计/app.js` and `node --check 管理端原型设计/workflow.js`.

### Task 3: Restyle the simplified workspace

**Files:**
- Modify: `管理端原型设计/styles.css`
- Modify: `管理端原型设计/index.html`

**Interfaces:**
- Consumes: new workspace class names and templates from Task 2.
- Produces: responsive status summary, filters, selection toolbar, tables, and detail/reply modals.

- [x] Add stable responsive dimensions for the three-state workspace and batch toolbar.
- [x] Remove layout dependencies on the old assignment and handling controls where no longer referenced.
- [x] Update asset cache keys in `管理端原型设计/index.html`.
- [x] Run `git diff --check`.

### Task 4: Browser verification

**Files:**
- Verify: `管理端原型设计/`
- Verify: `app.js`
- Verify: `prototype-data.js`

**Interfaces:**
- Consumes: completed centralized workflow.
- Produces: evidence for the approved end-to-end business behavior.

- [x] Verify all three tabs and their unified query fields.
- [x] Classify one pending affair and confirm it enters processing.
- [x] Select multiple processing affairs, send one unified reply, and confirm every record enters replied.
- [x] Publish one replied affair to the echo wall and confirm the affair remains replied.
- [x] Verify non-sensitive content bypasses review while sensitive hits remain reviewable.
- [x] Verify staff-side progress uses the same three states.
- [x] Check 736x929 and desktop layouts, document overflow, and browser console errors.
- [x] Re-run JavaScript syntax checks and `git diff --check`.
