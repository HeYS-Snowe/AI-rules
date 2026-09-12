---
title: 迁移脚本必须幂等——数据已被迁移后重跑会二次嵌套目录
type: knowledge
tags: [migration, idempotency, backup, script, node]
severity: high
status: resolved
first_seen: 2026-09-09
last_seen: 2026-09-09
occurrences: 1
---

## 错误描述

**发生时间**: 2026-09-09（Trace 项目 `D:\Desktop\Plan` md→plan 迁移）
**错误内容**: 迁移脚本先备份、后迁移，首次运行成功（8/8 md → 计划目录）。**第二次启动 dev 验证时误重跑**——脚本扫描到的 md 已位于"计划目录内"，但目标目录判定只查 `targetDir/plan.json`（此时是*新建的下一层*目录，不存在）→ 把每个计划又向下嵌套一层：`Daily_Plan-2026.../Daily_Plan-2026.../plan.json`，原计划节点内容被下移并重复。目录句柄被 chokidar 占用还导致后续 `mv` 被拒（Permission denied）。
**错误诱因**: ① 幂等守卫只看"目标位置"，不看"当前位置"；② 一个 md 被移入目录后，路径本身会变化——"扫描-迁移"循环对已迁移数据不收敛。

## 环境信息

- OS: Windows 11；Node 24（esbuild bundle 运行 TS 脚本）
- 场景: 一次性数据迁移脚本（node + fs + 复用业务转换函数）

## 复现步骤

1. 迁移脚本：扫描所有 `.md` → 在所在目录建同名目录 + 写 plan.json + rename md 进目录
2. 首跑成功；不改任何数据，原样重跑
3. 第二次扫描找到"计划目录内的 md"→ 在其下再建一层同名目录 → 迁移"成功"→ 树结构出现双层嵌套

## 解决方案

```javascript
// 幂等守卫：md 若已在同名计划目录内（当前位置含 plan.json 或目录名 == base）→ 跳过
const insideOwnPlan = (await exists(join(m.dir, 'plan.json'))) || m.dir.endsWith(m.base)
if (insideOwnPlan || (await exists(join(targetDir, 'plan.json')))) { skipped.push(...); continue }
```

配套：① 先全量备份（`fs.cp` 到库外兄弟路径），出错时 `mv` 出错结构留档还原备份，NEVER 直接删除；② 自检断言（plan.json 可读 + 含组件 + md 已移入）；③ 输出树轮廓（`[计划]/[文件夹]` 标注）供肉眼核验。

## 预防措施

- **动真实数据的一次性脚本：默认写成幂等**——重跑必须零副作用（用"当前位置"反推是否已完成，而非只看目标）
- 脚本执行前先备份（库外路径）；错误现场改名留档再还原，不覆盖不删除
- 脚本自身带结构自检与目录树输出，验证不做猜测
- Windows 下被 chokidar/监视器持有句柄的目录无法 rename——先停监视进程再操作目录级迁移
