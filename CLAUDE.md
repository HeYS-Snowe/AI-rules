# CLAUDE.md — AI 编码规则体系项目

## 项目概述

`D:\Code\.Rules` 是 **AI 编码规范系统**（v3.4.0），采用 **common/ + stacks/** 双库架构，提供可组合、可定制、全流程覆盖的编码规则。

**核心理念**: 规则即模板，模板即规则
**核心公式**: `基础规则 + 定制需求 = 项目定制规则`

关联仓库：`D:\Code\.prompt` — 提示词存储与解决方案沉淀。

## 文件导航（单事实来源，NEVER 在本文件重复维护）

| 文件 | 用途 |
|------|------|
| `main.md` | 完整规则索引 + 架构图 + 变更记录（人类阅读） |
| `compact-core.md` | AI 精简核心（Tier 1 约束 + Tier 2 按需加载清单） |
| `common/core/trigger-mechanism.md` | 可复用触发机制（自动落盘行为注册表） |
| `ROOT.md` | 路径基准（仓库根 + 跨仓库路径 SSOT） |
| `USAGE-GUIDE.md` | 各工具接入方式 |
| `README.md` | 项目门面说明 |

> 架构、加载模型、规则层级详见 `main.md` 与 `compact-core.md`，本文件只保留"如何维护本项目"。

## AI 协作原则

- **禁令优于指令** — 优先写"禁止做什么"，每条附原因
- **不画蛇添足** — NEVER 添加未被要求的东西
- **如实汇报** — 准确 > 简洁 > 详细
- **先看再改** — 修改前必须先读取理解
- **不知道就说不** — NEVER 编造信息

## 本项目维护规范

### 核心规则层 (`common/core/`)

- 变更需谨慎评估，影响所有下游项目
- 保持规则的可组合性，避免技术栈特定内容混入通用规则
- 修改后更新 `main.md` 版本号和变更记录，并在 `compact-core.md` 尾部同步更新日期

### 精简核心 (`compact-core.md`)

- Tier 1（§一~三）是从核心规则提取的始终生效约束——核心规则变更后需同步更新对应条目
- Tier 2（§四清单）是按需加载索引——新增规则文件或章节后需补充触发条件
- 保持精简：Tier 1 目标 ~100 行，不可膨胀为完整规则的副本

### 触发机制 (`common/core/trigger-mechanism.md`)

- 所有自动落盘行为（项目 bug 记录、AI 经验、改动文档、.prompt 沉淀等）走统一触发机制
- 新增行为 = 注册表加一行，NEVER 另写一套流程

### 错误资源库 (`ERROR/`)

- **全量记录**：任何问题都存储，不限代码错误（7 种类型）
- **检索优先**：出现问题先搜索 `ERROR/entries/`，参考但不盲从
- **无感存储**：AI 静默记录，无需用户确认
- 重复问题更新发生次数，不重复创建条目
- 记录格式参照 `ERROR/README.md`

### 预设配置 (`stacks/{tech}/presets/*.yaml`)

- YAML 格式，结构参考现有预设文件
- 预设名与目录名一致（如 `flutter/` → `flutter.yaml`）
- 新增技术栈需在 `stacks/` 下创建对应目录

### 模板文件 (`common/templates/`)

- `custom-requirements.yaml` — 有示例值的模板
- `custom-requirements-template.yaml` — 空白模板
- `CLAUDE-template.md` — Claude Code 项目规则模板
- `project-rules-template.md` — 项目定制规则输出模板

### 归档 (`archive/`)

- 旧版文档（`ai-coding-rulesystem.md`、`errors/`）已冻结归档，**NEVER 作为现行规则加载**

## 关联仓库交互

**提示词库 (`D:\Code\.prompt`)**：每次会话遵循"会话前搜索、会话后沉淀"工作流，详见 `D:\Code\.prompt\CLAUDE.md`。

## 提交规范

```
<type>(<scope>): <subject>

类型: feat | fix | refactor | docs | chore
scope: common | flutter | react | fastapi | fullstack | python-ml | minecraft-mod | templates | error
```
