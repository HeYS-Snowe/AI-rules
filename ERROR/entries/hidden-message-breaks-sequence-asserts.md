---
title: 新增隐藏消息打破测试的角色序列断言
type: knowledge
tags: [tests, hidden-message, assertion, session-messages]
severity: medium
status: resolved
first_seen: 2026-08-14
last_seen: 2026-08-14
occurrences: 1
---

## 错误描述

**发生时间**: 2026-08-14 会话中（deepcode-cli 运行时上下文快照化）

**错误内容**: 在会话消息流中新增"隐藏 user 角色快照"（visible=false）后，14 个测试因 `messages.find((m) => m.role === "user")` 这类**角色查找**拿到快照而非真正的用户消息而失败——断言对象错位，快照内容被当成 /init 消息等。

**错误诱因**: 测试断言用"角色"定位消息，隐含假设"user 角色 = 用户可见输入"；引入不可见 user 消息后假设失效。生产代码本身行为正确（隐藏消息本就是设计）。

## 环境信息

- deepcode-cli 核心测试套件（312 个测试）
- 快照机制：`ensureRuntimeContextSnapshot` 追加隐藏 user 消息

## 复现步骤

1. 在会话消息流中新增隐藏 user 消息
2. 运行既有测试套件，观察所有 `find(role === "user")` 断言错位

## 解决方案

### 方案一：角色查找加 visibility 过滤

```ts
messages.find((m) => m.role === "user" && m.visible)
```

### 方案二：线上消息按内容特征定位（wire 层）

对构建后的 OpenAI 消息（无 visible 字段），用内容特征（如"不是快照前缀"）定位目标消息。

## 预防措施

- 引入"同角色多语义"的消息类型时，同步审计测试中的角色查找断言
- 消息流设计时明确各角色的"可见性语义"，测试统一用 `visible` 过滤
