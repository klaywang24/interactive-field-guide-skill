# Interactive Field Guide · Claude Skill

> English · [README.md](README.md)

一个 Claude skill，把任何研究主题——公司、行业、赛道、战略问题——打成一份带侧栏导航、SVG 生态图、可点击 drawer、accordion、paired-bar 图表的精致 HTML field guide。**22 个 part 的编辑结构**。

按 VC 投决会 / 顶级二级 fund / CB Insights 行研报告的深度标准来。

---

## 它解决什么问题

写深度研究的时候，三种格式都不够用：

- **Markdown 文档** — 扁平，没法 explore，难快速 skim
- **PPT / Keynote** — 视觉好但牺牲文字密度
- **临时拼的 HTML** — 每次都要重新设计 CSS / 交互

这个 skill 用一个 1500 行的暖米色 + 砖红编辑风模板（已经把 sidebar 导航、⌘K 全文搜索、⌘G 术语表、深色模式、可点击 drawer、SVG 星座图、2×2 矩阵、accordion、paired-bar 图、tab 案例切换、8 大评估维度 grid 全部 wire up 好了），让 Claude 把任何研究主题做成一份比 markdown 更有信息密度、比临时 HTML 更视觉统一的可探索文档，并跑 6 项验证脚本确保产物质量。

---

## 质量标准

不是"博客 + 图表"——是**投决会级别的研究产物**：

- ✅ **每条事实带来源** — `[Source: SEC 10-K Q4 FY2026 / Bloomberg / 公司 PR]`。不允许"据估计" / "约"。
- ✅ **每个判断可证伪** — 不写"增长强劲"，写 `YoY +65% 至 $215.9B`；不写"领先"，写 `市占率 31.4%, 3 年 +12pp`。
- ✅ **每篇 3+ 反共识判断** — 直接挑战常识。"NVIDIA 真正的护城河是 CUDA + 网络栈，不是芯片 raw FLOPS" 比"NVIDIA 是 AI 龙头"好。
- ✅ **判断章节必有可证伪信号** — `Tier 1 如果 Q2 FY27 guidance > $50B; 下调 Tier 2 如果 < $42B`。

---

## 安装

### 方式 A：clone 然后打包

    git clone https://github.com/klaywang24/interactive-field-guide-skill.git
    cd interactive-field-guide-skill
    zip -r ../interactive-field-guide.zip . -x ".git/*" -x "README*.md" -x "LICENSE"

### 方式 B：从 Releases 下载

直接到 [Releases 页面](https://github.com/klaywang24/interactive-field-guide-skill/releases) 下载现成的 zip（如有发布）。

### 在 Claude.ai 里启用

1. Settings → Capabilities → 打开 **Code execution and file creation**
2. Customize → Skills → **+** → **Create skill** → **Upload a skill**
3. 选刚才的 zip
4. 在 skill 列表里把 toggle 打开

详细官方文档：[support.claude.com/en/articles/12512180](https://support.claude.com/en/articles/12512180)

---

## 怎么触发这个 skill

装好之后，跟 Claude 说类似下面的话，skill 会自动 trigger：

| 想要 | 可以这样说 |
|---|---|
| 行业全景 | "做一份 [赛道] 的 field guide" |
| 公司深度分析 | "分析下英伟达基本面" / "研究一下小红书的商业模式" |
| 战略生态地图 | "Map 出 [公司] 过去 12 个月的战略生态——所有合作方、投资、收购" |
| 个股研究 | "[股票] 值不值得买" / "[公司] 怎么样" |
| 板块格局 | "[赛道] 的竞争格局分析" / "半导体板块龙头都有哪些" |
| 创业 / 投资 thesis | "AI Agent 创业指南" / "做一份新能源车的投资 thesis" |

英文 trigger 也行——`Deep dive on [Company]` / `How does [Company] make money?` / `Competitive landscape of [vertical]`.

---

## 内部结构

| 文件 | 作用 |
|---|---|
| `SKILL.md` | 主入口——完整工作流 + 反模式清单 + 验证脚本 |
| `assets/template.html` | 1500 行暖米色 + 砖红编辑风模板，所有交互组件已 wire up |
| `references/structure.md` | 22-part 编辑结构详解：每个 part 何时用、何时跳 |
| `references/data-schemas.md` | 6 个 JS 数据对象的字段说明 + 所有组件的 HTML 模板 |
| `references/content-strategy.md` | VC / CB Insights 深度标准；11 类来源；反共识框架 |
| `references/pitfalls.md` | 10 个最容易踩的坑 + 一键验证脚本 |

---

## 22-Part 编辑结构

每份 field guide 从 22 个 part 里挑 10–22 个用。**Skip 规则严格**——填稀释 part 比跳过 part 更糟。

| # | Part | 等级 | 何时跳过 |
|---|---|---|---|
| 1 | 第一性原理 / 序言 | ✅ 核心 | 极少跳 |
| 2 | 数据可信度 | 🟡 可选 | 单一权威源时 |
| 3 | 核心条件 / 框架 | ✅ 核心 | 纯财务分析 |
| 4 | 锚点案例 | 🟡 可选 | 没有强代表性单一案例 |
| 5 | 板块深度（Tab）| ⭐ 重要 | 单公司分析 |
| 6 | 横向对比表 | ✅ 核心 | 单维度题 |
| 7 | Constellation Map（SVG）| ✅ 核心 | 极少跳 |
| 8 | 玩家原型 2×2 战略象限 | ✅ 核心 | 玩家不足以分 4 类 |
| 9 | 死亡 vs 成功 2×2 | 🟡 可选 | 新兴 / hyper-hyped 题 |
| 10 | 人才 / 资源热力图 | 🟡 可选 | 单公司题 |
| 11 | 趋势时间线 | ✅ 核心 | 纯未来预测题 |
| 12 | 死因 donut 分布 | ⚠ 少用 | 样本不足 |
| 13 | 痛点矩阵 / N 大原则（accordion）| ⭐ 重要 | 纯财务题 |
| 14 | N 大评估维度 / DD 支柱 | ⭐ 重要 | 调性偏教育 / 科普 |
| 15 | 退出 / 并购策略 | 🟡 可选 | 公开市场公司基本面 |
| 16 | 冷启动 3 路径 | ⚠ 少用 | 成熟公司分析 |
| 17 | N 大根本分歧 | ✅ 核心 | 共识强题 |
| 18 | GTM 策略库 | 🟡 可选 | 纯研究 / 财务题 |
| 19 | 验证方法 grid | 🟡 可选 | 结果题 / 财务题 |
| 20 | 前沿话题 / 重大变量 | ✅ 核心 | 完全静态历史题 |
| 21 | 综合判断 + 90 天行动 | ⭐ 重要 | 极少跳 |
| 22 | 术语 + 数据来源 | ✅ 核心 | 永不跳 |

不同主题 typical 用几个 part：

- **公司基本面**（NVIDIA 这种）：~12 个 part
- **战略生态地图**（Stripe 这种）：~11 个 part
- **行业 / 板块**：~14 个 part
- **完整行业学习手册**（Fintech 这种）：18–22 个 part

完整规则见 [references/structure.md](references/structure.md)。

---

## 设计原则

1. **优先独立分析师** — 公司 / 行业题先搜 Net Interest（Marc Rubinstein）/ Bits about Money（patio11）/ Acquired / Stratechery（Ben Thompson）/ Business Breakdowns / The Diff（Byrne Hobart）/ Not Boring（Packy McCormick）/ Money Stuff（Matt Levine），再考虑新闻聚合站。一篇 Net Interest 的深度分析顶五条 Reuters 头条。
2. **直接源胜过聚合源** — SEC 10-K / 8-K 直读、官方 IR、监管申报文件，胜过二手摘要。
3. **判断必须可证伪** — 每条结论附"如果 X 发生，我的判断错"。
4. **宁缺毋滥** — 12 个高密度 part > 22 个稀释 part。

完整 sourcing playbook 见 [references/content-strategy.md](references/content-strategy.md)。

---

## 兼容性

| 工具 | 能用？ |
|---|---|
| claude.ai（网页 / 桌面 / 手机）| ✅ |
| Claude Code（CLI）| ✅ |
| Claude API（开发者）| ✅ |
| Codex / ChatGPT | ❌ 非 Anthropic 生态 |
| Cursor / Copilot / 其他 | ❌ |

Skills 是 Anthropic 专属格式。需要 Claude 付费账号。

---

## License

Apache-2.0 — 见 [LICENSE](LICENSE)。
