---
name: zengbohan-skill
description: Workflow configuration guide for the Matt Pocock skill ecosystem. Recommends a two-tier setup: a universal minimal loop (Grill-With-Docs -> /to-spec -> /to-tickets -> /implement -> /code-review) for 90% of developers, plus an advanced tier that adds the two ace skills /improve-codebase-architecture and /diagnose for complex, quality-focused projects. Use when the user asks how to configure their AI coding workflow, wants a recommended skill setup, or mentions 配置 / 流程 / 技能组合 / workflow / 技能库.
---

# 曾波涵 Skill（Zengbohan Skill）

一套基于 Matt Pocock 技能库的 workflow 配置方案。按项目复杂度分两档，按需取用。

原始配置图见 `cover.jpg`。

## 一、通用极简配置（90% 开发者适用）

大多数开发任务走这一档。核心闭环只有五步：

```
Grill-With-Docs → /to-spec → /to-tickets → /implement → /code-review
```

| 步骤 | 技能 | 作用 |
| --- | --- | --- |
| 1 | `Grill-With-Docs` | 需求访谈，把设计树问到收敛，留一份纸面记录 |
| 2 | `/to-spec` | 把对话与已定设计综合成 spec |
| 3 | `/to-tickets` | 把 spec 拆成带阻塞边的 tracer-bullet ticket |
| 4 | `/implement` | 按 ticket 逐个实现（内嵌 `/tdd`） |
| 5 | `/code-review` | 沿 Standards / Spec 两轴复核改动 |

**取用原则**：无脑从第一档开始。它覆盖 90% 的开发者和 90% 的项目。

## 二、进阶增强配置（适合复杂项目、注重代码质量）

在通用流程基础上，**保留两个王牌技能**，补齐短板。

### 王牌 1：`/improve-codebase-architecture`

> 系统性扫描代码库，挖掘架构隐患、模块耦合、可测试性问题，生成可视化架构报告。

- 系统性扫描代码库
- 挖掘架构隐患、模块耦合、可测试性问题
- 生成可视化架构报告（HTML + Mermaid，带 before/after 深化候选与推荐强度）

**何时启用**：代码库增长快、测试越来越难写、或某个 bug 指向了纠缠的架构。

### 王牌 2：`/diagnose`

> 标准化六步调试闭环（复现 → 最小化 → 假设 → 仪器化 → 修复 → 回归），杜绝调试跳步、漏测问题。

```
复现 → 最小化 → 假设 → 仪器化 → 修复 → 回归
```

| 阶段 | 含义 |
| --- | --- |
| 复现 | 建立紧的、能报红的反馈环（先于任何假设） |
| 最小化 | 把 repro 缩到仍报红的最小场景 |
| 假设 | 先生成 3–5 个可证伪的 ranked 假设，再动手测 |
| 仪器化 | 每次只动一个变量，对应一个预测 |
| 修复 | 先写回归测试，再改代码 |
| 回归 | 重跑原始环，确认无回归 |

**何时启用**：硬 bug、偶发失败、性能回归——任何跳一步就会漏测的场景。

## 三、怎么选

- **默认走第一档**（通用极简配置）。
- **遇到以下情况上第二档**（进阶增强配置）：
  - 项目规模大、代码质量要求高，或通用流程已经推不动；
  - 架构在漂移 → 用 `/improve-codebase-architecture`；
  - 堆着难啃的 bug → 用 `/diagnose`。

两个王牌技能与通用流程是**叠加关系**，不是替换关系——先跑完通用闭环，再按需插入。
