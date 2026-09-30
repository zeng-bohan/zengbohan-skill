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
| **standard** (default) | fits one context window / one sitting | parallel repo exploration → one-round interview → one-pager plan with a **single approval** → in-session TDD build → one review pass |
| **full** | spans sessions or days | multi-round grilling → two-gate plan (content, then slices) → per-slice build → two-axis review |

Gates, deliberately: standard takes **one** approval (the plan covers seams, slice granularity, and blocking edges in one exchange); full takes **two** (the plan's content, then its slice breakdown). Environment facts never come back to you as questions — only decisions do, each with a recommended answer.

The stages underneath:

| Stage | Definition | What happens |
| --- | --- | --- |
| 1 | `stages/interview.md` | Parallel codebase exploration, then a design-tree interview — nothing written down |
| 2 | `stages/plan.md` | Synthesize the decisions into the plan file (stories, seams, slices with `Verify:` lines) — one approval, or two gates in full mode |
| 3 | `stages/implement.md` | Build each slice test-first at the pre-agreed seams, one commit each, ticking the plan as you go |
| 4 | `stages/code-review.md` | Two-axis review (coding standards + plan fidelity), then an acceptance checklist handed to you for sign-off |

The entry file (`SKILL.md`) picks the tier and routes; stage rules live only in `stages/`. Stage files are ordinary Markdown documents, not registered skills.

## What a run looks like

```text
> /zengbohan-skill Add citation tracing to the RAG answer pipeline

zengbohan-skill · tier: standard (one approval, then build)

▍ Interview   explored 3 areas in parallel; 2 decisions to make:
              · where citations attach (answer tokens vs. report sections)?
              · what counts as a trace? → recommendation offered
▍ Plan        .scratch/2026-09-30-citation-tracing.md — 3 slices, each
              with a Verify: line · approve to continue (y/n)
▍ Implement   slice 1/3: failing test at the generator seam → code →
              commit … slices 2-3 likewise, plan ticked as we go
▍ Review      two-axis pass (standards + plan fidelity) → acceptance
              checklist handed back for your sign-off
```

The skill announces its tier in the first line of its reply. You stay in control at the decision points: interview rounds pause for your answers, and the plan file is written only after you approve it. Interview and plan share one context window so the design thinking stays connected; implementation continues in the same session by default and only splits when the context runs low.

## Tech stack

No runtime, no build step, no dependencies — the skill is plain Markdown plus supporting files, packaged as a single folder:

| Piece | What it is |
| --- | --- |
| `SKILL.md` | Entry point — tier dispatch, routing, exit conditions (YAML frontmatter: `name`, `description`) |
| `stages/*.md` | The four stage definitions (rules live here, nowhere else) |
| `references/*.md` | Operational recipes the stages point at |
| Convention | [Agent Skills](https://agentskills.io) — any harness that scans a skills directory discovers it automatically |

## Self-contained by design

Earlier releases required installing ten companion skills alongside this one. They are now **embedded and consolidated under `stages/`**, so installing this single folder is enough on any harness. Nothing else to install, nothing to fall out of sync.

## Install

The repository root **is** the skill folder — clone it straight into a skills directory.

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

When the user invokes the zengbohan-skill pipeline (e.g. "/zengbohan-skill <task>"),
first read ~/.codex/zengbohan-skill/SKILL.md and follow its three-tier pipeline,
loading the stage files it references under stages/.
```

### Any other harness

The skill is plain Markdown plus supporting files. Clone it anywhere your agent can read, then point your harness's always-on instruction file at the entry definition:

```markdown
When the user invokes the zengbohan-skill pipeline (e.g. "/zengbohan-skill <task>"),
read <path-to>/zengbohan-skill/SKILL.md and follow it.
```

This works for Cursor rules, Windsurf, Cline, opencode, or anything that can inject instructions.

## Use

```text
/zengbohan-skill Add citation tracing to the RAG answer pipeline
```

## Notes and gotchas

- **Explicit invocation only.** The skill never fires on natural-language triggers like "develop" or 按流程走 — if you want the pipeline, call it by name.
- **Restart after install.** If the skill does not appear immediately, restart or refresh your harness.
- **Windows paths.** Replace `~/` with `%USERPROFILE%\` in every path above.
- **One artifact, not a paper trail.** The pipeline produces exactly one file — the plan under `.scratch/` — and the conversation carries everything else. No glossaries, ADRs, specs, or ticket lists.
- **Exactly one registered skill.** Only the top-level `SKILL.md` is named `SKILL.md`; stage files are ordinary Markdown, so harnesses register exactly one skill.
- **Upstream skills are not embedded.** `triage`, `prototype`, `pr`, `diagnosing-bugs`, `wayfinder`, `wizard` and the rest of the growing mattpocock suite are deliberately out — install them from [mattpocock/skills](https://github.com/mattpocock/skills) if you want them.

## Repository layout

```text
zengbohan-skill/          ← the repo IS the skill; clone this folder into a skills directory
├── SKILL.md              ← entry point — tier dispatch + routing + exit conditions
├── AGENTS.md
├── LICENSE
├── docs/
│   └── banner.svg
├── stages/               ← the four stage definitions (rules live here, nowhere else)
│   ├── interview.md
│   ├── plan.md
│   ├── implement.md
│   └── code-review.md
└── references/           ← operational recipes the stages point at
    ├── PHASE-BOUNDARIES.md
    ├── tests.md
    └── mocking.md
```

## Design principles

- **Design before code.** Make important decisions explicit — and only the important ones.
- **Ceremony scales with the work.** Gates and process earn their keep per feature size; a one-day feature gets one approval, not a paper trail of its own.
- **One artifact, not a paper trail.** The pipeline produces exactly one file — the plan under `.scratch/` — and the conversation carries everything else.
- **Ship vertical slices.** Every slice should be independently verifiable.
- **Test the seam.** Prefer tests that exercise the real boundary and failure mode.
- **Add complexity when evidence requires it.** Use architecture and diagnosis tools when the codebase earns them.

## Credits

The stage definitions are consolidated from Matt Pocock's engineering skill suite ([mattpocock/skills](https://github.com/mattpocock/skills)) — `grilling`, `domain-modeling`, `grill-with-docs`, `to-spec`, `to-tickets`, `implement`, `tdd`, `code-review`, `setup-matt-pocock-skills`, and `ask-matt`'s phase-boundary rules — last synced 2026-09-24. Upstream keeps growing (`triage`, `prototype`, `pr`, `diagnosing-bugs`, `wayfinder`, `wizard`, …); those are deliberately **not** embedded here — install them from mattpocock/skills if you want them. Packaging, consolidation, the three-tier flow, and the self-contained single-entry design by Bohan Zeng.

## Contributing

This is a solo-maintained project. Bugs, questions, and feature ideas: [open an issue](https://github.com/zeng-bohan/zengbohan-skill/issues).

## License

[MIT](LICENSE) © 2026 Bohan Zeng
