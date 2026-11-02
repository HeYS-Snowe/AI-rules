---
title: npm 全局更新 claude-code 报 EBUSY（claude.exe 被运行中进程锁定）
type: environment
tags: [npm, windows, ebusy, claude-code, file-lock, global-install]
severity: low
status: resolved
first_seen: 2026-10-20
last_seen: 2026-10-20
occurrences: 1
---

## 错误描述

**发生时间**: 2026-10-20 15:50 左右
**错误内容**: `npm install -g @anthropic-ai/claude-code` 报错：

```
npm error EBUSY: resource busy or locked, copyfile
'D:\...\node_modules\@anthropic-ai\claude-code\node_modules\@anthropic-ai\claude-code-win32-x64\claude.exe'
-> 'D:\...\node_modules\@anthropic-ai\.claude-code-mw7iEK1b\...'
```

**错误诱因**: Windows 不允许覆盖/删除正在运行的 exe。当时有两个 claude 进程在运行（`Get-Process claude` 确认：PID 40924、41472），锁定了 npm 全局目录下的 claude.exe，npm 在 reify（整理目录）阶段复制文件失败并回滚。其中一个进程就是发起本次咨询的 Claude Code 会话本身——在 claude 运行时升级 claude，必然撞锁。

## 环境信息

- Windows 11 Pro 10.0.26340
- npm 全局目录：`D:\Development_Kit\Node\LIB\npm_global`
- @anthropic-ai/claude-code 2.1.304（本次为同版本重装/更新尝试）

## 复现步骤

1. 启动任意 Claude Code 会话（claude.exe 进入运行状态）
2. 在任意终端执行 `npm install -g @anthropic-ai/claude-code`
3. npm 复制 claude.exe 时报 EBUSY

## 解决方案

### 方案一：先关闭所有 claude 进程，再安装

1. 退出所有 Claude Code 会话窗口
2. 新开一个独立 PowerShell（pwsh），保险起见先清残留进程：
   ```pwsh
   Get-Process claude -ErrorAction SilentlyContinue | Stop-Process -Force
   npm install -g @anthropic-ai/claude-code
   claude --version   # 验证
   ```

### 本例实况（无需操作）

诊断时 `claude --version` 与 `npm list -g` 均显示 2.1.304（即 npm 最新版）——此前安装已成功，本次报错发生在重复安装同版本的 reify 阶段，失败后 npm 自动回滚，现有安装完好，无需重装。

## 预防措施

- 升级 claude-code 前先退出所有 Claude Code 会话（含后台残留进程），在独立终端执行安装
- 遇到 EBUSY 先 `Get-Process claude` 确认占用进程，勿盲目反复重试
