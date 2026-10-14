# zengbohan-skill

`zengbohan-skill` 是一个显式调用的 Agent 开发流程技能，用于功能开发、缺陷修复和重构。它根据风险和复杂度选择流程档位，并将计划、实施和复核组织成可恢复的工作过程。

技能由 Markdown 与配置文件组成，没有运行时依赖、构建步骤或服务端 API。

## 适用场景

| 档位 | 适用任务 | 流程 |
| --- | --- | --- |
| `micro` | 局部、低风险且容易验证的修改 | 直接处理，并做与风险相称的验证 |
| `standard`（默认） | 范围明确的功能、修复和重构 | 需求澄清、计划、分片实施、代码复核 |
| `full` | 跨会话、多交付物或存在较大架构不确定性的工作 | 在 standard 流程上分阶段确认依赖和实施切片，支持恢复 |

涉及权限、资金、破坏性操作、数据库迁移、共享 API 兼容性或并发完整性的改动至少使用 standard 档位。

## 工作流程

~~~text
显式调用
  → 选择档位并澄清需求
  → 形成一个可恢复的计划
  → 按切片实施并运行约定检查
  → 从正确性与需求行为两个方面复核
~~~

计划保存在当前项目的 `.scratch/<feature-slug>/plan.md`。是否提交由用户授权和计划中的执行策略决定；规划本身不代表允许提交、推送、合并、部署或写入线上数据库。

## 安装与调用

将整个仓库安装到 Codex 用户技能目录：

~~~bash
mkdir -p "$HOME/.agents/skills"
git clone https://github.com/zeng-bohan/zengbohan-skill.git "$HOME/.agents/skills/zengbohan-skill"
~~~

Windows PowerShell：

~~~powershell
New-Item -ItemType Directory -Force "$HOME\.agents\skills" | Out-Null
git clone https://github.com/zeng-bohan/zengbohan-skill.git "$HOME\.agents\skills\zengbohan-skill"
~~~

安装后，可通过技能选择器或在任务中显式调用：

~~~text
$zengbohan-skill 给 RAG 问答链路增加引用溯源
~~~

也支持 `/zengbohan-skill <任务>` 调用形式。该技能关闭自动调用；普通的“帮我开发”请求不会自动启动此流程。安装后若技能列表中暂未出现，请刷新或重启宿主。

其他遵循 Agent Skills 目录约定的宿主，也可以将该文件夹放入宿主可发现的技能目录。技能入口与阶段规则见 [SKILL.md](SKILL.md)。

## 项目结构

~~~text
SKILL.md                 # 技能入口、档位选择与阶段路由
agents/openai.yaml       # Codex 技能选择器信息与调用策略
stages/                  # 需求澄清、计划、实施和代码复核规则
references/              # 上下文、测试、Mock 与阶段边界参考
docs/                    # 项目说明资源
AGENTS.md                # 本仓库工作约定
LICENSE
~~~

本技能是单文件夹分发；配套阶段和参考文件随技能一同安装，不需要额外安装伴随技能。仓库使用的项目约定可参见 [AGENTS.md](AGENTS.md)。

## 许可证

[MIT](LICENSE)