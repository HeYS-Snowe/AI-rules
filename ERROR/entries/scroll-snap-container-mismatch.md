---
title: 滚动容器错位——overflow 挂在子容器上而 JS 用 window.scrollTo，滚动静默失败
type: code
tags: [scroll-snap, css, javascript, hidden]
severity: high
status: resolved
first_seen: 2026-10-21
last_seen: 2026-10-21
occurrences: 1
---

## 错误描述

**发生时间**: 2026-10-21 课件项目骨架阶段
**错误内容**: 页面结构为 `#deck { height: 100vh; overflow-y: auto; scroll-snap-type: y mandatory }`（滚动发生在 #deck 元素内部），而翻页 JS 调用 `window.scrollTo(...)` 且监听 `window` 的 scroll 事件——滚动静默失败：`Pager.current` 状态已更新但页面纹丝不动，console 零报错。
**错误诱因**: CSS scroll-snap 的滚动容器与 JS 操纵的滚动器不是同一个对象。overflow 滚动属于 #deck，window.scrollY 恒为 0。

## 环境信息

- Chromium（Playwright headless）+ Windows 11
- 场景：全屏翻页课件（scroll-snap mandatory + JS 键盘翻页 + GSAP ScrollTrigger）
- 隐性错误：无报错；导航点状态正常更新造成"已翻页"的假象，对照截图才发现页面未滚

## 复现步骤

1. `#deck { height: 100vh; overflow-y: auto; scroll-snap-type: y mandatory }`
2. JS 执行 `window.scrollTo({ top: x })`
3. 页面不滚动，无任何报错

## 解决方案

### 方案一：滚动统一交给 window

1. `#deck` 去掉 `height/overflow`，只做层叠定位
2. `html { scroll-snap-type: y mandatory }`（根滚动器承载 snap）
3. JS 全部用 `window.scrollTo` / 监听 `window` scroll；ScrollTrigger 默认 scroller 即 window，无需额外配置
4. 注意子元素的 `offsetTop` 相对最近的定位父级（position:relative 的章节容器），取文档坐标用 `getBoundingClientRect().top + window.scrollY`

## 预防措施

- 引入 scroll-snap 时先确定唯一滚动器（推荐根滚动器），CSS 与 JS 引用保持同一对象
- 键盘/程序化翻页与 snap 共存时，不用 CSS `scroll-behavior: smooth`（会干扰 JS 精确滚动），smooth 交给 `scrollTo({ behavior: 'smooth' })`
