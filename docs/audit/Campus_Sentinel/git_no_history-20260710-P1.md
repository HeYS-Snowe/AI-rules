---
title: git 仓库存在但无任何提交历史
project: Campus_Sentinel
level: P1
category: git-hygiene
status: open
found: 2026-07-10
resolved:
---

## 问题描述

Campus_Sentinel 目录下存在 `.git`，但 git 无任何提交历史，且 `status` 报告 0 个文件——git 状态异常，无法判断工作内容是否被版本控制保护。

## 证据

2026-07-10 扫描输出：

```
===== Campus_Sentinel =====
branch :              ← 空（无 HEAD）
last   :              ← 空（无提交）
recent commits:       ← 空
uncommitted: 0 files  ← 反常：目录实有 backend/mobile/Kotlin/design 等内容
```

- `git rev-parse --abbrev-ref HEAD` 与 `git log -1` 均返回空 → 仓库从未提交过
- `git status --porcelain` 返回 0 文件，与目录实际内容矛盾 → 疑似内容被 `.gitignore` 全部忽略，或 git 未正确跟踪

根因无法仅凭扫描断定，需人工排查。

## 建议

1. 在 `D:\Code\Project\Campus_Sentinel` 执行 `git status` 与 `git log --oneline` 确认实际状态
2. 检查 `.gitignore` 是否过度忽略（把 backend/mobile 等也忽略了）
3. 根据排查结果二选一：
   - 若从未提交：`git add . && git commit -m "chore: 初始提交"`
   - 若 .gitignore 过度：修正 .gitignore 后提交
4. CLAUDE.md 标注项目"筹备中（文档阶段，尚无代码）"，但目录已有 backend/mobile 等——同步核实这些目录是空壳还是已有内容，并更新 CLAUDE.md 状态

## 解决记录

**2026-07-10 已诊断并修复 init**：

- 根因：`.git` 是一个**完全空的目录**（无 HEAD / config / objects / refs），系 7/5 某次 `git init` 中断或误操作留下的空壳，git 因此不识别为仓库
- 修复：`rmdir .git`（确认空目录后删除，零数据损失）→ `git init` 成功，默认分支 `master`
- 当前：15 个未跟踪项待首次提交（.gitignore / CLAUDE.md / backend / mobile / Kotlin / design / docs 等）
- 剩余待办：① 建议将默认分支 `master` 改为 `main`（与其他项目一致）② `.trae/`、`.\对话.txt` 等加入 .gitignore ③ 执行首次提交
- 状态：git 异常已解除，降为常规"首次提交"待办；待用户首次提交后本条目置 resolved
