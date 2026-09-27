---
title: 雁阵协作下 git add -A 盲打打包并行会话的删除
type: workflow
tags: [git, multi-agent, echelon, add-a, 工作区]
severity: high
status: resolved
first_seen: 2026-09-27
source: AI资源管理 会话（v3.8.0 提交 bb56bc4 误删 multi-agent.md，c07d7be 恢复）
---

# 雁阵协作下 git add -A 盲打打包并行会话的删除

## 错误描述

在 `.Rules` 仓库执行 `git add -A && git commit` 提交 v3.8.0 时，把**另一个并行会话（Codex/其他工具）删除的 `common/core/multi-agent.md`** 一并打包提交——导致雁阵规范本体随一次无关提交被删，`main.md` 两处引用变成死链。

**隐性错误特征**：提交本身"成功"、commit message 与实际改动无关的文件无冲突提示、`tail -1` 截断的 git status 输出看不出删除行——静态看一切正常，只有事后 `git show --stat` 才暴露 `delete mode` 行。

## 根本原因

1. **`git add -A` 在多智能体共享仓库是危险操作**——它无差别打包工作区的所有变更（含其他 Agent/会话的删除、改名、半成品）
2. 提交前 `git status --short | head -3`（tail 截断）——违反 §一.2 先看再改，没看全量状态就打包
3. 雁阵场景（multi-agent.md §四）本要求"文件所有权登记"——提交（对仓库根的写操作）前应核对所有权/工作区归属

## 解决方案

```yaml
多智能体共享仓库的提交纪律:
  - NEVER 在雁阵共享仓库使用 git add -A — 改用 git add <明确文件列表>（只加自己改的文件）
  - 提交前必看全量 git status（不截断），发现不认识的变更（删除/改名/陌生文件）→ 先查明归属，NEVER 顺手打包
  - 若误打包: git show --stat HEAD 核对，从历史版本恢复被误删文件（git checkout <commit>^ -- <path>），补提交说明恢复原因
```

## 预防措施

- compact-core §一.2（先看再改）在 git 场景的具体化：**先看全量 status 再 add**
- 雁阵协作（multi-agent.md §四）场景下，add -A 等价于"侵犯其他 Agent 的文件所有权"
- 实证关联：c07d7be 恢复提交；若删除系并行会话有意为之，用户会纠正（截至恢复时未获反对）
