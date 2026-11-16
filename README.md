# 手势控制 · 花朵绽放（网页版）

摄像头手势控制花朵绽放的纯前端网页项目：**握拳 = 花苞闭合，张开手掌 = 完全绽放**，连续渐变控制。

## 快速开始

1. 双击 `启动服务器.bat`（或在文件夹里运行 `python -m http.server 8080`）
2. 浏览器（Edge / Chrome）打开 `http://localhost:8080`
3. 弹出摄像头授权点「允许」→ 举手测试

> 注意：必须通过 localhost 或 https 打开（浏览器安全限制），直接双击 HTML 文件无法使用摄像头。

## 三种画面版本（右上角按钮切换）

| 版本 | 内容 |
|---|---|
| 视频版·红芍药 | 原始拍摄素材，手势逐帧控制（242帧） |
| 蓝玫瑰版 | AI 生成素材，同样手势控制（362帧） |
| 代码花版 | 纯代码绘制的花朵，可一键导出 .webm 视频 |

## 功能特性

- **手势连续控制**：张开度（0~1 的数值）→ 逐帧显示（每秒 30 帧），渐变丝滑
- **多手自动锁定**：多只手入镜时，自动跟踪"动作明显的那只手"，静止的手不干扰控制
- **抗干扰校准**：滚动分位数自适应校准 + 离群值拒绝，误检数据不会污染范围
- **缓存自愈**：帧序列缓存在加载时做内容体检，坏缓存自动丢弃并重新提取
- **键盘手动测试**：←/→ 逐帧拖动；双击画面全屏；代码花版可导出绽放视频

## 项目结构

```
├── index.html                  主页面（全部逻辑都在这一个文件里）
├── slides.html                 交互式项目讲解 PPT（浏览器直接打开）
├── 启动服务器.bat               一键启动本地服务器（关窗口=停止）
├── docs/                       交付文档
└── assets/
    ├── flower.mp4              红芍药素材
    ├── blue_rose.mp4           蓝玫瑰素材（AI 生成）
    ├── model/hand_landmarker.task   手部识别模型（已内置，克隆即可用）
    └── wasm/                   MediaPipe 运行引擎（本地加载，无需联网）
```

## 技术栈

- **MediaPipe Hand Landmarker**：浏览器本地推理，输出手部 21 个关键点（WASM/GPU）
- **Canvas 2D 逐帧渲染** + `requestVideoFrameCallback` 播放式抓帧
- **Web Worker**：JPEG 编码放后台线程，不卡主线程
- **IndexedDB**：帧序列缓存（首次提取约 15 秒，之后秒开；缓存按浏览器分别存储）
- 原生 HTML / CSS / JavaScript，无框架依赖

## 运行要求

- Windows / macOS / Linux，浏览器用 Edge 或 Chrome
- 光线充足，手离摄像头 0.5~1.5 米
- 多手入镜时程序会自动锁定"动的那只手"，拿东西的手保持静止即可

## 许可证与来源说明

- **代码**：MIT License（见 LICENSE 文件），可自由使用、修改、分发
- **手部识别模型**（assets/model/hand_landmarker.task）：来自 Google MediaPipe 官方发布
- **识别引擎**（assets/wasm/）：来自官方 npm 包 `@mediapipe/tasks-vision`
- **素材视频**：`assets/flower.mp4` 与 `assets/blue_rose.mp4` 均为作者使用豆包 Seedream（AI 生成）制作
