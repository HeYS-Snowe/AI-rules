# AGENTS.md — AI 编码规则体系项目

## 项目概述

本仓库 (`D:\Code\.Rules`) 是一个 **AI 编码规范系统**，v3.2.2，采用 **common/ + stacks/** 双库架构，提供可组合、可定制、全流程覆盖的编码规则。

**核心理念**: 规则即模板，模板即规则
**核心公式**: `基础规则 + 定制需求 = 项目定制规则`

关联仓库：`D:\Code\.prompt` — 提示词存储与解决方案沉淀。

## AI 协作原则

本仓库的编写和修改遵循以下原则（详见 `common/core/principles.md` 第六章）：

- **禁令优于指令** — 优先写"禁止做什么"，每条附原因
- **不画蛇添足** — NEVER 添加未被要求的东西
- **如实汇报** — 准确 > 简洁 > 详细
- **先看再改** — 修改前必须先读取理解
- **不知道就说不** — NEVER 编造信息

## 架构

```
.Rules/
├── main.md                  # 主入口（规则索引 + 使用说明）
├── ERROR/                   # 错误存储与纠正系统（全量问题记录）
│   ├── README.md            # 系统文档
│   ├── INDEX.md             # 错误索引
│   └── entries/             # 错误条目
├── common/                  # 通用库（所有技术栈可用）
│   ├── core/                # 核心规则：principles / security / code-style / workflow
│   ├── structures/          # 项目结构模板
│   ├── templates/           # 定制需求模板、Codex 模板、项目规则模板
│   └── errors/              # 旧错误知识库（冻结，新错误存入 ERROR/）
└── stacks/                  # 专用库（按技术栈）
    ├── flutter/             # 预设 + 国内镜像 + 错误条目
    ├── react/               # 预设 + 错误条目
    ├── fastapi/             # 预设 + 错误条目
    ├── fullstack/           # 预设 + 示例
    ├── python-ml/           # 预设 + 错误条目
    └── minecraft-mod/       # 预设 + 错误条目
```

## 规则层级（优先级从高到低）

```
定制规则 > 预设配置 > 项目结构规则 > 核心规则 > 默认行为
```

- **核心规则层**（必选）：SOLID、DRY、KISS、YAGNI；OWASP Top 10 安全规范
- **项目结构层**（必选）：根据项目类型选择结构模板
- **错误资源库层**（推荐）：全量问题记录与 AI 自我纠正（`ERROR/`，记 AI 跨项目经验；项目本身 bug 归项目根目录 `.issues/`，详见 workflow.md §9.4）
- **定制需求层**（可选）：项目特定配置
- **提示词知识库层**（推荐）：跨会话沉淀（D:\Code\.prompt）

## 编辑规范

### 核心规则层 (`common/core/`)

- 变更需谨慎评估，影响所有下游项目
- 保持规则的可组合性，避免技术栈特定内容混入通用规则
- 修改后更新 `main.md` 中的版本号和变更记录

### 预设配置 (`stacks/{tech}/presets/*.yaml`)

- YAML 格式，结构参考现有预设文件
- 预设名与目录名一致（如 `flutter/` → `flutter.yaml`）
- 新增技术栈需在 `stacks/` 下创建对应目录，含 `presets/` 和 `errors/entries/`

### 错误资源库 (`ERROR/`)

- **全量记录**：任何问题都存储，不限代码错误（7 种类型：code/config/dependency/environment/ai-behavior/workflow/knowledge）
- **检索优先**：出现问题先搜索 `ERROR/entries/`，参考但不盲从
- **无感存储**：AI 静默记录，无需用户确认
- 重复问题更新发生次数，不重复创建条目
- 记录格式参照 `ERROR/README.md`
- 旧系统 `common/errors/` + `stacks/{tech}/errors/` 保留但冻结

### 模板文件 (`common/templates/`)

- `custom-requirements.yaml` — 有示例值的模板
- `custom-requirements-template.yaml` — 空白模板
- `Codex-template.md` — Codex 项目规则模板
- `project-rules-template.md` — 项目定制规则输出模板

## 关联仓库交互

**提示词库 (`D:\Code\.prompt`)**：每次会话遵循"会话前搜索、会话后沉淀"工作流，详见 `D:\Code\.prompt\AGENTS.md`。

## 提交规范

```
<type>(<scope>): <subject>

类型: feat | fix | refactor | docs | chore
scope: common | flutter | react | fastapi | fullstack | python-ml | minecraft-mod | templates | error
```
