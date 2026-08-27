# zengbohan-skill — 源码仓规范

单一自包含 skill：五阶段开发流水线（Grill-With-Docs → to-spec → to-tickets → implement/tdd → code-review）。源头是 GitHub 仓 `zengbohan1/zengbohan-skill`（marketplace 布局：技能本体在 `zengbohan-skill/` 子目录）；安装副本在 `~/.zcode/skills/zengbohan-skill`。

## 改动流程

改本克隆 → commit → push → 把安装副本同步成最新（直接覆盖 `~/.zcode/skills/zengbohan-skill`）。

## 结构约束

- 十个上游 stage 已 vendor 到 `stages/<name>/<name>.md`，入口文件特意从 `SKILL.md` 改名，避免被注册为独立技能；内部引用一律用相对路径（如 `stages/tdd/tdd.md`、`../tdd/tdd.md`）。
- `README.md` 记录了各 harness（ZCode / Claude Code / Codex via AGENTS.md）的安装方式；用户偏好单文件夹安装，勿建议重新拆装旧的 companion skills（ask-matt 等，已不存在独立版）。

## 上游同步（mattpocock/skills）

- 上游已把技能重排进类别目录：映射关系 grilling → productivity/grilling，其余九个 → engineering/。
- 跨技能引用的差异是 BY DESIGN：上游写 "call the Skill tool"，vendored 版写 "read ../<skill>/<skill>.md"——**不要把这个差异回灌上游**。
