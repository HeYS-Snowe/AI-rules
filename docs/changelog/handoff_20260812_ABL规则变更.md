# 交接文档：.Rules 规则变更（自包含版，供无上下文智能体阅读）

> 本文档完整交代背景、动作链、改动内容与当前状态，无需任何对话上下文即可理解。
> 生成：2026-08-12

---

## 一、背景（为什么有这次变更）

用户在进行一个 **AI 编码工具横向基准测试**（项目：`D:\Code\Project\ABL-Agent-Benchmark-Lab`），做了两件事：

1. **智能体性能测试**：用同一套 prompt 测 4 个工具（Claude Code CLI 最新版 / Claude Code CLI 2.1.88 源码版 / Claude Code Desktop / OpenAI Codex），3 个任务（json2csv、todo CLI 改造、notes CLI）各 2 轮。核心目的是评估 **2.1.88 版本在缓存命中率方面的价值**（不是比出基准排名）。
2. **系统提示词抓包**：通过本地代理（127.0.0.1:15999 转发 deepseek）抓取 Claude Code CLI 最新版在 20 个场景下的完整请求体，分析其系统提示词结构。

**关键发现**：
- 4 工具实测：Codex 综合 5.0 > CC CLI 最新 4.7 > 2.1.88 4.2 > Desktop 3.2
- 2.1.88 与最新版缓存命中率同为 **100%**，但 2.1.88 token 用量仅为最新版 1/5（系统提示词更精简）
- CC CLI 系统提示词**完全静态**（不注入任务指令）→ 前缀稳定 → 高缓存命中
- 抓包提炼出 CC 的"严格"行为约束清单（被拒不重试、操作前确认、如实汇报等），其中 2 条现有规则库缺失
- 实测发现 2 个可复现 Bug，其中 1 个暴露测试盲区（转义类测试只断言内容不断言格式）

用户据此要求：**总结沉淀到 `.prompt` 提示词库 + 优化 `.Rules` 编码规则库**。

---

## 二、动作链（本会话已完成的全部操作）

| # | 操作 | 结果 |
|---|------|------|
| 1 | 读取 ABL 项目产出（质量评审报告、抓包分析） | 理解测试结论与抓包发现 |
| 2 | 沉淀 2 个文件到 `.prompt`（提示词库） | 见 §三.1 |
| 3 | 更新 `.prompt\CLAUDE.md` 索引 | 通用 +2 条 |
| 4 | 优化 `.Rules\compact-core.md` | §一 12→14 条（+2 条） |
| 5 | 优化 `.Rules\common\core\workflow.md` | §4.1 新增测试盲区禁令 |
| 6 | 同步 3 个工具的全局配置文件 | Claude Code / deepcode / Codex |
| 7 | 生成改动文档 + diff | `docs\changelog\` 下 2 个文件 |

---

## 三、改动详情（文件级）

### 3.1 沉淀（新增，非修改）

**`D:\Code\.prompt\通用\多智能体基准测试方案-20260812\prompt.md`**（新增）
- 测试设计骨架：4 工具 × 3 任务 × 2 轮、独立工作区、统一模型/环境
- 7 维评分（完成度/质量/测试/安全/扩展性/性能/Bug）
- 实测结果：排名、2 个可复现 Bug、性能对照表
- 3 个任务的可复用 prompt

**`D:\Code\.prompt\通用\Agent系统提示词设计与缓存命中-20260812\prompt.md`**（新增）
- CC 系统提示词 7 段结构（6,920 字符）
- 严格属性 5 类清单（安全红线/行为约束/诚实条款/代码规范/记忆纪律）
- 关键发现 4 条（静态提示词 → 100% 缓存命中）
- 可复用 prompt：设计高缓存 agent + 抓包验证流程

### 3.2 规则库修改（.Rules 仓库，git 管理）

**`D:\Code\.Rules\compact-core.md`** — §一 AI 行为约束 12→14 条：
- **新增 13. 被拒不原样重试**：被拒绝的调用（权限拒绝/工具报错）不得原样重试，先调整再试。来源：CC 系统提示词实证（"adjust, don't retry verbatim"）
- **新增 14. 发送内容即发布**：发往外部服务的内容可能被缓存/索引，删除不可完全回收；涉及密钥/隐私先脱敏再发

**`D:\Code\.Rules\common\core\workflow.md`** — §4.1 测试层次新增「测试盲区禁令」：
- 序列化/转义类输出（CSV/JSON/HTML/SQL）必须断言完整输出格式，NEVER 只断言内容包含
- 必测场景：特殊字符 → 转义后完整串（逗号/引号/换行/对象嵌套/空值）
- 依据：ABL 实测 Bug——某工具 CSV 转换测试漏断言转义，对象含逗号时 CSV 损坏上线

### 3.3 全局配置同步（非 git 管理）

| 文件 | 说明 |
|------|------|
| `C:\Users\aaa\.claude\CLAUDE.md` | Claude Code 全局规则，§一 同步至 14 条 |
| `C:\Users\aaa\.deepcode\AGENTS.md` | deepcode 全局规则，§1 同步至 14 条 |
| `C:\Users\aaa\.codex\AGENTS.md` | Codex 全局规则（精简版），补 9、10 条（对应 13、14 的简化措辞） |

---

## 四、完整 diff（.Rules 仓库内，2 文件）

```diff
diff --git a/common/core/workflow.md b/common/core/workflow.md
index e30fc79..2d438d2 100644
--- a/common/core/workflow.md
+++ b/common/core/workflow.md
@@ -364,6 +364,11 @@
   工具: Appium / Flutter Integration Test
 ```
 
+**测试盲区禁令**（序列化/转义类函数）：
+
+- **NEVER 只断言"内容包含"** — 序列化/转义类输出（CSV/JSON/HTML/SQL 转义）必须断言**完整输出格式**（转义后的整串），而非仅断言内容片段。原因：实测案例——某工具 CSV 转换测试只断言 `JSON.stringify` 后内容、未断言转义，导致对象值含逗号时 CSV 结构损坏上线（ABL 基准测试 Bug 1，见 `.prompt\通用\多智能体基准测试方案-20260812`）
+- 序列化类测试至少覆盖：**特殊字符入参 → 断言转义后完整串**（逗号/引号/换行/对象嵌套/空值 null/undefined）
+
 ### 4.2 测试结构 (AAA 模式)
 
 ```dart
diff --git a/compact-core.md b/compact-core.md
index 1e67cdb..51f0699 100644
--- a/compact-core.md
+++ b/compact-core.md
@@ -25,6 +25,8 @@
 10. **主动生成改动文档** — 以下任一情况发生时，主动询问用户是否生成改动文档：(1) 版本号迭代时（修改 version 字段，或 commit message 含版本号）；(2) 本次会话已执行 3+ 次 git commit；(3) 用户表示完成一批工作或切换话题时（如"先这样吧""换个事"）。用户主动要求时立即生成。格式见 `workflow.md` §12。
 11. **两手准备，兜底必达** — 做事情要做两手准备（Plan A 与 Plan B 与…），第二个准备一定要保证可用。NEVER 只给唯一解；涉及部署/发布/迁移等不可逆操作时先确认回滚方案就绪。详见 `principles.md` §6.10。
 12. **PowerShell 一律用 pwsh** — Windows 下执行 PowerShell 命令一律用 `pwsh`（PowerShell 7.6.4，首选），NEVER 用 Windows 自带 `powershell`（5.1 旧版，仅作备用）。原因：5.1 默认 GBK 编码，中文注释/脚本易乱码。详见 `workflow.md` §3.4。
+13. **被拒不原样重试** — 被拒绝的调用（权限拒绝/工具报错）不得原样重试，先调整再试。原因：原样重试既浪费 token 又大概率再次被拒；"adjust, don't retry verbatim" 是 Claude Code 系统提示词的实证约束（来源：ABL 抓包，见 `.prompt\通用\Agent系统提示词设计与缓存命中-20260812`）。
+14. **发送内容即发布** — 发往外部服务的内容可能被缓存/索引，即使事后删除也不可完全回收。原因：对外输出前应假设内容将公开；涉及密钥/隐私先脱敏再发。
 
 ---
```

---

## 五、当前状态与待办

**已完成**：沉淀 2 条、规则修改 2 处、全局同步 3 文件、文档 3 份。

**待办（尚未决定/执行）**：
1. `.Rules` 仓库有 2 个未提交变更（compact-core.md、workflow.md），**尚未 git commit**
2. 版本号维持 **v3.4.0 未升级**（是否升 v3.4.1 待用户决定；若升级需同步各文件中的版本引用，包括三个全局配置文件中"摘自 compact-core.md v3.4.0"字样）
3. `.prompt` 沉淀若需纳入版本管理，同样未提交

**决策建议**：若用户要求继续，可执行：升版本号 v3.4.1 → 同步版本引用 → git commit（约定式提交，如 `feat(common): ABL 抓包实证补 2 条行为约束 + 测试盲区禁令`）。

---

## 六、参考路径

- ABL 项目：`D:\Code\Project\ABL-Agent-Benchmark-Lab`（docs/ 质量评审报告 + system-prompt-analysis.md）
- 规则库：`D:\Code\.Rules`（main.md 索引 / compact-core.md 精简核心 / common/core/）
- 提示词库：`D:\Code\.prompt`（CLAUDE.md 索引）
- 本次产物：`.Rules\docs\changelog\`（改动文档 + diff + 本交接文档）
