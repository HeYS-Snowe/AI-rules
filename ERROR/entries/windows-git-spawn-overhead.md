---
title: Windows 下循环内逐文件 spawnSync git 进程导致事件循环阻塞数秒
type: environment
tags: [windows, git, spawnsync, performance, event-loop]
severity: high
status: resolved
first_seen: 2026-08-13
last_seen: 2026-08-13
occurrences: 2
---

## 错误描述

**发生时间**: 2026-08-13 会话中（deepcode-cli 首条消息 spinner 冻结；/undo 恢复慢）

**错误内容**: 对 N 个文件循环执行 `spawnSync("git", ...)`（每个文件一个 `git hash-object` / `git cat-file` 进程），Windows 上每次进程 spawn 约 30-50ms，N 达到数百时同步阻塞事件循环 5-20 秒——UI 表现为 spinner 停摆数秒后恢复、/undo 卡顿。两个具体场景：① 首条消息的文件历史快照（stat 缓存是进程内存级，每次启动 CLI 都是冷缓存，首条消息必然全量哈希）；② 恢复检查点时逐文件读 blob。

**错误诱因**: 同步 spawnSync + 每文件一进程的调用模式。Windows 进程创建开销（CreateProcess + 杀毒扫描）远高于 Unix fork。

## 环境信息

- Windows 11 Pro + Node 24.18.0 + git 2.x
- deepcode-cli 文件历史系统（hash-object/cat-file 封装）

## 复现步骤

1. 对一个含 200+ 文件的清单逐文件 spawnSync `git hash-object -w -- file`
2. 在 UI 线程（事件循环）中执行，观察 spinner 停摆数秒

## 解决方案

### 方案一：批量合并成单进程（首选）

```ts
// 哈希全部文件：一次进程
git hash-object -w --stdin-paths   // stdin 传换行分隔的路径列表，输出逐行哈希
// 读取全部 blob：一次进程（cat-file --batch，stdin 逐行 sha，输出 header+content 流）
git cat-file --batch
```

### 方案二：异步化 + 缓存

stat 缓存（mtime+size）避免重复哈希；真正的 spawn 用异步 API 并让出事件循环。

## 预防措施

- 设计文件批量操作时默认"一进程多输入"，拒绝"一文件一进程"
- 进程级缓存（stat cache）记得考虑冷启动场景——首次运行必然全量，必须靠批量而不是假设缓存命中
