# 项目结构规范指南

> 本规则用于指导 AI 编码时创建规范的项目目录结构

---

## 通用原则

1. **按功能/类型分层**：文件夹按功能模块或文件类型组织
2. **单一职责**：每个文件夹只负责一类内容
3. **命名规范**：使用小写字母，单词间用 `-` 或 `_` 连接
4. **避免过深嵌套**：通常不超过 3-4 层

---

## 一、Web 前端项目

### React / Vue / Angular 通用结构

```
project-root/
├── public/                     # 静态资源目录（不经过构建处理）
│   ├── favicon.ico            # 网站图标
│   ├── robots.txt             # 爬虫规则
│   └── images/                # 静态图片
│
├── src/                       # 源代码目录
│   ├── assets/                # 需要构建处理的静态资源
│   │   ├── images/            # 图片资源（会被hash处理）
│   │   ├── fonts/             # 字体文件
│   │   └── styles/            # 全局样式文件
│   │       ├── variables.scss # 样式变量
│   │       ├── mixins.scss    # 样式混入
│   │       └── global.scss    # 全局样式
│   │
│   ├── components/            # 可复用的UI组件
│   │   ├── common/            # 通用组件（Button, Input, Modal等）
│   │   ├── layout/            # 布局组件（Header, Footer, Sidebar等）
│   │   └── business/          # 业务组件
│   │
│   ├── pages/                 # 页面组件（路由对应的页面）
│   │   ├── home/
│   │   │   ├── index.tsx      # 页面主组件
│   │   │   ├── style.scss     # 页面样式
│   │   │   └── components/    # 页面私有组件
│   │   └── user/
│   │
│   ├── hooks/                 # 自定义Hooks（React）
│   │   └── useAuth.ts
│   │
│   ├── composables/           # 组合式函数（Vue）
│   │   └── useAuth.ts
│   │
│   ├── store/                 # 状态管理
│   │   ├── index.ts           # Store入口
│   │   ├── modules/           # Store模块
│   │   └── types.ts           # Store类型定义
│   │
│   ├── router/                # 路由配置
│   │   ├── index.ts           # 路由入口
│   │   └── guards.ts          # 路由守卫
│   │
│   ├── api/                   # API接口
│   │   ├── index.ts           # API入口/axios实例
│   │   ├── user.ts            # 用户相关API
│   │   └── types.ts           # API响应类型
│   │
│   ├── utils/                 # 工具函数
│   │   ├── format.ts          # 格式化函数
│   │   ├── validate.ts        # 验证函数
│   │   └── storage.ts         # 本地存储封装
│   │
│   ├── constants/             # 常量定义
│   │   └── index.ts
│   │
│   ├── types/                 # 全局类型定义
│   │   └── index.d.ts
│   │
│   ├── services/              # 业务服务层
│   │   └── auth.service.ts
│   │
│   ├── directives/            # 自定义指令（Vue）
│   │   └── permission.ts
│   │
│   ├── App.tsx                # 根组件
│   └── main.ts                # 应用入口
│
├── tests/                     # 测试文件
│   ├── unit/                  # 单元测试
│   └── e2e/                   # 端到端测试
│
├── .env                       # 环境变量
├── .env.development           # 开发环境变量
├── .env.production            # 生产环境变量
├── package.json               # 项目配置
├── tsconfig.json              # TypeScript配置
├── vite.config.ts             # 构建工具配置
└── README.md                  # 项目说明
```

---

## 二、Node.js 后端项目

### Express / NestJS / Koa 通用结构

```
project-root/
├── src/
│   ├── config/                # 配置文件
│   │   ├── index.ts           # 配置入口
│   │   ├── database.ts        # 数据库配置
│   │   └── app.ts             # 应用配置
│   │
│   ├── controllers/           # 控制器（处理HTTP请求）
│   │   └── user.controller.ts
│   │
│   ├── services/              # 业务逻辑层
│   │   └── user.service.ts
│   │
│   ├── repositories/          # 数据访问层
│   │   └── user.repository.ts
│   │
│   ├── models/                # 数据模型/实体
│   │   ├── index.ts
│   │   └── user.model.ts
│   │
│   ├── entities/              # 数据库实体（TypeORM等）
│   │   └── user.entity.ts
│   │
│   ├── dto/                   # 数据传输对象
│   │   ├── create-user.dto.ts
│   │   └── update-user.dto.ts
│   │
│   ├── middlewares/           # 中间件
│   │   ├── auth.middleware.ts
│   │   ├── error.middleware.ts
│   │   └── validate.middleware.ts
│   │
│   ├── routes/                # 路由定义
│   │   ├── index.ts           # 路由入口
│   │   └── user.routes.ts
│   │
│   ├── validators/            # 请求验证
│   │   └── user.validator.ts
│   │
│   ├── utils/                 # 工具函数
│   │   ├── logger.ts          # 日志工具
│   │   ├── response.ts        # 响应格式化
│   │   └── crypto.ts          # 加密工具
│   │
│   ├── types/                 # 类型定义
│   │   └── index.d.ts
│   │
│   ├── constants/             # 常量
│   │   └── error-codes.ts
│   │
│   ├── exceptions/            # 自定义异常
│   │   └── http.exception.ts
│   │
│   ├── interfaces/            # 接口定义
│   │   └── user.interface.ts
│   │
│   ├── migrations/            # 数据库迁移
│   │   └── 001-create-users.ts
│   │
│   ├── seeders/               # 数据库种子
│   │   └── user.seeder.ts
│   │
│   └── app.ts                 # 应用入口
│
├── prisma/                    # Prisma相关（如使用Prisma）
│   ├── schema.prisma          # 数据库模型定义
│   └── migrations/            # 迁移文件
│
├── tests/                     # 测试
│   ├── unit/                  # 单元测试
│   ├── integration/           # 集成测试
│   └── setup.ts               # 测试配置
│
├── logs/                      # 日志目录
│   ├── error.log
│   └── combined.log
│
├── uploads/                   # 上传文件目录
│
├── .env                       # 环境变量
├── .env.example               # 环境变量示例
└── package.json
```

---

## 三、全栈项目（Monorepo）

```
project-root/
├── apps/                      # 应用目录
│   ├── web/                   # 前端应用
│   │   └── ...（参考前端结构）
│   │
│   ├── admin/                 # 管理后台
│   │   └── ...
│   │
│   ├── api/                   # 后端API
│   │   └── ...（参考后端结构）
│   │
│   └── mobile/                # 移动端应用
│       └── ...
│
├── packages/                  # 共享包
│   ├── ui/                    # 共享UI组件库
│   │   ├── src/
│   │   ├── package.json
│   │   └── tsconfig.json
│   │
│   ├── types/                 # 共享类型定义
│   │   ├── src/
│   │   │   └── index.ts
│   │   └── package.json
│   │
│   ├── utils/                 # 共享工具函数
│   │   ├── src/
│   │   └── package.json
│   │
│   ├── config/                # 共享配置
│   │   ├── eslint/
│   │   ├── typescript/
│   │   └── prettier/
│   │
│   └── api-client/            # API客户端
│       ├── src/
│       └── package.json
│
├── tooling/                   # 工具配置
│   ├── eslint/                # ESLint配置
│   ├── typescript/            # TypeScript配置
│   └── tailwind/              # Tailwind配置
│
├── docs/                      # 项目文档
│   ├── api.md
│   └── deployment.md
│
├── turbo.json                 # Turborepo配置
├── pnpm-workspace.yaml        # pnpm工作区配置
└── package.json               # 根package.json
```

---

## 四、Python 项目

### Django 项目

```
project-root/
├── project_name/              # Django项目目录
│   ├── __init__.py
│   ├── settings/              # 设置（分离配置）
│   │   ├── __init__.py
│   │   ├── base.py            # 基础配置
│   │   ├── development.py     # 开发配置
│   │   └── production.py      # 生产配置
│   ├── urls.py                # URL路由
│   ├── asgi.py
│   └── wsgi.py
│
├── apps/                      # 应用目录
│   ├── users/                 # 用户应用
│   │   ├── __init__.py
│   │   ├── models.py          # 数据模型
│   │   ├── views.py           # 视图
│   │   ├── serializers.py     # 序列化器
│   │   ├── urls.py            # 路由
│   │   ├── admin.py           # Admin配置
│   │   ├── tests/             # 测试
│   │   │   ├── __init__.py
│   │   │   └── test_views.py
│   │   ├── migrations/        # 迁移文件
│   │   └── management/        # 管理命令
│   │       └── commands/
│   │
│   └── products/              # 其他应用
│       └── ...
│
├── core/                      # 核心功能
│   ├── __init__.py
│   ├── middleware.py          # 中间件
│   ├── exceptions.py          # 异常处理
│   └── utils.py               # 工具函数
│
├── static/                    # 静态文件
│   ├── css/
│   ├── js/
│   └── images/
│
├── media/                     # 用户上传文件
│   └── uploads/
│
├── templates/                 # 模板文件
│   ├── base.html
│   └── ...
│
├── requirements/              # 依赖（分离）
│   ├── base.txt
│   ├── development.txt
│   └── production.txt
│
├── manage.py                  # Django管理脚本
├── .env
└── README.md
```

### FastAPI / Flask 项目

```
project-root/
├── app/
│   ├── __init__.py
│   ├── main.py                # 应用入口
│   │
│   ├── api/                   # API路由
│   │   ├── __init__.py
│   │   ├── deps.py            # 依赖注入
│   │   └── v1/                # API版本
│   │       ├── __init__.py
│   │       ├── endpoints/
│   │       │   ├── users.py
│   │       │   └── auth.py
│   │       └── router.py
│   │
│   ├── core/                  # 核心配置
│   │   ├── __init__.py
│   │   ├── config.py          # 配置类
│   │   ├── security.py        # 安全相关
│   │   └── exceptions.py      # 异常定义
│   │
│   ├── models/                # 数据库模型
│   │   ├── __init__.py
│   │   └── user.py
│   │
│   ├── schemas/               # Pydantic模型
│   │   ├── __init__.py
│   │   ├── user.py
│   │   └── token.py
│   │
│   ├── services/              # 业务逻辑
│   │   ├── __init__.py
│   │   └── user_service.py
│   │
│   ├── repositories/          # 数据访问
│   │   ├── __init__.py
│   │   └── user_repo.py
│   │
│   ├── db/                    # 数据库相关
│   │   ├── __init__.py
│   │   ├── session.py         # 数据库会话
│   │   └── base.py            # 基础模型类
│   │
│   ├── utils/                 # 工具函数
│   │   ├── __init__.py
│   │   └── helpers.py
│   │
│   └── middleware/            # 中间件
│       └── auth.py
│
├── alembic/                   # 数据库迁移
│   ├── versions/
│   └── env.py
│
├── tests/                     # 测试
│   ├── __init__.py
│   ├── conftest.py            # pytest配置
│   ├── unit/
│   └── integration/
│
├── scripts/                   # 脚本
│   └── seed_data.py
│
├── requirements.txt
├── pyproject.toml
└── .env
```

---

## 五、移动端项目

### Flutter 项目

```
project-root/
├── lib/                       # Dart源代码
│   ├── main.dart              # 应用入口
│   │
│   ├── core/                  # 核心功能
│   │   ├── constants/         # 常量
│   │   ├── theme/             # 主题配置
│   │   ├── utils/             # 工具函数
│   │   └── errors/            # 错误处理
│   │
│   ├── data/                  # 数据层
│   │   ├── models/            # 数据模型
│   │   ├── repositories/      # 数据仓库
│   │   ├── datasources/       # 数据源
│   │   │   ├── local/         # 本地数据源
│   │   │   └── remote/        # 远程数据源
│   │   └── services/          # API服务
│   │
│   ├── domain/                # 领域层（Clean Architecture）
│   │   ├── entities/          # 实体
│   │   ├── repositories/      # 仓库接口
│   │   └── usecases/          # 用例
│   │
│   ├── presentation/          # 表现层
│   │   ├── pages/             # 页面
│   │   ├── widgets/           # 组件
│   │   ├── blocs/             # BLoC状态管理
│   │   └── providers/         # Provider
│   │
│   ├── routes/                # 路由
│   │   └── app_router.dart
│   │
│   └── injection_container.dart # 依赖注入
│
├── assets/                    # 资源文件
│   ├── images/
│   ├── fonts/
│   └── animations/
│
├── test/                      # 测试
│   ├── unit/
│   ├── widget/
│   └── integration/
│
├── android/                   # Android原生
├── ios/                       # iOS原生
├── pubspec.yaml               # 依赖配置
└── analysis_options.yaml      # 分析配置
```

### React Native 项目

```
project-root/
├── src/
│   ├── screens/               # 页面/屏幕
│   │   ├── Home/
│   │   │   ├── index.tsx
│   │   │   ├── styles.ts
│   │   │   └── components/
│   │   └── Profile/
│   │
│   ├── components/            # 可复用组件
│   │   ├── Button/
│   │   ├── Input/
│   │   └── index.ts           # 组件导出
│   │
│   ├── navigation/            # 导航配置
│   │   ├── index.tsx
│   │   ├── types.ts
│   │   └── AuthNavigator.tsx
│   │
│   ├── services/              # API服务
│   │   ├── api.ts             # Axios实例
│   │   └── auth.service.ts
│   │
│   ├── store/                 # 状态管理
│   │   ├── index.ts
│   │   └── slices/
│   │
│   ├── hooks/                 # 自定义Hooks
│   │   └── useTheme.ts
│   │
│   ├── utils/                 # 工具函数
│   │   ├── storage.ts
│   │   └── helpers.ts
│   │
│   ├── constants/             # 常量
│   │   └── theme.ts
│   │
│   ├── types/                 # 类型定义
│   │   └── index.d.ts
│   │
│   ├── assets/                # 资源文件
│   │   ├── images/
│   │   ├── fonts/
│   │   └── icons/
│   │
│   └── App.tsx                # 应用入口
│
├── android/                   # Android原生代码
├── ios/                       # iOS原生代码
├── __tests__/                 # 测试文件
├── app.json                   # 应用配置
├── package.json
├── tsconfig.json
└── metro.config.js            # Metro配置
```

---

## 六、微服务项目

```
project-root/
├── services/                  # 微服务目录
│   ├── auth-service/          # 认证服务
│   │   ├── src/
│   │   │   ├── controllers/
│   │   │   ├── services/
│   │   │   ├── repositories/
│   │   │   ├── models/
│   │   │   ├── dto/
│   │   │   └── index.ts
│   │   ├── tests/
│   │   ├── Dockerfile
│   │   └── package.json
│   │
│   ├── user-service/          # 用户服务
│   │   └── ...
│   │
│   └── order-service/         # 订单服务
│       └── ...
│
├── gateway/                   # API网关
│   ├── src/
│   │   ├── routes/
│   │   ├── middleware/
│   │   └── index.ts
│   ├── Dockerfile
│   └── package.json
│
├── shared/                    # 共享代码
│   ├── types/                 # 共享类型
│   ├── utils/                 # 共享工具
│   ├── middleware/            # 共享中间件
│   └── events/                # 事件定义
│
├── infrastructure/            # 基础设施
│   ├── docker/                # Docker配置
│   │   ├── docker-compose.yml
│   │   └── docker-compose.prod.yml
│   │
│   ├── kubernetes/            # K8s配置
│   │   ├── deployments/
│   │   ├── services/
│   │   └── configmaps/
│   │
│   └── terraform/             # IaC配置
│
├── proto/                     # Protobuf定义（gRPC）
│   ├── auth.proto
│   └── user.proto
│
├── scripts/                   # 脚本
│   ├── build.sh
│   └── deploy.sh
│
├── docs/                      # 文档
│   ├── architecture.md
│   └── api/
│
├── .github/                   # GitHub Actions
│   └── workflows/
│       ├── ci.yml
│       └── cd.yml
│
└── README.md
```

---

## 七、CLI 工具项目

```
project-root/
├── src/
│   ├── index.ts               # 入口文件
│   │
│   ├── commands/              # 命令实现
│   │   ├── index.ts           # 命令注册
│   │   ├── init.ts            # init命令
│   │   ├── build.ts           # build命令
│   │   └── deploy.ts          # deploy命令
│   │
│   ├── options/               # 命令选项
│   │   └── index.ts
│   │
│   ├── utils/                 # 工具函数
│   │   ├── logger.ts          # 日志输出
│   │   ├── config.ts          # 配置处理
│   │   └── fs.ts              # 文件操作
│   │
│   ├── templates/             # 模板文件
│   │   └── project-template/
│   │
│   ├── services/              # 服务层
│   │   └── generator.ts
│   │
│   └── types/                 # 类型定义
│       └── index.d.ts
│
├── bin/                       # 可执行文件
│   └── cli.js
│
├── tests/                     # 测试
│   ├── commands/
│   └── utils/
│
├── package.json
├── tsconfig.json
└── README.md
```

---

## 八、库/SDK 项目

```
project-root/
├── src/                       # 源代码
│   ├── index.ts               # 入口导出
│   │
│   ├── core/                  # 核心功能
│   │   ├── Client.ts          # 主客户端
│   │   └── Config.ts          # 配置
│   │
│   ├── modules/               # 功能模块
│   │   ├── auth/
│   │   │   ├── index.ts
│   │   │   └── types.ts
│   │   └── users/
│   │
│   ├── utils/                 # 工具函数
│   │   └── helpers.ts
│   │
│   ├── types/                 # 类型定义
│   │   ├── index.ts           # 公开类型
│   │   └── internal.ts        # 内部类型
│   │
│   └── errors/                # 错误定义
│       └── index.ts
│
├── examples/                  # 示例代码
│   ├── basic/
│   └── advanced/
│
├── docs/                      # 文档
│   ├── getting-started.md
│   ├── api-reference.md
│   └── examples.md
│
├── tests/                     # 测试
│   ├── unit/
│   ├── integration/
│   └── setup.ts
│
├── dist/                      # 构建输出
├── package.json
├── tsconfig.json
├── tsconfig.build.json        # 构建用TS配置
├── rollup.config.ts           # 打包配置
├── CHANGELOG.md               # 变更日志
├── LICENSE                    # 许可证
└── README.md
```

---

## 九、测试目录规范

```
tests/                         # 或 __tests__ / test
├── unit/                      # 单元测试
│   ├── components/            # 组件测试
│   ├── services/              # 服务测试
│   └── utils/                 # 工具函数测试
│
├── integration/               # 集成测试
│   └── api/                   # API集成测试
│
├── e2e/                       # 端到端测试
│   ├── fixtures/              # 测试数据
│   └── specs/                 # 测试规格
│
├── mocks/                     # Mock数据/函数
│   └── user.mock.ts
│
├── helpers/                   # 测试辅助函数
│   └── test-utils.tsx
│
├── setup.ts                   # 测试环境设置
├── teardown.ts                # 测试环境清理
└── jest.config.js             # 测试配置
```

---

## 十、配置文件命名规范

| 文件名 | 用途 |
|--------|------|
| `.env` | 环境变量（不提交到Git） |
| `.env.example` | 环境变量示例（提交到Git） |
| `.env.development` | 开发环境变量 |
| `.env.production` | 生产环境变量 |
| `.gitignore` | Git忽略文件 |
| `.dockerignore` | Docker忽略文件 |
| `.eslintrc.js` / `.eslintrc.json` | ESLint配置 |
| `.prettierrc` | Prettier配置 |
| `tsconfig.json` | TypeScript配置 |
| `package.json` | Node.js项目配置 |
| `docker-compose.yml` | Docker编排配置 |
| `Dockerfile` | Docker镜像构建 |
| `Makefile` | Make命令配置 |
| `README.md` | 项目说明文档 |
| `CHANGELOG.md` | 版本变更记录 |
| `CONTRIBUTING.md` | 贡献指南 |
| `LICENSE` | 开源许可证 |

---

## 快速决策指南

当创建新项目时，根据项目类型选择对应结构：

| 项目类型 | 推荐结构 |
|----------|----------|
| 单页应用（SPA） | [一、Web 前端项目](#一web-前端项目) |
| 后端 API | [二、Node.js 后端项目](#二nodejs-后端项目) |
| 全栈应用 | [三、全栈项目（Monorepo）](#三全栈项目monorepo) |
| Python Web | [四、Python 项目](#四python-项目) |
| 移动应用 | [五、移动端项目](#五移动端项目) |
| 微服务架构 | [六、微服务项目](#六微服务项目) |
| CLI 工具 | [七、CLI 工具项目](#七cli-工具项目) |
| 库/SDK | [八、库SDK 项目](#八库sdk-项目) |

---

## 最佳实践

1. **一致性**：同一项目内保持目录结构一致
2. **可预测性**：文件位置应符合直觉，易于查找
3. **模块化**：按功能/领域划分，便于维护和测试
4. **可扩展**：预留扩展空间，避免频繁重构目录
5. **文档化**：复杂结构应在 README 中说明

