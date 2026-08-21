# zengbohan-skill

一套基于 Matt Pocock 技能库的 workflow 配置方案，按项目复杂度分两档。

> 原始配置图：`zengbohan-skill/cover.jpg`

## 结构

```
zengbohan-skill/
├── SKILL.md      # 技能定义（ZCode 可发现）
└── cover.jpg     # 原始配置图
```

## 两档配置

### 1. 通用极简配置（90% 开发者适用）

核心闭环：

```
Grill-With-Docs → /to-spec → /to-tickets → /implement → /code-review
```

### 2. 进阶增强配置（适合复杂项目、注重代码质量）

在通用流程基础上，保留两个王牌技能，补齐短板：

- `/improve-codebase-architecture`：系统性扫描代码库，挖掘架构隐患、模块耦合、可测试性问题，生成可视化架构报告
- `/diagnose`：标准化六步调试闭环（复现 → 最小化 → 假设 → 仪器化 → 修复 → 回归），杜绝调试跳步、漏测问题

## 安装

把 `zengbohan-skill/` 目录放到 ZCode 的技能发现路径下即可（如 `~/.zcode/skills/zengbohan-skill/`），技能名 `zengbohan-skill` 会自动被发现。

## License

MIT
