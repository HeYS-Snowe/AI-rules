# MindCare AI 项目定制规则

> 基于规则体系 + 项目定制需求生成
> 生成日期：2025-03-12

---

## 项目信息

| 属性 | 值 |
|------|-----|
| 项目名称 | MindCare AI - 校园心理健康预警系统 |
| 项目类型 | 全栈项目（移动端 + 后端） |
| 前端技术 | Flutter 3.5+ / Dart |
| 后端技术 | Python FastAPI |
| 数据库 | PostgreSQL |
| 状态管理 | Riverpod 3.x |

---

## 一、目录结构规范

### Flutter 应用 (mindcare_app/)

```
mindcare_app/
├── lib/
│   ├── main.dart                    # 应用入口
│   ├── app.dart                     # 应用根组件
│   │
│   ├── core/                        # 核心功能
│   │   ├── constants/               # 常量定义
│   │   │   ├── app_constants.dart
│   │   │   └── api_constants.dart
│   │   ├── theme/                   # 主题配置
│   │   │   ├── app_theme.dart
│   │   │   └── colors.dart
│   │   ├── router/                  # 路由配置
│   │   │   └── app_router.dart
│   │   ├── errors/                  # 错误处理
│   │   │   ├── exceptions.dart
│   │   │   └── failures.dart
│   │   └── utils/                   # 工具函数
│   │       ├── validators.dart
│   │       └── formatters.dart
│   │
│   ├── data/                        # 数据层
│   │   ├── models/                  # 数据模型 (freezed)
│   │   │   ├── user_model.dart
│   │   │   ├── emotion_model.dart
│   │   │   └── alert_model.dart
│   │   ├── repositories/            # 仓库实现
│   │   │   ├── auth_repository_impl.dart
│   │   │   └── emotion_repository_impl.dart
│   │   └── datasources/             # 数据源
│   │       ├── local/               # 本地数据源
│   │       │   ├── database.dart    # Drift 数据库
│   │       │   └── secure_storage.dart
│   │       └── remote/              # 远程数据源
│   │           ├── api_client.dart  # Dio 客户端
│   │           └── auth_api.dart
│   │
│   ├── domain/                      # 领域层
│   │   ├── entities/                # 领域实体
│   │   │   ├── user.dart
│   │   │   └── emotion_record.dart
│   │   └── repositories/            # 仓库接口
│   │       ├── auth_repository.dart
│   │       └── emotion_repository.dart
│   │
│   ├── presentation/                # 展示层
│   │   ├── pages/                   # 页面
│   │   │   ├── auth/
│   │   │   │   ├── login_page.dart
│   │   │   │   └── register_page.dart
│   │   │   ├── home/
│   │   │   │   └── home_page.dart
│   │   │   ├── emotion/
│   │   │   │   ├── checkin_page.dart
│   │   │   │   └── history_page.dart
│   │   │   └── profile/
│   │   │       └── profile_page.dart
│   │   ├── widgets/                 # 通用组件
│   │   │   ├── common/
│   │   │   │   ├── app_button.dart
│   │   │   │   └── app_text_field.dart
│   │   │   └── emotion/
│   │   │       ├── emotion_picker.dart
│   │   │       └── emotion_chart.dart
│   │   └── providers/               # Riverpod Providers
│   │       ├── auth_provider.dart
│   │       ├── emotion_provider.dart
│   │       └── user_provider.dart
│   │
│   └── shared/                      # 共享模块
│       ├── services/                # 服务类
│       │   └── notification_service.dart
│       └── extensions/              # 扩展方法
│           └── context_extensions.dart
│
├── scripts/                         # 构建脚本
│   ├── build_apk.ps1
│   └── rename_apk.ps1
│
├── assets/                          # 资源文件
│   ├── images/
│   ├── fonts/
│   └── animations/
│
└── pubspec.yaml                     # 依赖配置
```

### Python 后端 (backend/)

```
backend/
├── app/
│   ├── __init__.py
│   ├── main.py                      # FastAPI 入口
│   │
│   ├── api/                         # API 路由
│   │   ├── __init__.py
│   │   ├── deps.py                  # 依赖注入
│   │   └── v1/                      # API 版本
│   │       ├── __init__.py
│   │       ├── router.py            # 路由汇总
│   │       └── endpoints/
│   │           ├── auth.py
│   │           ├── users.py
│   │           ├── emotions.py
│   │           └── alerts.py
│   │
│   ├── core/                        # 核心配置
│   │   ├── __init__.py
│   │   ├── config.py                # 配置类
│   │   ├── security.py              # 安全相关 (JWT, 密码)
│   │   └── exceptions.py            # 异常定义
│   │
│   ├── models/                      # SQLAlchemy 模型
│   │   ├── __init__.py
│   │   ├── base.py                  # 基础模型类
│   │   ├── user.py
│   │   ├── emotion.py
│   │   └── alert.py
│   │
│   ├── schemas/                     # Pydantic 模型
│   │   ├── __init__.py
│   │   ├── user.py
│   │   ├── emotion.py
│   │   ├── alert.py
│   │   └── token.py
│   │
│   ├── services/                    # 业务逻辑
│   │   ├── __init__.py
│   │   ├── auth_service.py
│   │   ├── emotion_service.py
│   │   └── alert_service.py
│   │
│   ├── repositories/                # 数据访问
│   │   ├── __init__.py
│   │   ├── user_repo.py
│   │   └── emotion_repo.py
│   │
│   └── db/                          # 数据库
│       ├── __init__.py
│       ├── session.py               # 数据库会话
│       └── base.py                  # 基础类
│
├── alembic/                         # 数据库迁移
│   ├── versions/
│   └── env.py
│
├── tests/                           # 测试
│   ├── __init__.py
│   ├── conftest.py
│   ├── unit/
│   └── integration/
│
├── requirements.txt
├── pyproject.toml
└── .env
```

---

## 二、代码规范

### Flutter/Dart 规范

#### 状态管理 (Riverpod)

```dart
// ✅ 正确：使用 @riverpod 注解
part 'user_provider.g.dart';

@riverpod
class UserNotifier extends _$UserNotifier {
  @override
  User? build() => null;

  Future<void> loadUser(String userId) async {
    state = const AsyncValue.loading();
    state = await AsyncValue.guard(() async {
      final user = await _userRepository.getUser(userId);
      return user;
    });
  }
}

// ✅ 使用时
class UserProfilePage extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final userAsync = ref.watch(userNotifierProvider);
    return userAsync.when(
      data: (user) => UserCard(user: user),
      loading: () => const CircularProgressIndicator(),
      error: (err, stack) => ErrorWidget(err),
    );
  }
}
```

#### 数据模型 (freezed)

```dart
// ✅ 正确：使用 freezed
@freezed
class EmotionRecord with _$EmotionRecord {
  const factory EmotionRecord({
    required String id,
    required String userId,
    required EmotionType type,
    required int intensity,
    required DateTime createdAt,
    String? note,
  }) = _EmotionRecord;

  factory EmotionRecord.fromJson(Map<String, dynamic> json) =>
      _$EmotionRecordFromJson(json);
}
```

#### 路由 (go_router)

```dart
// 路由配置
final goRouter = GoRouter(
  routes: [
    GoRoute(
      path: '/login',
      builder: (context, state) => const LoginPage(),
    ),
    GoRoute(
      path: '/home',
      builder: (context, state) => const HomePage(),
    ),
    GoRoute(
      path: '/emotion/:id',
      builder: (context, state) {
        final id = state.pathParameters['id']!;
        return EmotionDetailPage(emotionId: id);
      },
    ),
  ],
);
```

### Python/FastAPI 规范

#### API 端点

```python
# ✅ 正确：使用依赖注入和类型注解
from fastapi import APIRouter, Depends, HTTPException
from sqlalchemy.ext.asyncio import AsyncSession

from app.api.deps import get_current_user, get_db
from app.schemas.emotion import EmotionCreate, EmotionResponse
from app.services.emotion_service import EmotionService

router = APIRouter()

@router.post("/checkin", response_model=EmotionResponse)
async def create_emotion_checkin(
    *,
    db: AsyncSession = Depends(get_db),
    current_user: User = Depends(get_current_user),
    emotion_in: EmotionCreate,
) -> EmotionResponse:
    """创建情绪打卡记录"""
    service = EmotionService(db)
    emotion = await service.create_checkin(
        user_id=current_user.id,
        emotion_data=emotion_in,
    )
    return EmotionResponse.from_orm(emotion)
```

#### Pydantic Schema

```python
# ✅ 正确：使用 Pydantic v2
from pydantic import BaseModel, Field, ConfigDict
from datetime import datetime
from enum import Enum

class EmotionType(str, Enum):
    HAPPY = "happy"
    SAD = "sad"
    ANGRY = "angry"
    ANXIOUS = "anxious"
    CALM = "calm"

class EmotionCreate(BaseModel):
    model_config = ConfigDict(str_strip_whitespace=True)

    type: EmotionType
    intensity: int = Field(..., ge=1, le=10, description="情绪强度 1-10")
    note: str | None = Field(None, max_length=500)

class EmotionResponse(BaseModel):
    id: str
    user_id: str
    type: EmotionType
    intensity: int
    note: str | None
    created_at: datetime
```

---

## 三、API 接口规范

### 基础配置

```yaml
基础URL: http://192.168.0.3:8000/api
认证方式: Bearer Token (JWT)
响应格式: JSON
字符编码: UTF-8
```

### 统一响应格式

```json
// 成功响应
{
  "code": 200,
  "message": "success",
  "data": { ... }
}

// 错误响应
{
  "code": 400,
  "message": "Validation error",
  "errors": [
    {
      "field": "intensity",
      "message": "must be between 1 and 10"
    }
  ]
}
```

### API 端点列表

| 方法 | 端点 | 描述 | 认证 |
|------|------|------|------|
| POST | /auth/login | 用户登录 | ❌ |
| POST | /auth/register | 用户注册 | ❌ |
| POST | /auth/refresh | 刷新 Token | ✅ |
| GET | /users/me | 获取当前用户 | ✅ |
| PUT | /users/me | 更新用户信息 | ✅ |
| POST | /emotions/checkin | 情绪打卡 | ✅ |
| GET | /emotions/history | 打卡历史 | ✅ |
| GET | /alerts | 预警列表 | ✅ (Counselor) |

---

## 四、构建与部署规范

### APK 构建流程

```powershell
# 完整构建（推荐）
.\mindcare_app\scripts\build_apk.ps1 -BuildType release -Status release

# 仅重命名
.\mindcare_app\scripts\rename_apk.ps1 -Status "dev"
```

### 构建产物命名

```
MindCareAI_{状态}_{版本号}_{日期}_{序号}.apk

示例:
MindCareAI_release_1.0.0_20250312_01.apk
MindCareAI_dev_0.0.1_20250312_02.apk
```

### 构建后处理（自动）

1. 检测构建完成
2. 调用 `version-manager` 智能体
3. 重命名 APK 文件
4. 备份原始文件到 `backup/`
5. 生成构建日志

---

## 五、环境变量

### Flutter 应用

| 变量名 | 说明 | 示例值 |
|--------|------|--------|
| API_BASE_URL | API 基础地址 | http://192.168.0.3:8000/api |
| GRADLE_USER_HOME | Gradle 目录 | D:\Gradle |

### Python 后端

| 变量名 | 说明 | 示例值 |
|--------|------|--------|
| DATABASE_URL | 数据库连接 | postgresql://user:pass@localhost/mindcare |
| SECRET_KEY | JWT 密钥 | (随机字符串) |
| ACCESS_TOKEN_EXPIRE | Token 过期时间 | 30 (分钟) |

---

## 六、开发工作流

### 日常开发

```bash
# 启动后端
cd backend
python -m uvicorn app.main:app --reload --host 0.0.0.0

# 启动 Flutter 应用
cd mindcare_app
flutter run
```

### 功能开发流程

1. 创建功能分支: `git checkout -b feature/new-feature`
2. 开发 + 编写测试
3. 提交代码: `git commit -m "feat(scope): description"`
4. 合并到 develop 分支
5. 测试验证
6. 合并到 main 分支
7. 构建发布版本

---

## 七、日志规范

### 更新日志

存储位置: `log/update/`
命名格式: `{类型}_{YYYYMMDD}_{序号}.md`

### 崩溃日志

存储位置: `log/crash/`
命名格式: `crash_{YYYYMMDD}_{HHMMSS}.md`

---

*此规则由 AI 规则体系 + MindCare AI 定制需求自动生成*
*修改定制需求后可重新生成*
