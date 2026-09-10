# dsh-langpack-demo · DSH 语言包发布示例（多合一仓库）

> Language packs for [dsh-LexiForge](https://github.com/HCY7757/dsh-lexiforge) — a multi-pack repository: **one repo, four installable language packs**.
> 一次上传、四个语言包、逐个识别安装的官方示例。

## 这是什么 / What is this

DSH（DeepSeek Harness）的 [LexiForge 语言模组插件](https://github.com/HCY7757/dsh-lexiforge) 支持从 GitHub 安装「语言包」。本仓库示范如何用一个仓库同时发布 **4 个语言包**：

| Pack ID | 名称 | 模式 | 效果 |
|---|---|---|---|
| `demo-a-wenyan` | 文言风改写 | A | 白话回复 → 文言文风格 |
| `demo-b-huoxing` | 重度火星文 | composite (B→A→C) | 字形检索注入 + 六技法整段改写 + 混排符号保险层 |
| `demo-a-rikka` | 中二病·小鸟游六花 | A | 模仿六花语气：邪王真眼 / 契约者 / 招牌咒文 |
| `demo-a-proofread` | 输出复查·纠错 | A | 独立二读校对，无错逐字原样返回 |

## 如何被识别成 4 个包 / How the 4 packs are discovered

仓库根目录的 `langpacks.json`（本仓库自带，勿删）是多合一索引：

```json
{
  "version": 1,
  "packs": [
    { "id": "demo-a-wenyan", "name": "文言风改写", "version": "1.0.0",
      "author": "LexiForge Demo", "mode": "A", "file": "demo-a-wenyan.zip",
      "description": "把白话回复改写为文言文风格（模式 A，一次 LLM 改写）" }
  ]
}
```

识别规则（LexiForge v1.0.0+）：

1. 插件先读仓库根 `langpacks.json`；
2. 存在 → 按 `packs[]` 逐条列出（每个 `id` 对应一个可安装语言包，zip 见 `file` 字段，位于仓库根目录）；
3. 不存在 → 回退为“单包仓库”，默认取根目录 `langpack.zip`。

每个 zip 内部仍遵循语言包规范：仅 `manifest.yaml / dict.json / knowledge.db / 说明.txt`，根目录平铺，无子目录、无可执行文件。`manifest.yaml` 的 `mode` 支持 `A | B | C | composite`，复合模式用 `pipeline: [B, A, C]` 编排。

## 安装方法 / How to install

**网页端**：LexiForge 设置页 → 手动安装 → 粘贴本仓库 `owner/repo` → 会弹出「该仓库包含 4 个语言包」选择器，选一个安装。

**CLI**（需先运行插件自带 CLI）：

```powershell
# 查看免责声明（第三方包必读）
lexiforge <数据目录> disclaimer

# 安装指定包（四选一）
lexiforge <数据目录> market-install HCY7757/dsh-langpack-demo demo-a-wenyan --accept-risk
lexiforge <数据目录> market-install HCY7757/dsh-langpack-demo demo-b-huoxing --accept-risk
lexiforge <数据目录> market-install HCY7757/dsh-langpack-demo demo-a-rikka --accept-risk
lexiforge <数据目录> market-install HCY7757/dsh-langpack-demo demo-a-proofread --accept-risk

# 启用并开启全局开关
lexiforge <数据目录> enable demo-a-wenyan
lexiforge <数据目录> on
```

## 仓库结构 / Repository layout

```
dsh-langpack-demo/
├── langpacks.json            ← 多合一索引（识别为 4 个包的开关）
├── demo-a-wenyan.zip         ← ① 文言风
├── demo-b-huoxing.zip        ← ② 重度火星文（复合）
├── demo-a-rikka.zip          ← ③ 中二病·六花
├── demo-a-proofread.zip      ← ④ 输出复查·纠错
├── sources/                  ← 每个包的可编辑源码副本（改完重新打包即可）
└── README.md
```

## 作者如何发布自己的包 / How to publish your own

- 单包：仓库根放一个 `langpack.zip`（zip 内平铺 manifest.yaml 等）；
- 多包（推荐本模式）：根目录放 N 个 `*.zip` + 一个 `langpacks.json`；
- 建议给仓库打上 `dsh-langpack` topic，便于 LexiForge 市场搜索发现；
- 发布前自检：四个文件白名单、无目录穿越、manifest 字段合法——插件安装时会再次全量校验，不合格会被拒载并提示原因。

> ⚠️ 免责声明：本仓库四个语言包为 LexiForge 作者提供的演示包，供学习语言包格式使用。从任何来源安装语言包前，请阅读插件内置《第三方语言包免责声明》。
