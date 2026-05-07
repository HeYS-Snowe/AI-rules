# MindCare AI - 项目定制规则

> **版本**: 1.0.0
> **生成日期**: 2026-03-12
> **项目类型**: 全栈项目（移动端 + 后端）
> **技术栈**: Flutter / FastAPI / PostgreSQL / Riverpod

---

## 规则体系引用

### 基础规则（自动生效）

- **规则入口**: `D:\Code\.Rules\main.md`
- **规则优先级**: 本文件(定制规则) > 预设配置 > 核心规则 > 默认行为

### CRITICAL 禁令（绝对禁止）

- NEVER 硬编码 API 密钥、密码、Token 等敏感信息
- NEVER 使用字符串拼接 SQL
- NEVER 跳过用户输入验证
- NEVER 在未读取文件的情况下修改文件
- NEVER 编造不确定的信息
- NEVER 添加未被明确要求的功能

---

## 一、项目概述

### 1.1 项目信息

| 属性 | 值 |
|------|-----|
| 项目名称 | MindCare AI - 校园心理健康预警系统 |
| 项目类型 | 全栈项目（移动端 + 后端） |
| 版本 | 1.0.0 |
| 描述 | 为高校提供心理健康监测、预警和干预服务的校园心理健康系统 |

### 1.2 技术栈

| 层级 | 技术 | 版本 |
|------|------|------|
| 前端框架 | Flutter | 3.5+ |
| 状态管理 | Riverpod | 3.x |
| 路由管理 | go_router | 17.x |
| 本地数据库 | Drift | 2.x |
| 网络请求 | Dio | 5.x |
| 后端框架 | FastAPI | 0.109+ |
| 数据库 | PostgreSQL | 15+ |
| ORM | SQLAlchemy | 2.x |

---

## 二、目录结构规范

### 2.1 项目根目录

```
MindCare_AI/
├── mindcare_app/              # Flutter 移动端应用
│   ├── lib/
│   │   ├── main.dart
│   │   ├── app.dart
│   │   ├── core/
│   │   ├── data/
│   │   ├── domain/
│   │   ├── presentation/
│   │   └── shared/
│   ├── scripts/
│   ├── assets/
│   └── pubspec.yaml
│
├── backend/                   # Python FastAPI 后端
│   ├── app/
│   │   ├── main.py
│   │   ├── api/
│   │   ├── core/
│   │   ├── models/
│   │   ├── schemas/
│   │   ├── services/
│   │   └── db/
│   ├── alembic/
│   ├── tests/
│   └── requirements.txt
│
├── admin/                     # 管理后台 (可选)
├── docs/                      # 项目文档
├── log/                       # 日志目录
│   ├── update/
│   └── crash/
└── README.md
```

### 2.2 Flutter 应用结构

```
mindcare_app/lib/
├── main.dart                  # 应用入口
├── app.dart                   # 应用根组件
│
├── core/                      # 核心功能
│   ├── constants/
│   │   ├── app_constants.dart
│   │   └── api_constants.dart
│   ├── theme/
│   │   ├── app_theme.dart
│   │   └── colors.dart
│   ├── router/
│   │   └── app_router.dart
│   ├── errors/
│   │   ├── exceptions.dart
│   │   └── failures.dart
│   └── utils/
│       ├── validators.dart
│       └── formatters.dart
│
├── data/                      # 数据层
│   ├── models/                # 数据模型 (freezed)
│   │   ├── user_model.dart
│   │   ├── emotion_model.dart
│   │   └── alert_model.dart
│   ├── repositories/          # 仓库实现
│   │   ├── auth_repository_impl.dart
│   │   └── emotion_repository_impl.dart
│   └── datasources/
│       ├── local/
│       │   ├── database.dart
│       │   └── secure_storage.dart
│       └── remote/
│           ├── api_client.dart
│           └── auth_api.dart
│
├── domain/                    # 领域层
│   ├── entities/
│   │   ├── user.dart
│   │   └── emotion_record.dart
│   └── repositories/
│       ├── auth_repository.dart
│       └── emotion_repository.dart
│
├── presentation/              # 展示层
│   ├── pages/
│   │   ├── auth/
│   │   ├── home/
│   │   ├── emotion/
│   │   └── profile/
│   ├── widgets/
│   │   ├── common/
│   │   └── emotion/
│   └── providers/
│       ├── auth_provider.dart
│       ├── emotion_provider.dart
│       └── user_provider.dart
│
└── shared/
    ├── services/
    └── extensions/
```

### 2.3 后端结构

```
backend/app/
├── main.py                    # FastAPI 入口
│
├── api/
│   ├── deps.py                # 依赖注入
│   └── v1/
│       ├── router.py
│       └── endpoints/
│           ├── auth.py
│           ├── users.py
│           ├── emotions.py
│           └── alerts.py
│
├── core/
│   ├── config.py
│   ├── security.py
│   └── exceptions.py
│
├── models/                    # SQLAlchemy 模型
│   ├── base.py
│   ├── user.py
│   ├── emotion.py
│   └── alert.py
│
├── schemas/                   # Pydantic 模型
│   ├── user.py
│   ├── emotion.py
│   ├── alert.py
│   └── token.py
│
├── services/
│   ├── auth_service.py
│   ├── emotion_service.py
│   └── alert_service.py
│
├── repositories/
│   ├── user_repo.py
│   └── emotion_repo.py
│
└── db/
    ├── session.py
    └── base.py
```

---

## 三、代码规范

### 3.1 命名规范

```yaml
Flutter/Dart:
  类名: PascalCase           # UserService, UserProfile
  变量: camelCase            # userName, itemCount
  常量: camelCase            # maxRetryCount
  私有成员: _前缀            # _privateMethod
  文件名: snake_case         # user_service.dart

Python:
  类名: PascalCase           # UserService
  函数: snake_case           # get_user_by_id
  变量: snake_case           # user_name
  常量: UPPER_SNAKE_CASE     # MAX_RETRY_COUNT
  文件名: snake_case         # user_service.py
```

### 3.2 Flutter 状态管理 (Riverpod)

```dart
// [OK] 正确：使用 @riverpod 注解
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

// [OK] 使用时
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

### 3.3 数据模型 (freezed)

```dart
// [OK] 正确：使用 freezed
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

### 3.4 Python FastAPI 规范

```python
# [OK] 正确：使用依赖注入和类型注解
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
    return EmotionResponse.model_validate(emotion)
```

---

## 四、API 接口规范

### 4.1 基础配置

```yaml
基础URL: http://192.168.0.3:8000/api
认证方式: Bearer Token (JWT)
响应格式: JSON
字符编码: UTF-8
```

### 4.2 统一响应格式

```json
// 成功响应
{
  "code": 200,
  "message": "success",
  "data": { }
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

### 4.3 API 端点列表

| 方法 | 端点 | 描述 | 认证 |
|------|------|------|------|
| POST | /auth/login | 用户登录 | [X] |
| POST | /auth/register | 用户注册 | [X] |
| POST | /auth/refresh | 刷新 Token | [OK] |
| GET | /users/me | 获取当前用户 | [OK] |
| PUT | /users/me | 更新用户信息 | [OK] |
| POST | /emotions/checkin | 情绪打卡 | [OK] |
| GET | /emotions/history | 打卡历史 | [OK] |
| GET | /alerts | 预警列表 | [OK] (Counselor) |

---

## 五、构建与部署规范

### 5.1 构建流程

```powershell
# Flutter 构建
cd mindcare_app
flutter build apk --release

# 后端启动
cd backend
python -m uvicorn app.main:app --reload --host 0.0.0.0
```

### 5.2 构建产物命名

```
MindCareAI_{状态}_{版本号}_{日期}_{构建序号}.apk

版本号格式: 主版本.次版本.修订号.构建序号（如 1.0.0.1）

示例:
MindCareAI_release_1.0.0.1_20250312_01.apk
MindCareAI_dev_0.0.1.2_20250312_02.apk
```

### 5.3 构建后处理（自动）

1. 检测构建完成
2. 调用 `version-manager` 智能体
3. 重命名 APK 文件
4. 备份原始文件到 `backup/`
5. 生成构建日志

---

## 六、环境配置

### 6.1 Flutter 环境变量

| 变量名 | 说明 | 示例值 |
|--------|------|--------|
| API_BASE_URL | API 基础地址 | http://192.168.0.3:8000/api |
| GRADLE_USER_HOME | Gradle 目录 | D:\Gradle |

### 6.2 Python 环境变量

| 变量名 | 说明 | 示例值 |
|--------|------|--------|
| DATABASE_URL | 数据库连接 | postgresql://user:pass@localhost/mindcare |
| SECRET_KEY | JWT 密钥 | (随机字符串) |
| ACCESS_TOKEN_EXPIRE | Token 过期时间 | 30 (分钟) |

---

## 七、开发工作流

### 7.1 日常开发

```bash
# 启动后端
cd backend
python -m uvicorn app.main:app --reload --host 0.0.0.0

# 启动 Flutter 应用
cd mindcare_app
flutter run
```

### 7.2 功能开发流程

1. 创建功能分支: `git checkout -b feature/new-feature`
2. 开发 + 编写测试
3. 提交代码: `git commit -m "feat(scope): description"`
4. 合并到 develop 分支
5. 测试验证
6. 合并到 main 分支
7. 构建发布版本

---

## 八、测试规范

### 8.1 Flutter 测试

```dart
// 单元测试示例
void main() {
  group('EmotionService', () {
    test('should create emotion checkin successfully', () async {
      // Arrange
      final mockRepo = MockEmotionRepository();
      final service = EmotionService(mockRepo);

      // Act
      final result = await service.createCheckin(
        userId: 'user-123',
        type: EmotionType.happy,
        intensity: 8,
      );

      // Assert
      expect(result, isNotNull);
      expect(result.type, equals(EmotionType.happy));
    });
  });
}
```

### 8.2 Python 测试

```python
# 单元测试示例
import pytest
from httpx import AsyncClient
from app.main import app

@pytest.mark.asyncio
async def test_create_emotion_checkin():
    async with AsyncClient(app=app, base_url="http://test") as client:
        response = await client.post(
            "/api/v1/emotions/checkin",
            json={
                "type": "happy",
                "intensity": 8,
            },
            headers={"Authorization": f"Bearer {test_token}"},
        )

    assert response.status_code == 200
    data = response.json()
    assert data["type"] == "happy"
```

---

## 九、日志规范

### 9.1 更新日志

存储位置: `log/update/`
命名格式: `{类型}_{YYYYMMDD}_{序号}.md`

类型:
- feature: 新功能
- fix: Bug 修复
- refactor: 重构
- config: 配置变更

### 9.2 崩溃日志

存储位置: `log/crash/`
命名格式: `crash_{YYYYMMDD}_{HHMMSS}.md`

---

## 十、检查清单

### 开发前

- [ ] 理解需求，明确验收标准
- [ ] 查看项目结构，遵循现有模式
- [ ] 确认技术栈和依赖版本

### 编码时

- [ ] 遵循命名规范
- [ ] 使用 Riverpod 管理状态
- [ ] 使用 freezed 定义模型
- [ ] 处理错误和边界情况
- [ ] 不硬编码敏感信息

### 提交前

- [ ] 代码自检（格式、注释、逻辑）
- [ ] 运行测试确保通过
- [ ] 编写/更新测试用例
- [ ] 更新相关文档
- [ ] 使用规范的提交信息

### 构建前

- [ ] 检查环境变量配置
- [ ] 确认版本号正确
- [ ] 执行完整构建测试
- [ ] 准备构建日志

---

## 变更记录

| 版本 | 日期 | 变更内容 |
|------|------|----------|
| 1.0.0 | 2026-03-12 | 初始版本 |

---

*此规则由 AI 规则体系 + MindCare AI 定制需求自动生成*
*修改定制需求后可重新生成*
