# ai-project-director

[English README](README.md)

`ai-project-director` 是一个 Codex Skill，用来把本地 AI 协作项目变成一套分阶段、可审查、可恢复的工作流。

它不是一个“上来就写代码”的执行提示词，而是一个项目导演。它会先帮你澄清项目、讨论需求、建立可恢复的项目文件、拆任务路线图，再根据需要管理子线程、worktree、goal、heartbeat 自动推进和对抗性审查。

## 它解决什么问题

用 AI 做本地项目时，最常见的问题通常不是 AI 不会写代码，而是整个项目没有操作系统：

- 需求只留在聊天记录里
- 目标没澄清就开始实现
- 多个线程互相覆盖、互相干扰
- 完成结果没有证据就被接受
- 自动化在用户想暂停后还继续跑
- 新线程接手时不知道项目做到哪里

这个 Skill 把这些问题收束成一条可控流程：

```text
Intake
-> Discovery
-> Requirements
-> Harness
-> Roadmap
-> Orchestration
-> Adversarial Review
-> Heartbeat
-> Pause / Resume / Handoff
```

## 什么时候用

适合用在这些场景：

- 启动一个新的软件、内容、运营、自动化或 Skill 项目
- 在实现前先澄清一个模糊想法
- 讨论需求、MVP 范围和验收标准
- 为未来 AI 线程创建项目状态文件
- 把项目拆成阶段化任务卡
- 用多个 Codex 线程推进本地项目
- 用 Git worktree 或 goal 做隔离执行
- 设置 heartbeat 式自动推进
- 对完成结果做独立或加固后的对抗性审查
- 安全暂停、恢复和交接长期项目

不建议对单次小任务直接跑完整生命周期。若任务不超过 2 张任务卡、预计只改 1 个文件以内，并且不需要多线程或 heartbeat，应走 `SINGLE_TASK` 快速路径，或使用类似 `contract-first-loop` 的单任务工作流。

## 核心规则

这个 Skill 的核心是阶段门控：

```text
没有 Discovery Handoff -> 不写 requirements。
没有 Requirements Handoff -> 不建 roadmap。
没有 harness/status files -> 不启动多线程执行。
没有 acceptance criteria -> 不开始实现或 /goal。
没有独立审查或加固后的同线程审查 -> 不接受重要实现为完成。
没有 Git/worktree 隔离 -> 不在同一项目目录里并行运行多个写线程。
用户的 pause/stop 指令优先于所有自动化和进行中的任务。
```

## 工作流

### 0. Intake

先判断当前请求属于哪一类：

- 新项目
- 已有项目
- 恢复项目
- 单次任务
- 项目审查

这一阶段会检查项目路径、已有文件、Git 状态、状态文件和工具能力，再决定从哪个阶段开始。

### 1. Discovery

把粗略想法变成项目简报。

典型产物：

```text
docs/project-brief.md
docs/open-questions.md
```

### 2. Requirements

把项目简报转成可验证的需求、MVP 范围、非目标和验收标准。

典型产物：

```text
docs/requirements.md
feature_list.json
evaluator-rubric.md
```

非代码项目可以用 `TASKS.md` 替代 `feature_list.json`。

### 3. Harness

创建或补齐项目操作文件，让未来的新线程不依赖聊天记忆也能继续工作。

典型产物：

```text
AGENTS.md
CURRENT.md
HANDOFF.md
DECISIONS.md
THREADS.md
TASKS.md
docs/commands.md
docs/architecture.md
```

### 4. Roadmap

把工作拆成内部 Roadmap Stage：

```text
Roadmap Stage D0: 合同、schema、接口、数据模型或文档收敛
Roadmap Stage D1: 第一条可运行的纵向切片
Roadmap Stage D2: 功能扩展
Roadmap Stage D3: 体验、性能、内容、可靠性或细节打磨
Roadmap Stage D4: 发布、清理、最终证据和归档
```

每张任务卡都需要包含允许修改的文件、禁止修改的文件、验收标准、验证方式、依赖关系和审查要求。

### 5. Orchestration

通过子线程、worktree、goal 和状态文件管理执行。

线程角色：

```text
main-control
read-only-review
implementation
adversarial-review
repair
heartbeat
```

Git 项目优先用 worktree 隔离实现任务。非 Git 项目可以并行做只读审查，但不要在同一目录里并行运行多个写线程。

### 6. Adversarial Review

实现线程说“完成”只代表候选完成，不等于真正完成。

重要工作必须经过审查，重点检查：

- 是否满足全部验收标准
- 是否有可复现证据
- 是否改了禁止范围
- 是否沿用了过期假设
- 是否让旧项目方向回流
- 是否存在无法验证的声明
- 是否有回归风险

审查结果只使用：

```text
ACCEPTED
NEEDS_REPAIR
BLOCKED
NEEDS_USER_DECISION
INVALID_REVIEW
```

同线程审查只能作为降级备选，并且必须满足加固条件：至少 3 个 counterexample 尝试、至少 1 项可复现外部证据、说明为什么无法独立审查。否则不能给 `ACCEPTED`。

### 7. Heartbeat

创建或草拟定时推进项目的 heartbeat 自动化。

heartbeat 决策：

```text
DONT_NOTIFY
NOTIFY
ASK
PAUSE
STOP
```

heartbeat 应读取项目状态、检查子线程状态、把候选完成导向审查、更新状态文件，并避免在没有重要变化时打扰用户。

### 8. Pause, Resume, Handoff

手动暂停永远优先。

用户暂停时，Skill 应停止创建新线程，尽可能暂停 heartbeat，通知活跃子线程停止，并把 `CURRENT.md` / `HANDOFF.md` 标为 paused。

恢复时，必须从文件重建状态，而不是依赖聊天记忆。

## 安装

把这个文件夹复制到 Codex skills 目录。

Windows：

```powershell
Copy-Item -Recurse . "$env:USERPROFILE\.codex\skills\ai-project-director"
```

macOS 或 Linux：

```bash
cp -R . ~/.codex/skills/ai-project-director
```

然后按需要重启 Codex 或重新加载 skills。

## 使用示例

启动新项目：

```text
Use $ai-project-director to launch a new project.
Project idea: 我想做一个本地工具，把选品调研笔记变成短视频脚本。
Start from discovery. Do not write code yet.
```

恢复已有本地项目：

```text
Use $ai-project-director to resume this project.
Read CURRENT.md, HANDOFF.md, AGENTS.md, and THREADS.md first.
Tell me the current phase, blockers, and next safest action.
```

设置多线程执行：

```text
Use $ai-project-director to create a roadmap and decide which tasks can run in parallel.
Only start child threads after task cards, allowed files, forbidden files, and acceptance criteria are defined.
```

运行审查门：

```text
Use $ai-project-director to adversarially review the candidate work from Roadmap Stage D1.
Do not accept the implementation thread's self-report without evidence.
```

## 仓库结构

```text
ai-project-director/
├── README.md
├── README.zh-CN.md
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── 00-intake-routing.md
    ├── 01-discovery.md
    ├── 02-requirements.md
    ├── 03-harness.md
    ├── 04-roadmap.md
    ├── 05-orchestration.md
    ├── 06-adversarial-review.md
    ├── 07-heartbeat.md
    ├── 08-pause-resume.md
    ├── 09-status-schemas.md
    ├── 10-quality-audit.md
    └── 11-worked-example.md
```

`agents/openai.yaml` 是 Codex 用来展示 skill 名称、简介和默认提示词的 UI metadata。它不是模型供应商配置，也不保存项目状态。

## 与其他工作流的关系

这个 Skill 是顶层项目导演。

它可以按需调用更窄的工作流：

- 项目初始化模式：用于创建可恢复项目文件
- 单任务合同循环：用于一个边界清晰的小任务
- `/goal`：用于有明确完成条件、需要跨轮推进的任务
- 子线程和 worktree：用于隔离执行
- 对抗性审查线程：用于质量门控

## 安全说明

- 不要在没有明确暂停和停止条件时运行无人值守自动化。
- 不要在非 Git 项目目录里允许多个写线程并行工作。
- 删除生成产物前，先列出候选路径，并保护源素材。
- 不要默认复制 hidden、ignored 或本地密钥文件，除非用户确认。
- 没有审查和证据，不要接受重要工作为完成。

## 当前状态

初始公开版本。Skill 已通过本地 `quick_validate.py` 验证，并包含内部质量审查清单。
