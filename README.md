# AI 编码规则体系

> **版本**: v3.0.0
> **更新日期**: 2026-03-28
> **核心理念**: 规则即模板，模板即规则

---

## 简介

这是一个**可组合、可定制、全流程覆盖**的 AI 编码规范系统。

采用 **common/ + stacks/** 双库架构：
- **common/** — 通用库，所有技术栈皆可用的规则
- **stacks/** — 专用库，按技术栈分类的专用规则和配置

### 关联体系

| 仓库 | 路径 | 职责 |
|------|------|------|
| 编码规则 | `D:\Code\.Rules` | 编码规范、开发流程、项目结构 |
| 提示词库 | `D:\Code\.prompt` | 提示词存储、解决方案沉淀、经验复用 |

### 核心公式

```
基础规则 + 定制需求 = 项目定制规则
```

- **基础规则**: 本规则体系，可直接使用
- **定制需求**: 项目特定配置 (YAML/Markdown)
- **定制规则**: AI 生成的项目专属规则

---

## 快速开始

### 方式一：直接使用

将规则文件提供给 AI：

```
请按照 D:\Code\.Rules\main.md 规则体系进行开发。
```

### 方式二：使用预设

```
请按照 AI-Rules 规则体系，使用 stacks/flutter/presets/flutter.yaml 预设，
生成我的 Flutter 项目定制规则。
```

### 方式三：完全定制

1. 复制 `common/templates/custom-requirements.yaml`
2. 填写项目信息
3. 提供给 AI：

```
请基于 AI-Rules 规则体系和以下定制需求，生成项目定制规则：

[粘贴配置内容]
```

---

## 目录结构

```
.Rules/
├── main.md                          # 主入口文件
├── README.md                        # 本文件
├── USAGE-GUIDE.md                   # 使用指南
│
├── common/                          # 通用库 (所有技术栈可用)
│   ├── core/
│   │   ├── principles.md            # 核心原则 (SOLID, DRY, KISS, YAGNI)
│   │   ├── security.md              # 安全规范 (OWASP Top 10)
│   │   ├── code-style.md            # 代码风格规范
│   │   └── workflow.md              # 开发流程规范
│   ├── structures/
│   │   └── project-structure-guide.md
│   ├── templates/
│   │   ├── custom-requirements.yaml      # 定制需求模板
│   │   ├── custom-requirements-template.yaml
│   │   ├── project-rules-template.md     # 项目规则输出模板
│   │   └── CLAUDE-template.md            # Claude Code 规则模板
│   ├── errors/
│   │   ├── README.md              # 错误知识库说明
│   │   ├── ERRORS_INDEX.md        # 通用错误索引
│   │   └── entries/               # 通用错误记录条目
│   └── ai-coding-rulesystem.md    # 归档 (v1.0.0 旧版文档)
│
└── stacks/                          # 专用库 (按技术栈分类)
    ├── flutter/
    │   ├── presets/
    │   │   ├── flutter.yaml           # Flutter 项目预设
    │   │   └── loop.yaml              # Loop 预设
    │   ├── flutter-china-mirrors.md   # 国内镜像配置
    │   └── errors/entries/            # Flutter 错误条目
    ├── react/
    │   ├── presets/
    │   │   └── react.yaml             # React 项目预设
    │   └── errors/entries/            # React/TypeScript 错误条目
    ├── fastapi/
    │   ├── presets/
    │   │   └── fastapi.yaml           # FastAPI 项目预设
    │   └── errors/entries/            # FastAPI 错误条目
    ├── fullstack/
    │   ├── presets/
    │   │   └── fullstack.yaml         # 全栈项目预设
    │   └── examples/
    │       ├── mindcare-ai-rules.md       # 完整示例
    │       └── mindcare-ai-custom-rules.md
    └── python-ml/
        ├── presets/
        │   └── ocean-lstm.yaml        # Ocean LSTM 预设
        └── errors/entries/            # Python-ML 错误条目

关联仓库:
D:\Code\.prompt/                       # 提示词与解决方案存储库
```

---

## 规则层级

```
定制规则 > 预设配置 > 项目结构规则 > 核心规则 > 默认行为
```

当规则冲突时，优先级高的规则生效。

---

## 规则层说明

| 层级 | 类型 | 说明 |
|------|------|------|
| 核心规则层 | 必选 | SOLID、DRY、KISS、YAGNI 原则；OWASP Top 10 安全规范 |
| 项目结构层 | 必选 | 根据项目类型选择对应结构模板 |
| 错误知识库层 | 可选 | 无感存储开发过程中的问题和经验 |
| 定制需求层 | 可选 | 项目特定的定制配置 |
| 提示词知识库层 | 推荐 | 跨会话提示词与解决方案沉淀 (D:\Code\.prompt) |

### 错误知识库特性

- **无感存储**：AI 静默记录，无需用户确认
- **低门槛**：不限于严重错误，小坑、微扰也记录
- 支持按类型、技术栈、标签索引
- 重复问题自动更新发生次数
- 通用错误存放在 `common/errors/`，技术栈专用错误存放在 `stacks/{tech}/errors/`

---

## 全流程覆盖

| 阶段 | 规范文件 | 核心内容 |
|------|----------|----------|
| 需求分析 | `common/core/workflow.md` | PRD 模板、验收标准 |
| 架构设计 | `common/core/workflow.md` | 架构模式、技术选型 |
| 编码实现 | `common/core/code-style.md` + `common/core/principles.md` | 代码规范、设计原则 |
| 测试验证 | `common/core/workflow.md` | 测试策略、覆盖率要求 |
| 构建打包 | `common/core/workflow.md` | 构建流程、产物命名 |
| 部署发布 | `common/core/workflow.md` | 部署检查、回滚方案 |
| 运维监控 | `common/core/workflow.md` | 日志规范、告警配置 |
| 迭代优化 | `common/core/workflow.md` | 变更日志、版本管理 |

---

## 预设选择指南

| 项目类型 | 预设文件 | 适用场景 |
|----------|----------|----------|
| Flutter 应用 | `stacks/flutter/presets/flutter.yaml` | 移动端跨平台应用 |
| React 应用 | `stacks/react/presets/react.yaml` | Web 前端 SPA |
| FastAPI 后端 | `stacks/fastapi/presets/fastapi.yaml` | Python 后端 API |
| 全栈项目 | `stacks/fullstack/presets/fullstack.yaml` | 前后端一体化项目 |

---

## 更新日志

| 版本 | 日期 | 变更 |
|------|------|------|
| v3.0.0 | 2026-03-28 | **BREAKING CHANGE** 重组为 common/ + stacks/ 双库架构，所有文件路径变更 |
| v2.2.0 | 2026-03-28 | 新增提示词知识库层，关联 D:\Code\.prompt 仓库 |
| v2.1.1 | 2026-03-17 | 错误知识库改为无感存储，降低记录门槛 |
| v2.1.0 | 2026-03-17 | 新增错误知识库层 (errors/)，支持主动记录错误 |
| v2.0.0 | 2026-03-12 | 重构为分层架构，支持全流程覆盖 |
| v1.0.0 | 2026-03-12 | 初始版本 |

---

## 贡献指南

1. 核心规则 (`common/core/`) 变更需要谨慎评估
2. 新增预设 (`stacks/{tech}/presets/`) 欢迎提交
3. 示例 (`stacks/fullstack/examples/`) 有助于理解使用方式
4. 新增技术栈请在 `stacks/` 下创建对应目录

---

*规则即模板，模板即规则*
*让 AI 编码更规范、更高效*
