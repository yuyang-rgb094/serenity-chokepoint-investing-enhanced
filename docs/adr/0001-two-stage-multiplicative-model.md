# ADR-0001: Two-Stage Analysis Model (LLM Qualitative + Code Quantitative)

## Status

Accepted (Revised v2.1)

## Context

The original Serenity Chokepoint Investing Framework (v1.0) used a linear 100-point scoring model where chokepoint quality (20 points), company positioning (15 points), order validation (15 points), financial quality (10 points), and valuation/pricing (10 points) were additive. This created a structural bias: a company with a perfect chokepoint score (18/20) but fully priced valuation (2/10) could still receive a total score of 80+ points, classifying it as a "Strong research candidate" despite offering no alpha.

The enhanced v2.0 attempted to fix this with a four-dimensional multiplicative probability model:
```
Alpha Score = P(is_bottleneck) × P(mispriced) × P(catalyst) × P(liquidity)
```

However, peer review identified critical flaws in this approach:
1. **P(is_bottleneck) double-counted**: Stage 1 already acts as a hard filter (<12 rejects). Stage 2's Dimension 1 re-introduces the same variable, making it a near-constant multiplier (minimum 0.5 for all Stage 2 qualifiers) with little discriminating power.
2. **Multiplicative over-penalty**: Four dimensions each at 0.7 (a "good" score) produce a product of 0.24, classifying the opportunity as "barely interesting." This is counterintuitive and produces excessive false negatives.
3. **LLM probability calibration failure**: LLMs are systematically overconfident and poor at probability estimation. Asking an LLM to score P(catalyst) = 0.7 is asking for a hallucinated precision.
4. **Low signal-to-noise market failure**: In markets where signal is weak, averaging or multiplying noisy estimates further dilutes whatever genuine signal exists.

## Decision

Replace the LLM-driven multiplicative model with a **two-layer architecture: LLM Qualitative + Code Quantitative**.

### Layer 1: LLM Qualitative Assessment (Hypothesis Generation)

The LLM's job is NOT to compute alpha. The LLM's job is to:
1. Identify the supertrend
2. Validate the process node
3. Score the chokepoint quality (Stage 1 hard filter)
4. Assess evidence stage and transition probability (qualitative)
5. Classify bet type (Catalyst Alpha / Super Beta / Event-Driven)
6. Generate a **testable hypothesis**: "This company has abnormal alpha not explained by the four-factor model"

The LLM outputs a structured qualitative assessment, NOT a numeric alpha score.

### Layer 2: Code Quantitative Verification (Hypothesis Testing)

After the LLM generates the hypothesis, the agent MUST spawn a **Quantitative Verification Subagent** that writes and executes Python code to:

1. **Pull historical return data** for the target company (3-5 years, daily or weekly)
2. **Pull four-factor model data** (MKT, SMB, HML, MOM/UMD) for the same period
3. **Run Fama-MacBeth regression**:
   ```
   R_i - R_f = α + β₁·MKT + β₂·SMB + β₃·HML + β₄·MOM + ε
   ```
4. **Test H₀: α = 0** (is the intercept statistically significant?)
5. **Compute Information Ratio**: IR = α / σ(ε)
   - IR < 0.5: alpha too small, not worth the idiosyncratic risk
   - IR 0.5-1.0: moderate alpha, acceptable risk-adjusted return
   - IR > 1.0: strong alpha, significant abnormal return
6. **Run GMM robustness check** to verify alpha persistence across sub-periods
7. **Output structured results**: α, t-stat, p-value, IR, R², factor loadings

### Layer 3: LLM Synthesis (Interpretation)

The LLM receives the quantitative results and synthesizes:
- Does the statistical evidence support the qualitative hypothesis?
- If α is significant but small, is it economically meaningful given the target holding period?
- If α is not significant, is the thesis disconfirmed or does the sample lack power?
- Final opportunity classification based on BOTH qualitative and quantitative evidence

### Hard Constraints (Non-Negotiable)

| Condition | Action |
|-----------|--------|
| Chokepoint score < 12 (Stage 1) | Reject, no quantitative analysis needed |
| Quantitative verification fails (α not significant at 5% level) | Downgrade to "Watchlist" or "Avoid" |
| IR < 0.5 (alpha smaller than idiosyncratic risk) | Reject — alpha not economically meaningful |
| Any LLM qualitative dimension scores < 0.3 (e.g., liquidity) | Reject — hard floor constraint |

### Why This Architecture Works

| Problem | v2.0 Multiplicative | v2.1 LLM+Code |
|---------|---------------------|----------------|
| P(is_bottleneck) double-counted | Yes | Eliminated — Stage 1 is filter only |
| Multiplicative over-penalty | Yes (0.7^4 = 0.24) | Eliminated — code computes real α |
| LLM probability calibration | Poor | Not required — LLM does qualitative only |
| Low SNR market | Worsened by averaging | Improved — code extracts signal statistically |
| Alpha significance vs. magnitude | Confused | Separated — t-stat for significance, IR for magnitude |
| Factor collinearity | Ignored | Addressed — four-factor model controls for known factors |

## Consequences

### Positive
- Eliminates LLM hallucination in numeric scoring.
- Alpha is computed by statistical methods, not guessed by language model.
- Information Ratio ensures alpha is economically meaningful, not just statistically significant.
- Fama-MacBeth regression provides rigorous factor control.
- GMM robustness check validates persistence.

### Negative
- Requires agent platform to support code execution (Python + data access).
- Adds analysis time (data pull + regression + robustness check).
- If data is unavailable (e.g., pre-IPO, thin trading), quantitative layer cannot run — must fall back to qualitative-only with explicit uncertainty flag.

## Fallback Protocol (When Code Execution Unavailable)

If the agent platform does NOT support code execution:
1. Skip Layer 2 (quantitative verification).
2. LLM performs qualitative assessment only.
3. Output MUST include explicit warning: `[WARNING: Quantitative verification unavailable. All scores are qualitative estimates only.]`
4. Position sizing capped at 50% of normal maximum.

## Related Decisions
- ADR-0003: Evidence Stage Transition Map (qualitative layer input)
- ADR-0006: Catalyst-Driven Position Sizing (uses IR and t-stat for sizing, not LLM scores)
