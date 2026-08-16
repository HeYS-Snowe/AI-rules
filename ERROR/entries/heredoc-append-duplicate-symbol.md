---
title: 向已有 TS 文件 heredoc 追加代码导致重复声明 TransformError
type: ai-behavior
tags: [heredoc, typescript, duplicate-declaration, append]
severity: medium
status: resolved
first_seen: 2026-08-14
last_seen: 2026-08-14
occurrences: 1
---

## 错误描述

**发生时间**: 2026-08-14 会话中（deepcode-cli 验证脚本扩展）

**错误内容**: 用 `cat >> file.ts << 'EOF'` 向已有 TypeScript 文件追加测试函数和 `main()` 时，未检查文件尾部——原文件已有 `main()` 和入口调用，追加产生第二个 `main()` 声明，tsx 报 `TransformError`（重复标识符）。

**错误诱因**: 追加式修改前没有先读文件尾部结构；本案例同时违反"先看再改"——追加内容与原文件的入口结构重复。

## 环境信息

- Windows 11 + Node 24 + tsx
- packages/core/src/tests/verify-hotpaths.manual.ts（含 main + main().catch 入口模式）

## 复现步骤

1. 对已有入口模式的 TS 文件（含 `async function main()` + `main().catch(...)`）
2. 用 heredoc 追加新函数和一个新的 `main()`
3. 运行 → TransformError: Identifier 'main' has already been declared

## 解决方案

### 方案一：追加前读尾部，然后并入现有结构

先读文件尾部（或整文件），把新函数插到现有 `main()` 之前，并在现有 main 里加分支调用——不重复声明入口。

### 方案二：用 Edit 工具替代 heredoc 追加

Edit 要求 old_string 精确匹配，天然强制"先读后改"，从机制上避免盲追加。

## 预防措施

- **NEVER 对已有入口结构的文件盲追加**：追加前必须读尾部确认无重名符号
- 优先用 Edit（精确替换）而非 `cat >>`（盲追加）——Edit 的 old_string 匹配失败本身就是保护
