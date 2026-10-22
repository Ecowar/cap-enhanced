# Cap 增强版（cap-enhanced）

> 基于开源录屏工具 **[Cap](https://github.com/CapSoftware/Cap)** 的增强分支 —— 在完整保留原版功能的基础上，新增**录制实时画笔标注**，并完善**简体中文**界面。

---

## ✨ 与原版的差异

### 1. 录制实时画笔标注（核心新增）

录制过程中就能直接在屏幕上「边录边画」，标注实时显示，并会合成进最终视频：

- **三种工具**：画笔（自由曲线）、矩形框、箭头
- **五种颜色**：红 / 黄 / 绿 / 蓝 / 白（可扩展）
- **一键撤销**：画错了随时撤销上一步
- **跟随悬浮条**：录制时点击悬浮控制条上的画笔按钮即可开启

适用场景：录教程圈重点、录 Bug 报告指问题、给同事演示时标注界面元素。

### 2. 中文界面

- 新增/补齐标注功能的简体中文（`zh_hans`）与繁体中文（`zh_hant`）文案
- 底层沿用 `i18n` 多语言框架，界面文案可随系统语言自动切换

### 3. Windows 构建适配

- `build.bat` 适配了非标准 MSVC 安装路径，并在 MSVC 工具链异常时自动切换 `clang-cl` 编译，保证本地环境能顺利出包

---

## 📦 下载安装

到本仓库的 **[Releases](https://github.com/Ecowar/cap-enhanced/releases)** 页面下载：

- `Cap_0.6.1_x64-setup.exe` —— NSIS 安装包（推荐，双击安装）
> 安装包单个约 128MB，超过 GitHub 单文件 100MB 限制，因此不放在源码目录，仅作为 Release 附件提供。

---

## 🛠️ 从源码构建（Windows）

### 环境要求

| 依赖 | 版本 |
| --- | --- |
| Node.js | ≥ 20 |
| Bun | 1.4.0 |
| Rust | ≥ 1.88 |
| Git for Windows | 任意 |
| Visual Studio Build Tools | 2022（含 C++ 工具链） |

### 构建步骤

```bash
# 1. 安装依赖
bun install

# 2. 下载原生依赖（ffmpeg / onnxruntime / dxc）
bun run cap-setup

# 3. 构建 Windows 安装包
bash scripts/bundle-gpui.mjs x86_64-pc-windows-msvc
```

Windows 上也可直接双击 `build.bat`。

> **提示**：若 `bun run cap-setup` 下载 onnxruntime 时报 `UNABLE_TO_GET_ISSUER_CERT_LOCALLY`（SSL 证书问题），
> 可手动下载 `onnxruntime-win-x64-1.24.2.zip`（[官方 Release](https://github.com/microsoft/onnxruntime/releases/tag/v1.24.2)），
> 解压后把 `lib/*.dll` 放到 `target/native-deps/onnxruntime/lib/`。

构建产物位置：

- 可执行文件：`apps/desktop-gpui/target/x86_64-pc-windows-msvc/release/Cap.exe`
- 安装包：`apps/desktop-gpui/target/x86_64-pc-windows-msvc/release/bundle/nsis/Cap_0.6.1_x64-setup.exe`

---

## 🖊️ 画笔功能怎么用

1. 打开 Cap，开始录制
2. 屏幕上的悬浮控制条会出现**画笔按钮**
3. 点击进入画笔模式，选择工具（画笔 / 矩形 / 箭头）和颜色
4. 在屏幕上直接拖动绘制，标注实时显示
5. 画错了点**撤销**；标注会随录制一起合成进最终视频

---

## 📁 目录结构

```
apps/desktop-gpui/   Rust + GPUI 桌面主程序（画笔、编辑器、录制都在这里）
crates/rendering/    视频合成引擎（画笔 GPU 图层在这里）
crates/              其余 40+ 个 Rust 库
apps/web/            Web 前端（Next.js）
packages/            共享 TS 包
scripts/             构建与打包脚本
```

关键改动文件：

- `apps/desktop-gpui/src/annotation_canvas.rs` —— 全屏透明画板
- `crates/rendering/src/layers/annotation.rs` —— GPU 标注图层
- `crates/rendering/src/shaders/annotation.wgsl` —— 标注着色器
- `apps/desktop-gpui/src/controls_window.rs` —— 悬浮条画笔模式

更多实现细节见仓库内的 [`画笔功能开发总结.md`](画笔功能开发总结.md) 与 [`更新日志.md`](更新日志.md)。

---

## ⚖️ 许可与致谢

本项目基于 **Cap**（作者 Cap Software, Inc.）修改，遵循其原始许可，详见 [LICENSE](LICENSE)。

- 原项目：https://github.com/CapSoftware/Cap
- 官网：https://cap.so

感谢 Cap 团队开源了如此优秀的产品。