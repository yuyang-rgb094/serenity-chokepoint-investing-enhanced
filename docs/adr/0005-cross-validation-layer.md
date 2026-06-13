# ADR-0005: Cross-Validation Layer (Upstream/Downstream Financial Intersection)

## Status

Accepted

## Context

The original framework's data source hierarchy (Section 5 of SKILL.md) prioritized company filings but treated each company in isolation. In financial practice, a single company's management commentary is opinion. The truth often hides in the "hidden space" where upstream and downstream financial numbers intersect.

For example:
- A substrate company claims "strong demand" — but its inventory is rising while downstream module companies' COGS is falling. This is a **divergence signal** requiring explanation.
- A cooling company's prepayments received surge 200% QoQ while its largest customer's capex guidance increases 50%. This is a **resonance signal** confirming the thesis.

The original framework had no mechanism for detecting these intersection signals.

## Decision

For every target company analysis, the agent MUST retrieve and compare financial data across the supply chain.

### Required Data Points

#### Target Company
- Revenue by segment (AI-related vs. legacy)
- Inventory turnover days
- Prepayments received (预收款项)
- Capital expenditure (capex) and capex guidance
- Gross margin trend
- Operating margin trend
- Backlog / RPO (Remaining Performance Obligation)
- Customer concentration

#### Downstream Customers (2-3 key customers)
- Cost of goods sold (COGS) trend
- Inventory levels and inventory turnover
- Supplier concentration disclosures
- Capex guidance
- Revenue growth in relevant product lines

#### Upstream Suppliers (1-2 key suppliers, if relevant)
- Capacity utilization rates
- Pricing trends
- Delivery lead times
- Order backlog

#### External Data (if accessible)
- Customs import/export data for relevant product HS codes
- Industry association shipment data
- Equipment lead times (from equipment manufacturers)

### Signal Classification

| Signal Type | Definition | Action |
|------------|-----------|--------|
| **Resonance** | Consistent directional changes across supply chain nodes | Increases conviction; flag as `[TRIANGULATED_FACT]` |
| **Divergence** | Contradictory trends between nodes | Requires explanation; may indicate data error, timing difference, or thesis flaw |
| **Neutral** | No clear pattern across nodes | No adjustment to conviction |

### Example: Resonance Detection

**Thesis**: Company X (InP substrate supplier) is benefiting from 800G optical module demand surge.

| Node | Signal | Direction |
|------|--------|-----------|
| Company X | Revenue +60% YoY, backlog +80%, prepayments +150% | ↑ |
| Downstream: Optical Module Co Y | COGS +45%, inventory -20% (selling faster than producing) | ↑ |
| Downstream: Hyperscaler Z | Datacenter capex guidance raised +30% | ↑ |
| Upstream: High-purity In supplier | Shipment volume +40%, pricing +15% | ↑ |

**Classification**: Strong resonance across all nodes. Thesis is strongly supported by cross-sectional data.

### Example: Divergence Detection

| Node | Signal | Direction |
|------|--------|-----------|
| Company X | Revenue +10% YoY, claims "strong AI demand" | ↑ |
| Downstream: Optical Module Co Y | COGS -5%, inventory +30% (accumulating, not selling) | ↓ |
| Downstream: Hyperscaler Z | Datacenter capex flat QoQ | → |

**Classification**: Divergence. Company X's claim is NOT supported by downstream data. Requires investigation: Is Company X's revenue growth from non-AI segments? Is there a timing lag? Is the thesis flawed?

## Consequences

### Positive
- Dramatically increases signal-to-noise ratio by using cross-sectional validation.
- Detects management overstatement or narrative-market reality gaps.
- Creates natural triangulation for key facts.
- Prevents "echo chamber" effect where agent only reads company-produced content.

### Negative
- Requires access to multiple companies' financial data (financial API essential).
- Increases analysis complexity and time.
- Some supply chain relationships are not publicly disclosed — agent must infer from industry knowledge.
- Timing differences between fiscal quarters can create false divergences.

## Related Decisions
- ADR-0002: Supply-Chain Ontology (defines which nodes to compare)
- ADR-0004: Fact-Opinion-Inference Tagging (cross-validation facts are tagged `[TRIANGULATED_FACT]`)
