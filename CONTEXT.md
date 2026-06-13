# CONTEXT.md — Serenity Chokepoint Investing Framework (Enhanced)

## Project Overview

An enhanced research skill for AI infrastructure supply-chain chokepoint investing,
deployed on OpenClaw / Hermes-compatible agent platforms.

The core thesis: **find the bottleneck first, find the listed exposure second,
verify through evidence third, size by catalyst window fourth.**

This is NOT a buy/sell recommendation engine. It is a structured research workflow
that helps an agent identify whether a company sits inside a real supply-chain
bottleneck created by AI infrastructure expansion, and whether the market has
underpriced the factor exposure change driven by milestone events.

---

## Glossary

### Domain Terms

| Term | Definition |
|------|-----------|
| **Chokepoint** | A node in the AI industrial supply chain where demand is structurally growing, supply is constrained, capacity expansion is slow, and substitute technologies are limited. The node exhibits pricing power and affects system-level delivery. |
| **Supertrend** | A major long-term demand driver for AI infrastructure (e.g., AI compute expansion, memory bandwidth demand, data center power shortage, optical interconnect adoption). |
| **Supply-Chain Layer** | A position in the AI value chain defined by process node and value-add stage, NOT by material type. Examples: compute layer, memory layer, packaging layer, optical module layer, laser layer, materials layer (substrate vs. epitaxy), silicon photonics layer, equipment layer, power layer, cooling layer, data center layer, robotics layer. |
| **First-Order Beneficiary** | A company whose core product is directly consumed by the end-demand driver (e.g., NVIDIA for AI compute). |
| **Second-Order Beneficiary** | A company supplying critical components to first-order beneficiaries (e.g., HBM suppliers to NVIDIA). |
| **Third-Order Node** | A company supplying materials, equipment, or sub-components to second-order beneficiaries. Often the most underappreciated chokepoint opportunities. |
| **Evidence Stage** | A milestone in the customer validation timeline: Concept → Sample Submission → Pilot Order → Mass Production Ramp → Primary/Exclusive Supplier. Each stage has distinct factor exposure implications. |
| **Evidence Level** | The current validation strength of the investment thesis: A (financially verified), B (order/customer verified), C (management/industry supported), D (narrative only). |
| **Factor Exposure Change** | A shift in the portfolio of risk factors that a security loads on, triggered by milestone events (e.g., from "narrative-driven speculation" to "order-backed growth"). |
| **Milestone-Rerating Lag** | The time delay between a milestone event (e.g., mass production qualification) and the market's full recognition of the factor exposure change. The alpha window exists within this lag. |
| **Catalyst Window** | The period (typically 1-2 quarters) during which a milestone event's factor exposure change is partially but not fully priced in. The optimal entry zone for catalyst-driven positions. |
| **Crowding** | The degree to which the investment thesis is already held and discussed by market participants. Measured by KOL attention, social media volume, options activity, short interest, and sell-side coverage. |
| **Alpha Source** | The specific mechanism by which the investment thesis generates excess returns: (1) milestone-rerating lag, (2) super-trend duration, (3) factor mispricing, (4) crowding dislocation. |
| **Position Sizing Framework** | A risk-adjusted allocation scheme that maps evidence level, crowding, catalyst clarity, and liquidity to conservative position size ranges. |
| **Fact** | An objectively verifiable statement supported by primary data (filings, contracts, customs data, financial numbers). Must be tagged `[FACT]` in agent output. |
| **Opinion** | A subjective judgment, prediction, or evaluation. Must be tagged `[OPINION]` in agent output. |
| **Inference** | A conclusion derived by the agent from facts via logical reasoning. Must be tagged `[INFERENCE]` and include the derivation chain. |
| **Triangulated Fact** | A fact that has been verified against at least two independent data sources with consistent numerical values. |

### Architecture Terms

| Term | Definition |
|------|-----------|
| **Primary Analysis Subagent** | The main agent performing the chokepoint analysis, evidence evaluation, and opportunity classification. |
| **Verification Subagent** | A dedicated subagent responsible for cross-checking every analytical conclusion: (1) verifying the authenticity of data sources cited by the primary agent, (2) retrieving non-correlated data sources to confirm the same fact. |
| **Data Source Layer** | The hierarchy of information sources: L1 (company filings), L2 (earnings calls / investor presentations), L3 (official press releases / customer announcements), L4 (industry reports), L5 (financial news), L6 (social media / KOL). |
| **Cross-Validation Layer** | The process of comparing target company financials against upstream/downstream peer financials to detect corroborating or contradicting signals in the "hidden space" of numerical intersections. |
| **Supply-Chain Ontology** | A pre-defined knowledge graph mapping AI infrastructure supply chain nodes by process stage, NOT by material type. Used to validate whether a claimed supplier actually sits at the asserted node. |
| **Catalyst Signal Scanner** | A periodic (quarterly) scan of target supply chain layers for leading indicators of milestone events: inventory changes, prepayment shifts, capex guidance divergences, customs data anomalies. |
| **Fact-Opinion Discriminator** | A methodological layer (system prompt + structured rules) that forces the agent to explicitly tag every statement as Fact, Opinion, or Inference. |

---

## Key Decisions

### ADR-001: Two-Stage Analysis Model (Bottleneck × Pricing Efficiency)

**Status:** Accepted

**Context:** The original framework used a linear 100-point scoring model where chokepoint quality (20 points) and valuation/pricing (10 points) were additive. This created a structural bias toward "strong bottleneck but fully priced" opportunities receiving high total scores despite lacking alpha.

**Decision:**
Replace the additive scoring model with a **two-stage multiplicative model**:
- **Stage 1 (Bottleneck Identification):** Qualitative/structural filter. Uses a 10-question chokepoint score (0-20 points) to determine whether a node is a real bottleneck. Minimum threshold: 12 points (Medium chokepoint) to proceed.
- **Stage 2 (Alpha Assessment):** Multiplicative assessment of `Liquidity × Win Rate × Payoff × Catalyst Clarity`. Only applied to Stage 1 qualifiers.

The final opportunity quality is NOT a sum but a **conditional probability**: `P(alpha) = P(is_bottleneck) × P(mispriced | is_bottleneck) × P(catalyst_within_window | mispriced)`.

**Rationale:** A real bottleneck that is fully priced offers no alpha. A weak bottleneck that is undervalued is likely undervalued for good reason. The multiplicative model correctly penalizes either deficiency.

---

### ADR-002: Supply-Chain Layer Classification by Process Node

**Status:** Accepted

**Context:** The original framework grouped "InP, GaAs, SOI, substrates, epitaxy wafers" into a single "Materials layer." This is investment-practice fatal: InP substrate (upstream, high barrier, few suppliers) and InP epitaxy wafer (downstream, different equipment, different competitive dynamics) are fundamentally different investment opportunities.

**Decision:**
Reclassify supply-chain layers by **process node and value-add stage**, not by material chemistry:
- Raw Material → Substrate → Epitaxy → Device Fabrication → Module Assembly → System Integration

Each layer has independent competitive dynamics, barrier heights, and pricing power profiles. The agent must validate which specific node a company occupies before analysis begins.

**Rationale:** Misidentifying a company's supply-chain node leads to incorrect barrier assessment, wrong competitor mapping, and flawed valuation. The ontology layer prevents "search engine bias" (e.g., SEO-optimized content pushing a downstream company as an upstream play).

---

### ADR-003: Evidence Stage Transition Map

**Status:** Accepted

**Context:** The original framework used a static Evidence Level (A/B/C/D). Real-world investment validation is a dynamic process with conditional transition probabilities. "Sample submission" does NOT guarantee "mass production ramp."

**Decision:**
Replace static Evidence Level with a **dynamic Evidence Stage Transition Map**:

```
Concept ──→ Sample Submission ──→ Pilot Order ──→ Mass Production ──→ Primary/Exclusive Supplier
   │              │                  │                  │                      │
  C级            C+级               B级                B+/A-级                A级
```

For each analyzed company, the agent must:
1. Identify current stage
2. Estimate probability of advancing to next stage within 1-2 quarters
3. Identify specific signals that would trigger stage advancement
4. Identify specific signals that would indicate stage regression (disconfirmation)

**Rationale:** The alpha window is often between stages, not at a static point. Understanding transition probabilities allows better position sizing and stop-loss definition.

---

### ADR-004: Fact-Opinion-Inference Tagging with Triangulation

**Status:** Accepted

**Context:** Financial text is dense with opinions packaged as facts. LLMs can be misled by confident-sounding management guidance or KOL narratives. The original framework had no systematic fact/opinion discrimination.

**Decision:**
Implement a **dual-layer fact verification system**:

**Layer 1 (System Prompt):** Force the agent to tag every substantive statement:
- `[FACT:source]` — verifiable objective data with source URL/DOI
- `[OPINION:holder]` — subjective judgment with identified holder (management, analyst, KOL)
- `[INFERENCE:chain]` — agent-derived conclusion with explicit derivation chain

**Layer 2 (Verification Subagent):** For every `[FACT]` tag:
- Verify source URL is accessible and content matches citation
- Search for at least one non-correlated source confirming the same fact
- Compare numerical values across sources (flag discrepancies >5%)
- For financial figures, cross-check against primary filings

Facts that pass Layer 2 are promoted to `[TRIANGULATED_FACT]`.

**Rationale:** Agent hallucination and retrieval bias are the two largest risks in AI-driven research. This system makes verification explicit, auditable, and human-reviewable.

---

### ADR-005: Cross-Validation Layer (Upstream/Downstream Financial Intersection)

**Status:** Accepted

**Context:** The original framework's data source hierarchy prioritized company filings but treated each company in isolation. True signals often hide in the "hidden space" where upstream and downstream financials intersect.

**Decision:**
For every target company analysis, the agent MUST retrieve and compare:
- Target company: inventory turnover, prepayments received, capex, revenue by segment
- 2-3 downstream customers: cost of goods sold, supplier concentration, inventory levels
- 1-2 upstream suppliers (if relevant): capacity utilization, pricing trends
- Customs data (if available): import/export volumes for relevant product categories

The agent should flag **resonance signals** (consistent directional changes across the chain) and **divergence signals** (contradictory trends that require explanation).

**Rationale:** A single company's management commentary is opinion. The intersection of multiple companies' financial numbers is fact. Resonance across the supply chain dramatically increases signal-to-noise ratio.

---

### ADR-006: Catalyst-Driven Position Sizing with Dynamic Holding Period

**Status:** Accepted

**Context:** The original framework assumed a uniform "buy and hold" logic. In practice, optimal holding periods vary dramatically: a super-beta bet (e.g., 2015-2022 BYD) may warrant multi-year holding, while a catalyst-alpha bet (e.g., 2018 sugar substitute) may only have a 1-4 quarter window.

**Decision:**
The agent must classify each opportunity by **bet type** before position sizing:

| Bet Type | Holding Period | Key Framework Focus |
|----------|---------------|---------------------|
| Super Beta | 2-5 years | Supertrend durability, competitive evolution, TAM expansion |
| Catalyst Alpha | 1-4 quarters | Milestone timeline, catalyst window, factor exposure change timing |
| Event-Driven | 0-3 months | Specific catalyst event (earnings, contract announcement, regulatory decision) |

Position sizing is then adjusted by:
- Evidence stage (earlier stage = smaller size)
- Crowding level (higher crowding = reduced size)
- Catalyst clarity (clearer timeline = larger size)
- Liquidity constraint (lower liquidity = smaller size)

**Rationale:** Mismatched holding period and bet type is a major source of investor losses. A catalyst-alpha position held for 2 years will likely round-trip. A super-beta position sold after one quarter captures almost none of the upside.

---

## Design Principles

1. **Bottleneck First, Price Second:** A real bottleneck is necessary but not sufficient. The market must still be underestimating the node.
2. **Milestone Over Narrative:** Invest in evidence stage transitions, not in stories. KOL posts generate ideas; financial numbers validate them.
3. **Cross-Validation Over Isolation:** No single company's filings tell the full story. The truth lives in the intersection of upstream, downstream, and competitor data.
4. **Catalyst Window Discipline:** Enter when factor exposure change is beginning. Exit when fully priced or thesis disconfirmed. Time is not on the side of catalyst trades.
5. **Fact Discrimination:** Every statement must be tagged as Fact, Opinion, or Inference. Facts must be triangulated. Opinions must be attributed.
6. **Verification by Subagent:** No analytical conclusion stands without independent verification. The verification subagent is not optional — it is a structural safeguard.
