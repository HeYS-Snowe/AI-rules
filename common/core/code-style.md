# 代码风格规范 (Code Style Guidelines)

> 统一的代码风格，提升代码可读性和一致性

---

## 一、通用命名规范

### 1.1 命名风格对照表

| 类型 | 风格 | 示例 | 说明 |
|------|------|------|------|
| 类/类型 | PascalCase | `UserService` | 首字母大写驼峰 |
| 函数/方法 | camelCase | `getUserById` | 首字母小写驼峰 |
| 变量 | camelCase | `userName` | 首字母小写驼峰 |
| 常量 | UPPER_SNAKE_CASE | `MAX_RETRY_COUNT` | 全大写下划线 |
| 私有成员 | _前缀 + camelCase | `_internalState` | 下划线开头 |
| 文件名 | 语言约定 | 见下表 | 按语言规范 |

### 1.2 文件命名约定

| 语言 | 命名风格 | 示例 |
|------|----------|------|
| Dart | snake_case | `user_service.dart` |
| TypeScript | camelCase | `userService.ts` |
| Python | snake_case | `user_service.py` |
| Go | snake_case | `user_service.go` |
| Java | PascalCase | `UserService.java` |
| Rust | snake_case | `user_service.rs` |

### 1.3 有意义的命名

```dart
// ❌ 错误：无意义命名
var d; // 消逝的时间
var list; // 什么列表？
var flag; // 什么标志？

// ✅ 正确：有意义的命名
var elapsedTimeInDays;
var activeUsers;
var isPaymentProcessed;
```

### 1.4 布尔值命名

```dart
// ✅ 正确：布尔值命名前缀
bool isValid;
bool hasPermission;
bool canEdit;
bool shouldRefresh;
bool isActive;
bool containsElement;
```

---

## 二、代码格式规范

### 2.1 缩进与空格

```yaml
缩进:
  空格数: 2 或 4 (项目统一)
  使用空格: 推荐
  使用 Tab: 可接受

空格:
  运算符两侧: a + b
  逗号后: [1, 2, 3]
  冒号后: {key: value}
  函数名与括号间: func() // 无空格
```

### 2.2 行长度限制

```yaml
最大行长度:
  代码: 100 字符 (推荐) / 120 字符 (最大)
  注释: 80 字符
  字符串: 可适当放宽
```

### 2.3 空行规范

```dart
// ✅ 正确：空行使用
import 'package:flutter/material.dart';  // import 块后空一行

class UserService {                       // 类定义前空一行
  final UserRepository _repository;

  UserService(this._repository);          // 构造函数后空一行

  Future<User> getUser(String id) async { // 方法之间空一行
    // ...
  }

  Future<void> updateUser(User user) async {
    // ...
  }
}
```

### 2.4 大括号风格

```dart
// ✅ K&R 风格 (推荐)
if (condition) {
  doSomething();
} else {
  doOther();
}

// ✅ 函数定义
void processData() {
  // ...
}
```

### 2.5 禁止使用 Emoji

```yaml
适用范围:
  代码文件: 禁止在代码中添加 emoji
  注释: 禁止在注释中使用 emoji
  提交信息: 禁止在 git commit message 中使用 emoji
  文档: 业务文档和代码文档中禁止使用 emoji

例外情况:
  用户界面字符串: 允许 (如按钮文本、提示信息等由产品需求决定)
  配置文件: 允许 (如 emoji 映射配置)
  测试数据: 允许 (如测试包含 emoji 的输入场景)
```

```dart
// ❌ 错误：注释中使用 emoji
// TODO: 修复这个问题 🐛
// ✅ 正确：添加用户验证功能 ✅

// ✅ 正确：纯文本注释
// TODO: 修复这个问题
// 已完成：添加用户验证功能

// ❌ 错误：代码中包含 emoji
const String successMessage = '操作成功 🎉';

// ✅ 正确：用户界面字符串可根据产品需求决定
const String successMessage = '操作成功';  // 由产品决定是否包含 emoji
```

```python
# ❌ 错误：git commit message 中使用 emoji
git commit -m "✨ 新增用户登录功能"

# ✅ 正确：纯文本 commit message
git commit -m "feat: 新增用户登录功能"
```

---

## 三、注释规范

### 3.1 文档注释

```dart
/// 用户服务类
///
/// 提供用户相关的业务逻辑处理。
///
/// 示例:
/// ```dart
/// final userService = UserService(repository);
/// final user = await userService.getUser('123');
/// ```
class UserService {
  /// 获取用户信息
  ///
  /// [userId] 用户唯一标识
  ///
  /// 返回 [User] 对象，如果用户不存在则返回 null。
  ///
  /// 抛出 [ArgumentError] 如果 userId 为空。
  /// 抛出 [NetworkException] 如果网络请求失败。
  Future<User?> getUser(String userId) async {
    if (userId.isEmpty) {
      throw ArgumentError('userId cannot be empty');
    }
    return await _repository.findById(userId);
  }
}
```

### 3.2 行内注释

```dart
// ✅ 正确：解释为什么
// 使用指数退避策略避免服务器过载
await Future.delayed(Duration(seconds: pow(2, retryCount)));

// ✅ 正确：解释复杂逻辑
// 计算加权平均值，权重随时间衰减
final weightedSum = values.asMap().entries.fold(0.0, (sum, entry) {
  final weight = 1 / (entry.key + 1);
  return sum + entry.value * weight;
});

// ❌ 错误：描述做了什么（代码已经说明了）
// 设置用户名
userName = 'John';
```

### 3.3 TODO 注释

```dart
// TODO(author): 简短描述
// FIXME(author): 需要修复的问题
// HACK(author): 临时解决方案，需要后续优化
// NOTE: 重要说明

// 示例
// TODO(zhang): 添加缓存支持
// FIXME(li): 处理边界情况
// HACK(wang): 临时禁用验证，等待后端更新
```

### 3.4 注释比例

```yaml
注释原则:
  代码自解释: 优先通过好的命名和结构
  必须注释: 复杂算法、业务规则、非显而易见的设计
  避免: 显而易见的注释、注释掉的代码

推荐比例:
  文档注释: 所有公开 API
  行内注释: 复杂逻辑处
  总体: 约 10-20% 代码行数
```

---

## 四、代码组织规范

### 4.1 文件结构

```dart
// 1. 导入语句
import 'package:flutter/material.dart';
import 'package:riverpod/riverpod.dart';

// 2. 导出 (如有)
export 'src/user_service_base.dart';

// 3. 常量
const int kMaxRetryCount = 3;
const String kDefaultLocale = 'zh-CN';

// 4. 类型定义
typedef UserCallback = void Function(User user);

// 5. 主类/主函数
class UserService {
  // 5.1 静态成员
  static const String _defaultRole = 'user';

  // 5.2 实例变量
  final UserRepository _repository;
  bool _isInitialized = false;

  // 5.3 构造函数
  UserService(this._repository);

  // 5.4 公开方法
  Future<User> getUser(String id) async { }

  // 5.5 私有方法
  Future<void> _initialize() async { }
}

// 6. 辅助类 (如有)
class _UserCache { }
```

### 4.2 类成员顺序

```dart
class Example {
  // 1. 静态常量
  static const String constant = 'value';

  // 2. 静态变量
  static int staticVar = 0;

  // 3. 实例变量
  final String name;
  int _counter = 0;

  // 4. 构造函数
  Example(this.name);

  // 5. 初始化列表
  Example.withCounter(this.name, this._counter);

  // 6. 公开方法
  void publicMethod() { }

  // 7. 私有方法
  void _privateMethod() { }
}
```

### 4.3 代码度量标准

| 指标 | 标准 | 说明 |
|------|------|------|
| 单文件行数 | ≤ 500 行 | 超过则拆分 |
| 函数行数 | ≤ 50 行 | 超过则重构 |
| 嵌套深度 | ≤ 4 层 | 超过则提取函数 |
| 函数参数 | ≤ 5 个 | 超过则使用配置对象 |
| 圈复杂度 | ≤ 10 | 超过则简化 |

---

## 五、语言特定规范

### 5.1 Dart/Flutter

```dart
// ✅ 使用 final/const
final user = User(name: 'John');
const defaultTimeout = Duration(seconds: 30);

// ✅ 使用级联操作
final user = User()
  ..name = 'John'
  ..age = 30
  ..email = 'john@example.com';

// ✅ 使用空安全
String? nullableName;
String nonNullName = nullableName ?? 'Unknown';

// ✅ 使用扩展方法
extension StringExtensions on String {
  bool get isValidEmail => contains('@') && contains('.');
}

// ✅ 使用命名参数
void createUser({
  required String name,
  required String email,
  int age = 0,
}) { }
```

### 5.2 TypeScript

```typescript
// ✅ 使用类型注解
interface User {
  id: string;
  name: string;
  email: string;
}

function getUser(id: string): Promise<User> {
  // ...
}

// ✅ 使用可选链
const email = user?.profile?.email;

// ✅ 使用空值合并
const name = user?.name ?? 'Unknown';

// ✅ 使用 const 断言
const config = {
  apiUrl: '/api',
  timeout: 5000,
} as const;
```

### 5.3 Python

```python
# ✅ 使用类型注解
from typing import Optional, List

def get_user(user_id: str) -> Optional[User]:
    ...

def get_users() -> List[User]:
    ...

# ✅ 使用 dataclass
from dataclasses import dataclass

@dataclass
class User:
    id: str
    name: str
    email: str

# ✅ 使用上下文管理器
with open('file.txt', 'r') as f:
    content = f.read()

# ✅ 使用列表推导
active_users = [u for u in users if u.is_active]
```

---

## 六、最佳实践

### 6.1 避免魔法数字

```dart
// ❌ 错误：魔法数字
if (status == 200) { }
await Future.delayed(Duration(milliseconds: 5000));

// ✅ 正确：使用常量
const HttpStatus successStatus = HttpStatus.ok;
const Duration apiTimeout = Duration(seconds: 5);

if (status == successStatus) { }
await Future.delayed(apiTimeout);
```

### 6.2 早返回

```dart
// ❌ 错误：深层嵌套
void process(User? user) {
  if (user != null) {
    if (user.isActive) {
      if (user.hasPermission) {
        doSomething(user);
      }
    }
  }
}

// ✅ 正确：早返回
void process(User? user) {
  if (user == null) return;
  if (!user.isActive) return;
  if (!user.hasPermission) return;

  doSomething(user);
}
```

### 6.3 函数参数对象化

```dart
// ❌ 错误：参数过多
void createUser(
  String name,
  String email,
  String phone,
  String address,
  int age,
  String role,
) { }

// ✅ 正确：使用配置对象
class CreateUserParams {
  final String name;
  final String email;
  final String? phone;
  final String? address;
  final int? age;
  final String role;

  CreateUserParams({
    required this.name,
    required this.email,
    required this.role,
    this.phone,
    this.address,
    this.age,
  });
}

void createUser(CreateUserParams params) { }
```

---

## 七、代码审查检查清单

### 格式检查

- [ ] 缩进一致
- [ ] 空行合理
- [ ] 行长度符合限制
- [ ] 大括号风格统一
- [ ] 无 emoji (代码、注释、提交信息)

### 命名检查

- [ ] 变量名有意义
- [ ] 命名风格一致
- [ ] 无缩写/简写
- [ ] 布尔值使用正确前缀

### 注释检查

- [ ] 公开 API 有文档注释
- [ ] 复杂逻辑有解释
- [ ] 无多余注释
- [ ] TODO 有责任人

### 结构检查

- [ ] 函数长度合理
- [ ] 嵌套深度合理
- [ ] 参数数量合理
- [ ] 文件行数合理

---

*代码风格一致性是团队协作的基础*
*遵循规范，让代码更易读、易维护*
