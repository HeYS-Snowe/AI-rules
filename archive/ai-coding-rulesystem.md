# AI 编码规则体系 (AI Coding Rulesystem)

> 版本：v1.0.0
> 更新日期：2025-03-12
> 设计理念：**规则即模板，模板即规则**

---

## 概述

本规则体系采用**可组合、可定制**的设计：

```
┌─────────────────────────────────────────────────────────┐
│                    AI 编码规则体系                        │
├─────────────────────────────────────────────────────────┤
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐      │
│  │  核心原则    │  │  开发流程    │  │  项目结构    │      │
│  │  (必选)     │  │  (必选)     │  │  (必选)     │      │
│  └─────────────┘  └─────────────┘  └─────────────┘      │
│          │              │              │                │
│          └──────────────┼──────────────┘                │
│                         ▼                                │
│              ┌─────────────────────┐                    │
│              │   + 定制需求 (可选)  │                    │
│              └─────────────────────┘                    │
│                         ▼                                │
│              ┌─────────────────────┐                    │
│              │   = 项目定制规则     │                    │
│              └─────────────────────┘                    │
└─────────────────────────────────────────────────────────┘
```

**使用方式**：
1. **直接使用**：将本文件作为规则，AI 遵循默认行为
2. **定制使用**：附加项目类型/特定需求，AI 输出定制化规则

---

## 一、核心原则层 (Core Principles)

> 所有项目必须遵守的基础原则

### 1.1 代码质量原则

| 原则 | 全称 | 说明 |
|------|------|------|
| **SOLID** | 面向对象设计五大原则 | 单一职责、开闭、里氏替换、接口隔离、依赖倒置 |
| **DRY** | Don't Repeat Yourself | 避免代码重复，抽象公共逻辑 |
| **KISS** | Keep It Simple, Stupid | 保持简单，避免过度设计 |
| **YAGNI** | You Aren't Gonna Need It | 不做当前不需要的功能 |

### 1.2 安全原则 (OWASP Top 10)

| 编号 | 安全要求 |
|------|----------|
| SEC-01 | 所有用户输入必须验证和清理 |
| SEC-02 | 禁止硬编码敏感信息（密钥、密码、Token） |
| SEC-03 | 使用参数化查询防止 SQL 注入 |
| SEC-04 | 对输出进行编码防止 XSS 攻击 |
| SEC-05 | 敏感数据加密存储，传输使用 HTTPS |
| SEC-06 | 实施适当的认证和授权机制 |
| SEC-07 | 记录安全相关事件，但不记录敏感数据 |

### 1.3 代码风格原则

```yaml
命名规范:
  类名/类型: PascalCase        # UserService, UserProfile
  函数/方法: camelCase         # getUserById, calculateTotal
  变量: camelCase              # userName, itemCount
  常量: UPPER_SNAKE_CASE       # MAX_RETRY_COUNT, API_TIMEOUT
  私有成员: _前缀              # _privateMethod, _internalState
  文件名: 根据语言约定         # Dart: snake_case, TS: camelCase

注释规范:
  类/接口: 必须有文档注释
  公开方法: 必须有文档注释（参数、返回值、异常）
  复杂逻辑: 必须有行内注释
  TODO: 格式为 TODO(author): description

代码组织:
  单文件职责: 单一职责，不超过 500 行
  函数长度: 不超过 50 行
  嵌套深度: 不超过 4 层
  参数数量: 不超过 5 个（超过使用对象封装）
```

---

## 二、开发流程层 (Development Process)

> 完整的开发全流程规范

### 2.1 需求分析阶段

```yaml
输入: 用户需求描述
输出: 需求文档

分析要点:
  1. 功能需求: 明确要做什么
  2. 非功能需求: 性能、安全、可用性
  3. 边界条件: 正常/异常/边界情况
  4. 依赖关系: 外部系统、库、服务
  5. 风险点: 技术难点、不确定性

输出格式:
  ## 功能描述
  {功能概述}

  ## 用户故事
  作为 {角色}，我希望 {行为}，以便 {目的}

  ## 验收标准
  - [ ] 标准1
  - [ ] 标准2

  ## 技术要点
  {关键技术决策}
```

### 2.2 设计阶段

```yaml
输入: 需求文档
输出: 设计文档

设计层次:
  架构设计:
    - 系统架构图
    - 模块划分
    - 数据流向

  数据库设计:
    - ER 图
    - 表结构定义
    - 索引策略

  API 设计:
    - 接口列表
    - 请求/响应格式
    - 错误码定义

  UI 设计 (如适用):
    - 页面流程图
    - 组件层级
    - 状态管理方案
```

### 2.3 开发阶段

```yaml
开发流程:
  1. 分支策略:
     - main: 生产环境
     - develop: 开发环境
     - feature/*: 功能分支
     - fix/*: 修复分支
     - release/*: 发布分支

  2. 提交规范:
     格式: <type>(<scope>): <subject>

     类型:
       - feat: 新功能
       - fix: Bug 修复
       - refactor: 重构
       - style: 代码格式
       - docs: 文档
       - test: 测试
       - chore: 构建/工具

     示例:
       feat(auth): add JWT token refresh
       fix(api): resolve timeout issue in user service

  3. 代码审查:
     - 每次提交前自检
     - 关键变更需要审查
     - 遵循审查清单
```

### 2.4 测试阶段

```yaml
测试层次:
  单元测试:
    覆盖率: 核心逻辑 ≥ 80%
    范围: 工具函数、业务逻辑、状态管理

  集成测试:
    范围: API 接口、数据库操作、外部服务

  端到端测试:
    范围: 关键用户流程

测试命名:
  格式: test_<method>_<scenario>_<expected>
  示例: test_login_validCredentials_success

测试结构 (AAA 模式):
  // Arrange - 准备
  // Act - 执行
  // Assert - 断言
```

### 2.5 构建与部署阶段

```yaml
构建流程:
  1. 环境检查
     - 依赖版本
     - 环境变量
     - 构建工具

  2. 构建执行
     - 清理旧产物
     - 编译/打包
     - 资源优化

  3. 产物命名
     格式: {项目名}_{状态}_{版本}_{日期}_{序号}

     状态类型:
       - release: 正式版 (完全体)
       - beta: 测试版 (非完全体)
       - alpha: 内测版 (非完全体)
       - rc: 候选版 (接近完全体)
       - fix: 修复版 (完全体)
       - hotfix: 紧急修复 (完全体)
       - dev: 开发版 (非完全体)
       - debug: 调试版 (非完全体)

     示例: MindCareAI_release_1.0.0_20250312_01.apk

  4. 构建后处理
     - 调用版本管家 (version-manager)
     - 重命名构建产物
     - 备份原始文件
     - 生成构建日志

部署检查:
  - [ ] 环境变量配置正确
  - [ ] 数据库迁移已执行
  - [ ] 服务健康检查通过
  - [ ] 回滚方案已准备
```

### 2.6 文档与日志

```yaml
项目文档:
  必需文档:
    - README.md: 项目说明
    - CHANGELOG.md: 变更记录
    - API.md: API 文档 (如适用)

  可选文档:
    - ARCHITECTURE.md: 架构说明
    - CONTRIBUTING.md: 贡献指南
    - DEPLOYMENT.md: 部署指南

更新日志:
  存储位置: log/update/
  命名格式: {类型}_{YYYYMMDD}_{序号}.md

  类型:
    - feature: 新功能
    - fix: Bug 修复
    - refactor: 重构
    - config: 配置变更
    - docs: 文档更新
    - perf: 性能优化

  模板:
    # {标题}
    **类型**: {类型}
    **日期**: {YYYY-MM-DD HH:MM}
    **影响范围**: {文件/模块}

    ## 变更描述
    {描述}

    ## 修改详情
    - [文件]: {修改内容}

崩溃日志:
  存储位置: log/crash/
  命名格式: crash_{YYYYMMDD}_{HHMMSS}.md
```

---

## 三、项目结构层 (Project Structure)

> 详细的目录结构规范，参见 `common/structures/project-structure-guide.md`

### 3.1 通用原则

1. **按功能/类型分层**：文件夹按功能模块或文件类型组织
2. **单一职责**：每个文件夹只负责一类内容
3. **命名规范**：使用小写字母，单词间用 `-` 或 `_` 连接
4. **避免过深嵌套**：通常不超过 3-4 层

### 3.2 项目类型快速索引

| 项目类型 | 结构模板 |
|----------|----------|
| Web 前端 | React/Vue/Angular 结构 |
| Node.js 后端 | Express/NestJS/Koa 结构 |
| 全栈 Monorepo | 多应用 + 共享包结构 |
| Python Web | Django/FastAPI/Flask 结构 |
| 移动端 | Flutter/React Native 结构 |
| 微服务 | 服务 + 网关 + 共享层结构 |
| CLI 工具 | 命令 + 工具 + 模板结构 |
| 库/SDK | 核心 + 模块 + 文档结构 |

**详细结构请参考**: `D:\Code\.Rules\common\structures\project-structure-guide.md`

---

## 四、定制需求层 (Customization Layer)

> 通过附加定制需求，生成项目特定规则

### 4.1 定制需求模板

```yaml
# 定制需求文件模板 (custom-requirements.yaml)

项目信息:
  项目名称: ""
  项目类型: "" # web-frontend | web-backend | mobile | desktop | fullstack | cli | library
  技术栈:
    前端: "" # react | vue | angular | flutter | react-native
    后端: "" # node-express | node-nest | python-fastapi | python-django | go
    数据库: "" # postgresql | mysql | mongodb | sqlite
    状态管理: "" # redux | mobx | riverpod | bloc | provider

特殊需求:
  - "" # 例如: "需要离线支持"
  - "" # 例如: "需要国际化"

约束条件:
  - "" # 例如: "必须兼容 IE11"
  - "" # 例如: "包体积不超过 5MB"

第三方服务:
  - "" # 例如: "使用 Firebase Auth"
  - "" # 例如: "集成 Sentry 错误监控"
```

### 4.2 项目类型预设

#### Web 前端项目 (React + TypeScript)

```yaml
自动应用规则:
  目录结构: src/{components,pages,hooks,store,api,utils,types,assets}
  状态管理: 推荐 Zustand 或 Redux Toolkit
  路由: React Router v6+
  样式: CSS Modules / Tailwind CSS / Styled Components
  测试: Vitest + Testing Library
```

#### Flutter 项目

```yaml
自动应用规则:
  目录结构: lib/{core,data,domain,presentation}
  状态管理: Riverpod 3.x (推荐 @riverpod 注解)
  路由: go_router 17.x
  本地数据库: Drift 2.x
  网络请求: Dio 5.x
  数据建模: freezed
  测试: flutter_test + mockito
```

#### Python FastAPI 项目

```yaml
自动应用规则:
  目录结构: app/{api,core,models,schemas,services,repositories,db}
  数据验证: Pydantic v2
  数据库: SQLAlchemy 2.x / Prisma
  异步: async/await
  测试: pytest + httpx
```

### 4.3 定制规则生成指令

当需要生成定制规则时，使用以下指令格式：

```
基于 [ai-coding-rulesystem.md] 规则体系，为以下项目生成定制规则：

项目类型: [类型]
技术栈: [技术栈]
特殊需求: [需求列表]

请输出完整的项目定制规则。
```

---

## 五、规则使用指南

### 5.1 直接使用（默认模式）

将本规则文件直接提供给 AI：

```
请按照 [ai-coding-rulesystem.md] 中的规则进行开发。
```

AI 将：
1. 遵守核心原则层的所有规则
2. 按照开发流程层的规范执行
3. 参考项目结构层创建目录
4. 使用默认的项目结构

### 5.2 定制使用（定制模式）

**步骤 1**: 准备定制需求

```yaml
# my-project-requirements.yaml
项目信息:
  项目名称: "MindCare AI"
  项目类型: "mobile"
  技术栈:
    前端: "flutter"
    后端: "python-fastapi"
    数据库: "postgresql"
    状态管理: "riverpod"
```

**步骤 2**: 请求定制规则

```
基于 [ai-coding-rulesystem.md] 规则体系和以下定制需求，
生成 MindCare AI 项目的定制规则：

[粘贴 my-project-requirements.yaml 内容]
```

**步骤 3**: AI 输出定制规则

AI 将生成：
- 项目特定的目录结构
- 技术栈特定的代码规范
- 项目特定的构建流程
- 项目特定的 API 规范

### 5.3 规则优先级

```
定制规则 > 项目类型预设 > 默认规则
```

当规则冲突时，优先级高的规则生效。

---

## 六、快速参考卡

### 开发检查清单

```markdown
## 开始开发前
- [ ] 理解需求，明确验收标准
- [ ] 查看项目结构，遵循现有模式
- [ ] 确认技术栈和依赖版本

## 编写代码时
- [ ] 遵循命名规范
- [ ] 添加必要的注释和文档
- [ ] 处理错误和边界情况
- [ ] 不硬编码敏感信息

## 提交代码前
- [ ] 代码自检（格式、注释、逻辑）
- [ ] 运行测试确保通过
- [ ] 编写/更新测试用例
- [ ] 更新相关文档
- [ ] 使用规范的提交信息

## 构建部署前
- [ ] 检查环境变量配置
- [ ] 确认版本号正确
- [ ] 执行完整构建测试
- [ ] 准备构建日志
```

### 常用命令模板

```bash
# 提交代码
git commit -m "feat(module): description"

# 创建功能分支
git checkout -b feature/new-feature

# 运行测试
npm test / flutter test / pytest

# 构建项目
npm run build / flutter build apk / python -m build
```

---

## 七、附录

### A. 文件编码规范

| 文件类型 | 编码 | 换行符 |
|----------|------|--------|
| 源代码 | UTF-8 | LF |
| 配置文件 | UTF-8 | LF |
| Markdown | UTF-8 | LF |
| JSON/YAML | UTF-8 (无 BOM) | LF |

### B. Git 忽略文件模板

```gitignore
# 依赖
node_modules/
vendor/
.venv/

# 构建产物
dist/
build/
*.apk
*.aab

# 环境变量
.env
.env.local
.env.*.local

# IDE
.idea/
.vscode/
*.swp

# 日志
logs/
*.log

# 系统文件
.DS_Store
Thumbs.db
```

### C. 版本号规范

```
主版本.次版本.修订号.构建序号

示例:
1.0.0.1      # 首次发布
1.1.0.2      # 新增功能
1.1.1.3      # Bug 修复
2.0.0.4      # 重大更新
```

---

## 变更记录

| 版本 | 日期 | 变更内容 |
|------|------|----------|
| v1.0.0 | 2025-03-12 | 初始版本，整合核心原则、开发流程、项目结构 |

---

*本规则体系设计为"规则即模板，模板即规则"的元规则系统*
*可直接使用，也可通过定制需求生成项目特定规则*
