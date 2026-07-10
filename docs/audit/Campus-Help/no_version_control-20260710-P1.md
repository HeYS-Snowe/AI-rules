---
title: 活跃开发却未纳入版本控制
project: Campus-Help
level: P1
category: git-hygiene
status: resolved
found: 2026-07-10
resolved: 2026-07-10
---

## 问题描述

校园互助产品（`D:\Code\Project\创新创业小组作业`，原误记为"叩心 KouXin"）正在活跃开发，但整个项目未纳入 git——无版本控制保护，代码/文档一旦误删或覆盖无法恢复。

## 证据

2026-07-10 扫描输出：

```
===== 创新创业小组作业 =====
NOT a git repo — recent files:
    Jul 9 12:59 AGENTS.md      ← 昨天仍在改
    Jul 8 18:07 doc
    Jul 6 13:28 uniapp
    Jun 16 13:22 miniprogram
    Jun 15 21:29 CLAUDE.md
```

- 目录无 `.git`，所有改动均无版本历史
- 近期改动频繁（7/06、7/08、7/09），丢失风险随活跃度上升

## 建议

1. 在 `D:\Code\Project\创新创业小组作业` 初始化 git
2. 创建 `.gitignore`（排除 `node_modules/`、`uniapp/unpackage/`、小程序私有配置等）
3. 首次提交
4. 建议关联远程仓库做异地备份

## 解决记录

**2026-07-10 已建仓**：

- 新建 `.gitignore`（忽略 `node_modules/`、`miniprogram/miniprogram_npm/`、`uniapp/unpackage/`、`miniprogram/project.private.config.json`、`.claude/settings.local.json`、`.对话.txt`、IDE/系统文件等）
- `git init` + 默认分支 `main` + 首次提交 `0e1a88b`（227 文件，16376 行）
- **miniprogram 嵌套仓库修复**：miniprogram 原含误建的 `.git`（仅 1 个自动 Initial Commit，无 remote），导致被当成坏 submodule、内容未入库；已删除其 `.git`，miniprogram 作为普通子目录并入（118 文件）
- **结构勘误**：原"叩心 KouXin"为组织误称，已统一为 Qore；该目录是**单产品双端**（miniprogram 微信原生 + uniapp 跨端），非多个独立项目
- 剩余建议：关联远程仓库做异地备份

> 关联：本条修正同步至 `PROJECTLIST.md`（校园互助归入 Qore 产品线）；本项目 audit 标识为 `Campus-Help`（英文代号待用户确认正式名）。
