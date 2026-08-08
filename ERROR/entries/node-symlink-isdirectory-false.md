---
title: Node 符号链接条目 isDirectory() 为 false，扫描需检查 isSymbolicLink()
type: knowledge
tags: [node, filesystem, symlink, junction, skill-scan]
severity: medium
status: resolved
first_seen: 2026-08-01
last_seen: 2026-08-03
occurrences: 2
---

## 错误描述

**发生时间**: 2026-08-01（统计 skills 数量时）
**错误内容**: `fs.readdirSync(root, { withFileTypes: true })` 遍历一个**由符号链接组成的目录**（如 `~/.claude/skills` 的 104 个 junction）时，用 `entry.isDirectory()` 过滤得到 0 个目录——因为符号链接条目的 `isDirectory()` 为 `false`，必须检查 `entry.isSymbolicLink()`。
**错误诱因**: 假设目录条目就一定是目录；未考虑 Windows junction / symlink 场景。

## 环境信息

- OS: Windows 11（junction 由 `mklink /J` 创建）
- 场景: `C:\Users\aaa\.claude\skills`（104 个符号链接 → `.cc-switch\skills`）、`C:\Users\aaa\.agents\skills`（junction → `D:\Code\.AI-TOOLS\.skills`）

## 复现步骤

1. `fs.readdirSync('C:/Users/aaa/.claude/skills', {withFileTypes:true})`
2. 过滤 `e.isDirectory()` → 空
3. 过滤 `e.isDirectory() || e.isSymbolicLink()` → 104 个

## 解决方案

```javascript
const entries = fs.readdirSync(root, { withFileTypes: true });
for (const e of entries) {
  if (!e.isDirectory() && !e.isSymbolicLink()) continue; // 符号链接目录也算
  if (!fs.existsSync(path.join(root, e.name, 'SKILL.md'))) continue; // 再解析 SKILL.md
}
```

Deep Code 的 `collectSkills`（session.ts）正是这样处理的。

## 预防措施

- 扫描"可能由 junction/symlink 组成的目录"时，把 `isSymbolicLink()` 与 `isDirectory()` 并列
- 用 `fs.existsSync(子路径)` 做二次确认（能跟随解析即有效），不要只看条目类型
