# Skill / Agent 注册中心（三级加载策略）

> 定位：全量 skill 与 agent 的分类索引 + 适用范围 + 触发条件。新项目/新会话先查本文件，按需决定加载哪些 skill。
> 更新：2026-08-07（工具适配维度）

## 〇、工具适配（本机实际环境）

**Skill 单一真实存储**：`C:\Users\aaa\.cc-switch\skills`（158 个有效 skill，每目录一个；`~/.deepcode/skills` 是指向它的 symlink）。cc-switch 统一管理，以下工具均可调用：cc cli、cc desktop、codex、gemini、grok、opencode、hermes、deepcode、claude code。

| 工具 | 用户级全局规则文件 | skills 接入 | agents 接入 |
|------|-------------------|-------------|-------------|
| deepcode | `~/.deepcode/AGENTS.md` | `~/.deepcode/skills`（symlink→cc-switch） | 无单独目录（skill 触发） |
| claude code | `~/.claude/CLAUDE.md` | `~/.claude/skills`（可指向 cc-switch） | `~/.claude/agents`（junction→`.AI-TOOLS\.agent`） |
| codex | `~/.codex/AGENTS.md` | `~/.codex/skills`（可指向 cc-switch） | `~/.codex/agents` |
| gemini / grok / opencode / hermes / cc cli / cc desktop | 各自平台规则 | 由 cc-switch 分发 | 平台自有 |

> 三级加载策略以本文件为唯一事实来源；各工具用户级规则文件只需引用本文件（见 §六），不必复制索引内容。

## 一、三级加载策略（核心）

```
L1 全局白名单  —— 日常高频 skill，name+description 常驻上下文（控制 10-20 个）
L2 项目注册    —— 项目 CLAUDE.md/AGENTS.md 声明 + 项目级 settings.json enabledSkills 启用
L3 按需索引    —— 其余全部：任务匹配本文件触发条件时，用 Read 加载对应 skill
```

| 层级 | 机制 | 上下文成本 | 说明 |
|------|------|-----------|------|
| L1 全局 | 工具自动扫描常驻 | ~15 个 × 150 tokens | 每个会话都在用的核心 skill |
| L2 项目 | CLAUDE.md/AGENTS.md 注册 + `enabledSkills` | 仅注册的 | 项目相关但非全局通用 |
| L3 按需 | 本文件索引 + 触发条件 | 触发时加载 | 长尾 skill，绝不常驻 |

**原则：**

- skill 的 name+description 是触发机制——完全不加载 = 该触发时不触发（undertrigger）。L1 白名单保留高频触发能力，L2/L3 靠注册与索引
- agent（`D:\Code\.AI-TOOLS\.agent\` 约 13 个 .md）**本来就是按需读取**（任务匹配触发场景才读），不占常驻上下文，无需降级
- 新装 skill 后：登记到本文件分类索引；判断是否高频 → 决定进 L1 白名单还是 L2/L3

## 二、L1 全局白名单（日常高频，常驻）

> 可调整：使用习惯变化时增删。目标是 10-20 个。

| Skill | 触发场景 |
|-------|----------|
| verification-before-completion | 宣称完成/修复/通过前，必须有验证证据 |
| systematic-debugging / diagnose | 任何 bug、测试失败、意外行为 |
| tdd / test-driven-development | 测试优先开发、red-green-refactor |
| brainstorming | 任何新功能/组件/行为设计前的需求探索 |
| using-superpowers / using-git-worktrees | 工作流引导、隔离工作区 |
| create-skill / skill-digester / find-skills | skill 创建/优化/检索 |
| deepcode-self-refer | 关于 Deep Code 自身的问题 |
| kb-retriever | 知识库目录检索问答 |
| lark-base / lark-sheets | 飞书多维表格 / 电子表格 |
| xlsx | 表格文件处理（.xlsx/.csv） |
| writing-plans / executing-plans | 计划编写 / 计划执行 |

## 三、分类索引（L2 项目注册 / L3 按需触发）

> 触发条件命中时：项目级优先（CLAUDE.md 已注册），否则按本表 Read 加载对应 skill 目录的 SKILL.md。

### 3.1 验证 / 质量 / 审查

| Skill | 触发条件 |
|-------|----------|
| requesting-code-review | 完成任务后请求代码审查 |
| receiving-code-review | 收到审查反馈后、实施前 |
| code-review | 代码审查（mattpocock） |
| webapp-testing | Web 应用测试 |
| babysit | 维护 PR 可合并状态 |

### 3.2 工程方法论（mattpocock / superpowers）

| Skill | 触发条件 |
|-------|----------|
| codebase-design / domain-modeling | 代码库设计、领域建模 |
| diagnosing-bugs / to-tickets / to-spec | 排障、转工单、转规格 |
| implement / wayfinder / wizard | 实施、代码导航、向导式 |
| resolving-merge-conflicts | 解决合并冲突 |
| to-issues / to-prd / triage | 计划拆 issue、写 PRD、工单分类 |
| split-to-prs | 拆分工作为小 PR |
| subagent-driven-development | 子代理驱动开发 |
| writing-for-agents | 写面向代理的文档/规则 |
| grill-me / grilling / wait-what / teach | 追问打磨、教学 |
| setup-matt-pocock-skills | 配置 Matt Pocock 技能 |

### 3.3 前端 / 设计 / 动效

| Skill | 触发条件 |
|-------|----------|
| gsap-core/timeline/scrolltrigger/plugins/utils/react/performance/frameworks | GSAP 动画（8 个） |
| animate / animation-vocabulary / improve-animations / review-animations / find-animation-opportunities / pick-ui-library | 动画设计（emil 6 个） |
| apple-design / emil-design-eng | 设计语言（emil） |
| frontend-design / web-design-engineer / web-design-guidelines / high-end-visual-design / minimalist-ui / ui-ux-pro-max / design-taste-frontend | 前端/UI 设计 |
| industrial-brutalist-ui / theme-factory / stitch-design-taste / redesign-existing-projects | 风格/主题/重构 |
| web-artifacts-builder / image-to-code / imagegen-frontend-web / imagegen-frontend-mobile / gpt-image-2 | 图片生成/代码还原 |
| canvas / canvas-design / algorithmic-art / brandkit / brand-guidelines / gpt-taste | 视觉/品牌 |
| website-to-design-md | 网页转 DESIGN.md 设计系统 |

### 3.4 WebGL / 3D / 动画实现

| Skill | 触发条件 |
|-------|----------|
| animate | 网页动画实现（emil） |
| remotion-best-practices | Remotion 视频 |
| web-video-presentation / slack-gif-creator | 视频/动图 |
| 拆解参考 | `libs\effects\`（流体/引力透镜/黑洞）+ `.prompt\前端\WebGL*` 提示词 |

### 3.5 文档 / 写作 / 办公

| Skill | 触发条件 |
|-------|----------|
| docx / pptx / pdf | 办公文档处理 |
| doc-coauthoring / internal-comms / editing | 文档协作、内部沟通、编辑 |
| 写作三件套（in-progress，未装） | writing-beats/fragments/shape |
| csharp / shell / sdk / mcp-builder / create-hook / create-subagent / create-rule | 语言/工具/构建 |

### 3.6 AI 工具 / 平台（vercel / trae / claude）

| Skill | 触发条件 |
|-------|----------|
| vercel-*（6 个：cli-with-tokens/optimize/react-best-practices/composition-patterns/react-native-skills/react-view-transitions） | Vercel 相关 |
| deploy-to-vercel / vercel-cli-with-tokens | 部署 Vercel |
| TRAE-code-review / TRAE-debugger / TRAE-security-review / TRAE-generate-mini-app | Trae 平台 |
| claude-api / sdk / update-cli-config / update-cursor-settings / statusline | Claude/CLI 配置 |
| migrate-to-skills / migrate-to-shoehorn / setup-pre-commit / scaffold-exercises / full-output-enforcement | 迁移/脚手架 |
| handoff / loop-me（未装） | 交接/追问 |

### 3.7 飞书系列（lark-*，27 个）

| Skill | 触发条件 |
|-------|----------|
| lark-base / lark-sheets | 多维表格 / 电子表格 |
| lark-doc / lark-wiki / lark-note / lark-drive / lark-okr | 文档/知识库/云盘 |
| lark-im / lark-mail / lark-task / lark-calendar / lark-contact / lark-approval / lark-attendance / lark-minutes / lark-vc / lark-vc-agent / lark-whiteboard / lark-slides / lark-markdown / lark-event / lark-apps / lark-openapi-explorer / lark-skill-maker / lark-workflow-meeting-summary / lark-workflow-standup-report / lark-shared | 飞书各应用 |

> 依赖 `lark-cli` 二进制；认证走 `lark-shared`（先 `lark-cli auth login`）

### 3.8 其他 / 归档

| Skill | 触发条件 |
|-------|----------|
| beautiful-article / kb-retriever | 文章美化 / 知识库 |
| prototype / template-skill / zoom-out | 原型 / 模板 / 全局视角 |
| univer-cli | Univer 表格 |
| 其他长尾 | 按 name 查 SKILL.md 的 description 判断 |

## 四、项目注册示例（L2）

项目 `CLAUDE.md` / `AGENTS.md` 中声明：

```markdown
## Skill 注册（按需加载）
本项目启用以下 skill（其余禁用，见 .deepcode/settings.json）：
- gsap-scrolltrigger、animation-vocabulary、improve-animations  # 动效相关
- code-review、tdd、verification-before-completion              # 工程质量
- lark-sheets                                                   # 飞书表格
```

项目 `.deepcode/settings.json`：

```json
{
  "enabledSkills": {
    "gsap-scrolltrigger": true,
    "animation-vocabulary": true,
    "improve-animations": true,
    "code-review": true,
    "tdd": true,
    "verification-before-completion": true,
    "lark-sheets": true
  }
}
```

> `enabledSkills["<name>"] === false` 表示禁用该 skill（不加载其 description）。未列出的按 L1/L3 处理。

## 五、维护规则

1. 安装新 skill 后：登记到本文件对应分类，判断是否高频（→L1）或项目相关（→L2）
2. 卸载 skill：从本文件移除
3. 本文件按需加载：任务涉及 skill 选择/加载策略时读取；不常驻
4. 新增分类时保持"触发条件"列可检索（写具体场景/关键词）

## 六、各工具用户级规则引用（推荐写法）

三个工具的用户级全局规则文件只需引用本文件，不复制索引内容：

```markdown
## Skills（三级加载）
- 真实存储：`C:\Users\aaa\.cc-switch\skills`（158 个有效 skill，本工具自动扫描/由 cc-switch 分发）
- 加载策略与全量索引：`D:\Code\.Rules\common\core\skill-registry.md`（L1 全局白名单 / L2 项目注册 / L3 按需索引）
- 新装 skill 后必须登记到 skill-registry.md 分类索引
```

- deepcode → 写入 `~/.deepcode/AGENTS.md`
- claude code → 写入 `~/.claude/CLAUDE.md`
- codex → 写入 `~/.codex/AGENTS.md`
