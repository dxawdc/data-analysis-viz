# data-analysis-viz

一个把「数据 → 洞察 + 图表」全流程固化下来的 WorkBuddy Skill：**喂进一份数据，按固定流程走完数据体检、清洗、统计特征、关键指标、图表匹配、可视化设计、洞察交付**。

配套公众号文章《我做了一个数据分析与可视化 Skill：从数据体检到出图洞察的完整流程》（可以叫我才哥）。

## 它解决什么问题

拿到一份数据，最常见的困境不是"不会画图"，而是**画出来的图回答不了问题**：

- 一行数据代表什么、这个数的分母是谁、口径有没有混用？
- 该看均值还是中位数？分布偏了该怎么报？
- 手上这堆字段，到底该用折线图、柱状图还是热力图？
- 图出来了，但它有没有误导读者？

这个 Skill 把这些问题收敛成七步固定流程，每一步都有明确产出，其中"数据体检"和"选图建议"两步有脚本可直接跑。

## 七步工作流

| 步骤 | 做什么 | 产出 |
| --- | --- | --- |
| 1 接收与对齐 | 一行代表什么、口径是什么、要回答什么问题 | 假设清单 |
| 2 数据体检 | 规模、类型、缺失、重复、异常、粒度 | 体检报告 |
| 3 清洗 | 按"能标不删、能改不猜"的阶梯处理，全程留痕 | 清洗后数据 + 留痕 |
| 4 统计特征 | 集中趋势、离散、偏度、分位数、相关性 | 特征清单 |
| 5 关键指标 | 用"结果 / 驱动 / 诊断 / 护栏"四层收敛到 3–7 个 | 指标口径 |
| 6 图表匹配 | 从分析意图推导图型，校验视觉通道精度 | 候选图表 + 理由 |
| 7 设计与交付 | 按准确性/可读性/有效性/完整性四维打磨 | 图表 + 洞察摘要 |

## 目录结构

| 路径 | 说明 |
| --- | --- |
| `SKILL.md` | Skill 入口：工作流、运行规则、触发条件 |
| `references/01-数据概览与清洗.md` | 字段角色判定、脏数据类型、清洗阶梯与留痕 |
| `references/02-统计特征提取.md` | 稳健统计量、分布形态、相关性陷阱 |
| `references/03-关键指标识别.md` | 指标四层结构、口径自包含定义、恒等式拆解 |
| `references/04-图表匹配.md` | 按意图选图、视觉通道精度排序、数据规模适配 |
| `references/05-可视化设计原则.md` | 准确性/可读性/有效性/完整性四维原则 |
| `references/06-洞察摘要与交付.md` | 洞察摘要模板、因果状态标注、行动与护栏 |
| `references/source-synthesis.md` | 方法来源与整合说明 |
| `scripts/profile_data.py` | 数据体检：输出 JSON + Markdown 报告 |
| `scripts/suggest_chart.py` | 选图建议：按意图 + 字段类型 + 规模推荐图型 |
| `scripts/viz_style.py` | matplotlib 统一样式（中文字体、配色、图注、导出） |
| `demo/数据模拟.py` | 生成七套演示数据（固定种子，可复现） |
| `demo/数据分析.py` | 完整实测：清洗 → 统计 → 生成 8 张图 |
| `demo/洞察摘要.md` | 按交付模板产出的洞察摘要示例 |
| `demo/reports/` | 七套数据的体检报告与统计特征汇总 |
| `demo/images/` | 图 1—图 8 |
| `data/raw/` | 演示数据（模拟生成，无业务含义） |

## 安装

将本仓库的 Skill 文件放到 WorkBuddy 的用户级 skill 目录：

```bash
git clone https://github.com/dxawdc/data-analysis-viz.git /tmp/dav
mkdir -p ~/.workbuddy/skills/data-analysis-viz
cp -r /tmp/dav/SKILL.md /tmp/dav/references /tmp/dav/scripts ~/.workbuddy/skills/data-analysis-viz/
```

Windows PowerShell：

```powershell
git clone https://github.com/dxawdc/data-analysis-viz.git "$env:TEMP\dav"
$dst = "$env:USERPROFILE\.workbuddy\skills\data-analysis-viz"
New-Item -ItemType Directory -Force $dst | Out-Null
Copy-Item -Recurse "$env:TEMP\dav\SKILL.md","$env:TEMP\dav\references","$env:TEMP\dav\scripts" $dst
```

安装后在 WorkBuddy 对话里直接给出数据文件并提出分析需求即可触发（关键词：数据分析、数据可视化、数据清洗、图表推荐、洞察报告等）。

## 运行环境

- Python 3.10+
- 依赖：`pandas`、`numpy`、`matplotlib`

```bash
pip install -r requirements.txt
```

## 快速开始

只用体检脚本（最常用）：

```bash
python scripts/profile_data.py 你的数据.csv --out 体检报告
```

选图建议：

```bash
python scripts/suggest_chart.py --intent compare --dim-unique 31 --label-len long
python scripts/suggest_chart.py --intent trend --series 8 --time-points 240
```

复现本文全套示例（先生成数据，再跑分析，两步都很快）：

```bash
python demo/数据模拟.py     # 生成七套模拟数据到 data/raw/
python demo/数据分析.py     # 清洗 + 统计 + 生成 8 张图到 demo/images/
```

七套数据覆盖七种分析意图：比较（渠道月度）、分布（用户时长）、趋势（月度经营）、相关性（城市指标）、集中度（订单流水）、分类×分类（活跃时段）、趋势+构成（品类构成）。

数据里故意埋入了常见质量问题用于演示：重复行、渠道名同义异写、收入单位混用、缺失值、毫秒误记为秒。可以对照 `demo/reports/` 的体检报告，看 Skill 如何识别与处置。

## 方法来源

- **可视化设计原则**：整理自《数据可视化设计指南：从数据到新知》（蓝星宇，2023）中的视觉通道、图表分类与设计打磨部分，为方法论的归纳与转述，非原文转载。
- **分析方法体系**：整理自游戏数据分析领域的指标口径、恒等式拆解与决策报告结构，并泛化到通用表格数据场景。
- 视觉通道准确性排序（位置 > 长度 > 角度/斜率 > 面积/体积/密度）来自 Cleveland 与 McGill 1984 年的知觉实验结论。

## 局限

- 覆盖静态图表；交互式可视化、实时看板不在范围内。
- 不替代业务判断：脚本负责把可疑数据挑出来，异常值判定、口径确认仍需回到业务值域核对。
- 设计原则整理自 2023 年出版物，具体工具的界面与默认行为以官方文档为准。

## 许可

代码以 MIT 许可发布；文档部分为方法归纳，引用请注明出处。
