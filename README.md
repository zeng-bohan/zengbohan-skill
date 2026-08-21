<p align="center">
  <img src="docs/banner.svg" alt="zengbohan-skill banner" width="100%" />
</p>

<p align="center">
  <strong>一套面向 AI 辅助开发的实用型 Workflow 配置方案</strong><br />
  <sub>A practical two-tier configuration for high-quality AI-assisted development.</sub>
</p>

<p align="center">
  <a href="https://github.com/zengbohan1/zengbohan-skill/blob/main/zengbohan-skill/SKILL.md">
    <img src="https://img.shields.io/badge/ZCode-SKILL.md-2F80ED?style=flat-square" alt="ZCode Skill" />
  </a>
  <img src="https://img.shields.io/badge/Workflow-5--stage-1769AA?style=flat-square" alt="Five-stage workflow" />
  <img src="https://img.shields.io/badge/Advanced-Architecture_%2B_Diagnosis-4C9F70?style=flat-square" alt="Advanced skills" />
  <a href="LICENSE">
    <img src="https://img.shields.io/badge/License-MIT-4EB1BA?style=flat-square" alt="MIT License" />
  </a>
</p>

<p align="center">
  <code>Grill-With-Docs</code> → <code>/to-spec</code> → <code>/to-tickets</code> → <code>/implement</code> → <code>/code-review</code>
</p>

**Topics:** `ai-coding` · `workflow` · `zcode` · `developer-tools` · `software-design` · `tdd` · `code-review`

## What is this?

`zengbohan-skill` 是一套基于 Matt Pocock 技能生态整理的开发工作流配置。它不试图让每个项目都加载几十个技能，而是先提供一条稳定的主路径，再在确有需要时叠加架构审查和系统调试能力。

> **Start small. Add depth only when the project earns it.**

这套配置解决的是一个实际问题：AI 可以很快写出代码，但如果没有设计收敛、任务切片、测试和复核，开发过程很容易变成“边猜边改”。本仓库把这些环节整理成一条可重复的闭环。

## Workflow at a glance

```mermaid
flowchart LR
    A([Development task]) --> B[Grill-With-Docs<br/>settle the design]
    B --> C[/to-spec<br/>publish the spec]
    C --> D[/to-tickets<br/>slice vertical tickets]
    D --> E[/implement<br/>build with TDD]
    E --> F[/code-review<br/>Standards + Spec]
    F --> G([Ship])

    B -. complex architecture .-> H[/improve-codebase-architecture/]
    B -. hard bug or regression .-> I[/diagnose/]
    H -. deepen the design .-> C
    I -. lock the regression .-> E
```

## Two-tier configuration

### 1. 通用极简配置 · Universal minimal setup

> 适用于约 90% 的开发任务。

```text
Grill-With-Docs → /to-spec → /to-tickets → /implement → /code-review
```

| Stage | Skill | Outcome |
| --- | --- | --- |
| 1 | `Grill-With-Docs` | 通过访谈让设计树收敛，并留下决策记录 |
| 2 | `/to-spec` | 将对话与已定设计综合成可执行 spec |
| 3 | `/to-tickets` | 拆成带阻塞边的 tracer-bullet tickets |
| 4 | `/implement` | 按 ticket 实现，并在约定 seam 使用 TDD |
| 5 | `/code-review` | 沿 Standards / Spec 两条轴复核改动 |

### 2. 进阶增强配置 · Advanced quality setup

在通用闭环基础上，保留两个王牌技能，专门处理“代码库已经变复杂”之后的短板。

#### `/improve-codebase-architecture`

系统性扫描代码库，寻找架构隐患、模块耦合和可测试性问题，并输出可视化架构报告。

适合：

- 代码库增长很快，模块之间开始互相泄漏；
- 测试越来越难写，正确的测试 seam 不明显；
- 某个 bug 暴露出更深层的架构纠缠。

#### `/diagnose`

用标准化六步闭环处理硬 bug、偶发失败和性能回归：

```text
复现 → 最小化 → 假设 → 仪器化 → 修复 → 回归
```

| Phase | Focus |
| --- | --- |
| Reproduce | 建立一个紧的、确定的、能针对真实症状报红的反馈环 |
| Minimise | 把 repro 缩小到仍然报红的最小场景 |
| Hypothesise | 先生成 3–5 个可证伪的 ranked hypotheses |
| Instrument | 每次只改变一个变量，并让探针对应一个预测 |
| Fix | 在正确的 seam 先写回归测试，再修改代码 |
| Regress | 重跑原始反馈环，确认修复覆盖真实场景 |

## How to choose

| Situation | Start here |
| --- | --- |
| 新功能、常规重构、一般开发任务 | 通用五阶段闭环 |
| 架构漂移、模块变浅、测试 seam 不清楚 | 先加 `/improve-codebase-architecture` |
| 硬 bug、偶发失败、性能回归 | 先加 `/diagnose` |
| 两种情况同时出现 | 先诊断真实症状，再做架构深化，最后回到主闭环 |

两个增强技能与主流程是**叠加关系**，不是替换关系。先保持主路径稳定，再按证据增加复杂度。

## Installation

### ZCode / local skill discovery

将技能目录放入 ZCode 的技能发现路径：

```bash
# User-level installation
mkdir -p ~/.zcode/skills
git clone https://github.com/zengbohan1/zengbohan-skill.git /tmp/zengbohan-skill
cp -R /tmp/zengbohan-skill/zengbohan-skill ~/.zcode/skills/
```

安装后，ZCode 会从以下文件加载技能定义：

```text
~/.zcode/skills/zengbohan-skill/SKILL.md
```

> 本仓库是**配置 skill**，不是上游技能的替代品。使用它时，仍需在环境中安装 `Grill-With-Docs`、`/to-spec`、`/to-tickets`、`/implement`、`/code-review` 及按需使用的增强技能。

## Repository layout

```text
zengbohan-skill/
├── README.md
├── LICENSE
├── docs/
│   └── banner.svg       # README 展示图
└── zengbohan-skill/
    ├── SKILL.md         # 可被 ZCode 发现的技能定义
    └── cover.jpg        # 原始配置图
```

## Design principles

- **默认简单**：先走五阶段闭环，不为尚未发生的问题引入流程负担。
- **设计先行**：在写代码前让关键设计决策显式化，并保留可追溯记录。
- **切片交付**：每个 ticket 都应当切过完整路径，完成后可以独立验证。
- **测试对准 seam**：测试真实的调用链和故障模式，而不是只测一个孤立函数。
- **用证据加复杂度**：架构审查和系统调试都由实际摩擦触发，而不是凭感觉启动。

## License

[MIT](LICENSE) © 2026 Bohan Zeng
