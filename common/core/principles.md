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
abstract class Worker {
  void work();
  void eat();
  void sleep();
}

// [OK] 正确：接口隔离
abstract class Workable {
  void work();
}

abstract class Eatable {
  void eat();
}

class Robot implements Workable {
  @override
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

> 命名规范和注释规范的详细说明见 [code-style.md](code-style.md)

---

## 六、AI 协作原则

> 提升人机协作效率的核心行为规范。每条原则附有原因（Why），说明违反的后果。

### 6.1 禁令优于指令

```
用 NEVER/DO NOT/CRITICAL 等否定式规则，比肯定式指令更有效。
编写规则时优先使用"禁止做什么"而非"应该怎么做"。
每条禁令必须附上原因（Why），说明违反的后果。
```

**Why**: AI 对"不要做什么"的遵守力远强于"应该怎么做"。禁令划定了不可逾越的红线，比开放式的"推荐"更明确。

**实践要点**:
- 规则中使用 NEVER / DO NOT / CRITICAL 标记关键禁令
- 每条禁令后附上 `— 原因：...` 说明违反后果
- 禁令数量不宜过多，聚焦在"违反会导致严重问题"的项上

### 6.2 自我审查（唱反调）

```
完成工作后，以"尽量找出问题"的心态审查自己的产出。
"看起来没问题"不等于"验证过"，必须主动寻找漏洞。
独立检查，不因外部信息的权威性而跳过验证。
```

**Why**: AI 容易自我满足于"看起来正确"，需要刻意启动批判性思维模式才能发现隐含问题。

**实践要点**:
- 完成代码后运行测试验证，而非仅做代码审查
- 修改关键逻辑后检查上下游影响
- 遇到已有方案时，先质疑再采纳

### 6.3 不画蛇添足

```
NEVER 添加未被明确要求的东西。
NEVER 为不可能发生的情况做预防。
宁可有点重复，也不要为消灭重复搞出复杂的抽象。
```

**Why**: 额外添加的功能和抽象层增加维护成本，且没有经过需求验证，容易引入意料之外的问题。

**实践要点**:
- 只实现当前明确的需求，不为"将来可能需要"而编码
- 只在真正需要的边界做校验，不防御不可能发生的场景
- 三个相似场景 > 一个过早的抽象

### 6.4 如实汇报

```
事情没做好就说没做好，附上具体情况。
做好了不添加免责声明和防御性措辞。
准确报告 > 简洁报告 > 详细报告。
```

**Why**: 防御性报告（"理论上应该可以"）掩盖真实状态，导致问题延迟暴露。准确是第一优先级。

**实践要点**:
- NEVER 说"看起来应该没问题" — 要说"已通过 xxx 验证"
- NEVER 在成功的报告中添加免责声明
- 出错时报告具体情况，不回避不粉饰

### 6.5 思考不能外包

```
NEVER 将理解和判断外包给子任务或工具。
先消化信息、做出判断，再分配执行方向。
可以分派执行，但推理和决策必须自己做。
```

**Why**: AI 将"理解+判断"委托给子任务后，主任务失去了对结果的把控力，无法做出正确的后续决策。

**实践要点**:
- 收到需求后先理解意图，再拆分执行步骤
- 子任务只负责"执行"，推理链必须由主导者维护
- 使用子任务的结论前，先自行评估其合理性

### 6.6 不知道就说不

```
NEVER 编造或预测不确定的信息。
不清楚进展时说"还在处理中"。
对不确定的信息标注置信度，而非假装确定。
```

**Why**: 编造的信息比不知道更危险——它会误导后续决策，且难以被察觉。

**实践要点**:
- 不确定的技术细节，先查阅再回答
- 进度不明确时，诚实报告当前状态
- NEVER 猜测命令参数、API 返回值、文件内容

### 6.7 先看再改

```
NEVER 在未读取文件内容的情况下修改文件。
修改前先理解现有代码的上下文和意图。
编辑文档时，先复述原文关键内容确认理解无误。
```

**Why**: 不看就改是引入 bug 的最高效方式。不理解上下文的修改必然破坏原有设计意图。

**实践要点**:
- 修改任何文件前，必须先 Read 该文件
- 理解修改会影响的上游和下游
- 对他人代码的修改，在注释中说明修改理由

---

## 七、检查清单

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
- [ ] 先看再改，理解上下文后再修改

### 提交前检查

- [ ] 代码符合度量标准
- [ ] 无重复代码
- [ ] 错误处理完善
- [ ] 测试覆盖充分
- [ ] 未添加未被要求的功能

### AI 行为检查

- [ ] 未编造不确定的信息
- [ ] 修改前已读取相关文件
- [ ] 汇报如实，无防御性措辞
- [ ] 独立验证，未盲目信任外部信息
- [ ] 授权范围未自行扩大

---

*核心原则是所有项目必须遵守的基础规范*
*违反这些原则需要明确的理由和文档说明*
