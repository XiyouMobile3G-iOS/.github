# Organization Homepage Refresh Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** 在不增加栏目或图片素材的前提下，将组织首页重排为清晰、克制、面向 iOS 学习者的 Apple 风极简页面。

**Architecture:** `profile/README.md` 继续作为首页唯一事实来源，只调整现有内容的层级、措辞和间距。`memory.md` 同步保存已确定的长期视觉决策，并移除已经解决的待确认事项。

**Tech Stack:** GitHub Flavored Markdown、GitHub README 安全 HTML、Git

---

### Task 1: 重排组织首页

**Files:**
- Modify: `profile/README.md`
- Read: `AGENTS.md`
- Read: `memory.md`
- Read: `docs/plans/2026-08-08-organization-homepage-refresh-design.md`

**Step 1: 验证新首页尚未实现**

Run: `! rg -q '探索 Apple 平台。沉淀工程实践。构建可交付的产品。' profile/README.md`

Expected: 命令退出码为 0，证明新首屏标语尚不存在。

**Step 2: 用最小实现替换首页正文**

将 `profile/README.md` 完整替换为：

```markdown
<div align="center">
  <a href="https://github.com/XiyouMobile3G-iOS">
    <img src="https://avatars.githubusercontent.com/u/268368317?v=4" alt="XiyouMobile3G iOS" width="96" />
  </a>

  <h1>XiyouMobile3G · iOS</h1>
  <p><strong>探索 Apple 平台。沉淀工程实践。构建可交付的产品。</strong></p>
  <p>
    <a href="#精选项目">浏览项目</a>
    &nbsp;·&nbsp;
    <a href="#知识沉淀">查看知识库</a>
    &nbsp;·&nbsp;
    <a href="#一起构建">参与共建</a>
  </p>
</div>

---

## 关于我们

这里汇集与 iOS 开发相关的学习资料、项目规范和开发工具。

我们从代码与原理出发，在持续学习、实践与复盘中积累对 Apple 平台开发的理解。

## 精选项目

| 项目 | 内容 |
| --- | --- |
| [Weather-Forecast-Requests](https://github.com/XiyouMobile3G-iOS/Weather-Forecast-Requests) | 天气预报功能的需求、实现要求与核验资料。 |
| [Raycast-Apple-Developer-Docs](https://github.com/XiyouMobile3G-iOS/Raycast-Apple-Developer-Docs) | 面向 Apple 开发者文档的 Raycast 检索工具。 |
| [3G-share-request-source](https://github.com/XiyouMobile3G-iOS/3G-share-request-source) | 3G Share 的需求说明与配套资源。 |
| [books](https://github.com/XiyouMobile3G-iOS/books) | iOS 与软件开发相关的技术阅读资源。 |

## 知识沉淀

- [**awesome-ios-interview**](https://github.com/XiyouMobile3G-iOS/awesome-ios-interview)：系统整理从基础到进阶的 iOS 面试知识。
- [**tips**](https://github.com/XiyouMobile3G-iOS/tips)：记录 iOS 学习与实践过程中值得沉淀的知识点。

## 关注方向

`iOS` · `Objective-C` · `Apple Developer Documentation` · `Developer Tools` · `Knowledge Sharing`

## 一起构建

欢迎通过 [Issues](https://github.com/orgs/XiyouMobile3G-iOS/issues) 提出建议、补充资料或分享实践。

<div align="center">
  <sub>Designed for clarity.<br />Built for learning.</sub>
</div>
```

**Step 3: 验证页面内容契约**

Run: `rg -n '^## (关于我们|精选项目|知识沉淀|关注方向|一起构建)$|探索 Apple 平台|href="#精选项目"|href="#知识沉淀"|href="#一起构建"' profile/README.md`

Expected: 输出五个既有二级标题、新标语和三个页内导航链接。

**Step 4: 验证范围和空白**

Run: `git diff HEAD --check && git diff --name-only HEAD`

Expected: 无空白错误，只有 `profile/README.md`。

**Step 5: 提交首页重排**

```bash
git add profile/README.md
git commit -m "docs: 美化组织首页"
```

### Task 2: 同步长期视觉决策

**Files:**
- Modify: `memory.md`
- Read: `AGENTS.md`
- Read: `profile/README.md`

**Step 1: 验证记忆仍是旧状态**

Run: `rg -q '组织首页的美化方向尚未选定' memory.md && ! rg -q 'Apple 风极简黑白排版' memory.md`

Expected: 命令退出码为 0。

**Step 2: 更新长期决策和待确认事项**

在 `## 长期决策` 末尾增加：

```markdown
- 组织首页采用 Apple 风极简黑白排版：品牌区居中、正文左对齐，不使用装饰横幅、Emoji 或复杂 HTML。
- 首页美化只重排现有栏目，不新增内容栏目或图片素材。
```

删除 `## 待确认事项` 中的：

```markdown
- 组织首页的美化方向尚未选定。
```

将 `## 更新规则` 第一项替换为：

```markdown
- 组织定位、首页结构、目标受众、长期设计原则或组织与仓库元数据变化时更新本文件。
```

**Step 3: 验证记忆同步**

Run: `rg -n 'Apple 风极简黑白排版|只重排现有栏目|组织与仓库元数据变化' memory.md && ! rg -q '美化方向尚未选定' memory.md`

Expected: 三条新规则均被找到，旧待确认项不存在。

**Step 4: 验证范围和空白**

Run: `git diff HEAD --check && git diff --name-only HEAD`

Expected: 无空白错误，只有 `memory.md`。

**Step 5: 提交记忆更新**

```bash
git add memory.md
git commit -m "docs: 记录组织首页视觉方向"
```

### Task 3: 验证最终首页

**Files:**
- Verify: `profile/README.md`
- Verify: `memory.md`
- Verify: `AGENTS.md`

**Step 1: 验证文件与阅读链路**

Run: `test -f AGENTS.md && test -f memory.md && test -f profile/README.md && rg -q 'memory\.md' AGENTS.md && rg -q 'profile/README\.md' AGENTS.md`

Expected: 命令退出码为 0。

**Step 2: 验证首页结构没有新增栏目**

Run: `test "$(rg -c '^## ' profile/README.md)" -eq 5`

Expected: 命令退出码为 0，首页仍有五个正文二级标题。

**Step 3: 验证链接目标存在**

依次运行：

```bash
gh api repos/XiyouMobile3G-iOS/Weather-Forecast-Requests --jq .html_url
gh api repos/XiyouMobile3G-iOS/Raycast-Apple-Developer-Docs --jq .html_url
gh api repos/XiyouMobile3G-iOS/3G-share-request-source --jq .html_url
gh api repos/XiyouMobile3G-iOS/books --jq .html_url
gh api repos/XiyouMobile3G-iOS/awesome-ios-interview --jq .html_url
gh api repos/XiyouMobile3G-iOS/tips --jq .html_url
```

Expected: 六个命令均输出对应仓库的 GitHub URL。

**Step 4: 验证完整功能差异**

Run: `git diff main...HEAD --check && git status --short --branch && git diff --stat main...HEAD`

Expected: 无空白错误，工作区干净；完整分支包含 Agent 阅读体系、首页设计文档、实施计划、首页重排和记忆更新。
