# Interactive Field Guide Skill

> 把任何研究主题，变成一份精致、可交互的 HTML 研究报告——侧栏导航、⌘K 全文搜索、SVG 生态图、2×2 战略矩阵、22-part 编辑结构。**按 VC / CB Insights 深度标准产出。**

[![Agent Skills](https://img.shields.io/badge/Agent_Skills-open_standard-blue)](https://agentskills.io)
[![Release](https://img.shields.io/github/v/release/klaywang24/interactive-field-guide-skill)](https://github.com/klaywang24/interactive-field-guide-skill/releases)
[![License](https://img.shields.io/badge/license-Apache_2.0-green)](LICENSE)
[![Stars](https://img.shields.io/github/stars/klaywang24/interactive-field-guide-skill)](https://github.com/klaywang24/interactive-field-guide-skill/stargazers)
[![Downloads](https://img.shields.io/github/downloads/klaywang24/interactive-field-guide-skill/total)](https://github.com/klaywang24/interactive-field-guide-skill/releases)
[![Last Commit](https://img.shields.io/github/last-commit/klaywang24/interactive-field-guide-skill)](https://github.com/klaywang24/interactive-field-guide-skill/commits)
[![English](https://img.shields.io/badge/English-README-blue)](README.md)

---

## 这是什么

你说一句：**"分析下英伟达基本面"** 或 **"研究下半导体行业"** 或 **"Map 出 Stripe 的战略生态"**

它给你的：一个独立 HTML 文件（约 110KB），带侧栏导航、⌘K 全文搜索、可点击展开的 drawer、可点的 SVG 生态节点图、2×2 战略定位矩阵，以及最多 22 个 part 的结构化分析——**每条事实带来源，每个判断可证伪**，并且**显式提出反共识观点**（市场错在哪里、为什么）。

**质量标准**：对标 Net Interest、Stratechery、CB Insights 研究报告的水准。是 VC 合伙人开投决会前会读的那种深度。

[看一份真实样例 →](https://github.com/klaywang24/interactive-field-guide-skill/releases) （NVIDIA 基本面分析，119KB HTML）

---

## 兼容性

本 skill 遵循开放 [Agent Skills 标准](https://agentskills.io)（Anthropic 于 2025 年 12 月 18 日发布）。**任何兼容 agent 都能加载它**——但**输出质量取决于底层模型**。

| Agent | 安装路径 | 状态 |
|---|---|---|
| **Claude.ai** | Settings 上传 zip | ✅ 参考实现 |
| **Claude Code** | `~/.claude/skills/` | ✅ 参考实现 |
| **Codex CLI**（OpenAI）| `~/.codex/skills/` | 🟡 格式兼容，输出未验证 |
| **Gemini CLI**（Google）| `~/.gemini/skills/` | 🟡 格式兼容，输出未验证 |
| **Cursor** | `.cursor/skills/`（仅项目级）| 🟡 格式兼容，输出未验证 |

> **🟡 的含义**：SKILL.md 格式能被加载，但我没有亲自验证过这些平台跑出来的 HTML 质量。本 skill 的设计前提是：**强推理 + 长 context + 联网搜索**——所以质量在 Claude Opus / GPT-5 / Gemini 2.5 Pro 上最好，小模型上可能偏浅。

---

## 安装

### 1. Claude.ai（推荐非开发者用）

1. 下载最新 zip：[releases 页面](https://github.com/klaywang24/interactive-field-guide-skill/releases)
2. Claude.ai → **Settings** → **Capabilities** → 打开 **Code execution and file creation**
3. **Customize** → **Skills** → **+** → **Upload a skill** → 选 zip → 打开开关
4. 试一句：*"分析下英伟达基本面"*

### 2. Claude Code

```bash
git clone https://github.com/klaywang24/interactive-field-guide-skill.git \
  ~/.claude/skills/interactive-field-guide
```

重启 Claude Code，skill 会被自动识别。

### 3. Codex CLI（OpenAI）

```bash
# 启用 skills 特性开关（一次性）
codex --enable skills

# 安装 skill
git clone https://github.com/klaywang24/interactive-field-guide-skill.git \
  ~/.codex/skills/interactive-field-guide
```

重启 Codex，会话内输入 `/skills` 验证。

**触发方式：**
- 隐式：`codex "分析下英伟达基本面"`
- 显式：`codex "$interactive-field-guide 研究 Stripe"`

### 4. Gemini CLI（Google）— 见下方[配置说明 ↓](#gemini-cli-配置)

```bash
# 一行命令，Gemini CLI 帮你 clone
gemini skills install https://github.com/klaywang24/interactive-field-guide-skill
```

或者手动：
```bash
git clone https://github.com/klaywang24/interactive-field-guide-skill.git \
  ~/.gemini/skills/interactive-field-guide
```

会话内 `/skills list` 验证；如果不重启就想看到，用 `/skills reload`。

### 5. Cursor — 见下方[配置说明 ↓](#cursor-配置)

⚠️ **Cursor 只支持项目级 skill**——没有全局 `~/.cursor/skills/` 目录。每个想用它的项目都要装一遍。

```bash
# 在你的项目根目录下
mkdir -p .cursor/skills
git clone https://github.com/klaywang24/interactive-field-guide-skill.git \
  .cursor/skills/interactive-field-guide
```

重新加载窗口：**Cmd/Ctrl+Shift+P** → **Developer: Reload Window**

在 Cursor agent 模式下用斜杠菜单（`/`）调用。

---

## 配置说明

本 skill 需要 agent 提供四种能力：**(1) 联网搜索**取最新事实，**(2) 文件写入**输出 110KB HTML，**(3) Python 或 Node** 跑校验脚本，**(4) 长 context**（约 50K token）容纳模板和 reference 文件。

Claude.ai / Claude Code 默认四项都有。Codex / Gemini / Cursor 有时候没全开。

### Gemini CLI 配置

Gemini CLI 自带超长 context（1M+ token），模板装得下没问题。要确认两件事：

**A. 联网搜索必须开。** Gemini CLI 自带 Google Search，但有些安装默认关闭。会话内：

```
/tools list
```

找 `google_search` 之类的工具。如果没有，装搜索扩展，或在 `~/.gemini/settings.json` 的 `tools` 里启用。

**B. 文件写入权限。** 写 HTML 时 Gemini 会弹窗问你授权。每个会话授权一次。想跳过：

```jsonc
// ~/.gemini/settings.json
{
  "tools": {
    "auto_approve": ["write_file"]
  }
}
```

**C. 工作区信任**（仅当装在 `.gemini/skills/` 工作区路径时需要）。会话内运行 `/trust` 然后重启，或者直接装在 `~/.gemini/skills/` 用户路径下，那个不需要 trust。

### Cursor 配置

Cursor 的主要差异是**HTML 输出的呈现方式**。Cursor 是编辑器优先，不是聊天优先——生成的文件出现在文件树里，不是聊天侧的 artifact。

**A. 用 Agent 模式，不要用 Ask。** 打开 chat 面板 → 顶部切换到 **Agent**（不是 **Ask** 或 **Edit**）。Skill 只在 Agent 模式下触发。

**B. 联网搜索必须开。** Cursor Settings → **Features** → **Agent** → 打开 **Web search**。在 chat 里输入 `@web` 验证选项有没有出现。

**C. 允许终端命令。** Skill 的校验脚本要跑 Node 或 Python。Settings → **Features** → **Agent** → 开 **Allow terminal commands**（或者按命令逐个授权）。

**D. 选强模型。** Settings → **Models** → 把 agent 模型设成 Claude Sonnet 4.6 / Opus 4.7 / GPT-5 / Gemini 2.5 Pro。**小模型 / 快模型会输出明显更浅的分析**——本 skill 的质量门槛假设有强推理能力。

**E. 输出位置。** Skill 把 HTML 写到你项目根目录。在 Cursor 文件树里找到它，右键 **Reveal in Finder/Explorer**，双击在浏览器打开。

---

## 不同模型的输出质量

诚实预期，基于本 skill 的设计要求（带来源 + 可证伪反共识 + 多源综合）：

| 模型 | 预期质量 | 备注 |
|---|---|---|
| Claude Opus 4.7 | ⭐⭐⭐⭐⭐ | 参考实现，skill 在 Opus 上设计和测试 |
| GPT-5 / GPT-5-Codex | ⭐⭐⭐⭐ | 推理强，风格略不同，接近参考水准 |
| Gemini 2.5 Pro | ⭐⭐⭐⭐ | 长 context 优秀，接近参考水准 |
| Claude Sonnet 4.6 | ⭐⭐⭐⭐ | 更快更便宜，反共识深度比 Opus 略弱 |
| Gemini 2.5 Flash / GPT-4o-mini | ⭐⭐⭐ | 格式能跑通，深度打折，适合做初稿 |

如果输出感觉浅，**先换更强的模型再说**，别先怪 skill。

---

## 试试看

装好后，试试这些 prompt：

- *"分析下英伟达基本面"*
- *"研究下 Stripe 的战略生态"*
- *"做一份半导体行业深度报告"*
- *"Map 出 AI agent 赛道格局"*
- *"中国新能源车行业报告"*

Skill 会自动判断主题类型（单公司 / 行业 / 生态），从 22 part 里挑合适的子集——单公司基本面用约 12 part，行业分析用约 16，生态对比用约 18。

---

## 文件结构

```
interactive-field-guide-skill/
├── SKILL.md                       # 主指令，22-part 工作流
├── assets/
│   └── template.html              # 1500 行 HTML 模板（暖米色 + 砖红色）
└── references/
    ├── structure.md               # 22-part 菜单 + skip 规则
    ├── data-schemas.md            # 6 个 JS 对象 schema + 组件模式
    ├── content-strategy.md        # VC 深度标准、来源分级、反共识框架
    └── pitfalls.md                # 10 个已知坑 + 6 项 Node 校验脚本
```

22 part 编辑结构涵盖：hero 反共识、4 个核心 stat block、生态图、2×2 战略矩阵、商业模式、单位经济、增长归因、8 维度尽调（带 1-10 评分）、护城河、5 大基本面分歧（Bull vs Bear，带可证伪触发条件）、90 天行动清单（带数值阈值），等等。

完整 part 菜单和按主题类型的 skip 逻辑，见 [`references/structure.md`](references/structure.md)。

---

## 故障排除

**Skill 没触发。** 检查 `SKILL.md` 里 description 和你的 prompt 匹不匹配。Skill 看到 *分析 / 研究 / 深度 / 基本面 / 生态 / 行业报告* 这类关键词会激活。

**HTML 看起来坏了 / 缺章节。** 校验脚本会抓出大部分问题。重跑：*"重新跑一遍 field guide，跑完做 6 项校验"*。

**Gemini CLI / Cursor 上联网搜索没结果。** 见上面的[配置说明](#配置说明)——这俩平台默认可能没开搜索。

**输出太浅。** 大概率是模型问题不是 skill 问题。换 Claude Opus 4.7 / GPT-5 / Gemini 2.5 Pro 重跑。

**Cursor 看不到 skill。** 确认你装在了**项目根目录**的 `.cursor/skills/`，不是某个全局路径。装完重新加载窗口。

---

## 为什么叫 "field guide" 不叫 "report"

Report 是看一遍就放下的。Field guide 是你会一直开着、搜索、点来点去、反复回去看的。这个输出的设计是为后一种行为——**让你在里面待 30 分钟**，不是 3 分钟扫一眼。

---

## 协议

Apache 2.0。随便用、fork、改。欢迎 PR——特别是各个平台兼容性问题的修复。

## 作者

[@klaywang24](https://github.com/klaywang24) 制作。[Nexar](https://nexar.io) 创始人——为美国 DTC 品牌做创作者支付决策层。

如果这个 skill 对你有用，repo 给个 ⭐ 让更多人看到。
