# LSFG Android Chinese

> 基于 [LSFG-Android](https://github.com/FrankBarretta/LSFG-Android) v0.1.2 中文翻译版

[![Discord](https://img.shields.io/discord/1496212333595463780?label=Discord&logo=discord)](https://discord.gg/CkuumNJ7s4)

LSFG Android 是将帧生成技术带到 Android 设备的应用程序。通过 Vulkan 实现 Lossless Scaling 帧生成，让您的手机或平板也能享受流畅的游戏体验。

## 功能特点

- **帧生成技术**：支持 LSFG 3.1 / LSFG 3.1P，在 GPU 上通过 AHardwareBuffer 共享运行
- **实时游戏内设置抽屉**：可调节帧倍率（2×-8×）、光流缩放、性能模式、HDR 模式、抗伪影等
- **自动叠加层**：选择目标应用后，叠加层会在游戏切换到前台时自动启动
- **双入口模式**：可拖动的浮动图标或屏幕边缘滑动抽屉
- **首次启动教程**：逐步引导完成无障碍权限设置
- **触控穿透**：支持全透明叠加层，可在玩游戏时正常触控
- **多种捕获源**：MediaProjection（默认）、Shizuku 指标模式
- **后处理管线**：NPU 预设（锐化、细节增强、色度清洁）、GPU 放大、CPU 增强
- **帧图表 HUD**：实时显示帧率、帧时间图和同步诊断
- **崩溃报告器**：捕获 Java/Kotlin 异常和原生信号

## 仓库结构

| 路径 | 说明 |
|------|------|
| [`LSFG-Android-Application/`](LSFG-Android-Application/) | Android Studio 项目 — Kotlin + Jetpack Compose UI，JNI/C++ 渲染循环 |
| [`lsfg-vk-android/`](lsfg-vk-android/) | 子模块，基于 `lsfg-vk` 的 Android 适配版本 |

## 下载安装

从 [Releases](https://github.com/timyang2005/LSFG-Android/releases) 页面下载 APK：

- `app-arm64-v8a-debug.apk` — ARM64 设备（大多数手机）
- `app-x86_64-debug.apk` — x86_64 设备（模拟器/部分平板）

使用 `adb install` 安装，或直接在设备上安装。

## 编译

```sh
cd LSFG-Android-Application
./gradlew :app:assembleDebug
```

APK 输出路径：`app/build/outputs/apk/debug/`

工具链要求：Android Studio Ladybug+、NDK 27.0.12077973、CMake 3.22.1、JDK 17、C++20。ABI：`arm64-v8a` 和 `x86_64`。`minSdk=29`（Android 10），`targetSdk=35`（Android 15）。

## 设备要求

- **Vulkan 1.2+**
- `VK_ANDROID_external_memory_android_hardware_buffer`
- `VK_KHR_external_memory` + `VK_KHR_sampler_ycbcr_conversion`
- **`VK_EXT_robustness2`** — 帧生成必需
- `VK_EXT_queue_family_foreign` — 推荐

端到端帧生成在 Adreno 7xx 级及以上 GPU 上工作（骁龙 8 Gen 2/3/4）。

## 权限说明

| 权限 | 用途 |
|------|------|
| `SYSTEM_ALERT_WINDOW` | 在目标游戏上方显示叠加层 |
| `FOREGROUND_SERVICE_MEDIA_PROJECTION` | 运行可见捕获路径 |
| `FOREGROUND_SERVICE_SPECIAL_USE` | 运行 Shizuku 计时路径 |
| `POST_NOTIFICATIONS` | 前台服务通知 |
| `BIND_ACCESSIBILITY_SERVICE`（可选） | 以无障碍叠加层托管，在严格 OEM 设备上更稳健 |

## 重要提示

> [!IMPORTANT]
> 您需要拥有合法购买的 Lossless Scaling 副本。`Lossless.dll` **不**由本项目分发、下载或捆绑。用户通过存储访问框架选择自己的 DLL，应用在设备上提取着色器到私有存储，然后删除 DLL 副本。本项目不分发 Lossless Scaling 资源。

## 平台限制

在非 Root 的 Android 上，无法像 Linux 的 Vulkan 隐式层那样挂钩另一个应用。这意味着：

- 无法直接注入到游戏的 Vulkan 交换链
- 帧生成运行在 `MediaProjection` 屏幕捕获流上
- 延迟比 Linux 版本高约 50-80ms
- 每次会话启动需要用户同意屏幕录制

## 致谢

本项目基于以下工作：

- **[PancakeTAS](https://github.com/PancakeTAS)** — 原始 `lsfg-vk` Vulkan 帧生成层
- **THS / Lossless Scaling** — Lossless Scaling 帧生成着色器原始作者
- **[FrankBarretta](https://github.com/FrankBarretta)** — Android 移植版本

## 许可证

| 子目录 | 许可证 | 文件 |
|------|--------|------|
| `LSFG-Android/` | **自定义许可证 — 禁止 Play 商店、禁止商业使用** | [`LSFG-Android-Application/LICENSE`](LSFG-Android-Application/LICENSE) |
| `lsfg-vk-android/` | **MIT**（继承自上游 `lsfg-vk`） | [`lsfg-vk-android/LICENSE.md`](lsfg-vk-android/LICENSE.md) |

`Lossless.dll` 是 THS / Lossless Scaling 的财产，**不**由本项目以任何方式分发。
