---
title: 复制 venv 后用 Scripts\pip.exe 安装，包落到原始 venv
type: dependency
tags: [venv, pip, python, windows, copied-venv]
severity: medium
status: resolved
first_seen: 2026-09-01
last_seen: 2026-09-01
occurrences: 1
---

## 错误描述

**发生时间**: 2026-09-01 12:25-12:28
**错误内容**: 把整个项目（含 .venv，目录复制非 junction）复制到新位置后，
用 `新位置\.venv\Scripts\pip.exe install xxx` 安装 funasr/torch，
装完目标 venv 的 python 却 import 不到；pip 的提示（notice 里的路径）
指向**原位置**（源项目）的 venv——安装实际全部写进了原始 venv
（源 venv 的 site-packages 出现今天时间戳的新包，复制出来那份没有）。
现象：`新venv\Scripts\python -c "import funasr"` 报 ModuleNotFoundError，
但 `pip show funasr` 显示已安装。

**错误诱因**: venv 创建时 pyvenv.cfg 记录了原始创建路径（command 字段），
脚本 shim（pip.exe/python.exe 等）在复制后的 venv 里行为不透明——
sys.prefix 跟随实际 exe 位置，但 Scripts 启动器对"被复制"的 venv
解析出了源位置的解释器并运行其 pip，安装由此落到源 venv。

## 环境信息

- Windows 11 Pro 10.0.26340，Python 3.11.5（venv 经由目录整树复制产生）
- 项目：语音助手本地测试（复制于 D:\Code\Project\intelligent-laboratory-system\Demo\voice-assistant）

## 复现步骤

1. 将带 .venv 的项目目录整体复制到新路径
2. `cd 新路径 && .venv\Scripts\pip install funasr`
3. `新路径\.venv\Scripts\python -c "import funasr"` → ModuleNotFoundError
4. 检查源项目 `\.venv\Lib\site-packages` → 新包已落地（时间戳=今天）

## 解决方案

### 方案一（推荐）：用解释器 -m pip 显式运行

```bash
"新路径\.venv\Scripts\python.exe" -m pip install funasr
```

`python -m pip` 的 sys.prefix 与实际 exe 位置强绑定（可用
`python -c "import sys; print(sys.prefix)"` 先自证），不再经 Scripts shim 转发。

## 预防措施

1. 对复制产生的 venv：先验证 `python -c "import sys; print(sys.prefix)"` 指向本位置
2. 装包一律用 `python -m pip`，避免直接调用 Scripts\pip.exe
3. 安装入口用 `pip install -r requirements.txt` 全量重装比"塞进复制 venv"更干净；
   复制出来的标准建议是重建 venv（`python -m venv`），不要复用旧 venv 目录
