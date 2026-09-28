# Content Review Table Fields Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 调整信息内容审核表格，使字段随审核队列匹配当前业务阶段。

**Architecture:** 仅修改 `contentReviewTable` 的派生展示逻辑，根据 `pendingView` 生成不同的表头和行单元格。现有审核、发布及事项生成逻辑保持不变。

**Tech Stack:** HTML 字符串模板、JavaScript、Playwright CLI

## Global Constraints

- 不改变数据模型和状态生成规则。
- 不修改事项处理模块。
- 不提交 Git commit。

---

### Task 1: 调整审核表格字段

**Files:**
- Modify: `管理端原型设计/workflow.js`

**Interfaces:**
- Consumes: `contentReviewStatus`、`pendingView`、`processPost(post)`、`post.handlingStatus`
- Produces: 按审核队列动态生成的表头和行单元格

- [x] **Step 1: 更新行文案**

  将待审核项的去向文案改为“发布并进入待处理”或“仅发布”。

- [x] **Step 2: 按队列生成列**

  待审核/风险待审不输出状态单元格，列名使用“通过后处理”；已处理输出“事项状态”并保持行对齐。

- [x] **Step 3: 语法和差异检查**

  Run: `node --check 管理端原型设计/workflow.js`

  Expected: exit code `0`.

  Run: `git diff --check`

  Expected: no whitespace errors.

- [x] **Step 4: 浏览器验收**

  用 Playwright CLI 分别打开“待审核”“风险待审”“已处理”，核对表头、行数据及 `736×929` 视口溢出情况。

### Task 2: 精简审核详情信息

**Files:**
- Modify: `管理端原型设计/workflow.js`
- Modify: `管理端原型设计/styles.css`

**Interfaces:**
- Consumes: `contentRisk(post, data)` 返回的 `hits`、`labels`、`level`、`suggestion`
- Produces: 精简后的审核详情侧栏和待审列表

- [x] **Step 1: 精简待审列表**

  删除“通过后处理”表头及行单元格，已处理列表继续展示“事项状态”。

- [x] **Step 2: 重组审核详情侧栏**

  展示“系统检测结论”“命中敏感词（N）”和“发布信息”，删除办理状态、审核队列及通过后流向。

- [x] **Step 3: 补充敏感词可读样式**

  在现有 `content-evidence-tags` 样式上增加计数与辅助说明布局，保持窄屏可读。

- [x] **Step 4: 执行验证**

  Run: `node --check 管理端原型设计/workflow.js`

  Expected: exit code `0`.

  Run: `git diff --check`

  Expected: no whitespace errors.

  用 Playwright CLI 检查待审列表、中风险详情和高风险详情，并确认 `736×929` 视口没有页面级水平溢出或控制台错误。

### Task 3: 合并风险结论与命中词

**Files:**
- Modify: `管理端原型设计/workflow.js`
- Modify: `管理端原型设计/styles.css`

**Interfaces:**
- Consumes: `contentRisk(post, data)` 的 `hits`、`labels`、`level`
- Produces: 单一的 `content-evidence-section` 敏感词检测区块

- [x] **Step 1: 合并详情模板**

  删除独立 `content-review-risk-head` 区块，将风险等级与人工确认标签移入“敏感词检测”标题行。

- [x] **Step 2: 压缩区块样式**

  增加紧凑的标题行、风险标签和辅助说明样式，保留高中低风险的可辨识性。

- [x] **Step 3: 验证合并效果**

  Run: `node --check 管理端原型设计/workflow.js`

  Expected: exit code `0`.

  用 Playwright CLI 在 `736×929` 视口打开审核详情，确认只有一个敏感词检测区块、命中词可读、无页面溢出且控制台无错误。

### Task 4: 删除详情重复标签

**Files:**
- Modify: `管理端原型设计/workflow.js`
- Modify: `管理端原型设计/styles.css`

**Interfaces:**
- Consumes: `contentReviewDetail(data, post)`
- Produces: 仅保留内容分类和风险等级的精简详情头部

- [x] **Step 1: 删除标题区发布状态与人工确认标签**

  删除 `badgeFor(contentState(post))`、`reviewRequirement` 及其 `em` 标签。

- [x] **Step 2: 清理样式并验证**

  移除已无用的 `em` 样式，执行 `node --check` 和 `git diff --check`，并用 Playwright CLI 验证 `736×929` 详情页。

### Task 5: 移动审核记录

**Files:**
- Modify: `管理端原型设计/workflow.js`

**Interfaces:**
- Consumes: `events` 审核记录数组
- Produces: 右侧信息栏中的审核记录区块

- [x] **Step 1: 移动详情模板区块**

  将审核记录模板从 `content-review-document` 移至 `content-review-inspector`，放在发布信息之后。

- [x] **Step 2: 验证布局**

  执行 `node --check` 和 `git diff --check`，再用 Playwright CLI 验证 `736×929` 下审核记录只出现在右侧信息栏且页面无溢出。
