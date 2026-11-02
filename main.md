# AI 编码规则体系 (AI Coding Rules System)

> **版本**: v3.9.8
> **更新日期**: 2026-10-15
> **核心理念**: 规则即模板，模板即规则
> **AI 入口**: [`compact-core.md`](./compact-core.md) — 精简核心（始终加载约束 + 按需加载清单），适合放入项目 CLAUDE.md
> **路径基准**: [`ROOT.md`](./ROOT.md) — 仓库根 + 跨仓库路径 SSOT

---

## 概述

本规则体系是一个**可组合、可定制、全流程覆盖**的 AI 编码规范系统。

采用 **common/ + stacks/** 双库架构：

- **common/** — 通用库，所有技术栈皆可用的规则
- **stacks/** — 专用库，按技术栈分类的专用规则和配置

### 关联体系

| 仓库   | 路径                | 职责                |
| ---- | ----------------- | ----------------- |
| 编码规则 | `D:\Code\.Rules`  | 编码规范、开发流程、项目结构    |
| 提示词库 | `D:\Code\.prompt` | 提示词存储、解决方案沉淀、经验复用 |

**提示词库工作流（每次会话必须遵循）：**

```
会话前 ──→ 搜索 D:\Code\.prompt 中是否有相关提示词/解决方案
         │
         ├── 有 → 读取并整合到回复中
         │
         └── 无 → 正常进行对话
                        │
                        ↓
会话后 ──→ 评估并存储高质量内容到 D:\Code\.prompt
           更新索引表（CLAUDE.md）
```

### 核心公式

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│    基础规则  +  定制需求  =  项目定制规则        │
│                                                                 │
│    基础规则: 本规则体系，可直接使用                              │
│    定制需求: 项目特定配置 (YAML/Markdown)                        │
│    定制规则: AI 生成的项目专属规则                               │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 体系架构

```
.Rules/
├── main.md                          # 主入口 (本文件)
├── OrganizationAndUser.md           # 组织与开发者信息（包名/署名等身份来源）
├── README.md                        # 说明文档
├── USAGE-GUIDE.md                   # 使用指南
│
├── ERROR/                           # 错误存储与纠正系统
│   ├── README.md                    # 系统文档
│   ├── INDEX.md                     # 错误索引
│   └── entries/                     # 错误条目
│
├── common/                          # 通用库 (所有技术栈可用)
│   ├── core/
│   │   ├── principles.md            # 核心原则
│   │   ├── security.md              # 安全规范
│   │   ├── code-style.md            # 代码风格
│   │   └── workflow.md              # 开发流程
│   ├── structures/
│   │   └── project-structure-guide.md
│   ├── templates/
│   │   ├── custom-requirements.yaml
│   │   ├── custom-requirements-template.yaml
│   │   ├── project-rules-template.md
│   │   └── CLAUDE-template.md
│
├── archive/                          # 归档（NEVER 作为现行规则加载）
│   ├── ai-coding-rulesystem.md       # 旧版完整文档 v1.0.0
│   └── errors/                       # 旧错误知识库（冻结）
│
├── stacks/                          # 专用库 (按技术栈分类)
    ├── flutter/
    │   ├── presets/
    │   │   ├── flutter.yaml
    │   │   └── loop.yaml
    │   ├── flutter-china-mirrors.md
    │   └── errors/entries/
    ├── react/
    │   ├── presets/
    │   │   └── react.yaml
    │   └── errors/entries/
    ├── fastapi/
    │   ├── presets/
    │   │   └── fastapi.yaml
    │   └── errors/entries/
    ├── fullstack/
    │   ├── presets/
    │   │   └── fullstack.yaml
    │   └── examples/
    │       ├── mindcare-ai-rules.md
    │       └── mindcare-ai-custom-rules.md
    ├── python-ml/
    │   ├── presets/
    │   │   └── ocean-lstm.yaml
    │   └── errors/entries/
    └── minecraft-mod/
        ├── presets/
        │   └── minecraft-mod.yaml
        └── errors/entries/

├── workflow/                       # 项目工作流程样本库（提炼自各项目实战）
│   ├── README.md                   # 用途 + 提炼原则 + 索引
│   └── {项目名}/                   # 每项目一目录
│       └── 工作流程.md

关联仓库:
D:\Code\.prompt/                       # 提示词与解决方案存储库
├── CLAUDE.md                          # 工作流规则与索引
├── 前端/                              # 前端类提示词
├── 设计/                              # 设计类提示词
└── 解决方案/                           # 问题解决方案
```

---

## 一、规则层级结构

### 1.1 核心规则层 (Core Rules)

> 必选，所有项目必须遵守的基础规则

| 文件                          | 内容                      | 强制性 |
| --------------------------- | ----------------------- | --- |
| `common/core/principles.md` | SOLID、DRY、KISS、YAGNI 原则 + AI 协作原则 | 强制  |
| `common/core/security.md`   | OWASP Top 10 安全规范       | 强制  |
| `common/core/code-style.md` | 沟通规范 + 命名、注释、格式 + 降AI率（条件触发） | 推荐  |
| `common/core/workflow.md`   | 开发全流程规范 + AI 行为约束        | 推荐  |

### 1.2 项目结构层 (Structure Rules)

> 必选，定义项目目录结构

- 引用 `common/structures/project-structure-guide.md`
- 根据项目类型选择对应结构模板

### 1.3 错误资源库层 (Error Repository)

> 推荐，AI 自动进化的核心机制

- **位置**：`ERROR/` — 独立的错误存储与纠正系统
- **全量记录**：任何问题都存储（代码错误、AI 行为纠正、工作流问题等）
- **检索优先**：出现问题先检索已有记录，参考但不盲从
- **持续更新**：重复问题更新次数，过时方案标记 deprecated
- 7 种错误类型：code / config / dependency / environment / ai-behavior / workflow / knowledge
- 详见 `ERROR/README.md`

> 旧系统 `archive/errors/` + `stacks/{tech}/errors/` 已归档冻结（NEVER 加载），新错误统一存入 `ERROR/`

### 1.4 定制需求层 (Customization Layer)

> 可选，项目特定的定制配置

- 使用 `common/templates/custom-requirements.yaml` 定义需求
- 或使用 `stacks/{tech}/presets/` 下的预设配置快速开始

### 1.5 提示词知识库层 (Prompt Knowledge Base)

> 强烈推荐，跨会话的提示词与解决方案沉淀

- 位置：`D:\Code\.prompt`
- **会话前查找**：搜索已有提示词/解决方案，避免重复劳动
- **会话后存储**：沉淀高质量内容，持续积累经验
- 详见 `D:\Code\.prompt\CLAUDE.md`

---

## 二、使用方式

### 方式一：直接使用（默认模式）

将本规则文件提供给 AI：

```
请按照 D:\Code\.Rules\main.md 规则体系进行开发。
```

AI 将：

1. 遵守核心规则层所有规则
2. 按照开发流程层规范执行
3. 参考项目结构层创建目录
4. 使用默认配置

### 方式二：预设模式（快速定制）

选择一个预设配置：

```
请按照 D:\Code\.Rules\main.md 规则体系，使用 stacks/flutter/presets/flutter.yaml 预设，
为我的 Flutter 项目生成定制规则。
```

### 方式三：完全定制模式

提供完整的定制需求：

```
请基于 D:\Code\.Rules 规则体系和以下定制需求，生成项目定制规则：

[粘贴 custom-requirements.yaml 内容]
```

---

## 三、规则优先级

```
定制规则 > 预设配置 > 项目结构规则 > 核心规则 > 默认行为
提示词知识库(.prompt) 作为经验参考层，在所有阶段辅助决策
```

当规则冲突时，优先级高的规则生效。

---

## 四、全流程覆盖

### 4.1 开发生命周期

```
┌─────────────────────────────────────────────────────────────────┐
│                        开发全流程                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1.需求分析 ──→ 2.架构设计 ──→ 3.编码实现 ──→ 4.测试验证       │
│       │              │              │              │            │
│       ↓              ↓              ↓              ↓            │
│  ┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐         │
│  │ PRD文档 │   │ 设计文档 │   │ 代码产物 │   │ 测试报告 │         │
│  └─────────┘   └─────────┘   └─────────┘   └─────────┘         │
│                                                                 │
│  5.构建打包 ──→ 6.部署发布 ──→ 7.运维监控 ──→ 8.迭代优化       │
│       │              │              │              │            │
│       ↓              ↓              ↓              ↓            │
│  ┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐         │
│  │ 构建产物 │   │ 部署记录 │   │ 监控日志 │   │ 更新日志 │         │
│  └─────────┘   └─────────┘   └─────────┘   └─────────┘         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 4.2 各阶段规则引用

| 阶段   | 规则文件                                                      | 核心内容        |
| ---- | --------------------------------------------------------- | ----------- |
| 需求分析 | `common/core/workflow.md#需求分析`                            | 需求文档模板、验收标准 |
| 架构设计 | `common/core/workflow.md#架构设计`                            | 架构模式、技术选型   |
| 编码实现 | `common/core/code-style.md` + `common/core/principles.md` | 代码规范、设计原则   |
| 测试验证 | `common/core/workflow.md#测试规范`                            | 测试策略、覆盖率要求  |
| 构建打包 | `common/core/workflow.md#构建规范`                            | 构建流程、产物命名、版本推荐   |
| 部署发布 | `common/core/workflow.md#部署规范`                            | 部署检查、回滚方案   |
| 私有包发布 | `common/core/npm-private-publish.md`                        | GitHub Packages 双 Token + 8 步发布流程 |
| 运维监控 | `common/core/workflow.md#运维规范`                            | 日志规范、告警配置   |
| 迭代优化 | `common/core/workflow.md#迭代规范`                            | 更新日志、版本管理   |
| 信息持久化 | `common/core/workflow.md#信息持久化`                          | 联网搜索结果存储、MCP 信息归档 |
| 多智能体协作（雁阵/潮汐调度） | `common/core/multi-agent.md`                              | 潮汐雁阵 Tidal Echelon 规范：状态源铁律、文件所有权、交接协议 |

---

## 五、定制规则生成

### 5.1 生成指令格式

```
基于 [D:\Code\.Rules] 规则体系，为以下项目生成定制规则：

项目名称: [名称]
项目类型: [类型]
技术栈: [技术栈列表]
特殊需求: [需求列表]

请输出完整的项目定制规则文件。
```

### 5.2 定制规则输出格式

生成的定制规则应包含：

1. **项目信息** - 基本信息、技术栈
2. **目录结构** - 项目特定的目录结构
3. **代码规范** - 技术栈特定的代码规范
4. **API 规范** - 接口设计规范（如适用）
5. **构建规范** - 构建流程和产物命名
6. **环境配置** - 环境变量、配置文件
7. **工作流程** - 开发、测试、部署流程
8. **日志规范** - 更新日志、崩溃日志

---

## 六、快速参考

### 项目类型快速选择

| 项目类型       | 预设文件                                      | 推荐场景          |
| ---------- | ----------------------------------------- | ------------- |
| Flutter 应用 | `stacks/flutter/presets/flutter.yaml`     | 移动端跨平台应用      |
| React 应用   | `stacks/react/presets/react.yaml`         | Web 前端 SPA    |
| FastAPI 后端 | `stacks/fastapi/presets/fastapi.yaml`     | Python 后端 API |
| 全栈项目       | `stacks/fullstack/presets/fullstack.yaml` | 前后端一体化项目      |
| Minecraft Mod | `stacks/minecraft-mod/presets/minecraft-mod.yaml` | MC 模组开发 |

### 常用命令

```bash
# 生成定制规则
基于 D:\Code\.Rules 规则体系和 stacks/flutter/presets/flutter.yaml 生成定制规则

# 使用定制规则开发
请按照 [项目定制规则文件] 进行开发

# 查看规则优先级
规则优先级: 定制规则 > 预设 > 结构 > 核心 > 默认
```

---

## 七、规则文件索引

### AI 精简核心

- [精简核心 (Compact Core)](./compact-core.md) - **AI 始终加载的入口**。提取安全红线、代码底线、AI 行为约束为始终生效层；其余规则按触发条件懒加载。放入项目 CLAUDE.md 即可生效。

### 项目元信息

- [组织与开发者信息](./OrganizationAndUser.md) - 组织/开发者身份、包名前缀、版权署名的唯一事实来源
- [项目清单](./PROJECTLIST.md) - 全项目导航索引（路径、定位、活跃状态）
- [项目审计系统](./docs/audit/README.md) - 定期审计机制与待办池

### 通用库 (common/)

#### 核心规则

- [核心原则](./common/core/principles.md) - SOLID、DRY、KISS、YAGNI + AI 协作原则
- [安全规范](./common/core/security.md) - OWASP Top 10
- [代码风格](./common/core/code-style.md) - 沟通规范 + 命名、注释、格式 + 降AI率（条件触发）
- [开发流程](./common/core/workflow.md) - 全流程规范 + AI 行为约束 + 项目问题档案(.issues/) + 编译后版本推荐
- [npm 私有包发布](./common/core/npm-private-publish.md) - GitHub Packages 双 Token 策略 + 8 步发布流程 + release.js 模板 + 常见问题
- [Skill/Agent 注册中心](./common/core/skill-registry.md) - 三级加载策略（L1 白名单 / L2 项目注册 / L3 按需索引）+ 全量 skill 分类索引

#### 项目结构

- [项目结构指南](./common/structures/project-structure-guide.md) - 各类项目目录结构

#### 错误资源库

- [错误存储与纠正系统](./ERROR/README.md) - 系统文档、字段定义、存储策略
- [错误索引](./ERROR/INDEX.md) - 按类型/标签查找

#### 模板

- [定制需求模板](./common/templates/custom-requirements.yaml) - YAML 配置模板
- [定制需求模板(空)](./common/templates/custom-requirements-template.yaml) - 空白模板
- [项目规则模板](./common/templates/project-rules-template.md) - 输出模板
- [CLAUDE 模板](./common/templates/CLAUDE-template.md) - Claude Code 规则模板

#### 归档（NEVER 作为现行规则加载）

- [AI 编码规则体系 v1.0.0](./archive/ai-coding-rulesystem.md) - 旧版完整文档
- [旧错误知识库](./archive/errors/README.md) - 冻结

### 专用库 (stacks/)

#### Flutter

- [Flutter 预设](./stacks/flutter/presets/flutter.yaml)
- [Loop 预设](./stacks/flutter/presets/loop.yaml)
- [Flutter 国内镜像配置](./stacks/flutter/flutter-china-mirrors.md)

#### React

- [React 预设](./stacks/react/presets/react.yaml)

#### FastAPI

- [FastAPI 预设](./stacks/fastapi/presets/fastapi.yaml)

#### 全栈

- [全栈预设](./stacks/fullstack/presets/fullstack.yaml)
- [MindCare AI 规则示例](./stacks/fullstack/examples/mindcare-ai-rules.md)
- [MindCare AI 定制规则示例](./stacks/fullstack/examples/mindcare-ai-custom-rules.md)

#### Python-ML

- [Ocean LSTM 预设](./stacks/python-ml/presets/ocean-lstm.yaml)

#### Minecraft-Mod

- [Minecraft Mod 预设](./stacks/minecraft-mod/presets/minecraft-mod.yaml)

### 项目工作流程样本 (workflow/)

- [工作流程样本库](./workflow/README.md) - 各项目实战工作手册的提炼存档，与通用模板 `common/core/workflow.md` 互补
- [Jeenith 工作流程](./workflow/jeenith/工作流程.md) - Flutter 移动 App 实战样本（结构/构建/归档/发布/版本/文档/Git 全流程 + 可复用经验提炼）

### 关联仓库

- [提示词与解决方案库](D:\Code\.prompt\CLAUDE.md) - 提示词存储、解决方案沉淀、工作流规则

---

## 变更记录

| 版本     | 日期         | 变更内容                                                    |
| v3.9.8 | 2026-11-03 | multi-agent.md 新增 §十 拉起与通讯（无人值守：执行者两类拉起矩阵——CLI 进程直调/Electron 优先找等价 CLI 入口（桌面版↔codex exec、dsh desktop↔dsh headless），兜底守护脚本轮询与通知叫人；通讯文件总线 dispatch 任务包+回执、剪贴板桥过渡；心跳守护脚本实体化——无人值守场景必要（修正"人在盯"前提）；安全闸：无人值守可写限本项目、NEVER 自动执行不可逆操作、拉起必带受限权限）；§十补"使用偏好≠拉起方式"（日常有 Desktop 先用 Desktop、CC 例外偏好 CLI；无人值守拉起走 CLI 内核不冲突——一个大脑两扇门）+ 拉起模式定则最终版（2026-11-03 用户拍板：**全用心跳统一**——心跳为唯一默认 pull，直调/注入降为可选优化；实证：dsh headless 会话桌面版不实时出现且永远落"未分组"，分组表由桌面版 UI 维护 headless 不登记，GUI 围观不可依赖）+ CLI 常驻双向流实测入矩阵（stdin 持续派任务 stdout 实时出过程，TUI 注入 Windows 无干净通道）；档案 §6.3 登记操作入口偏好与协作编制（Codex 编排/DSH+CC 执行） |
| v3.9.7 | 2026-11-03 | multi-agent.md 新增 §九 主从编排（头雁制）与派遣记录（编制三型：接力/并行分工/主从编排；编排者拆任务+验收、NEVER 亲自执行，执行者遇超出任务包即回写阻塞 NEVER 扩权；派遣记录表格——时间戳命令实测到秒 NEVER 模型自填、派遣即登记、回执对称登记；任务包五件套：目标/验收标准/文件边界/并行声明/回传要求；已知弱点自防：编排者单点可接力/粒度失控/集成断裂）；§七 适用禁止与 §八 落地清单配套补充；四全局文件触发概述同步 |
| v3.9.6 | 2026-11-01 | §一 新增 25 禁止模糊时间词（时间锚定：禁用"早年/晚年/小时候/以前/最近/当年"等模糊时间表述，必须给具体时间点或范围——给不出精确给范围标"约"、真不知道写"时间不详"，NEVER 用模糊词掩盖或编造精确时间；原因：模糊时间无法定位/排序/回溯，时间线失真） |
| v3.9.5 | 2026-10-20 | §一 新增 24 意引补全与引语记录（意引=引其意不引其词；五机制：识别信号——文白夹杂/对仗/异常凝练/经典母题/化用典故，凡有公认原文出处的固定表达皆算、口语 NEVER 硬凑；补全原句+出处、拿不准 NEVER 编造；映射记入 OrganizationAndUser §6.7；反向对齐——AI 引用优先库内句、新引用入库；温和校正——只纠影响理解的字词偏差）；§6.7 建库+识别校准样例（构造样例，粒度参照）；识别信号审计（对照 trigger-mechanism §一"必须可判定"标准）：§一.15 收尾信号例、§一.18a 现象边界、§〇 画像新信息信号、trigger-mechanism 三个沉淀行为触发条件改可判定+意引行为注册入表；修正 §一 条目顺序（21/22/23 错位）；§6.6 待确认三项落实；compact-core 签名补正 |
| v3.9.4 | 2026-10-15 | §一 新增 23 禁止行业黑话（说人话：面向用户汇报大白话，术语少量+首次附大白话解释；验收汇报模板：改了什么/查了什么/没查什么——源自 AI 口癖现象视频） |
| v3.9.3 | 2026-10-06 | §一 新增 22 禁止报喜不报忧（信息全面性义务：全量信息一个不少，NEVER 隐瞒淡化坏消息/夸大好消息/因怕担责而省略已知信息——残缺汇报制造虚假确定感） |
| v3.9.2 | 2026-10-03 | §一 新增 21 组件设计可拓展结构化、NEVER 重复堆砌（数据驱动，加同类项只改配置；便于阅读与维护） |
| v3.9.1 | 2026-10-02 | §一 新增 20 禁止逃避问题（NEVER 静默降级方向：方向由目标决定而非实现难度；难走只能明示换方案由用户拍板，不得私自绕行） |
| v3.9.0 | 2026-09-28 | §二 安全红线新增"个人衍生信息本地计算优先"（八字等可推导数据本地公式算，NEVER 上传外部网站换算）；配套沉淀 .prompt 解决方案 八字四柱本地计算（完整公式+五虎遁五鼠遁+神煞判定表，以王喆排盘为例：丙戌/己亥/丙寅/壬辰，华盖成立、十灵日/太极贵人证伪）；OrganizationAndUser 6.0 更新四柱与神煞 |
| v3.8.0 | 2026-09-27 | 用户画像持续蒸馏：compact-core §〇 蒸馏对象第 1 条新增持续蒸馏义务（档案保持最新，发现画像新信息即时更新 SSOT）；OrganizationAndUser §六 重构——新增 6.0 个人画像属性（王喆/2006-12-03 辰时/射手座/性格实证/MBTI 待补）、6.1 项目地图全量更新（git remote 实测：1010523654 疑似小号待确认、克隆参考项目单列、workbench 伴生补充）、6.5 荣誉与竞赛（技能大赛国赛/数学建模参赛，获奖待补）；三全局文件同步 |
| v3.7.0 | 2026-09-14 | 新增多智能体协作规范 `common/core/multi-agent.md`（潮汐雁阵 Tidal Echelon：蜂巢/雁阵谱系、潮汐调度费率错峰、状态源铁律 HANDOFF-CURRENT、文件所有权登记、原型/已实现/已验证三态、交接三清单）；compact-core §四新增分发入口行；首发于 Trace 项目双智能体实践（Claude Code + Codex） |
| ------ | ---------- | ------------------------------------------------------- |
| v3.6.4 | 2026-09-07 | issues/ERROR 沉淀范围扩展：新增显隐性分级——隐性错误（静态检查/测试全过但实际有问题，需人辅助发现）必须收录且价值最高（工具盲区=AI 复犯区）；ERROR/README 新增分级定义与触发条款，workflow §9.4 触发条件同步 |
| v3.6.3 | 2026-09-07 | §一 新增 19 思考与回答用中文（思维链是用户校准理解的窗口，中文才能对比、及时止损） |
| v3.6.2 | 2026-09-07 | §一 新增 18a 用户报现象必查地基（Trace 会话回溯："这些只是现象，透露出的是地基没搭好"——报现象要向上修到根） |
| v3.6.1 | 2026-08-16 | §一 新增 18 透过现象看本质·验收看三层（测试全绿是基础，逻辑没问题才能优化体验，最终站在用户视角）——用户原话蒸馏 |
| v3.6.0 | 2026-08-16 | 新增 §〇 蒸馏规则（最高优先级，仅次于 agent 系统提示词）：会话前读用户档案、会话中即时记录、会话后追加第 5 类习惯蒸馏判定；§一 新增 16 永远积极不偏不倚长远思考 / 17 临时埋的雷必须及时处理干净；OrganizationAndUser 新增 §六 用户数字档案（项目地图 5 来源 + AI 经历时间线 + 工具切换）；三个全局配置文件头部注入蒸馏规则 |
| v3.5.1 | 2026-08-16 | workflow §4.1 新增测试防线分层（单测/冒烟不可延后+人肉验收可延后+回归爆炸半径禁令）；workflow §12.1 新增改动文档批量制约定（5~10 版一份，区间命名）；compact-core §二 新增配置脱敏红线（KEY/TOKEN/SECRET 值先替换占位符）；配套沉淀 .prompt 通用 配置脱敏模板 |
| v3.5.0 | 2026-08-13 | compact-core §一 新增 13-15 条（被拒不重试 / 发送内容即发布 / 会话收尾自检）+ §四分发入口约定（唯一分发入口 + 版本递增=重读信号）；code-style 新增 §九隐藏AI痕迹（代码外伪装，条件触发）；workflow §4.1 测试盲区禁令（序列化/转义类必须断言完整格式）；ERROR 新增边界判定准则（ERROR 库 vs .issues 双写）与 ai-behavior 判例补充 |
| v3.4.0 | 2026-08-04 | 新增 AI 行为约束两条（§一 11 两手准备兜底必达 / 12 PowerShell 一律用 pwsh）；principles 新增 §6.10 两手准备规则；workflow §3.4 工具使用新增 pwsh 源定义；OrganizationAndUser 新增服务器与域名章节（腾讯云 62.234.115.217 + heys-snowe.tj.cn）、口号新增「未雨绸缪，兜底必达」 |
| v3.3.0 | 2026-08-03 | 新增可复用触发机制（`common/core/trigger-mechanism.md`）；workflow 新增改动文档生成（§12）；项目问题档案改由触发机制驱动（§9.4）；旧文档归档至 `archive/`；入口文档（README/CLAUDE/AGENTS）精简去重 |
| v3.2.2 | 2026-07-10 | 新增项目级问题档案(.issues/)、编译后版本号推荐、组织身份SSOT(OrganizationAndUser.md)、项目清单(PROJECTLIST.md)、项目审计系统(docs/audit，workflow §9.5) |
| v3.2.1 | 2026-05-11 | 新增"降AI率"规则（code-style.md 第八节），仅用户明确触发时生效 |
| v3.2.0 | 2026-04-25 | 新增错误存储与纠正系统 (ERROR/)，覆盖全量问题，支持检索优先和 AI 行为纠正 |
| v3.1.1 | 2026-04-21 | 新增信息持久化规则：联网搜索/MCP 工具获取的信息自动存储到 information/ |
| v3.1.0 | 2026-04-02 | 引入 AI 协作原则（7 条核心行为规范），改进模板引用方式 |
| v3.0.0 | 2026-03-28 | **BREAKING CHANGE** 重组为 common/ + stacks/ 双库架构，所有文件路径变更 |
| v2.2.0 | 2026-03-28 | 新增提示词知识库层，关联 D:\Code\.prompt 仓库                         |
| v2.1.1 | 2026-03-17 | 错误知识库改为无感存储，降低记录门槛                                      |
| v2.1.0 | 2026-03-17 | 新增错误知识库层 (errors/)，支持主动记录错误                             |
| v2.0.0 | 2026-03-12 | 重构为分层架构，支持全流程覆盖                                         |
| v1.0.0 | 2026-03-12 | 初始版本                                                    |

---

*本规则体系遵循"规则即模板，模板即规则"的设计理念*
*可直接使用，也可通过定制需求生成项目特定规则*
