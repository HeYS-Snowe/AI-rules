# Flutter 国内网络环境配置规则

> **版本**: v1.0.0
> **更新日期**: 2026-03-28
> **适用范围**: 所有 Flutter 项目（国内开发环境）
> **前置条件**: 已运行 `init_flutter_env.ps1` 全局初始化

---

## 概述

本规则定义了 Flutter 项目在中国大陆网络环境下的基础设施配置标准。
采用**全局初始化 + 项目级配置**两层架构，确保 AI 在开发过程中能够自动处理镜像相关问题。

### 架构

```
全局层（一次配置，永久生效）                    项目层（每个新项目一次）
+----------------------------+    +----------------------------+
| Gradle 全局 init.d 镜像     |    | Windows CMake sqlite3 引用  |
| Flutter SDK 环境变量镜像    |    | Web flutter_bootstrap.js   |
| sqlite3 全局本地缓存        |    | Drift 条件导入文件          |
| flutter_bootstrap.js 模板   |    +----------------------------+
+----------------------------+
```

---

## 一、AI 必须遵守的行为规范

### 1.1 新项目创建后

当 AI 创建新的 Flutter 项目或检测到项目未配置国内镜像时，**必须自动执行以下操作**：

**步骤 1**：检查全局配置是否就绪

```powershell
# 检查 Gradle 全局镜像
Test-Path "$env:GRADLE_USER_HOME\init.d\china-mirrors.init.gradle.kts"

# 检查 sqlite3 全局缓存
Test-Path "$env:GRADLE_USER_HOME\flutter-deps\sqlite3\sqlite-autoconf-3520000\sqlite3.c"

# 检查 Flutter SDK 镜像环境变量
[System.Environment]::GetEnvironmentVariable("PUB_HOSTED_URL", "User")
```

**步骤 2**：如果全局配置不存在，提示用户运行全局初始化

```
检测到国内镜像全局配置未就绪。请先运行全局初始化脚本：
powershell -ExecutionPolicy Bypass -File {脚本路径}/init_flutter_env.ps1
该脚本只需运行一次，所有 Flutter 项目将自动受益。
```

**步骤 3**：全局配置就绪后，自动运行项目级配置

```powershell
powershell -ExecutionPolicy Bypass -File setup_project.ps1
```

### 1.2 构建失败时自动诊断

当构建命令（`flutter build`、`flutter pub get`、`build_runner`）失败时，AI **必须**：

1. 检查错误输出是否包含以下关键词：
   
   - `Could not resolve` / `Connection refused` / `Connection timed out`
   - `Failed to load` / `HTTP error` / `SSL handshake failed`
   - `pub get failed` / `Gradle build failed`
   - `SocketException` / `HandshakeException`

2. 如果是网络问题，按以下顺序排查：
   
   - 检查 `~/.gradle/init.d/china-mirrors.init.gradle.kts` 是否存在
   - 检查 `PUB_HOSTED_URL` 和 `FLUTTER_STORAGE_BASE_URL` 环境变量是否已设置
   - 检查 Windows 构建的 sqlite3 全局缓存是否就绪
   - 检查 Web 构建的 `flutter_bootstrap.js` 是否配置了 CanvasKit CDN

3. 如果配置缺失，自动修复或提示用户运行对应脚本

### 1.3 代码修改时的镜像感知

AI 在修改以下文件时，**必须**保持已有的镜像配置不变：

| 文件                                            | 配置内容                                                      | 严禁删除 |
| --------------------------------------------- | --------------------------------------------------------- | ---- |
| `windows/CMakeLists.txt`                      | `FETCHCONTENT_SOURCE_DIR_SQLITE3` 全局缓存引用                  | 是    |
| `web/flutter_bootstrap.js`                    | `canvasKitBaseUrl` CDN 配置                                 | 是    |
| `lib/data/database/app_database.dart`         | Drift 条件导入（`dart.library.io` / `dart.library.js_interop`） | 是    |
| `lib/data/database/database_connection*.dart` | native / web / stub 三个文件                                  | 是    |

### 1.4 禁止行为

- 禁止在项目级 `settings.gradle.kts` / `build.gradle.kts` 中添加 Maven 镜像（已由全局 init.d 处理）
- 禁止在项目中下载 sqlite3 源码到 `build_deps/`（已由全局缓存处理）
- 禁止修改 `windows/flutter/ephemeral/` 下的任何文件（会被 `flutter pub get` 覆盖）
- 禁止在 `index.html` 中使用 `window.flutterConfiguration`（已弃用）

---

## 二、全局配置详情（AI 参考）

### 2.1 Gradle 全局 init.d

**文件位置**: `~/.gradle/init.d/china-mirrors.init.gradle.kts`

```kotlin
fun RepositoryHandler.aliyunMirrors() {
    maven { url = uri("https://maven.aliyun.com/repository/google") }
    maven { url = uri("https://maven.aliyun.com/repository/central") }
    maven { url = uri("https://maven.aliyun.com/repository/public") }
    maven { url = uri("https://maven.aliyun.com/repository/gradle-plugin") }
    maven { url = uri("https://maven.aliyun.com/repository/jcenter") }
}

settingsEvaluated {
    pluginManagement {
        repositories { aliyunMirrors() }
    }
}

allprojects {
    repositories { aliyunMirrors() }
}
```

**作用**: 所有 Gradle 项目自动使用阿里云 Maven 镜像，无需项目级配置。

### 2.2 Flutter SDK 环境变量

| 变量名                        | 值                               |
| -------------------------- | ------------------------------- |
| `PUB_HOSTED_URL`           | `https://pub.flutter-io.cn`     |
| `FLUTTER_STORAGE_BASE_URL` | `https://storage.flutter-io.cn` |

**作用**: `flutter pub get` / `flutter pub run` 自动使用国内镜像。

### 2.3 sqlite3 全局缓存

**位置**: `~/.gradle/flutter-deps/sqlite3/sqlite-autoconf-3520000/`

**作用**: 所有 Flutter 项目的 Windows/Linux 构建共用，避免从 sqlite.org 下载。

### 2.4 flutter_bootstrap.js 全局模板

**位置**: `~/.gradle/flutter-deps/templates/web/flutter_bootstrap.js`

**作用**: 新项目 Web 构建时自动复制，使用 jsdelivr CDN 加速 CanvasKit。

---

## 三、项目级配置详情（AI 参考）

### 3.1 Windows CMake sqlite3 配置

在 `windows/CMakeLists.txt` 中添加（位于 `add_subdirectory` 之前）：

```cmake
# China mirror: use cached sqlite3 source from global Gradle deps
if(DEFINED ENV{GRADLE_USER_HOME})
  set(_SQLITE3_GLOBAL_DIR "$ENV{GRADLE_USER_HOME}/flutter-deps/sqlite3/sqlite-autoconf-3520000")
else()
  set(_SQLITE3_GLOBAL_DIR "$ENV{USERPROFILE}/.gradle/flutter-deps/sqlite3/sqlite-autoconf-3520000")
endif()

if(EXISTS "${_SQLITE3_GLOBAL_DIR}/sqlite3.c")
  set(FETCHCONTENT_SOURCE_DIR_SQLITE3 "${_SQLITE3_GLOBAL_DIR}" CACHE PATH "Local sqlite3 source" FORCE)
elseif(EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/../build_deps/sqlite-autoconf-3520000/sqlite3.c")
  set(FETCHCONTENT_SOURCE_DIR_SQLITE3 "${CMAKE_CURRENT_SOURCE_DIR}/../build_deps/sqlite-autoconf-3520000" CACHE PATH "Local sqlite3 source" FORCE)
endif()
```

### 3.2 Web flutter_bootstrap.js

`web/flutter_bootstrap.js` 使用 CanvasKit CDN：

```javascript
{{flutter_js}}
{{flutter_build_config}}

_flutter.loader.loadEntrypoint({
  serviceWorker: {
    serviceWorkerVersion: {{flutter_service_worker_version}},
  },
  onEntrypointLoaded: async function(engineInitializer) {
    let appRunner = await engineInitializer.initializeEngine({
      canvasKitBaseUrl: "https://cdn.jsdelivr.net/npm/canvaskit-wasm/bin/",
    });
    await appRunner.runApp();
  }
});
```

### 3.3 Drift Web 条件导入

数据库类使用条件导入实现跨平台：

**`lib/data/database/database_connection.dart`** (Native):

```dart
import 'dart:io';
import 'package:drift/native.dart';
import 'package:drift/drift.dart';
import 'package:path/path.dart' as p;
import 'package:path_provider/path_provider.dart';

LazyDatabase openConnection() {
  return LazyDatabase(() async {
    final dbFolder = await getApplicationDocumentsDirectory();
    final file = File(p.join(dbFolder.path, 'app.db'));
    return NativeDatabase.createInBackground(file);
  });
}
```

**`lib/data/database/database_connection_web.dart`** (Web):

```dart
import 'package:drift/drift.dart';
import 'package:drift/wasm.dart';

LazyDatabase openConnection() {
  return LazyDatabase(() async {
    final db = await WasmDatabase.open(
      databaseName: 'app',
      sqlite3Uri: Uri.parse('https://cdn.jsdelivr.net/npm/sqlite-wasm-umd@0.1.18/sqlite3.wasm'),
      driftWorkerUri: Uri.parse('https://cdn.jsdelivr.net/npm/sqlite-wasm-umd@0.1.18/sqlite3.wasm'),
    );
    return db.resolvedExecutor;
  });
}
```

**`lib/data/database/database_connection_stub.dart`** (Stub):

```dart
import 'package:drift/drift.dart';

LazyDatabase openConnection() {
  throw UnsupportedError('No database implementation for this platform');
}
```

**`lib/data/database/app_database.dart`** 中使用条件导入：

```dart
import 'database_connection_stub.dart'
    if (dart.library.io) 'database_connection.dart'
    if (dart.library.js_interop) 'database_connection_web.dart';
```

---

## 四、脚本工具

### 4.1 init_flutter_env.ps1（全局初始化，只运行一次）

```powershell
powershell -ExecutionPolicy Bypass -File scripts/init_flutter_env.ps1
```

功能：

- 创建 `~/.gradle/init.d/china-mirrors.init.gradle.kts`
- 设置 `PUB_HOSTED_URL` / `FLUTTER_STORAGE_BASE_URL` 环境变量
- 下载 sqlite3 源码到全局缓存
- 创建 flutter_bootstrap.js 全局模板

### 4.2 setup_project.ps1（项目级配置，每个新项目一次）

```powershell
powershell -ExecutionPolicy Bypass -File setup_project.ps1

# 如果使用 Drift，指定数据库名
powershell -ExecutionPolicy Bypass -File setup_project.ps1 -DbName myapp
```

功能：

- 修改 `windows/CMakeLists.txt` 引用全局 sqlite3 缓存
- 复制 `flutter_bootstrap.js` 模板到项目
- 创建 Drift 条件导入文件（如检测到 Drift 项目）

---

## 五、下载失败重试策略

### 5.1 自动重试规则

| 重试次数  | 等待时间 | 动作      |
| ----- | ---- | ------- |
| 第 1 次 | 10 秒 | 自动重试原命令 |
| 第 2 次 | 20 秒 | 自动重试原命令 |
| 第 3 次 | 30 秒 | 自动重试原命令 |

### 5.2 重试全部失败后的排查

1. 检查全局镜像配置：`~/.gradle/init.d/china-mirrors.init.gradle.kts`
2. 检查环境变量：`PUB_HOSTED_URL` / `FLUTTER_STORAGE_BASE_URL`
3. 清理缓存后重试：`flutter pub cache clean && flutter clean && flutter pub get`
4. 检查 VPN 代理设置（仅访问 GitHub 等未镜像资源时可能需要）

---

## 六、平台支持矩阵

| 平台      | 需要项目级配置                           | 需要全局配置                  | 镜像问题                     |
| ------- | --------------------------------- | ----------------------- | ------------------------ |
| Android | 无                                 | Gradle init.d + 环境变量    | Maven 仓库                 |
| Windows | CMake sqlite3                     | sqlite3 全局缓存            | sqlite.org               |
| Web     | flutter_bootstrap.js + Drift 条件导入 | flutter_bootstrap.js 模板 | CanvasKit / sqlite3.wasm |
| macOS   | 无                                 | 环境变量                    | CocoaPods（国内可用）          |
| iOS     | 无                                 | 环境变量                    | CocoaPods（国内可用）          |
| Linux   | CMake sqlite3                     | sqlite3 全局缓存            | sqlite.org               |

---

## 变更记录

| 版本     | 日期         | 变更内容                 |
| ------ | ---------- | -------------------- |
| v1.0.0 | 2026-03-28 | 初始版本，定义全局+项目两层镜像配置架构 |
