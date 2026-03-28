# 安全规范 (Security Guidelines)

> 基于 OWASP Top 10 的安全最佳实践

---

## 一、安全原则

### 1.1 核心安全原则

| 原则 | 说明 |
|------|------|
| **最小权限** | 只授予完成任务所需的最小权限 |
| **纵深防御** | 多层安全防护，单点失败不影响整体 |
| **安全默认** | 默认配置应该是安全的 |
| **失败安全** | 失败时不暴露敏感信息 |
| **不信任输入** | 所有外部输入都是不可信的 |

---

## 二、OWASP Top 10 防护

### SEC-01: 注入攻击防护

```
所有用户输入必须验证和清理
使用参数化查询，禁止字符串拼接 SQL
```

```dart
// [X] 错误：SQL 注入风险
final query = "SELECT * FROM users WHERE id = '$userId'";

// [OK] 正确：参数化查询
final query = "SELECT * FROM users WHERE id = ?";
final result = await db.query(query, [userId]);
```

```python
# [X] 错误：SQL 注入风险
query = f"SELECT * FROM users WHERE id = '{user_id}'"

# [OK] 正确：参数化查询
query = "SELECT * FROM users WHERE id = ?"
cursor.execute(query, (user_id,))
```

### SEC-02: 失效的身份认证

```
实施强密码策略
使用多因素认证
安全的会话管理
```

**密码策略**:
- 最小长度 8 位
- 包含大小写字母、数字、特殊字符
- 禁止常见弱密码
- 使用 bcrypt/argon2 哈希存储

```dart
// [OK] 正确：密码哈希
import 'package:bcrypt/bcrypt.dart';

String hashPassword(String password) {
  return BCrypt.hashpw(password, BCrypt.gensalt());
}

bool verifyPassword(String password, String hash) {
  return BCrypt.checkpw(password, hash);
}
```

### SEC-03: 敏感数据泄露防护

```
敏感数据加密存储
传输使用 HTTPS
禁止硬编码敏感信息
```

**敏感数据类型**:
- 密码、密钥、Token
- 个人身份信息 (PII)
- 财务信息
- 健康信息

```dart
// [X] 错误：硬编码密钥
const apiKey = 'sk-1234567890abcdef';

// [OK] 正确：从安全存储读取
final apiKey = await secureStorage.read(key: 'api_key');
```

**安全存储**:

| 数据类型 | 存储方式 |
|----------|----------|
| API 密钥 | flutter_secure_storage / 环境变量 |
| 用户 Token | flutter_secure_storage |
| 用户偏好 | shared_preferences |
| 临时数据 | 内存变量 |

### SEC-04: XML 外部实体 (XXE)

```
禁用 XML 外部实体处理
使用 JSON 替代 XML
```

### SEC-05: 失效的访问控制

```
实施基于角色的访问控制 (RBAC)
验证每个请求的权限
不信任客户端的权限判断
```

```dart
// [OK] 正确：服务端权限验证
@router.get('/admin/users')
Future<List<User>> getUsers(User user) async {
  if (!user.hasRole('admin')) {
    throw ForbiddenException('需要管理员权限');
  }
  return await userService.getAllUsers();
}
```

### SEC-06: 安全配置错误

```
禁用不必要的功能
使用安全配置
定期更新依赖
```

**检查清单**:
- [ ] 移除默认账户和密码
- [ ] 禁用调试模式
- [ ] 设置安全响应头
- [ ] 配置 CORS 策略
- [ ] 禁用目录列表

### SEC-07: 跨站脚本 (XSS)

```
对输出进行编码
使用 Content-Security-Policy
验证和清理用户输入
```

```dart
// [OK] 正确：输出编码
String sanitizeHtml(String input) {
  return htmlEscape.convert(input);
}
```

### SEC-08: 不安全的反序列化

```
验证反序列化数据
使用安全的序列化格式
实施完整性检查
```

### SEC-09: 使用含有已知漏洞的组件

```
定期扫描依赖漏洞
及时更新有漏洞的组件
移除未使用的依赖
```

```bash
# 检查 Flutter 依赖
flutter pub outdated

# 检查 npm 依赖
npm audit

# 检查 Python 依赖
pip-audit
```

### SEC-10: 日志与监控不足

```
记录安全相关事件
不记录敏感数据
设置告警机制
```

---

## 三、API 安全规范

### 3.1 认证机制

| 方式 | 适用场景 | 安全级别 |
|------|----------|----------|
| JWT | 无状态 API | 中-高 |
| OAuth 2.0 | 第三方授权 | 高 |
| API Key | 服务间调用 | 中 |
| Session | 传统 Web | 中 |

### 3.2 Token 安全

```yaml
JWT 配置:
  算法: RS256 或 HS256
  过期时间: 30分钟 (Access Token)
  刷新机制: Refresh Token (7天)
  存储: HttpOnly Cookie 或 Secure Storage
```

```dart
// [OK] 正确：JWT 验证
Future<User> verifyToken(String token) async {
  try {
    final payload = jwt.verify(token, secretKey);
    return await getUser(payload['userId']);
  } on JwtExpiredException {
    throw AuthenticationException('Token 已过期');
  } on JwtInvalidException {
    throw AuthenticationException('Token 无效');
  }
}
```

### 3.3 请求安全

```yaml
请求头:
  Authorization: Bearer <token>
  X-Request-ID: <uuid>
  Content-Type: application/json

响应头:
  X-Content-Type-Options: nosniff
  X-Frame-Options: DENY
  X-XSS-Protection: 1; mode=block
  Content-Security-Policy: default-src 'self'
```

---

## 四、移动端安全

### 4.1 本地存储安全

```dart
// [OK] 正确：使用安全存储
import 'package:flutter_secure_storage/flutter_secure_storage.dart';

final storage = FlutterSecureStorage();

// 存储敏感数据
await storage.write(key: 'auth_token', value: token);

// 读取敏感数据
final token = await storage.read(key: 'auth_token');

// 删除敏感数据
await storage.delete(key: 'auth_token');
```

### 4.2 网络安全

```dart
// [OK] 正确：配置 SSL Pinning
final dio = Dio();
(dio.httpClientAdapter as DefaultHttpClientAdapter).onHttpClientCreate = (client) {
  client.badCertificateCallback = (cert, host, port) => false;
  return client;
};
```

### 4.3 代码安全

- 代码混淆
- 反调试保护
- Root/越狱检测
- 证书校验

---

## 五、后端安全

### 5.1 环境变量管理

```bash
# .env.example (提交到 Git)
DATABASE_URL=postgresql://user:pass@localhost:5432/db
SECRET_KEY=your-secret-key
API_KEY=your-api-key

# .env (不提交到 Git)
DATABASE_URL=postgresql://real_user:real_pass@prod-host:5432/db
SECRET_KEY=real-secret-key
API_KEY=real-api-key
```

```python
# [OK] 正确：从环境变量读取
import os
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    database_url: str
    secret_key: str
    api_key: str

    class Config:
        env_file = '.env'

settings = Settings()
```

### 5.2 数据库安全

```yaml
数据库安全:
  连接:
    使用连接池
    最小权限账户
    加密连接 (SSL/TLS)

  查询:
    参数化查询
    避免动态 SQL
    限制返回数据量

  数据:
    敏感数据加密
    定期备份
    审计日志
```

---

## 六、安全检查清单

### 开发阶段

- [ ] 所有用户输入都经过验证
- [ ] 使用参数化查询
- [ ] 敏感数据加密存储
- [ ] 不硬编码敏感信息
- [ ] 实施适当的权限控制

### 测试阶段

- [ ] 安全测试覆盖主要场景
- [ ] 依赖漏洞扫描
- [ ] 渗透测试（如适用）

### 部署阶段

- [ ] 环境变量配置正确
- [ ] 关闭调试模式
- [ ] HTTPS 配置正确
- [ ] 日志不包含敏感信息

### 运维阶段

- [ ] 定期更新依赖
- [ ] 监控安全事件
- [ ] 定期安全审计
- [ ] 应急响应预案

---

## 七、安全事件处理

### 事件分类

| 级别 | 描述 | 响应时间 |
|------|------|----------|
| P0 | 数据泄露、系统入侵 | 立即 |
| P1 | 认证绕过、权限提升 | 4小时内 |
| P2 | 普通漏洞 | 24小时内 |
| P3 | 潜在风险 | 1周内 |

### 处理流程

```
发现 → 评估 → 抑制 → 根除 → 恢复 → 复盘
```

---

*安全是所有人的责任*
*每个开发者都应该了解并遵守这些安全规范*
