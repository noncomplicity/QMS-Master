---
id: 725af57
title: "generation 14971 rma"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Prompt"
category: "generation"
standard: "ISO 14971"
clause: "5, 6, 7"
inputs: ["risks.json (from extraction)", "Risk Management Plan", "software requirements"]
outputs: ["Risk Analysis document with hazards, situations, controls, and residual risks"]
software_class: "all"
process: "[Risk Management Process](../../Canvases/Risk%20Management%20Process.canvas)"
requirements: "[ISO 14971 Risk management for medical devices](../../Requirements/ISO_14971_Requirements.md)"
owner: "[Head of Risk Management](../../Assets/Head%20of%20Risk%20Management.md)"
---

# Generate Risk Analysis Document

## Context

The Risk Analysis document (clauses 5-7) is the core deliverable of ISO 14971, documenting hazard identification, risk estimation, risk evaluation, and risk control. This prompt transforms extracted risk data into a compliant Risk Analysis document that demonstrates systematic hazard identification and control.

## Inputs

### Required
- `risks.json` — Output from [extraction-14971-risks](../extraction/extraction-14971-risks.md)
- Risk Management Plan (for acceptability criteria)

### Optional (enriches output)
- Software Requirements Specification (for traceability)
- System architecture documentation
- Previous risk analyses (for product revisions)
- Clinical literature on hazards

## Instructions

1. **Validate Input Data**
   - Verify risks.json contains hazards, situations, and controls
   - Check for complete traceability chain
   - Identify gaps in risk coverage

2. **Organize Hazards**
   - Group by hazard category
   - Ensure unique identifiers
   - Link to source code evidence

3. **Document Risk Estimation**
   - Apply severity and probability from extraction
   - Document rationale for each estimate
   - Flag where probability cannot be estimated

4. **Document Risk Evaluation**
   - Apply acceptability criteria from RMP
   - Identify risks requiring control
   - Document ALARP decisions

5. **Document Risk Controls**
   - Group by control type (inherent, protective, information)
   - Link to implementation evidence
   - Include verification status

6. **Document Residual Risks**
   - Show post-control risk levels
   - Document acceptability decisions
   - Identify disclosure requirements

7. **Generate Traceability**
   - Hazard → Situation → Harm chain
   - Situation → Control → Verification chain
   - Control → Requirement linkage

## Output Schema

Generate a markdown document with this structure:

```markdown
---
id: 725af57
title: "generation 14971 rma"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Report"
document_id: "RA-[product]-[version]"
process: "[Risk Management Process](../../Canvases/Risk%20Management%20Process.canvas)"
requirements: "[ISO 14971](../../Requirements/ISO_14971_Requirements.md)"
owner: "[Head of Risk Management](../../Assets/Head%20of%20Risk%20Management.md)"
---

# Risk Analysis

## 1. Document Information

### 1.1 Purpose
This document presents the systematic risk analysis for [Product Name], identifying hazards, estimating and evaluating risks, and documenting risk control measures per ISO 14971:2019.

### 1.2 Scope
| Attribute | Value |
|-----------|-------|
| Product | [Product Name] |
| Version | [Version] |
| Analysis Date | [Date] |
| Analyst(s) | [Names] |

### 1.3 Referenced Documents
| Document | Version | Purpose |
|----------|---------|---------|
| Risk Management Plan | [ver] | Acceptability criteria |
| Software Requirements Specification | [ver] | Requirement traceability |
| System Architecture | [ver] | Component identification |

### 1.4 Risk Acceptability Criteria
[Reference or embed the acceptability matrix from RMP]

## 2. Intended Use and Reasonably Foreseeable Misuse

### 2.1 Intended Use
[From extraction - medical function, users, environment, operating principle]

| Element | Description |
|---------|-------------|
| Medical Function | [description] |
| Intended Users | [user types] |
| Use Environment | [settings] |
| Patient Population | [if applicable] |
| Operating Principle | [how it works] |

### 2.2 Reasonably Foreseeable Misuse
| Misuse Scenario | Likelihood | Addressed In |
|-----------------|------------|--------------|
| [misuse description] | [likelihood] | [hazard ref] |
| ... | ... | ... |

## 3. Characteristics Related to Safety

| ID | Characteristic | Limits/Criteria | Source |
|----|----------------|-----------------|--------|
| SC-001 | [characteristic] | [limits] | [code ref] |
| SC-002 | [characteristic] | [limits] | [code ref] |
| ... | ... | ... | ... |

## 4. Hazard Identification

### 4.1 Hazard Summary

| Hazard ID | Hazard Description | Category | Related Situations |
|-----------|-------------------|----------|-------------------|
| HAZ-001 | [hazard] | [category] | HS-001, HS-002 |
| HAZ-002 | [hazard] | [category] | HS-003 |
| ... | ... | ... | ... |

### 4.2 Detailed Hazard Analysis

#### HAZ-001: [Hazard Title]

**Description:** [Detailed hazard description]

**Category:** [clinical_data | calculation | timing | availability | security | user_interface | integration]

**Source Evidence:**
- Code: `[file:line-range]`
- Commit: `[hash]`
- PR: `[#number]`

**Related Safety Characteristics:** SC-001, SC-002

**Foreseeable Causes:**
1. [Cause 1]
2. [Cause 2]

---

[Repeat for each hazard]

## 5. Hazardous Situations and Risk Estimation

### 5.1 Risk Estimation Summary

| Situation ID | Hazard | Potential Harm | Severity | Probability | Initial Risk |
|--------------|--------|----------------|----------|-------------|--------------|
| HS-001 | HAZ-001 | [harm] | S3 | P3 | ALARP |
| HS-002 | HAZ-001 | [harm] | S4 | P2 | ALARP |
| HS-003 | HAZ-002 | [harm] | S2 | P4 | ALARP |
| ... | ... | ... | ... | ... | ... |

### 5.2 Detailed Situation Analysis

#### HS-001: [Situation Title]

**Related Hazard:** HAZ-001 - [Hazard name]

**Hazardous Situation:** [Description of circumstance where people/property exposed to hazard]

**Foreseeable Sequence of Events:**
1. [Initial event or condition]
2. [Subsequent event]
3. [Event leading to exposure]
4. [Harm occurrence]

**Potential Harm:** [Description of injury/damage]

**Risk Estimation:**

| Factor | Level | Rationale |
|--------|-------|-----------|
| Severity | S3 (Serious) | [Evidence-based rationale] |
| Probability | P3 (Remote) | [Evidence-based rationale] |
| Initial Risk | ALARP | Per acceptability matrix |

**Source Evidence:**
- Code: `[file:line-range]`

---

[Repeat for each hazardous situation]

## 6. Risk Evaluation

### 6.1 Risk Evaluation Summary

| Situation ID | Initial Risk | Evaluation | Action Required |
|--------------|--------------|------------|-----------------|
| HS-001 | ALARP | Reduce to acceptable | Yes - implement RC-001 |
| HS-002 | ALARP | Reduce if practicable | Yes - implement RC-002 |
| HS-003 | Acceptable | No further action | No |
| ... | ... | ... | ... |

### 6.2 Risks Requiring Control

The following hazardous situations require risk control measures:

| Situation ID | Initial Risk | Required Control Type |
|--------------|--------------|----------------------|
| HS-001 | ALARP | Protective measure |
| HS-002 | ALARP | Inherent safety or protective |
| ... | ... | ... |

## 7. Risk Control

### 7.1 Risk Control Summary

| Control ID | Situation | Control Type | Description | Status |
|------------|-----------|--------------|-------------|--------|
| RC-001 | HS-001 | Protective | [brief description] | Implemented |
| RC-002 | HS-002 | Inherent | [brief description] | Implemented |
| RC-003 | HS-002 | Information | [brief description] | Implemented |
| ... | ... | ... | ... | ... |

### 7.2 Detailed Risk Control Analysis

#### RC-001: [Control Title]

**Controlled Situation:** HS-001 - [Situation name]

**Control Type:** Protective Measure (ISO 14971 clause 7.1 b)

**Control Description:** [What the control does to reduce risk]

**Implementation:**
| Attribute | Value |
|-----------|-------|
| Mechanism | [validation | error_handling | interlock | warning | etc.] |
| Code Location | `[file:line-range]` |
| Requirement Reference | [SRS-xxx] |

**Verification:**
| Verification Method | Evidence | Status |
|--------------------|----------|--------|
| Code Review | [review record] | ✓ Complete |
| Unit Test | `[test file:test name]` | ✓ Pass |
| Integration Test | `[test file:test name]` | ✓ Pass |

**Effectiveness:** [eliminates | reduces | informs]

**New Hazards Introduced:** [None | List of new hazards with references]

---

[Repeat for each risk control]

### 7.3 Risk Control Option Analysis

For situations where multiple control options were considered:

#### HS-002 Control Options Analysis

| Option | Control Type | Effectiveness | Practicability | Selected |
|--------|--------------|---------------|----------------|----------|
| Option A | Inherent safety | Eliminates | High | Yes |
| Option B | Protective | Reduces | High | Yes (additional) |
| Option C | Information only | Informs | High | No - insufficient |

**Rationale:** [Why the selected options were chosen]

## 8. Residual Risk Evaluation

### 8.1 Residual Risk Summary

| Situation ID | Initial Risk | Controls Applied | Residual Risk | Acceptable? |
|--------------|--------------|------------------|---------------|-------------|
| HS-001 | ALARP (S3/P3) | RC-001 | Acceptable (S3/P1) | Yes |
| HS-002 | ALARP (S4/P2) | RC-002, RC-003 | ALARP (S4/P1) | Yes (B-R) |
| HS-003 | Acceptable | None required | Acceptable | Yes |
| ... | ... | ... | ... | ... |

### 8.2 Detailed Residual Risk Analysis

#### RR-001: Residual Risk for HS-001

**Original Situation:** HS-001 - [Situation name]

**Controls Implemented:** RC-001

**Residual Risk Estimation:**

| Factor | Before Control | After Control | Rationale |
|--------|---------------|---------------|-----------|
| Severity | S3 | S3 | Severity unchanged (consequence same if failure occurs) |
| Probability | P3 | P1 | Validation reduces probability of invalid data reaching system |
| Risk Level | ALARP | Acceptable | Probability reduction moves risk to acceptable region |

**Acceptability Decision:** Acceptable - no further action required

---

#### RR-002: Residual Risk for HS-002

**Original Situation:** HS-002 - [Situation name]

**Controls Implemented:** RC-002, RC-003

**Residual Risk Estimation:**

| Factor | Before Control | After Control | Rationale |
|--------|---------------|---------------|-----------|
| Severity | S4 | S4 | Severity unchanged |
| Probability | P2 | P1 | Multiple controls reduce probability to improbable |
| Risk Level | ALARP | ALARP | Still in ALARP region, but reduced |

**Benefit-Risk Analysis Required:** Yes

**Benefit-Risk Analysis:**
- **Benefits:** [List of clinical benefits of the device function]
- **Residual Risk:** [Description of remaining risk]
- **Conclusion:** Benefits outweigh residual risk because [rationale]

**Acceptability Decision:** Acceptable (with benefit-risk justification)

**Disclosure Required:** Yes

**Disclosure Text:** [Text for user documentation / IFU]

---

[Repeat for each residual risk]

### 8.3 Residual Risks Requiring Disclosure

| Situation | Residual Risk | Disclosure Location |
|-----------|---------------|---------------------|
| HS-002 | [brief description] | Instructions for Use, Section X |
| ... | ... | ... |

## 9. Completeness of Risk Control

### 9.1 Risk Control Completeness Check

| Check Item | Status | Evidence |
|------------|--------|----------|
| All identified hazardous situations evaluated | ✓ | Sections 4-5 |
| All unacceptable/ALARP risks have controls | ✓ | Section 7 |
| All controls verified | ✓ | Section 7.2 verification tables |
| All residual risks evaluated | ✓ | Section 8 |
| New hazards from controls analyzed | ✓ | Section 7.2 "New Hazards" |
| Disclosures identified | ✓ | Section 8.3 |

### 9.2 Risk Coverage Matrix

| Hazard | Situations | Controls | Verification | Residual Accepted |
|--------|------------|----------|--------------|-------------------|
| HAZ-001 | HS-001, HS-002 | RC-001, RC-002, RC-003 | TC-001, TC-002 | ✓ |
| HAZ-002 | HS-003 | (Acceptable) | N/A | ✓ |
| ... | ... | ... | ... | ... |

## 10. Traceability

### 10.1 Hazard to Control Traceability

```
HAZ-001 (Invalid data)
├── HS-001 (Missing data displayed)
│   └── RC-001 (Null validation) → RR-001 (Acceptable)
└── HS-002 (Incorrect data used clinically)
    ├── RC-002 (Range validation) → RR-002 (ALARP)
    └── RC-003 (Warning display)

HAZ-002 (System unavailability)
└── HS-003 (Monitoring gap)
    └── (Acceptable - no control required)
```

### 10.2 Control to Requirement Traceability

| Control ID | Requirement ID | Test ID |
|------------|----------------|---------|
| RC-001 | SRS-VAL-001 | TC-VAL-001 |
| RC-002 | SRS-VAL-002 | TC-VAL-002 |
| RC-003 | SRS-UI-015 | TC-UI-015 |
| ... | ... | ... |

## 11. Gaps and Action Items

| Gap ID | Type | Description | Recommended Action | Status |
|--------|------|-------------|-------------------|--------|
| GAP-001 | Untested control | RC-004 lacks integration test | Add integration test | Open |
| GAP-002 | Undocumented | HS-005 probability rationale unclear | Update documentation | Closed |
| ... | ... | ... | ... | ... |

---

## Appendix A: Extraction Metadata

| Attribute | Value |
|-----------|-------|
| Repository | [repo-url] |
| Commit | [hash] |
| Extraction Date | [timestamp] |
| Standard | ISO 14971:2019 |

## Appendix B: Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | [date] | [author] | Initial analysis |
| ... | ... | ... | ... |
```

## Compliance Mapping

| Document Section | ISO 14971 Clause | Evidence Provided |
|------------------|------------------|-------------------|
| Section 1 | 5.1 | Risk analysis documentation |
| Section 2.1 | 5.2 | Intended use |
| Section 2.2 | 5.2 | Reasonably foreseeable misuse |
| Section 3 | 5.3 | Characteristics related to safety |
| Section 4 | 5.4 | Hazard identification |
| Section 5 | 5.4 | Hazardous situation identification |
| Section 5 | 5.5 | Risk estimation |
| Section 6 | 6 | Risk evaluation |
| Section 7.1-7.2 | 7.1 | Risk control option analysis |
| Section 7.2 | 7.2 | Implementation and verification |
| Section 8 | 7.3 | Residual risk evaluation |
| Section 8.2 (B-R) | 7.4 | Benefit-risk analysis |
| Section 7.2 (New Hazards) | 7.5 | Risks from control measures |
| Section 9 | 7.6 | Completeness of risk control |
| Section 10 | 4.5 | Risk management file traceability |

## Examples

### Input: risks.json (partial)

```json
{
  "hazardous_situations": [
    {
      "situation_id": "HS-001",
      "hazard_id": "HAZ-001",
      "situation": "Clinician views patient record with missing heart rate",
      "potential_harm": "Delayed detection of cardiac event",
      "severity": "serious",
      "probability": "remote",
      "initial_risk_level": "ALARP"
    }
  ],
  "risk_controls": [
    {
      "control_id": "RC-001",
      "situation_id": "HS-001",
      "control_type": "protective_measure",
      "control_description": "Null check rejects missing heart rate values",
      "implementation": {
        "files": ["src/vitals/validation.ts:3-5"],
        "mechanism": "validation"
      },
      "verification": {
        "test_files": ["tests/vitals/validation.test.ts"],
        "verified": true
      },
      "residual_risk_level": "acceptable"
    }
  ]
}
```

### Output: Generated Risk Analysis Section

```markdown
#### HS-001: Missing Vital Sign Data Displayed to Clinician

**Related Hazard:** HAZ-001 - Missing heart rate data

**Hazardous Situation:** Clinician views patient record with missing heart rate, unaware data is absent

**Foreseeable Sequence of Events:**
1. Sensor disconnection or communication failure
2. Null/undefined value transmitted to system
3. Value displayed as blank or zero on patient record
4. Clinician assumes no recent reading rather than missing data
5. Cardiac event goes undetected

**Potential Harm:** Delayed detection of cardiac event

**Risk Estimation:**

| Factor | Level | Rationale |
|--------|-------|-----------|
| Severity | S3 (Serious) | Delayed cardiac intervention could require additional medical treatment |
| Probability | P3 (Remote) | Validation logic rejects null values with explicit error |
| Initial Risk | ALARP | S3/P3 per acceptability matrix |
```

## Validation Criteria

- [ ] All hazards from extraction are documented 🆔 O0XbGW
- [ ] All hazardous situations have severity and probability with rationale 🆔 YLFpq2
- [ ] Risk evaluation applies criteria from Risk Management Plan 🆔 9Lcj7d
- [ ] Risk controls are categorized correctly (inherent, protective, information) 🆔 73k93P
- [ ] Each control has implementation evidence (code location) 🆔 TqHl1C
- [ ] Each control has verification evidence (test references) 🆔 b8x2WR
- [ ] Residual risks are evaluated with post-control S/P levels 🆔 oGnQ11
- [ ] Benefit-risk analysis included for ALARP residual risks 🆔 ZXBoDg
- [ ] Completeness check confirms all risks addressed 🆔 5eNDLW
- [ ] Traceability links hazards through to verification 🆔 c3hh3b
- [ ] Gaps from extraction are documented with actions 🆔 czv64C
