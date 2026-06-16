---
id: 725af57
title: "_62304 coverage matrix"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Index"
process: "[Software Development Process](../Canvases/Software%20Development%20Process.canvas)"
requirements:
owner: "[Head of Quality Management](../Assets/Head%20of%20Quality%20Management.md)"
---

# IEC 62304:2006+A1:2015 Prompt Coverage Matrix

This matrix maps IEC 62304 requirements to code-as-truth extraction and generation prompts.

## Hierarchy Note

IEC 62304 applies primarily at the **software item level** (single repository). For multi-repository products:
- **Item-level prompts** extract from individual repositories
- **Module-level prompts** aggregate across related items
- **System-level prompts** aggregate for the complete product

```
System (IEC 82304-1 scope)
    └── Module (deployable component)
            └── Software Item (repository) ← IEC 62304 primary scope
                    └── Software Unit (class/function)
```

## Coverage Summary

| Clause | Title | Class | Coverage | Level | Prompt(s) |
|--------|-------|-------|----------|-------|-----------|
| **4** | General Requirements | A,B,C | ⚠️ Partial | — | Manual (QMS, Risk Management) |
| **5.1** | Software Development Planning | A,B,C | ⚠️ Partial | — | Manual (dev plan) |
| **5.2** | Software Requirements Analysis | A,B,C | ✅ Covered | Item | `item-extraction-62304-requirements.md` |
| **5.3** | Software Architectural Design | B,C | ✅ Covered | Item+Module | `item-extraction-62304-architecture.md`, `module-aggregation-architecture.md` |
| **5.4** | Software Detailed Design | C | ✅ Covered | Item | `item-extraction-62304-detailed-design.md` |
| **5.5** | Software Unit Implementation | B,C | ✅ Covered | Item | `item-extraction-62304-verification.md` |
| **5.6** | Software Integration Testing | B,C | ✅ Covered | Item+Module | `item-extraction-62304-verification.md`, `module-aggregation-verification.md` |
| **5.7** | Software System Testing | A,B,C | ✅ Covered | System | `system-aggregation-verification.md` |
| **5.8** | Software Release | A,B,C | ✅ Covered | System | `extraction-62304-change-control.md`, `generation-62304-release-notes.md` |
| **6** | Software Maintenance | A,B,C | ⚠️ Partial | — | Manual process |
| **7** | Software Risk Management | B,C | ✅ Covered | Item+System | `item-extraction-62304-risk-contribution.md`, `system-aggregation-risk-file.md` |
| **8.1** | Configuration Identification | A,B,C | ✅ Covered | Item+Module | `item-extraction-62304-soup.md`, `module-aggregation-soup.md` |
| **8.2** | Change Control | A,B,C | ✅ Covered | Item | `extraction-62304-change-control.md` |
| **8.3** | Configuration Status Accounting | A,B,C | ✅ Covered | Item | `extraction-62304-change-control.md` |
| **9** | Software Problem Resolution | A,B,C | ✅ Covered | Item | `extraction-62304-change-control.md` |

## Detailed Clause Coverage

### Clause 4: General Requirements

| Sub-clause | Requirement | Coverage | Notes |
|------------|-------------|----------|-------|
| 4.1 | Quality Management System | Manual | QMS documentation outside code |
| 4.2 | Risk Management | Manual | Links to ISO 14971 |
| 4.3 | Software Safety Classification | ✅ | All prompts include safety_class |
| 4.4 | Legacy Software | Manual | Case-by-case assessment |

### Clause 5.1: Software Development Planning

| Sub-clause | Requirement | Class | Coverage | Notes |
|------------|-------------|-------|----------|-------|
| 5.1.1 | Software Development Plan | A,B,C | Manual | Plan document |
| 5.1.2 | Keep Plan Updated | A,B,C | Manual | Plan maintenance |
| 5.1.3 | Reference to System Design | A,B,C | ⚠️ | Requirements prompt links to system |
| 5.1.4 | Standards, Methods, Tools | C | Manual | Plan content |
| 5.1.5 | Integration Planning | B,C | ✅ | Verification prompt |
| 5.1.6 | Verification Planning | A,B,C | ✅ | Verification prompt |
| 5.1.7 | Risk Management Planning | A,B,C | ✅ | Risk prompt |
| 5.1.8 | Documentation Planning | A,B,C | Manual | Plan content |
| 5.1.9 | Configuration Management Planning | A,B,C | ✅ | Change control prompt |
| 5.1.10 | Supporting Items Control | B,C | ✅ | SOUP prompt |
| 5.1.11 | Config Item Control Before Verification | B,C | ✅ | Change control prompt |
| 5.1.12 | Common Software Defects | B,C | ⚠️ | Partial in verification |

### Clause 5.2: Software Requirements Analysis

| Sub-clause | Requirement | Class | Coverage | Prompt |
|------------|-------------|-------|----------|--------|
| 5.2.1 | Define Software Requirements | A,B,C | ✅ | `extraction-62304-requirements.md` |
| 5.2.2 a) | Functional Requirements | A,B,C | ✅ | type: functional |
| 5.2.2 b) | Inputs/Outputs | A,B,C | ✅ | interface extraction |
| 5.2.2 c) | Interfaces | A,B,C | ✅ | type: interface |
| 5.2.2 d) | Alarms/Warnings | A,B,C | ✅ | type: safety |
| 5.2.2 e) | Security | A,B,C | ✅ | type: security |
| 5.2.2 f) | User Interface | A,B,C | ✅ | type: ui |
| 5.2.2 g) | Data/Database | A,B,C | ✅ | type: data |
| 5.2.2 h) | Installation | A,B,C | ✅ | type: operational |
| 5.2.2 i) | Operation/Maintenance | A,B,C | ✅ | type: operational |
| 5.2.2 j) | IT-network | A,B,C | ✅ | type: operational |
| 5.2.2 k) | User Maintenance | A,B,C | ✅ | type: operational |
| 5.2.2 l) | Regulatory | A,B,C | ✅ | type: regulatory |
| 5.2.3 | Risk Control in Requirements | B,C | ✅ | safety_class field |
| 5.2.4 | Re-evaluate Risk Analysis | A,B,C | ⚠️ | Risk prompt links |
| 5.2.5 | Update Requirements | A,B,C | ✅ | Version tracking |
| 5.2.6 | Verify Requirements | A,B,C | ✅ | Verification links |

### Clause 5.3: Software Architectural Design

| Sub-clause | Requirement | Class | Coverage | Prompt |
|------------|-------------|-------|----------|--------|
| 5.3.1 | Transform to Architecture | B,C | ✅ | `extraction-62304-architecture.md` |
| 5.3.2 | Interface Architecture | B,C | ✅ | interfaces array |
| 5.3.3 | SOUP Functional Requirements | B,C | ✅ | soup_components.functional_requirements |
| 5.3.4 | SOUP Hardware Requirements | B,C | ✅ | soup_components.hardware_requirements |
| 5.3.5 | Segregation for Risk Control | C | ✅ | safety_segregation |
| 5.3.6 | Verify Architecture | B,C | ✅ | Architecture verification section |

### Clause 5.4: Software Detailed Design (Class C only)

| Sub-clause | Requirement | Class | Coverage | Prompt |
|------------|-------------|-------|----------|--------|
| 5.4.1 | Subdivide into Units | B,C | ✅ | `extraction-62304-detailed-design.md` |
| 5.4.2 | Detailed Design per Unit | C | ✅ | software_units with algorithm, signature |
| 5.4.3 | Interface Detailed Design | C | ✅ | unit_interfaces |
| 5.4.4 | Verify Detailed Design | C | ✅ | architecture_verification |

### Clause 5.5-5.7: Implementation and Testing

| Sub-clause | Requirement | Class | Coverage | Prompt |
|------------|-------------|-------|----------|--------|
| 5.5.1 | Implement Units | A,B,C | ✅ | Code is the implementation |
| 5.5.2 | Unit Verification Process | B,C | ✅ | `extraction-62304-verification.md` |
| 5.5.3 | Unit Acceptance Criteria | B,C | ✅ | acceptance_criteria_coverage |
| 5.5.4 | Additional Acceptance Criteria | C | ✅ | All 8 criteria types |
| 5.5.5 | Unit Verification | B,C | ✅ | unit_verification |
| 5.6.1-5.6.8 | Integration Testing | B,C | ✅ | integration_verification |
| 5.7.1-5.7.5 | System Testing | A,B,C | ✅ | system_verification |

### Clause 5.8: Software Release

| Sub-clause | Requirement | Class | Coverage | Notes |
|------------|-------------|-------|----------|-------|
| 5.8.1 | Verification Complete | A,B,C | ✅ | Verification prompt |
| 5.8.2 | Document Residual Anomalies | A,B,C | ✅ | anomalies in verification |
| 5.8.3 | Evaluate Residual Anomalies | B,C | ⚠️ | Manual evaluation |
| 5.8.4 | Document Released Versions | A,B,C | ✅ | Change control prompt |
| 5.8.5 | Document Release Process | B,C | ⚠️ | CI/CD extraction partial |
| 5.8.6 | Ensure Activities Complete | B,C | ⚠️ | Manual verification |
| 5.8.7 | Archive Software | A,B,C | ✅ | Git provides this |
| 5.8.8 | Reliable Delivery | A,B,C | ⚠️ | Deployment process manual |

### Clause 7: Software Risk Management

| Sub-clause | Requirement | Class | Coverage | Prompt |
|------------|-------------|-------|----------|--------|
| 7.1.1 | Identify Contributing Items | B,C | ✅ | `extraction-62304-risk.md` |
| 7.1.2 | Identify Potential Causes | B,C | ✅ | software_causes |
| 7.1.3 | Evaluate SOUP Anomalies | B,C | ✅ | soup_anomalies |
| 7.1.4 | Document Potential Causes | B,C | ✅ | Full documentation |
| 7.2.1 | Define Risk Controls | B,C | ✅ | risk_control_measures |
| 7.2.2 | Risk Controls in Software | B,C | ✅ | software_safety_requirements |
| 7.3.1 | Verify Risk Controls | B,C | ✅ | risk_control_verification |
| 7.3.3 | Document Traceability | B,C | ✅ | traceability object |
| 7.4.1-7.4.3 | Change Risk Management | A,B,C | ✅ | change_impact_analysis |

### Clauses 8-9: Configuration and Problem Resolution

| Sub-clause | Requirement | Class | Coverage | Prompt |
|------------|-------------|-------|----------|--------|
| 8.1.1 | Identify Config Items | A,B,C | ✅ | Git provides this |
| 8.1.2 | Identify SOUP | A,B,C | ✅ | `extraction-62304-soup.md` |
| 8.1.3 | System Configuration Doc | A,B,C | ✅ | configuration_status |
| 8.2.1-8.2.4 | Change Control | A,B,C | ✅ | `extraction-62304-change-control.md` |
| 8.3 | Status Accounting | A,B,C | ✅ | configuration_status |
| 9.1-9.8 | Problem Resolution | A,B,C | ✅ | problem_reports |

## Prompt Inventory by Level

### Item-Level Prompts (Repository Scope)

| Category | Prompt | IEC 62304 Coverage | Output |
|----------|--------|-------------------|--------|
| Extraction | `item-extraction-62304-requirements.md` | 5.2 | `item-requirements.json` |
| Extraction | `item-extraction-62304-architecture.md` | 5.3 | `item-architecture.json` |
| Extraction | `item-extraction-62304-detailed-design.md` | 5.4 | `item-detailed-design.json` |
| Extraction | `item-extraction-62304-soup.md` | 8.1 | `item-soup.json` |
| Extraction | `item-extraction-62304-verification.md` | 5.5-5.7 | `item-verification.json` |
| Extraction | `item-extraction-62304-risk-contribution.md` | 7 | `item-risk-contribution.json` |
| Generation | `item-generation-62304-requirements.md` | 5.2 | Item-Requirements.md |
| Generation | `item-generation-62304-architecture.md` | 5.3 | Item-Architecture.md |
| Generation | `item-generation-62304-soup-list.md` | 8.1 | Item-SOUP-List.md |
| Generation | `item-generation-62304-verification.md` | 5.5-5.7 | Item-Verification.md |
| Generation | `item-generation-62304-risk-contribution.md` | 7 | Item-Risk-Contribution.md |

### Module-Level Prompts (Multi-Item Scope)

| Category | Prompt | IEC 62304 Coverage | Output |
|----------|--------|-------------------|--------|
| Aggregation | `module-aggregation-architecture.md` | 5.3, 5.6 | `module-architecture.json` |
| Aggregation | `module-aggregation-soup.md` | 8.1 | `module-soup.json` |
| Aggregation | `module-aggregation-risks.md` | 7 | `module-risk.json` |
| Aggregation | `module-aggregation-verification.md` | 5.6 | `module-verification.json` |

### System-Level Prompts (Product Scope)

| Category | Prompt | IEC 62304 Coverage | Output |
|----------|--------|-------------------|--------|
| Aggregation | `system-aggregation-risk-file.md` | 7 | `product-risk-file.json` |
| Generation | `generation-62304-srs.md` | 5.2 | Software Requirements Specification |
| Generation | `generation-62304-sad.md` | 5.3 | Software Architecture Description |
| Generation | `generation-62304-soup-list.md` | 8.1 | SOUP List |
| Generation | `generation-62304-verification-report.md` | 5.5-5.7 | Software Verification Report |
| Generation | `generation-62304-release-notes.md` | 5.8 | Software Release Notes |

## Remaining Gaps (Manual Process Required)

| Gap | Clause | Priority | Recommendation |
|-----|--------|----------|----------------|
| Maintenance Process | 6 | Medium | Create `extraction-62304-maintenance.md` |
| Legacy Software Assessment | 4.4 | Low | Manual case-by-case |
| Development Plan | 5.1 | Low | Manual document |

## Usage

### Single-Item Product

1. Run item-level extraction prompts against repository
2. Review extracted JSON for completeness
3. Run item-level generation prompts for item documentation
4. Run system-level generation prompts (item data serves as system data)

### Multi-Item Product

1. Run item-level extraction prompts against **each** repository
2. Run module-level aggregation prompts for each module
3. Run system-level aggregation prompts
4. Run system-level generation prompts for regulatory documents
5. Review generated documents for accuracy
6. Submit for QMS approval
