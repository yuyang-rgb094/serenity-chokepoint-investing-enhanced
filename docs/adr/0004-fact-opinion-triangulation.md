# ADR-0004: Fact-Opinion-Inference Tagging with Triangulation

## Status

Accepted

## Context

Financial text is dense with opinions packaged as facts. Management guidance in earnings calls, analyst reports, and KOL narratives often present subjective judgments with the confidence of objective truth. LLMs are particularly susceptible to this because:
1. They weight confident-sounding statements heavily.
2. They lack native ability to distinguish assertion from evidence.
3. Retrieval-augmented generation can amplify bias by surfacing SEO-optimized opinion content.

The original framework had no systematic fact/opinion discrimination. This creates two major risks:
- **Agent hallucination**: The agent invents facts to support a narrative.
- **Retrieval bias**: The agent accepts retrieved opinions as facts because they appear in authoritative-sounding sources.

## Decision

Implement a **dual-layer fact verification system**.

### Layer 1: System Prompt Tagging

Force the agent to tag every substantive statement in its output:

| Tag | Definition | Example |
|-----|-----------|---------|
| `[FACT:source]` | Objectively verifiable data with explicit source | `[FACT:公司2024年年报] 该公司2024年营收12.3亿元，同比增长45%。` |
| `[OPINION:holder]` | Subjective judgment with identified holder | `[OPINION:管理层] 管理层预计2025年AI相关收入占比将达到30%。` |
| `[INFERENCE:chain]` | Agent-derived conclusion with derivation chain | `[INFERENCE:存货下降+预收款上升→需求旺盛] 基于Q3存货周转天数从120天降至90天，且预收款环比增长20%，推断下游需求正在加速。` |

**Rules for tagging:**
- Any numerical claim MUST be tagged `[FACT]` with source.
- Any forward-looking statement MUST be tagged `[OPINION]` with holder.
- Any causal conclusion MUST be tagged `[INFERENCE]` with explicit chain.
- Untagged statements are treated as unverified and should not be used for decision-making.

### Layer 2: Verification Subagent

For every `[FACT]` tag, a dedicated verification subagent must:

1. **Source Authenticity Check**
   - Verify the cited source URL/DOI is accessible.
   - Confirm the cited content matches the agent's representation.
   - Flag if source is paywalled, inaccessible, or misrepresented.

2. **Non-Correlated Source Confirmation**
   - Search for at least one independent source confirming the same fact.
   - Independent means: different publisher, different author, different data collection method.
   - Example: If primary agent cites company filing, verification subagent should search for industry report or customer disclosure confirming the same number.

3. **Numerical Consistency Check**
   - Compare numerical values across sources.
   - Flag discrepancies >5% for investigation.
   - For financial figures, cross-check against primary filings (10-K, 10-Q, 20-F, annual reports).

4. **Promotion to Triangulated Fact**
   - Facts passing all three checks are promoted to `[TRIANGULATED_FACT]`.
   - Facts failing any check remain `[FACT:unverified]` with explicit warning.

### Verification Subagent Workflow

```
Primary Agent Output
    ↓
Extract all [FACT] tags
    ↓
For each [FACT]:
  ├─ Verify source accessibility
  ├─ Search non-correlated source
  ├─ Compare numerical values
  └─ Assign: [TRIANGULATED_FACT] or [FACT:unverified]
    ↓
Return verification report with:
  - List of triangulated facts
  - List of unverified facts with warnings
  - List of discrepancies found
```

## Consequences

### Positive
- Makes agent reasoning explicit, auditable, and human-reviewable.
- Dramatically reduces hallucination risk.
- Forces agent to be precise about source attribution.
- Creates a clear quality gradient: triangulated facts > single-source facts > opinions > inferences.

### Negative
- Doubles analysis time (every fact must be verified).
- Requires verification subagent to have independent search capabilities.
- Some facts may be genuinely single-source (e.g., proprietary company data) — these must be explicitly flagged as `[FACT:single_source]`.
- May produce overly cautious outputs if verification is too strict.

## Related Decisions
- ADR-0002: Supply-Chain Ontology (node identification facts must be triangulated)
- ADR-0005: Cross-Validation Layer (cross-validation facts are a form of triangulation)
