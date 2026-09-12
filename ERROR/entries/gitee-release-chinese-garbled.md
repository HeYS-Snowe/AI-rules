---
title: Gitee API 中文乱码三坑——发布必读（令牌用户名/参数名/文件传输/附件上限）
type: knowledge
tags: [gitee, release, api, curl, encoding, 乱码]
severity: high
status: resolved
first_seen: 2026-09-09
last_seen: 2026-09-09
occurrences: 1
---

## 错误描述

**发生时间**: 2026-09-09（Trace 双平台发布）
**错误内容**: Gitee 推送与 Release 创建连续踩坑：

1. **HTTPS 推送认证**：`https://{username}:{token}@gitee.com` 形式报 "The token username invalid"——Gitee 私人令牌须**作为 username 本身**（URL `https://{token}@gitee.com/repo.git`，无密码）；更稳的是直接用 SSH（`git@gitee.com`）。
2. **API 参数名**：创建 Release 时 `target_branch` 无效——正确是 **`target_commitish`**（否则 500 "target_commitish is missing"）；且 Gitee 不支持 PUT 更新 release（405），只能 DELETE + 重建。
3. **中文乱码**：`curl --data-urlencode "name=中文"`（命令行参数）与 `--data-urlencode name@file` 在 Windows 下均出现中文折叠（服务器端存成 `��`）——文件为 UTF-8 也不能保证（curl 8.x 对 @file 走本地编码/代理层折损）。**可靠路径**：python 生成 UTF-8 JSON（ensure_ascii=False）+ `curl -d @payload.json -H "Content-Type: application/json"`（-d @file 原始字节透传）。
4. **附件上限**：Gitee Release 附件单文件 **100 MB** 上限；超过则降级"正文内嵌外链"方案（本项目实测 117.69MB 被拒）。

**错误诱因**: Gitee API 文档的认证/参数差异 + Windows 中文入参链路（bash→curl→HTTP 编码）不可靠；发布类操作须以"网页实测"为最终依据（API 读取路径自带 GBK 干扰，曾两次误判乱码方向）。

## 环境信息

- Windows 11 / Git Bash / curl 8.x / Gitee API v5
- 项目：溯源 Trace 双平台首发（GitHub + Gitee）

## 复现步骤

1. 命令行长中文参数发 Gitee API → 返回 name/body 乱码
2. 500MB 附件上传 → "文件大小已超出限制：100 MB"

## 解决方案

### 方案一：全链路文件传输 + JSON（本次最终路径）

1. python 构造 payload（ensure_ascii=False）写 `.json` 文件
2. `curl -X POST -H "Content-Type: application/json" -d @payload.json <url>`
3. 以 Gitee 网页显示核对；API 侧读取用文件法（python open utf-8），勿用管道+GBK 终端

## 预防措施

- 对 Gitee 一切含中文的 API 请求：JSON 文件 + `-d @file`，NEVER 命令行内联中文
- 发布检查单：令牌推送用 SSH；创建→GET 验证→附件（<100MB）；"网页实测"为裁决源
