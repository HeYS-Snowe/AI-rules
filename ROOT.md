# 路径基准 (Path Reference)

> 本仓库**相对路径的基准** + **跨仓库路径的唯一事实来源（SSOT）**。
> 创建: 2026-08-03 | 目的: 解决绝对路径硬编码导致的不可移植问题

---

## 仓库根

- 本仓库根: `D:\Code\.Rules`
- 本文档内所有相对路径（`common/core/...`、`stacks/...`、`ERROR/...`、`archive/...`）均以仓库根为基准

## 路径引用规范

1. **仓库内引用**：一律使用相对路径（如 `common/core/workflow.md`），NEVER 硬编码绝对路径
2. **跨仓库引用**：仅在本文件维护；其他文档引用跨仓库路径时写"见 `ROOT.md` §跨仓库路径"
3. **移动仓库**：只需修改本文件"仓库根"，其余文件相对路径不受影响

## 跨仓库路径

| 仓库 | 路径 | 用途 |
|------|------|------|
| 提示词库 | `D:\Code\.prompt` | 提示词存储、解决方案沉淀（会话前搜索 / 会话后存储） |
| AI-TOOLS | `D:\Code\.AI-TOOLS` | skills（`.skills/`）与 agents（`.agent/`）统一存储；junction: `~/.agents/skills`、`~/.claude/agents` |
| 用户级 Deep Code 规则 | `~/.deepcode/AGENTS.md` | 全局规则主载体（内嵌本仓库 compact-core.md 内容） |

## 更新规则

- 新增/变更跨仓库路径时更新上表
- 移动本仓库时更新"仓库根"
- 各文档中的路径若与本表冲突，以本表为准
