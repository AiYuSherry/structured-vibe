# structured-vibe

**structured-vibe** 是一套面向 Vibe Coding 工作流的开发文档管理规范。它通过 **PRD（产品需求文档）、Changelog（变更记录）、Dev-Log（开发日志）、Todo List（待办清单）** 四类文档，为 AI 驱动的开发过程提供结构化的信息管理能力。

**structured-vibe** is a development-documentation workflow for Vibe Coding. It uses four document types, **PRD (Product Requirements Document), Changelog, Dev-Log, and Todo List**, to give AI-assisted development a structured way to manage requirements, changes, decisions, and follow-up tasks.

---

## 目录 / Table of Contents

- [背景 / Background](#背景--background)
- [解决方案 / Solution](#解决方案--solution)
- [文档体系 / Document System](#文档体系--document-system)
- [安装指南 / Installation](#安装指南--installation)
- [使用方式 / Usage](#使用方式--usage)
- [工作流程 / Workflow](#工作流程--workflow)
- [项目结构 / Project Structure](#项目结构--project-structure)
- [常见问题 / FAQ](#常见问题--faq)
- [贡献 / Contributing](#贡献--contributing)
- [许可证 / License](#许可证--license)

---

## 背景 / Background

Vibe Coding 的开发模式以自然语言向 AI 描述需求，再由 AI 直接生成代码。它显著提升效率，但也容易带来信息管理问题。

Vibe Coding lets developers describe requirements in natural language and ask AI to generate code directly. It can greatly improve speed, but it also introduces information-management problems.

| 问题 / Problem | 具体表现 / Symptoms |
| --- | --- |
| 需求边界模糊 / Unclear requirement boundaries | 需求未经结构化整理即进入编码阶段，中期发现理解偏差，导致反复修改。 Requirements enter implementation before being structured, causing misunderstandings and rework. |
| 变更缺乏追溯 / Poor change traceability | 功能的增删改无系统记录，事后复盘依赖记忆。 Feature additions, removals, and updates are not recorded systematically. |
| 开发上下文丢失 / Lost development context | 技术决策、踩坑记录随对话结束一同消失。 Technical decisions and lessons disappear when the conversation ends. |
| 待办事项散落 / Scattered todos | “后续版本处理”等约定散落在对话记录中，无法被有效追踪。 Follow-up items are buried in chat history and become hard to track. |

structured-vibe 针对上述问题提供了一套轻量级的文档管理方案，在不打断开发节奏的前提下，建立可追溯的开发记录体系。

structured-vibe provides a lightweight documentation system for these issues. It keeps development traceable without interrupting the speed of AI-assisted coding.

---

## 解决方案 / Solution

本规范的核心逻辑是：**先对齐需求，再实施编码**。在进入编码阶段之前，由 AI 将零散需求整理为结构化的 PRD 草案，经确认后再开始实现，从根本上减少因需求理解不一致导致的返工。

The core principle is: **align requirements before coding**. Before implementation begins, AI turns scattered requests into a structured PRD draft. The user confirms the scope first, reducing rework caused by requirement mismatch.

在此基础上，通过四类文档覆盖开发全流程的信息管理需求。

The workflow then uses four document types to cover the full development lifecycle.

```text
开工前 ──────→ 开发中 ──────→ 交付时 ──────→ 复盘时
审阅 PRD        AI 自动写入     Changelog      翻阅 Dev-Log
对齐需求范围     Dev-Log        自动追加        回溯历史决策

Before work ─→ During dev ──→ Delivery ────→ Review
Review PRD     AI writes       Changelog     Read Dev-Log
Align scope    Dev-Log         is appended   Trace decisions
```

---

## 文档体系 / Document System

### PRD（产品需求文档）/ PRD (Product Requirements Document)

产品定义与功能规格的权威记录。每份 PRD 需明确以下内容：

The authoritative record for product definition and feature specifications. Each PRD should define:

- 功能范围（含明确的包含与不包含边界）/ Feature scope, including explicit in-scope and out-of-scope boundaries
- 用户交互方式 / User interaction model
- 技术选型及依据 / Technical choices and rationale
- 验收标准 / Acceptance criteria

**文件命名规则 / Naming rule:**

```text
{YYMMDD} PRD-v{major}.{minor}-{status}-{product-name}.md
```

其中 `YYMMDD` 是 6 位日期前缀，例如 2026-06-09 写作 `260609`。

`YYMMDD` is a six-digit date prefix. For example, 2026-06-09 becomes `260609`.

**管理策略 / Management strategy:**

- 项目根目录仅保留一份当前生效的 PRD / Keep only one active PRD in the project root
- 历史版本自动归档至 `Archive/` 目录 / Archive historical versions into `Archive/`
- 伴随功能版本升级进行更新 / Update the PRD when the feature version changes

### Changelog（变更记录）/ Changelog

按时间顺序堆叠的变更摘要。每次交付时自动追加，记录变更内容与变更原因。

A chronological summary of changes. It is appended on delivery and records what changed and why.

```markdown
## [v1.2.0] - 2026-06-07

### Added
- 新增用户登录功能，采用邮箱加验证码方式
- Added email-and-code login

### Changed
- 首页布局改为双栏设计，适配移动端
- Changed the homepage to a two-column layout with mobile adaptation

### Fixed
- 修复搜索框在 iOS 下的白屏问题
- Fixed a white-screen issue for the search box on iOS
```

### Dev-Log（开发日志）/ Dev-Log

开发过程的时间序列记录。用于保存技术决策上下文、踩坑记录、方案对比等信息。格式自由，以清晰传递上下文为原则。

A chronological development log. It preserves technical decisions, lessons learned, tradeoffs, and debugging context. The format is flexible as long as the context is clear.

```markdown
## 2026-06-07

- 尝试使用 WebSocket 实现实时同步，但目标场景以离线操作为主，改为 Service Worker 方案
- Tried WebSocket for real-time sync, but the target workflow is mostly offline, so switched to a Service Worker approach

- IndexedDB 写入性能在 1 MB 以上数据量存在瓶颈，临时方案改为分块存储
- IndexedDB writes became slow above 1 MB, so the temporary solution uses chunked storage
```

### Todo List（待办清单）/ Todo List

待办事项的分层管理列表。按优先级归类，已完成事项移入已完成区并标注完成日期与结果。

A prioritized task list. Items are grouped by priority, and completed tasks are moved into the completed section with the completion date and result.

```markdown
## 高优先级 / High Priority

- [ ] 用户注册流程优化 — 减少必填字段
- [ ] Improve user registration flow — reduce required fields

## 中优先级 / Medium Priority

- [ ] 深色模式适配
- [ ] Dark mode adaptation

## 已完成 / Completed

- [x] 登录页重构 — 2026-06-05
- [x] Refactor login page — 2026-06-05
```

---

## 安装指南 / Installation

### 前置条件 / Prerequisites

- 已安装 [Claude Code](https://claude.ai/code) / [Claude Code](https://claude.ai/code) is installed
- Claude Code 支持 Skill 系统 / Claude Code supports the Skill system

### 安装步骤 / Steps

```bash
# 克隆仓库 / Clone the repository
git clone https://github.com/AiYuSherry/structured-vibe.git

# 安装 Skill（核心）/ Install the Skill file
cp structured-vibe/SKILL.md ~/.claude/skills/

# 安装 Hook 配置（可选，用于提交提醒）/ Install hook settings (optional, for commit reminders)
cp structured-vibe/hook-settings.local.json ~/.claude/
```

### 验证方式 / Verification

在 Claude Code 中提出开发类需求（如“创建一个新功能”），Skill 应自动触发并引导完成 PRD 编写流程。

Ask Claude Code for a development task, such as “create a new feature”. The Skill should trigger automatically and guide the PRD drafting flow.

> 本 Skill 为自动触发设计，无需手动调用。  
> This Skill is designed to trigger automatically. Manual invocation is not required.

---

## 使用方式 / Usage

### 新项目启动 / Starting a New Project

```text
用户：帮我做一个番茄钟应用
系统：正在整理 PRD 草案……
      生成 PRD 结构，请逐条确认功能范围、交互方式与技术方案
用户：确认以上内容，开始开发
系统：PRD 已创建，开始编码。Dev-Log 记录已就绪。

User: Help me build a Pomodoro app.
System: Drafting a PRD...
        A PRD structure has been generated. Please confirm the feature scope, interactions, and technical plan.
User: Confirmed. Start development.
System: PRD created. Development begins. Dev-Log is ready.
```

### 功能迭代 / Feature Iteration

```text
用户：给番茄钟增加白噪音功能
系统：检测到功能变更，更新 PRD 版本……
      请确认变更范围，确认后开始实现并记录决策过程

User: Add white noise to the Pomodoro app.
System: Feature change detected. Updating the PRD version...
        Please confirm the change scope. After confirmation, implementation will begin and decisions will be recorded.
```

### 交付 / Delivery

```text
用户：今日开发完成
系统：自动追加 Changelog，整理 Dev-Log，归档旧版 PRD

User: Today's development is complete.
System: Changelog appended, Dev-Log organized, and old PRD archived.
```

---

## 工作流程 / Workflow

```text
传统 Vibe Coding：
  需求描述 → 编码 → 反复修改 → 对话结束 → 上下文丢失

Traditional Vibe Coding:
  Request → Code → Rework → Conversation ends → Context is lost

使用 structured-vibe：
  需求描述 → PRD 对齐 → 编码 → 自动记录 → 交付归档 → 可追溯

With structured-vibe:
  Request → PRD alignment → Code → Auto records → Delivery archive → Traceable history
```

### 建议的日常习惯 / Recommended Daily Habits

1. **开工前 / Before work**：用 5 分钟审阅 PRD，确认当日开发范围。 Spend five minutes reviewing the PRD and confirming the scope for the day.
2. **开发中 / During development**：AI 自动记录 Dev-Log，关键决策时可补充说明。 Let AI maintain the Dev-Log, and add notes for key decisions when needed.
3. **交付时 / At delivery**：确认 Changelog 记录完整。 Confirm that the Changelog is complete.
4. **复盘时 / During review**：查阅 Dev-Log 回溯历史决策，查阅 Todo List 确认后续计划。 Read the Dev-Log for past decisions and the Todo List for follow-up plans.

---

## 项目结构 / Project Structure

```text
structured-vibe/
├── README.md                   # 项目说明 / Project documentation
├── SKILL.md                    # Claude Code Skill 指令（核心文件）/ Claude Code Skill instructions
└── hook-settings.local.json    # Claude Code Hook 配置 / Claude Code hook settings
```

---

## 常见问题 / FAQ

**Q：这套规范是否会拖慢开发节奏？**  
**Q: Will this workflow slow down development?**

不会。PRD 审阅通常只需数分钟，但其明确需求范围的作用能够节省数小时的返工时间。Dev-Log 与 Changelog 由 AI 自动维护，不产生额外操作成本。

No. Reviewing a PRD usually takes only a few minutes, and clear requirements can save hours of rework. Dev-Log and Changelog are maintained by AI, so they add little manual overhead.

**Q：个人开发者是否有必要使用？**  
**Q: Is this useful for solo developers?**

有必要。个人开发者缺乏团队协作中的信息同步机制，文档是唯一能够穿越时间传递开发上下文的信息载体。

Yes. Solo developers do not have the same shared context mechanisms as teams. Documentation becomes the main way to preserve development context over time.

**Q：文档是否需要手动维护？**  
**Q: Do these documents need to be maintained manually?**

PRD 由 AI 生成草案后需用户确认或修改；Dev-Log 与 Changelog 由 AI 自动维护；Todo List 建议定期整理。

AI drafts the PRD, and the user confirms or edits it. Dev-Log and Changelog are maintained by AI. Todo List should be reviewed regularly.

### 日期与 Todo 规则 / Date and Todo Rules

- 活跃 PRD、Changelog、Dev-Log、Todo List、测试清单统一使用 `YYMMDD` 文件名前缀。 Active PRD, Changelog, Dev-Log, Todo List, and test checklists use the `YYMMDD` filename prefix.
- 每轮改动前先检查日期；跨天继续开发时，先把活跃文档重命名到今天前缀，再写新内容。 Check the date before each round of changes. If work continues on a new day, rename active documents to today's prefix before writing new content.
- 如果项目 Todo 使用“长期待办 / 每日待办”结构，完成长期待办时要同步把原提出日期的每日待办打勾并补完成结果。 If the project uses “long-term todos / daily todos”, mark the original daily todo as done and add the result when a long-term item is completed.

**Q：如何与既有项目配合？**  
**Q: How does this work with existing projects?**

将 `SKILL.md` 放入项目 `.claude/skills/` 目录即可。Skill 会自动检测项目状态，存在 PRD 则更新，不存在则新建。

Place `SKILL.md` into the project's `.claude/skills/` directory. The Skill will detect the project state automatically: update the PRD if one exists, or create a new one if not.

---

## 贡献 / Contributing

欢迎通过 Issue 和 Pull Request 参与贡献。

Issues and pull requests are welcome.

1. Fork 本仓库 / Fork this repository
2. 创建特性分支 / Create a feature branch: `git checkout -b feat/my-feature`
3. 提交变更 / Commit changes: `git commit -m 'feat: add some feature'`
4. 推送分支 / Push the branch: `git push origin feat/my-feature`
5. 提交 Pull Request / Open a pull request

---

## 许可证 / License

[MIT](LICENSE) © AiYuSherry
