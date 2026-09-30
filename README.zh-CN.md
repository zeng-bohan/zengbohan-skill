<p align="center">
  <img src="docs/banner.svg" alt="zengbohan-skill" width="100%" />
</p>

<h1 align="center">zengbohan-skill</h1>

<p align="center">
  面向 AI 编码代理的自包含三层开发流水线 — 一个文件夹，零伴随技能。
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-4EB1BA?style=flat-square" alt="MIT License" /></a>
</p>

<p align="center">
  <a href="README.md">English</a> | 简体中文
</p>

## 它做什么

`zengbohan-skill` 是开发工作的唯一入口 — 一条流水线，两种深度。每个功能都走同样的四个阶段；你选的档位决定每个阶段走多深，让流程的仪式感随工作量伸缩，而不是反客为主。

| 档位 | 适用 | 执行内容 |
| --- | --- | --- |
| **micro** | 错别字、文案微调、单行修复 | 直接修，不走流水线 |
| **standard**（默认） | 一个上下文窗口 / 一次坐下来能完成 | 并行探索仓库 → 一轮访谈 → 一页纸计划 + **单次批准** → 会话内 TDD 构建 → 一轮评审 |
| **full** | 跨会话或跨天 | 多轮拷问 → 双门计划（先内容、后切片）→ 逐切片构建 → 双轴评审 |

门槛是刻意设计的：standard 只收 **一次** 批准（计划在一次交互里覆盖接缝、切片粒度与阻塞边）；full 收 **两次**（先批计划内容，再批切片拆分）。环境事实永远不会以问题的形式回到你这里 — 只有决策会，且每条都附推荐答案。

底下的四个阶段：

| 阶段 | 定义文件 | 做什么 |
| --- | --- | --- |
| 1 | `stages/interview.md` | 并行探索代码库，然后进行设计树访谈 — 不落盘任何东西 |
| 2 | `stages/plan.md` | 把决策综合成计划文件（故事、接缝、带 `Verify:` 行的切片）— 一次批准，full 模式则两道门 |
| 3 | `stages/implement.md` | 按预定接缝逐切片测试先行构建，每个切片一次提交，随手勾掉计划项 |
| 4 | `stages/code-review.md` | 双轴评审（编码规范 + 计划还原度），最后交给你一份验收清单签字 |

入口文件（`SKILL.md`）负责选档与路由；阶段规则只存在于 `stages/`。阶段文件是普通 Markdown 文档，不是注册技能。

## 一次运行长什么样

```text
> /zengbohan-skill 给 RAG 问答链路加上引用溯源

zengbohan-skill · 档位：standard（一次批准，然后开工）

▍ 访谈   并行探索了 3 个区域；有 2 个决策需要你拍板：
         · 引用挂在哪里（答案 token 还是报告章节）？
         · 什么算一条溯源？→ 已给推荐方案
▍ 计划   .scratch/2026-09-30-citation-tracing.md — 3 个切片，
         各带 Verify: 行 · 批准后继续 (y/n)
▍ 实现   切片 1/3：生成器接缝先写失败测试 → 实现 →
         提交 …… 切片 2-3 同样处理，随手勾掉计划项
▍ 评审   双轴评审（规范 + 计划还原度）→ 验收清单
         交回给你签字
```

技能在回复第一行报出档位。决策点始终由你掌控：访谈轮次停下来等你的回答，计划文件只有在你批准后才会写出。访谈与计划共享同一个上下文窗口，让设计思考保持连贯；实现默认在同一会话继续，只在上下文吃紧时才拆分。

## 技术栈

无运行时、无构建步骤、无依赖 — 技能就是纯 Markdown 加配套文件，打包成单个文件夹：

| 组成 | 说明 |
| --- | --- |
| `SKILL.md` | 入口 — 档位分发、路由、退出条件（YAML frontmatter：`name`、`description`） |
| `stages/*.md` | 四个阶段定义（规则只在这里） |
| `references/*.md` | 阶段引用的操作手册 |
| 遵循约定 | [Agent Skills](https://agentskills.io) — 任何扫描技能目录的宿主都会自动发现它 |

## 自包含设计

早期版本需要随本技能一起安装十个伴随技能。现在它们已 **内嵌并合并到 `stages/` 下**，任何宿主装这一个文件夹就够。没有别的东西要装，也不会再有东西失步。

## 安装

仓库根目录 **就是** 技能文件夹 — 直接克隆进技能目录即可。

### Claude Code

```bash
git clone https://github.com/zeng-bohan/zengbohan-skill.git ~/.claude/skills/zengbohan-skill
```

### ZCode

```bash
git clone https://github.com/zeng-bohan/zengbohan-skill.git ~/.zcode/skills/zengbohan-skill
```

### Codex CLI

Codex 没有原生的技能目录，通过 `AGENTS.md` 接入：

```bash
git clone https://github.com/zeng-bohan/zengbohan-skill.git ~/.codex/zengbohan-skill
```

然后把这段加进 `~/.codex/AGENTS.md`（或仓库级 `AGENTS.md`）：

```markdown
## Development workflow

When the user invokes the zengbohan-skill pipeline (e.g. "/zengbohan-skill <task>"),
first read ~/.codex/zengbohan-skill/SKILL.md and follow its three-tier pipeline,
loading the stage files it references under stages/.
```

### 其他宿主

技能是纯 Markdown 加配套文件。克隆到你的代理能读到的任何位置，然后在宿主的常驻指令文件里指向入口定义：

```markdown
When the user invokes the zengbohan-skill pipeline (e.g. "/zengbohan-skill <task>"),
read <path-to>/zengbohan-skill/SKILL.md and follow it.
```

Cursor rules、Windsurf、Cline、opencode，任何能注入指令的宿主都适用。

## 使用

```text
/zengbohan-skill 给 RAG 问答链路加上引用溯源
```

## 注意事项与避坑

- **只认显式调用。** 技能不会在「develop」「按流程走」这类自然语言触发词上自动启动 — 想走流水线，请按名字调用。
- **装完要重启。** 技能没有立即出现的话，重启或刷新你的宿主。
- **Windows 路径。** 把上面所有路径里的 `~/` 换成 `%USERPROFILE%\`。
- **一个产物，不是一堆文书。** 流水线只产出唯一一个文件 — `.scratch/` 下的计划；其余都在对话里。没有术语表、ADR、规格文档或票务清单。
- **注册的技能只有一个。** 只有顶层 `SKILL.md` 叫这个名字；阶段文件是普通 Markdown，宿主只会注册一个技能。
- **上游技能不内嵌。** `triage`、`prototype`、`pr`、`diagnosing-bugs`、`wayfinder`、`wizard` 等持续增长的 mattpocock 技能刻意不收 — 需要就从 [mattpocock/skills](https://github.com/mattpocock/skills) 安装。

## 仓库结构

```text
zengbohan-skill/          ← 仓库即技能；把这个文件夹克隆进技能目录
├── SKILL.md              ← 入口 — 档位分发 + 路由 + 退出条件
├── AGENTS.md
├── LICENSE
├── docs/
│   └── banner.svg
├── stages/               ← 四个阶段定义（规则只在这里）
│   ├── interview.md
│   ├── plan.md
│   ├── implement.md
│   └── code-review.md
└── references/           ← 阶段引用的操作手册
    ├── PHASE-BOUNDARIES.md
    ├── tests.md
    └── mocking.md
```

## 设计原则

- **先设计后编码。** 把重要决策显式化 — 且只显式化重要的。
- **仪式感随工作量伸缩。** 门槛与流程按功能大小体现价值；一天的功能配一次批准，而不是一套自己的文书流程。
- **一个产物，不是一堆文书。** 流水线只产出唯一一个文件 — `.scratch/` 下的计划；其余都在对话里。
- **交付垂直切片。** 每个切片都应可独立验证。
- **测接缝。** 优先写打在真实边界与失败模式上的测试。
- **有证据才加复杂度。** 代码库配得上时才动用架构与诊断工具。

## 致谢

阶段定义整合自 Matt Pocock 的工程技能套件（[mattpocock/skills](https://github.com/mattpocock/skills)）— `grilling`、`domain-modeling`、`grill-with-docs`、`to-spec`、`to-tickets`、`implement`、`tdd`、`code-review`、`setup-matt-pocock-skills` 与 `ask-matt` 的阶段边界规则 — 最后同步于 2026-09-24。上游还在持续增长（`triage`、`prototype`、`pr`、`diagnosing-bugs`、`wayfinder`、`wizard`……）；这些刻意 **不** 内嵌 — 需要请从 mattpocock/skills 安装。打包、整合、三层流程与自包含单入口设计由 Bohan Zeng 完成。

## 贡献

个人维护项目。Bug、问题与功能建议：[提 Issue](https://github.com/zeng-bohan/zengbohan-skill/issues)。

## 许可证

[MIT](LICENSE) © 2026 Bohan Zeng
