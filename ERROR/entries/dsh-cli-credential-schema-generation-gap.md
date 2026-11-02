# dsh CLI 与桌面版凭证文件 schema 代差（headless 启动即崩）

**严重等级**: medium
**发生时间**: 2026-11-03
**环境**: Windows 11 / @deepseek-ai/dsh npm 全局 0.1.0-rc.6（2026-08 装）/ 桌面版 2026-10-06 起写入新凭证格式 / Node v24.18.0

## 问题现象

`dsh --profile headless "<任务>"` 启动即崩（plugin tree failed to load）：
1. 第一层报错：`credentials-local: the value for "version" in .credentials.yaml must be a string`
2. 修复 version 后逐个冒泡：`refs must be a string` → `records must be a string`
3. 升级 CLI 后格式解析通过，但业务层报 `MISSING_CREDENTIAL: llm-deepseek: no API key for provider route "deepseek-official"`——尽管 records 里明确有 DEEPSEEK_API_KEY 条目

## 根本原因

**npm 全局 CLI 与桌面版存在两个代差**：
1. **文件 schema 代差**：桌面版（2026-10-06 起）把 `~/.dsh/.credentials.yaml` 升级为三段式 `{version, refs, records}`；0.1.0-rc.6 的解析器只认旧版平铺 `{ref: "value"}`，把 version/refs/records 当凭证条目校验（要求每值皆字符串）——源码 grep 确认 rc.6 解析器 0 处提及 version/refs
2. **凭证服务路由代差**：升级 0.2.1-rc.5 后三段式识别通过，但 records 内条目带 `{kind, payload, secret, id}` 包装，CLI 按 provider route 找 key 时未匹配——desktop 的凭证写入路径与 CLI 读取路径未打通（未继续深挖）

## 解决方案

1. **升级 CLI 到 0.2.1-rc.5**（`npm i -g @deepseek-ai/dsh@0.2.1-rc.5`）——格式层打通（回滚：`npm i -g @deepseek-ai/dsh@0.1.0-rc.6`）
2. **环境变量直供 key**（报错信息自给的捷径）：`setx DEEPSEEK_API_KEY "<key>"` 后重开 shell——绕过凭证文件路由问题

## 样例

- 修复第 1 行类型：`sed -i '1s/^version: 1$/version: "1"/' ~/.dsh/.credentials.yaml`（数字→字符串）
- 解析器 schema 实证：`parseCredentialsDocument` 遍历顶层 key 要求 `typeof value === "string"`，空值报错、映射值报"must be a string"

## 标签

`dsh` `headless` `credentials` `版本代差` `桌面版与CLI分叉`

## 事后补录（2026-11-03 同日事故链完整版）

**为救 CLI 把桌面版搞崩了**：将顶层 `version: 1` 改为 `version: "1"` 后，桌面版（`D:\DeepSeek Harness\resources\app.asar`，自带独立内核）启动即崩——报错 `declares version "1"; this build reads version 1`：桌面版要求数字。

**架构真相（重要修正）**：桌面版与 npm CLI **代码完全独立**（桌面版自带 app.asar 内核，npm 全局升级不影响它），但**共享 `~/.dsh` 数据文件**（凭证/会话/profiles）。改共享数据前必须盘点所有读写方。

**最终解（双端兼容）**：回滚 `version: 1`（数字）。0.2.1-rc.5 对数字/字符串都接受——rc.6 的 "must be a string" 是旧解析器把 version 误当凭证条目的错误行为，不是真 schema 要求。

**遗留观察**：桌面版崩溃日志里另有 `llm-deepseek-account: models is no longer configurable` 激活失败（桌面版自身更新遗留，与本次修改无关）——回滚后若仍崩再处理。

**升级守则**：升级共享数据的任何一端前，先列出所有读者（桌面版/CLI/web 三端）各自的 schema 版本；两端解析器对同一字段的合法类型可能相反。
