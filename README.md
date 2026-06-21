![YoloTouchHelp Icon](Image/apk-icon.webp)

# YoloTouchHelp

Android 端 AI 检测与悬浮控制项目，支持 YOLO 多模型、QNN HTP / CPU 推理、悬浮菜单、区域设置与触摸注入。

[![Telegram](https://img.shields.io/badge/Telegram-Channel-26A5E4?logo=telegram&logoColor=white)](https://t.me/YoloTouchHelp)

本项目基于 [xiangsu1145/Auto-aim\_android-yolo](https://github.com/xiangsu1145/Auto-aim_android-yolo) 进行持续修改与扩展，在原有工程基础上完成了界面、功能与交互层面的多项调整。特别感谢 [xiangsu1145](https://github.com/xiangsu1145) 提供的项目基础与相关支持。

## 项目简介

`YoloTouchHelp` 是一个基于 Android + YOLO + NDK 的本地推理项目，核心能力包括：

- 本地目标检测，支持多种 YOLO TFLite 模型
- 高通平台 QNN HTP 加速，并保留 CPU 推理切换
- 悬浮球、悬浮菜单、区域设置与检测框绘制
- Root / Shizuku 双触摸注入链路
- HTML 主界面、HTML 弹窗与统一风格设置页

## 相较原项目的更新

### 新增特性

- 新增 `KalmanObjectTracker`，支持目标短时丢失后的续跟与速度输出
- 接入 `IoU + 中心距离` 贪心匹配，提升多目标场景下的轨迹延续性与锁框稳定性
- 自瞄预判已联动目标速度，并优先使用 `Kalman` 输出结果进行提前量修正
- 扩展自瞄与扳机参数，支持丢帧容忍、锁框阈值、中心权重、死区保持、边界回中、吸附式扳机等能力
- 区域设置新增 `开镜区`，并支持与触发区共同参与自瞄激活判断
- 开放更多菜单调参项，包括 `Kalman` 参数、预测参数、压枪强度与触摸方向

### 修复内容

- 修复区域判定不准的问题，重做物理手指区域命中逻辑
- 修复触摸坐标逆变换不对称导致的“只有四条边能触发”问题
- 修复关闭自瞄或未满足激活条件时误清框的问题，保留当前检测与锁定显示
- 修复压枪链路缺失、力度过强或无明显效果的问题
- 修复 `Shizuku` 模式下分辨率、旋转与用户服务更新带来的初始化异常
- 修复区域设置层未铺满全屏的问题，让调整区域时可完整覆盖屏幕边缘

  <br />

### UI 重构

- 主页已重构为统一的 HTML 风格主界面
- 更新日志、免责声明、权限说明、模型选择等弹窗已统一为 HTML 风格
- 悬浮菜单已改为 HTML 布局，并保留原有核心参数调节能力
- 设置页与高级设置页已重新整理配色、入口结构与信息层级

## 项目展示

| 首页                          | 设置                          |
| --------------------------- | --------------------------- |
| ![预览1](Image/preview-2.jpg) | ![预览2](Image/preview-1.jpg) |

| 弹窗                          | 免责声明                        |
| --------------------------- | --------------------------- |
| ![预览3](Image/preview-3.jpg) | ![预览4](Image/preview-4.jpg) |

| 免责声明                        | 免责声明                        |
| --------------------------- | --------------------------- |
| ![预览5](Image/preview-5.jpg) | ![预览6](Image/preview-6.jpg) |

## 功能特性

- **推理后端**：支持 `QNN HTP`、`NNAPI` 回退与 `CPU 推理`
- **模型管理**：支持多模型切换、模型信息展示、首次启动自动解压
- **悬浮控制**：支持悬浮球、HTML 风格主页、HTML 风格悬浮菜单
- **区域设置**：支持开火区、触发区、瞄准区、摇杆范围、开镜区
- **触摸链路**：支持 `Root` 与 `Shizuku` 两条注入链路
- **辅助能力**：支持自动扳机、按住激活、压枪、预测、Kalman 参数调节
- **可视化**：支持检测框、锁定射线、中心点、触摸显示与区域调整

## 运行要求

| 项目   | 要求                          |
| ---- | --------------------------- |
| 系统   | Android 12 及以上              |
| 架构   | arm64-v8a                   |
| 权限   | 悬浮窗、屏幕录制、Shizuku 或 Root     |
| 芯片建议 | 骁龙 8 Gen 1 及以上，QNN HTP 体验更好 |

> 非高通平台也可切换 CPU 推理，但速度与体验通常会明显下降。

## 快速开始

1. 安装并启动 `Shizuku`，或确保设备已具备 `Root`
2. 安装本项目生成的 APK
3. 首次进入应用后，按提示完成免责声明、悬浮窗、录屏等授权
4. 在主页选择模型并启动悬浮窗
5. 点击悬浮球打开悬浮菜单，按需调整参数与区域

## APK 构建

```bash
./gradlew assembleDebug
./gradlew assembleRelease
```

当前 `release` 安装包输出路径：

```text
app/build/outputs/apk/release/app-release.apk
```

## 主要结构

| 模块                     | 说明                         |
| ---------------------- | -------------------------- |
| `MainActivity.kt`      | HTML 主界面、免责声明、更新日志、设备信息等入口 |
| `FloatService.kt`      | 前台服务、检测循环、悬浮球、区域设置、触摸注入协同  |
| `GuiPanelView.kt`      | HTML 风格悬浮菜单宿主              |
| `OverlayCanvasView.kt` | 检测框、锁定线、中心点等绘制             |
| `AreaSettingsView.kt`  | 区域调整层                      |
| `aimbot.cpp`           | Native 推理入口                |
| `uinput_inject.cpp`    | Native 触摸注入                |

## 使用说明

### 主页

- 查看当前状态、权限模式、悬浮权限和后端信息
- 切换模型
- 启动或停止悬浮窗

### 设置页

- 导出 / 导入配置
- 查看权限说明
- 进入高级设置
- 查看 GitHub、Telegram 频道、更新日志、友情致谢

### 悬浮菜单

- 自瞄参数
- 扳机参数
- 模型切换与模型状态
- 系统显示项与区域设置

## 友情致谢

- 感谢 [speed-arch](https://github.com/speed-arch) 提供底层框架支持
- 感谢 [xiangsu1145](https://github.com/xiangsu1145) 提供 APK 框架及模型支持
- 感谢 [Aliang1337](https://github.com/Aliang1337) 提供中转站支持
- 中转站地址：
- 老站：<https://api.alysc.top>
- 新站一：<https://code.alysc.top>
- 新站二：<https://api.nullyz.cn>
- 感谢 GPT、Claude 提供代码支持

## 注意事项

- 项目仅供学习、研究与技术交流使用
- 请自行评估相关使用风险与适用场景
- 首次启动时模型会从 `assets` 解压到内部存储
- 如果界面改动后手机上看不到新内容，建议卸载旧包后重新安装最新 `release` 包

## License

MIT License，详见 [LICENSE](LICENSE)
