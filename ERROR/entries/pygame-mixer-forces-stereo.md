---
title: pygame mixer 初始化强制立体声，get_raw 解交错才能当单声道
type: environment
tags: [pygame, audio, mixer, stereo, wav]
severity: medium
status: resolved
first_seen: 2026-09-01
last_seen: 2026-09-01
occurrences: 1
---

## 错误描述

**发生时间**: 2026-09-01 12:44
**错误内容**: pygame.mixer.pre_init(16000, -16, 1) + mixer.init(16000) 后，
get_init() 实际返回 (16000, -16, 2)——音频设备强制 stereo。
Sound(io.BytesIO(mp3)).get_raw() 因此返回 **L/R 交错**的 16k stereo 数据；
若按 mono 写 WAV（setnchannels(1)），字节流被重新解释成单声道，
音频变成半速/失真，喂给 ASR 后识别结果完全错误（"启动系统"被识成"许多是ppppo"）。

**错误诱因**: SDL/SDL_mixer 在多数 Windows 音频设备上不支持单声道播放，
即使 pre_init 明确请求 1 通道也会 fallback 到 2 通道；代码据此假设了 mono。

## 环境信息

- Windows 11 Pro 10.0.26340，Python 3.11.5
- pygame 2.6.1 (SDL 2.28.4)
- 背景：edge-tts 合成 MP3 → pygame 解码 → 生成 16k mono WAV 给 funasr 做链路测试

## 复现步骤

1. `pygame.mixer.pre_init(16000, -16, 1); pygame.mixer.init(16000)`
2. `print(pygame.mixer.get_init())` → 打印 `(16000, -16, 2)`
3. `pygame.mixer.Sound(io.BytesIO(mp3_bytes)).get_raw()` → 得到的字节是 stereo 交错

## 解决方案

### 方案一：去交错（推荐，不依赖设备）

```python
raw = np.frombuffer(sound.get_raw(), dtype=np.int16)
mono = raw.reshape(-1, 2).mean(axis=1).astype(np.int16)   # 或取 [:, 0]
```

## 预防措施

1. 用 `pygame.mixer.get_init()` 实测初始化格式，NEVER 假设 pre_init 参数即实际格式
2. 文本->WAV 转换链路出错时先检查采样率/声道数是否"看着对、实际错"
3. 生成测试音频后，先跑一次命令识别（TTS 生成已知句子 → 识别回文本）验证链路，
   别直接相信"音频字节到了"——失真音频识别结果差得离谱但不会报错
