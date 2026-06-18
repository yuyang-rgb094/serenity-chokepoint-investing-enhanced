# Serenity Chokepoint Investing Framework (Enhanced v2.1)

> **Find the bottleneck first. Find the listed exposure second. Verify through evidence third. Validate with statistics fourth. Size by alpha significance fifth.**
>
> 先找到真正的瓶颈。再找到上市企业的敞口。然后通过证据验证。再用统计方法检验。最后根据alpha显著性确定仓位。

---

## Overview

An enhanced research skill for AI infrastructure supply-chain chokepoint investing, compatible with **OpenClaw / Hermes** agent platforms.

This is NOT a buy/sell recommendation engine. It is a structured research workflow that helps an agent:
1. Identify whether a company sits inside a real supply-chain bottleneck
2. Generate a **testable hypothesis** about abnormal alpha
3. **Verify the hypothesis with code** (Fama-MacBeth regression, Information Ratio)
4. Size positions based on **statistical evidence**

一套增强版AI基础设施供应链瓶颈投资研究技能，兼容 **OpenClaw / Hermes** 智能体平台。

这不是一个买入/卖出推荐引擎。它是一个结构化研究工作流，帮助智能体：
1. 识别一家公司是否处于真正的供应链瓶颈中
2. 生成关于异常alpha的**可检验假设**
3. **用代码验证假设**（Fama-MacBeth回归、信息比率）
4. 基于**统计证据**确定仓位

---

## What's New in v2.1

### v2.1: LLM Qualitative + Code Quantitative Architecture

**v2.0 Problem:** Multiplicative probability model `P(is_bottleneck) × P(mispriced) × P(catalyst) × P(liquidity)` produced excessive false negatives. Four dimensions each at 0.7 scored 0.24 — classifying a "good" opportunity as "barely interesting." LLMs are poor at probability calibration, and averaging noisy estimates further dilutes signal in low-SNR markets.

**v2.0问题：** 乘法概率模型产生过度假阴性。四个维度各0.7得分仅0.24——将"不错"的机会归类为" barely interesting"。LLM概率校准能力差，在低信噪比市场中平均化噪声估计进一步稀释信号。

**v2.1 Solution:** Two-layer architecture:

**Layer 1 — LLM Qualitative (Hypothesis Generation):**
- Identify supertrend, validate process node, score chokepoint quality
- Assess evidence stage, cross-validate supply chain, classify bet type
- Generate testable hypothesis: "This company has abnormal alpha not explained by the four-factor model"

**Layer 2 — Code Quantitative (Hypothesis Testing):**
- Pull 3-5 years historical returns + four-factor data (MKT, SMB, HML, MOM)
- Run Fama-MacBeth regression: `R_i - R_f = α + β₁·MKT + β₂·SMB + β₃·HML + β₄·MOM + ε`
- Test H₀: α = 0
- Compute **Information Ratio**: `IR = α / σ(ε)`
- Run GMM robustness check

**Layer 3 — LLM Synthesis (Interpretation):**
- Does statistical evidence support the qualitative hypothesis?
- Is α economically meaningful given the target holding period?
- Final classification based on BOTH qualitative and quantitative evidence

**v2.1解决方案：** 双层架构：
- **层1**——LLM定性（假设生成）：识别趋势、验证节点、评分瓶颈、生成分子假设
- **层2**——代码定量（假设检验）：拉数据、跑回归、算IR、做稳健性检验
- **层3**——LLM综合（解读）：统计证据是否支持定性假设？alpha是否经济显著？

**Hard Constraints:**
- Chokepoint score < 12 → Reject
- α not significant (p > 0.05) → Downgrade to "Watchlist" or "Avoid"
- IR < 0.3 → Reject — alpha not economically meaningful
- Any qualitative dimension < 0.3 (e.g., liquidity) → Reject

**Fallback:** If code execution unavailable, LLM performs qualitative-only with explicit warning and position sizing capped at 50%.

---

### v2.0 Features (Retained)

### 2. Supply-Chain Ontology by Process Node / 按工艺节点的供应链本体

**Original (v1.0):** Grouped "InP, GaAs, SOI, substrates, epitaxy wafers" into a single "Materials layer." This conflated upstream substrate (e.g., 鑫耀半导) with downstream epitaxy (e.g., 三安光电).

**Enhanced (v2.0+):** Reclassifies by process node:
```
Raw Material → Substrate → Epitaxy → Device Fabrication
→ Module Assembly → System Integration
```

### 3. Evidence Stage Transition Map / 证据阶段转移图

Dynamic stage map with transition probabilities:
```
Concept → Sample Submission → Pilot Order → Mass Production → Primary/Exclusive Supplier
   C级         C+级            B级           B+/A-级              A级
```

### 4. Cross-Validation Layer / 交叉验证层

Compares financials across supply chain: target company, downstream customers, upstream suppliers. Detects resonance signals and divergence signals.

### 5. Fact-Opinion-Inference Tagging with Triangulation / 事实-观点-推断标注与三角验证

Dual-layer verification: `[FACT:source]`, `[OPINION:holder]`, `[INFERENCE:chain]`, `[TRIANGULATED_FACT]`.

### 6. Catalyst-Driven Position Sizing / 催化剂驱动仓位管理

Bet-type classification with **weighted (not multiplicative)** adjustments and hard floor:

| Bet Type | Holding Period | Base Size |
|----------|---------------|-----------|
| Super Beta | 2-5 years | 8%-15% |
| Catalyst Alpha | 1-4 quarters | 3%-8% |
| Event-Driven | 0-3 months | 1%-4% |

Adjustments are **absolute percentage point modifiers** (e.g., -1%, -2%) with hard floor at max(Base × 25%, 0.5%).

---

## Core Framework

### Layer 1: Bottleneck Identification (Hard Filter)

Score using 10 questions (0=No, 1=Partially, 2=Yes). Total: 20 points.

| Score | Classification | Action |
|-------|---------------|--------|
| 16-20 | Strong chokepoint | Proceed to Layer 2 |
| 12-15 | Medium chokepoint | Proceed with caution |
| 8-11 | Weak chokepoint | Reject unless exceptional |
| 0-7 | Not a chokepoint | Reject |

### Layer 2: Alpha Verification (Code-Driven Quantitative)

**Requires code execution.** Spawn Quantitative Verification Subagent:

1. **Data Pull:** 3-5 years returns + four-factor data (MKT, SMB, HML, MOM)
2. **Fama-MacBeth Regression:** `R_i - R_f = α + β₁·MKT + β₂·SMB + β₃·HML + β₄·MOM + ε`
3. **Significance Test:** H₀: α = 0
4. **Information Ratio:** `IR = α / σ(ε)`

| IR | Interpretation | Action |
|----|---------------|--------|
| IR > 1.0 | Strong alpha | High-conviction opportunity |
| 0.5 < IR < 1.0 | Moderate alpha | Attractive but monitor |
| 0.3 < IR < 0.5 | Weak alpha | Small position only |
| IR < 0.3 | Alpha too small | Reject |

5. **GMM Robustness Check:** Verify alpha persistence across sub-periods

### Layer 3: Synthesis

Compare qualitative hypothesis with quantitative results. Final classification:

| Condition | Classification |
|-----------|---------------|
| α sig (p<0.05) + IR>1.0 + GMM persistent | **High-conviction** |
| α sig (p<0.05) + 0.5<IR<1.0 | **Attractive, monitor** |
| α marginally sig (p<0.10) + 0.3<IR<0.5 | **Small position only** |
| α not sig OR IR<0.3 OR GMM non-persistent | **Avoid / Watchlist** |

---

## Repository Structure

```
serenity-chokepoint-investing-enhanced/
├── README.md                          # This file
├── CONTEXT.md                         # Domain glossary and key decisions
├── LICENSE                            # MIT License
├── docs/
│   └── adr/
│       ├── 0001-two-stage-multiplicative-model.md  # Revised v2.1
│       ├── 0002-supply-chain-ontology-by-process-node.md
│       ├── 0003-evidence-stage-transition-map.md
│       ├── 0004-fact-opinion-triangulation.md
│       ├── 0005-cross-validation-layer.md
│       └── 0006-catalyst-driven-position-sizing.md
└── serenity-chokepoint-investing/
    └── SKILL.md                       # Deployable skill file (v2.1)
```

---

## Installation

```bash
git clone https://github.com/yuyang-rgb094/serenity-chokepoint-investing-enhanced.git
cp -r serenity-chokepoint-investing-enhanced/serenity-chokepoint-investing \
  /path/to/your/openclaw/skills/
```

---

## Requirements

- **Web search tools** (DuckDuckGo / EXA / similar)
- **Financial data API** (Wind / Bloomberg / Choice / Yahoo Finance)
- **Subagent spawning** for verification AND quantitative analysis
- **Code execution environment** (Python with pandas, statsmodels, numpy)

---

## Best Use Cases

- AI infrastructure / Semiconductors / HBM / DRAM / NAND
- Advanced packaging (CoWoS, ABF, OSAT)
- Optical modules / Silicon photonics / CPO
- InP / GaAs / SOI **substrates** (NOT epitaxy)
- Lasers / Data center power / Liquid cooling
- AI data centers / Neocloud / GPU cloud
- Robotics supply chain / High-volatility small-cap AI supply-chain stocks

---

## Safety Notes

The SKILL.md file contains no executable code. However, v2.1 REQUIRES the agent to spawn a **Quantitative Verification Subagent** that writes and executes Python code for statistical analysis (Fama-MacBeth regression, GMM). The Python code pulls historical data via financial APIs but does NOT access wallets, credentials, or brokerage accounts.

---

## Disclaimer

This skill is for educational and research workflow purposes only. It does not provide financial advice, investment recommendations, or personalized portfolio management. All outputs should be treated as research assistance, not buy or sell instructions. Investing involves risk, including the possible loss of principal. Always do your own due diligence.

---

## Credits

- **Original framework**: [leospark/serenity-chokepoint-investing-skills](https://github.com/leospark/serenity-chokepoint-investing-skills) by hart-li
- **Enhanced v2.0-v2.1**: Domain expert collaboration with financial industry practitioner insights on factor exposure dynamics, catalyst windows, cross-validation, and quantitative verification methodologies.

---

## License

MIT
