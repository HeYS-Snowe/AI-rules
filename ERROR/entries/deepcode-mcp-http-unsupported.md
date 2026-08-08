---
title: Deep Code MCP 仅支持 stdio，HTTP MCP 需用 mcp-remote 包装
type: config
tags: [deepcode, mcp, settings, http]
severity: medium
status: resolved
first_seen: 2026-08-01
last_seen: 2026-08-03
occurrences: 2
---

## 错误描述

**发生时间**: 2026-08-01（配置 Deep Code 全局 settings.json 时）
**错误内容**: 把 `.codex/config.toml` 中 `type = "http"` 的 MCP 服务器（web-reader / web-search-prime / zread，带 `url` + `http_headers`）原样抄入 `~/.deepcode/settings.json` 的 `mcpServers` 后，这些服务器不生效——Deep Code 的 `McpServerConfig` 类型只有 `command` / `args` / `env`，**没有 `url` / `headers` 字段**，HTTP 传输不受支持（源码 `packages/core/src/settings.ts` 确认）。
**错误诱因**: 假设所有支持 MCP 的 CLI 都支持 HTTP 传输；未先核对目标工具的配置 schema。

## 环境信息

- OS: Windows 11
- 工具: Deep Code CLI v0.1.34（@vegamo/deepcode-cli）
- 对照: .codex/config.toml 中同批 HTTP MCP 正常

## 复现步骤

1. 在 `~/.deepcode/settings.json` 写入 `"mcpServers": { "web-reader": { "url": "...", "http_headers": {...} } }`
2. 启动 deepcode，执行 `/mcp`
3. 该服务器不出现或启动失败

## 解决方案

1. 核对工具支持的传输类型（查源码类型定义或官方文档）
2. stdio MCP 直接映射：`command` + `args` + `env`
3. HTTP MCP 用 stdio 代理包装：`npx -y mcp-remote <url> --header "Authorization: Bearer <key>"`

## 预防措施

- 跨工具迁移配置前，先确认目标工具 `McpServerConfig` 的 schema（Deep Code 查 `settings.ts`）
- 迁移后立即用 `/mcp` 验证，不要假设"别的工具能用这里也能用"
