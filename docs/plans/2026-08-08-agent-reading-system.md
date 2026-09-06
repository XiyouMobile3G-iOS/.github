# Agent Reading System Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** 在仓库根目录建立可自动发现的 `AGENTS.md` 与被其引用的 `memory.md`，让 Agent 在修改组织首页前获得稳定上下文和明确约束。

**Architecture:** `AGENTS.md` 是覆盖整个仓库的执行入口，定义阅读顺序、事实来源、编辑边界和验证流程。`memory.md` 只保存稳定背景、长期决策和待确认事项；实际首页内容继续由 `profile/README.md` 唯一承载。

**Tech Stack:** Markdown、Git、GitHub Organization Profile README

---

### Task 1: 建立 Agent 自动入口

**Files:**
- Create: `AGENTS.md`
- Read: `profile/README.md`
- Read: `docs/plans/2026-08-08-agent-reading-system-design.md`

**Step 1: 确认入口尚不存在**

Run: `test ! -e AGENTS.md`

Expected: 命令退出码为 0。

**Step 2: 创建最小入口文件**

创建 `AGENTS.md`，完整内容如下：

```markdown
# AGENTS.md

## 适用范围

本文件适用于仓库根目录及其全部子目录。

## 必读顺序

开始任务前，按顺序阅读：

1. `AGENTS.md`
2. `memory.md`
3. `profile/README.md`
4. 与当前任务直接相关的 `docs/plans/` 文档

## 仓库目标

本仓库是 `XiyouMobile3G-iOS` 的 GitHub 组织资料仓库。`profile/README.md` 会渲染为组织首页，所有改动应服务于清晰介绍组织、展示项目和引导参与。

## 事实来源

- 首页实际内容以 `profile/README.md` 为唯一事实来源。
- 稳定背景、长期决策和待确认事项记录在 `memory.md`。
- 不要在 `memory.md` 中复制首页正文。

## 工作规则

- 修改前先查看 `git status`，保留与当前任务无关的用户改动。
- 优先进行范围最小、易于审阅的修改。
- 面向访客的内容默认使用简洁、自然的中文，除非任务另有要求。
- 仅使用 GitHub Markdown 支持的语法与安全 HTML。
- 不记录或提交 Token、凭据、成员隐私等敏感信息。
- 只有长期有效的目标、结构或设计决策变化时才更新 `memory.md`；普通措辞和格式调整无需记录。
- 未经明确要求，不修改仓库设置、远程地址、分支策略，不提交或推送变更。

## 修改流程

1. 阅读必读文件并确认任务边界。
2. 检查目标文件和现有风格。
3. 实施最小变更。
4. 长期决策有变化时同步更新 `memory.md`。
5. 执行验证并检查最终差异。

## 验证清单

- 运行 `git diff --check`，确保没有空白错误。
- 检查 Markdown 标题层级、HTML 标签闭合和表格结构。
- 检查新增或修改的链接、图片地址和仓库名称。
- 运行 `git status --short` 和 `git diff --stat`，确认变更范围符合任务。
```

**Step 3: 验证入口结构**

Run: `rg -n '^# AGENTS.md$|^## (适用范围|必读顺序|仓库目标|事实来源|工作规则|修改流程|验证清单)$' AGENTS.md`

Expected: 输出主标题和七个二级标题。

**Step 4: 提交入口文件**

```bash
git add AGENTS.md
git commit -m "docs: 添加 Agent 仓库指引"
```

### Task 2: 建立持久上下文

**Files:**
- Create: `memory.md`
- Read: `AGENTS.md`
- Read: `profile/README.md`

**Step 1: 确认记忆文件尚不存在**

Run: `test ! -e memory.md`

Expected: 命令退出码为 0。

**Step 2: 创建最小持久记忆**

创建 `memory.md`，完整内容如下：

```markdown
# Repository Memory

## 使用方式

本文件为 Agent 提供稳定、精简的仓库上下文。仅记录长期有效的信息；页面正文以 `profile/README.md` 为准。

## 组织身份

- GitHub 组织：`XiyouMobile3G-iOS`
- 展示名称：`XiyouMobile3G - iOS`
- 方向：iOS 与 Apple 平台开发、学习资料、项目规范、开发工具和知识分享

## 仓库职责

- 仓库：`XiyouMobile3G-iOS/.github`
- 可见性：公开
- 默认分支：`main`
- 组织首页入口：`profile/README.md`
- 当前阅读体系仅服务本仓库，不作为跨仓库通用模板

## 长期决策

- `profile/README.md` 是组织首页内容的唯一事实来源。
- `AGENTS.md` 是 Agent 的仓库级入口，并强制阅读本文件。
- `memory.md` 不复制首页正文，只保留背景、决策和待确认事项。
- 面向访客的主要内容默认使用中文。
- 保持阅读体系轻量，不引入脚本或复杂文档层级。

## 待确认事项

- 组织首页的美化方向尚未选定。
- 组织简介、所在地、官网和公开邮箱等 GitHub 组织元数据目前未完善。

## 更新规则

- 组织定位、首页结构、目标受众或长期设计原则改变时更新本文件。
- 普通措辞、链接维护、标点和排版微调不记录。
- 禁止写入 Token、凭据、成员隐私或其他敏感信息。
```

**Step 3: 验证持久上下文结构**

Run: `rg -n '^# Repository Memory$|^## (使用方式|组织身份|仓库职责|长期决策|待确认事项|更新规则)$' memory.md`

Expected: 输出主标题和六个二级标题。

**Step 4: 提交记忆文件**

```bash
git add memory.md
git commit -m "docs: 添加仓库持久记忆"
```

### Task 3: 验证阅读链路与仓库状态

**Files:**
- Verify: `AGENTS.md`
- Verify: `memory.md`
- Verify: `profile/README.md`

**Step 1: 验证三个必读文件存在**

Run: `test -f AGENTS.md && test -f memory.md && test -f profile/README.md`

Expected: 命令退出码为 0。

**Step 2: 验证入口引用阅读链路**

Run: `rg -n 'memory\.md|profile/README\.md' AGENTS.md`

Expected: 两个路径都至少出现一次。

**Step 3: 检查 Markdown 与变更范围**

Run: `git diff HEAD~2 --check && git status --short --branch && git log -4 --oneline`

Expected: 无空白错误，工作区干净，历史中包含两个新文件的提交。
