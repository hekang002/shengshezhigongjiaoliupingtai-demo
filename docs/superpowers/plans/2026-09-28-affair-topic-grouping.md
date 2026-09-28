# Affair Topic Grouping Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 在事项处理原型中增加人工建立问题专题、关联事项和按专题统一回复的完整流程。

**Architecture:** 在现有 `workflow.js` 的本地数据和事件分发机制上增加 `affairTopics` 数据归一化、专题查询及专题弹窗；复用现有事项多选和批量回复逻辑。样式继续放在 `styles.css`，不引入依赖或新的页面路由。

**Tech Stack:** 原生 JavaScript 模板、HTML、CSS、localStorage、Playwright CLI

## Global Constraints

- 不使用语义模型或自动共性判断。
- 不建设会议管理、事项分办、承办和审核流程。
- 保留“待处理、处理中、已回复”三状态。
- 回音壁发布仍为独立操作。
- 不提交 Git commit。

---

### Task 1: 专题数据与列表呈现

**Files:**
- Modify: `管理端原型设计/workflow.js`

**Interfaces:**
- Produces: `affairTopics(data): Array<Topic>`、`topicForAffair(data, affairId): Topic | undefined`
- Produces: `Topic = { id, name, summary, affairIds, discussionConclusion, createdBy, createdAt, updatedAt }`

- [ ] **Step 1: 增加专题数据归一化与查询助手**

确保旧 localStorage 没有 `affairTopics` 时初始化为空数组，并提供按事项编号查询专题的助手。

- [ ] **Step 2: 替换无依据的共性统计和筛选**

将“共性事项”汇总改为“问题专题”；事项属性仅保留重点/一般，新增专题关联筛选。

- [ ] **Step 3: 在事项表格展示关联专题**

增加“关联专题”列，专题名称可点击打开详情，未关联时显示“未关联”。

- [ ] **Step 4: 运行静态检查**

Run: `node --check 管理端原型设计/workflow.js && git diff --check`

Expected: 两条命令均退出码为 0。

### Task 2: 建立专题与加入专题

**Files:**
- Modify: `管理端原型设计/workflow.js`
- Modify: `管理端原型设计/styles.css`

**Interfaces:**
- Consumes: `affairSelection: Set<string>`
- Produces: actions `affair-topic-create`、`affair-topic-create-save`、`affair-topic-join`、`affair-topic-join-save`

- [ ] **Step 1: 扩展处理中批量工具栏**

在现有“统一回复”旁增加“建立问题专题”和“加入已有专题”，并对未达到最小选择数量的操作给出提示。

- [ ] **Step 2: 实现建立专题弹窗和保存逻辑**

弹窗展示已选事项、专题名称和问题概述；保存时生成专题编号，维护唯一关联关系并写入审计记录。

- [ ] **Step 3: 实现加入已有专题弹窗和保存逻辑**

展示可选专题、已关联事项数和问题概述；确认时把选中事项从原专题移除后加入目标专题。

- [ ] **Step 4: 补充响应式样式**

保持桌面左右结构；在 760px 以下切换为单列，弹窗正文可滚动且不产生页面横向溢出。

- [ ] **Step 5: 运行静态检查**

Run: `node --check 管理端原型设计/workflow.js && git diff --check`

Expected: 两条命令均退出码为 0。

### Task 3: 专题详情与统一回复

**Files:**
- Modify: `管理端原型设计/workflow.js`
- Modify: `管理端原型设计/styles.css`

**Interfaces:**
- Produces: action `affair-topic-view`
- Reuses: modal type `affair-batch-reply` with comma-separated affair IDs

- [ ] **Step 1: 实现专题详情弹窗**

详情展示专题名称、问题概述、创建信息、讨论结论和关联事项，并提供“统一回复”按钮。

- [ ] **Step 2: 从专题进入现有批量回复流程**

将仍处于“处理中”的关联事项编号传给现有批量回复弹窗；没有可回复事项时显示明确提示。

- [ ] **Step 3: 在统一回复后更新专题讨论结论**

回复成功时把本次讨论结论同步写入关联专题并更新时间，不改变已回复事项与专题的关联关系。

- [ ] **Step 4: 运行静态与浏览器验证**

Run: `node --check 管理端原型设计/workflow.js && git diff --check`

Expected: 两条命令均退出码为 0。

Run: 使用 Playwright 在 `1440x900` 和 `736x929` 验证建立专题、加入专题、专题详情、统一回复和页面无横向溢出。

Expected: 操作链路可完成，控制台无错误，`document.documentElement.scrollWidth === document.documentElement.clientWidth`。

