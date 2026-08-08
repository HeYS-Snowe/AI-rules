---
title: 项目存在 AGENTS.md 时，Deep Code 用户级全局规则不加载
type: workflow
tags: [deepcode, agents-md, scope, hierarchy]
severity: medium
status: resolved
first_seen: 2026-08-01
last_seen: 2026-08-03
occurrences: 1
---

## 错误描述

**发生时间**: 2026-08-01（设计全局规则接入时）
**错误内容**: 以为 `~/.deepcode/AGENTS.md` 在所有会话都生效；实际 Deep Code 的 `loadAgentInstructions()`（session.ts）**优先加载项目级**（`./.deepcode/AGENTS.md` → `./AGENTS.md`），只有项目级不存在时才回退到用户级 `~/.deepcode/AGENTS.md`。项目有自己的 AGENTS.md 时，全局规则被静默跳过。
**错误诱因**: 未读源码，假设用户级规则会与项目级合并。

## 环境信息

- 工具: Deep Code CLI v0.1.34
- 文件: `packages/core/src/session.ts`（loadAgentInstructions 实现）

## 复现步骤

1. 配置 `~/.deepcode/AGENTS.md`（全局规则）
2. 进入一个有 `AGENTS.md` 的项目运行 deepcode
3. 全局规则内容不生效（只加载了项目文件）

## 解决方案

- 项目文件显式引用全局规则：`全局规则见 ~/.deepcode/AGENTS.md（必须遵守）`
- 或把通用规则放入项目 `.deepcode/AGENTS.md`

## 预防措施

- 文档中明确"用户级是回退而非合并"的语义（已写入 USAGE-GUIDE.md §四）
- 涉及"某配置是否全局生效"时先查源码加载顺序，NEVER 凭直觉
