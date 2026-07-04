<div align="center">

<img src="script/main_icon.ico" width="96" height="96" alt="logo">

# 🔫 PUBG 迫击炮标点测距工具

### PUBG Mortar Range Finder & Ballistic Calculator

[![Platform](https://img.shields.io/badge/Platform-Windows%2010%2F11-blue?logo=windows)](https://www.microsoft.com/windows)
[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python)](https://www.python.org/)
[![.NET](https://img.shields.io/badge/.NET-9.0-512BD4?logo=dotnet)](https://dotnet.microsoft.com/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Xbox](https://img.shields.io/badge/Xbox%20Game%20Bar-Widget-107C10?logo=xbox)](https://learn.microsoft.com/windows/uwp/gaming/gamebar)

</div>

---

## 📖 简介

一套完整的 **PUBG: BATTLEGROUNDS 迫击炮战术辅助系统**，通过 **屏幕颜色识别 + 弹道物理计算**，实时计算迫击炮开火密位，并以 **Xbox Game Bar 透明小组件** 的形式叠加在游戏画面上方。

> 🛡️ **安全声明**：本工具为纯外部视觉辅助，仅通过屏幕截图进行像素匹配，**不读取游戏内存、不注入 DLL、不修改任何游戏文件**，符合 PUBG 外部覆盖层 (External Overlay) 规范。

---

## ✨ 核心特性

| 模块 | 功能 |
|------|------|
| 🎯 **多目标追踪** | 同时识别 4 个队友标点（黄 / 红 / 蓝 / 绿），同屏显示实时距离 |
| 📏 **密位标尺** | F8 一键开启攻城模式，绘制垂直迫击炮开火密位刻度尺 |
| 🚀 **弹道计算** | 基于迫击炮物理模型 (初速 82.9m/s, 重力 9.8m/s²) 自动换算开火角度 |
| 🔭 **双地图模式** | 小地图后台静默测距 + 大地图一键宏缩放测距 (F9) |
| 🎮 **Xbox Game Bar 集成** | Win+G 呼出透明叠加层，不遮挡游戏画面 |
| 🔧 **可视化配置** | PyQt6 图形界面，支持自定义分辨率、色盲模式、扫描频率 |
| 🎨 **色盲适配** | 内置多套颜色预设，红色盲 / 绿色盲友好 |
| 📐 **任意 UI 比例** | 支持自定义截图区域绝对坐标，兼容特殊 UI 缩放 |

---

## 🏗️ 架构概览

```
┌──────────────────────────────────────────────────────┐
│                   script/ (Python 后端)                │
│                                                        │
│  ┌──────────┐    ┌──────────┐    ┌──────────────────┐ │
│  │  截取屏幕  │ → │ OpenCV   │ → │ 弹道物理计算      │ │
│  │  (mss)   │    │  颜色识别 │    │ 距离 → 密位/刻度  │ │
│  └──────────┘    └──────────┘    └────────┬─────────┘ │
│                                           │            │
│                          WebSocket (8899) │            │
│                          radar_data.json  │            │
└───────────────────────────────────────────┬────────────┘
                                            │
                                            ▼
┌──────────────────────────────────────────────────────┐
│                widget/ (Xbox Game Bar 小组件)          │
│                                                        │
│  ┌─────────────────┐  ┌─────────────────────────────┐ │
│  │  距离面板        │  │  攻城标尺                     │ │
│  │  黄 150m 🔴 230m │  │  ┊    650                    │ │
│  │  🔵 0m   🟢 410m│  │  ┊──────── 720               │ │
│  └─────────────────┘  └─────────────────────────────┘ │
│                Win+G 呼出 · 透明叠加                    │
└──────────────────────────────────────────────────────┘
```

---

## 🚀 快速开始

### 环境要求

| 依赖 | 版本 |
|------|------|
| 🪟 Windows | 10 (19041+) / 11 |
| 🐍 Python | 3.10 或更高 |
| 🎮 PUBG | 无边框窗口模式 |
| 🛠️ Visual Studio | 2022 (仅小组件编译需要) |

### 1. 启动后端脚本

```bash
cd script
pip install -r requirements.txt
cp config.example.json config.json
python ui_config_tool.py          # 图形化校准截取区域
python radar_scanner.py           # 启动扫描引擎
```

### 2. 部署 Xbox 小组件

1. 用 **Visual Studio 2022** 打开 `widget/TacticalRadarFinal.sln`
2. 右键项目 → **发布 (Publish)** → 选择目标架构 (x64)
3. 双击生成的 `.msix` 安装包
4. 游戏中按 **Win+G** → 将"战术雷达终端"小组件添加到游戏叠加层

> 📚 详细配置说明见 [script/README.md](script/README.md) 和 [widget/README.md](widget/README.md)

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
| **Python** | 后端核心逻辑 |
| **OpenCV** (`cv2`) | 屏幕颜色识别与轮廓检测 |
| **mss** | 高性能屏幕截图 |
| **WebSocket** | 前后端实时数据推送 (30 FPS) |
| **PyQt6** | 图形化配置面板 |
| **C# / UWP** | Xbox Game Bar 小组件 |
| **XAML** | 小组件 UI 布局 |

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
├── script/                       # Python 后端
│   ├── radar_scanner.py          # 核心引擎（屏幕捕获 + 识别 + WebSocket）
│   ├── ui_config_tool.py         # PyQt6 图形化配置工具
│   ├── index.html                # Web HUD 叠加层
│   ├── config.example.json       # 配置文件模板
│   ├── requirements.txt          # Python 依赖
│   └── test/                     # 测试截图
├── widget/                       # Xbox Game Bar 小组件
│   ├── TacticalRadarFinal.sln    # VS 解决方案
│   └── TacticalRadarFinal/       # UWP 项目源码
│       ├── MainPage.xaml(.cs)    # 距离显示面板
│       ├── RulerPage.xaml(.cs)   # 标尺/准星面板
│       └── Package.appxmanifest  # 小组件注册清单
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

`PUBG` `绝地求生` `迫击炮` `测距` `标尺` `密位` `辅助` `Mortar` `Range Finder` `Ballistic Calculator` `External Overlay` `Xbox Game Bar Widget` `OpenCV` `Python` `UWP` `游戏叠加层` `屏幕颜色识别` `弹道计算` `攻城模式`

</div>
