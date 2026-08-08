# AI 规则体系使用指南

> 在 Claude Code 和 Trae 编辑器中使用本规则体系

---

## 一、Claude Code 使用方式

### 方式 1：CLAUDE.md 文件（推荐）

在项目根目录创建 `CLAUDE.md` 文件：

```markdown
# 项目规则

## 规则引用

请遵循 AI 编码规则体系进行开发：
- 规则位置: D:\Code\.Rules\main.md

## 项目信息

- 项目名称: {你的项目名}
- 项目类型: {类型}
- 技术栈: {技术栈}

## 特殊需求

- {需求1}
- {需求2}
```

### 方式 2：直接对话引用

```
请按照以下规则体系进行开发：

@D:\Code\.Rules\main.md

项目信息：
- 项目名称: MyProject
- 技术栈: Flutter + Riverpod
```

### 方式 3：生成项目定制规则

**步骤 1**：复制模板

```bash
cp "D:\Code\.Rules\common\templates\custom-requirements.yaml" "./my-project-requirements.yaml"
```

**步骤 2**：填写配置

```yaml
project:
  name: "MyProject"
  type: "mobile"

tech:
  frontend:
    framework: "flutter"
    state: "riverpod"
```

**步骤 3**：生成规则

```
基于 AI-rules 规则体系 (D:\Code\.Rules\main.md)
和以下配置，生成项目定制规则：

[粘贴 my-project-requirements.yaml 内容]

请将生成的规则保存到项目的 CLAUDE.md 文件。
```

### 方式 4：使用预设快速开始

```
请基于 AI-rules 规则体系和 stacks/flutter/presets/flutter.yaml 预设，
为我的 Flutter 项目生成定制规则，保存为 CLAUDE.md
```

---

## 二、Trae 编辑器使用方式

### 方式 1：项目规则文件

在项目目录创建规则文件：

```
{项目目录}/.trae/rules/project_rules.md
```

内容示例：

```markdown
# 项目规则 - MyProject

## 规则引用

遵循 AI 编码规则体系: D:\Code\.Rules\main.md

## 项目配置

项目名称: MyProject
技术栈: Flutter + Riverpod

## 定制规则

[AI 生成的定制规则内容]
```

### 方式 2：全局规则

将通用规则放到全局位置：

```
D:\Trae-Tools\rules\personal_rules.md
```

添加引用：

```markdown
# 个人规则

## 引用外部规则体系

请同时遵循 AI 编码规则体系:
- D:\Code\.Rules\main.md

## 项目特定规则

[其他内容]
```

### 方式 3：生成项目定制规则

**步骤 1**：准备定制需求

```yaml
# 保存为 .trae/custom-requirements.yaml
project:
  name: "MyProject"
  type: "mobile"

tech:
  frontend:
    framework: "flutter"
```

**步骤 2**：请求生成

```
基于 AI-rules 规则体系和 .trae/custom-requirements.yaml，
生成项目定制规则并保存到 .trae/rules/project_rules.md
```

---

## 三、Cursor IDE 使用方式

### 方式 1：项目规则文件（推荐）

在项目根目录创建 `.cursor/rules/` 目录，添加规则文件：

```
{项目目录}/.cursor/rules/project-rules.mdc
```

内容示例：

```markdown
---
description: 项目编码规则
globs:
alwaysApply: true
---

# 项目规则 - MyProject

## 规则引用

遵循 AI 编码规则体系: D:\Code\.Rules\main.md

## 项目配置

项目名称: MyProject
技术栈: Flutter + Riverpod

## 定制规则

[AI 生成的定制规则内容]
```

### 方式 2：全局规则

将通用规则放到全局位置：

```
~/.cursor/rules/ai-coding-rules.mdc
```

添加引用：

```markdown
---
description: AI 编码规则体系
globs:
alwaysApply: true
---

# 全局规则

## 引用外部规则体系

请同时遵循 AI 编码规则体系:
- D:\Code\.Rules\main.md
```

### 方式 3：生成项目定制规则

**步骤 1**：准备定制需求

```yaml
# 保存为 .cursor/custom-requirements.yaml
project:
  name: "MyProject"
  type: "mobile"

tech:
  frontend:
    framework: "flutter"
```

**步骤 2**：请求生成

```
基于 AI-rules 规则体系和 .cursor/custom-requirements.yaml，
生成项目定制规则并保存到 .cursor/rules/project-rules.mdc
```

---

## 四、Deep Code 使用方式

Deep Code CLI（`~/.deepcode`）已通过全局配置接入本规则体系，任意项目会话自动生效。

### 方式 1：全局接入（已配置，推荐）

`~/.deepcode/AGENTS.md` 已内嵌 `compact-core.md`（Tier 1 约束 + Tier 2 清单 + 触发机制入口），Deep Code 在每个**无项目级 AGENTS.md** 的会话中自动加载。

- 无需额外操作；规则更新时按该文件 §6 核对 `main.md` 版本号
- 若某项目有自己的 `AGENTS.md`，全局文件不再自动加载，须在项目文件中显式引用（见方式 2）

### 方式 2：项目级接入

在项目根创建 `AGENTS.md` 或 `.deepcode/AGENTS.md`：

```markdown
# 项目规则

全局规则见 ~/.deepcode/AGENTS.md（必须遵守）
项目特定约定写在这里...
```

### 方式 3：初始化

在项目中运行 Deep Code 的 `/init` 命令，交互式生成项目 `AGENTS.md`。

### 方式 4：直接对话引用

```
请按照 D:\Code\.Rules\main.md 规则体系进行开发。
```

### 已配置的关联能力

| 能力 | 配置 |
|------|------|
| Skills（104 个） | `~/.deepcode/skills`（junction → `.cc-switch/skills`）自动扫描，`/skills` 查看 |
| Skills（28 个 lark/univer） | `~/.agents/skills`（→ `.AI-TOOLS/.skills`）自动扫描，按名去重 |
| Agents（13 个角色） | `D:\Code\.AI-TOOLS\.agent\*.md`，任务匹配时读取并扮演 |
| MCP（7 个服务器） | `~/.deepcode/settings.json`：figma / ref / replicate-flux / zai-mcp / web-reader / web-search-prime / zread |

---

## 五、规则文件位置对照

| 工具          | 规则文件位置                                  | 作用域  |
| ----------- | --------------------------------------- | ---- |
| Claude Code | `项目根目录/CLAUDE.md`                       | 当前项目 |
| Claude Code | `~/.claude/CLAUDE.md`                   | 全局   |
| Deep Code   | `项目根目录/AGENTS.md` 或 `项目根目录/.deepcode/AGENTS.md` | 当前项目 |
| Deep Code   | `~/.deepcode/AGENTS.md`                  | 全局（无项目文件时生效） |
| Cursor      | `项目目录/.cursor/rules/*.mdc`              | 当前项目 |
| Cursor      | `~/.cursor/rules/*.mdc`                 | 全局   |
| Trae        | `项目目录/.trae/rules/project_rules.md`     | 当前项目 |
| Trae        | `D:\Trae-Tools\rules\personal_rules.md` | 全局   |

---

## 六、快速开始模板

### Flutter 项目

**CLAUDE.md** (Claude Code):

```markdown
# Flutter 项目规则

## 规则体系
@D:\Code\.Rules\main.md

## 预设配置
使用 stacks/flutter/presets/flutter.yaml

## 项目信息
- 项目名称: MyApp
- 技术栈: Flutter 3.5+ / Riverpod 3.x / go_router 17.x

## 特殊需求
- 需要离线支持
- 支持 Android 和 iOS
```

### React 项目

**CLAUDE.md**:

```markdown
# React 项目规则

## 规则体系
@D:\Code\.Rules\main.md

## 预设配置
使用 stacks/react/presets/react.yaml

## 项目信息
- 项目名称: MyWebApp
- 技术栈: React 18 / TypeScript / Zustand / Tailwind

## 特殊需求
- 需要国际化支持
- 目标覆盖率 85%
```

### 全栈项目

**CLAUDE.md**:

```markdown
# 全栈项目规则

## 规则体系
@D:\Code\.Rules\main.md

## 预设配置
使用 stacks/fullstack/presets/fullstack.yaml

## 项目信息
- 项目名称: MyFullstackApp
- 前端: React + TypeScript
- 后端: FastAPI + PostgreSQL

## 特殊需求
- Monorepo 结构
- 共享类型定义
```

---

## 七、规则优先级

```
┌─────────────────────────────────────────────────────────┐
│                     规则优先级                           │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  项目定制规则 (CLAUDE.md / project_rules.md)            │
│       ↓                                                 │
│  预设配置 (stacks/{tech}/presets/*.yaml)                │
│       ↓                                                 │
│  项目结构规则 (common/structures/)                       │
│       ↓                                                 │
│  核心规则 (common/core/)                                │
│       ↓                                                 │
│  默认行为                                               │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

高优先级规则覆盖低优先级规则。

---

## 八、常用指令

### 生成定制规则

```
基于 [AI-rules] 规则体系和 [预设文件/配置] 生成项目定制规则
```

### 使用规则开发

```
请按照项目规则 (CLAUDE.md / .cursor/rules/ / .trae/rules/) 实现：
[功能描述]
```

### 检查代码规范

```
请检查以下代码是否符合项目规则：
[代码片段]
```

---

## 九、示例对话

### 示例 1：新项目初始化

```
用户: 我要创建一个 Flutter 项目，请帮我生成规则。

AI: 请基于 AI-rules 规则体系和 stacks/flutter/presets/flutter.yaml 预设，
    为你的项目生成定制规则。请提供：
    - 项目名称
    - 特殊需求（如有）

用户: 项目名称 MindCare，需要离线支持。

AI: [生成定制规则]
    已为你生成项目定制规则，请保存为 CLAUDE.md
```

### 示例 2：功能开发

```
用户: 请按照 CLAUDE.md 规则实现用户登录功能。

AI: [根据规则实现功能]
    - 创建 LoginPage 组件
    - 使用 Riverpod 管理登录状态
    - 使用 flutter_secure_storage 存储 token
    ...
```

---

## 十、注意事项

1. **规则文件编码**: 使用 UTF-8 编码
2. **路径引用**: 使用绝对路径或相对于项目根目录的路径
3. **规则更新**: 修改定制需求后重新生成规则
4. **版本管理**: 将规则文件（CLAUDE.md / .cursor/rules/ / .trae/rules/）提交到 Git 仓库

---

*根据你的工具选择对应的使用方式*
