---
title: Node spawnSync 在 encoding=buffer 时传字符串 input 抛 ERR_UNKNOWN_ENCODING
type: knowledge
tags: [node, child-process, spawnsync, buffer, git]
severity: high
status: resolved
first_seen: 2026-08-13
last_seen: 2026-08-13
occurrences: 2
---

## 错误描述

**发生时间**: 2026-08-13 会话中（deepcode-cli 文件历史批量哈希重构，同一坑踩了两次）

**错误内容**: `child_process.spawnSync(cmd, args, { encoding: "buffer", input: "字符串" })` 抛出 `TypeError [ERR_UNKNOWN_ENCODING]: Unknown encoding: buffer`（栈：`Buffer.from` → `node:child_process:899`）。更隐蔽的变体：被 try/catch 吞掉时表现为**静默失败**——上层函数返回 undefined，行为"莫名其妙不工作"（文件历史 checkpoint 全部失效，测试全红才暴露）。

**错误诱因**: Node 内部对字符串 input 执行 `Buffer.from(input, encoding)` 编码转换；`encoding: "buffer"` 不是合法文本编码，转换直接抛错。二进制的 stdout 需求（encoding=buffer）与字符串 stdin 输入不能共存。

## 环境信息

- Windows 11 Pro + Node 24.18.0（Linux/macOS 同样触发）
- deepcode-cli 文件历史系统（GitFileHistory.spawnGit 封装，encoding 参数化）

## 复现步骤

1. 写一个 spawnSync 封装，encoding 取参数值 "buffer"
2. 调用时传 `input: "file.txt\n"`（字符串）
3. 观察抛出 ERR_UNKNOWN_ENCODING；若调用点有 try/catch 则静默吞掉

## 解决方案

### 方案一：input 转 Buffer（二进制输出场景）

```ts
const output = spawnSync("git", args, {
  encoding: "buffer",
  input: Buffer.from(paths.join("\n") + "\n", "utf8"), // 必须 Buffer
});
```

### 方案二：改用 utf8 编码（ASCII 输出场景）

hash-object 等纯十六进制输出的命令直接用 `encoding: "utf8"` + 字符串 input。

## 预防措施

- 封装 spawnSync 时在 `encoding === "buffer" && typeof input === "string"` 时显式抛错（fail-fast），或封装内自动转换
- 批量重构涉及进程调用的代码后，必须跑全量测试——静默失败只有测试能兜住
