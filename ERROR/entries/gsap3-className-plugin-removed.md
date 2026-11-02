---
title: GSAP 3 已移除 className 特殊属性，className 补间把 class 写成字面量
type: knowledge
tags: [gsap, className, css, hidden]
severity: medium
status: resolved
first_seen: 2026-10-21
last_seen: 2026-10-21
occurrences: 1
---

## 错误描述

**发生时间**: 2026-10-21 课件项目幕 7（代码逐行点亮动画）
**错误内容**: `gsap.to(lines, { className: '+=on', stagger: 0.06 })` 执行后，所有目标元素的 class 属性被字面写成 `"+=on"`（`<span class="+=on">`），原 class `ln` 丢失，对应 CSS（`display: block` 逐行布局）全部失配，行内容挤成一行。console 无任何报错。
**错误诱因**: GSAP 2 时代 `className` 是可补间的特殊属性；GSAP 3 移除了该插件（官方建议改用 CSS 过渡或 classList），`className` 被当作普通属性按字符串赋值。

## 环境信息

- GSAP 3.12.5（UMD）
- 场景：纯静态课件页，`pre.code` 内 `<span class="ln">` 逐行加 `.on` 类点亮
- 隐性错误：静态检查/运行时全无报错，仅视觉行为异常（截图走查才发现）

## 复现步骤

1. `gsap.to(el, { className: '+=active' })`（GSAP 3 环境）
2. 检查 `el.className` → 得到字面字符串 `"+=active"`

## 解决方案

### 方案一：显式 classList + delayedCall 做 stagger

1. `els.forEach((el, i) => gsap.delayedCall(0.06 * i, () => el.classList.add('on')))`
2. 类切换配合 CSS transition 完成动效

## 预防措施

- GSAP 3 中 NEVER 使用 `className` 补间属性；批量加类用 classList + delayedCall/stagger 回调
- 依赖库大版本升级后，旧版特有 API 先查迁移指南再使用
