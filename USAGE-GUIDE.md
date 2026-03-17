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
- 规则位置: D:\Desktop\Claude临时对话\AI-rules\main.md

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

@D:\Desktop\Claude临时对话\AI-rules\main.md

项目信息：
- 项目名称: MyProject
- 技术栈: Flutter + Riverpod
```

### 方式 3：生成项目定制规则

**步骤 1**：复制模板
```bash
cp "D:\Desktop\Claude临时对话\AI-rules\templates\custom-requirements.yaml" "./my-project-requirements.yaml"
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
基于 AI-rules 规则体系 (D:\Desktop\Claude临时对话\AI-rules\main.md)
和以下配置，生成项目定制规则：

[粘贴 my-project-requirements.yaml 内容]

请将生成的规则保存到项目的 CLAUDE.md 文件。
```

### 方式 4：使用预设快速开始

```
请基于 AI-rules 规则体系和 presets/flutter.yaml 预设，
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

遵循 AI 编码规则体系: D:\Desktop\Claude临时对话\AI-rules\main.md

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
- D:\Desktop\Claude临时对话\AI-rules\main.md

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

## 三、规则文件位置对照

| 工具 | 规则文件位置 | 作用域 |
|------|-------------|--------|
| Claude Code | `项目根目录/CLAUDE.md` | 当前项目 |
| Claude Code | `~/.claude/CLAUDE.md` | 全局 |
| Trae | `项目目录/.trae/rules/project_rules.md` | 当前项目 |
| Trae | `D:\Trae-Tools\rules\personal_rules.md` | 全局 |

---

## 四、快速开始模板

### Flutter 项目

**CLAUDE.md** (Claude Code):
```markdown
# Flutter 项目规则

## 规则体系
@D:\Desktop\Claude临时对话\AI-rules\main.md

## 预设配置
使用 presets/flutter.yaml

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
@D:\Desktop\Claude临时对话\AI-rules\main.md

## 预设配置
使用 presets/react.yaml

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
@D:\Desktop\Claude临时对话\AI-rules\main.md

## 预设配置
使用 presets/fullstack.yaml

## 项目信息
- 项目名称: MyFullstackApp
- 前端: React + TypeScript
- 后端: FastAPI + PostgreSQL

## 特殊需求
- Monorepo 结构
- 共享类型定义
```

---

## 五、规则优先级

```
┌─────────────────────────────────────────────────────────┐
│                     规则优先级                           │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  项目定制规则 (CLAUDE.md / project_rules.md)            │
│       ↓                                                 │
│  预设配置 (presets/*.yaml)                              │
│       ↓                                                 │
│  项目结构规则 (structures/)                             │
│       ↓                                                 │
│  核心规则 (core/)                                       │
│       ↓                                                 │
│  默认行为                                               │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

高优先级规则覆盖低优先级规则。

---

## 六、常用指令

### 生成定制规则
```
基于 [AI-rules] 规则体系和 [预设文件/配置] 生成项目定制规则
```

### 使用规则开发
```
请按照项目规则 (CLAUDE.md / .trae/rules/project_rules.md) 实现：
[功能描述]
```

### 检查代码规范
```
请检查以下代码是否符合项目规则：
[代码片段]
```

---

## 七、示例对话

### 示例 1：新项目初始化

```
用户: 我要创建一个 Flutter 项目，请帮我生成规则。

AI: 请基于 AI-rules 规则体系和 presets/flutter.yaml 预设，
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

## 八、注意事项

1. **规则文件编码**: 使用 UTF-8 编码
2. **路径引用**: 使用绝对路径或相对于项目根目录的路径
3. **规则更新**: 修改定制需求后重新生成规则
4. **版本管理**: 将 CLAUDE.md 提交到 Git 仓库

---

*根据你的工具选择对应的使用方式*
