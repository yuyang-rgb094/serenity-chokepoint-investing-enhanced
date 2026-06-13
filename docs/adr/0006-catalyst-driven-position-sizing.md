# ADR-0006: Catalyst-Driven Position Sizing with Dynamic Holding Period

## Status

Accepted

## Context

The original framework used a uniform position sizing scheme based on portfolio role (core compounder: 10-20%, thematic growth: 5-10%, etc.). This assumes all opportunities have the same holding period logic.

In practice, optimal holding periods vary dramatically:
- **Super Beta** (e.g., 2015-2022 BYD): multi-year holding, driven by super-trend durability and competitive evolution
- **Catalyst Alpha** (e.g., 2018 sugar substitute): 1-4 quarter window, driven by milestone-rerating lag
- **Event-Driven** (e.g., earnings surprise, contract announcement): 0-3 months, driven by specific catalyst event

Mismatched holding period and bet type is a major source of investor losses. A catalyst-alpha position held for 2 years will likely round-trip. A super-beta position sold after one quarter captures almost none of the upside.

## Decision

The agent must classify each opportunity by **bet type** BEFORE position sizing. Position size and holding period are jointly determined.

### Bet Type Classification

| Bet Type | Holding Period | Key Framework Focus | Typical Chokepoint Score |
|----------|---------------|---------------------|-------------------------|
| **Super Beta** | 2-5 years | Supertrend durability, competitive evolution, TAM expansion, margin trajectory | Strong (16-20) |
| **Catalyst Alpha** | 1-4 quarters | Milestone timeline, catalyst window, factor exposure change timing, evidence stage transition | Medium-Strong (12-20) |
| **Event-Driven** | 0-3 months | Specific catalyst event timing, market positioning ahead of event, post-event exit plan | Any |

### Base Position Sizing by Bet Type

| Bet Type | Base Size Range | Rationale |
|----------|----------------|-----------|
| Super Beta | 8%-15% | Long holding period requires high conviction; size reflects confidence in multi-year compounding |
| Catalyst Alpha | 3%-8% | Medium holding period; size reflects catalyst clarity and evidence stage |
| Event-Driven | 1%-4% | Short holding period; smaller size due to higher timing risk and lower information edge |

### Sizing Adjustments

Apply these adjustments to the base size:

| Factor | Condition | Adjustment |
|--------|-----------|------------|
| **Evidence Stage** | Concept / Sample | -50% (max 50% of base) |
| | Pilot Order | -25% (max 75% of base) |
| | Mass Production | No adjustment |
| | Primary/Exclusive | +25% (max 125% of base) |
| **Crowding** | Low | No adjustment |
| | Medium | -25% |
| | High | -50% |
| | Extreme | Reject or watchlist only |
| **Catalyst Clarity** | Clear timeline within 1-2 quarters | No adjustment |
| | Vague timeline | -25% |
| | No visible catalyst | -50% or reject |
| **Liquidity** | Sufficient for intended size | No adjustment |
| | Constrained | Reduce to liquidity-safe level |
| **Dilution Risk** | Near-term equity issuance / convertible | -25% |
| **Cash Burn** | Persistent cash burn with no path to profitability | -25% |

### Final Size Calculation

```
Final Size = Base Size × (1 + Evidence Adjustment) × (1 + Crowding Adjustment) × (1 + Catalyst Adjustment) × (1 + Liquidity Adjustment) × (1 + Dilution Adjustment) × (1 + Cash Burn Adjustment)
```

Minimum final size: 0% (reject)
Maximum final size: 20% (hard cap for any single position)

### Example

**Opportunity**: InP substrate company, Mass Production stage, Low crowding, Clear catalyst (customer qualification expected Q2), Sufficient liquidity, No dilution risk, Cash flow positive.

| Factor | Adjustment | Calculation |
|--------|-----------|-------------|
| Base (Catalyst Alpha) | 3%-8% | 5% (midpoint) |
| Evidence Stage (Mass Production) | 0% | 5% × 1.0 = 5% |
| Crowding (Low) | 0% | 5% × 1.0 = 5% |
| Catalyst Clarity (Clear) | 0% | 5% × 1.0 = 5% |
| Liquidity (Sufficient) | 0% | 5% × 1.0 = 5% |
| Dilution Risk (None) | 0% | 5% × 1.0 = 5% |
| Cash Burn (Positive) | 0% | 5% × 1.0 = 5% |

**Final Size**: 5%

## Consequences

### Positive
- Prevents mismatched holding period and bet type.
- Creates explicit link between evidence quality and position size.
- Crowding adjustment prevents entering crowded trades at peak.
- Hard cap prevents overconcentration.

### Negative
- More complex than original framework's fixed role-based sizing.
- Adjustment factors are multiplicative — compounding can produce extreme reductions.
- Requires agent to make subjective judgments about crowding, catalyst clarity, etc.

## Related Decisions
- ADR-0001: Two-Stage Multiplicative Model (alpha score informs bet type classification)
- ADR-0003: Evidence Stage Transition Map (evidence stage is a key sizing input)
