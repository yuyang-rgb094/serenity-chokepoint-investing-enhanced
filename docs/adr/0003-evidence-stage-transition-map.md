# ADR-0003: Evidence Stage Transition Map

## Status

Accepted

## Context

The original framework used a static Evidence Level classification:
- A: Financially verified
- B: Order/customer verified
- C: Management/industry supported
- D: Narrative only

This is a snapshot, not a process. In real investment practice, validation is a dynamic timeline with conditional transition probabilities. A company at "sample submission" stage may or may not advance to "mass production ramp." The alpha window often exists BETWEEN stages, not at a static point.

Moreover, the original framework conflated **evidence strength** with **development stage**. "Sample submission" is both a weaker evidence than "mass production" AND an earlier stage in the timeline. These two dimensions need to be separated.

## Decision

Replace static Evidence Level with a **dynamic Evidence Stage Transition Map**.

### Stage Timeline

```
Concept ────────────────────────────────────────────────────────────────────────→ Primary/Exclusive
   │                                                                              Supplier
   │         Sample              Pilot              Mass Production
   │      Submission            Order                  Ramp
   │          │                  │                      │
   │          ▼                  ▼                      ▼
   │        C+级                B级                  B+/A-级
   │
   └── C级 (narrative only)
```

### Stage Definitions

| Stage | Definition | Typical Evidence | Factor Exposure |
|-------|-----------|------------------|-----------------|
| Concept | Company mentions AI opportunity in filings/presentations | Management commentary, investor presentation slides | Narrative-driven speculation |
| Sample Submission | Product samples sent to key customers for evaluation | Customer disclosure, supply chain checks | Technical validation pending |
| Pilot Order | Small-batch orders for testing/qualification | Customer PO, backlog disclosure | Revenue visibility emerging |
| Mass Production Ramp | Qualified for volume production, scaling deliveries | Revenue growth, margin expansion, named customer wins | Order-backed growth |
| Primary/Exclusive Supplier | Designated as main or sole supplier for critical component | Long-term supply agreements, RPO, customer dependency disclosure | Monopoly rent exposure |

### Transition Probability Assessment

For each analyzed company, the agent must estimate:

1. **Current Stage**: Where is the company NOW?
2. **Next Stage**: What is the next logical milestone?
3. **P(advancement within 1-2 quarters)**: Probability of reaching next stage
4. **Advancement Triggers**: Specific signals that would confirm stage advancement
5. **Regression Triggers**: Specific signals that would indicate stage regression or thesis disconfirmation

### Example: InP Substrate Company

| Element | Assessment |
|---------|-----------|
| Current Stage | Sample Submission |
| Next Stage | Pilot Order |
| P(advancement) | 0.6 (based on: customer qualification timeline typical 6-9 months, company has passed initial technical review) |
| Advancement Triggers | Customer announces qualification win; company discloses first revenue from new product line; industry report names company as qualified supplier |
| Regression Triggers | Customer switches to alternative substrate supplier; company misses qualification timeline; technical failure reported in industry channels |

## Consequences

### Positive
- Captures the dynamic nature of investment validation.
- Defines explicit entry/exit signals tied to stage transitions.
- Enables better position sizing (earlier stage = smaller size, higher uncertainty discount).
- Provides clear disconfirmation criteria for stop-loss decisions.

### Negative
- Requires more detailed information about company-specific development timelines.
- Transition probability estimates are inherently subjective — must be explicitly tagged as `[INFERENCE]`.
- Increases analysis complexity and time.

## Related Decisions
- ADR-0001: Two-Stage Multiplicative Model (Stage 2 uses P(catalyst) which is derived from transition probability)
- ADR-0006: Catalyst-Driven Position Sizing (position size is adjusted by evidence stage)
