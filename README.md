# AI 编码规则体系

> **版本**: v2.0.0
> **核心理念**: 规则即模板，模板即规则

---

## 简介

这是一个**可组合、可定制、全流程覆盖**的 AI 编码规范系统。

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
请按照 AI-Rules/main.md 规则体系进行开发。
```

### 方式二：使用预设

```
请按照 AI-Rules 规则体系，使用 presets/flutter.yaml 预设，
生成我的 Flutter 项目定制规则。
```

### 方式三：完全定制

1. 复制 `templates/custom-requirements.yaml`
2. 填写项目信息
3. 提供给 AI：

```
请基于 AI-Rules 规则体系和以下定制需求，生成项目定制规则：

[粘贴配置内容]
```

---

## 目录结构

```
AI-Rules/
├── main.md                    # 主入口文件
├── README.md                  # 本文件
│
├── core/                      # 核心规则层 (必选)
│   ├── principles.md          # 核心原则 (SOLID, DRY, KISS, YAGNI)
│   ├── security.md            # 安全规范 (OWASP Top 10)
│   ├── code-style.md          # 代码风格规范
│   └── workflow.md            # 开发流程规范
│
├── structures/                # 项目结构层 (必选)
│   └── project-structure-guide.md
│
├── templates/                 # 模板层
│   ├── custom-requirements.yaml    # 定制需求模板
│   └── project-rules-template.md   # 项目规则输出模板
│
├── presets/                   # 预设配置层
│   ├── flutter.yaml           # Flutter 项目预设
│   ├── react.yaml             # React 项目预设
│   ├── fastapi.yaml           # FastAPI 项目预设
│   └── fullstack.yaml         # 全栈项目预设
│
└── examples/                  # 示例层
    └── mindcare-ai-rules.md   # 完整示例
```

---

## 规则层级

```
定制规则 > 预设配置 > 项目结构规则 > 核心规则 > 默认行为
```

当规则冲突时，优先级高的规则生效。

---

## 全流程覆盖

| 阶段 | 规范文件 | 核心内容 |
|------|----------|----------|
| 需求分析 | workflow.md | PRD 模板、验收标准 |
| 架构设计 | workflow.md | 架构模式、技术选型 |
| 编码实现 | code-style.md + principles.md | 代码规范、设计原则 |
| 测试验证 | workflow.md | 测试策略、覆盖率要求 |
| 构建打包 | workflow.md | 构建流程、产物命名 |
| 部署发布 | workflow.md | 部署检查、回滚方案 |
| 运维监控 | workflow.md | 日志规范、告警配置 |
| 迭代优化 | workflow.md | 变更日志、版本管理 |

---

## 预设选择指南

| 项目类型 | 预设文件 | 适用场景 |
|----------|----------|----------|
| Flutter 应用 | `presets/flutter.yaml` | 移动端跨平台应用 |
| React 应用 | `presets/react.yaml` | Web 前端 SPA |
| FastAPI 后端 | `presets/fastapi.yaml` | Python 后端 API |
| 全栈项目 | `presets/fullstack.yaml` | 前后端一体化项目 |

---

## 定制需求配置说明

### 基础信息

```yaml
project:
  name: "My Project"
  description: "项目描述"
  version: "0.0.1"

type:
  category: "mobile"           # web-frontend | web-backend | mobile | fullstack
```

### 技术栈

```yaml
tech:
  frontend:
    framework: "flutter"
    language: "dart"
    state: "riverpod"

  backend:
    framework: "fastapi"
    language: "python"
    database: "postgresql"
```

### 特殊需求

```yaml
requirements:
  features:
    - "需要离线支持"
    - "需要推送通知"

  non_functional:
    performance: "首屏加载 < 3s"
    security: ["数据加密", "HTTPS only"]
```

### 约束条件

```yaml
constraints:
  compatibility:
    - "必须兼容 Android 8.0+"

  limits:
    bundle_size: "不超过 20MB"
```

---

## 常用指令模板

### 生成定制规则

```
基于 [AI-Rules] 规则体系和以下配置，生成项目定制规则：

项目名称: {名称}
项目类型: {类型}
技术栈: {技术栈列表}
特殊需求: {需求列表}
```

### 使用定制规则开发

```
请按照 [项目定制规则文件] 进行开发：

{功能需求描述}
```

---

## 与现有规则整合

本规则体系可以与以下现有规则共存：

| 现有规则 | 位置 | 整合方式 |
|----------|------|----------|
| 项目规则 | `.trae/rules/` | 项目特定配置 |
| 个人规则 | `Trae-Tools/rules/` | 通用基础规则 |
| 项目结构 | `Code/.Rules/` | 已整合到 structures/ |

---

## 更新日志

| 版本 | 日期 | 变更 |
|------|------|------|
| v2.0.0 | 2026-03-12 | 重构为分层架构，支持全流程覆盖 |
| v1.0.0 | 2026-03-12 | 初始版本 |

---

## 贡献指南

1. 核心规则 (`core/`) 变更需要谨慎评估
2. 新增预设 (`presets/`) 欢迎提交
3. 示例 (`examples/`) 有助于理解使用方式

---

*规则即模板，模板即规则*
*让 AI 编码更规范、更高效*
