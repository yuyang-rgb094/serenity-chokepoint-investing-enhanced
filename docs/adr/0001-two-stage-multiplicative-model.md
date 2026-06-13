# ADR-0001: Two-Stage Multiplicative Analysis Model

## Status

Accepted

## Context

The original Serenity Chokepoint Investing Framework used a linear 100-point scoring model where chokepoint quality (20 points), company positioning (15 points), order validation (15 points), financial quality (10 points), and valuation/pricing (10 points) were additive. This created a structural bias: a company with a perfect chokepoint score (18/20) but fully priced valuation (2/10) could still receive a total score of 80+ points, classifying it as a "Strong research candidate" despite offering no alpha.

In financial practice, this is a critical flaw. A real bottleneck that is fully priced is not an investment opportunity — it is a crowded trade.

## Decision

Replace the additive scoring model with a **two-stage multiplicative model**.

### Stage 1: Bottleneck Identification (Qualitative Filter)

Use the original 10-question chokepoint scoring (0-20 points) as a **hard filter**, not a score component.

| Score | Classification | Action |
|-------|---------------|--------|
| 16-20 | Strong chokepoint | Proceed to Stage 2 |
| 12-15 | Medium chokepoint | Proceed to Stage 2 with caution |
| 8-11 | Weak chokepoint | Reject unless exceptional dislocation |
| 0-7 | Not a chokepoint | Reject |

### Stage 2: Alpha Assessment (Multiplicative Evaluation)

For Stage 1 qualifiers, evaluate four multiplicative dimensions:

**Alpha Probability = P(is_bottleneck) × P(mispriced | is_bottleneck) × P(catalyst_within_window | mispriced) × P(liquidity_sufficient)**

Each dimension is scored 0.0-1.0. The final alpha score is the product, not the sum.

| Dimension | Score 1.0 | Score 0.0 |
|-----------|-----------|-----------|
| P(is_bottleneck) | Strong chokepoint (Stage 1 ≥16) | Weak/non-chokepoint (Stage 1 <12) |
| P(mispriced) | Clear undervaluation vs. historical range | Severely priced in, extreme valuation |
| P(catalyst) | Milestone event within 1-2 quarters | No visible catalyst, thesis is long-dated |
| P(liquidity) | Sufficient for intended position size | Insufficient float, extreme impact cost |

### Score Interpretation

| Alpha Score | Classification |
|-------------|---------------|
| 0.50-1.00 | High-conviction opportunity |
| 0.25-0.49 | Attractive but requires monitoring |
| 0.10-0.24 | Interesting, not yet actionable |
| 0.00-0.09 | Avoid — either no bottleneck or no mispricing |

## Consequences

### Positive
- Eliminates false positives from "strong bottleneck but fully priced" opportunities.
- Correctly penalizes opportunities that fail on ANY critical dimension.
- Aligns the framework with real-world alpha generation: multiplicative, not additive.

### Negative
- More complex to explain to non-technical users.
- Requires explicit probability estimates rather than simple point scores.
- May produce counterintuitive results (e.g., a "medium bottleneck + medium mispricing" scores lower than a "strong bottleneck + slight mispricing").

## Related Decisions
- ADR-0003: Evidence Stage Transition Map (defines how P(catalyst) is estimated)
- ADR-0006: Catalyst-Driven Position Sizing (defines how alpha score maps to position size)
