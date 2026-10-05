# Plants vs. Zombies 1 Decomp —— OG 英文原版分支（OG_Only）

> **分支说明**：本分支为仓库的 **`OG_Only`**，面向 **1.0.0.1051 英文原版数据包**。
> 中文化（2012 中文年度版数据包）的主线在 **`main`** 分支。

---

## 这是什么

本仓库是基于植物大战僵尸 1（PVZ1）Decomp 源码的**英文原版（OG）分支**，专注 **1.0.0.1051 英文原版 `main.pak`**，并包含生存模式越界崩溃修复。

- 源码来源：Discord 社区 **「Plants Vs. Zombies 1 Modders Association」** 的 Decomp。
- 修改方：**豆包（Doubao）**。
- 提示词与测试环境：由**用户**提供。

## 与 main 分支的区别

| 项 | main | 本分支 OG_Only |
| --- | --- | --- |
| 数据包 | 2012 中文年度版 / 1.0.0.1051 英文原版 | 1.0.0.1051 英文原版 |
| 编译配置 | `Debug` / `Release` / `Debug-GOTY` / `Release-GOTY` | `Debug` / `Release` |
| 中文化渲染 | 有（UTF-8 码点感知 + 整包加载中文 PAK） | 无（原版英文渲染） |
| 生存修复 | 有 | 有 |

## 如何编译

### 环境

- Windows
- Visual Studio 2022（或 VS2022 Build Tools，含 **C++ 桌面开发**负载）
- 平台名统一为 **`x86`**（不是 `Win32`）

### 编译配置

| 配置 | 对应数据包 | 用途 |
| --- | --- | --- |
| `Debug` | 1.0.0.1051 英文原版 `main.pak` | 调试版 |
| `Release` | 1.0.0.1051 英文原版 `main.pak` | 发布版 |

### 在 IDE 中编译

1. 用 Visual Studio 2022 打开根目录的 `PlantsVsZombies.sln`。
2. 在配置管理器中选择平台 `x86`，再选择 `Debug` 或 `Release`。
3. 菜单 **生成 → 重新生成解决方案**。

### 命令行编译

```powershell
"C:\Program Files (x86)\Microsoft Visual Studio\2022\BuildTools\MSBuild\Current\Bin\MSBuild.exe" `
    "PlantsVsZombies.sln" `
    /p:Configuration=Release /p:Platform=x86 /m /v:minimal /nologo
```

产物输出到 `Debug\` 或 `Release\` 目录。

### 运行 / 测试

把编译出的 `LawnProject.exe` 复制到 **1.0.0.1051 英文原版目录**（含英文 `main.pak`）再运行。

### CI

本仓库已配置 GitHub Actions（`.github/workflows/ci.yml`），会自动在 `windows-latest` 上编译 `Debug` 与 `Release` 两个配置。

## PAK 兼容

本分支面向**英文原版数据包**。若想加载 2012 中文年度版数据包，需要注意 **reanim 命名差异**（`Zombie_Jackson`/`Zombie_dancer` vs `Zombie_disco`/`Zombie_backup`），并自行补全相关动画，否则会报错无法启动。详细说明见 **main** 分支 README 的"PAK 兼容"一节。

## 目录结构

```
PVZ1MA-Decomp-OG/
├── PlantsVsZombies.sln        # 解决方案（2 配置）
├── SexyAppFramework/          # 游戏框架与主工程（SexyAppBase.vcxproj）
├── Lawn/                      # 游戏逻辑
├── Sexy.TodLib/               # 渲染 / 动画 / 文本
├── ImageLib/ PakLib/          # 图像与 PAK 资源库
├── dx8sdk/                    # 编译所需 DirectX SDK 头
├── PopcapDocs/ PopcapTools/
├── bass.dll                   # 运行时音频库
├── docs/                      # 社区 Decomp/Modding 文档中文翻译与扩写版（.md / .docx）
├── README.md
└── .github/workflows/ci.yml   # GitHub Actions 云编译
```

## 关联文档

- 详细的**社区 Decomp / Modding 文档**（英文原版由 Discord 用户 `scarletstarz2009` 编写，为 PvZ1 Modders Association 社区维护文档，**不是** PopCap / EA 官方出版物）及**中文翻译与扩写版**，由豆包翻译扩写、用户提供提示词与测试环境。
- 文档位于本仓库 `docs/` 目录（以 `.md` 为准，`.docx` 为发布版）。该文档是 `main` 与 `OG_Only` **共用**的基础手册；其中涉及 `Debug-GOTY` / `Release-GOTY`、中文 PAK、`PVZ_GOTY_ZH_PAK` 等**中文化内容仅适用于 `main` 分支**，`OG_Only` 分支不含这些内容。
