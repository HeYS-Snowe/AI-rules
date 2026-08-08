# AI 编码规则 — 精简核心 (Compact Core)

> **本文件始终加载。** §一~三 为始终生效的约束；§四为按需加载清单。
>
> **设计原理**：规则分为「约束层」和「知识层」。
> 约束层（§一~三）不可懒加载——安全红线和代码底线必须在触发时已生效。
> **知识层（§四）按需加载**——AI 判断任务匹配触发条件后，用 Read 工具加载对应章节。
> **路径基准**：仓库根 `D:\Code\.Rules`，跨仓库路径见 `ROOT.md`

---

## 一、AI 行为约束（始终生效）

> 元规则——"AI 怎么干活"，不可降级。完整版含 Why 和实践要点 → `principles.md` §6。

1. **不画蛇添足** — NEVER 添加未被明确要求的东西。NEVER 为不可能的场景做预防。宁可有点重复也不要过早抽象。
2. **先看再改** — NEVER 在未读取文件内容的情况下修改文件。修改前先理解上下文和上下游影响。
3. **如实汇报** — 准确 > 简洁 > 详细。NEVER 说"看起来应该没问题"——要说"已通过 xxx 验证"。做好了不加免责声明，出错了报告具体情况不回避。
4. **不知道就说不** — NEVER 编造不确定的信息。不确定的技术细节先查阅再回答。NEVER 猜测命令参数、API 返回值、文件内容。
5. **禁止自行假设** — 信息不足时向用户提问，NEVER 脑补缺失信息。一次性提出所有问题，给选项优于开放式提问。
6. **自我审查** — 完成后以"找问题"的心态验证产出。"看起来没问题" ≠ "验证过"。运行测试而非仅做代码审查。
7. **主动反驳** — 发现用户方案有隐患或更优解时主动提出，附具体理由。用户坚持原方案时执行用户决定，不纠缠。
8. **禁令优于指令** — 编写规则时优先写"禁止做什么"而非"应该怎么做"，每条禁令附原因。
9. **思考不能外包** — 推理和决策必须自己做。子任务只负责执行，使用其结论前先自行评估合理性。
10. **主动生成改动文档** — 以下任一情况发生时，主动询问用户是否生成改动文档：(1) 版本号迭代时（修改 version 字段，或 commit message 含版本号）；(2) 本次会话已执行 3+ 次 git commit；(3) 用户表示完成一批工作或切换话题时（如"先这样吧""换个事"）。用户主动要求时立即生成。格式见 `workflow.md` §12。
11. **两手准备，兜底必达** — 做事情要做两手准备（Plan A 与 Plan B 与…），第二个准备一定要保证可用。NEVER 只给唯一解；涉及部署/发布/迁移等不可逆操作时先确认回滚方案就绪。详见 `principles.md` §6.10。
12. **PowerShell 一律用 pwsh** — Windows 下执行 PowerShell 命令一律用 `pwsh`（PowerShell 7.6.4，首选），NEVER 用 Windows 自带 `powershell`（5.1 旧版，仅作备用）。原因：5.1 默认 GBK 编码，中文注释/脚本易乱码。详见 `workflow.md` §3.4。

---

## 二、安全红线（始终生效）

> 违反会导致数据泄露或系统被攻破——不可懒加载。完整 OWASP Top 10 → 按需加载 `security.md`。

- **NEVER 提交密钥/Token/密码到 Git** — 使用 `.env`（不入库）+ `.env.example`（入库）
- **NEVER 字符串拼接 SQL** — 必须参数化查询
- **所有外部输入不可信** — 必须验证、清理、转义
- **密码必须加密存储** — bcrypt / argon2，NEVER 明文或可逆加密
- **NEVER 在日志/错误信息中输出敏感数据** — 脱敏后记录
- **默认拒绝** — 访问控制默认拒绝，显式授权

---

## 三、代码风格底线（始终生效）

> 适用于每一行代码。完整命名对照表、格式规范、语言规则 → 按需加载 `code-style.md`。

- **NEVER 使用 emoji** — 代码、注释、commit message、文档中均禁止
- **命名有意义** — 名字说明意图，避免无意义缩写
- **早返回（Guard Clause）** — 先处理异常/边界再写主逻辑，减少嵌套
- **避免魔法数字** — 用命名常量替代裸数字
- **Commit Message** — 约定式提交 `<type>(<scope>): <subject>`，禁止 emoji 前缀
- **沟通** — 先结论后理由；能一句话说完 NEVER 用三句

---

## 四、按需加载规则清单

> **AI 使用说明**：以下规则不预加载。当任务明确匹配"触发条件"时，用 Read 工具加载对应文件的对应章节，然后遵循该规则。
> **加载后产出**：遵循该规则执行；若规则含模板/清单则按模板产出对应文档或检查项（PRD、测试报告、部署清单等）。产出完成后即止，NEVER 过度引用。
> 判断原则：**明确涉及才加载；不确定时先查本清单；NEVER 预加载"可能用到"的规则。**

### 设计原则

| 触发条件 | 加载 | 内容概述 |
|----------|------|----------|
| 架构设计、模块划分、代码质量决策 | `common/core/principles.md` §1-5 | SOLID / DRY / KISS / YAGNI 详解 + 模块化 + 可维护性原则 |

### 安全规范

| 触发条件 | 加载 | 内容概述 |
|----------|------|----------|
| 涉及认证、用户输入、API 设计、防注入 | `common/core/security.md` §2 | OWASP Top 10 逐条防护 + 代码示例 |
| Flutter / 移动端开发 | `common/core/security.md` §4 | 本地存储安全、网络安全、代码安全 |
| 后端 / 服务端开发 | `common/core/security.md` §3,§5 | API 安全、环境变量管理、数据库安全 |
| 安全审计 / 上线前检查 | `common/core/security.md` §6-7 | 分阶段检查清单 + 安全事件处理 |

### 代码风格

| 触发条件 | 加载 | 内容概述 |
|----------|------|----------|
| 写 Dart / Flutter 代码 | `common/core/code-style.md` §5.1 | Dart 特定规范 |
| 写 TypeScript 代码 | `common/core/code-style.md` §5.2 | TypeScript 特定规范 |
| 写 Python 代码 | `common/core/code-style.md` §5.3 | Python 特定规范 |
| 用户明确要求"降AI率" | `common/core/code-style.md` §8 | 降AI率手法（仅条件触发） |
| 代码审查 | `common/core/code-style.md` §7 | 审查检查清单（格式/命名/注释/结构） |
| 需要命名或格式对照 | `common/core/code-style.md` §1-4 | 命名对照表、格式规范、注释规范、代码组织 |

### 开发流程

| 触发条件 | 加载 | 内容概述 |
|----------|------|----------|
| 需求分析、写 PRD | `common/core/workflow.md` §1 | 需求分析 + PRD 文档模板 |
| 技术设计、架构文档 | `common/core/workflow.md` §2 | 设计层次 + 技术设计文档模板 |
| 编码实现、提交规范 | `common/core/workflow.md` §3 | 开发流程 + 提交规范 + AI 行为约束 + 代码审查清单 |
| 写测试、测试策略 | `common/core/workflow.md` §4 | 测试层次 + AAA 模式 + 测试报告模板 |
| 构建 / 打包 | `common/core/workflow.md` §5 | 构建流程 + 产物命名 + **builds 目录结构（release/packages 分离，§5.1b）** + 编译后版本号推荐 |
| 部署发布 | `common/core/workflow.md` §6 | 部署检查清单 + 回滚方案 |
| npm 私有包发布（GitHub Packages） | `common/core/npm-private-publish.md` | 双 Token 策略 + 8 步发布流程 + release.js 模板 + 常见问题 |
| 运维监控、日志规范 | `common/core/workflow.md` §7 | 日志规范 + 更新日志模板 + 监控告警 |
| 版本管理、Changelog | `common/core/workflow.md` §8 | 版本管理 + 变更记录模板 |
| 联网搜索/MCP 获取信息需持久化 | `common/core/workflow.md` §10 | 信息持久化规则 |
| 生成改动文档 / 版本迭代总结 | `common/core/workflow.md` §12 | 改动文档格式（Bug 修复 + 功能更新模板） |

### 错误与问题

| 触发条件 | 加载 | 内容概述 |
|----------|------|----------|
| 遇到错误/问题 | 先搜 `ERROR/INDEX.md` → 对应条目 | 检索已有错误记录，参考但不盲从 |
| 记录新错误 | `ERROR/README.md` | 7 种错误类型、存储格式、字段定义 |
| 项目级 bug 归档 | 项目根 `.issues/`（详见 `workflow.md` §9.4） | 问题档案格式 |
| 项目 bug 记录 / 其他自动落盘类行为 | `common/core/trigger-mechanism.md` | 可复用触发机制：注册表 + 初始化 + 位置解析 |

### 项目结构与模板

| 触发条件 | 加载 | 内容概述 |
|----------|------|----------|
| 创建新项目 / 规划目录结构 | `common/structures/project-structure-guide.md` | 各类项目目录结构模板 |
| 生成项目定制规则 | `common/templates/custom-requirements.yaml` | 定制需求 YAML 模板 |
| 创建项目 CLAUDE.md | `common/templates/CLAUDE-template.md` | Claude Code 规则模板 |

### 技术栈预设

| 触发条件 | 加载 | 内容概述 |
|----------|------|----------|
| Flutter 项目 | `stacks/flutter/presets/flutter.yaml` | Flutter 预设配置 |
| React 项目 | `stacks/react/presets/react.yaml` | React 预设配置 |
| FastAPI 项目 | `stacks/fastapi/presets/fastapi.yaml` | FastAPI 预设配置 |
| 全栈项目 | `stacks/fullstack/presets/fullstack.yaml` | 全栈预设配置 |
| Minecraft Mod | `stacks/minecraft-mod/presets/minecraft-mod.yaml` | MC 模组预设配置 |
| Flutter 国内网络问题 | `stacks/flutter/flutter-china-mirrors.md` | 国内镜像配置 |

### Skill / Agent 加载

| 触发条件 | 加载 | 内容概述 |
|----------|------|----------|
| 选择/加载 skill、判断 skill 适用范围、新装 skill 登记 | `common/core/skill-registry.md` | 三级加载策略（L1 全局白名单 / L2 项目注册 / L3 按需索引）+ 全量 skill 分类索引 + 项目注册示例 |
| skill/agent 加载机制优化 | `common/core/skill-registry.md` §一 | L1 常驻 10-20 个高频；L2 项目 CLAUDE.md+enabledSkills；L3 索引触发；agent 本已按需 |

### 规则文件最后更新时间

> AI 判断文件是否需要重新读取的依据：引用的规则来自某文件且其更新日期晚于本会话开始时间时，重新加载对应章节。**修改核心规则文件后必须更新此表。**

| 文件 | 最后更新 |
|------|----------|
| `common/core/workflow.md` | 2026-08-07 |
| `common/core/skill-registry.md` | 2026-08-07 |
| `common/core/trigger-mechanism.md` | 2026-08-03 |
| `ERROR/README.md` | 2026-08-03 |
| `compact-core.md`（本文件） | 2026-08-07 |
| `common/core/code-style.md` | 2026-05-11 |
| `common/templates/custom-requirements.yaml` | 2026-05-07 |
| `stacks/flutter/presets/flutter.yaml` | 2026-05-07 |
| `common/core/principles.md` | 2026-08-04 |
| `stacks/minecraft-mod/presets/minecraft-mod.yaml` | 2026-04-28 |
| `stacks/python-ml/presets/ocean-lstm.yaml` | 2026-04-08 |
| `common/core/security.md` | 2026-03-28 |
| `common/structures/project-structure-guide.md` | 2026-03-28 |
| `stacks/react/presets/react.yaml` | 2026-03-12 |
| `stacks/fastapi/presets/fastapi.yaml` | 2026-03-12 |
| `stacks/fullstack/presets/fullstack.yaml` | 2026-03-12 |

---

## 与 main.md 的关系

| 文件 | 定位 | 适合 |
|------|------|------|
| `main.md` | 完整规则索引 + 使用指南 | 人类阅读、项目初始化 |
| `compact-core.md`（本文件） | AI 始终加载的精简核心 | 放入项目 CLAUDE.md |

**规则优先级**：定制规则 > 预设配置 > 项目结构规则 > 核心规则 > 默认行为

---

*v3.4.0 · 精简核心版 · 更新于 2026-08-07 · 完整规则见 main.md*
