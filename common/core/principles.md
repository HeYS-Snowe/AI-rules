# 核心原则 (Core Principles)

> 所有项目必须遵守的基础设计原则

---

## 一、设计原则 (SOLID)

### S - 单一职责原则 (Single Responsibility)

```
一个类/模块只负责一项职责
```

**实践要点**:
- 每个类只有一个变更理由
- 功能拆分到独立的模块
- 避免上帝类 (God Class)

```dart
// [X] 错误：一个类处理多种职责
class UserManager {
  void createUser() { }
  void sendEmail() { }
  void generateReport() { }
  void exportToExcel() { }
}

// [OK] 正确：职责分离
class UserRepository {
  void createUser() { }
}

class EmailService {
  void sendEmail() { }
}

class ReportGenerator {
  void generateReport() { }
  void exportToExcel() { }
}
```

### O - 开闭原则 (Open/Closed)

```
对扩展开放，对修改关闭
```

**实践要点**:
- 使用接口/抽象类定义契约
- 通过继承/组合扩展功能
- 避免直接修改已有代码

```dart
// [OK] 正确：通过扩展添加新功能
abstract class PaymentProcessor {
  void process(double amount);
}

class CreditCardProcessor implements PaymentProcessor {
  void process(double amount) { /* 信用卡处理 */ }
}

class AlipayProcessor implements PaymentProcessor {
  void process(double amount) { /* 支付宝处理 */ }
}

// 新增微信支付，不修改现有代码
class WechatProcessor implements PaymentProcessor {
  void process(double amount) { /* 微信处理 */ }
}
```

### L - 里氏替换原则 (Liskov Substitution)

```
子类必须能够替换其父类
```

**实践要点**:
- 子类不应破坏父类的行为契约
- 子类不应抛出父类没有的异常
- 子类方法的参数应该更宽松，返回值更严格

### I - 接口隔离原则 (Interface Segregation)

```
客户端不应依赖它不需要的接口
```

**实践要点**:
- 接口要小而专注
- 避免臃肿接口
- 使用多个专用接口替代一个通用接口

```dart
// [X] 错误：臃肿接口
interface Worker {
  void work();
  void eat();
  void sleep();
}

// [OK] 正确：接口隔离
interface Workable {
  void work();
}

interface Eatable {
  void eat();
}

class Robot implements Workable {
  void work() { }
  // Robot 不需要 eat
}
```

### D - 依赖倒置原则 (Dependency Inversion)

```
高层模块不应依赖低层模块，两者都应依赖抽象
```

**实践要点**:
- 使用依赖注入
- 面向接口编程
- 通过抽象解耦

---

## 二、简洁原则

### DRY - Don't Repeat Yourself

```
不要重复代码，抽象公共逻辑
```

**实践要点**:
- 提取重复代码到公共函数/类
- 使用继承/组合复用代码
- 保持单一事实来源 (Single Source of Truth)

```dart
// [X] 错误：重复的验证逻辑
void validateEmail(String email) {
  if (!email.contains('@')) throw Exception('Invalid email');
}

void createUser(String email) {
  if (!email.contains('@')) throw Exception('Invalid email');
  // ...
}

void updateUser(String email) {
  if (!email.contains('@')) throw Exception('Invalid email');
  // ...
}

// [OK] 正确：提取公共逻辑
class Validator {
  static void validateEmail(String email) {
    if (!email.contains('@')) throw Exception('Invalid email');
  }
}

void createUser(String email) {
  Validator.validateEmail(email);
  // ...
}
```

### KISS - Keep It Simple, Stupid

```
保持简单，避免过度设计
```

**实践要点**:
- 选择最简单的解决方案
- 避免不必要的抽象
- 代码应该易于理解

```dart
// [X] 错误：过度设计
abstract class AbstractValidatorFactoryBuilder {
  Validator create(ValidationContext context);
}

class EmailValidatorFactory extends AbstractValidatorFactoryBuilder {
  Validator create(ValidationContext context) {
    return EmailValidator(context.getConfig());
  }
}

// [OK] 正确：保持简单
class Validator {
  static bool isValidEmail(String email) {
    return email.contains('@') && email.contains('.');
  }
}
```

### YAGNI - You Aren't Gonna Need It

```
不做当前不需要的功能
```

**实践要点**:
- 只实现当前需求
- 不要为"将来可能需要"而编码
- 避免过度抽象

---

## 三、代码质量原则

### 3.1 可读性优先

```
代码是写给人看的，顺便给机器执行
```

**实践要点**:
- 使用有意义的命名
- 保持函数简短
- 添加必要的注释

### 3.2 防御性编程

```
假设一切可能出错的地方都会出错
```

**实践要点**:
- 验证所有输入
- 处理所有可能的异常
- 使用断言检查不变量

```dart
// [OK] 正确：防御性编程
Future<User> getUser(String userId) async {
  if (userId.isEmpty) {
    throw ArgumentError('userId cannot be empty');
  }

  final user = await _repository.findById(userId);
  if (user == null) {
    throw NotFoundException('User not found: $userId');
  }

  return user;
}
```

### 3.3 错误处理

```
优雅地处理错误，提供有意义的反馈
```

**实践要点**:
- 使用特定的异常类型
- 提供错误上下文信息
- 区分可恢复和不可恢复错误

---

## 四、模块化原则

### 4.1 高内聚

```
模块内部的元素应该紧密相关
```

**实践要点**:
- 相关功能放在同一模块
- 模块有清晰的边界
- 模块内部的变更不影响外部

### 4.2 低耦合

```
模块之间应该尽量独立
```

**实践要点**:
- 通过接口通信
- 减少模块间的依赖
- 使用事件/消息解耦

---

## 五、可维护性原则

### 5.1 代码度量标准

| 指标 | 标准 | 说明 |
|------|------|------|
| 单文件行数 | ≤ 500 行 | 超过则考虑拆分 |
| 函数行数 | ≤ 50 行 | 超过则重构 |
| 嵌套深度 | ≤ 4 层 | 超过则提取函数 |
| 函数参数 | ≤ 5 个 | 超过则使用对象 |
| 圈复杂度 | ≤ 10 | 超过则简化逻辑 |

### 5.2 命名规范

| 类型 | 命名风格 | 示例 |
|------|----------|------|
| 类/类型 | PascalCase | `UserService` |
| 函数/方法 | camelCase | `getUserById` |
| 变量 | camelCase | `userName` |
| 常量 | UPPER_SNAKE_CASE | `MAX_RETRY_COUNT` |
| 私有成员 | _前缀 | `_privateMethod` |
| 文件名 | 语言约定 | Dart: snake_case, TS: camelCase |

### 5.3 注释规范

```dart
/// 用户服务类
///
/// 提供用户相关的业务逻辑处理，包括注册、登录、信息更新等。
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
  /// 返回用户信息，如果用户不存在则返回 null
  ///
  /// 抛出 [ArgumentError] 如果 userId 为空
  Future<User?> getUser(String userId) async {
    // 实现...
  }
}
```

---

## 六、检查清单

### 开发前检查

- [ ] 理解需求，明确边界
- [ ] 设计方案符合 SOLID 原则
- [ ] 避免过度设计 (YAGNI)
- [ ] 复用现有代码 (DRY)

### 开发中检查

- [ ] 代码易于理解 (KISS)
- [ ] 命名有意义
- [ ] 函数职责单一
- [ ] 添加必要注释

### 提交前检查

- [ ] 代码符合度量标准
- [ ] 无重复代码
- [ ] 错误处理完善
- [ ] 测试覆盖充分

---

*核心原则是所有项目必须遵守的基础规范*
*违反这些原则需要明确的理由和文档说明*
