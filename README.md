# Serenity Chokepoint Investing Framework (Enhanced v2.0)

> **Find the bottleneck first. Find the listed exposure second. Verify through evidence third. Size by catalyst window fourth.**
>
> 先找到真正的瓶颈。再找到上市企业的敞口。然后通过证据验证。最后根据催化剂窗口确定仓位。

---

## Overview

An enhanced research skill for AI infrastructure supply-chain chokepoint investing, compatible with **OpenClaw / Hermes** agent platforms.

This is NOT a buy/sell recommendation engine. It is a structured research workflow that helps an agent identify whether a company sits inside a real supply-chain bottleneck created by AI infrastructure expansion, and whether the market has underpriced the factor exposure change driven by milestone events.

一套增强版AI基础设施供应链瓶颈投资研究技能，兼容 **OpenClaw / Hermes** 智能体平台。

这不是一个买入/卖出推荐引擎。它是一个结构化研究工作流，帮助智能体识别一家公司是否处于AI基础设施扩张所创造的真正供应链瓶颈中，以及市场是否低估了由里程碑事件驱动的因子暴露变化。

---

## What's New in v2.0

### 1. Two-Stage Multiplicative Model / 两阶段乘法模型

**Original (v1.0):** Linear additive scoring (100 points). A "strong bottleneck but fully priced" opportunity could still score 80+ and be classified as attractive.

**原版(v1.0)：** 线性相加评分（100分制）。一个"强瓶颈但已充分定价"的机会仍然可以拿到80+分并被归类为有吸引力。

**Enhanced (v2.0):** Multiplicative probability model:
```
Alpha Score = P(is_bottleneck) × P(mispriced | is_bottleneck)
              × P(catalyst_within_window | mispriced)
              × P(liquidity_sufficient)
```

A real bottleneck that is fully priced correctly scores near zero. A weak bottleneck that is undervalued is penalized at the bottleneck filter stage.

**增强版(v2.0)：** 乘法概率模型。一个已充分定价的真瓶颈会正确地被评分为接近零。一个被低估的弱瓶颈会在瓶颈过滤阶段就被惩罚。

### 2. Supply-Chain Ontology by Process Node / 按工艺节点的供应链本体

**Original (v1.0):** Grouped "InP, GaAs, SOI, substrates, epitaxy wafers" into a single "Materials layer." This conflated upstream substrate (e.g., 鑫耀半导) with downstream epitaxy (e.g., 三安光电) — fundamentally different investment opportunities.

**原版(v1.0)：** 将"InP、GaAs、SOI、衬底、外延片"混在同一个"材料层"。这混淆了上游衬底（如鑫耀半导）和下游外延（如三安光电）——根本不同的投资机会。

**Enhanced (v2.0):** Reclassifies by process node:
```
Raw Material → Substrate → Epitaxy → Device Fabrication
→ Module Assembly → System Integration
```

Each node has independent competitive dynamics, barrier heights, and pricing power profiles.

**增强版(v2.0)：** 按工艺节点重新分类。每个节点有独立的竞争动态、壁垒高度和定价权特征。

### 3. Evidence Stage Transition Map / 证据阶段转移图

**Original (v1.0):** Static Evidence Level (A/B/C/D) — a snapshot, not a process.

**原版(v1.0)：** 静态证据等级（A/B/C/D）——快照，不是过程。

**Enhanced (v2.0):** Dynamic stage map with transition probabilities:

```
Concept → Sample Submission → Pilot Order → Mass Production → Primary/Exclusive Supplier
   C级         C+级            B级           B+/A-级              A级
```

For each company, the agent estimates:
- Current stage / 当前阶段
- P(advancement within 1-2 quarters) / 1-2个季度内进阶概率
- Advancement triggers / 进阶触发信号
- Regression triggers / 回归触发信号（止损依据）

### 4. Cross-Validation Layer / 交叉验证层

**Original (v1.0):** Analyzed each company in isolation.

**原版(v1.0)：** 孤立分析每家公司。

**Enhanced (v2.0):** Compares financials across the supply chain:
- Target company: inventory, prepayments, capex, backlog / 目标公司：存货、预收款、资本开支、积压订单
- Downstream customers: COGS, inventory, capex guidance / 下游客户：销售成本、存货、资本开支指引
- Upstream suppliers: capacity utilization, pricing / 上游供应商：产能利用率、定价

Detects **resonance signals** (consistent trends across nodes) and **divergence signals** (contradictory trends requiring explanation).

检测**共振信号**（跨节点趋势一致）和**背离信号**（矛盾趋势需要解释）。

### 5. Fact-Opinion-Inference Tagging with Triangulation / 事实-观点-推断标注与三角验证

**Original (v1.0):** No systematic fact discrimination.

**原版(v1.0)：** 无系统的事实区分机制。

**Enhanced (v2.0):** Dual-layer verification:
- **Layer 1:** Every statement tagged as `[FACT:source]`, `[OPINION:holder]`, or `[INFERENCE:chain]`
- **Layer 2:** Verification subagent cross-checks every fact against non-correlated sources, promotes to `[TRIANGULATED_FACT]`

**增强版(v2.0)：** 双层验证：
- **层1：** 每条陈述标注为 `[FACT:来源]`、`[OPINION:持有者]` 或 `[INFERENCE:推理链]`
- **层2：** 验证子代理对每个事实进行非同源交叉核验，晋升为 `[TRIANGULATED_FACT]`

### 6. Catalyst-Driven Position Sizing / 催化剂驱动仓位管理

**Original (v1.0):** Fixed sizing by portfolio role (Core: 10-20%, Thematic: 5-10%, etc.).

**原版(v1.0)：** 按投资组合角色固定仓位（核心：10-20%，主题：5-10%等）。

**Enhanced (v2.0):** Bet-type classification with dynamic sizing:

| Bet Type / 赌注类型 | Holding Period / 持仓周期 | Base Size / 基础仓位 |
|--------------------|--------------------------|---------------------|
| Super Beta / 超大Beta | 2-5 years / 2-5年 | 8%-15% |
| Catalyst Alpha / 催化剂Alpha | 1-4 quarters / 1-4季度 | 3%-8% |
| Event-Driven / 事件驱动 | 0-3 months / 0-3月 | 1%-4% |

Adjustments for evidence stage, crowding, catalyst clarity, liquidity, dilution risk, and cash burn.

根据证据阶段、拥挤度、催化剂清晰度、流动性、稀释风险和现金消耗进行调整。

---

## Core Framework

### Stage 1: Bottleneck Identification / 阶段一：瓶颈识别

Score using 10 questions (0=No, 1=Partially, 2=Yes). Total: 20 points.

1. Is demand structurally growing? / 需求是否结构性增长？
2. Is supply limited? / 供应是否有限？
3. Is capacity expansion slow? / 产能扩张是否缓慢？
4. Are technical barriers high? / 技术壁垒是否高？
5. Is customer qualification difficult? / 客户认证是否困难？
6. Are substitute technologies limited? / 替代技术是否有限？
7. Are supplier options limited? / 供应商选择是否有限？
8. Does this product affect system-level delivery? / 该产品是否影响系统级交付？
9. Are customers willing to pay for reliable supply? / 客户是否愿意为可靠供应付费？
10. Is the market still underestimating this node? / 市场是否仍低估这个节点？

| Score / 分数 | Classification / 分类 | Action / 行动 |
|-------------|----------------------|--------------|
| 16-20 | Strong chokepoint / 强瓶颈 | Proceed to Stage 2 / 进入阶段二 |
| 12-15 | Medium chokepoint / 中等瓶颈 | Proceed with caution / 谨慎进入 |
| 8-11 | Weak chokepoint / 弱瓶颈 | Reject unless exceptional / 除非异常错配否则拒绝 |
| 0-7 | Not a chokepoint / 非瓶颈 | Reject / 拒绝 |

### Stage 2: Alpha Assessment / 阶段二：Alpha评估

Evaluate four dimensions (0.0-1.0 each). Final score is the **product**.

| Dimension / 维度 | High Score (1.0) / 高分 | Low Score (0.0) / 低分 |
|-----------------|------------------------|-----------------------|
| P(is_bottleneck) | Strong chokepoint (≥16) | Weak/non-chokepoint (<12) |
| P(mispriced) | Clearly undervalued, low crowding | Severely priced in, extreme valuation |
| P(catalyst) | Catalyst within 1-2 quarters, high clarity | No visible catalyst |
| P(liquidity) | Highly liquid, no constraints | Illiquid for intended size |

| Alpha Score / Alpha分数 | Classification / 分类 |
|------------------------|----------------------|
| 0.50-1.00 | High-conviction opportunity / 高置信度机会 |
| 0.25-0.49 | Attractive but requires monitoring / 有吸引力但需监控 |
| 0.10-0.24 | Interesting, not yet actionable / 有趣但尚未可行动 |
| 0.00-0.09 | Avoid / 回避 |

---

## Repository Structure

```
serenity-chokepoint-investing-enhanced/
├── README.md                          # This file
├── CONTEXT.md                         # Domain glossary and key decisions
├── docs/
│   └── adr/
│       ├── 0001-two-stage-multiplicative-model.md
│       ├── 0002-supply-chain-ontology-by-process-node.md
│       ├── 0003-evidence-stage-transition-map.md
│       ├── 0004-fact-opinion-triangulation.md
│       ├── 0005-cross-validation-layer.md
│       └── 0006-catalyst-driven-position-sizing.md
└── serenity-chokepoint-investing/
    └── SKILL.md                       # Deployable skill file for OpenClaw/Hermes
```

---

## Installation

Clone this repository:

```bash
git clone https://github.com/yuyang-rgb094/serenity-chokepoint-investing-enhanced.git
```

Copy the skill folder into your OpenClaw or Hermes skills directory:

```bash
cp -r serenity-chokepoint-investing-enhanced/serenity-chokepoint-investing \
  /path/to/your/openclaw/skills/
```

Ensure the folder contains `SKILL.md`.

---

## Requirements

This enhanced skill requires the agent platform to support:

- **Web search tools** (DuckDuckGo / EXA / similar) / 网页搜索工具
- **Financial data API access** (Wind / Bloomberg / Choice / similar) / 金融数据API
- **Subagent spawning** for verification / 子代理 spawning 用于验证

---

## Best Use Cases

This skill is especially useful for analyzing:

- AI infrastructure / AI基础设施
- Semiconductors / 半导体
- HBM / DRAM / NAND / 存储
- Advanced packaging (CoWoS, ABF, OSAT) / 先进封装
- Optical modules / 光模块
- Silicon photonics / CPO / 硅光子学/共封装光学
- InP / GaAs / SOI **substrates** (NOT epitaxy) / InP/GaAs/SOI **衬底**（非外延）
- Lasers and external light sources / 激光器和外部光源
- Semiconductor testing equipment / 半导体测试设备
- Data center power / cooling / 数据中心电源/液冷
- AI data centers / Neocloud / GPU cloud / AI数据中心/新云/GPU云
- Robotics supply chain / 机器人供应链
- High-volatility small-cap AI supply-chain stocks / 高波动小盘AI供应链股票

---

## Safety Notes

This skill contains no executable code. It does not request API keys, access wallets, access browsers, access local credentials, execute shell commands, modify files, or connect to brokerage accounts. It is a text-based research workflow skill only.

本技能不包含可执行代码。它不请求API密钥、不访问钱包、不访问浏览器、不访问本地凭证、不执行shell命令、不修改文件、不连接经纪账户。它只是一个基于文本的研究工作流技能。

---

## Disclaimer

This skill is for educational and research workflow purposes only. It does not provide financial advice, investment recommendations, or personalized portfolio management. All outputs should be treated as research assistance, not buy or sell instructions. Investing involves risk, including the possible loss of principal. Always do your own due diligence.

本技能仅供教育和研究工作流目的使用。它不提供财务建议、投资建议或个性化投资组合管理。所有输出应视为研究辅助，而非买入或卖出指令。投资涉及风险，包括可能损失本金。请始终自行进行尽职调查。

---

## Credits

- **Original framework**: [leospark/serenity-chokepoint-investing-skills](https://github.com/leospark/serenity-chokepoint-investing-skills) by hart-li
- **Enhanced v2.0**: Domain expert collaboration with financial industry practitioner insights on factor exposure dynamics, catalyst windows, and cross-validation methodologies.

---

## License

MIT
