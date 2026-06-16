---
id: 725af57
title: "generation 14971 rmr"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Prompt"
category: "generation"
standard: "ISO 14971"
clause: "8, 9"
inputs: ["risks.json (from extraction)", "Risk Management Plan", "Risk Analysis", "verification records"]
outputs: ["Risk Management Report document"]
software_class: "all"
process: "[Risk Management Process](../../Canvases/Risk%20Management%20Process.canvas)"
requirements: "[ISO 14971 Risk management for medical devices](../../Requirements/ISO_14971_Requirements.md)"
owner: "[Head of Risk Management](../../Assets/Head%20of%20Risk%20Management.md)"
---

# Generate Risk Management Report

## Context

The Risk Management Report (RMR) is the final deliverable per ISO 14971 clause 9, required prior to commercial release. It summarizes the execution of the Risk Management Plan, confirms overall residual risk acceptability, and declares readiness for post-production monitoring. This prompt transforms extracted risk data and analysis into a compliant RMR document.

## Inputs

### Required
- `risks.json` — Output from [extraction-14971-risks](../extraction/extraction-14971-risks.md)
- Risk Management Plan (for plan compliance verification)
- Risk Analysis document (for summary of activities)
- Verification records (test reports, review records)

### Optional (enriches output)
- Clinical evaluation data (benefit evidence)
- Usability test results
- Previous Risk Management Reports (for product revisions)

## Instructions

1. **Verify Plan Execution**
   - Check all planned activities completed
   - Confirm all hazards analyzed
   - Verify all controls implemented and verified

2. **Summarize Risk Analysis**
   - Count hazards, situations, controls
   - Summarize risk levels before/after control
   - Identify outstanding gaps

3. **Evaluate Overall Residual Risk**
   - Aggregate all residual risks
   - Compare against benefits
   - Document acceptability decision

4. **Confirm Post-Production Readiness**
   - Verify monitoring methods in place
   - Confirm information collection processes
   - Document review triggers

5. **Generate Executive Summary**
   - Key findings
   - Acceptability conclusion
   - Release recommendation

## Output Schema

Generate a markdown document with this structure:

```markdown
---
id: 725af57
title: "generation 14971 rmr"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Report"
document_id: "RMR-[product]-[version]"
process: "[Risk Management Process](../../Canvases/Risk%20Management%20Process.canvas)"
requirements: "[ISO 14971](../../Requirements/ISO_14971_Requirements.md)"
owner: "[Head of Risk Management](../../Assets/Head%20of%20Risk%20Management.md)"
---

# Risk Management Report

## Executive Summary

### Product
| Attribute | Value |
|-----------|-------|
| Product Name | [Name] |
| Version | [Version] |
| Regulatory Class | [Class] |
| Report Date | [Date] |

### Key Findings

| Metric | Count |
|--------|-------|
| Total Hazards Identified | [n] |
| Total Hazardous Situations | [n] |
| Risk Controls Implemented | [n] |
| Residual Risks (Acceptable) | [n] |
| Residual Risks (ALARP with B-R) | [n] |
| Residual Risks (Unacceptable) | [n] |
| Outstanding Gaps | [n] |

### Conclusion

**Overall Residual Risk:** [Acceptable | Unacceptable]

**Release Recommendation:** [Recommended for release | Not recommended - actions required]

**Summary Statement:**
[1-2 sentence summary of risk management outcome]

---

## 1. Purpose and Scope

### 1.1 Purpose
This Risk Management Report summarizes the execution of the Risk Management Plan for [Product Name] and provides evidence that:
- The Risk Management Plan has been appropriately implemented
- The overall residual risk is acceptable
- Methods are in place for production and post-production information collection and review

### 1.2 Scope
| Element | Status |
|---------|--------|
| Risk Management Plan | [RMP-xxx, version] |
| Risk Analysis | [RA-xxx, version] |
| Product Version | [version] |
| Lifecycle Phase | Pre-release review |

### 1.3 Referenced Documents

| Document | Version | Location |
|----------|---------|----------|
| Risk Management Plan | [ver] | [link] |
| Risk Analysis | [ver] | [link] |
| Software Requirements Specification | [ver] | [link] |
| Verification Test Reports | [ver] | [link] |
| Clinical Evaluation Report | [ver] | [link] |

## 2. Review of Risk Management Plan Implementation

### 2.1 Planned vs. Completed Activities

| Planned Activity (from RMP) | Status | Evidence |
|----------------------------|--------|----------|
| Intended use analysis | ✓ Complete | RA Section 2 |
| Hazard identification | ✓ Complete | RA Section 4 |
| Risk estimation | ✓ Complete | RA Section 5 |
| Risk evaluation | ✓ Complete | RA Section 6 |
| Risk control implementation | ✓ Complete | RA Section 7 |
| Risk control verification | ✓ Complete | Test Reports |
| Residual risk evaluation | ✓ Complete | RA Section 8 |
| Overall residual risk evaluation | ✓ Complete | This report, Section 4 |
| Post-production planning | ✓ Complete | This report, Section 5 |

### 2.2 Deviations from Plan

| Deviation | Rationale | Impact |
|-----------|-----------|--------|
| [description or "None"] | [rationale] | [impact assessment] |

### 2.3 Risk Management Team Confirmation

| Role | Name | Confirmation |
|------|------|--------------|
| Risk Management Lead | [name] | Activities completed per plan |
| Design Engineering | [name] | Controls implemented as specified |
| Quality Assurance | [name] | Verification completed |

## 3. Summary of Risk Analysis Results

### 3.1 Hazard Summary

**Total Hazards Identified:** [n]

| Category | Count | Percentage |
|----------|-------|------------|
| Clinical Data | [n] | [%] |
| Calculation | [n] | [%] |
| Timing/Availability | [n] | [%] |
| Security | [n] | [%] |
| User Interface | [n] | [%] |
| Integration | [n] | [%] |

### 3.2 Risk Estimation Summary

**Total Hazardous Situations:** [n]

**Initial Risk Distribution (before controls):**

| Risk Level | Count | Percentage |
|------------|-------|------------|
| Acceptable | [n] | [%] |
| ALARP | [n] | [%] |
| Unacceptable | [n] | [%] |

### 3.3 Risk Control Summary

**Total Risk Controls Implemented:** [n]

| Control Type | Count | Percentage |
|--------------|-------|------------|
| Inherent Safety by Design | [n] | [%] |
| Protective Measures | [n] | [%] |
| Information for Safety | [n] | [%] |

**Verification Status:**

| Status | Count |
|--------|-------|
| Verified - Pass | [n] |
| Verified - Fail | [n] |
| Pending Verification | [n] |

### 3.4 Residual Risk Summary

**Residual Risk Distribution (after controls):**

| Risk Level | Count | Percentage |
|------------|-------|------------|
| Acceptable | [n] | [%] |
| ALARP (with B-R justification) | [n] | [%] |
| Unacceptable | [n] | [%] |

### 3.5 Risk Reduction Achieved

| Metric | Before Controls | After Controls | Reduction |
|--------|-----------------|----------------|-----------|
| Unacceptable Risks | [n] | [n] | [%] |
| ALARP Risks | [n] | [n] | [n/a or %] |
| Acceptable Risks | [n] | [n] | [+n] |

## 4. Overall Residual Risk Evaluation

### 4.1 Aggregate Residual Risk Assessment

This section evaluates the overall residual risk by considering all individual residual risks together.

**Individual Residual Risks Considered:**

| Situation | Residual Level | Category | Cumulative Impact |
|-----------|----------------|----------|-------------------|
| HS-001 | Acceptable | [category] | Low |
| HS-002 | ALARP | [category] | Moderate |
| ... | ... | ... | ... |

**Cumulative Risk Assessment:**
- Number of residual risks: [n]
- Distribution across categories: [summary]
- Potential for combined effects: [assessment]
- Patient exposure frequency: [assessment]

### 4.2 Benefits of Intended Use

**Clinical Benefits:**
1. [Benefit 1 - quantified if possible]
2. [Benefit 2]
3. [Benefit 3]

**Evidence Sources:**
- Clinical evaluation: [reference]
- Literature review: [reference]
- User studies: [reference]

### 4.3 Benefit-Risk Comparison

| Consideration | Assessment |
|---------------|------------|
| Clinical benefits to patient | [High/Moderate/Low] |
| Aggregate residual risk | [High/Moderate/Low] |
| Availability of alternatives | [description] |
| Risk vs. no treatment | [comparison] |

**Benefit-Risk Conclusion:**
[Detailed statement comparing benefits against residual risks]

### 4.4 Overall Residual Risk Acceptability Decision

**Decision:** [Acceptable | Not Acceptable]

**Rationale:**
1. All individual residual risks are at acceptable or ALARP levels
2. ALARP residual risks have documented benefit-risk justification
3. Clinical benefits outweigh aggregate residual risks
4. [Additional rationale]

**Signatures for Acceptability Decision:**

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Risk Management Lead | | | |
| Clinical Expert | | | |
| Regulatory Affairs | | | |

### 4.5 Residual Risk Disclosure

**Residual Risks Requiring User Disclosure:**

| Risk | Severity | Disclosure Location | Disclosure Text |
|------|----------|---------------------|-----------------|
| [risk description] | [severity] | IFU Section X | [text] |
| ... | ... | ... | ... |

**Disclosure Implementation Status:**

| Location | Status | Verification |
|----------|--------|--------------|
| Instructions for Use | [Implemented/Pending] | [reference] |
| Quick Reference Guide | [Implemented/Pending] | [reference] |
| Training Materials | [Implemented/Pending] | [reference] |

## 5. Production and Post-Production Activities

### 5.1 Information Collection System

**System Established:** [Yes/No]

| Information Source | Collection Method | Responsible |
|--------------------|-------------------|-------------|
| Customer complaints | [method] | [role] |
| Field service reports | [method] | [role] |
| Adverse event reports | [method] | [role] |
| Production quality data | [method] | [role] |
| User feedback | [method] | [role] |
| Scientific literature | [method] | [role] |

### 5.2 Information Review Process

| Element | Status | Evidence |
|---------|--------|----------|
| Review procedure documented | ✓ | [SOP reference] |
| Review frequency defined | ✓ | [frequency] |
| Review criteria established | ✓ | [criteria reference] |
| Responsible personnel assigned | ✓ | [role/name] |

### 5.3 Action Triggers

The following conditions will trigger risk management file review:

| Trigger | Response | Timeline |
|---------|----------|----------|
| Safety-related complaint | Immediate review | Within 24 hours |
| Adverse event report | Immediate review | Within 24 hours |
| Production trend indicating safety issue | Scheduled review | Within 1 week |
| Design change | Pre-implementation review | Before release |
| Annual review cycle | Comprehensive review | Annual |
| Regulatory/standard update | Gap analysis | Within 30 days |

### 5.4 Post-Production Monitoring Confirmation

| Requirement | Status | Evidence |
|-------------|--------|----------|
| Complaint handling procedure | In place | [SOP reference] |
| Vigilance reporting procedure | In place | [SOP reference] |
| CAPA process | In place | [SOP reference] |
| Management review process | In place | [SOP reference] |

## 6. Outstanding Issues and Action Items

### 6.1 Open Gaps

| Gap ID | Description | Severity | Owner | Due Date | Status |
|--------|-------------|----------|-------|----------|--------|
| [GAP-001] | [description] | [High/Med/Low] | [name] | [date] | [status] |
| ... | ... | ... | ... | ... | ... |

**Assessment:** [None / Gaps do not prevent release / Gaps prevent release]

### 6.2 Action Items for Post-Release

| Action | Owner | Due Date | Tracking |
|--------|-------|----------|----------|
| [action description] | [name] | [date] | [ticket/ref] |
| ... | ... | ... | ... |

## 7. Conclusions and Recommendations

### 7.1 Risk Management Plan Implementation
The Risk Management Plan has been [fully implemented / implemented with noted deviations]. All planned risk management activities have been [completed / substantially completed with the following exceptions: ...].

### 7.2 Overall Residual Risk
The overall residual risk for [Product Name] is [acceptable / not acceptable].

[For acceptable:] The benefits of the intended use outweigh the residual risks. All significant residual risks have been disclosed to users in the accompanying documentation.

[For not acceptable:] The following actions are required before release: [list actions]

### 7.3 Post-Production Readiness
Appropriate methods are in place to collect and review production and post-production information. The organization is prepared to take appropriate action based on post-production findings.

### 7.4 Release Recommendation

**Recommendation:** [Approved for Release | Conditional Approval | Not Approved]

**Conditions (if applicable):**
1. [condition 1]
2. [condition 2]

**Statement:**
Based on the review of risk management activities documented in this report, [Product Name] version [version] is [recommended / not recommended] for commercial release from a risk management perspective.

## 8. Approval

### 8.1 Report Approval

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Risk Management Lead | | | |
| Quality Assurance | | | |
| Regulatory Affairs | | | |

### 8.2 Release Authorization

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Top Management / Authorized Representative | | | |

---

## Appendix A: Risk Management File Contents

| Document | Version | Date | Status |
|----------|---------|------|--------|
| Risk Management Plan | [ver] | [date] | Approved |
| Risk Analysis | [ver] | [date] | Approved |
| Verification Records | [ver] | [date] | Complete |
| Risk Management Report (this document) | [ver] | [date] | [status] |

## Appendix B: Extraction Metadata

| Attribute | Value |
|-----------|-------|
| Repository | [repo-url] |
| Commit | [hash] |
| Extraction Date | [timestamp] |
| Standard | ISO 14971:2019 |

## Appendix C: Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | [date] | [author] | Initial report |
| ... | ... | ... | ... |
```

## Compliance Mapping

| Document Section | ISO 14971 Clause | Evidence Provided |
|------------------|------------------|-------------------|
| Section 2 | 9 a) | Plan appropriately implemented |
| Section 4.4 | 9 b) | Overall residual risk acceptable |
| Section 5 | 9 c) | Methods in place for post-production |
| Section 4.2 | 8 | Benefit-risk analysis |
| Section 4.5 | 8 | Residual risk disclosure |
| Appendix A | 4.5 | Risk management file contents |

## Examples

### Input: risks.json Summary Data

```json
{
  "extraction_metadata": {
    "repository": "github.com/example/vital-monitor",
    "commit": "abc123def",
    "extracted_at": "2024-01-15T10:30:00Z"
  },
  "hazards": [
    {"hazard_id": "HAZ-001", ...},
    {"hazard_id": "HAZ-002", ...}
  ],
  "hazardous_situations": [
    {"situation_id": "HS-001", "initial_risk_level": "ALARP", ...},
    {"situation_id": "HS-002", "initial_risk_level": "acceptable", ...}
  ],
  "risk_controls": [
    {"control_id": "RC-001", "verification": {"verified": true}, ...}
  ],
  "residual_risks": [
    {"residual_id": "RR-001", "acceptability": "acceptable", ...}
  ],
  "gaps": []
}
```

### Output: Generated Executive Summary

```markdown
## Executive Summary

### Product
| Attribute | Value |
|-----------|-------|
| Product Name | Vital Signs Monitor |
| Version | 2.0.0 |
| Regulatory Class | MDR Class IIb |
| Report Date | 2024-01-20 |

### Key Findings

| Metric | Count |
|--------|-------|
| Total Hazards Identified | 2 |
| Total Hazardous Situations | 2 |
| Risk Controls Implemented | 1 |
| Residual Risks (Acceptable) | 2 |
| Residual Risks (ALARP with B-R) | 0 |
| Residual Risks (Unacceptable) | 0 |
| Outstanding Gaps | 0 |

### Conclusion

**Overall Residual Risk:** Acceptable

**Release Recommendation:** Recommended for release

**Summary Statement:**
All identified hazards have been analyzed and controlled. Residual risks are acceptable and benefits of intended use outweigh remaining risks. The product is ready for commercial release from a risk management perspective.
```

## Validation Criteria

- [ ] All Risk Management Plan activities confirmed complete 🆔 taFUv4
- [ ] Risk analysis summary includes counts for all categories 🆔 o3039c
- [ ] All risk controls have verification status 🆔 jORZ1A
- [ ] Residual risk distribution shows before/after comparison 🆔 2ix7Tz
- [ ] Overall residual risk has explicit acceptability decision 🆔 hWIW3S
- [ ] Benefit-risk analysis documented for ALARP residual risks 🆔 9SKeb2
- [ ] Residual risk disclosures identified and implemented 🆔 YgYzLF
- [ ] Post-production monitoring system confirmed 🆔 nMJuMl
- [ ] Action triggers and response procedures defined 🆔 aiLm5p
- [ ] Outstanding gaps assessed for release impact 🆔 fcwHHe
- [ ] Approval signatures section included 🆔 yBWQ2l
- [ ] Release recommendation stated clearly 🆔 kEpNKS
