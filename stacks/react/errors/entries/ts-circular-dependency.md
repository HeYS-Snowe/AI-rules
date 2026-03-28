---
title: TypeScript 循环依赖导致运行时错误
type: runtime
tech_stack: TypeScript + Vite + React
tags: [typescript, import, circular-dependency, vite]
severity: medium
first_seen: 2026-03-17
last_seen: 2026-03-17
occurrences: 1
references:
  - https://stackoverflow.com/questions/38841469
---

## 错误描述

在开发过程中遇到运行时错误：

```
Uncaught ReferenceError: Cannot access 'SomeClass' before initialization
```

或者：

```
Warning: Invalid hook call. Hooks can only be called inside of the body of a function component
```

## 复现步骤

1. 文件 A 导入文件 B 的某个导出
2. 文件 B 导入文件 A 的某个导出
3. 运行项目时出现上述错误

## 解决方案

### 方案一：重构代码结构（推荐）

将共享的逻辑/类型提取到第三个文件 C：
- A 和 B 都导入 C
- 避免直接相互导入

### 方案二：延迟导入

将导入语句移到函数内部：

```typescript
// 错误：顶层导入
import { someFunc } from './moduleB'

// 正确：函数内导入
function myFunc() {
  const { someFunc } = require('./moduleB')
  // 或使用动态 import
  import('./moduleB').then(module => module.someFunc())
}
```

### 方案三：重新导出

创建一个 `index.ts` 统一管理导出。

## 原因分析

循环依赖导致模块在初始化完成前就被访问。JavaScript/TypeScript 的模块系统在遇到循环引用时，会返回未完全初始化的导出。

## 预防措施

1. 使用工具检测循环依赖：`madge --circular ./src`
2. 遵循单向数据流原则
3. 将共享类型/工具放到独立的 `shared/` 或 `common/` 目录
4. 定期运行 ESLint 的 `import/no-cycle` 规则检查
