<div align="center">

<img src="script/main_icon.ico" width="96" height="96" alt="logo">

# PUBG 迫击炮标点测距工具

### PUBG Mortar Range Finder & Ballistic Calculator

[![Platform](https://img.shields.io/badge/Platform-Windows%2010%2F11-blue?logo=windows)](https://www.microsoft.com/windows)
[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Release](https://img.shields.io/badge/Download-Release-brightgreen)](https://github.com/AGMXiaoTian/PUBG-Mortar-Radar/releases)

</div>

---

## 📖 简介

一套完整的 **PUBG 迫击炮标点测距工具**。通过 **屏幕颜色识别 + 弹道物理计算**，实时计算迫击炮开火密位，并以 **tkinter 全息透明叠加层** 的形式直接绘制在游戏画面上方。

> 🛡️ **安全声明**：本工具为纯外部视觉辅助，仅通过屏幕截图进行像素匹配，**不读取游戏内存、不注入 DLL、不修改任何游戏文件**，符合 PUBG 外部覆盖层 (External Overlay) 规范。

> 💡 **单文件自包含**：主程序 `radar_scanner.py` 集成了截屏、识别、弹道计算、全息标尺渲染所有功能，启动即用。项目也附带了可选装的 Xbox Game Bar 小组件版本（`widget/`），供偏好 Win+G 方式的用户使用。

---

## ✨ 核心特性

| 模块 | 功能 |
|------|------|
| 🎯 **多目标追踪** | 同时识别 4 个队友标点（黄 / 红 / 蓝 / 绿），同屏显示实时距离 |
| 📏 **密位标尺** | F8 一键开启攻城模式，全屏绘制垂直迫击炮开火密位刻度尺 |
| 🚀 **弹道计算** | 内置抛物线弹道引擎 (初速 82.9m/s, 重力 9.8m/s²) 自动换算开火角度 |
| 🔭 **双地图模式** | 小地图后台静默测距 + 大地图一键宏缩放测距 (F9) |
| 🖥️ **全息透明叠加** | tkinter 全屏透明窗口，直接在游戏画面上渲染，无需额外组件 |
| 🔧 **可视化配置** | PyQt6 图形界面，支持自定义分辨率、色盲模式、扫描频率 |
| 🎨 **色盲适配** | 内置多套颜色预设，红色盲 / 绿色盲友好 |
| 📐 **任意 UI 比例** | 支持自定义截图区域绝对坐标，兼容特殊 UI 缩放 |

---

## 🏗️ 架构概览

```
┌─────────────────────────────────────────────────────────┐
│              radar_scanner.py (单文件自包含)               │
│                                                           │
│  ┌──────────┐    ┌──────────┐    ┌─────────────────────┐ │
│  │  mss 截屏 │ → │ OpenCV   │ → │ 弹道物理引擎         │ │
│  │          │    │  颜色识别 │    │ calculate_mortar_dial│ │
│  └──────────┘    └──────────┘    └──────────┬──────────┘ │
│                                             │             │
│                                             ▼             │
│  ┌──────────────────────────────────────────────────────┐ │
│  │           tkinter 全息透明叠加层                      │ │
│  │                                                      │ │
│  │   ┌─────────────┐   ┌────────────────────────────┐   │ │
│  │   │ 距离显示条   │   │  F8 攻城标尺 (全屏)        │   │ │
│  │   │ 黄150 红230 │   │   ┊    [650]              │   │ │
│  │   │ 蓝 0  绿410 │   │   ┊──────── [720]         │   │ │
│  │   └─────────────┘   └────────────────────────────┘   │ │
│  └──────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────┘
```

---

## 🚀 快速开始

### 环境要求

| 依赖 | 版本 |
|------|------|
| 🪟 Windows | 10 / 11 |
| 🐍 Python | 3.10 或更高 |
| 🎮 PUBG | 无边框窗口模式 |

### ⚡ 使用流程（必须按顺序！）

```
① 首次校准                        ② 日常使用
┌────────────────────┐        ┌──────────────────────┐
│ 调试工具校准参数    │        │ 启动测距工具          │
│ (ui_config_tool)   │  ───→  │ (radar_scanner)      │
│                    │        │                      │
│ • 框选截取区域     │        │ • 屏幕颜色识别       │
│ • 设定玩家锚点     │        │ • 弹道物理计算       │
│ • 色盲模式匹配     │        │ • 全息标尺叠加       │
│ • 保存 config.json │        │ • F8 攻城 / F9 大地图│
└────────────────────┘        └──────────────────────┘
```

### 第一步：安装依赖

```bash
cd script
pip install -r requirements.txt
```

### 第二步：校准参数（首次必做！）

> 🔴 **这是最关键的一步**：每台电脑的屏幕分辨率、UI 缩放不同，必须先用调试工具校准，否则测距结果偏差巨大。

```bash
cp config.example.json config.json
python ui_config_tool.py          # 启动图形化调试工具
```

**校准要点**：
1. 将游戏设为 **无边框窗口模式** 并进入训练场
2. 用调试工具的截图预览框，精确框选屏幕右下角的小地图区域
3. 标定小地图上代表你自身的 **白色箭头中心点** 坐标
4. 确认游戏内色盲模式与调试工具中选择的一致
5. 保存，生成 `config.json`

### 第三步：启动测距工具（日常使用）

```bash
python radar_scanner.py
```

启动后脚本自动在后台截屏识别，并在屏幕上绘制透明叠加层。**标点后立即显示距离，F8 开关攻城标尺**。

> 📚 详细说明见 [script/README.md](script/README.md)

### 可选：Xbox Game Bar 小组件

`widget/` 目录中另有一套 Xbox Game Bar 小组件版本，供偏好 Win+G 呼出方式的用户选用。详见 [widget/README.md](widget/README.md)。

---

## ⌨️ 快捷键

| 快捷键 | 功能 |
|:------:|------|
| **`F8`** | 开启 / 关闭攻城模式迫击炮标尺 |
| **`F9`** | 大地图一键宏指令测距 |
| **`N`** | 切换小地图 / 小小图识别模式 |
| **`Shift + N`** | 仅切换游戏地图，脚本不响应 |

---

## 🧰 技术栈

| 技术 | 用途 |
|------|------|
| **Python** | 核心逻辑 |
| **OpenCV** (`cv2`) | 屏幕颜色识别与轮廓检测 |
| **mss** | 高性能屏幕截图 |
| **tkinter** | 全息透明叠加层渲染 |
| **PyQt6** | 图形化配置面板 |
| **PyInstaller** | 打包为独立 .exe |

---

## ❓ 常见问题

<details>
<summary><b>Q: 按 F8 有标尺但没有数字？</b></summary>

- 标点距离 < 100 米或 > 700 米被自动过滤
- 游戏色盲模式与配置工具设置不一致
- UI 缩放特殊，需用 `ui_config_tool.py` 重新校准截图区域
</details>

<details>
<summary><b>Q: 呼出标尺时游戏卡顿/丢失焦点？</b></summary>

确保游戏为 **无边框窗口模式**，并尝试：游戏 exe → 右键属性 → 兼容性 → 勾选「禁用全屏优化」
</details>

<details>
<summary><b>Q: 主程序闪退？</b></summary>

检查同目录下是否存在 `config.json`。先用配置工具保存一次生成该文件。
</details>

<details>
<summary><b>Q: 大小地图识别反了？</b></summary>

按 `Shift + N` 同步：按 N 切换游戏地图时按住 Shift，防止脚本也切换模式。
</details>

---

## 📂 项目结构

```
PUBG-Mortar-Radar/
├── script/                       # 主程序（自包含，启动即用）
│   ├── radar_scanner.py          # 核心引擎（截屏 + 识别 + 弹道 + 全息叠加）
│   ├── ui_config_tool.py         # PyQt6 图形化配置工具
│   ├── config.example.json       # 配置文件模板
│   ├── requirements.txt          # Python 依赖
│   └── test/                     # 测试截图
├── widget/                       # 可选：Xbox Game Bar 小组件版本
│   ├── TacticalRadarFinal.sln    # VS 解决方案
│   └── TacticalRadarFinal/       # UWP 项目源码
├── LICENSE                       # MIT
└── README.md
```

---

## ⚠️ 免责声明

本项目仅供 **学习交流** 使用。使用者需自行遵守《PUBG: BATTLEGROUNDS》服务条款。开发者不对因使用本工具导致的账号封禁或其他后果承担责任。

---

## 📄 开源协议

[MIT License](LICENSE) © 2026 AGMXiaoTian

---

<div align="center">

### 🔍 关键词 Keywords

`PUBG` `绝地求生` `迫击炮` `测距` `标尺` `密位` `辅助` `Mortar` `Range Finder` `Ballistic Calculator` `External Overlay` `OpenCV` `Python` `tkinter` `游戏叠加层` `屏幕颜色识别` `弹道计算` `攻城模式`

</div>
