# zengbohan-skill — 源码仓规范

单一自包含 skill：三档开发流水线（micro / standard / full），技能本体即仓库根目录（根目录 `SKILL.md` 是唯一入口）。GitHub 仓 `zeng-bohan/zengbohan-skill`；安装副本在 `~/.zcode/skills/zengbohan-skill` 与 `~/.claude/skills/zengbohan-skill`。

## 改动流程

改本克隆 → commit → push → 把安装副本同步成最新（直接覆盖）。

## 结构约束

- 入口只有根目录 `SKILL.md`（thin router：分档 + 路由 + 出口条件）；规则只活在 `stages/*.md` 里，避免双份维护 —— 不要把 stage 规则抄回 `SKILL.md`。
- stage 文件名不叫 `SKILL.md`，避免被注册为独立技能；内部引用一律相对路径（如 `../references/tests.md`）。
- `references/` 放格式模板与操作参考（PHASE-BOUNDARIES、CONTEXT/ADR-FORMAT、tests/mocking、issue-tracker 模板等）。
- `README.md` / `README.zh-CN.md` 记录各 harness 的一步安装方式；用户偏好单文件夹安装，勿建议拆回 companion skills（ask-matt 等，已不存在独立版）。

## 上游同步（mattpocock/skills）

- 上游技能按类别目录组织：grilling 在 `productivity/grilling`，其余在 `engineering/<name>`。本仓的映射：interview ← grilling + domain-modeling + grill-with-docs；plan ← to-spec + to-tickets；implement ← implement + tdd；setup ← setup-matt-pocock-skills；references/PHASE-BOUNDARIES ← ask-matt。
- 跨技能引用的差异是 BY DESIGN：上游写 "call the Skill tool"，本仓写 "read <path>" —— **不要把这个差异回灌上游**。
- 上游新技能（triage / prototype / pr / diagnosing-bugs / wayfinder / wizard 等）按设计不内嵌；需要时直接用上游。
