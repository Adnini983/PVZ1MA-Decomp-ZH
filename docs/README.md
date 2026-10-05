# docs —— PVZ1 Decomp / Modding 文档（中文翻译与扩写版）

本目录收录社区 Decomp / Modding 文档的**中文翻译与扩写版**。

## 文件说明

| 文件 | 说明 |
| --- | --- |
| `The OFFICIAL PVZ Decomp DOC V1.8.5 中文翻译与扩写.md` | **规范源（canonical source）**，日常修改都改这一份 |
| `The OFFICIAL PVZ Decomp DOC V1.8.5 中文翻译与扩写.docx` | **发布版**，由上面的 `.md` 生成，不手动编辑 |
| `media/` | 文档引用的截图（`image1.png …`） |

## 维护约定

- **Markdown 是规范源，DOCX 是发布版。** 需要改内容时，只改 `.md`，再据此重新生成 `.docx`；不要同时人工维护两套，避免两者不一致。
- 文档标题中的 "OFFICIAL" 沿用英文原版标题，仅表示社区文档自身命名，**不代表 PopCap / EA 官方出版物**。

## 分支适用

本文是 `main`（ZH）与 `OG_Only` 两个分支**共用**的基础手册。其中涉及 `Debug-GOTY` / `Release-GOTY` 配置、中文 PAK、`PVZ_GOTY_ZH_PAK` 宏、2012 中文年度版等**中文化内容仅适用于 `main` 分支**；`OG_Only` 分支只提供 `Debug` / `Release` 两个配置，且不含中文化渲染改动。

## 致谢

- 英文原版由 **Discord 用户 `scarletstarz2009`** 编写，源码来自 Discord 社区 **「Plants Vs. Zombies 1 Modders Association」**。
- 中文翻译与扩写由 **豆包（Doubao）** 完成，**提示词与测试环境由用户提供**。
