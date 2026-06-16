---
id: 725af57
title: "item generation 62304 risk contribution"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Prompt"
category: "generation"
level: "item"
standard: "IEC 62304"
clause: "7"
inputs: ["item-risk-contribution.json"]
outputs: ["Item-Risk-Contribution.md"]
software_class: "B,C"
process: "[Risk Management Process](../../../Canvases/Risk%20Management%20Process.canvas)"
requirements: "[IEC 62304](../../../Requirements/IEC_62304_Requirements.md)"
owner: "[Head of Risk Management](../../../Assets/Head%20of%20Risk%20Management.md)"
---

# Generate Item Risk Contribution Document

## Context

This prompt generates a human-readable document describing how a software item contributes to the overall system risk profile. It is explicitly **NOT** a Risk Management Report (RMR) — that document is generated at the system level.

**Purpose of this document:**
- Document hazards this software item can cause
- Document risk controls implemented in this codebase
- Identify controls that must be implemented at module/system level
- Support system-level risk aggregation
- Provide traceability from system risks to source code

## Inputs

### Required
- **`item-risk-contribution.json`** — Output from item-extraction-62304-risk-contribution

### Optional
- **`item-requirements.json`** — For requirement-to-risk traceability
- **`item-verification.json`** — For control verification evidence

## Instructions

Generate a markdown document following the output structure below. The document should:
1. Clearly state this is an item-level document, not a system RMR
2. Summarize hazard contributions
3. Detail risk controls with verification status
4. Identify upstream controls needed
5. Include full traceability to source code

## Output Document Structure

```markdown
---
id: 725af57
title: "item generation 62304 risk contribution"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "RiskContribution"
document_id: "ITEM-RISK-[item-id]-[version]"
level: "item"
process: "[Risk Management Process](../../Canvases/Risk%20Management%20Process.canvas)"
requirements: "[IEC 62304](../../Requirements/IEC_62304_Requirements.md)"
owner: "[Head of Risk Management](../../Assets/Head%20of%20Risk%20Management.md)"
---

# Item Risk Contribution

## [Item Name]

**Software Item:** [item_id]
**Repository:** [repository URL]
**Version/Commit:** [commit hash]
**Safety Classification:** [Class A/B/C]
**Extraction Date:** [date]

---

## Important Notice

> ⚠️ **This is an ITEM-LEVEL document.**
>
> This document describes how **[Item Name]** contributes to the system risk profile.
> It is NOT a Risk Management Report (RMR).
>
> For complete risk management documentation, see the **System-Level** documents:
> - Risk Management Plan: `SYS-RMP-[system]-[version].md`
> - Risk Analysis: `SYS-RA-[system]-[version].md`
> - Risk Management Report: `SYS-RMR-[system]-[version].md`

---

## 1. Executive Summary

### 1.1 Risk Contribution Summary

| Metric | Count |
|--------|-------|
| Hazard Contributions | [n] |
| Risk Controls Implemented | [n] |
| Controls Requiring Upstream Implementation | [n] |
| Failure Modes Documented | [n] |
| Interfaces with Safety Impact | [n] |
| Verification Gaps | [n] |

### 1.2 Safety Classification Rationale

**Classification:** [Class A/B/C]

**Rationale:** [From item_risk_contribution.safety_class_rationale]

---

## 2. Hazard Contributions

This section documents hazards that **[Item Name]** can cause or contribute to.

### 2.1 Summary

| ID | Description | Severity Contribution | Controls |
|----|-------------|-----------------------|----------|
| [ITEM-XX-HAZ-001] | [description] | [severity] | [control IDs] |
| ... | ... | ... | ... |

### 2.2 Detailed Hazard Analysis

#### [ITEM-XX-HAZ-001]: [Hazard Title]

**Description:** [From hazard_contributions[].description]

**Cause:** [From cause]

**Potential Harm:** [From potential_harm]

**Severity Contribution:** [From severity_contribution]

**Affected Functions:** [From affected_functions]

**Source:**
- [file:line-range]

**Related Controls:**
- [ITEM-XX-RC-001]: [control description]

---

[Repeat for each hazard]

---

## 3. Risk Controls Implemented

This section documents risk controls implemented **within this software item**.

### 3.1 Summary by Control Type

| Type | Count | Verified | Partial | Unverified |
|------|-------|----------|---------|------------|
| Inherent Safety | [n] | [n] | [n] | [n] |
| Protective Measure | [n] | [n] | [n] | [n] |
| Information for Safety | [n] | [n] | [n] | [n] |

### 3.2 Detailed Controls

#### [ITEM-XX-RC-001]: [Control Title]

**Type:** [inherent_safety / protective_measure / information_for_safety]

**Description:** [From description]

**Mitigates:**
- [ITEM-XX-HAZ-001]: [hazard description]

**Implementation:** [From implementation]

**Source:**
- [file:line-range]

**Verification:**
- **Status:** [verified / partial / unverified]
- **Evidence:** [test_file:test_function] or "No test coverage"

---

[Repeat for each control]

---

## 4. Controls Requiring Upstream Implementation

These risk controls **cannot be fully implemented at the item level** and must be addressed at module or system level.

### 4.1 Summary

| ID | Description | Recommended Level | Control Type |
|----|-------------|-------------------|--------------|
| [ITEM-XX-RC-UP-001] | [description] | [module/system] | [type] |
| ... | ... | ... | ... |

### 4.2 Detailed Upstream Controls

#### [ITEM-XX-RC-UP-001]: [Control Title]

**Description:** [From description]

**Rationale (why item cannot implement):** [From rationale]

**Recommended Level:** [module / system]

**Control Type:** [inherent_safety / protective_measure / information_for_safety]

**Mitigates:**
- [ITEM-XX-HAZ-001]: [hazard description]

**Suggested Implementation:**
- [Specific recommendation for module/system level]

---

[Repeat for each upstream control]

---

## 5. Failure Modes

### 5.1 Summary

| ID | Component | Failure Mode | Effect | Detection |
|----|-----------|--------------|--------|-----------|
| [ITEM-XX-FM-001] | [component] | [failure mode] | [effect] | [detection] |
| ... | ... | ... | ... | ... |

### 5.2 Detailed Failure Mode Analysis

#### [ITEM-XX-FM-001]: [Component] - [Failure Mode]

**Component:** [From component]

**Failure Mode:** [From failure_mode]

**Effect:** [From effect]

**Detection:** [From detection]

**Recovery:** [From recovery]

**Related Hazards:**
- [ITEM-XX-HAZ-001]

**Source:**
- [file:line-range]

---

[Repeat for each failure mode]

---

## 6. Interfaces with Safety Impact

### 6.1 Summary

| ID | Interface | Direction | Safety Impact | Failure Handling |
|----|-----------|-----------|---------------|------------------|
| [ITEM-XX-IF-001] | [interface] | [direction] | [impact] | [handling] |
| ... | ... | ... | ... | ... |

### 6.2 Detailed Interface Analysis

#### [ITEM-XX-IF-001]: [Interface Name]

**Type:** [REST / JMS / database / etc]

**Direction:** [inbound / outbound / bidirectional]

**Safety Impact:** [From safety_impact]

**Failure Handling:** [From failure_handling]

**Timeout:** [From timeout] or "Not configured"

**Retry Policy:** [From retry_policy] or "None"

**Source:**
- [file:line-range]

---

[Repeat for each interface]

---

## 7. Verification Gaps

### 7.1 Summary

| Priority | Count |
|----------|-------|
| High | [n] |
| Medium | [n] |
| Low | [n] |

### 7.2 Gap Details

| ID | Type | Description | Affected Items | Priority | Recommendation |
|----|------|-------------|----------------|----------|----------------|
| [ITEM-XX-GAP-001] | [type] | [description] | [IDs] | [priority] | [recommendation] |
| ... | ... | ... | ... | ... | ... |

---

## 8. Aggregation Notes

Information to assist module/system-level aggregation:

### 8.1 Cross-Item Interfaces

These interfaces likely connect to other software items:

- [interface names]

### 8.2 Shared Hazards

These hazards likely exist in other items too:

- [hazard descriptions]

### 8.3 System-Level Controls Needed

These controls must be coordinated at system level:

- [control descriptions]

---

## 9. Traceability

### 9.1 Hazard to Source Code

| Hazard ID | Source Files |
|-----------|--------------|
| [ITEM-XX-HAZ-001] | [files] |
| ... | ... |

### 9.2 Control to Source Code

| Control ID | Source Files |
|------------|--------------|
| [ITEM-XX-RC-001] | [files] |
| ... | ... |

### 9.3 Control to Verification

| Control ID | Verification Status | Test Evidence |
|------------|---------------------|---------------|
| [ITEM-XX-RC-001] | [status] | [test file] |
| ... | ... | ... |

---

## Appendix A: Extraction Metadata

| Attribute | Value |
|-----------|-------|
| Repository | [repository] |
| Commit | [commit] |
| Extraction Date | [extracted_at] |
| Extractor Version | [extractor_version] |
| Standard | [standard] |

## Appendix B: Document History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | [date] | [author] | Initial extraction |

---

*This document is part of the regulatory documentation for [Item Name].*
*IEC 62304:2006+AMD1:2015 Clause 7 Compliant — Item Level*
```

## Compliance Checklist

Before finalizing the document:

- [ ] Document clearly states it is item-level, not system RMR 🆔 Bud0Rc
- [ ] All hazard contributions are documented 🆔 uRXnHl
- [ ] Each hazard has source file references 🆔 J2XTKN
- [ ] All implemented controls have verification status 🆔 A14FTQ
- [ ] Upstream controls clearly identify recommended level 🆔 d9wnif
- [ ] Failure modes cover critical components 🆔 jfsUT0
- [ ] Interfaces with safety impact are documented 🆔 PtAmZt
- [ ] Verification gaps are prioritized 🆔 KL8Qre
- [ ] Aggregation notes assist system-level work 🆔 9sDKAq
- [ ] Full traceability to source code 🆔 72eq24

## Validation Criteria

- [ ] Document follows the prescribed structure 🆔 NJrUXF
- [ ] All sections populated from extracted JSON 🆔 yMzKhw
- [ ] Missing data flagged with [TODO] markers 🆔 7luhTL
- [ ] Important notice about document scope is prominent 🆔 FZKo5K
- [ ] References to system-level documents are included 🆔 q4OSqW
- [ ] Document is readable by risk management personnel 🆔 AsrU1V
