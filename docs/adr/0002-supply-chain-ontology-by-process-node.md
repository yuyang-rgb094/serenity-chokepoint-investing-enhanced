# ADR-0002: Supply-Chain Ontology by Process Node

## Status

Accepted

## Context

The original framework classified supply-chain layers by material type:
- "Materials layer: InP, GaAs, SOI, substrates, epitaxy wafers"

This is investment-practice fatal. Consider InP:
- **InP substrate** (e.g., 鑫耀半导): upstream, high technical barrier, global suppliers <5, pricing power concentrated
- **InP epitaxy wafer** (e.g., 三安光电): downstream of substrate, requires MOCVD equipment, different competitive dynamics, more competitors

An investor searching "InP supplier" via search engine or RAG system may be directed to the SEO-optimized downstream company (三安光电) rather than the true bottleneck upstream company (鑫耀半导). The original framework's classification system does not prevent this error.

## Decision

Reclassify supply-chain layers by **process node and value-add stage**, not by material chemistry.

### Process Node Hierarchy

```
Raw Material
    ↓
Substrate (晶圆衬底)
    ↓
Epitaxy (外延生长)
    ↓
Device Fabrication (器件制造)
    ↓
Module Assembly (模块封装)
    ↓
System Integration (系统集成)
```

### AI Infrastructure Supply-Chain Ontology

| Process Node | Example Products | Example Companies | Barrier Profile |
|-------------|------------------|-------------------|-----------------|
| Raw Material | High-purity metals, specialty gases, CMP slurry | - | Commodity/contractual |
| Substrate | Si wafer, InP substrate, GaAs substrate, SOI wafer | Shin-Etsu, SUMCO, 鑫耀半导 | Very high (crystal growth) |
| Epitaxy | InP epitaxy, GaN-on-SiC, epi-wafers | 三安光电, IQE | High (MOCVD/MBE equipment) |
| Device Fabrication | EML laser chip, DFB laser, modulator, TIA | Lumentum, Coherent, Macom | Very high (design + fab) |
| Module Assembly | 800G/1.6T optical transceiver, CPO module | Coherent, Fabrinet, 中际旭创 | Medium-high (assembly + test) |
| System Integration | AI server, switch, data center infrastructure | NVIDIA, Cisco, Dell | Medium (integration + software) |

### Validation Rule

Before analyzing any company, the agent MUST:
1. Identify which specific process node the company occupies
2. Verify this node against at least two independent sources (company filings, industry reports, customer disclosures)
3. Reject analysis if node identification is ambiguous or contradictory

## Consequences

### Positive
- Prevents "search engine bias" where downstream companies are misidentified as upstream bottleneck plays.
- Enables correct competitive mapping (a substrate company competes with other substrate companies, not epitaxy companies).
- Allows accurate barrier assessment (crystal growth barrier ≠ MOCVD barrier).

### Negative
- Requires maintaining an up-to-date ontology as new process technologies emerge (e.g., silicon photonics blurring device/module boundary).
- More complex for agent to reason about — requires explicit node validation step.

## Related Decisions
- ADR-0004: Fact-Opinion-Inference Tagging (node validation facts must be triangulated)
- ADR-0005: Cross-Validation Layer (downstream customer data helps confirm node position)
