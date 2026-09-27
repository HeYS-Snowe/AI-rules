---
title: Flutter pub test fails while renaming a temporary package across drives
type: environment
tags: [flutter, windows, pub, cache, cross-drive]
severity: low
status: open
first_seen: 2026-09-26
last_seen: 2026-09-26
occurrences: 2
---

## 错误描述

**发生时间**: 2026-09-26 会话中（Jeenith 本地 AI 案例功能验证）

**错误内容**: 在 `mobile/` 执行 `flutter test` 时，Flutter 尝试更新 Pub 依赖缓存，Windows 报告“系统无法将文件移到不同的磁盘驱动器”，测试未启动。

**错误诱因**: 尚未确认。报错发生在 Pub 对临时包目录执行重命名时，可能与临时目录和 Pub 缓存位于不同盘符有关；当前证据不足以断定根因。

## 环境信息

- Windows
- Flutter 桌面/移动项目：`D:\Code\Project\Qore\Jeenith\mobile`
- 操作：运行 Flutter 测试时发生 Pub 缓存更新
- 再次发生：2026-09-26 Android Release 构建在依赖更新阶段遇到相同错误，构建尚未开始，版本脚本已回滚版本号
- 已有依赖配置可复用；不涉及项目依赖文件变更

## 复现步骤

1. 在 Jeenith 的 `mobile/` 目录执行 `flutter test`。
2. Pub 更新依赖缓存时出现跨磁盘移动错误，测试无法启动。

## 解决方案

### 方案一：复用现有依赖配置完成本次验证

在确认现有 package 配置和依赖已存在后，使用 `flutter test --no-pub` 跳过 Pub 更新。本次完整测试集通过。

### 方案二：复用依赖完成 Android Release 构建

`mobile/scripts/build_apk.ps1` 支持 `-NoPub`，可在依赖已解析且项目未新增依赖时，把 `--no-pub` 传给 `flutter build apk`。本次 Android Release 构建通过。

## 预防措施

- 若 Flutter 命令只在 Pub 缓存更新阶段因跨盘重命名失败，先确认当前依赖配置是否可用，再使用 `--no-pub` 完成不需要解析新依赖的本地验证。
- 不要据此推断项目依赖配置错误；若需修复缓存根因，应先检查系统临时目录与 Pub 缓存的实际盘符及 Dart/Flutter 版本。
