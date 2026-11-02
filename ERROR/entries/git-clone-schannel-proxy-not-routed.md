---
title: git clone GitHub 传输中断（本机代理在跑但 git 未走代理）
type: environment
tags: [git, clone, schannel, proxy, github, curl-56, early-eof]
severity: medium
status: resolved
first_seen: 2026-10-23
last_seen: 2026-10-23
occurrences: 1
---

## 错误描述

**发生时间**: 2026-10-23 上午
**错误内容**: `git clone https://github.com/langchain-ai/langchain.git` 传输中途失败：

```
error: RPC failed; curl 56 schannel: server closed abruptly (missing close_notify)
error: 2342 bytes of body are still expected
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```

**错误诱因**: 本机代理（127.0.0.1:7890，Clash 系端口）在运行，但 git 未配置代理 → 裸连 GitHub。小请求（`git ls-remote` 握手）正常，大数据流（pack 传输）被链路中断。git 全局 `http.proxy` 为空、`http.sslBackend=schannel`。

## 环境信息

- Windows 11 Pro / git 2.54.0.windows.1（schannel 后端）
- 本机代理：127.0.0.1:7890（监听中，git 未感知）
- 项目上下文：克隆 langchain 仓库（30 万+ 对象，全量数 GB）

## 复现步骤

1. 本机开着代理软件，git 不配代理，直连 GitHub
2. clone 大仓库（或链路不稳时中等仓库）→ 传输中途被掐

关键鉴别特征：`git ls-remote` 成功但 clone/fetch 中途断；浅克隆（`--depth 1`）也同样断 → **不是传输量问题，是链路问题**。

## 解决方案

### 方案一：让 git 只对 github.com 走本机代理（已验证有效）

1. 先探测本机是否有代理在监听：
   ```bash
   netstat -ano | grep -E "127\.0\.0\.1:(7890|7897|10808|10809|1080|2080|8889)" | grep LISTEN
   ```
2. 只对 github.com 域配置代理（不影响其他 git 远端）：
   ```bash
   git config --global http.https://github.com.proxy http://127.0.0.1:7890
   ```
3. 配合 `--depth 1` 浅克隆进一步降低传输量；需要完整历史时 `git fetch --unshallow`。

撤销方式（代理软件关闭后 GitHub 操作会失败，届时执行）：

```bash
git config --global --unset http.https://github.com.proxy
```

### 方案二（代理不可用时）：分步拉取拆短传输

`--depth 1` 后反复 `git fetch --deepen 100` 逐步加深；必要时调大 `http.postBuffer`、设 `http.lowSpeedLimit 0`。

## 预防措施

- 遇到 `curl 56 schannel` / `early EOF` 类 GitHub 传输中断，**先 netstat 检测本机代理端口**，别先怀疑仓库大小或反复重试——本机有代理而 git 没走是最常见根因
- 浅克隆失败可排除"传输量太大"假设，直接转向链路诊断
- 长期方案：代理软件常开则保留该 git 配置；代理关开频繁的用户可用环境变量 `HTTP_PROXY/HTTPS_PROXY` 方案替代（随终端生效）
