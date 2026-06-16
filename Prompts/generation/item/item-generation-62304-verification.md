---
id:
title: "Generate Item Verification Document"
version:
author:
effective_date:
type: "Prompt"
category: "generation"
level: "item"
standard: "IEC 62304"
clause: "5.5, 5.6, 5.7"
inputs: ["item-verification.json"]
outputs: ["Item-Verification.md"]
software_class: "all"
process: "[Software Development Process](../../../Canvases/Software%20Development%20Process.canvas)"
requirements: "[IEC 62304](../../../Requirements/IEC_62304_Requirements.md)"
owner: "[Head of Quality Management](../../../Assets/Head%20of%20Quality%20Management.md)"
---

# Generate Item Verification Document

## Context

This prompt generates a human-readable document describing the verification evidence for a software item. It is explicitly **NOT** a complete Verification Report or Validation Report — those documents are generated at the system level.

**Purpose of this document:**
- Document test coverage within this repository
- Track unit and integration test results
- Map tests to requirements
- Report anomalies and their resolution
- Support system-level verification aggregation
- Identify verification gaps

## Inputs

### Required
- **`item-verification.json`** — Output from item-extraction-62304-verification

### Optional
- **`item-requirements.json`** — For requirement traceability
- **`item-architecture.json`** — For component mapping
- **`item-risk-contribution.json`** — For risk control verification

## Instructions

Generate a markdown document following the output structure below. The document should:
1. Clearly state this is an item-level document, not a system report
2. Summarize test results and coverage
3. Detail verification by category (unit, integration, requirement)
4. Report anomalies
5. Identify verification gaps

## Output Document Structure

```markdown
---
id:
title: "Item Verification - [Item Name]"
version:
author:
effective_date:
type: "Verification"
document_id: "ITEM-VER-[item-id]-[version]"
level: "item"
process: "[Software Development Process](../../Canvases/Software%20Development%20Process.canvas)"
requirements: "[IEC 62304](../../Requirements/IEC_62304_Requirements.md)"
owner: "[Head of Quality Management](../../Assets/Head%20of%20Quality%20Management.md)"
---

# Item Verification

## [Item Name]

**Software Item:** [item_id]
**Repository:** [repository URL]
**Version/Commit:** [commit hash]
**Safety Classification:** [Class A/B/C]
**Test Framework:** [jest | pytest | junit | etc]
**CI System:** [github-actions | gitlab-ci | jenkins | etc]
**Extraction Date:** [date]

---

## Important Notice

> This is an **ITEM-LEVEL** document.
>
> This document describes verification evidence for **[Item Name]** only.
> It is NOT a Verification Report or Validation Report.
>
> For complete verification documentation, see the **System-Level** documents:
> - System Verification Report: `SYS-VER-[system]-[version].md`
> - Validation Report: `SYS-VAL-[system]-[version].md`

---

## 1. Executive Summary

### 1.1 Test Results Summary

| Metric | Value |
|--------|-------|
| Total Tests | [n] |
| Passed | [n] ([%]%) |
| Failed | [n] ([%]%) |
| Skipped | [n] ([%]%) |
| Execution Time | [duration] |

### 1.2 Coverage Summary

| Metric | Coverage | Target | Status |
|--------|----------|--------|--------|
| Lines | [n]% | [n]% | [Pass/Fail] |
| Branches | [n]% | [n]% | [Pass/Fail] |
| Functions | [n]% | [n]% | [Pass/Fail] |

### 1.3 Requirements Coverage

| Category | Total | Covered | Partial | Missing |
|----------|-------|---------|---------|---------|
| All Requirements | [n] | [n] | [n] | [n] |
| Safety Requirements | [n] | [n] | [n] | [n] |

### 1.4 Anomaly Summary

| Severity | Open | Resolved | Deferred |
|----------|------|----------|----------|
| Critical | [n] | [n] | [n] |
| Major | [n] | [n] | [n] |
| Minor | [n] | [n] | [n] |

---

## 2. Unit Verification (IEC 62304 5.5)

### 2.1 Component Coverage

| Component | Tests | Passed | Coverage | Acceptance Criteria |
|-----------|-------|--------|----------|---------------------|
| [ITEM-XX-COMP-001] | [n] | [n] | [%]% | [5/7] |
| [ITEM-XX-COMP-002] | [n] | [n] | [%]% | [6/7] |

### 2.2 Acceptance Criteria Coverage (Class B/C)

Per IEC 62304 5.5.3/5.5.4, unit tests must cover:

| Criterion | Required | Covered | Evidence |
|-----------|----------|---------|----------|
| Functional Correctness | Yes | [Yes/No] | [test count] tests |
| Event Sequence | Yes | [Yes/No] | [test count] tests |
| Data/Control Flow | Yes | [Yes/No] | [test count] tests |
| Fault Handling | Yes | [Yes/No] | [test count] tests |
| Boundary Conditions | Yes | [Yes/No] | [test count] tests |
| Memory Management | Class C | [Yes/No/N/A] | [test count] tests |
| Initialization | Yes | [Yes/No] | [test count] tests |

### 2.3 Unit Test Details by Component

#### [ITEM-XX-COMP-001]: [Component Name]

**Path:** `[directory path]`

**Test Files:**
- `[test file path]`

**Coverage:**
- Lines: [%]%
- Branches: [%]%
- Functions: [%]%

**Acceptance Criteria Status:**
| Criterion | Status | Tests |
|-----------|--------|-------|
| Functional | Covered | [UT-001], [UT-002] |
| Boundary | Covered | [UT-003] |
| Fault Handling | Missing | - |

**Test Results:**

| ID | Test Name | Type | Status | Duration |
|----|-----------|------|--------|----------|
| [ITEM-XX-UT-001] | [test name] | [functional] | [passed] | [5ms] |
| [ITEM-XX-UT-002] | [test name] | [boundary] | [passed] | [3ms] |
| [ITEM-XX-UT-003] | [test name] | [fault_handling] | [failed] | [10ms] |

---

[Repeat for each component]

---

### 2.4 Untested Components

| Component | Reason | Recommendation |
|-----------|--------|----------------|
| [ITEM-XX-COMP-xxx] | [reason] | [recommendation] |

---

## 3. Integration Verification (IEC 62304 5.6)

### 3.1 Integration Test Summary

| Metric | Value |
|--------|-------|
| Integration Tests | [n] |
| Passed | [n] |
| Failed | [n] |
| Components Covered | [n] / [n] |
| Interfaces Tested | [n] / [n] |

### 3.2 Integration Tests

| ID | Name | Components | Coverage Type | Status |
|----|------|------------|---------------|--------|
| [ITEM-XX-IT-001] | [test name] | [COMP-001], [COMP-002] | [interface] | [passed] |
| [ITEM-XX-IT-002] | [test name] | [COMP-002], [COMP-003] | [functionality] | [passed] |

### 3.3 Internal Interface Coverage

| Interface | Tests | Status |
|-----------|-------|--------|
| [ITEM-XX-INT-001] | [IT-001], [IT-002] | Covered |
| [ITEM-XX-INT-002] | - | Missing |

### 3.4 Regression Test Suite

**Execution Frequency:** [per-commit | nightly | weekly | manual]

**Last Full Run:** [timestamp]

**Regression Tests:**
| ID | Name | Last Status | Last Run |
|----|------|-------------|----------|
| [ITEM-XX-IT-001] | [test name] | [passed] | [timestamp] |

---

## 4. Requirement Verification (IEC 62304 5.7)

### 4.1 Requirements Traceability

| Requirement | Type | Test(s) | Status |
|-------------|------|---------|--------|
| [ITEM-XX-REQ-001] | Functional | [TC-001] | Verified |
| [ITEM-XX-REQ-002] | Interface | [TC-002], [TC-003] | Verified |
| [ITEM-XX-REQ-003] | Safety | [TC-004] | Partial |
| [ITEM-XX-REQ-004] | Performance | - | Missing |

### 4.2 Verification Details

#### [ITEM-XX-REQ-001]: [Requirement Title]

**Type:** [Functional | Interface | Safety | Performance | Security]

**Tests:**

| ID | Name | Input | Expected | Criteria | Status |
|----|------|-------|----------|----------|--------|
| [ITEM-XX-TC-001] | [test name] | [input description] | [expected outcome] | [pass/fail criteria] | [passed] |

**Verification Status:** [verified | partial | missing]

---

[Repeat for key requirements, especially safety-related]

---

### 4.3 Untested Requirements

| Requirement | Type | Reason |
|-------------|------|--------|
| [ITEM-XX-REQ-xxx] | [type] | [no_test_exists | test_not_run | unclear_mapping] |

---

## 5. Risk Control Verification

### 5.1 Summary

| Hazard | Risk Control | Test | Status |
|--------|--------------|------|--------|
| [ITEM-XX-HAZ-001] | [RC-001] | [TC-xxx] | Verified |
| [ITEM-XX-HAZ-002] | [RC-002] | - | Missing |

### 5.2 Details

#### [ITEM-XX-HAZ-001]: [Hazard Description]

**Risk Control:** [ITEM-XX-RC-001] - [control description]

**Verification Test:** [ITEM-XX-TC-xxx]

**Test Description:** [how the control is verified]

**Status:** [Verified | Partial | Missing]

**Evidence:** `[test file:test name]`

---

## 6. Anomalies

### 6.1 Open Anomalies

| ID | Source Test | Severity | Description | Days Open |
|----|-------------|----------|-------------|-----------|
| [ITEM-XX-ANO-001] | [TC-xxx] | [critical] | [description] | [n] |

### 6.2 Resolved Anomalies

| ID | Source Test | Severity | Resolution | Resolved Date |
|----|-------------|----------|------------|---------------|
| [ITEM-XX-ANO-010] | [TC-xxx] | [major] | [resolution] | [date] |

### 6.3 Deferred Anomalies

| ID | Severity | Rationale for Deferral | Risk Assessment |
|----|----------|------------------------|-----------------|
| [ITEM-XX-ANO-020] | [minor] | [rationale] | [acceptable risk statement] |

---

## 7. Test Environment

### 7.1 CI/CD Configuration

**System:** [CI system name]

**Workflow File:** `[path to workflow]`

**Triggers:**
- [push to main]
- [pull request]
- [nightly schedule]

**Test Stages:**
1. [stage 1]
2. [stage 2]

### 7.2 Runtime Environment

| Component | Version |
|-----------|---------|
| Operating System | [OS version] |
| [Runtime] | [version] |
| [Database] | [version] |

### 7.3 Test Tools

| Tool | Version | Purpose |
|------|---------|---------|
| [tool name] | [version] | [purpose] |
| [coverage tool] | [version] | [coverage analysis] |

---

## 8. Gaps and Issues

### 8.1 Summary

| Priority | Count |
|----------|-------|
| High | [n] |
| Medium | [n] |
| Low | [n] |

### 8.2 Gap Details

| ID | Type | Description | Affected | IEC 62304 | Priority | Recommendation |
|----|------|-------------|----------|-----------|----------|----------------|
| [ITEM-XX-VER-GAP-001] | [untested_component] | [description] | [IDs] | [5.5.x] | [high] | [recommendation] |
| [ITEM-XX-VER-GAP-002] | [missing_acceptance_criteria] | [description] | [IDs] | [5.5.3] | [medium] | [recommendation] |

**Gap Types:**
- `untested_component`: Component without test coverage
- `missing_acceptance_criteria`: Missing IEC 62304 5.5.3/5.5.4 criterion
- `untested_requirement`: Requirement without verification test
- `missing_interface_test`: Interface not tested
- `no_regression_suite`: No regression tests defined
- `failed_tests`: Tests currently failing
- `insufficient_coverage`: Below target coverage

---

## 9. Traceability

### 9.1 Tests to Components

| Test | Components |
|------|------------|
| [ITEM-XX-TC-001] | [COMP-001] |
| [ITEM-XX-IT-001] | [COMP-001], [COMP-002] |

### 9.2 Tests to Requirements

| Test | Requirements |
|------|--------------|
| [ITEM-XX-TC-001] | [REQ-001] |

### 9.3 Tests to Risks

| Test | Hazards |
|------|---------|
| [ITEM-XX-TC-001] | [HAZ-001] |

---

## Appendix A: Complete Test List

| ID | Name | Type | File | Status | Duration |
|----|------|------|------|--------|----------|
| [ITEM-XX-UT-001] | [name] | unit | [file] | passed | [5ms] |
| [ITEM-XX-UT-002] | [name] | unit | [file] | passed | [3ms] |
| [ITEM-XX-IT-001] | [name] | integration | [file] | passed | [50ms] |

---

## Appendix B: Extraction Metadata

| Attribute | Value |
|-----------|-------|
| Repository | [repository] |
| Commit | [commit] |
| Extraction Date | [extracted_at] |
| Extractor Version | [extractor_version] |
| Standard | [standard] |

---

*This document is part of the regulatory documentation for [Item Name].*
*IEC 62304:2006+AMD1:2015 Clauses 5.5, 5.6, 5.7 Compliant — Item Level*
```

## Compliance Checklist

Before finalizing the document:

- [ ] Document clearly states it is item-level, not system report 🆔 Zdr6vH
- [ ] Test results summary is accurate 🆔 OHfXOq
- [ ] Unit tests categorized by acceptance criteria type 🆔 WI1WTI
- [ ] Integration tests identify components tested 🆔 rDdhh7
- [ ] Requirement traceability is complete 🆔 DyWLM9
- [ ] Risk control verification documented 🆔 aqphz1
- [ ] Anomalies have severity and status 🆔 bucm0J
- [ ] Gaps are identified with IEC 62304 clause reference 🆔 hCCu4i
- [ ] Test environment documented 🆔 btOmyj
- [ ] Traceability matrix is complete 🆔 W17lcn

## Validation Criteria

- [ ] Document follows the prescribed structure 🆔 nZWx3T
- [ ] All sections populated from extracted JSON 🆔 52qZd5
- [ ] Missing data flagged with [TODO] markers 🆔 q90265
- [ ] Important notice about document scope is prominent 🆔 JRJbPu
- [ ] References to system-level documents are included 🆔 8QNEdO
- [ ] Document is readable by QA personnel 🆔 WQfwg8
