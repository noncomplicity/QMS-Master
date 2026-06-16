---
id: 725af57
title: "generation 82304 validation report"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Prompt"
category: "generation"
standard: "IEC 82304-1"
clause: "6.3"
inputs: ["validation.json", "use-requirements.json", "verification.json"]
outputs: ["Validation_Report.md"]
software_class: "all"
process: "[Software Development Process](../../Canvases/Software%20Development%20Process.canvas)"
requirements:
owner: "[Head of Quality Management](../../Assets/Head%20of%20Quality%20Management.md)"
---

# Generate Validation Report

## Context

IEC 82304-1 clause 6.3 requires manufacturers to document validation results demonstrating that the health software product meets use requirements. The Validation Report provides evidence that the product is suitable for its intended use and that residual risks are acceptable.

Validation differs from verification:
- **Verification** (IEC 62304): Did we build the software correctly? (requirements → implementation)
- **Validation** (IEC 82304-1): Did we build the right product? (use requirements → product behavior)

## Inputs

### Required
- `validation.json` — Output from [extraction-82304-validation](../extraction/extraction-82304-validation.md)
- `use-requirements.json` — Output from [extraction-82304-use-requirements](../extraction/extraction-82304-use-requirements.md)

### Optional (enriches output)
- `verification.json` — Software verification evidence
- `risks.json` — Risk management data for residual risk assessment
- Clinical evaluation data
- Usability test results

## Instructions

1. **Validate Input Data**
   - Verify validation.json contains validation activities and results
   - Check traceability to use requirements
   - Confirm all use requirements are addressed

2. **Summarize Validation Scope**
   - List validation activities performed
   - Document constraints and limitations
   - Identify validation methods used

3. **Document Results by Use Requirement**
   - Map each use requirement to validation evidence
   - Document pass/fail status
   - Note any deviations or issues

4. **Assess Residual Risk**
   - Summarize residual risks from risk management
   - Confirm acceptability per risk management plan
   - Document any disclosures required

5. **Generate Conclusions**
   - Overall validation status
   - Release recommendation
   - Outstanding issues

## Output Schema

Generate a markdown document with this structure:

```markdown
---
id: 725af57
title: "generation 82304 validation report"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Report"
document_id: "VAL-RPT-[product]-[version]"
process: "[Document and Record Control](../Canvases/Document%20and%20Record%20Control.canvas)"
requirements: "[IEC 82304-1](../Requirements/IEC_82304_Requirements.md)"
owner: "[Head of Quality Management](../Assets/Head%20of%20Quality%20Management.md)"
---

# Validation Report

## [Product Name] v[Version]

**Manufacturer:** [Manufacturer Name]
**Document Version:** [Report Version]
**Validation Date:** [Date]

---

## Executive Summary

### Product Information

| Attribute | Value |
|-----------|-------|
| Product Name | [Name] |
| Product Version | [Version] |
| Intended Use | [Brief statement] |
| Regulatory Classification | [Class] |

### Validation Summary

| Metric | Result |
|--------|--------|
| Total Use Requirements | [n] |
| Requirements Validated | [n] |
| Validation Pass | [n] |
| Validation Fail | [n] |
| Validation Partial/Conditional | [n] |
| Overall Status | [Pass / Fail / Conditional] |

### Conclusion

**Validation Result:** [Pass / Fail / Conditional Pass]

**Summary Statement:** [1-2 sentence summary of validation outcome]

---

## 1. Introduction

### 1.1 Purpose

This Validation Report documents the results of validation activities performed on [Product Name] to demonstrate that the health software product meets its use requirements and is suitable for its intended use, per IEC 82304-1:2016 clause 6.3.

### 1.2 Scope

| Element | Description |
|---------|-------------|
| Product | [Product Name] v[Version] |
| Validation Period | [Start Date] to [End Date] |
| Validation Environment | [Environment description] |
| Validation Team | [Team members/roles] |

### 1.3 Referenced Documents

| Document | Version | Purpose |
|----------|---------|---------|
| Validation Plan | [ver] | Defines validation approach |
| Use Requirements | [ver] | Requirements being validated |
| Software Verification Report | [ver] | Supporting verification evidence |
| Risk Management Report | [ver] | Residual risk assessment |
| Usability Test Report | [ver] | Usability validation evidence |

---

## 2. Validation Approach

### 2.1 Validation Plan Summary

[Brief summary of the validation plan approach]

### 2.2 Validation Activities

| Activity ID | Activity | Method | Scope |
|-------------|----------|--------|-------|
| [VA-001] | [Activity name] | [Method] | [What was validated] |
| [VA-002] | [Activity name] | [Method] | [What was validated] |
| ... | ... | ... | ... |

### 2.3 Validation Methods

| Method | Description | When Used |
|--------|-------------|-----------|
| Functional Testing | End-to-end feature testing | Core functionality |
| Usability Testing | User testing with representative users | User interface requirements |
| Performance Testing | Load and stress testing | Performance requirements |
| Security Testing | Penetration and vulnerability testing | Security requirements |
| Integration Testing | External system integration | Interoperability requirements |
| Clinical Validation | Clinical workflow testing | Clinical use requirements |

### 2.4 Validation Constraints

[From validation.json constraints]

| Constraint | Impact | Mitigation |
|------------|--------|------------|
| [Constraint description] | [How it affects validation] | [How addressed] |
| ... | ... | ... |

### 2.5 Validation Team

| Role | Name | Qualification | Independence |
|------|------|---------------|--------------|
| Validation Lead | [name] | [qualifications] | [independent from design?] |
| Tester | [name] | [qualifications] | [independence] |
| Clinical Expert | [name] | [qualifications] | [independence] |
| ... | ... | ... | ... |

---

## 3. Validation Results by Use Requirement

### 3.1 Results Summary

| Req ID | Requirement | Status | Evidence |
|--------|-------------|--------|----------|
| USE-001 | [requirement title] | Pass | VA-001, VA-003 |
| USE-002 | [requirement title] | Pass | VA-002 |
| USE-003 | [requirement title] | Conditional | VA-004 (see 3.2.3) |
| ... | ... | ... | ... |

### 3.2 Detailed Results

#### 3.2.1 USE-001: [Requirement Title]

**Requirement:** [Full requirement statement]

**Validation Activities:**
- VA-001: [Activity description]
- VA-003: [Activity description]

**Results:**

| Test/Activity | Expected Result | Actual Result | Status |
|---------------|-----------------|---------------|--------|
| [Test name] | [Expected] | [Actual] | Pass |
| [Test name] | [Expected] | [Actual] | Pass |

**Evidence:**
- [Test report reference]
- [Screenshot/log reference]

**Conclusion:** Requirement validated successfully.

---

#### 3.2.2 USE-002: [Requirement Title]

**Requirement:** [Full requirement statement]

**Validation Activities:**
- VA-002: [Activity description]

**Results:**

| Test/Activity | Expected Result | Actual Result | Status |
|---------------|-----------------|---------------|--------|
| [Test name] | [Expected] | [Actual] | Pass |

**Evidence:**
- [Evidence reference]

**Conclusion:** Requirement validated successfully.

---

#### 3.2.3 USE-003: [Requirement Title] (Conditional)

**Requirement:** [Full requirement statement]

**Validation Activities:**
- VA-004: [Activity description]

**Results:**

| Test/Activity | Expected Result | Actual Result | Status |
|---------------|-----------------|---------------|--------|
| [Test name] | [Expected] | [Actual] | Partial |

**Deviation/Issue:**
- **Description:** [What didn't meet expectations]
- **Impact:** [Impact on intended use]
- **Justification:** [Why still acceptable]
- **Conditions:** [Conditions under which product is acceptable]

**Evidence:**
- [Evidence reference]

**Conclusion:** Requirement conditionally validated. [Conditions for acceptance]

---

[Repeat for each use requirement]

---

## 4. Platform Validation

### 4.1 Claimed Platforms

| Platform | Version | Validation Status |
|----------|---------|-------------------|
| [OS/Browser/Device] | [Version] | [Validated/Not Validated] |
| ... | ... | ... |

### 4.2 Platform-Specific Results

[Summary of any platform-specific validation findings]

---

## 5. Residual Risk Assessment

### 5.1 Summary of Residual Risks

| Risk ID | Description | Severity | Status |
|---------|-------------|----------|--------|
| [RR-001] | [Risk description] | [Severity] | Acceptable |
| [RR-002] | [Risk description] | [Severity] | Acceptable (B-R) |
| ... | ... | ... | ... |

### 5.2 Residual Risk Acceptability

**Assessment:** All residual risks have been evaluated and are acceptable per the Risk Management Plan criteria.

**Benefit-Risk Summary:** [For risks requiring benefit-risk analysis]

### 5.3 Residual Risk Disclosures

The following residual risks require disclosure to users:

| Risk | Disclosure | Location |
|------|------------|----------|
| [Risk description] | [Disclosure text] | IFU Section X |
| ... | ... | ... |

**Disclosure Implementation:** [Confirmed / Pending]

---

## 6. Traceability

### 6.1 Use Requirement to Validation Traceability

| Use Requirement | System Requirement | Validation Activity | Result |
|-----------------|-------------------|---------------------|--------|
| USE-001 | SYS-001, SYS-002 | VA-001, VA-003 | Pass |
| USE-002 | SYS-003 | VA-002 | Pass |
| ... | ... | ... | ... |

### 6.2 Coverage Analysis

| Category | Total | Validated | Coverage |
|----------|-------|-----------|----------|
| Use Requirements | [n] | [n] | [%] |
| Intended Users Tested | [n] | [n] | [%] |
| Platforms Tested | [n] | [n] | [%] |
| Essential Functions | [n] | [n] | [%] |

### 6.3 Gaps

| Gap | Description | Impact | Recommendation |
|-----|-------------|--------|----------------|
| [GAP-001] | [Description] | [Impact] | [Action] |
| ... | ... | ... | ... |

---

## 7. Issues and Deviations

### 7.1 Validation Issues

| Issue ID | Description | Severity | Resolution | Status |
|----------|-------------|----------|------------|--------|
| [VAL-ISS-001] | [Issue] | [Sev] | [Resolution] | [Status] |
| ... | ... | ... | ... | ... |

### 7.2 Deviations from Validation Plan

| Deviation | Rationale | Impact |
|-----------|-----------|--------|
| [Deviation description] | [Why deviated] | [Impact on results] |
| ... | ... | ... |

### 7.3 Known Anomalies

| Anomaly | Severity | Risk Assessment | Disposition |
|---------|----------|-----------------|-------------|
| [Anomaly description] | [Sev] | [Risk assessment] | [Accept/Fix/Defer] |
| ... | ... | ... | ... |

---

## 8. Conclusions

### 8.1 Validation Summary

| Criterion | Result |
|-----------|--------|
| All use requirements addressed | [Yes/No] |
| All validation activities completed | [Yes/No] |
| All critical issues resolved | [Yes/No] |
| Residual risk acceptable | [Yes/No] |
| Product suitable for intended use | [Yes/No] |

### 8.2 Validation Statement

Based on the validation activities documented in this report:

**[Select one:]**

**PASS:** The health software product [Product Name] v[Version] has been validated and demonstrates that it meets the documented use requirements. The product is suitable for its intended use, and the residual risk is acceptable.

**CONDITIONAL PASS:** The health software product [Product Name] v[Version] has been validated with the following conditions: [List conditions]. Subject to these conditions, the product meets the documented use requirements and is suitable for its intended use.

**FAIL:** The health software product [Product Name] v[Version] has not passed validation. The following issues must be resolved before release: [List issues].

### 8.3 Recommendations

[Any recommendations for release, post-market monitoring, or future validation]

---

## 9. Approval

### 9.1 Report Approval

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Validation Lead | | | |
| Quality Assurance | | | |
| Clinical Expert (if applicable) | | | |

### 9.2 Release Authorization

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Regulatory Affairs | | | |
| Top Management | | | |

---

## Appendix A: Validation Evidence Index

| Evidence ID | Description | Location |
|-------------|-------------|----------|
| [EVD-001] | [Description] | [Path/Link] |
| ... | ... | ... |

## Appendix B: Test Environment Details

| Component | Specification |
|-----------|---------------|
| Hardware | [Details] |
| Operating System | [Details] |
| Browser | [Details] |
| Network | [Details] |
| Test Data | [Details] |

## Appendix C: Extraction Metadata

| Attribute | Value |
|-----------|-------|
| Repository | [repo-url] |
| Commit | [hash] |
| Extraction Date | [timestamp] |
| Standard | IEC 82304-1:2016 |

## Appendix D: Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | [date] | [author] | Initial report |
| ... | ... | ... | ... |
```

## Compliance Mapping

| Document Section | IEC 82304-1 Clause | Evidence Provided |
|------------------|-------------------|-------------------|
| Section 3 | 6.3 a) | Traceability to use requirements |
| Section 3.2 | 6.3 b) | Evidence of meeting requirements |
| Section 5 | 6.3 c) | Residual risk acceptability |
| Section 6 | 6.3 a) | Traceability documentation |
| Section 2.5 | 6.1 e), f) | Personnel qualification and independence |
| Section 4 | 6.2 | Platform validation |

## Compliance Checklist

Before finalizing the generated Validation Report, verify:

- [ ] **6.3 a)** - Traceability to use requirements is documented 🆔 77e5tl
- [ ] **6.3 b)** - Evidence that product meets use requirements is provided 🆔 vKBf8X
- [ ] **6.3 c)** - Residual risk evaluation confirms acceptability 🆔 h3v9I0
- [ ] All use requirements have validation results 🆔 RiIml6
- [ ] All claimed platforms are validated 🆔 MpOIwj
- [ ] Validation team independence is documented 🆔 OUY7YF
- [ ] Deviations from validation plan are justified 🆔 gzh8up
- [ ] Known anomalies are risk-assessed 🆔 HizNRE
- [ ] Residual risk disclosures are identified 🆔 0UUSUl
- [ ] Approval signatures section is included 🆔 nIcYo3

## Examples

### Input: validation.json (partial)

```json
{
  "validation_activities": [
    {
      "activity_id": "VA-001",
      "name": "Core Functionality Validation",
      "method": "functional_testing",
      "use_requirements_covered": ["USE-001", "USE-002"],
      "results": {
        "status": "pass",
        "tests_executed": 45,
        "tests_passed": 45
      }
    }
  ],
  "requirement_coverage": [
    {
      "use_requirement": "USE-001",
      "validation_activities": ["VA-001"],
      "status": "validated",
      "evidence": ["test_report_001.pdf"]
    }
  ]
}
```

### Output: Generated Validation Results Section

```markdown
#### 3.2.1 USE-001: Track Patient Vital Signs

**Requirement:** The system shall allow patients to track blood pressure, heart rate, weight, and blood glucose measurements.

**Validation Activities:**
- VA-001: Core Functionality Validation (functional testing)

**Results:**

| Test/Activity | Expected Result | Actual Result | Status |
|---------------|-----------------|---------------|--------|
| Enter blood pressure | Value stored and displayed | Value stored and displayed correctly | Pass |
| Enter heart rate | Value stored and displayed | Value stored and displayed correctly | Pass |
| Enter weight | Value stored and displayed | Value stored and displayed correctly | Pass |
| Enter blood glucose | Value stored and displayed | Value stored and displayed correctly | Pass |

**Evidence:**
- test_report_001.pdf

**Conclusion:** Requirement validated successfully. All vital sign tracking functions operate as specified.
```

## Validation Criteria

- [ ] All use requirements from use-requirements.json are addressed 🆔 dvtkQl
- [ ] Each requirement has clear pass/fail/conditional status 🆔 QJUpX4
- [ ] Conditional passes have documented justification and conditions 🆔 SAykHz
- [ ] Platform coverage matches claimed platforms 🆔 SrpKBm
- [ ] Residual risks are summarized from risk management 🆔 ySe1XF
- [ ] Traceability links use requirements to validation evidence 🆔 nUqhYh
- [ ] Validation team and independence are documented 🆔 PCml3v
- [ ] Issues and deviations are documented with resolution 🆔 UBPivQ
- [ ] Executive summary provides clear validation status 🆔 T9oHlt
- [ ] Approval section includes all required signatures 🆔 fKIGw1
