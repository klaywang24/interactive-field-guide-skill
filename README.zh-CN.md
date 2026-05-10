# Interactive Field Guide Skill

> 给 AI agent 用的研究纪律框架。在公司、行业、市场、竞争、战略、投资研究主题上，产出精致的可交互 HTML 研究报告或结构化研究 brief——侧栏导航、⌘K 全文搜索、SVG 生态图、22-part 编辑结构。**6 种 mode × 7 种 persona。VC / CB Insights 深度。**

[![Agent Skills](https://img.shields.io/badge/Agent_Skills-open_standard-blue)](https://agentskills.io)
[![Release](https://img.shields.io/github/v/release/klaywang24/interactive-field-guide-skill)](https://github.com/klaywang24/interactive-field-guide-skill/releases)
[![License](https://img.shields.io/badge/license-Apache_2.0-green)](LICENSE)
[![Stars](https://img.shields.io/github/stars/klaywang24/interactive-field-guide-skill)](https://github.com/klaywang24/interactive-field-guide-skill/stargazers)
[![Downloads](https://img.shields.io/github/downloads/klaywang24/interactive-field-guide-skill/total)](https://github.com/klaywang24/interactive-field-guide-skill/releases)
[![Last Commit](https://img.shields.io/github/last-commit/klaywang24/interactive-field-guide-skill)](https://github.com/klaywang24/interactive-field-guide-skill/commits)
[![English](https://img.shields.io/badge/English-README-blue)](README.md)

---

## v3 新增什么

这**不只是个 HTML 生成器**。v3 把 skill 改造成一个**研究套件**——6 种操作模式按需调用，7 种分析视角彻底改变看问题的角度，2 种输出格式（HTML 报告或 markdown brief）可选。

它的设计是让你**对一个主题反复使用**，不是用一次就完。

| Mode | 什么时候用 |
|---|---|
| **Generate** | 新主题首轮研究 |
| **Discovery** | 重要决策——5 个问题对话产出更深的报告，最后让你选 brief 还是 HTML |
| **Critique** | "我自己写了个分析，帮我找漏洞" |
| **Refresh** ⭐ | "用最新数据更新我 Q1 那份 NVIDIA 报告"——季度复盘的 LTV 引擎 |
| **Compare** | "NVIDIA 和 AMD 用 side-by-side 表格对比" |
| **Drill-down** | "把客户集中度那个分歧扩成独立深度报告" |

### Personas 视角

**核心视角（高频使用）：**

| Persona | 它的视角 |
|---|---|
| **Tier 2 投资人**（公司股票默认）| "做多、做空、还是中性？什么价位？" |
| **行业战略家**（行业默认）| "5 年后利润池往哪迁移？" |
| **工程师** | "技术押注靠不靠谱？给我 benchmark" |
| **竞争对手** | "如果我是对手，从哪里下手攻击？" |

**进阶视角（特定场景）：**

| Persona | 它的视角 |
|---|---|
| **Tier 1 投资人** | "这是不是 fund-returner？为什么是现在？" |
| **CEO** | "如果明天我接手，第一件事做什么？" |
| **颠覆侦察兵** | "什么会在 5 年内杀死这个生意？" |

同一个主题，换个 persona = 完全不同的报告。NVIDIA 用 Tier 2 投资人视角，重点是 Q2 财报指引和客户集中度。NVIDIA 用颠覆侦察兵视角，重点是 Cerebras / Groq / 硅光子学的颠覆威胁。两份都有用，两份都犀利。

---

## 输出格式

默认：**可交互 HTML 报告**（约 110KB，侧栏导航、⌘K 全文搜索、可点击展开 drawer、SVG 生态图、2×2 战略矩阵、22-part 结构化分析）

可选：**Markdown 研究 brief**（1-2 页 memo，包含反共识假设、带来源的事实、steel-manned Bull / Bear、watch list）。在以下情况启用：
- 用户显式说要 "brief" 或 "markdown 总结"
- Discovery 模式结束后用户选 "只要 brief"

两种格式都遵循同一质量标准：每条事实带 `[Source: X]` 引用，每个判断可证伪，每份 artifact 都有显式反共识。

---

## 为什么做成套件，不是单一工具

大多数研究工具是一锤子买卖——问一次、出一份、用完丢。

真实研究不是这样。真实研究是：
1. 从多个角度看同一主题（= **personas**）
2. 时间推移后更新分析（= **Refresh** ⭐）
3. 决策前对比选项（= **Compare**）
4. 压力测试自己的结论（= **Critique**）
5. 钻进关键分歧（= **Drill-down**）
6. 思考过程中给自己 coaching（= **Discovery**）

Garry Tan 的 [gstack](https://github.com/garrytan/gstack) 在工程领域证明了这个道理——一个虚拟团队（CEO / 工程师 / 评审 / QA / Release）比一个万能助手强得多。这个 skill 把同样原理用到研究上。

**一个主题，6 种 mode，7 种 persona，反复使用。** 这就是设计哲学。

### LTV 在哪里

LTV 引擎是 **Refresh** 和 **Drill-down**：

- **Refresh**：市场每个季度都在变。Q1 做完 NVIDIA 报告的用户，Q2 回来要 refresh——Refresh 模式解析旧报告、刷新每个数字、标实质变化、告诉你反共识假设是否还成立。一句 prompt 替代 30 分钟工作。
- **Drill-down**：母报告里每个有意思的分歧都能扩展成独立深度报告。一份 NVIDIA 报告能 spawn 出 3-5 份 drill-down（客户集中度、NVL72 架构、中国敞口等）。

加上 persona 矩阵（同一主题 × 7 个 persona），一个用户、一个主题，**长期能产出十几份不同的报告**。这就是复利价值。

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

> **🟡 含义**：SKILL.md 格式能被加载，但我没有亲自验证过这些平台跑出来的 HTML 质量。本 skill 的设计前提是：**强推理 + 长 context + 联网搜索**——质量最好的是 Claude Opus / GPT-5 / Gemini 2.5 Pro。

---

## 边界——这个 skill **不是**什么

本 skill 产出**研究框架**，包含可证伪信号、带来源的事实、结构化的 Bull / Bear 分析。它**不是个性化投资建议**，**不能替代持牌财务顾问**。

输出被定位为分析 artifact，不是买卖建议。每份 HTML 和 markdown 输出底部都会带这个免责声明。

如果你需要个性化投资建议，请咨询持牌顾问。

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
codex --enable skills

git clone https://github.com/klaywang24/interactive-field-guide-skill.git \
  ~/.codex/skills/interactive-field-guide
```

重启 Codex，会话内输入 `/skills` 验证。

### 4. Gemini CLI（Google）— 见下方[配置说明 ↓](#gemini-cli-配置)

```bash
gemini skills install https://github.com/klaywang24/interactive-field-guide-skill
```

### 5. Cursor — 见下方[配置说明 ↓](#cursor-配置)

```bash
mkdir -p .cursor/skills
git clone https://github.com/klaywang24/interactive-field-guide-skill.git \
  .cursor/skills/interactive-field-guide
```

重新加载窗口：**Cmd/Ctrl+Shift+P** → **Developer: Reload Window**

---

## 怎么用各个 mode

### Generate — 一次性研究

```
"分析下英伟达基本面"
"研究下半导体行业"
"Map 出 Stripe 的战略生态"
```

Skill 自动识别 mode + persona。可以显式指定：

```
"分析下英伟达基本面，用 Tier 1 投资人视角"
"研究下半导体行业，从 CEO 视角"
"Map 出 Stripe 的战略生态，用竞争对手视角"
```

### Discovery — 通过对话产出更深报告

```
"先帮我把 NVIDIA 想清楚再生成报告"
"对 AI agent 赛道做研究对话"
```

Skill 走 5 个问题（共识 → 反证 → 假设 → 事实 → steel-man Bull/Bear），**然后问你**：**"要 markdown 简短 brief，还是要完整可交互 HTML 报告？"**

你选。Brief 更轻量更快，HTML 是完整 artifact。

**什么时候用 Discovery**：高 stakes 决策、仓位决定、要给老板看的内部 memo，或者纯粹想先理清思路再生成。

### Critique — 对抗式审查

```
"看一下我这份 NVIDIA 分析，找漏洞"
"这份报告评判一下——找未注明出处的论断和弱反共识"
```

提供你已有的分析（粘贴文本、上传文件）。Skill 产出 **Markdown 评论**（不出 HTML）——最强论断、最弱论断、缺什么、具体怎么修。

**什么时候用**：报告给 stakeholder 看之前。压力测试自己的工作。

### Refresh — 用最新数据更新 ⭐

```
"用最新数据更新我 Q1 那份 NVIDIA 报告"
"复盘我 1 月做的 Stripe 分析"
"我那份 AI agent 赛道格局报告——三个月过去了发生了什么变化？"
```

提供之前的报告（上传 HTML 或粘贴内容）。Skill 会：
- 把每个数字对照当前数据更新
- 标出实质变化（>10% 偏移、定性反转）
- 重新评估你的反共识假设（还成立？已被证伪？已被验证？）
- 在报告顶部加"自 [日期] 以来的变化"提示框

**这是 LTV 模式。** 市场每个季度都在变。每个你持有的标的，每个季度用一次 Refresh。

### Compare — 平行对比

```
"NVIDIA 和 AMD 做对比"
"Stripe vs Adyen——哪个更适合做多？"
"Cursor vs GitHub Copilot——技术 head-to-head"
```

Skill 产出对比研究报告，**hero 区有对比汇总表（10-15 维度）**，**各章节内有 side-by-side 小表**，最后给明确评分裁定。（完整 2 列镜像布局在 v3.x roadmap 上；当前版本用标准模板 + 对比表，实操效果已经够好。）

### Drill-down — 扩展子章节

```
"我那份 NVIDIA 报告里客户集中度那段，扩成独立报告"
"把半导体报告里 'AI 工厂运营' 那个论点单独做一份"
"把硅光子颠覆那个场景扩成独立 field guide"
```

提供母报告 + 指定章节。Skill 会把那个子章节当作**新报告的全部主题**——做完整的 22-part 处理。

---

## 配置说明

本 skill 需要 agent 提供四种能力：**(1) 联网搜索** 取最新事实，**(2) 文件写入** 输出 HTML，**(3) Python 或 Node** 跑校验脚本，**(4) 长 context**（约 50K token）容纳模板和 reference 文件。

Claude.ai / Claude Code 默认四项都有。Codex / Gemini / Cursor 有时候没全开。

### Gemini CLI 配置

Gemini CLI 自带超长 context（1M+ token），模板装得下没问题。要确认两件事：

**A. 联网搜索必须开。** 会话内：`/tools list` —— 找 `google_search`。如果没有，装搜索扩展或在 `~/.gemini/settings.json` 启用。

**B. 文件写入权限。** 写 HTML 时 Gemini 会弹窗。想跳过：

```jsonc
// ~/.gemini/settings.json
{
  "tools": {
    "auto_approve": ["write_file"]
  }
}
```

**C. 工作区信任**（仅 `.gemini/skills/` 时需要）。运行 `/trust`，或装在 `~/.gemini/skills/` 用户路径下。

### Cursor 配置

**A. 用 Agent 模式。** Chat 面板顶部 → 选 **Agent**。Skill 只在 Agent 模式触发。

**B. 联网搜索必须开。** Settings → **Features** → **Agent** → 打开 **Web search**。

**C. 允许终端命令。** Settings → **Features** → **Agent** → 开 **Allow terminal commands**。

**D. 选强模型。** Settings → **Models** → Claude Sonnet 4.6 / Opus 4.7 / GPT-5 / Gemini 2.5 Pro。

**E. 输出位置。** Skill 自动检测平台，把 HTML 写到当前工作区根目录。文件树里找 → 右键 **Reveal in Finder/Explorer**。

---

## 不同模型的输出质量

| 模型 | 预期质量 | 备注 |
|---|---|---|
| Claude Opus 4.7 | ⭐⭐⭐⭐⭐ | 参考实现，skill 在 Opus 上设计和测试 |
| GPT-5 / GPT-5-Codex | ⭐⭐⭐⭐ | 推理强，风格略不同 |
| Gemini 2.5 Pro | ⭐⭐⭐⭐ | 长 context 优秀 |
| Claude Sonnet 4.6 | ⭐⭐⭐⭐ | 更快更便宜，反共识深度略弱 |
| Gemini 2.5 Flash / GPT-4o-mini | ⭐⭐⭐ | 格式能跑通，深度打折，适合做初稿 |

如果输出感觉浅，**先换更强的模型再说**，别先怪 skill。

---

## 文件结构

```
interactive-field-guide-skill/
├── SKILL.md                       # 轻量级路由器：mode + persona + 格式调度
├── assets/
│   └── template.html              # 1500 行 HTML 模板
└── references/
    ├── modes.md                   # 6 种操作模式的工作流
    ├── personas.md                # 7 种分析视角的 lens（核心 + 进阶）
    ├── structure.md               # 22-part 菜单 + skip 规则
    ├── data-schemas.md            # JS 对象 schema + 组件模式
    ├── content-strategy.md        # 来源分级、反共识、可证伪
    └── pitfalls.md                # 已知坑 + 校验脚本
```

**架构**：SKILL.md 是路由器。它识别 mode + persona + 输出格式，派发到 `references/modes.md` 的对应工作流和 `references/personas.md` 的对应视角。Personas 描述的是"看问题的角度"（topical emphasis），不是具体章节编号——agent 在执行时把它映射到 `structure.md` 当前的 22-part 结构上。

22 part 编辑结构涵盖：hero 反共识、stat block、生态图、战略矩阵、商业模式、单位经济、增长归因、多维度尽调、护城河、基本面分歧（Bull vs Bear，带可证伪触发条件）、90 天行动清单（带数值阈值），等等。

---

## 试试看

装好后，每个 mode 都试一句感受一下：

**Generate**（一锤子）：
- *"分析下英伟达基本面"*
- *"Map 出 AI agent 赛道格局"*

**Discovery**（先想清楚，再选 brief 或 HTML）：
- *"先帮我把 Stripe 的战略地位想清楚再生成"*

**Critique**（对抗式自审）：
- *"看一下我这份 [主题] 分析，找漏洞"*

**Refresh**（更新旧报告）：
- *"用最新数据更新我 Q1 那份 NVIDIA 报告"*

**Compare**（对比）：
- *"NVIDIA 和 AMD 做对比，用 Tier 2 投资人视角"*

**Drill-down**（扩展子章节）：
- *"从我 NVIDIA 报告里钻进客户集中度那段"*

**Persona 显式指定**（任何 mode）：
- *"...用工程师视角"*
- *"...用颠覆侦察兵视角"*
- *"...从 CEO 角度"*

**格式指定**：
- *"...输出 markdown brief，不要 HTML"*

---

## Roadmap

**2026 Q2**（开发中）：
- Compare 模式真正的 2-列镜像布局（当前用对比表）
- `scripts/validate-field-guide.js` 和 `scripts/validate-compare.js` 独立脚本（当前内嵌在 pitfalls.md）
- pitfalls.md 加 mode-specific 校验

**2026 Q3**（计划中）：
- "Watch list" mode——被动追踪 5-10 个标的，发生实质变化时自动告警
- `agents/openai.yaml` 给 Codex / OpenAI 生态更好展示
- 提交到 skills.sh 市场
- 注册到 Claude Code Plugin Marketplace

**2026 Q4**（探索中）：
- 语音模式 Discovery（替代打字对话）
- 协作式 Critique（多个 agent 辩论同一份分析）
- 双语自动输出（中英切换）

---

## 故障排除

**Skill 没触发。** 触发是有意收紧的——只在用户要"公司 / 行业 / 市场 / 竞争 / 战略 / 投资研究 artifact"时触发。普通问答、BI 数据查询、简单总结**不会**触发。如果想强制触发，说"给 X 生成一份 field guide"或"X 的研究 artifact"。

**Mode 识别错了。** 显式说："用 Discovery 模式分析 X" 或 "对这份做 Critique"。

**Persona 感觉不对。** 显式指定："...用 Tier 1 投资人视角" 或 "...用工程师 lens"。

**HTML 看起来坏了 / 缺章节。** 重跑：*"重新跑一遍 field guide，跑完做校验"*。

**Gemini CLI / Cursor 上联网搜索没结果。** 见上面的[配置说明](#配置说明)——这俩平台默认可能没开搜索。

**输出太浅。** 大概率是模型问题。换 Claude Opus 4.7 / GPT-5 / Gemini 2.5 Pro 重跑。

**Refresh 模式找不到旧数据。** 确认你上传或粘贴了之前的 field guide。Skill 没有跨会话记忆，必须由你提供前次内容。

**Compare 模式看起来还是单列。** v3.0-beta 用对比表实现，不是镜像 2 列布局。完整镜像布局在 Q2 roadmap 上。

---

## 为什么叫 "field guide" 不叫 "report"

Report 是看一遍就放下的。Field guide 是你会一直开着、搜索、点来点去、反复回去看的。这个输出的设计是为后一种行为。

但更重要：field guide 是给"导航"用的，不是给"展示"用的。22-part 结构、可证伪判断、带来源的事实——这些不是格式，是让产物**值得信任**的研究纪律。

Skill 的设计就是把这套纪律编码进来。HTML 是产物。**纪律才是产品。**

---

## 协议

Apache 2.0。随便用、fork、改。欢迎 PR——特别是平台兼容性问题的修复、新 persona 的贡献、mode-specific 校验脚本。

## 作者

[@klaywang24](https://github.com/klaywang24) 制作。[Nexar](https://nexar.io) 创始人——为美国 DTC 品牌做创作者支付决策层。

如果这个 skill 对你有用，repo 给个 ⭐ 让更多人看到。如果**很**有用，发一张你跑出来的 field guide 截图——这是最高级的赞美。
