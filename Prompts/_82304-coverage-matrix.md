---
id: 725af57
title: "_82304 coverage matrix"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Index"
process: "[Software Development Process](../Canvases/Software%20Development%20Process.canvas)"
requirements:
owner: "[Head of Quality Management](../Assets/Head%20of%20Quality%20Management.md)"
---

# IEC 82304-1:2016 Prompt Coverage Matrix

This matrix maps IEC 82304-1 requirements to code-as-truth extraction and generation prompts.

## Hierarchy Note

IEC 82304-1 applies at the **system level** (product scope). It operates above IEC 62304:

```
┌─────────────────────────────────────────────────────────────────────┐
│ SYSTEM LEVEL — IEC 82304-1 Primary Scope                            │
│                                                                      │
│ Owns: Use Requirements (4.2), System Requirements (4.5),            │
│       Validation (6), IFU (7.2.2), Technical Description (7.2.3)    │
│                                                                      │
│ Extraction: extraction-82304-use-requirements.md                    │
│             extraction-82304-system-requirements.md                 │
│             extraction-82304-validation.md                          │
│             extraction-82304-accompanying-docs.md                   │
│                                                                      │
│ Generation: generation-82304-ifu.md                                 │
│             generation-82304-technical-description.md               │
│             generation-82304-validation-report.md                   │
├─────────────────────────────────────────────────────────────────────┤
│ MODULE LEVEL — Aggregation of Items         ▲                       │
│ module-aggregation-*.md                     │ aggregates            │
├─────────────────────────────────────────────────────────────────────┤
│ ITEM LEVEL — IEC 62304 Scope                ▲                       │
│ item-extraction-62304-*.md                  │ aggregates            │
│ item-generation-62304-*.md                                          │
└─────────────────────────────────────────────────────────────────────┘
```

## Relationship to IEC 62304

IEC 82304-1 builds on IEC 62304 for standalone health software products. Where 82304-1 references 62304, the corresponding 62304 prompts apply at item/module level.

```
IEC 82304-1 (Product/System Level)
    │
    ├── Clause 4: Product requirements (NEW - System Level)
    ├── Clause 5: Software lifecycle → References IEC 62304 (Item Level)
    ├── Clause 6: Product validation (NEW - System Level)
    ├── Clause 7: Accompanying documents (NEW - System Level)
    └── Clause 8: Post-market activities (NEW - System Level)
```

## Coverage Summary

| Clause | Title | Coverage | Prompt(s) |
|--------|-------|----------|-----------|
| **4.1** | General Requirements & Initial Risk Assessment | ⚠️ Partial | `extraction-62304-risk.md` + manual |
| **4.2** | Health Software Product Use Requirements | ✅ Covered | `extraction-82304-use-requirements.md` |
| **4.3** | Verification of Use Requirements | ✅ Covered | `extraction-82304-use-requirements.md` |
| **4.4** | Updating Use Requirements | ✅ Covered | Version tracking in extraction |
| **4.5** | System Requirements | ✅ Covered | `extraction-82304-system-requirements.md` |
| **4.6** | Verification of System Requirements | ✅ Covered | `extraction-82304-system-requirements.md` |
| **4.7** | Updating System Requirements | ✅ Covered | Version tracking in extraction |
| **5** | Software Life Cycle Processes | ✅ Covered | All `extraction-62304-*.md` prompts |
| **6.1** | Validation Plan | ✅ Covered | `extraction-82304-validation.md` |
| **6.2** | Performing Validation | ✅ Covered | `extraction-82304-validation.md` |
| **6.3** | Validation Report | ✅ Covered | `generation-82304-validation-report.md` |
| **7.1** | Identification | ✅ Covered | `extraction-82304-accompanying-docs.md` |
| **7.2.1** | Accompanying Documents General | ✅ Covered | `extraction-82304-accompanying-docs.md` |
| **7.2.2** | Instructions for Use | ✅ Covered | `generation-82304-ifu.md` |
| **7.2.3** | Technical Description | ✅ Covered | `generation-82304-technical-description.md` |
| **8.1** | Post-Market General | ⚠️ Partial | `extraction-82304-post-market.md` |
| **8.2** | Software Maintenance | ✅ Covered | `extraction-62304-change-control.md` |
| **8.3** | Re-Validation | ✅ Covered | `extraction-82304-validation.md` |
| **8.4** | Post-Market Communication | ✅ Covered | `extraction-82304-post-market.md` |
| **8.5** | Decommissioning and Disposal | ⚠️ Partial | Manual process |

## Detailed Clause Coverage

### Clause 4: Health Software Product Requirements

#### 4.1 General Requirements and Initial Risk Assessment

| Requirement | Coverage | Prompt | Notes |
|-------------|----------|--------|-------|
| Document intended use | ✅ | `extraction-82304-use-requirements.md` | From README, package.json, config |
| Document intended user profile | ✅ | `extraction-82304-use-requirements.md` | From docs and UI patterns |
| Document intended operational environment | ✅ | `extraction-82304-use-requirements.md` | From deployment configs |
| Identify hazards and estimate risks | ⚠️ | `extraction-62304-risk.md` | Links to ISO 14971 |
| Identify need for risk control measures | ⚠️ | `extraction-62304-risk.md` | Risk management process |

#### 4.2 Health Software Product Use Requirements

| Requirement | Coverage | Prompt |
|-------------|----------|--------|
| 4.2 a) Requirements addressing intended use | ✅ | `extraction-82304-use-requirements.md` |
| 4.2 b) Interface requirements | ✅ | `extraction-82304-use-requirements.md` |
| 4.2 c) Immunity from unintended influence | ✅ | Security analysis |
| 4.2 d) Privacy and security requirements | ✅ | `extraction-82304-use-requirements.md` |
| 4.2 e) Accompanying document requirements | ✅ | `extraction-82304-accompanying-docs.md` |
| 4.2 f.1) Upgrade support | ✅ | `extraction-82304-use-requirements.md` |
| 4.2 f.2) Rollback support | ✅ | `extraction-82304-use-requirements.md` |
| 4.2 f.3) Security patches/updates | ✅ | `extraction-82304-use-requirements.md` |
| 4.2 f.4) Software distribution integrity | ✅ | `extraction-82304-use-requirements.md` |
| 4.2 f.5) Decommissioning/data handling | ✅ | `extraction-82304-use-requirements.md` |
| 4.2 g) Regulatory requirements | ✅ | `extraction-82304-use-requirements.md` |

#### 4.3-4.4 Verification and Updates

| Requirement | Coverage | Prompt |
|-------------|----------|--------|
| 4.3 a) Requirements defined as system input | ✅ | Traceability in extraction |
| 4.3 b) Manufacturer can meet requirements | ✅ | Gap analysis |
| 4.4 Update requirements as needed | ✅ | Git version tracking |

#### 4.5 System Requirements

| Requirement | Coverage | Prompt |
|-------------|----------|--------|
| 4.5 a) Inter-operability | ✅ | `extraction-82304-system-requirements.md` |
| 4.5 b) Localization and language | ✅ | `extraction-82304-system-requirements.md` |
| 4.5 c) Risk control measures | ✅ | Links to risk extraction |
| 4.5 d) User interface specification | ✅ | `extraction-82304-system-requirements.md` |
| 4.5 e) Platform requirements | ✅ | `extraction-82304-system-requirements.md` |
| 4.5 f) Security compromise detection | ✅ | `extraction-82304-system-requirements.md` |
| 4.5 g) Essential function protection | ✅ | `extraction-82304-system-requirements.md` |
| 4.5 h) Configuration recovery | ✅ | `extraction-82304-system-requirements.md` |

#### 4.6-4.7 Verification and Updates

| Requirement | Coverage | Prompt |
|-------------|----------|--------|
| 4.6 a) No contradictions | ✅ | Validation in extraction |
| 4.6 b) Avoid ambiguity | ✅ | Validation in extraction |
| 4.6 c) Testable | ✅ | Links to verification |
| 4.6 d) Uniquely identifiable | ✅ | Requirement IDs |
| 4.7 Update as needed | ✅ | Git version tracking |

### Clause 5: Software Life Cycle Processes

**Covered entirely by IEC 62304 prompts:**

| Sub-clause | IEC 62304 Prompt |
|------------|------------------|
| Software requirements | `extraction-62304-requirements.md` |
| Architecture | `extraction-62304-architecture.md` |
| Detailed design | `extraction-62304-detailed-design.md` |
| Implementation | Code is the implementation |
| Verification | `extraction-62304-verification.md` |
| Configuration | `extraction-62304-soup.md`, `extraction-62304-change-control.md` |
| Problem resolution | `extraction-62304-change-control.md` |

### Clause 6: Health Software Product Validation

| Requirement | Coverage | Prompt |
|-------------|----------|--------|
| 6.1 a) Validation scope and activities | ✅ | `extraction-82304-validation.md` |
| 6.1 b) Constraints identification | ✅ | `extraction-82304-validation.md` |
| 6.1 c) Validation methods and criteria | ✅ | `extraction-82304-validation.md` |
| 6.1 d) Enabling systems/services | ✅ | `extraction-82304-validation.md` |
| 6.1 e) Personnel qualification | ⚠️ | Manual |
| 6.1 f) Independence from design team | ⚠️ | Manual |
| 6.2 Perform validation activities | ✅ | `extraction-82304-validation.md` |
| 6.3 a) Traceability to use requirements | ✅ | `extraction-82304-validation.md` |
| 6.3 b) Evidence of meeting requirements | ✅ | `generation-82304-validation-report.md` |
| 6.3 c) Residual risk acceptable | ⚠️ | Links to risk management |

### Clause 7: Identification and Accompanying Documents

| Requirement | Coverage | Prompt |
|-------------|----------|--------|
| 7.1 Product identification | ✅ | `extraction-82304-accompanying-docs.md` |
| 7.2.1 General document requirements | ✅ | `extraction-82304-accompanying-docs.md` |
| 7.2.2.1 IFU General | ✅ | `generation-82304-ifu.md` |
| 7.2.2.2 Software description | ✅ | `generation-82304-ifu.md` |
| 7.2.2.3 Safety/security warnings | ✅ | `generation-82304-ifu.md` |
| 7.2.2.4 Installation | ✅ | `generation-82304-ifu.md` |
| 7.2.2.5 Start-up procedure | ✅ | `generation-82304-ifu.md` |
| 7.2.2.6 Shutdown procedure | ✅ | `generation-82304-ifu.md` |
| 7.2.2.7 Operating instructions | ✅ | `generation-82304-ifu.md` |
| 7.2.2.8 Messages | ✅ | `generation-82304-ifu.md` |
| 7.2.2.9 Decommissioning | ✅ | `generation-82304-ifu.md` |
| 7.2.2.10 Reference to technical description | ✅ | `generation-82304-ifu.md` |
| 7.2.3.1 Technical description general | ✅ | `generation-82304-technical-description.md` |
| 7.2.3.2 IT-network requirements | ✅ | `generation-82304-technical-description.md` |

### Clause 8: Post-Market Activities

| Requirement | Coverage | Prompt |
|-------------|----------|--------|
| 8.1 General lifecycle coverage | ✅ | Design coverage |
| 8.2 Software maintenance | ✅ | `extraction-62304-change-control.md` |
| 8.3 Re-validation | ✅ | `extraction-82304-validation.md` |
| 8.4 a) New features communication | ✅ | `extraction-82304-post-market.md` |
| 8.4 b) Corrected errors | ✅ | `extraction-82304-post-market.md` |
| 8.4 c) Safety/security impact | ✅ | `extraction-82304-post-market.md` |
| 8.4 d) Identification updates | ✅ | `extraction-82304-post-market.md` |
| 8.4 e) Documentation updates | ✅ | `extraction-82304-post-market.md` |
| 8.5 Decommissioning | ⚠️ | Manual process |

## Extraction Prompts

| Prompt | IEC 82304-1 Coverage |
|--------|---------------------|
| `extraction-82304-use-requirements.md` | 4.1, 4.2, 4.3, 4.4 |
| `extraction-82304-system-requirements.md` | 4.5, 4.6, 4.7 |
| `extraction-82304-validation.md` | 6.1, 6.2, 6.3, 8.3 |
| `extraction-82304-accompanying-docs.md` | 7.1, 7.2.1 |
| `extraction-82304-post-market.md` | 8.1, 8.4 |

## Generation Prompts

| Prompt | Output Document | IEC 82304-1 Coverage |
|--------|-----------------|---------------------|
| `generation-82304-ifu.md` | Instructions for Use | 7.2.2 |
| `generation-82304-technical-description.md` | Technical Description | 7.2.3 |
| `generation-82304-validation-report.md` | Validation Report | 6.3 |

## Gaps Requiring Manual Process

| Gap | Clause | Recommendation |
|-----|--------|----------------|
| Personnel qualification for validation | 6.1 e) | QMS training records |
| Validation team independence | 6.1 f) | QMS organizational structure |
| Decommissioning procedures | 8.5 | Manual documentation |
| Initial risk assessment | 4.1 | Link to ISO 14971 process |

## Usage

### Single-Item Product (One Repository = One System)

```bash
# Phase 1: Item-level (IEC 62304 - Clause 5)
item-extraction-62304-requirements.md → item-requirements.json
item-extraction-62304-architecture.md → item-architecture.json
item-extraction-62304-soup.md → item-soup.json
item-extraction-62304-verification.md → item-verification.json
item-extraction-62304-risk-contribution.md → item-risk-contribution.json

# Phase 2: System-level (IEC 82304-1)
extraction-82304-use-requirements.md → use-requirements.json
extraction-82304-system-requirements.md → system-requirements.json
extraction-82304-validation.md → validation.json
system-aggregation-risk-file.md → product-risk-file.json

# Phase 3: Generation
generation-82304-ifu.md → IFU.md
generation-82304-technical-description.md → Technical-Description.md
generation-82304-validation-report.md → Validation-Report.md
```

### Multi-Item Product (Multiple Repositories)

```bash
# Phase 1: Item-level extraction (for EACH repository)
# IEC 62304 Clause 5 - run for each item
item-extraction-62304-*.md → items/<item>/*.json

# Phase 2: Module-level aggregation (for each module)
module-aggregation-*.md → modules/<module>/*.json

# Phase 3: System-level extraction (IEC 82304-1)
extraction-82304-use-requirements.md → system/use-requirements.json
extraction-82304-system-requirements.md → system/system-requirements.json
extraction-82304-validation.md → system/validation.json
system-aggregation-risk-file.md → system/product-risk-file.json

# Phase 4: System-level generation
generation-82304-ifu.md → IFU.md
generation-82304-technical-description.md → Technical-Description.md
generation-82304-validation-report.md → Validation-Report.md
```

### Key Points

1. IEC 82304-1 prompts operate at **system level** only
2. Use item/module outputs to populate system documentation
3. Validation requires traceability from system requirements → item verification
4. IFU and Technical Description aggregate from all levels
