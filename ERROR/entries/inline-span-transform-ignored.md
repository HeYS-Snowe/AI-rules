---
title: 非替换 inline 元素不可 transform——裸 span 上设置 transform 被静默忽略
type: code
tags: [css, transform, animation, transition, span]
severity: high
status: resolved
first_seen: 2026-09-09
last_seen: 2026-09-09
occurrences: 1
---

## 错误描述

**发生时间**: 2026-09-09（Trace demo 会话）
**错误内容**: 给轮带/翻板动画写 CSS transition，用户反馈"看不到翻页动画、数字瞬间变、显示滞后一步"。静态推理难以定位（transition/timeout 互猜），逐步排查发现：`.flip-inner` 是裸 `<span>`（默认 display:inline），其上设置的 `transform: translateY(-64px)` 作为计算样式存在但**不发生任何视觉效果**——CSS 规范规定非替换 inline 元素（如裸 span）**不可被变换**，transform 被浏览器静默忽略，无任何控制台警告。
**错误诱因**: CSS Transforms 规范：可变换元素 = 块级容器或原子级联 inline（inline-block/flex/grid 等）；普通 inline span 不满足；显式加 `display: inline-block` 即恢复可变换。此失败当时无任何报错（计算样式有值、渲染无效果），所以排查若只盯 JS/transition 代码会一直找不到。

## 环境信息

- Windows 11 / Chromium（demo 浏览器直开）
- 无构建依赖的独立 HTML demo（Electron 项目 Trace demo/mood-score-roll-demo.html）
- 上下文：odometer 数值滚动 demo 三变体——V2 翻页式（整串两行内嵌容器翻一格）

## 复现步骤

1. 写 `.flip-inner { transition: transform 260ms; }`，元素 `<span class="flip-inner">`（未设 display）
2. JS 改 `el.style.transform = 'translateY(-64px)'`（从 translateY(0) 变化）
3. 观察：内容瞬间切换，无过渡动画；transform 计算样式显示正确值

## 解决方案

### 方案一：给翻转容器加 `display: inline-block`（本次采用）

1. `.flip-inner { display: inline-block; transition: transform var(--t-roll) var(--ease-out); }`
2. 效果：翻页动画正常播放

## 预防措施

- 用 span 做动画承载容器时，**先确认 transformable**：inline-block/flex/grid/block 均行，裸 inline 不行
- "transform 计算值有值但视觉无效果" → 第一步查元素是否为非替换 inline
- 动画宿主元素建议统一写成 block/inline-block（或直接用 div），从结构上消灭此类静默失败
