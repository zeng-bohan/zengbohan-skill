<p align="center">
  <img src="docs/banner.svg" alt="zengbohan-skill" width="100%" />
</p>

<h1 align="center">zengbohan-skill</h1>

<p align="center">
  面向 AI 编码智能体的自包含五阶段开发流水线——一个文件夹，零伴随技能。
</p>

<p align="center">
  <a href="README.zh-CN.md"><img src="https://img.shields.io/badge/%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87-%E6%9C%AC%E9%A1%B5-2F80ED?style=flat-square" alt="简体中文" /></a>
  <a href="README.md">English</a>
</p>

## 它做什么

`zengbohan-skill` 是开发工作的单一入口。它让 AI 辅助的改动始终锚在显式决策、小粒度交付、测试和评审上。

流水线如下：

| 阶段 | 定义文件 | 做什么 |
| --- | --- | --- |
| 0 | `stages/setup-matt-pocock-skills/` | 仅首次运行：配置项目的工单系统与文档布局 |
| 1 | `stages/grill-with-docs/` | 分轮访谈你，直到每个设计决策落定；沉淀术语表（`CONTEXT.md`）与 ADR |
| 2 | `stages/to-spec/` | 把决策综合成规格说明并发布到工单系统 |
| 3 | `stages/to-tickets/` | 把规格拆成可独立验证、带阻塞关系的工单；拆分方案由你批准 |
| 4 | `stages/implement/` + `stages/tdd/` | 每张工单测试先行，每票一个全新上下文窗口，一票一 commit |
| 5 | `stages/code-review/` | 双轴评审（编码规范 + 规格保真）；修到干净为止 |

入口文件（`SKILL.md`）直接从 `stages/` 读取各阶段定义来驱动流水线。阶段文件是普通 Markdown 文档，不是注册技能。

## 自包含设计

早期版本需要额外安装十个伴随技能。现在它们**内嵌在 `stages/` 下**，任何一个 harness 只装这一个文件夹就够。没有别的东西要装，也就没有东西会失同步。

## 安装

本技能遵循 [Agent Skills](https://agentskills.io) 约定：一个文件夹，其 `SKILL.md` 带 YAML frontmatter（`name`、`description`）。扫描技能目录的 harness 会自动发现它。

### ZCode

```bash
git clone https://github.com/zengbohan1/zengbohan-skill.git
cp -R zengbohan-skill/zengbohan-skill ~/.zcode/skills/
```

从 `~/.zcode/skills/zengbohan-skill/SKILL.md` 加载。

### Claude Code

```bash
git clone https://github.com/zengbohan1/zengbohan-skill.git
mkdir -p ~/.claude/skills
cp -R zengbohan-skill/zengbohan-skill ~/.claude/skills/
```

从 `~/.claude/skills/zengbohan-skill/SKILL.md` 加载。

### Codex CLI

Codex 没有原生技能目录，通过 `AGENTS.md` 接入：

```bash
git clone https://github.com/zengbohan1/zengbohan-skill.git ~/.codex/zengbohan-skill
```

然后把这段加进 `~/.codex/AGENTS.md`（或仓库级 `AGENTS.md`）：

```markdown
## Development workflow

For every non-trivial development task, first read ~/.codex/zengbohan-skill/SKILL.md
and follow its five-stage pipeline exactly, loading the stage files it references
under stages/.
```

### 其他任意 harness

本技能就是普通 Markdown 加配套文件。克隆到你的智能体读得到的任意位置，然后让 harness 的常驻指令文件指向入口定义：

```markdown
Before any development task, read <path-to>/zengbohan-skill/SKILL.md and follow it.
```

Cursor rules、Windsurf、Cline、opencode，任何能注入指令的都适用。

> **Windows：** 把上面路径里的 `~/` 换成 `%USERPROFILE%\`。

安装后如果技能没立刻出现，重启或刷新你的 harness。

## 使用

显式调用：

```text
/zengbohan-skill 给 RAG 问答链路加引用溯源
```

……或者自然描述任务——描述里匹配「develop」「implement」「let's build」、开发 / 实现 / 开始做 / 按流程走等说法：

```text
按流程走，给导出功能加一个 CSV 后端
```

决策点始终由你掌控：访谈轮次会停下来等你的回答，测试接缝需要你签字，工单拆分经你批准后才发布。阶段 1–3 共享同一个上下文窗口，让设计思考保持连贯；阶段 4 每票重开新会话，实现永远不会淹死在访谈历史里。

## 仓库布局

```text
zengbohan-skill/          ← 本仓库
├── README.md
├── LICENSE
├── docs/
│   └── banner.svg
└── zengbohan-skill/      ← 把这个文件夹复制进你的技能目录
    ├── SKILL.md          ← 入口——唯一注册的技能
    ├── cover.jpg
    └── stages/           ← 十个内嵌阶段定义（普通文档，非注册技能）
        ├── ask-matt/
        ├── setup-matt-pocock-skills/
        ├── grill-with-docs/
        ├── grilling/
        ├── domain-modeling/
        ├── to-spec/
        ├── to-tickets/
        ├── implement/
        ├── tdd/
        └── code-review/
```

只有顶层 `SKILL.md` 叫 `SKILL.md`，所以 harness 只注册这一个技能。每个阶段文件夹有自己的参考文档（`PHASE-BOUNDARIES.md`、`ADR-FORMAT.md`、`tests.md`、工单模板等），流水线在对应阶段运行时读取。

## 设计原则

- **先设计后编码。** 重要决策必须显式化。
- **交付垂直切片。** 每张工单都应可独立验证。
- **在接缝处测试。** 优先测真实边界与失败模式。
- **有证据才加复杂度。** 代码库配得上时才动用架构与诊断工具。

## 致谢

十个内嵌阶段整合自 Matt Pocock 的工程技能套件（`grilling`、`to-spec`、`to-tickets`、`implement`、`tdd`、`code-review`、`ask-matt`、`domain-modeling` 等）。打包、整合与自包含单入口设计由 Bohan Zeng 完成。

## 许可证

[MIT](LICENSE) © 2026 Bohan Zeng
