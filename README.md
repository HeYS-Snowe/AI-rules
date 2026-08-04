# AI 编码规则体系 (AI Coding Rules System)

> **版本**: v3.4.0
> **更新日期**: 2026-08-04
> **核心理念**: 规则即模板，模板即规则
> **核心公式**: `基础规则 + 定制需求 = 项目定制规则`

---

## 简介

这是一个**可组合、可定制、全流程覆盖**的 AI 编码规范系统，采用 **common/ + stacks/** 双库架构：

- **common/** — 通用库，所有技术栈皆可用的规则
- **stacks/** — 专用库，按技术栈分类的专用规则和配置

### 关联体系

| 仓库 | 路径 | 职责 |
|------|------|------|
| 编码规则 | `D:\Code\.Rules`（本仓库） | 编码规范、开发流程、项目结构 |
| 提示词库 | `D:\Code\.prompt` | 提示词存储、解决方案沉淀、经验复用 |

---

## 入口导航（单事实来源，NEVER 在本文件重复维护）

| 文件 | 用途 |
|------|------|
| `main.md` | 完整规则索引 + 使用指南（人类阅读、项目初始化）；含架构图、规则层级、加载模型、变更记录 |
| `compact-core.md` | AI 精简核心（始终加载约束 + 按需加载清单），嵌入项目 CLAUDE.md / 全局 AGENTS.md |
| `common/core/trigger-mechanism.md` | 可复用触发机制（自动落盘行为注册表 + 初始化流程） |
| `ROOT.md` | 路径基准（仓库根 + 跨仓库路径 SSOT） |
| `USAGE-GUIDE.md` | 各工具接入方式（Claude Code / Trae / Deep Code） |
| `OrganizationAndUser.md` | 组织与开发者身份 SSOT |
| `PROJECTLIST.md` | 项目导航索引 |
| `ERROR/README.md` | 错误存储与纠正系统说明 |
| `workflow/README.md` | 项目工作流程样本库索引 |

---

## 快速开始

### 直接使用

```
请按照 D:\Code\.Rules\main.md 规则体系进行开发。
```

### 使用预设

```
请按照 AI-Rules 规则体系，使用 stacks/flutter/presets/flutter.yaml 预设，
生成我的 Flutter 项目定制规则。
```

### 完全定制

1. 复制 `common/templates/custom-requirements.yaml` 并填写项目信息
2. 提供给 AI 生成项目定制规则

---

## 贡献指南

1. 核心规则 (`common/core/`) 变更需谨慎评估，并同步 `compact-core.md` 与 `main.md`
2. 新增预设 (`stacks/{tech}/presets/`) 欢迎提交
3. 新增技术栈请在 `stacks/` 下创建对应目录
4. 完整变更记录见 `main.md`

---

*规则即模板，模板即规则*
*让 AI 编码更规范、更高效*
