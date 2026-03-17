# 🔧 错误知识库系统

> 记录编程中遇到的错误和解决方案，避免重复踩坑

---

## 📁 目录结构

```
errors/
├── README.md              # 本文档
├── ERRORS_INDEX.md        # 索引文件，快速查找
└── entries/               # 错误记录条目
    ├── ts-import-cycle.md
    ├── vite-env-variables.md
    └── ...
```

---

## 📋 字段定义

### 必填字段

| 字段            | 类型     | 说明                                                                       |
| ------------- | ------ | ------------------------------------------------------------------------ |
| `title`       | string | 错误名称，简洁明了                                                                |
| `type`        | enum   | 错误类型：`compile` / `runtime` / `logic` / `config` / `dependency` / `other` |
| `description` | text   | 错误的详细描述，包括报错信息                                                           |
| `solution`    | text   | 解决方案，步骤清晰                                                                |
| `tech_stack`  | string | 技术栈，如 `React + Vite + TypeScript`                                        |
| `tags`        | array  | 关键词标签，方便搜索                                                               |

### 可选字段

| 字段             | 类型     | 说明                                          |
| -------------- | ------ | ------------------------------------------- |
| `root_cause`   | text   | 错误根本原因分析                                    |
| `reproduce`    | text   | 复现步骤                                        |
| `severity`     | enum   | 严重程度：`critical` / `high` / `medium` / `low` |
| `occurrences`  | number | 发生次数                                        |
| `first_seen`   | date   | 首次遇到日期                                      |
| `last_seen`    | date   | 最近遇到日期                                      |
| `prevention`   | text   | 预防措施，下次如何避免                                 |
| `references`   | array  | 参考链接（StackOverflow、GitHub issue 等）          |
| `project_type` | string | 项目类型（Web、CLI、Mobile 等）                      |

---

## 📝 记录模板

每个错误保存为单独的 Markdown 文件，命名规则：`{简短标识}.md`

```markdown
---
title: 错误名称
type: compile | runtime | logic | config | dependency | other
tech_stack: 技术栈
tags: [tag1, tag2, tag3]
severity: critical | high | medium | low
first_seen: YYYY-MM-DD
last_seen: YYYY-MM-DD
occurrences: 1
references:
  - https://...
---

## 错误描述

<!-- 报错信息、截图、现象等 -->

## 复现步骤

1. 步骤一
2. 步骤二
3. ...

## 解决方案

1. 步骤一
2. 步骤二
3. ...

## 原因分析

<!-- 为什么会出现这个错误 -->

## 预防措施

<!-- 下次怎么避免 -->
```

---

## 🔄 存储策略

### 无感存储（默认）

AI 在以下情况 **静默记录**，无需用户指令：

| 级别 | 触发条件 | 示例 |
|------|----------|------|
| **严重** | 构建失败、运行时崩溃 | 类型错误、空指针 |
| **常见** | 配置问题、依赖冲突 | 版本不兼容、环境变量 |
| **小坑** | API 行为异常、边界情况 | 返回 undefined、时区问题 |
| **微扰** | 轻微困扰、调试发现 | console 输出意外、小配置遗漏 |

### 记录原则

1. **广覆盖** - 不限于严重错误，小问题也记录
2. **静默执行** - 解决后自动记录，不干扰用户
3. **去重优先** - 遇到相同问题，更新已有记录而非新建
4. **按需汇报** - 仅在用户查询时展示知识库内容

### 自动记录流程

1. AI 解决问题后判断是否值得记录
2. 检查 `entries/` 是否有相似记录
3. 新建或更新记录文件
4. 更新 `ERRORS_INDEX.md` 索引
5. 不主动告知用户（无感）

### 用户查询

- "查一下 xxx 相关的错误"
- "之前遇到过 xxx 吗？"
- "有什么关于 xxx 的坑？"

AI 会搜索知识库并返回相关记录。

---

## 📚 索引文件说明

`ERRORS_INDEX.md` 用于快速查找，按以下方式组织：

```markdown
# 错误索引

## 按类型

### 编译错误 (compile)
- [ts-import-cycle](entries/ts-import-cycle.md) - TypeScript 循环引用

### 运行时错误 (runtime)
- ...

### 配置错误 (config)
- ...

## 按技术栈

### TypeScript
- ...

### React
- ...

## 按标签

#eslint
- ...

#vite
- ...
```

---

## 💡 最佳实践

1. **及时记录** - 解决后立即记录，别拖延
2. **标签规范** - 使用一致的标签命名（小写、连字符分隔）
3. **保持更新** - 再次遇到时更新发生次数
4. **链接优先** - 有参考链接尽量附上
5. **原因优先** - 理解"为什么"比"怎么解决"更重要
