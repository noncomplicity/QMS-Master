---
id: 725af57
title: "_14971 coverage matrix"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Index"
process: "[Risk Management Process](../Canvases/Risk%20Management%20Process.canvas)"
requirements:
owner: "[Head of Risk Management](../Assets/Head%20of%20Risk%20Management.md)"
---

# ISO 14971:2019 Prompt Coverage Matrix

This matrix maps ISO 14971 requirements to code-as-truth extraction and generation prompts.

## Hierarchy Note

ISO 14971 applies at the **product level**, but risk information is extracted and aggregated hierarchically:

```
┌─────────────────────────────────────────────────────────────────────┐
│ SYSTEM LEVEL — ISO 14971 Scope                                       │
│ system-aggregation-risk-file.md → product-risk-file.json            │
│ Owns: Risk acceptability, benefit-risk, overall residual risk       │
│                                                                      │
│ generation-14971-rmp.md → RMP.md                                     │
│ generation-14971-rma.md → Risk-Analysis.md                           │
│ generation-14971-rmr.md → RMR.md                                     │
├─────────────────────────────────────────────────────────────────────┤
│ MODULE LEVEL — Cross-Item Risk Aggregation           ▲               │
│ module-aggregation-risks.md → module-risk.json      │               │
│ Identifies: Cross-item hazards, interface risks     │ aggregates    │
├─────────────────────────────────────────────────────────────────────┤
│ ITEM LEVEL — IEC 62304 Clause 7 Scope               ▲               │
│ item-extraction-62304-risk-contribution.md          │               │
│   → item-risk-contribution.json                     │ aggregates    │
│ Identifies: Hazard contributions, controls, failure modes           │
└─────────────────────────────────────────────────────────────────────┘
```

## Relationship to IEC 62304 and IEC 82304-1

ISO 14971 applies to all medical devices including software. For software-specific risk management:

```
ISO 14971:2019 (Product-Level Risk Management)
    │
    ├── IEC 62304 Clause 7 (Software Risk Management)
    │   └── Software-specific hazard analysis (item-level)
    │   └── Software risk controls (item-level)
    │   └── Risk control verification (item + module level)
    │
    └── IEC 82304-1 Clause 4.1 (Initial Risk Assessment)
        └── Use requirement risk analysis (system-level)
        └── System-level risk controls (system-level)
```

## Coverage Summary

| Clause | Title | Coverage | Prompt(s) |
|--------|-------|----------|-----------|
| **3** | Terms and Definitions | N/A | Reference only |
| **4.1** | Risk Management Process | ⚠️ Partial | QMS procedure (manual) |
| **4.2** | Management Responsibilities | ❌ Manual | QMS documentation |
| **4.3** | Competence of Personnel | ❌ Manual | Training records |
| **4.4** | Risk Management Plan | ✅ Covered | `generation-14971-rmp.md` |
| **4.5** | Risk Management File | ✅ Covered | All 14971 prompts combined |
| **5.1** | Risk Analysis Process | ✅ Covered | `extraction-14971-risks.md` |
| **5.2** | Intended Use | ✅ Covered | `extraction-14971-risks.md` |
| **5.3** | Safety Characteristics | ✅ Covered | `extraction-14971-risks.md` |
| **5.4** | Hazard Identification | ✅ Covered | `extraction-14971-risks.md` |
| **5.5** | Risk Estimation | ✅ Covered | `extraction-14971-risks.md` |
| **6** | Risk Evaluation | ✅ Covered | `generation-14971-rma.md` |
| **7.1** | Risk Control Option Analysis | ✅ Covered | `extraction-14971-risks.md` |
| **7.2** | Risk Control Implementation | ✅ Covered | `extraction-14971-risks.md` |
| **7.3** | Residual Risk Evaluation | ✅ Covered | `extraction-14971-risks.md` |
| **7.4** | Benefit-Risk Analysis | ⚠️ Partial | `generation-14971-rma.md` (template) |
| **7.5** | Risks from Control Measures | ✅ Covered | `extraction-14971-risks.md` |
| **7.6** | Completeness of Risk Control | ✅ Covered | `generation-14971-rma.md` |
| **8** | Overall Residual Risk | ✅ Covered | `generation-14971-rmr.md` |
| **9** | Risk Management Review | ✅ Covered | `generation-14971-rmr.md` |
| **10** | Production/Post-Production | ⚠️ Partial | `extraction-82304-post-market.md` |

## Detailed Clause Coverage

### Clause 4: General Requirements for Risk Management

| Sub-clause | Requirement | Coverage | Notes |
|------------|-------------|----------|-------|
| 4.1 | Risk management process | ⚠️ Partial | QMS procedure manual; prompts implement the process |
| 4.2 | Management responsibilities | ❌ Manual | Organizational responsibility |
| 4.3 | Competence of personnel | ❌ Manual | Training records |
| 4.4 a) | RMP: Scope of planned activities | ✅ | `generation-14971-rmp.md` Section 1 |
| 4.4 b) | RMP: Assignment of responsibilities | ✅ | `generation-14971-rmp.md` Section 4 |
| 4.4 c) | RMP: Review requirements | ✅ | `generation-14971-rmp.md` Section 6-7 |
| 4.4 d) | RMP: Risk acceptability criteria | ✅ | `generation-14971-rmp.md` Section 5 |
| 4.4 e) | RMP: Overall residual risk method | ✅ | `generation-14971-rmp.md` Section 5.5 |
| 4.4 f) | RMP: Verification activities | ✅ | `generation-14971-rmp.md` Section 7 |
| 4.4 g) | RMP: Production/post-production | ✅ | `generation-14971-rmp.md` Section 8 |
| 4.5 | Risk management file | ✅ | Combined output of all prompts |

### Clause 5: Risk Analysis

| Sub-clause | Requirement | Coverage | Prompt |
|------------|-------------|----------|--------|
| 5.1 | Risk analysis process | ✅ | `extraction-14971-risks.md` |
| 5.2 | Intended use and misuse | ✅ | `extraction-14971-risks.md` → `intended_use` |
| 5.3 | Safety characteristics | ✅ | `extraction-14971-risks.md` → `safety_characteristics` |
| 5.4 | Hazard identification | ✅ | `extraction-14971-risks.md` → `hazards` |
| 5.4 | Hazardous situations | ✅ | `extraction-14971-risks.md` → `hazardous_situations` |
| 5.5 | Risk estimation - severity | ✅ | `hazardous_situations[].severity` |
| 5.5 | Risk estimation - probability | ✅ | `hazardous_situations[].probability` |

### Clause 6: Risk Evaluation

| Sub-clause | Requirement | Coverage | Prompt |
|------------|-------------|----------|--------|
| 6 | Risk evaluation against criteria | ✅ | `extraction-14971-risks.md` → `initial_risk_level` |
| 6 | Apply risk acceptability criteria | ✅ | `generation-14971-rma.md` Section 6 |

### Clause 7: Risk Control

| Sub-clause | Requirement | Coverage | Prompt |
|------------|-------------|----------|--------|
| 7.1 | Risk control option analysis | ✅ | `extraction-14971-risks.md` → `risk_controls[].control_type` |
| 7.1 a) | Inherent safety by design | ✅ | `control_type: "inherent_safety"` |
| 7.1 b) | Protective measures | ✅ | `control_type: "protective_measure"` |
| 7.1 c) | Information for safety | ✅ | `control_type: "information_for_safety"` |
| 7.2 | Implementation of controls | ✅ | `risk_controls[].implementation` |
| 7.2 | Verification of implementation | ✅ | `risk_controls[].verification` |
| 7.3 | Residual risk evaluation | ✅ | `extraction-14971-risks.md` → `residual_risks` |
| 7.4 | Benefit-risk analysis | ⚠️ Partial | `generation-14971-rma.md` template; clinical input manual |
| 7.5 | Risks from control measures | ✅ | `generation-14971-rma.md` Section 7.2 "New Hazards" |
| 7.6 | Completeness of risk control | ✅ | `generation-14971-rma.md` Section 9 |

### Clause 8: Evaluation of Overall Residual Risk

| Sub-clause | Requirement | Coverage | Prompt |
|------------|-------------|----------|--------|
| 8 | Overall residual risk evaluation | ✅ | `generation-14971-rmr.md` Section 4 |
| 8 | Benefit-risk for overall | ✅ | `generation-14971-rmr.md` Section 4.2-4.3 |
| 8 | Disclosure of residual risk | ✅ | `generation-14971-rmr.md` Section 4.5 |

### Clause 9: Risk Management Review

| Sub-clause | Requirement | Coverage | Prompt |
|------------|-------------|----------|--------|
| 9 a) | RMP appropriately implemented | ✅ | `generation-14971-rmr.md` Section 2 |
| 9 b) | Overall residual risk acceptable | ✅ | `generation-14971-rmr.md` Section 4.4 |
| 9 c) | Methods for post-production | ✅ | `generation-14971-rmr.md` Section 5 |

### Clause 10: Production and Post-Production Activities

| Sub-clause | Requirement | Coverage | Prompt |
|------------|-------------|----------|--------|
| 10.1 | Information collection system | ⚠️ Partial | `extraction-82304-post-market.md` |
| 10.2 | Information review | ⚠️ Partial | Manual process definition |
| 10.3 | Actions from post-production | ⚠️ Partial | Manual process definition |

## Prompt Inventory by Level

### Item-Level Prompts (Repository Scope)

| Prompt | ISO 14971 Coverage | Output |
|--------|-------------------|--------|
| `item-extraction-62304-risk-contribution.md` | 5.4, 7.1-7.3 (partial) | `item-risk-contribution.json` |
| `item-generation-62304-risk-contribution.md` | — | Item-Risk-Contribution.md |

**Item-level captures:**
- Hazard contributions (what this item can cause)
- Risk controls implemented in code
- Failure modes
- Interfaces with safety impact
- Upstream controls needed

**Item-level does NOT decide:**
- Risk acceptability (system-level decision)
- Benefit-risk analysis (system-level)
- Overall residual risk (system-level)

### Module-Level Prompts (Multi-Item Scope)

| Prompt | ISO 14971 Coverage | Output |
|--------|-------------------|--------|
| `module-aggregation-risks.md` | 5.4, 7.1-7.3, 7.5 | `module-risk.json` |

**Module-level captures:**
- Cross-item hazards
- Interface risk assessment
- Module-level controls
- Control gaps requiring system action

### System-Level Prompts (Product Scope)

| Prompt | ISO 14971 Coverage | Output |
|--------|-------------------|--------|
| `system-aggregation-risk-file.md` | 4.5, 5, 6, 7, 8 | `product-risk-file.json` |
| `generation-14971-rmp.md` | 4.4 | Risk Management Plan |
| `generation-14971-rma.md` | 5, 6, 7 | Risk Analysis |
| `generation-14971-rmr.md` | 8, 9 | Risk Management Report |

**System-level owns:**
- Risk acceptability criteria
- Benefit-risk analysis
- Overall residual risk evaluation
- Disclosure requirements

## Integration with IEC 62304

| ISO 14971 | IEC 62304 | Integration |
|-----------|-----------|-------------|
| 5.4 Hazard identification | 7.1 Software hazard analysis | 62304 adds software-specific hazards |
| 7.1 Risk control options | 7.2 Risk control measures | 62304 specifies software controls |
| 7.2 Implementation | 7.2.2 Software requirements | Controls become software requirements |
| 7.2 Verification | 7.3 Risk control verification | 62304 adds verification methods |
| 7.3 Residual risk | 5.8.3 Residual anomalies | Anomalies linked to residual risk |

### Cross-Standard Data Flow (Hierarchical)

```
ITEM LEVEL (per repository)
├── item-extraction-62304-risk-contribution.md
│   └── item-risk-contribution.json
│       ├── hazard_contributions[]
│       ├── risk_controls_implemented[]
│       ├── risk_controls_required_upstream[]
│       ├── failure_modes[]
│       └── interfaces_with_safety_impact[]
│
▼ aggregates to
MODULE LEVEL (per deployable component)
├── module-aggregation-risks.md
│   └── module-risk.json
│       ├── module_hazards[] (mapped from items)
│       ├── cross_item_hazards[] (new at module level)
│       ├── risk_controls[] (aggregated + module-level)
│       ├── interface_risk_assessment[]
│       └── control_gaps[]
│
▼ aggregates to
SYSTEM LEVEL (product)
├── system-aggregation-risk-file.md
│   └── product-risk-file.json
│       ├── risk_acceptability_criteria
│       ├── system_hazards[] (mapped from modules/items)
│       ├── cross_item_hazards[] (system scope)
│       ├── risk_controls{} (complete hierarchy)
│       ├── residual_risk_evaluation[]
│       ├── benefit_risk_analysis{}
│       └── overall_residual_risk{}
│
└── generation-14971-*.md
    ├── RMP.md (Risk Management Plan)
    ├── Risk-Analysis.md (Full Risk Analysis)
    └── RMR.md (Risk Management Report)
```

## Integration with IEC 82304-1

| ISO 14971 | IEC 82304-1 | Integration |
|-----------|-------------|-------------|
| 5.2 Intended use | 4.2 Use requirements | Same intended use definition |
| 7.1 Risk control | 4.5 c) System requirements | System-level controls |
| 7.3 Residual risk | 6.3 c) Validation | Validation confirms risk acceptability |
| 8 Disclosure | 7.2.2 IFU | Safety information in Instructions for Use |
| 10 Post-production | 8 Post-market | Ongoing risk monitoring |

## Gaps Requiring Manual Process

| Gap | Clause | Priority | Recommendation |
|-----|--------|----------|----------------|
| Management responsibilities | 4.2 | Medium | QMS documentation |
| Personnel competence records | 4.3 | Medium | Training management system |
| Clinical benefit quantification | 7.4 | Medium | Clinical evaluation input |
| Post-production procedures | 10 | Medium | Link to `extraction-82304-post-market.md` |

## Risk Management File Contents

The Risk Management File (clause 4.5) is assembled from:

| Document | Source | Status |
|----------|--------|--------|
| Risk Management Plan | `generation-14971-rmp.md` | Generated |
| Intended Use Statement | `extraction-14971-risks.md` → `intended_use` | Extracted |
| Risk Analysis | `generation-14971-rma.md` | Generated |
| Risk Evaluation Records | Included in Risk Analysis | Generated |
| Risk Control Records | Included in Risk Analysis | Generated |
| Residual Risk Evaluation | Included in Risk Analysis | Generated |
| Overall Residual Risk Evaluation | `generation-14971-rmr.md` Section 4 | Generated |
| Risk Management Report | `generation-14971-rmr.md` | Generated |
| Production/Post-Production Info | `extraction-82304-post-market.md` | Extracted |

## Usage

### For New Products

1. Run `extraction-14971-risks.md` against codebase
2. Generate `generation-14971-rmp.md` (may need manual input for acceptability criteria)
3. Generate `generation-14971-rma.md` from extracted risks
4. Review and complete benefit-risk analysis sections manually
5. Generate `generation-14971-rmr.md` as final review

### For Product Updates

1. Re-run `extraction-14971-risks.md` to identify new/changed hazards
2. Update Risk Analysis with new hazards and controls
3. Re-evaluate overall residual risk
4. Update Risk Management Report

### Integration with 62304/82304

1. Run `extraction-14971-risks.md` first (provides input to other extractions)
2. Run `extraction-62304-risk.md` (uses risks.json for software-specific analysis)
3. Run `extraction-82304-system-requirements.md` (4.5 c references risk controls)
4. Run `extraction-82304-validation.md` (6.3 c references residual risk)

## Compliance Evidence

| ISO 14971 Requirement | Evidence Location |
|----------------------|-------------------|
| 4.4 Risk Management Plan | `RMP-[product]-[version].md` |
| 5 Risk Analysis | `RA-[product]-[version].md` |
| 6 Risk Evaluation | `RA-[product]-[version].md` Section 6 |
| 7 Risk Control | `RA-[product]-[version].md` Section 7-8 |
| 8 Overall Residual Risk | `RMR-[product]-[version].md` Section 4 |
| 9 Risk Management Review | `RMR-[product]-[version].md` |
| 4.5 Risk Management File | Collection of above documents |
