# 错误资源库索引

> 自动维护 — 新增或更新条目时同步更新本文件
> 完整说明见 [README.md](README.md)

---

## 按类型

### code

- [inline-span-transform-ignored](entries/inline-span-transform-ignored.md) — 裸 span（非替换 inline）不可 transform，transform 计算值存在但被静默忽略，需 display:inline-block
- [catch-comment-claims-phantom-mechanism](entries/catch-comment-claims-phantom-mechanism.md) — 空 catch 注释声称"由别处弹出"但该机制不存在——采信注释前先验证机制真实存在

### config

- [deepcode-mcp-http-unsupported](entries/deepcode-mcp-http-unsupported.md) — Deep Code MCP 仅支持 stdio，HTTP MCP 需用 mcp-remote 包装

### dependency

- [copied-venv-pip-target-source](entries/copied-venv-pip-target-source.md) — 复制 venv 后用 Scripts\pip.exe 安装包会落到原始 venv，用 python -m pip

### environment

- [windows-git-spawn-overhead](entries/windows-git-spawn-overhead.md) — Windows 循环内逐文件 spawnSync git 阻塞事件循环数秒，批量合并单进程
- [pygame-mixer-forces-stereo](entries/pygame-mixer-forces-stereo.md) — pygame mixer 强制 stereo，get_raw 需去交错才可当单声道（写 WAV/喂 ASR 前先实测 get_init）
- [flutter-gradle-init-repositories-conflict](entries/flutter-gradle-init-repositories-conflict.md) — 用户级 Gradle init 脚本全局注入仓库，与 Flutter included build 的 FAIL_ON_PROJECT_REPOS 冲突
- [flutter-pub-cache-cross-drive-rename](entries/flutter-pub-cache-cross-drive-rename.md) — Windows 上 Pub 更新依赖缓存时临时包目录跨盘重命名失败，本地验证可在现有依赖可用时使用 `--no-pub`

### ai-behavior

- [heredoc-append-duplicate-symbol](entries/heredoc-append-duplicate-symbol.md) — heredoc 盲追加 TS 文件导致重复 main() 声明，优先用 Edit 工具

### workflow

- [deepcode-agentsmd-scope](entries/deepcode-agentsmd-scope.md) — 项目存在 AGENTS.md 时，Deep Code 用户级全局规则不加载

### knowledge

- [node-symlink-isdirectory-false](entries/node-symlink-isdirectory-false.md) — Node 符号链接条目 isDirectory() 为 false，扫描需检查 isSymbolicLink()
- [gitee-release-chinese-garbled](entries/gitee-release-chinese-garbled.md) — Gitee 发布四坑：令牌作 username、target_commitish、中文走 JSON 文件传输、附件 100MB 上限
- [node-spawnsync-buffer-input](entries/node-spawnsync-buffer-input.md) — spawnSync encoding=buffer + 字符串 input 抛 ERR_UNKNOWN_ENCODING（被 catch 吞掉时静默失败）
- [terminal-glyph-width-mismatch](entries/terminal-glyph-width-mismatch.md) — 终端字符宽度三方（Ink 测量/终端渲染/项目自算）不一致导致动画帧抖动，修复靠实测不靠猜测
- [hidden-message-breaks-sequence-asserts](entries/hidden-message-breaks-sequence-asserts.md) — 新增隐藏消息打破角色序列断言，测试定位须 visibility 过滤
- [migration-script-not-idempotent](entries/migration-script-not-idempotent.md) — 迁移脚本必须幂等——数据已被迁移后重跑会二次嵌套目录

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

### flutter / gradle

- [flutter-gradle-init-repositories-conflict](entries/flutter-gradle-init-repositories-conflict.md)

### flutter / windows

- [flutter-pub-cache-cross-drive-rename](entries/flutter-pub-cache-cross-drive-rename.md)

### terminal / unicode

- [terminal-glyph-width-mismatch](entries/terminal-glyph-width-mismatch.md)

### css / transform

- [inline-span-transform-ignored](entries/inline-span-transform-ignored.md)

### gitee / release

- [gitee-release-chinese-garbled](entries/gitee-release-chinese-garbled.md)
