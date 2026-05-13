# LSFG-Android 中文版

> 基于 [LSFG-Android v0.1.2](https://github.com/FrankBarretta/LSFG-Android) 翻译

将 LSFG-Android 应用的全部英文界面翻译为中文，包括字符串资源、硬编码文本、教程步骤和设置抽屉等。

## 翻译范围

| 文件 | 说明 |
|------|------|
| `strings.xml` | 150+ 条字符串资源全部翻译 |
| `HomeScreen.kt` | 首页状态标签、捕获模式描述、菜单项 |
| `ParamsScreen.kt` | 帧生成描述、帧同步信息、变更提示 |
| `TutorialScreen.kt` | 27 个教程步骤标题和描述 |
| `DllPickerScreen.kt` | DLL 选择界面提示和状态消息 |
| `AppPickerScreen.kt` | 应用搜索和加载提示 |
| `AutomaticOverlayScreen.kt` | 自动叠加层界面 |
| `BenchmarkScreen.kt` | 基准测试界面 |
| `LegalScreen.kt` | 法律声明 |
| `LsfgComponents.kt` | 通用组件文本 |
| `SettingsDrawerOverlay.kt` | 游戏内设置抽屉 50+ 处文本 |

### 翻译策略

- **用户界面文本**：全部翻译为中文
- **技术术语/算法名称**（FSR1、NIS、Lanczos、Anime4K 等）：保留英文原名，这些是行业标准术语
- **运行时调试状态**（如 `LSFG: frame-gen active`）：保留英文前缀，属于技术诊断信息
- 所有翻译均通过 Android 字符串资源系统实现，使用 `stringResource()` 和 `ctx.getString()` 规范调用

## 功能概述

LSFG-Android 是一个通过 Vulkan 实现 Lossless Scaling 帧生成的 Android 应用。它从用户提供的 `Lossless.dll` 中提取 SPIR-V 着色器，通过 `MediaProjection` 捕获屏幕，在捕获流上运行帧生成，并将生成的帧合成到目标游戏上方的系统叠加层中。

> **注意**：仅限侧载安装。端到端帧生成在 Adreno 7xx 级及以上 GPU 上工作（已验证骁龙 8 Gen 2 / Gen 3 / Gen 4）。较旧的 Adreno 和大多数 Mali 设备缺少 `VK_EXT_robustness2`，会回退到仅捕获镜像模式。

### 主要功能

- **帧生成**：LSFG 3.1 / LSFG 3.1P，支持 2× 到 8× 倍率
- **帧同步**：VSYNC 对齐、预设模式、目标 FPS 上限
- **画质增强**：GPU 放大/增强、NPU 后处理、CPU 后处理
- **捕获方式**：MediaProjection（默认）、Shizuku 捕获、Root 捕获
- **叠加层**：图标按钮或边缘滑动抽屉两种入口模式
- **自动叠加层**：选择目标应用后自动启动
- **首次启动教程**：多步骤引导，涵盖无障碍权限设置
- **崩溃报告器**：捕获 Java/Kotlin 异常和原生信号

## 下载安装

从 [Releases](https://github.com/timyang2005/LSFG-Android/releases) 页面下载 APK：

- `app-arm64-v8a-debug.apk` — 适用于 ARM64 设备（大多数手机）
- `app-x86_64-debug.apk` — 适用于 x86_64 设备（模拟器/部分平板）

使用 `adb install` 安装，或直接在设备上安装。

## 编译

```sh
cd LSFG-Android-Application
./gradlew :app:assembleDebug
```

APK 输出路径：`app/build/outputs/apk/debug/`

工具链要求：Android Studio Ladybug+、NDK 27.0.12077973、CMake 3.22.1、JDK 17、C++20、Kotlin Compose 插件。ABI：`arm64-v8a`（生产）和 `x86_64`（模拟器）。`minSdk=29`（Android 10），`targetSdk=35`（Android 15）。

## 设备要求

- **Vulkan 1.2+**
- `VK_ANDROID_external_memory_android_hardware_buffer`
- `VK_KHR_external_memory` + `VK_KHR_sampler_ycbcr_conversion`
- **`VK_EXT_robustness2`** — 帧生成必需（Adreno ≥ 7xx 级支持）
- `VK_EXT_queue_family_foreign` — 推荐

## 权限

| 权限 | 用途 |
|------|------|
| `SYSTEM_ALERT_WINDOW` | 在目标游戏上方显示叠加层 |
| `FOREGROUND_SERVICE_MEDIA_PROJECTION` | 运行可见捕获路径 |
| `FOREGROUND_SERVICE_SPECIAL_USE` | 运行 Shizuku 计时路径 |
| `POST_NOTIFICATIONS` | 前台服务通知 |
| `BIND_ACCESSIBILITY_SERVICE`（可选） | 以无障碍叠加层托管，在严格 OEM 设备上更稳健 |
| `moe.shizuku.manager.permission.API_V23`（可选） | Shizuku 指标模式 |
| MediaProjection 授权 | 每次会话启动时由用户授予 |

## 限制

- **非 Root 设备无法挂钩交换链**：Android 12+ 阻止向非调试进程加载外部代码
- **MediaProjection 增加延迟**：约 50-80ms
- **Google Play 政策限制**：不可上架 Play 商店，仅限侧载
- **需要用户提供的 `Lossless.dll`**：用户必须拥有 Steam 合法副本

## 致谢

- 原项目作者 [FrankBarretta](https://github.com/FrankBarretta)
- [LSFG-Android](https://github.com/FrankBarretta/LSFG-Android) 原始项目
- [lsfg-vk](https://github.com/LordOfMinecraft841/lsfg-vk) 帧生成库

## 许可证

本应用基于原项目许可证发布 — **自定义许可证：禁止 Play 商店，禁止商业使用**。详见原项目 [`LICENSE`](https://github.com/FrankBarretta/LSFG-Android/blob/main/LSFG-Android-Application/LICENSE)。

`Lossless.dll` **不**属于本项目，其版权归 THS / Lossless Scaling 所有。本项目不分发、不捆绑 `Lossless.dll` 的任何部分。用户必须从合法购买的 Steam 许可证中提供自己的副本。