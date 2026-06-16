---
id:
title: "Generate Software Verification Report"
version:
author:
effective_date:
type: "Prompt"
category: "generation"
standard: "IEC 62304"
clause: "5.5, 5.6, 5.7"
inputs: ["verification.json (from extraction)"]
outputs: ["Software Verification Report document"]
software_class: "all"
process: "[Software Development Process](../../Canvases/Software%20Development%20Process.canvas)"
requirements:
owner: "[Head of Quality Management](../../Assets/Head%20of%20Quality%20Management.md)"
---

# Generate Software Verification Report

## Context

The Software Verification Report documents evidence of verification activities per IEC 62304 clauses 5.5-5.7. This prompt transforms extracted verification data into a compliant report suitable for regulatory submission and design history file inclusion.

## Inputs

### Required
- `verification.json` — Output from [extraction-62304-verification](../extraction/extraction-62304-verification.md)
- `requirements.json` — For traceability to requirements

### Optional
- `architecture.json` — For component context
- `software-risk.json` — For risk control verification linkage

## Instructions

1. **Validate Input Data**
   - Verify verification.json contains test results
   - Check traceability links are complete
   - Confirm anomalies are documented

2. **Organize Verification Evidence**
   Structure by verification level:
   - Unit verification (5.5)
   - Integration testing (5.6)
   - System testing (5.7)

3. **Generate Traceability Matrix**
   Complete bidirectional traceability:
   - Requirements → Tests → Results
   - Include pass/fail status

4. **Document Anomalies**
   - List all anomalies found
   - Document resolution status
   - Assess safety impact

5. **Calculate Metrics**
   - Coverage percentages
   - Pass/fail rates
   - Acceptance criteria compliance

## Output Schema

```markdown
---
id:
title: "Software Verification Report - [Product Name]"
version:
author:
effective_date:
type: "Report"
document_id: "SVR-[product]-[version]"
software_safety_class: "A | B | C"
process: "[Software Development Process](../Canvases/Software%20Development%20Process.canvas)"
requirements:
owner: "[Head of Quality Management](../Assets/Head%20of%20Quality%20Management.md)"
---

# Software Verification Report

## 1. Introduction

### 1.1 Purpose
This report documents the software verification activities and results for [Product Name] per IEC 62304:2006+A1:2015.

### 1.2 Scope
This report covers verification of software version [version] including:
- Unit verification (Class B, C)
- Integration testing (Class B, C)
- System testing (Class A, B, C)

### 1.3 Software Safety Classification
This software is classified as **Class [A/B/C]** per IEC 62304.

### 1.4 Referenced Documents

| Document | Version | Relationship |
|----------|---------|--------------|
| Software Requirements Specification | [ver] | Requirements verified |
| Software Architecture Description | [ver] | Architecture verified |
| Risk Management File | [ver] | Risk controls verified |
| Software Development Plan | [ver] | Verification plan reference |

### 1.5 Verification Tools

| Tool | Version | Purpose | Validation Status |
|------|---------|---------|-------------------|
| [test framework] | [ver] | Unit/Integration testing | [status] |
| [coverage tool] | [ver] | Coverage measurement | [status] |
| [CI system] | [ver] | Automated execution | [status] |

## 2. Verification Summary

### 2.1 Overall Results

| Metric | Value |
|--------|-------|
| **Total Test Cases** | [n] |
| **Passed** | [n] ([%]) |
| **Failed** | [n] ([%]) |
| **Skipped** | [n] ([%]) |
| **Requirements Covered** | [n]/[total] ([%]) |
| **Code Coverage (Lines)** | [%] |
| **Code Coverage (Branches)** | [%] |

### 2.2 Verification Status by Level

| Level | Class | Tests | Passed | Failed | Coverage |
|-------|-------|-------|--------|--------|----------|
| Unit | B, C | [n] | [n] | [n] | [%] |
| Integration | B, C | [n] | [n] | [n] | [%] |
| System | A, B, C | [n] | [n] | [n] | [%] |

### 2.3 Acceptance Criteria Compliance (Class C)

| Criteria | Status | Evidence |
|----------|--------|----------|
| Proper event sequence | ✅/❌ | [reference] |
| Data and control flow | ✅/❌ | [reference] |
| Planned resource allocation | ✅/❌ | [reference] |
| Fault handling | ✅/❌ | [reference] |
| Initialization of variables | ✅/❌ | [reference] |
| Self-diagnostics | ✅/❌ | [reference] |
| Memory management | ✅/❌ | [reference] |
| Boundary conditions | ✅/❌ | [reference] |

## 3. Unit Verification (5.5)

> *Applicable to Class B and C software*

### 3.1 Unit Verification Process

**Verification Strategy:** [Description of unit test approach]

**Acceptance Criteria:** [Unit acceptance criteria per 5.5.3]

### 3.2 Unit Verification Results

#### 3.2.1 [Software Unit Name] (SU-xxx)

| Property | Value |
|----------|-------|
| Unit Path | `[file-path]` |
| Test File | `[test-file-path]` |
| Tests | [n] |
| Passed | [n] |
| Coverage | [%] |

**Test Cases:**

| Test ID | Description | Status | Duration |
|---------|-------------|--------|----------|
| UT-001 | [test name] | ✅ Pass | [ms] |
| UT-002 | [test name] | ✅ Pass | [ms] |

**Acceptance Criteria Coverage:**

| Criteria | Covered | Test Reference |
|----------|---------|----------------|
| Functional correctness | ✅ | UT-001 |
| Boundary conditions | ✅ | UT-002 |
| Fault handling | ✅ | UT-003 |
| ... | ... | ... |

---

[Repeat for each software unit]

### 3.3 Units Without Full Coverage

| Unit | Coverage | Gap | Justification |
|------|----------|-----|---------------|
| [unit] | [%] | [what's missing] | [why acceptable] |

## 4. Integration Testing (5.6)

> *Applicable to Class B and C software*

### 4.1 Integration Test Plan Reference
Integration testing performed per plan in [reference].

### 4.2 Integration Sequence
```
[Integration sequence diagram or description]
```

### 4.3 Integration Test Results

#### 4.3.1 [Integration Test Name] (IT-xxx)

| Property | Value |
|----------|-------|
| Integrated Items | SI-01, SI-02 |
| Test File | `[test-file-path]` |
| Status | ✅ Pass |

**Test Objective:** [What this integration test verifies]

**Test Procedure:**
1. [Step 1]
2. [Step 2]
3. [Step 3]

**Expected Result:** [Expected outcome]

**Actual Result:** [Actual outcome]

**Pass/Fail:** ✅ Pass

---

[Repeat for each integration test]

### 4.4 Interface Test Coverage

| Interface | Provider | Consumer | Test | Status |
|-----------|----------|----------|------|--------|
| IF-001 | SI-01 | SI-02 | IT-001 | ✅ |
| IF-002 | SI-02 | SI-03 | IT-002 | ✅ |

### 4.5 Regression Testing

| Regression Suite | Tests | Passed | Last Run |
|------------------|-------|--------|----------|
| [suite name] | [n] | [n] | [date] |

## 5. System Testing (5.7)

> *Applicable to Class A, B, and C software*

### 5.1 System Test Approach
[Description of system test methodology]

### 5.2 Test Configuration

| Component | Version/Configuration |
|-----------|----------------------|
| Software Under Test | [version] |
| Operating System | [OS version] |
| Hardware Platform | [hardware] |
| Database | [DB version] |
| Test Data | [data set reference] |

### 5.3 System Test Results

#### 5.3.1 [System Test Name] (ST-xxx)

| Property | Value |
|----------|-------|
| Requirements Verified | SRS-001, SRS-002 |
| Test File | `[test-file-path]` |
| Tester | [tester ID] |
| Date | [test date] |
| Status | ✅ Pass |

**Test Objective:** [What requirement(s) this test verifies]

**Input Stimuli:**
[Description of test inputs]

**Expected Outcome:**
[Description of expected behavior]

**Pass/Fail Criteria:**
[Specific criteria for pass/fail determination]

**Actual Result:**
[Description of actual behavior]

**Verdict:** ✅ Pass

**Test Configuration:**
- Software Version: [version]
- Test Tools: [tools used]

---

[Repeat for each system test per 5.7.5 requirements]

## 6. Requirements Traceability Matrix

### 6.1 Requirements to Test Traceability

| Requirement | Description | Test(s) | Result |
|-------------|-------------|---------|--------|
| SRS-001 | [title] | ST-001, ST-002 | ✅ Pass |
| SRS-002 | [title] | ST-003 | ✅ Pass |
| SRS-003 | [title] | ST-004, ST-005 | ✅ Pass |
| ... | ... | ... | ... |

### 6.2 Coverage Analysis

| Category | Total | Tested | Not Tested | Coverage |
|----------|-------|--------|------------|----------|
| Functional | [n] | [n] | [n] | [%] |
| Interface | [n] | [n] | [n] | [%] |
| Safety | [n] | [n] | [n] | [%] |
| Security | [n] | [n] | [n] | [%] |
| Performance | [n] | [n] | [n] | [%] |

### 6.3 Untested Requirements

| Requirement | Reason | Mitigation |
|-------------|--------|------------|
| [SRS-xxx] | [why not tested] | [alternative verification] |

## 7. Risk Control Verification

### 7.1 Risk Controls Tested

| Risk Control | Hazard | Test | Result |
|--------------|--------|------|--------|
| RC-SW-001 | HAZ-001 | ST-SAF-001 | ✅ Pass |
| RC-SW-002 | HAZ-001 | ST-SAF-002 | ✅ Pass |

### 7.2 Safety Requirement Verification

| Safety Req | Description | Test | Result |
|------------|-------------|------|--------|
| SRS-SAF-001 | [description] | ST-SAF-001 | ✅ Pass |

## 8. Anomalies

### 8.1 Anomaly Summary

| Severity | Found | Resolved | Open |
|----------|-------|----------|------|
| Critical | [n] | [n] | [n] |
| Major | [n] | [n] | [n] |
| Minor | [n] | [n] | [n] |

### 8.2 Anomaly Details

#### ANO-001: [Anomaly Title]

| Property | Value |
|----------|-------|
| ID | ANO-001 |
| Severity | [severity] |
| Found In | [test phase] |
| Test Reference | [test ID] |
| Status | Resolved |

**Description:**
[Description of the anomaly]

**Root Cause:**
[Identified root cause]

**Resolution:**
[How it was resolved]

**Verification:**
[How resolution was verified]

---

[Repeat for each anomaly]

### 8.3 Open Anomalies

| ID | Severity | Description | Safety Impact | Justification for Release |
|----|----------|-------------|---------------|---------------------------|
| ANO-xxx | [sev] | [description] | [impact] | [why acceptable] |

## 9. Verification Conclusion

### 9.1 Verification Statement

Based on the verification activities documented in this report:

- [ ] All software requirements have been verified 🆔 MBjXr5
- [ ] All risk control measures have been verified 🆔 z03juO
- [ ] All anomalies have been evaluated for safety impact 🆔 N4aXNr
- [ ] Open anomalies do not contribute to unacceptable risk 🆔 3YtwWN
- [ ] Verification is complete per the Software Development Plan 🆔 yynnuz

### 9.2 Recommendation

[Recommendation for release / further action required]

### 9.3 Approvals

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Software Developer | | | |
| Quality Assurance | | | |
| Project Manager | | | |

## 10. Appendices

### Appendix A: Complete Test Results
[Reference to detailed test execution reports]

### Appendix B: Coverage Reports
[Reference to code coverage reports]

### Appendix C: Extraction Metadata
- Repository: [repo]
- Commit: [hash]
- Extracted: [timestamp]
- Test Run: [CI run ID]
```

## Compliance Mapping

| Document Section | IEC 62304 Clause | Evidence Provided |
|------------------|------------------|-------------------|
| Section 3 | 5.5.1-5.5.5 | Unit verification |
| Section 3.3 | 5.5.3, 5.5.4 | Unit acceptance criteria |
| Section 4 | 5.6.1-5.6.8 | Integration testing |
| Section 4.5 | 5.6.6 | Regression testing |
| Section 5 | 5.7.1-5.7.5 | System testing |
| Section 5.3 | 5.7.5 | System test record contents |
| Section 6 | 5.7.4 | Requirements traceability |
| Section 7 | 7.3 | Risk control verification |
| Section 8 | 5.6.8, 5.7.2 | Anomaly documentation |

## Validation Criteria

- [ ] All verification levels are documented 🆔 bL3BiJ
- [ ] Test results include all required fields per 5.7.5 🆔 xkH4wm
- [ ] Traceability matrix is complete 🆔 PXvRub
- [ ] All requirements have verification evidence 🆔 W8b6yK
- [ ] Anomalies are documented with resolution status 🆔 hwdtyB
- [ ] Safety impact of open anomalies is assessed 🆔 OMvRSt
- [ ] Acceptance criteria compliance is documented (Class C) 🆔 rfwIxP
- [ ] Risk control verification is included 🆔 Xr1rEX
- [ ] Approval signatures section is present 🆔 jj3FIY
