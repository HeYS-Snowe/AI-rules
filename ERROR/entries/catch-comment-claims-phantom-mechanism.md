---
title: catch 注释声称的兜底机制不存在——静默吞错被当成"已有提示"
type: code
tags: [error-handling, catch, silent-failure, comment-drift]
severity: medium
status: resolved
first_seen: 2026-09-10
last_seen: 2026-09-10
occurrences: 1
---

## 错误描述

**发生时间**: 2026-09-10（Trace 会话）
**错误内容**: NameDialogModal 提交失败的 `catch {}` 空块上注释写着"重名等错误提示由 IPC 层弹出"——但 IPC 层（ipc-client）只 throw ClientError、**无任何弹层机制**，tree-store 也只透传。错误被无声吞掉，用户点创建重名时"点击以后没效果"（对话框开着但零反馈）。注释描述的机制是**幻觉**（可能来自从未落地的计划，或机制在重构中被删而注释未同步）。
**错误诱因**: 用注释代替实现；评审/排障时倾向相信注释而不验证其声称的机制真实存在。

## 环境信息

- Electron + TS 渲染层；错误链路：main 抛业务错 → ipc-client throw → store 透传 → 组件 catch 吞

## 复现步骤

1. 新建与现有计划/文件夹同名的节点
2. 点创建 → 无任何提示（catch 空块 + 幻觉注释）

## 解决方案

### 方案一：catch 显式弹因 + 校验注释真实性

1. `catch (e) { message.error(e instanceof ClientError ? e.message : t('errors.opFailed')) }`，对话框保持开启供改名
2. 排障时对"注释声称由别处处理"的兜底，**先验证别处真的处理了**（grep 该机制是否存在）再采信

## 预防措施

- 空 catch 的注释必须指向真实存在的处理点；写"由 X 层弹出"前先确认 X 有弹层代码
- 代码审查检查项：空 catch + 长注释解释 = 高危信号
