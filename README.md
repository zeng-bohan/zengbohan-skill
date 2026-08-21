<p align="center">
  <img src="docs/banner.svg" alt="zengbohan-skill" width="100%" />
</p>

<h1 align="center">zengbohan-skill</h1>

<p align="center">
  A focused workflow for disciplined AI-assisted software development in ZCode.
</p>

<p align="center">
  <a href="zengbohan-skill/SKILL.md"><img src="https://img.shields.io/badge/ZCode-Skill-2F80ED?style=flat-square" alt="ZCode Skill" /></a>
  <img src="https://img.shields.io/badge/Workflow-5%20stages-1769AA?style=flat-square" alt="Five stages" />
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-4EB1BA?style=flat-square" alt="MIT License" /></a>
</p>

## What it does

`zengbohan-skill` is a single entry point for development work. It keeps AI-assisted changes grounded in explicit decisions, small deliverables, tests, and review.

The default path is:

1. **Grill-With-Docs** - clarify the problem and settle the design.
2. **to-spec** - turn the decisions into an implementation-ready specification.
3. **to-tickets** - split the specification into independently verifiable tickets.
4. **implement** - build the tickets with tests.
5. **code-review** - review the result against project standards and the specification.

## Install

Install the skill into your user-level ZCode skills directory:

```bash
git clone https://github.com/zengbohan1/zengbohan-skill.git
cp -R zengbohan-skill/zengbohan-skill ~/.zcode/skills/
```

The skill definition is loaded from:

```text
~/.zcode/skills/zengbohan-skill/SKILL.md
```

Restart or refresh ZCode after installation if the skill does not appear immediately.

## Use

Start a normal feature or refactor with:

```text
/zengbohan-skill <describe the development task>
```

Examples:

```text
/zengbohan-skill Add citation tracing to the RAG answer pipeline
/zengbohan-skill Add timeout handling for tool calls in agentflow
```

The skill pauses at decision points that require your input. It keeps design, specification, ticket approval, testing, and review in the same development loop.

## Optional quality checks

Use these alongside the main workflow when the problem calls for them:

| Situation | Use |
| --- | --- |
| Architecture drift, unclear module boundaries, hard-to-test code | `/improve-codebase-architecture` |
| Hard bug, intermittent failure, or performance regression | `/diagnosing-bugs` |

These are additions to the main workflow, not replacements for it.

## Required companion skills

The entry point coordinates these skills:

- `ask-matt`
- `grill-with-docs` and its `grilling` / `domain-modeling` support
- `to-spec`
- `to-tickets`
- `implement` and `tdd`
- `code-review`
- `setup-matt-pocock-skills` for first-time project setup

Install the companion skills separately in environments that do not already provide them.

## Repository layout

```text
zengbohan-skill/
├── README.md
├── LICENSE
├── docs/
│   └── banner.svg
└── zengbohan-skill/
    ├── SKILL.md
    └── cover.jpg
```

## Design principles

- **Design before code.** Make important decisions explicit.
- **Ship vertical slices.** Every ticket should be independently verifiable.
- **Test the seam.** Prefer tests that exercise the real boundary and failure mode.
- **Add complexity when evidence requires it.** Use architecture and diagnosis tools when the codebase earns them.

## License

[MIT](LICENSE) © 2026 Bohan Zeng
