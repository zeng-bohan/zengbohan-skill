<p align="center">
  <img src="docs/banner.svg" alt="zengbohan-skill" width="100%" />
</p>

<h1 align="center">zengbohan-skill</h1>

<p align="center">
  A self-contained, three-tier development pipeline for AI coding agents — one folder, zero companion skills.
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-4EB1BA?style=flat-square" alt="MIT License" /></a>
</p>

<p align="center">
  English | <a href="README.zh-CN.md">简体中文</a>
</p>

## What it does

`zengbohan-skill` is a single entry point for development work — one pipeline, two depths. Every feature runs the same four stages; the tier you pick decides how deep each stage goes, so ceremony scales with the work instead of dwarfing it.

| Tier | When | What runs |
| --- | --- | --- |
| **micro** | typo, copy tweak, one-line fix | fixed directly, no pipeline |
| **standard** (default) | fits one context window / one sitting | one-round interview → one-pager plan with a **single approval** → in-session TDD build → one review pass |
| **full** | spans sessions or days | multi-round grilling → published spec → tracer-bullet tickets you approve → per-ticket build → two-axis review |

Gates, deliberately: standard takes **one** approval (the plan covers seams, ticket granularity, and blocking edges in one exchange); full takes **two** (spec, then ticket breakdown). Environment facts never come back to you as questions — only decisions do, each with a recommended answer.

The stages underneath:

| Stage | Definition | What happens |
| --- | --- | --- |
| 0 (on demand) | `stages/setup.md` | First time tracker artifacts are wanted: configure the issue tracker and doc layout |
| 1 | `stages/interview.md` | Design-tree interview; leaves a glossary (`CONTEXT.md`) and ADRs behind |
| 2 | `stages/plan.md` | Synthesize the decisions into a plan (one-pager, or spec + tickets) |
| 3 | `stages/implement.md` | Build each ticket test-first at the pre-agreed seams, one commit each |
| 4 | `stages/code-review.md` | Two-axis review (coding standards + spec fidelity) |

The entry file (`SKILL.md`) picks the tier and routes; stage rules live only in `stages/`. Stage files are ordinary Markdown documents, not registered skills.

## Self-contained by design

Earlier releases required installing ten companion skills alongside this one. They are now **embedded and consolidated under `stages/`**, so installing this single folder is enough on any harness. Nothing else to install, nothing to fall out of sync.

## Install

The repository root **is** the skill folder — clone it straight into a skills directory. The skill follows the [Agent Skills](https://agentskills.io) convention: a folder whose `SKILL.md` carries YAML frontmatter (`name`, `description`). Harnesses that scan a skills directory discover it automatically.

### Claude Code

```bash
git clone https://github.com/zeng-bohan/zengbohan-skill.git ~/.claude/skills/zengbohan-skill
```

### ZCode

```bash
git clone https://github.com/zeng-bohan/zengbohan-skill.git ~/.zcode/skills/zengbohan-skill
```

### Codex CLI

Codex has no native skills directory, so wire it in through `AGENTS.md`:

```bash
git clone https://github.com/zeng-bohan/zengbohan-skill.git ~/.codex/zengbohan-skill
```

Then add this block to `~/.codex/AGENTS.md` (or the repo-level `AGENTS.md`):

```markdown
## Development workflow

For every non-trivial development task, first read ~/.codex/zengbohan-skill/SKILL.md
and follow its three-tier pipeline, loading the stage files it references under stages/.
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

The skill announces its tier in the first line of its reply. You stay in control at the decision points: interview rounds pause for your answers, and the plan (or spec + ticket breakdown) is published only after you approve it. Interview and plan share one context window so the design thinking stays connected; implementation continues in the same session by default and only splits when the context runs low.

## Repository layout

```text
zengbohan-skill/          ← the repo IS the skill; clone this folder into a skills directory
├── SKILL.md              ← entry point — tier dispatch + routing + exit conditions
├── AGENTS.md
├── LICENSE
├── docs/
│   └── banner.svg
├── stages/               ← the five stage definitions (rules live here, nowhere else)
│   ├── setup.md
│   ├── interview.md
│   ├── plan.md
│   ├── implement.md
│   └── code-review.md
└── references/           ← formats and operational recipes the stages point at
    ├── PHASE-BOUNDARIES.md
    ├── CONTEXT-FORMAT.md
    ├── ADR-FORMAT.md
    ├── tests.md
    ├── mocking.md
    ├── issue-tracker-github.md
    ├── issue-tracker-gitlab.md
    ├── issue-tracker-local.md
    ├── triage-labels.md
    └── domain.md
```

Only the top-level `SKILL.md` is named `SKILL.md`, so harnesses register exactly one skill.

## Design principles

- **Design before code.** Make important decisions explicit — and only the important ones.
- **Ceremony scales with the work.** Gates, docs, and process earn their keep per feature size; a one-day feature gets one approval, not a paper trail of its own.
- **Ship vertical slices.** Every ticket should be independently verifiable.
- **Test the seam.** Prefer tests that exercise the real boundary and failure mode.
- **Add complexity when evidence requires it.** Use architecture and diagnosis tools when the codebase earns them.

## Credits

The stage definitions are consolidated from Matt Pocock's engineering skill suite ([mattpocock/skills](https://github.com/mattpocock/skills)) — `grilling`, `domain-modeling`, `grill-with-docs`, `to-spec`, `to-tickets`, `implement`, `tdd`, `code-review`, `setup-matt-pocock-skills`, and `ask-matt`'s phase-boundary rules — last synced 2026-09-24. Upstream keeps growing (`triage`, `prototype`, `pr`, `diagnosing-bugs`, `wayfinder`, `wizard`, …); those are deliberately **not** embedded here — install them from mattpocock/skills if you want them. Packaging, consolidation, the three-tier flow, and the self-contained single-entry design by Bohan Zeng.

## License

[MIT](LICENSE) © 2026 Bohan Zeng
