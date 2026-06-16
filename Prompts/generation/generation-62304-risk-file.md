---
id:
title: "Generate Software Risk Management File"
version:
author:
effective_date:
type: "Prompt"
category: "generation"
standard: "IEC 62304"
clause: "7"
inputs: ["software-risk.json (from extraction)", "architecture.json", "soup-list.json"]
outputs: ["Software Risk Management File document"]
software_class: "B, C"
process: "[Risk Management Process](../../Canvases/Risk%20Management%20Process.canvas)"
requirements:
owner: "[Head of Risk Management](../../Assets/Head%20of%20Risk%20Management.md)"
---

# Generate Software Risk Management File

## Context

The Software Risk Management File documents software-specific risk management activities per IEC 62304 clause 7, integrated with ISO 14971. This prompt generates a compliant risk file from extracted risk data.

## Inputs

### Required
- `software-risk.json` — Output from [extraction-62304-risk](../extraction/extraction-62304-risk.md)

### Optional (enriches output)
- `architecture.json` — Software item context
- `soup-list.json` — SOUP anomaly details
- `verification.json` — Risk control verification evidence

## Instructions

1. **Validate Input Data**
   - Verify hazard analysis is complete
   - Check all causes have controls
   - Confirm verification linkages

2. **Structure Risk Documentation**
   - Hazardous situations from software
   - Causal analysis
   - Risk control measures
   - Verification evidence
   - Change impact

3. **Generate Traceability**
   - Hazard → Cause → Control → Requirement → Test

4. **Assess Completeness**
   - All software items analyzed
   - SOUP anomalies evaluated
   - Controls verified

## Output Schema

```markdown
---
id:
title: "Software Risk Management File - [Product Name]"
version:
author:
effective_date:
type: "Report"
document_id: "SRMF-[product]-[version]"
software_safety_class: "B | C"
process: "[Risk Management Process](../Canvases/Risk%20Management%20Process.canvas)"
requirements:
owner: "[Head of Risk Management](../Assets/Head%20of%20Risk%20Management.md)"
---

# Software Risk Management File

## 1. Introduction

### 1.1 Purpose
This document records the software risk management activities for [Product Name] per IEC 62304:2006+A1:2015 clause 7, integrated with ISO 14971:2019.

### 1.2 Scope
This file covers risk management for software version [version], safety class [B/C].

### 1.3 Software Safety Classification

**Overall Classification:** Class [B/C]

**Classification Basis:**
[Rationale for software safety classification per 4.3]

### 1.4 Referenced Documents

| Document | Version | Relationship |
|----------|---------|--------------|
| Product Risk Management File | [ver] | Parent risk file (ISO 14971) |
| Software Requirements Specification | [ver] | Safety requirements |
| Software Architecture Description | [ver] | Software item definitions |
| SOUP List | [ver] | Third-party component risks |
| Software Verification Report | [ver] | Risk control verification |

### 1.5 Definitions

| Term | Definition |
|------|------------|
| HAZARD | Potential source of harm |
| HAZARDOUS SITUATION | Circumstance in which people are exposed to a hazard |
| HARM | Physical injury or damage to health |
| RISK | Combination of probability and severity of harm |
| RISK CONTROL | Action taken to reduce risk |

## 2. Risk Management Process

### 2.1 Process Reference
Software risk management follows [Risk Management Procedure], integrated with the product-level ISO 14971 process.

### 2.2 Software-Specific Activities
Per IEC 62304 clause 7:
- 7.1: Analysis of software contributing to hazardous situations
- 7.2: Risk control measures
- 7.3: Verification of risk control measures
- 7.4: Risk management of software changes

### 2.3 Roles and Responsibilities

| Role | Responsibilities |
|------|------------------|
| Risk Management Lead | Overall risk file ownership |
| Software Architect | Hazard identification, control design |
| Software Developer | Control implementation |
| Quality Assurance | Control verification |

## 3. Hazardous Situations Analysis (7.1)

### 3.1 Summary

| Metric | Value |
|--------|-------|
| Hazardous situations identified | [n] |
| Software items contributing | [n] |
| Potential causes identified | [n] |
| SOUP anomalies evaluated | [n] |

### 3.2 Software Items Contributing to Hazardous Situations (7.1.1)

| Item ID | Name | Safety Class | Hazards Contributed |
|---------|------|--------------|---------------------|
| SI-xxx | [name] | [class] | HAZ-SW-001, HAZ-SW-002 |
| ... | ... | ... | ... |

### 3.3 Hazardous Situation Details

#### HAZ-SW-001: [Hazardous Situation Title]

| Property | Value |
|----------|-------|
| **Hazard ID** | HAZ-SW-001 |
| **Description** | [Description of the hazardous situation] |
| **Potential Harm** | [What harm could result] |
| **Severity** | [Negligible / Minor / Serious / Critical / Catastrophic] |
| **Contributing Software** | SI-xxx, SI-yyy |
| **Parent Hazard (ISO 14971)** | [Reference to product-level hazard] |

**Code Evidence:**
```
[Relevant code locations: file:line]
```

---

[Repeat for each hazardous situation]

### 3.4 Potential Causes Analysis (7.1.2)

#### CAUSE-001: [Cause Title]

| Property | Value |
|----------|-------|
| **Cause ID** | CAUSE-001 |
| **Related Hazard** | HAZ-SW-001 |
| **Software Item** | SI-xxx |
| **Cause Type** | [Specification / Defect / SOUP Failure / HW Interaction / Misuse] |

**Description:**
[How this cause could lead to the hazardous situation]

**Code Evidence:**
```
[Relevant code patterns or locations]
```

---

[Repeat for each cause]

### 3.5 SOUP Anomaly Evaluation (7.1.3)

| SOUP | Version | Published Anomalies | Safety Relevance | Assessment |
|------|---------|---------------------|------------------|------------|
| [pkg] | [ver] | [anomaly list ref] | [relevant/not] | [assessment] |

#### SOUP Anomaly Details

**[SOUP Name] - [Anomaly Reference]**

| Property | Value |
|----------|-------|
| SOUP | [package name] |
| Anomaly | [anomaly description] |
| Could Contribute To | [hazard reference] |
| Likelihood | [assessment] |
| Mitigation | [how addressed] |

## 4. Risk Control Measures (7.2)

### 4.1 Summary

| Control Type | Count | Verified |
|--------------|-------|----------|
| Input Validation | [n] | [n] |
| Error Handling | [n] | [n] |
| Alarms/Warnings | [n] | [n] |
| Access Control | [n] | [n] |
| Data Protection | [n] | [n] |
| Redundancy | [n] | [n] |
| Segregation | [n] | [n] |

### 4.2 Risk Control Details

#### RC-SW-001: [Risk Control Title]

| Property | Value |
|----------|-------|
| **Control ID** | RC-SW-001 |
| **Related Cause** | CAUSE-001 |
| **Related Hazard** | HAZ-SW-001 |
| **Control Type** | [Type] |
| **Effectiveness** | [Prevents / Reduces Probability / Reduces Severity / Detects] |

**Description:**
[What the risk control does]

**Implementation:**

| Property | Value |
|----------|-------|
| Software Item | SI-xxx |
| Code Location | `[file:line]` |
| Safety Class | [B/C] |

**Implementation Details:**
```
[Code snippet or description of implementation]
```

**Residual Risk:**
[Description of any remaining risk after this control]

**New Hazards Introduced:**
[Yes/No - if yes, describe and reference new hazard analysis]

---

[Repeat for each risk control]

### 4.3 Software Safety Requirements (7.2.2)

Risk controls implemented in software are captured as safety requirements:

| Requirement ID | Description | Implements Control | Safety Class |
|----------------|-------------|-------------------|--------------|
| SRS-SAF-001 | [requirement text] | RC-SW-001 | [B/C] |
| SRS-SAF-002 | [requirement text] | RC-SW-002 | [B/C] |

## 5. Risk Control Verification (7.3)

### 5.1 Verification Summary

| Controls Defined | Verified | Pending | Not Verified |
|------------------|----------|---------|--------------|
| [n] | [n] | [n] | [n] |

### 5.2 Verification Details

#### RC-SW-001 Verification

| Property | Value |
|----------|-------|
| Control | RC-SW-001 |
| Verification Method | [Test / Analysis / Inspection] |
| Status | [Verified / Pending / Failed] |

**Verification Evidence:**

| Test/Analysis | Reference | Result |
|---------------|-----------|--------|
| [test name] | ST-SAF-001 | ✅ Pass |
| [analysis] | [document ref] | Complete |

**New Hazard Check:**
- Reviewed for introduction of new hazards: ✅ No new hazards identified

---

[Repeat for each control]

### 5.3 Unverified Controls

| Control | Reason | Plan |
|---------|--------|------|
| [RC-id] | [why not verified] | [plan to verify] |

## 6. Traceability (7.3.3)

### 6.1 Complete Traceability Chain

```
Hazardous Situation → Software Cause → Risk Control → Safety Requirement → Verification
```

| Hazard | Cause | Control | Requirement | Test | Status |
|--------|-------|---------|-------------|------|--------|
| HAZ-SW-001 | CAUSE-001 | RC-SW-001 | SRS-SAF-001 | ST-SAF-001 | ✅ |
| HAZ-SW-001 | CAUSE-002 | RC-SW-002 | SRS-SAF-002 | ST-SAF-002 | ✅ |
| HAZ-SW-002 | CAUSE-003 | RC-SW-003 | SRS-SAF-003 | ST-SAF-003 | ✅ |

### 6.2 Traceability Gaps

| Gap | Description | Action |
|-----|-------------|--------|
| [gap-type] | [what's missing] | [how to address] |

## 7. Risk Management of Software Changes (7.4)

### 7.1 Change Analysis Summary

| Period | Changes Analyzed | Safety-Relevant | Additional Controls Required |
|--------|------------------|-----------------|------------------------------|
| [date range] | [n] | [n] | [n] |

### 7.2 Recent Change Analysis

#### Change: [PR/Commit Reference]

| Property | Value |
|----------|-------|
| Change Reference | [PR-xxx / commit-hash] |
| Description | [what changed] |
| Date | [date] |
| Affected Items | SI-xxx, SI-yyy |

**Safety Analysis (7.4.1):**
- Additional causes introduced: [Yes/No]
- New causes identified: [list or N/A]

**Impact on Existing Controls (7.4.2):**
- Existing controls affected: [Yes/No]
- Controls requiring re-verification: [list or N/A]

**Additional Risk Activities (7.4.3):**
[Description of any additional risk management performed]

---

[Repeat for significant changes]

## 8. Residual Risk Assessment

### 8.1 Residual Risk Summary

| Hazard | Initial Risk | After Controls | Acceptable |
|--------|--------------|----------------|------------|
| HAZ-SW-001 | [level] | [level] | ✅ |
| HAZ-SW-002 | [level] | [level] | ✅ |

### 8.2 Benefit-Risk Analysis Reference
[Reference to product-level benefit-risk analysis per ISO 14971]

## 9. Conclusion

### 9.1 Risk Management Statement

Based on the software risk management activities documented in this file:

- [ ] All software items contributing to hazardous situations have been identified 🆔 Mj85M9
- [ ] Potential causes have been analyzed including SOUP anomalies 🆔 U3UMMN
- [ ] Risk control measures have been defined for all identified causes 🆔 NNamBx
- [ ] Risk controls have been implemented and verified 🆔 YxeZGf
- [ ] Residual risks are acceptable per the risk acceptability criteria 🆔 y7Dtwg
- [ ] Change impacts have been assessed 🆔 TwjLWa

### 9.2 Open Items

| Item | Description | Owner | Due |
|------|-------------|-------|-----|
| [item] | [description] | [owner] | [date] |

### 9.3 Approvals

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Risk Management Lead | | | |
| Software Lead | | | |
| Quality Assurance | | | |

## 10. Appendices

### Appendix A: Software Item Safety Classification

[Complete list of software items with safety class assignments]

### Appendix B: SOUP Anomaly Lists Reviewed

[References to vendor anomaly lists evaluated]

### Appendix C: Extraction Metadata

- Repository: [repo]
- Commit: [hash]
- Extracted: [timestamp]
```

## Compliance Mapping

| Document Section | IEC 62304 Clause | Evidence Provided |
|------------------|------------------|-------------------|
| Section 3.2 | 7.1.1 | Software items contributing to hazards |
| Section 3.4 | 7.1.2 | Potential causes identification |
| Section 3.5 | 7.1.3 | SOUP anomaly evaluation |
| Section 3 | 7.1.4 | Documentation of causes |
| Section 4 | 7.2.1 | Risk control measures |
| Section 4.3 | 7.2.2 | Controls in software requirements |
| Section 5 | 7.3.1 | Verification of controls |
| Section 6 | 7.3.3 | Traceability documentation |
| Section 7 | 7.4.1-7.4.3 | Change risk management |

## Validation Criteria

- [ ] All Class B/C software items have hazard analysis 🆔 3QIniW
- [ ] Each hazard has identified causes 🆔 9d0oeW
- [ ] SOUP anomalies are evaluated for safety relevance 🆔 T1OY0n
- [ ] Each cause has risk control measures defined 🆔 RbWSwT
- [ ] Controls are linked to software requirements 🆔 neNvsq
- [ ] All controls have verification evidence 🆔 1V4FZr
- [ ] Traceability chain is complete 🆔 jkKpz3
- [ ] Recent changes are analyzed for safety impact 🆔 LWYb9t
- [ ] Residual risk assessment is complete 🆔 X7zD1d
- [ ] Approval section is present 🆔 V1tmZW
