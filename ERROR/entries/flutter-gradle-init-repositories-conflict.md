---
title: Flutter included Gradle build conflicts with a global Gradle init repository injection
type: environment
tags: [flutter, gradle, init-script, repositories, windows]
severity: medium
status: open
first_seen: 2026-09-26
last_seen: 2026-09-26
occurrences: 1
---

## 错误描述

**发生时间**: 2026-09-26 会话中（Jeenith Android Debug 构建）

**错误内容**: `flutter build apk --debug --no-pub` 在解析 `dev.flutter.flutter-plugin-loader` 时失败，提示 `Build was configured to prefer settings repositories over project repositories but repository 'maven' was added by settings file 'settings.gradle.kts'`。

**错误诱因**: 用户级 `C:\Users\aaa\.gradle\init.gradle` 对所有 Gradle 项目执行 `allprojects.repositories` / `buildscript.repositories` 注入；Flutter SDK 的 included Gradle build 在 `packages/flutter_tools/gradle/settings.gradle.kts` 使用 `RepositoriesMode.FAIL_ON_PROJECT_REPOS`，二者冲突。Jeenith 的 `mobile/android/settings.gradle.kts` 与主工作区基线一致，不是本次 Flutter/Dart 改动引入。

## 环境信息

- Windows 10.0.26340.9577
- Flutter 3.41.6
- Gradle Wrapper 8.14
- Jeenith Android Debug build；AGP 8.11.1
- 用户级 Gradle 初始化脚本：`C:\Users\aaa\.gradle\init.gradle`

## 复现步骤

1. 在 Jeenith 的 `mobile/` 目录运行 `flutter build apk --debug --no-pub`。
2. Gradle 在 `settings.gradle.kts` 第 20 行应用 Flutter plugin loader 时报告 settings/project repository mode 冲突。

## 解决方案

暂无已验证的解决方案。不要为绕过用户级初始化脚本而直接修改项目仓库配置；先确认用户是否需要调整全局 Gradle 初始化策略，或在隔离的 `GRADLE_USER_HOME` 中验证构建。

## 预防措施

- Flutter Android 构建失败并指向 `flutter-plugin-loader` 时，检查 Gradle 用户级 `init.gradle` 是否向所有项目注入仓库。
- 先比较项目 `settings.gradle` 与基线，再区分项目配置问题和用户级 Gradle 初始化脚本冲突。
