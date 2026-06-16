---
id:
title: "Unified Regulatory Coverage Matrix"
version:
author:
effective_date:
type: "Index"
process: "[Document and Record Control](../Canvases/Document%20and%20Record%20Control.canvas)"
requirements:
owner: "[Head of Quality Management](../Assets/Head%20of%20Quality%20Management.md)"
---

# Unified Regulatory Coverage Matrix

This matrix shows how code-as-truth prompts cover ISO 14971, IEC 62304, and IEC 82304-1 requirements and how they integrate.

## Standards Relationship

```
┌─────────────────────────────────────────────────────────────────────┐
│                        ISO 14971:2019                               │
│                    Risk Management for Medical Devices              │
│         (Applies to all medical devices including software)         │
└─────────────────────────────┬───────────────────────────────────────┘
                              │ Risk management input
                              ▼
┌─────────────────────────────────────────────────────────────────────┐
│                     IEC 82304-1:2016                                │
│              Health Software Product Requirements                    │
│        (Product-level: use, system, validation, post-market)        │
└─────────────────────────────┬───────────────────────────────────────┘
                              │ References for software lifecycle
                              ▼
┌─────────────────────────────────────────────────────────────────────┐
│                   IEC 62304:2006+A1:2015                            │
│              Medical Device Software Life Cycle                      │
│    (Software-level: requirements, architecture, verification)       │
└─────────────────────────────────────────────────────────────────────┘
```

## Hierarchical Documentation Model

Documentation is generated at appropriate levels of the software hierarchy:

```
┌─────────────────────────────────────────────────────────────────────┐
│ SYSTEM LEVEL (IEC 82304-1 / ISO 14971 Scope)                        │
│                                                                      │
│ Owns: Use Requirements, System Requirements, Validation Report,     │
│       IFU, Technical Description, Product Risk File (RMP, RMR)      │
│                                                                      │
│ Prompts: system-extraction-*, system-generation-*,                  │
│          system-aggregation-*                                        │
├─────────────────────────────────────────────────────────────────────┤
│ MODULE LEVEL (Multi-Repository Components)                          │
│                            ▲                                         │
│                            │ aggregates                              │
│ Owns: Module Architecture, Module Integration, Module Risk          │
│                                                                      │
│ Prompts: module-aggregation-architecture.md,                        │
│          module-aggregation-soup.md, module-aggregation-risks.md,   │
│          module-aggregation-verification.md                          │
├─────────────────────────────────────────────────────────────────────┤
│ SOFTWARE ITEM LEVEL (Single Repository = IEC 62304 Scope)           │
│                            ▲                                         │
│                            │ aggregates                              │
│ Owns: Item Requirements, Item Architecture, Item SOUP,              │
│       Item Verification, Item Risk Contribution                      │
│                                                                      │
│ Prompts: item-extraction-62304-*, item-generation-62304-*           │
├─────────────────────────────────────────────────────────────────────┤
│ SOFTWARE UNIT LEVEL (Class/Function)                                 │
│                            ▲                                         │
│                            │ analyzed by                             │
│ Covered by: Unit tests, code coverage (no separate prompts)         │
└─────────────────────────────────────────────────────────────────────┘
```

### Documentation Ownership by Level

| Level | Documents Owned | Aggregates From |
|-------|-----------------|-----------------|
| **System** | Use Requirements (SRS), System Requirements, RMP, RMR, IFU, Technical Description, Validation Report | Module outputs |
| **Module** | Module Architecture, Module Integration Report, Module Risk | Item outputs |
| **Item** | Item Requirements, Item Architecture, Item SOUP, Item Verification, Item Risk Contribution | Code analysis |

## Complete Prompt Inventory

### Item-Level Prompts (Repository Scope)

**Extraction prompts** — Extract data from a single repository:

| Standard | Prompt | Clause | Output |
|----------|--------|--------|--------|
| IEC 62304 | `extraction/item/item-extraction-62304-requirements.md` | 5.2 | `item-requirements.json` |
| IEC 62304 | `extraction/item/item-extraction-62304-architecture.md` | 5.3 | `item-architecture.json` |
| IEC 62304 | `extraction/item/item-extraction-62304-soup.md` | 8.1 | `item-soup.json` |
| IEC 62304 | `extraction/item/item-extraction-62304-verification.md` | 5.5-5.7 | `item-verification.json` |
| IEC 62304 | `extraction/item/item-extraction-62304-risk-contribution.md` | 7 | `item-risk-contribution.json` |

**Generation prompts** — Generate item-level documents:

| Standard | Prompt | Output |
|----------|--------|--------|
| IEC 62304 | `generation/item/item-generation-62304-requirements.md` | Item-Requirements.md |
| IEC 62304 | `generation/item/item-generation-62304-architecture.md` | Item-Architecture.md |
| IEC 62304 | `generation/item/item-generation-62304-soup-list.md` | Item-SOUP-List.md |
| IEC 62304 | `generation/item/item-generation-62304-verification.md` | Item-Verification.md |
| IEC 62304 | `generation/item/item-generation-62304-risk-contribution.md` | Item-Risk-Contribution.md |

### Module-Level Prompts (Multi-Repository Scope)

**Aggregation prompts** — Combine item-level outputs:

| Standard | Prompt | Output |
|----------|--------|--------|
| IEC 62304 | `aggregation/module/module-aggregation-architecture.md` | `module-architecture.json` |
| IEC 62304 | `aggregation/module/module-aggregation-soup.md` | `module-soup.json` |
| IEC 62304 | `aggregation/module/module-aggregation-risks.md` | `module-risk.json` |
| IEC 62304 | `aggregation/module/module-aggregation-verification.md` | `module-verification.json` |

### System-Level Prompts (Product Scope)

**Extraction prompts** — Extract system-level data:

| Standard | Prompt | Clause | Output |
|----------|--------|--------|--------|
| IEC 82304-1 | `extraction-82304-use-requirements.md` | 4.1-4.4 | `use-requirements.json` |
| IEC 82304-1 | `extraction-82304-system-requirements.md` | 4.5-4.7 | `system-requirements.json` |
| IEC 82304-1 | `extraction-82304-validation.md` | 6.1-6.3 | `validation.json` |
| IEC 82304-1 | `extraction-82304-accompanying-docs.md` | 7.1-7.2 | `accompanying-docs.json` |
| IEC 82304-1 | `extraction-82304-post-market.md` | 8 | `post-market.json` |

**Aggregation prompts** — Combine module/item outputs into system-level:

| Standard | Prompt | Output |
|----------|--------|--------|
| ISO 14971 | `aggregation/system/system-aggregation-risk-file.md` | `product-risk-file.json` |
| IEC 62304 | `aggregation/system/system-aggregation-soup.md` | `system-soup.json` |
| IEC 62304 | `aggregation/system/system-aggregation-verification.md` | `system-verification.json` |

**Generation prompts** — Generate system-level documents:

| Standard | Prompt | Output |
|----------|--------|--------|
| ISO 14971 | `generation-14971-rmp.md` | Risk Management Plan |
| ISO 14971 | `generation-14971-rma.md` | Risk Analysis |
| ISO 14971 | `generation-14971-rmr.md` | Risk Management Report |
| IEC 82304-1 | `generation-82304-ifu.md` | Instructions for Use |
| IEC 82304-1 | `generation-82304-technical-description.md` | Technical Description |
| IEC 82304-1 | `generation-82304-validation-report.md` | Validation Report |
| IEC 62304 | `generation-62304-srs.md` | System Software Requirements Specification |

### Legacy Prompts (Flat Structure — Deprecated)

The following prompts treat a repository as a complete system. Retained for single-item products:

| Prompt | Replacement |
|--------|-------------|
| `extraction-62304-requirements.md` | `item-extraction-62304-requirements.md` |
| `extraction-62304-architecture.md` | `item-extraction-62304-architecture.md` |
| `extraction-14971-risks.md` | `item-extraction-62304-risk-contribution.md` + `system-aggregation-risk-file.md` |

## Cross-Standard Data Flow

### Hierarchical Risk Flow

```
ITEM LEVEL (per repository)
┌─────────────────────────────────────────────────────────────────────┐
│ item-extraction-62304-risk-contribution.md                          │
│   → item-risk-contribution.json                                      │
│   Contains: hazard contributions, controls implemented,              │
│             failure modes, upstream control requirements             │
└─────────────────────────────────────┬───────────────────────────────┘
                                      │ aggregates
                                      ▼
MODULE LEVEL (per deployable component)
┌─────────────────────────────────────────────────────────────────────┐
│ module-aggregation-risks.md                                          │
│   → module-risk.json                                                 │
│   Contains: cross-item hazards, module-level controls,               │
│             interface risk assessment, control gaps                  │
└─────────────────────────────────────┬───────────────────────────────┘
                                      │ aggregates
                                      ▼
SYSTEM LEVEL (product)
┌─────────────────────────────────────────────────────────────────────┐
│ system-aggregation-risk-file.md (ISO 14971)                          │
│   → product-risk-file.json                                           │
│   Contains: system hazards, risk acceptability decisions,            │
│             benefit-risk analysis, overall residual risk             │
│                                                                      │
│ generation-14971-rmp.md → RMP.md                                     │
│ generation-14971-rma.md → Risk-Analysis.md                           │
│ generation-14971-rmr.md → RMR.md                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Requirements Traceability (Hierarchical)

```
SYSTEM LEVEL
┌─────────────────────────────────────────────────────────────────────┐
│ Use Requirements (82304-1 4.2)                                       │
│       ↓ decomposed into                                              │
│ System Requirements (82304-1 4.5)                                    │
└─────────────────────────────────────┬───────────────────────────────┘
                                      │ traced to
                                      ▼
ITEM LEVEL
┌─────────────────────────────────────────────────────────────────────┐
│ Item Requirements (62304 5.2)                                        │
│       ↓                                                              │
│ Item Architecture (62304 5.3-5.4)                                    │
│       ↓                                                              │
│ Implementation (code)                                                │
│       ↓                                                              │
│ Item Verification (62304 5.5-5.7)                                    │
└─────────────────────────────────────┬───────────────────────────────┘
                                      │ aggregates to
                                      ▼
SYSTEM LEVEL
┌─────────────────────────────────────────────────────────────────────┐
│ System Validation (82304-1 6)                                        │
│       ↓                                                              │
│ Release (62304 5.8)                                                  │
└─────────────────────────────────────────────────────────────────────┘
```

## Regulatory Document to Standard Mapping

| Document | Primary Standard | Supporting Standards |
|----------|------------------|---------------------|
| Risk Management File | ISO 14971 | IEC 62304 clause 7 |
| Software Requirements Specification | IEC 62304 5.2 | IEC 82304-1 4.5 |
| Software Architecture Description | IEC 62304 5.3 | — |
| Detailed Design | IEC 62304 5.4 | — |
| SOUP List | IEC 62304 8.1 | — |
| Software Verification Report | IEC 62304 5.5-5.7 | IEC 82304-1 6 |
| Software Release Notes | IEC 62304 5.8 | — |
| Instructions for Use | IEC 82304-1 7.2.2 | — |
| Technical Description | IEC 82304-1 7.2.3 | — |
| Validation Report | IEC 82304-1 6.3 | — |

## Coverage by Safety Class

### Class A (Low Risk)

| Standard | Clauses Applicable | Prompts Required |
|----------|-------------------|------------------|
| ISO 14971 | All | `extraction-14971-risks.md` |
| IEC 82304-1 | All | All 82304 prompts |
| IEC 62304 | 4, 5.1.1-3/6-9, 5.2.1-2/4-6, 5.5.1, 5.7, 5.8.1-2/4/7-8, 6, 7.4.1, 8, 9 | Subset of 62304 prompts |

### Class B (Medium Risk)

| Standard | Clauses Applicable | Prompts Required |
|----------|-------------------|------------------|
| ISO 14971 | All | `extraction-14971-risks.md` |
| IEC 82304-1 | All | All 82304 prompts |
| IEC 62304 | All except 5.1.4, 5.3.5, 5.4.2-4, 5.5.4 | Most 62304 prompts |

### Class C (High Risk)

| Standard | Clauses Applicable | Prompts Required |
|----------|-------------------|------------------|
| ISO 14971 | All | `extraction-14971-risks.md` |
| IEC 82304-1 | All | All 82304 prompts |
| IEC 62304 | **All clauses** | **All 62304 prompts** |

## Integration Points

### 1. ISO 14971 ↔ IEC 62304

| ISO 14971 | IEC 62304 | Integration |
|-----------|-----------|-------------|
| 5.2 Intended use | 5.2 Software requirements | Same product scope |
| 5.4 Hazard identification | 7.1 Software hazards | Software-specific hazards |
| 7.1 Risk control option analysis | 7.2 Software risk controls | Controls in software |
| 7.2 Implementation | 5.2.3 Safety requirements | Requirements include controls |
| 7.3 Residual risk | 5.8.3 Residual anomalies | Anomaly = residual risk |

### 2. IEC 82304-1 ↔ IEC 62304

| IEC 82304-1 | IEC 62304 | Integration |
|-------------|-----------|-------------|
| 4.5 System requirements | 5.2 Software requirements | System → Software traceability |
| 5 Software lifecycle | All of 62304 | 82304-1 references 62304 |
| 6 Validation | 5.7 System testing | Validation vs verification |
| 8 Post-market | 6 Maintenance | Ongoing lifecycle |

### 3. ISO 14971 ↔ IEC 82304-1

| ISO 14971 | IEC 82304-1 | Integration |
|-----------|-------------|-------------|
| 5.2 Intended use | 4.2 Use requirements | Same intended use definition |
| 7 Risk control | 4.5 c) Risk control measures | System-level controls |
| 8 Production info | 7.2.2 IFU warnings | Safety information in IFU |

## Gap Analysis Summary

### Fully Covered (Code-Extractable)

✅ Software requirements (62304 5.2)
✅ Architecture (62304 5.3)
✅ Detailed design (62304 5.4)
✅ Verification (62304 5.5-5.7)
✅ SOUP management (62304 8.1)
✅ Change control (62304 8.2-8.3)
✅ Problem resolution (62304 9)
✅ Software risk management (62304 7)
✅ Use requirements (82304-1 4.2)
✅ System requirements (82304-1 4.5)
✅ Hazard identification (14971 5.4)
✅ Risk controls (14971 7)

### Partially Covered (Requires Manual Input)

⚠️ Software development planning (62304 5.1) — Plan document manual
⚠️ Intended use definition (14971 5.2) — Initial definition manual
⚠️ Risk acceptability criteria (14971 6) — Criteria manual
⚠️ Validation independence (82304-1 6.1 e-f) — Organizational
⚠️ Post-market feedback integration (82304-1 8) — External data

### Manual Only

❌ QMS compliance (62304 4.1) — QMS documents
❌ Legacy software assessment (62304 4.4) — Case-by-case
❌ Decommissioning (82304-1 8.5) — Procedure document
❌ Clinical evaluation — Outside code scope
❌ Usability engineering file — Partially extractable

## Execution Summary

### Single-Item Product (One Repository)

For simple products where one repository = one system:

```bash
# Phase 1: Item-level extraction (per repository)
item-extraction-62304-requirements.md → item-requirements.json
item-extraction-62304-architecture.md → item-architecture.json
item-extraction-62304-soup.md → item-soup.json
item-extraction-62304-verification.md → item-verification.json
item-extraction-62304-risk-contribution.md → item-risk-contribution.json

# Phase 2: System-level (for single-item, same as item)
extraction-82304-use-requirements.md → use-requirements.json
extraction-82304-system-requirements.md → system-requirements.json
system-aggregation-risk-file.md → product-risk-file.json

# Phase 3: Generation
generation-14971-rmp.md → RMP.md
generation-14971-rma.md → Risk-Analysis.md
generation-14971-rmr.md → RMR.md
generation-82304-ifu.md → IFU.md
generation-82304-technical-description.md → Technical-Description.md
```

### Multi-Item Product (Multiple Repositories)

For complex products with multiple software items:

```bash
# Phase 1: Item-level extraction (for EACH repository)
# Run in parallel across all items:
item-extraction-62304-requirements.md → items/<item>/item-requirements.json
item-extraction-62304-architecture.md → items/<item>/item-architecture.json
item-extraction-62304-soup.md → items/<item>/item-soup.json
item-extraction-62304-verification.md → items/<item>/item-verification.json
item-extraction-62304-risk-contribution.md → items/<item>/item-risk-contribution.json

# Phase 2: Module-level aggregation (for each module)
module-aggregation-architecture.md → modules/<module>/module-architecture.json
module-aggregation-soup.md → modules/<module>/module-soup.json
module-aggregation-risks.md → modules/<module>/module-risk.json
module-aggregation-verification.md → modules/<module>/module-verification.json

# Phase 3: System-level extraction + aggregation
extraction-82304-use-requirements.md → system/use-requirements.json
extraction-82304-system-requirements.md → system/system-requirements.json
system-aggregation-risk-file.md → system/product-risk-file.json
system-aggregation-soup.md → system/system-soup.json
system-aggregation-verification.md → system/system-verification.json

# Phase 4: System-level generation
generation-14971-rmp.md → RMP.md
generation-14971-rma.md → Risk-Analysis.md
generation-14971-rmr.md → RMR.md
generation-82304-ifu.md → IFU.md
generation-82304-technical-description.md → Technical-Description.md
generation-82304-validation-report.md → Validation-Report.md
```

### Class C Addition

```bash
# Add detailed design extraction for each Class C item
item-extraction-62304-detailed-design.md → item-detailed-design.json
```

## Quick Reference

### By Document Type

| I need to generate... | Run these extractions | Then generate with |
|-----------------------|----------------------|-------------------|
| SRS | 82304-use, 82304-system, 62304-requirements | generation-62304-srs |
| SAD | 62304-requirements, 62304-architecture, 62304-soup | generation-62304-sad |
| Risk File | 14971-risks, 62304-risk | generation-62304-risk-file |
| SOUP List | 62304-architecture, 62304-soup | generation-62304-soup-list |
| Verification Report | All 62304 extractions | generation-62304-verification-report |
| Release Notes | 62304-verification, 62304-change-control | generation-62304-release-notes |
| IFU | 82304-use, 82304-system | generation-82304-ifu |
| Technical Description | 82304-system, 62304-architecture | generation-82304-technical-description |
| Validation Report | 82304-validation | generation-82304-validation-report |

### By Change Type

| What changed | Re-extract | Re-generate |
|--------------|------------|-------------|
| Source code (src/) | requirements, architecture, risk, verification | SRS, SAD, Risk File, SVR |
| Tests (tests/) | verification | SVR, Release Notes |
| Dependencies (package.json) | soup | SOUP List, SAD |
| Documentation (README, docs/) | use-requirements | IFU, Tech Description |
| Any safety control | risks, software-risk | Risk File |
| New release | change-control | Release Notes |
