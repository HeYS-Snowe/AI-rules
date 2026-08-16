# 错误资源库索引

> 自动维护 — 新增或更新条目时同步更新本文件
> 完整说明见 [README.md](README.md)

---

## 按类型

### code

### config

- [deepcode-mcp-http-unsupported](entries/deepcode-mcp-http-unsupported.md) — Deep Code MCP 仅支持 stdio，HTTP MCP 需用 mcp-remote 包装

### dependency

### environment

- [windows-git-spawn-overhead](entries/windows-git-spawn-overhead.md) — Windows 循环内逐文件 spawnSync git 阻塞事件循环数秒，批量合并单进程

### ai-behavior

- [heredoc-append-duplicate-symbol](entries/heredoc-append-duplicate-symbol.md) — heredoc 盲追加 TS 文件导致重复 main() 声明，优先用 Edit 工具

### workflow

- [deepcode-agentsmd-scope](entries/deepcode-agentsmd-scope.md) — 项目存在 AGENTS.md 时，Deep Code 用户级全局规则不加载

### knowledge

- [node-symlink-isdirectory-false](entries/node-symlink-isdirectory-false.md) — Node 符号链接条目 isDirectory() 为 false，扫描需检查 isSymbolicLink()
- [node-spawnsync-buffer-input](entries/node-spawnsync-buffer-input.md) — spawnSync encoding=buffer + 字符串 input 抛 ERR_UNKNOWN_ENCODING（被 catch 吞掉时静默失败）
- [terminal-glyph-width-mismatch](entries/terminal-glyph-width-mismatch.md) — 终端字符宽度三方（Ink 测量/终端渲染/项目自算）不一致导致动画帧抖动，修复靠实测不靠猜测
- [hidden-message-breaks-sequence-asserts](entries/hidden-message-breaks-sequence-asserts.md) — 新增隐藏消息打破角色序列断言，测试定位须 visibility 过滤

---

## 按标签

### deepcode

- [deepcode-mcp-http-unsupported](entries/deepcode-mcp-http-unsupported.md)
- [deepcode-agentsmd-scope](entries/deepcode-agentsmd-scope.md)
- [windows-git-spawn-overhead](entries/windows-git-spawn-overhead.md)
- [node-spawnsync-buffer-input](entries/node-spawnsync-buffer-input.md)
- [terminal-glyph-width-mismatch](entries/terminal-glyph-width-mismatch.md)

### node / filesystem

- [node-symlink-isdirectory-false](entries/node-symlink-isdirectory-false.md)
- [node-spawnsync-buffer-input](entries/node-spawnsync-buffer-input.md)

### mcp

- [deepcode-mcp-http-unsupported](entries/deepcode-mcp-http-unsupported.md)

### performance / git

- [windows-git-spawn-overhead](entries/windows-git-spawn-overhead.md)

### terminal / unicode

- [terminal-glyph-width-mismatch](entries/terminal-glyph-width-mismatch.md)
