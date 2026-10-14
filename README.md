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

`zengbohan-skill` is an explicitly invoked workflow for features, fixes, and refactors. Its tier scales with risk and complexity, so the process stays proportional to the work.

| Tier | When | What runs |
| --- | --- | --- |
| **micro** | local, low-risk changes such as a typo or copy tweak | fix directly and verify proportionately; line count alone does not qualify |
| **standard** (default) | bounded features, fixes, and refactors | inspect the code → settle material decisions → one approved plan → verified slices → review |
| **full** | work spanning sessions, dependent deliverables, or major uncertainty | approve plan content, then slices and dependencies → implement and review in resumable stages |

Standard normally has **one** plan approval; full has **two** (plan content, then its slice breakdown). The agent looks up available environment facts and asks only about material decisions, with a recommendation. High-risk work such as authorization, money, migrations, or data integrity is at least standard regardless of size.

The stages underneath:

| Stage | Definition | What happens |
| --- | --- | --- |
| 1 | `stages/interview.md` | Parallel codebase exploration, then a design-tree interview — nothing written down |
| 2 | `stages/plan.md` | Synthesize the decisions into the plan file (stories, seams, slices with `Verify:` lines) — one approval, or two gates in full mode |
| 3 | `stages/implement.md` | Implement and verify slices; record progress and commit only under the agreed authorization |
| 4 | `stages/code-review.md` | Review standards and correctness, then plan and behaviour; finish with evidence-backed acceptance |

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
              verify each slice; commit only if the plan authorizes it
▍ Review      two-axis pass (standards + plan fidelity) → acceptance
              checklist handed back for your sign-off
```

The skill announces its tier and reason in the first line. It writes a plan draft before asking for approval; material decisions pause for your input, while routine implementation choices proceed. The plan tracks implementation, verification, commit state, evidence, and the next step so work can resume after an interruption. Review includes task-owned working-tree changes as well as commits.

## Tech stack

No runtime, no build step, no dependencies — the skill is plain Markdown plus supporting files, packaged as a single folder:

| Piece | What it is |
| --- | --- |
| `SKILL.md` | Entry point — tier dispatch, routing, exit conditions (YAML frontmatter: `name`, `description`) |
| `agents/openai.yaml` | Codex skill picker metadata and explicit-only invocation policy |
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

### Codex

Install the folder in Codex's user skills directory, then invoke it explicitly with the skill picker or `$zengbohan-skill`:

```bash
git clone https://github.com/zeng-bohan/zengbohan-skill.git ~/.agents/skills/zengbohan-skill
```

The included `agents/openai.yaml` disables implicit invocation. For a harness without skill discovery, point its instructions at `SKILL.md` and load the referenced stage files only when the user explicitly invokes the pipeline.

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

Include relevant requirements and execution constraints in the request. For example: `Do not commit or push.` The default tier is standard; specify `full` for multi-session work. Generic development requests do not automatically activate the skill.

## Notes and gotchas

- **Explicit invocation only.** The skill never fires on natural-language triggers like "develop" or 按流程走 — if you want the pipeline, call it by name.
- **Restart after install.** If the skill does not appear immediately, restart or refresh your harness.
- **Windows paths.** Replace `~/` with `%USERPROFILE%\` in every path above.
- **One artifact, not a paper trail.** The pipeline produces exactly one file — the plan under `.scratch/` — and the conversation carries everything else. No glossaries, ADRs, specs, or ticket lists.
- **Authorization stays explicit.** Plan approval does not by itself authorize commits, pushes, deployment, or live database writes.
- **Acceptance follows evidence.** Local checks do not stand in for required deployment, integration, or device acceptance.
- **Exactly one registered skill.** Only the top-level `SKILL.md` is named `SKILL.md`; stage files are ordinary Markdown, so harnesses register exactly one skill.
- **Upstream skills are not embedded.** `triage`, `prototype`, `pr`, `diagnosing-bugs`, `wayfinder`, `wizard` and the rest of the growing mattpocock suite are deliberately out — install them from [mattpocock/skills](https://github.com/mattpocock/skills) if you want them.

## Repository layout

```text
zengbohan-skill/          ← the repo IS the skill; clone this folder into a skills directory
├── SKILL.md              ← entry point — tier dispatch + routing + exit conditions
├── agents/
│   └── openai.yaml       ← Codex skill picker metadata and invocation policy
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
