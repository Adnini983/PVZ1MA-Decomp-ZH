# Plants vs. Zombies 1 Decomp —— 中文化分支（master）

> **分支说明**：本仓库当前位于 **master**（主线），对应 `source_chinese`——一个**中文化（2012 中文年度版数据包）**的 PVZ1 Decomp fork。
> 面向英文原版数据包的 **`OG_Only`** 分支对应 `source_modding`（见[分支说明](#分支说明)）。

---

## 这是什么

本仓库是对植物大战僵尸 1（PVZ1）Decomp 源码的一次**中文化改造**，采用**整包加载 2012 中文年度版 `main.pak`** 的路线，让游戏原生渲染中文，并同时保持 **1.0.0.1051 英文原版 `main.pak`** 的兼容性。

- 源码来源：Discord 社区 **「Plants Vs. Zombies 1 Modders Association」** 的 Decomp。
- 修改方：**豆包（Doubao）**。
- 提示词与测试环境：由**用户**提供。
- 中文化方案参考：2012 中文年度版（纯 PAK 实现中文化）；资源打包工具参考 [Pistonight/pvz-bintools](https://github.com/Pistonight/pvz-bintools)。

## 关键特性

- **单一源码树 + 双数据包分支**：在 IDE 中自选配置即可编译英文（OG）或中文（GOTY）版本。
- **整包加载中文 `main.pak`**：中文由游戏原生渲染，无需 gdi42.dll 注入或外部 TTF。
- **UTF-8 码点感知渲染**：中文 3 字节不再被拆开量宽，避免空白文本与不换行问题。
- **生存模式越界崩溃修复**。
- **reanim 命名差异隔离**（详见下文"PAK 兼容"）。

## 分支说明

| 分支 | 对应目录 | 数据包 | 说明 |
| --- | --- | --- | --- |
| **master**（本分支） | `source_chinese` | 2012 中文年度版 + 1.0.0.1051 英文原版 | 中文化主线，含 OG / GOTY 两套编译配置 |
| **OG_Only** | `source_modding` | 1.0.0.1051 英文原版 | 纯英文分支（无中文化渲染改动，只含生存修复） |

## 如何编译

### 环境

- Windows
- Visual Studio 2022（或 VS2022 Build Tools，含 **C++ 桌面开发**负载）
- 平台名统一为 **`x86`**（不是 `Win32`）

### 编译配置（4 个）

| 配置 | 对应数据包 | 用途 |
| --- | --- | --- |
| `Debug` | 1.0.0.1051 英文原版 `main.pak` | 英文 OG 分支，调试版 |
| `Release` | 1.0.0.1051 英文原版 `main.pak` | 英文 OG 分支，发布版 |
| `Debug-GOTY` | 2012 中文年度版 `main.pak` | 中文 GOTY 分支，调试版 |
| `Release-GOTY` | 2012 中文年度版 `main.pak` | 中文 GOTY 分支，发布版 |

> `GOTY` 配置定义编译宏 `PVZ_GOTY_ZH_PAK`，用于在源码中隔离两套 PAK 的命名差异。

### 在 IDE 中编译

1. 用 Visual Studio 2022 打开根目录的 `PlantsVsZombies.sln`。
2. 在配置管理器中选择平台 `x86`，再选择配置（`Debug` / `Release` / `Debug-GOTY` / `Release-GOTY`）。
3. 菜单 **生成 → 重新生成解决方案**。

### 命令行编译

```powershell
"C:\Program Files (x86)\Microsoft Visual Studio\2022\BuildTools\MSBuild\Current\Bin\MSBuild.exe" `
    "PlantsVsZombies.sln" `
    /p:Configuration=Release-GOTY /p:Platform=x86 /m /v:minimal /nologo
```

将 `Release-GOTY` 换成任意配置名即可。产物输出到对应目录（`Debug\`、`Release\`、`Debug-GOTY\`、`Release-GOTY\`）。

### 运行 / 测试

把编译出的 `LawnProject.exe` 复制到对应数据包目录再运行：

- **OG 英文分支**：复制到 1.0.0.1051 英文原版目录（含英文 `main.pak`）。
- **GOTY 中文分支**：复制到 2012 中文年度版目录（含中文 `main.pak`）。

### CI

本仓库已配置 GitHub Actions（`.github/workflows/ci.yml`），会自动在 `windows-latest` 上编译全部 4 个配置，验证源码可构建。

## PAK 兼容

### reanim 命名差异（重要）

两套数据包对**迪斯科僵尸**及其小弟的 reanim 动画命名不同：

| 资源 | 1.0.0.1051 英文原版 | 2012 中文年度版 |
| --- | --- | --- |
| 迪斯科僵尸 | `Zombie_Jackson` | `Zombie_disco` |
| 迪斯科僵尸小弟 | `Zombie_dancer` | `Zombie_backup` |

源码中由宏 `PVZ_GOTY_ZH_PAK` 隔离（`Sexy.TodLib\Reanimator.cpp`）。

> ⚠️ **OG 分支原则上可以加载中文年度版 PAK，但需要自行补全相关材料（`Zombie_disco` / `Zombie_backup` 动画），否则游戏会因找不到 `compiled\reanim\Zombie_disco.reanim.compiled` 而报错、无法启动。** 推荐做法：英文原版数据用 **OG** 配置，中文年度版数据用 **GOTY** 配置，二者不要混用。

## 目录结构

```
source_chinese/
├── PlantsVsZombies.sln        # 解决方案（4 配置）
├── SexyAppFramework/          # 游戏框架与主工程（SexyAppBase.vcxproj）
├── Lawn/                      # 游戏逻辑
├── Sexy.TodLib/               # 渲染 / 动画 / 文本
├── ImageLib/ PakLib/          # 图像与 PAK 资源库
├── dx8sdk/                    # 编译所需 DirectX SDK 头
├── PopcapDocs/ PopcapTools/
├── bass.dll                   # 运行时音频库
├── docs/                      # 官方 Decomp 文档中文翻译与扩写版（.md / .docx）
├── README.md
└── .github/workflows/ci.yml   # GitHub Actions 云编译
```

## 关联文档

- 详细的官方 Decomp 文档（英文原版由 Discord 用户 `scarletstarz2009` 编写）及**中文翻译与扩写版**（含编译与故障排查说明），由豆包翻译扩写、用户提供提示词与测试环境。
- 中文文档与截图位于本仓库的 `docs/` 目录（含 `.md` 与 `.docx` 两种版本）。

## 致谢

- 源码：Discord 社区 **「Plants Vs. Zombies 1 Modders Association」**。
- 本分支由 **豆包（Doubao）** 修改，**提示词与测试环境由用户提供**。
