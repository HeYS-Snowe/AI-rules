# AI 编码规则体系 (AI Coding Rules System)

> **版本**: v3.4.0
> **更新日期**: 2026-08-04
> **核心理念**: 规则即模板，模板即规则
> **AI 入口**: [`compact-core.md`](./compact-core.md) — 精简核心（始终加载约束 + 按需加载清单），适合放入项目 CLAUDE.md
> **路径基准**: [`ROOT.md`](./ROOT.md) — 仓库根 + 跨仓库路径 SSOT

---

## 概述

本规则体系是一个**可组合、可定制、全流程覆盖**的 AI 编码规范系统。

采用 **common/ + stacks/** 双库架构：

- **common/** — 通用库，所有技术栈皆可用的规则
- **stacks/** — 专用库，按技术栈分类的专用规则和配置

### 关联体系

| 仓库   | 路径                | 职责                |
| ---- | ----------------- | ----------------- |
| 编码规则 | `D:\Code\.Rules`  | 编码规范、开发流程、项目结构    |
| 提示词库 | `D:\Code\.prompt` | 提示词存储、解决方案沉淀、经验复用 |

**提示词库工作流（每次会话必须遵循）：**

```
会话前 ──→ 搜索 D:\Code\.prompt 中是否有相关提示词/解决方案
         │
         ├── 有 → 读取并整合到回复中
         │
         └── 无 → 正常进行对话
                        │
                        ↓
会话后 ──→ 评估并存储高质量内容到 D:\Code\.prompt
           更新索引表（CLAUDE.md）
```

### 核心公式

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│    基础规则  +  定制需求  =  项目定制规则        │
│                                                                 │
│    基础规则: 本规则体系，可直接使用                              │
│    定制需求: 项目特定配置 (YAML/Markdown)                        │
│    定制规则: AI 生成的项目专属规则                               │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 体系架构

```
.Rules/
├── main.md                          # 主入口 (本文件)
├── OrganizationAndUser.md           # 组织与开发者信息（包名/署名等身份来源）
├── README.md                        # 说明文档
├── USAGE-GUIDE.md                   # 使用指南
│
├── ERROR/                           # 错误存储与纠正系统
│   ├── README.md                    # 系统文档
│   ├── INDEX.md                     # 错误索引
│   └── entries/                     # 错误条目
│
├── common/                          # 通用库 (所有技术栈可用)
│   ├── core/
│   │   ├── principles.md            # 核心原则
│   │   ├── security.md              # 安全规范
│   │   ├── code-style.md            # 代码风格
│   │   └── workflow.md              # 开发流程
│   ├── structures/
│   │   └── project-structure-guide.md
│   ├── templates/
│   │   ├── custom-requirements.yaml
│   │   ├── custom-requirements-template.yaml
│   │   ├── project-rules-template.md
│   │   └── CLAUDE-template.md
│
├── archive/                          # 归档（NEVER 作为现行规则加载）
│   ├── ai-coding-rulesystem.md       # 旧版完整文档 v1.0.0
│   └── errors/                       # 旧错误知识库（冻结）
│
├── stacks/                          # 专用库 (按技术栈分类)
    ├── flutter/
    │   ├── presets/
    │   │   ├── flutter.yaml
    │   │   └── loop.yaml
    │   ├── flutter-china-mirrors.md
    │   └── errors/entries/
    ├── react/
    │   ├── presets/
    │   │   └── react.yaml
    │   └── errors/entries/
    ├── fastapi/
    │   ├── presets/
    │   │   └── fastapi.yaml
    │   └── errors/entries/
    ├── fullstack/
    │   ├── presets/
    │   │   └── fullstack.yaml
    │   └── examples/
    │       ├── mindcare-ai-rules.md
    │       └── mindcare-ai-custom-rules.md
    ├── python-ml/
    │   ├── presets/
    │   │   └── ocean-lstm.yaml
    │   └── errors/entries/
    └── minecraft-mod/
        ├── presets/
        │   └── minecraft-mod.yaml
        └── errors/entries/

├── workflow/                       # 项目工作流程样本库（提炼自各项目实战）
│   ├── README.md                   # 用途 + 提炼原则 + 索引
│   └── {项目名}/                   # 每项目一目录
│       └── 工作流程.md

关联仓库:
D:\Code\.prompt/                       # 提示词与解决方案存储库
├── CLAUDE.md                          # 工作流规则与索引
├── 前端/                              # 前端类提示词
├── 设计/                              # 设计类提示词
└── 解决方案/                           # 问题解决方案
```

---

## 一、规则层级结构

### 1.1 核心规则层 (Core Rules)

> 必选，所有项目必须遵守的基础规则

| 文件                          | 内容                      | 强制性 |
| --------------------------- | ----------------------- | --- |
| `common/core/principles.md` | SOLID、DRY、KISS、YAGNI 原则 + AI 协作原则 | 强制  |
| `common/core/security.md`   | OWASP Top 10 安全规范       | 强制  |
| `common/core/code-style.md` | 沟通规范 + 命名、注释、格式 + 降AI率（条件触发） | 推荐  |
| `common/core/workflow.md`   | 开发全流程规范 + AI 行为约束        | 推荐  |

### 1.2 项目结构层 (Structure Rules)

> 必选，定义项目目录结构

- 引用 `common/structures/project-structure-guide.md`
- 根据项目类型选择对应结构模板

### 1.3 错误资源库层 (Error Repository)

> 推荐，AI 自动进化的核心机制

- **位置**：`ERROR/` — 独立的错误存储与纠正系统
- **全量记录**：任何问题都存储（代码错误、AI 行为纠正、工作流问题等）
- **检索优先**：出现问题先检索已有记录，参考但不盲从
- **持续更新**：重复问题更新次数，过时方案标记 deprecated
- 7 种错误类型：code / config / dependency / environment / ai-behavior / workflow / knowledge
- 详见 `ERROR/README.md`

> 旧系统 `archive/errors/` + `stacks/{tech}/errors/` 已归档冻结（NEVER 加载），新错误统一存入 `ERROR/`

### 1.4 定制需求层 (Customization Layer)

> 可选，项目特定的定制配置

- 使用 `common/templates/custom-requirements.yaml` 定义需求
- 或使用 `stacks/{tech}/presets/` 下的预设配置快速开始

### 1.5 提示词知识库层 (Prompt Knowledge Base)

> 强烈推荐，跨会话的提示词与解决方案沉淀

- 位置：`D:\Code\.prompt`
- **会话前查找**：搜索已有提示词/解决方案，避免重复劳动
- **会话后存储**：沉淀高质量内容，持续积累经验
- 详见 `D:\Code\.prompt\CLAUDE.md`

---

## 二、使用方式

### 方式一：直接使用（默认模式）

将本规则文件提供给 AI：

```
请按照 D:\Code\.Rules\main.md 规则体系进行开发。
```

AI 将：

1. 遵守核心规则层所有规则
2. 按照开发流程层规范执行
3. 参考项目结构层创建目录
4. 使用默认配置

### 方式二：预设模式（快速定制）

选择一个预设配置：

```
请按照 D:\Code\.Rules\main.md 规则体系，使用 stacks/flutter/presets/flutter.yaml 预设，
为我的 Flutter 项目生成定制规则。
```

### 方式三：完全定制模式

提供完整的定制需求：

```
请基于 D:\Code\.Rules 规则体系和以下定制需求，生成项目定制规则：

[粘贴 custom-requirements.yaml 内容]
```

---

## 三、规则优先级

```
定制规则 > 预设配置 > 项目结构规则 > 核心规则 > 默认行为
提示词知识库(.prompt) 作为经验参考层，在所有阶段辅助决策
```

当规则冲突时，优先级高的规则生效。

---

## 四、全流程覆盖

### 4.1 开发生命周期

```
┌─────────────────────────────────────────────────────────────────┐
│                        开发全流程                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1.需求分析 ──→ 2.架构设计 ──→ 3.编码实现 ──→ 4.测试验证       │
│       │              │              │              │            │
│       ↓              ↓              ↓              ↓            │
│  ┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐         │
│  │ PRD文档 │   │ 设计文档 │   │ 代码产物 │   │ 测试报告 │         │
│  └─────────┘   └─────────┘   └─────────┘   └─────────┘         │
│                                                                 │
│  5.构建打包 ──→ 6.部署发布 ──→ 7.运维监控 ──→ 8.迭代优化       │
│       │              │              │              │            │
│       ↓              ↓              ↓              ↓            │
│  ┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐         │
│  │ 构建产物 │   │ 部署记录 │   │ 监控日志 │   │ 更新日志 │         │
│  └─────────┘   └─────────┘   └─────────┘   └─────────┘         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 4.2 各阶段规则引用

| 阶段   | 规则文件                                                      | 核心内容        |
| ---- | --------------------------------------------------------- | ----------- |
| 需求分析 | `common/core/workflow.md#需求分析`                            | 需求文档模板、验收标准 |
| 架构设计 | `common/core/workflow.md#架构设计`                            | 架构模式、技术选型   |
| 编码实现 | `common/core/code-style.md` + `common/core/principles.md` | 代码规范、设计原则   |
| 测试验证 | `common/core/workflow.md#测试规范`                            | 测试策略、覆盖率要求  |
| 构建打包 | `common/core/workflow.md#构建规范`                            | 构建流程、产物命名、版本推荐   |
| 部署发布 | `common/core/workflow.md#部署规范`                            | 部署检查、回滚方案   |
| 运维监控 | `common/core/workflow.md#运维规范`                            | 日志规范、告警配置   |
| 迭代优化 | `common/core/workflow.md#迭代规范`                            | 更新日志、版本管理   |
| 信息持久化 | `common/core/workflow.md#信息持久化`                          | 联网搜索结果存储、MCP 信息归档 |

---

## 五、定制规则生成

### 5.1 生成指令格式

```
基于 [D:\Code\.Rules] 规则体系，为以下项目生成定制规则：

项目名称: [名称]
项目类型: [类型]
技术栈: [技术栈列表]
特殊需求: [需求列表]

请输出完整的项目定制规则文件。
```

### 5.2 定制规则输出格式

生成的定制规则应包含：

1. **项目信息** - 基本信息、技术栈
2. **目录结构** - 项目特定的目录结构
3. **代码规范** - 技术栈特定的代码规范
4. **API 规范** - 接口设计规范（如适用）
5. **构建规范** - 构建流程和产物命名
6. **环境配置** - 环境变量、配置文件
7. **工作流程** - 开发、测试、部署流程
8. **日志规范** - 更新日志、崩溃日志

---

## 六、快速参考

### 项目类型快速选择

| 项目类型       | 预设文件                                      | 推荐场景          |
| ---------- | ----------------------------------------- | ------------- |
| Flutter 应用 | `stacks/flutter/presets/flutter.yaml`     | 移动端跨平台应用      |
| React 应用   | `stacks/react/presets/react.yaml`         | Web 前端 SPA    |
| FastAPI 后端 | `stacks/fastapi/presets/fastapi.yaml`     | Python 后端 API |
| 全栈项目       | `stacks/fullstack/presets/fullstack.yaml` | 前后端一体化项目      |
| Minecraft Mod | `stacks/minecraft-mod/presets/minecraft-mod.yaml` | MC 模组开发 |

### 常用命令

```bash
# 生成定制规则
基于 D:\Code\.Rules 规则体系和 stacks/flutter/presets/flutter.yaml 生成定制规则

# 使用定制规则开发
请按照 [项目定制规则文件] 进行开发

# 查看规则优先级
规则优先级: 定制规则 > 预设 > 结构 > 核心 > 默认
```

---

## 七、规则文件索引

### AI 精简核心

- [精简核心 (Compact Core)](./compact-core.md) - **AI 始终加载的入口**。提取安全红线、代码底线、AI 行为约束为始终生效层；其余规则按触发条件懒加载。放入项目 CLAUDE.md 即可生效。

### 项目元信息

- [组织与开发者信息](./OrganizationAndUser.md) - 组织/开发者身份、包名前缀、版权署名的唯一事实来源
- [项目清单](./PROJECTLIST.md) - 全项目导航索引（路径、定位、活跃状态）
- [项目审计系统](./docs/audit/README.md) - 定期审计机制与待办池

### 通用库 (common/)

#### 核心规则

- [核心原则](./common/core/principles.md) - SOLID、DRY、KISS、YAGNI + AI 协作原则
- [安全规范](./common/core/security.md) - OWASP Top 10
- [代码风格](./common/core/code-style.md) - 沟通规范 + 命名、注释、格式 + 降AI率（条件触发）
- [开发流程](./common/core/workflow.md) - 全流程规范 + AI 行为约束 + 项目问题档案(.issues/) + 编译后版本推荐

#### 项目结构

- [项目结构指南](./common/structures/project-structure-guide.md) - 各类项目目录结构

#### 错误资源库

- [错误存储与纠正系统](./ERROR/README.md) - 系统文档、字段定义、存储策略
- [错误索引](./ERROR/INDEX.md) - 按类型/标签查找

#### 模板

- [定制需求模板](./common/templates/custom-requirements.yaml) - YAML 配置模板
- [定制需求模板(空)](./common/templates/custom-requirements-template.yaml) - 空白模板
- [项目规则模板](./common/templates/project-rules-template.md) - 输出模板
- [CLAUDE 模板](./common/templates/CLAUDE-template.md) - Claude Code 规则模板

#### 归档（NEVER 作为现行规则加载）

- [AI 编码规则体系 v1.0.0](./archive/ai-coding-rulesystem.md) - 旧版完整文档
- [旧错误知识库](./archive/errors/README.md) - 冻结

### 专用库 (stacks/)

#### Flutter

- [Flutter 预设](./stacks/flutter/presets/flutter.yaml)
- [Loop 预设](./stacks/flutter/presets/loop.yaml)
- [Flutter 国内镜像配置](./stacks/flutter/flutter-china-mirrors.md)

#### React

- [React 预设](./stacks/react/presets/react.yaml)

#### FastAPI

- [FastAPI 预设](./stacks/fastapi/presets/fastapi.yaml)

#### 全栈

- [全栈预设](./stacks/fullstack/presets/fullstack.yaml)
- [MindCare AI 规则示例](./stacks/fullstack/examples/mindcare-ai-rules.md)
- [MindCare AI 定制规则示例](./stacks/fullstack/examples/mindcare-ai-custom-rules.md)

#### Python-ML

- [Ocean LSTM 预设](./stacks/python-ml/presets/ocean-lstm.yaml)

#### Minecraft-Mod

- [Minecraft Mod 预设](./stacks/minecraft-mod/presets/minecraft-mod.yaml)

### 项目工作流程样本 (workflow/)

- [工作流程样本库](./workflow/README.md) - 各项目实战工作手册的提炼存档，与通用模板 `common/core/workflow.md` 互补
- [Jeenith 工作流程](./workflow/jeenith/工作流程.md) - Flutter 移动 App 实战样本（结构/构建/归档/发布/版本/文档/Git 全流程 + 可复用经验提炼）

### 关联仓库

- [提示词与解决方案库](D:\Code\.prompt\CLAUDE.md) - 提示词存储、解决方案沉淀、工作流规则

---

## 变更记录

| 版本     | 日期         | 变更内容                                                    |
| ------ | ---------- | ------------------------------------------------------- |
| v3.4.0 | 2026-08-04 | 新增 AI 行为约束两条（§一 11 两手准备兜底必达 / 12 PowerShell 一律用 pwsh）；principles 新增 §6.10 两手准备规则；workflow §3.4 工具使用新增 pwsh 源定义；OrganizationAndUser 新增服务器与域名章节（腾讯云 62.234.115.217 + heys-snowe.tj.cn）、口号新增「未雨绸缪，兜底必达」 |
| v3.3.0 | 2026-08-03 | 新增可复用触发机制（`common/core/trigger-mechanism.md`）；workflow 新增改动文档生成（§12）；项目问题档案改由触发机制驱动（§9.4）；旧文档归档至 `archive/`；入口文档（README/CLAUDE/AGENTS）精简去重 |
| v3.2.2 | 2026-07-10 | 新增项目级问题档案(.issues/)、编译后版本号推荐、组织身份SSOT(OrganizationAndUser.md)、项目清单(PROJECTLIST.md)、项目审计系统(docs/audit，workflow §9.5) |
| v3.2.1 | 2026-05-11 | 新增"降AI率"规则（code-style.md 第八节），仅用户明确触发时生效 |
| v3.2.0 | 2026-04-25 | 新增错误存储与纠正系统 (ERROR/)，覆盖全量问题，支持检索优先和 AI 行为纠正 |
| v3.1.1 | 2026-04-21 | 新增信息持久化规则：联网搜索/MCP 工具获取的信息自动存储到 information/ |
| v3.1.0 | 2026-04-02 | 引入 AI 协作原则（7 条核心行为规范），改进模板引用方式 |
| v3.0.0 | 2026-03-28 | **BREAKING CHANGE** 重组为 common/ + stacks/ 双库架构，所有文件路径变更 |
| v2.2.0 | 2026-03-28 | 新增提示词知识库层，关联 D:\Code\.prompt 仓库                         |
| v2.1.1 | 2026-03-17 | 错误知识库改为无感存储，降低记录门槛                                      |
| v2.1.0 | 2026-03-17 | 新增错误知识库层 (errors/)，支持主动记录错误                             |
| v2.0.0 | 2026-03-12 | 重构为分层架构，支持全流程覆盖                                         |
| v1.0.0 | 2026-03-12 | 初始版本                                                    |

---

*本规则体系遵循"规则即模板，模板即规则"的设计理念*
*可直接使用，也可通过定制需求生成项目特定规则*
