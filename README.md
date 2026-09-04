<p align="center">
  <img src="docs/banner.svg" alt="zengbohan-skill" width="100%" />
</p>

<h1 align="center">zengbohan-skill</h1>

<p align="center">
  A self-contained, five-stage development pipeline for AI coding agents — one folder, zero companion skills.
</p>

<p align="center">
  <a href="zengbohan-skill/SKILL.md"><img src="https://img.shields.io/badge/Agent-Skills-2F80ED?style=flat-square" alt="Agent Skills" /></a>
  <img src="https://img.shields.io/badge/ZCode-supported-1769AA?style=flat-square" alt="ZCode" />
  <img src="https://img.shields.io/badge/Claude_Code-supported-5C6BC0?style=flat-square" alt="Claude Code" />
  <img src="https://img.shields.io/badge/Codex-via_AGENTS.md-F2994A?style=flat-square" alt="Codex" />
  <img src="https://img.shields.io/badge/Workflow-5%20stages-1769AA?style=flat-square" alt="Five stages" />
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-4EB1BA?style=flat-square" alt="MIT License" /></a>
</p>

<p align="center">
  English | <a href="README.zh-CN.md">简体中文</a>
</p>

## What it does

`zengbohan-skill` is a single entry point for development work. It keeps AI-assisted changes grounded in explicit decisions, small deliverables, tests, and review.

The pipeline is:

| Stage | Definition | What happens |
| --- | --- | --- |
| 0 | `stages/setup-matt-pocock-skills/` | First run only: configure the project's issue tracker and doc layout |
| 1 | `stages/grill-with-docs/` | Interview you in rounds until every design decision is settled; leaves a glossary (`CONTEXT.md`) and ADRs behind |
| 2 | `stages/to-spec/` | Synthesize the decisions into a spec and publish it to the tracker |
| 3 | `stages/to-tickets/` | Split the spec into independently verifiable tickets with blocking edges; you approve the breakdown |
| 4 | `stages/implement/` + `stages/tdd/` | Build each ticket test-first, one fresh context window per ticket, one commit each |
| 5 | `stages/code-review/` | Two-axis review (coding standards + spec fidelity); fix findings until clean |

The entry file (`SKILL.md`) drives each stage by reading its definition straight from `stages/`. Stage files are ordinary Markdown documents, not registered skills.

## Self-contained by design

Earlier releases required installing ten companion skills alongside this one. They are now **embedded under `stages/`**, so installing this single folder is enough on any harness. Nothing else to install, nothing to fall out of sync.

## Install

The skill follows the [Agent Skills](https://agentskills.io) convention: a folder whose `SKILL.md` carries YAML frontmatter (`name`, `description`). Harnesses that scan a skills directory discover it automatically.

### ZCode

```bash
git clone https://github.com/zeng-bohan/zengbohan-skill.git
cp -R zengbohan-skill/zengbohan-skill ~/.zcode/skills/
```

Loaded from `~/.zcode/skills/zengbohan-skill/SKILL.md`.

### Claude Code

```bash
git clone https://github.com/zeng-bohan/zengbohan-skill.git
mkdir -p ~/.claude/skills
cp -R zengbohan-skill/zengbohan-skill ~/.claude/skills/
```

Loaded from `~/.claude/skills/zengbohan-skill/SKILL.md`.

### Codex CLI

Codex has no native skills directory, so wire it in through `AGENTS.md`:

```bash
git clone https://github.com/zeng-bohan/zengbohan-skill.git ~/.codex/zengbohan-skill
```

Then add this block to `~/.codex/AGENTS.md` (or the repo-level `AGENTS.md`):

```markdown
## Development workflow

For every non-trivial development task, first read ~/.codex/zengbohan-skill/SKILL.md
and follow its five-stage pipeline exactly, loading the stage files it references
under stages/.
```

### Any other harness

The skill is plain Markdown plus supporting files. Clone it anywhere your agent can read, then point your harness's always-on instruction file at the entry definition:

```markdown
Before any development task, read <path-to>/zengbohan-skill/SKILL.md and follow it.
```

This works for Cursor rules, Windsurf, Cline, opencode, or anything that can inject instructions.

> **Windows:** replace `~/` with `%USERPROFILE%\` in the paths above.

Restart or refresh your harness after installation if the skill does not appear immediately.

## Use

Invoke it explicitly:

```text
/zengbohan-skill Add citation tracing to the RAG answer pipeline
```

…or just describe the task naturally — the description matches phrases like "develop", "implement", "let's build", 开发 / 实现 / 开始做 / 按流程走:

```text
按流程走，给导出功能加一个 CSV 后端
```

You stay in control at the decision points: interview rounds pause for your answers, test seams need your sign-off, and the ticket breakdown is published only after you approve it. Stages 1–3 share one context window so the design thinking stays connected; stage 4 restarts fresh per ticket so implementation never drowns in interview history.

## Repository layout

```text
zengbohan-skill/          ← this repo
├── README.md
├── LICENSE
├── docs/
│   └── banner.svg
└── zengbohan-skill/      ← copy THIS folder into your skills directory
    ├── SKILL.md          ← entry point — the only registered skill
    ├── cover.jpg
    └── stages/           ← ten embedded stage definitions (plain docs, not registered skills)
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

Only the top-level `SKILL.md` is named `SKILL.md`, so harnesses register exactly one skill. Each stage folder keeps its own reference documents (`PHASE-BOUNDARIES.md`, `ADR-FORMAT.md`, `tests.md`, tracker templates, …), which the pipeline reads when the corresponding stage runs.

## Design principles

- **Design before code.** Make important decisions explicit.
- **Ship vertical slices.** Every ticket should be independently verifiable.
- **Test the seam.** Prefer tests that exercise the real boundary and failure mode.
- **Add complexity when evidence requires it.** Use architecture and diagnosis tools when the codebase earns them.

## Credits

The ten embedded stages are consolidated from Matt Pocock's engineering skill suite (`grilling`, `to-spec`, `to-tickets`, `implement`, `tdd`, `code-review`, `ask-matt`, `domain-modeling`, and friends). Packaging, consolidation, and the self-contained single-entry design by Bohan Zeng.

## License

[MIT](LICENSE) © 2026 Bohan Zeng
