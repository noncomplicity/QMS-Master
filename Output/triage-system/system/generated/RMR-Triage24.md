---
id:
title: "Risk Management Report - Triage24"
version:
author:
effective_date:
type: "Report"
document_id: "RMR-Triage24-1.0"
process: "[Risk Management Process](../../Canvases/Risk%20Management%20Process.canvas)"
requirements: "[ISO 14971](../../Requirements/ISO_14971_Requirements.md)"
owner: "[Head of Risk Management](../../Assets/Head%20of%20Risk%20Management.md)"
---

# Risk Management Report - Triage24

## Executive Summary

### Product

| Attribute | Value |
|-----------|-------|
| Product Name | Triage24 |
| Version | 1.0 |
| Regulatory Class | MDR Class IIa (Rule 11) |
| Software Safety Class | IEC 62304 Class B |
| Report Date | 2026-03-06 |
| Basic UDI-DI | 735012722P24001LR |

### Key Findings

| Metric | Count |
|--------|-------|
| Total System Hazards Identified | 8 |
| Total Module Hazards | 33 |
| Total Failure Modes | 40 |
| Risk Controls Implemented | 14 (system) + 38 (module) |
| Verification Gaps (Critical) | 2 |
| Verification Gaps (High) | 3 |
| Verification Gaps (Medium) | 1 |

### Hazard Severity Distribution

| Severity | Count |
|----------|-------|
| Catastrophic | 1 |
| Critical | 5 |
| Serious | 2 |
| Minor | 0 |

### Conclusion

**Overall Residual Risk:** Conditional - Requires completion of blocking items

**Release Recommendation:** Not recommended until blocking items resolved

**Summary Statement:**
Risk management activities have been substantially completed with comprehensive hazard identification across 7 software items and 4 modules. However, 2 critical verification gaps and incomplete clinical hazard assessments prevent final acceptability determination. Resolution of blocking items required before release recommendation can be issued.

---

## 1. Purpose and Scope

### 1.1 Purpose

This Risk Management Report summarizes the execution of the Risk Management Plan for Triage24 and provides evidence that:

- The Risk Management Plan has been appropriately implemented
- Risk analysis has been performed at item, module, and system levels
- Risk controls have been identified and implementation status documented
- Outstanding items preventing final acceptability determination are identified

### 1.2 Scope

| Element | Status |
|---------|--------|
| Risk Management Plan | RMP-Triage24-1.0, Draft |
| Risk Analysis | Complete (code-based extraction) |
| Clinical Assessments | Awaiting clinical input |
| Product Version | 1.0 |
| Lifecycle Phase | Pre-release review |

### 1.3 Referenced Documents

| Document | Version | Location |
|----------|---------|----------|
| Risk Management Plan | 1.0 | system/generated/RMP-Triage24.md |
| System Risk File | 1.0 | system/aggregated/system-risk-file.json |
| Intended Use | 1.0 | system/defined/intended-use.json |
| Safety Characteristics | 1.0 | system/defined/safety-characteristics.json |
| Risk Criteria | 1.0 | system/defined/risk-criteria.json |
| Hazard Assessments | Draft | system/defined/hazard-assessments.json |

## 2. Review of Risk Management Plan Implementation

### 2.1 Planned vs. Completed Activities

| Planned Activity (from RMP) | Status | Evidence |
|----------------------------|--------|----------|
| Intended use analysis | Complete | intended-use.json |
| Safety characteristic assessment | Complete | safety-characteristics.json (16 characteristics) |
| Hazard identification | Complete | 8 system hazards, 33 module hazards |
| Failure mode identification | Complete | 40 module failure modes |
| Risk control identification | Complete | 14 system controls, 38 module controls |
| Risk control verification | Partial | Verification status documented per control |
| Clinical severity/probability assessment | **Incomplete** | hazard-assessments.json has placeholders |
| Overall residual risk evaluation | **Blocked** | Awaiting clinical assessments |
| Post-production planning | Complete | RMP Section 8 |

### 2.2 Deviations from Plan

| Deviation | Rationale | Impact |
|-----------|-----------|--------|
| Code-based extraction used for hazard identification | Enables systematic, traceable identification from implemented behavior | Positive - ensures documentation matches code |
| Clinical assessments not yet completed | Requires clinical risk management expertise | Blocks final acceptability determination |

### 2.3 Risk Management Team Confirmation

| Role | Confirmation |
|------|--------------|
| Risk Management Lead | Activities substantially completed per plan |
| Design Engineering | Controls identified and mapped to code |
| Quality Assurance | Verification status documented |
| Clinical Expert | **Pending** - Clinical assessments required |

## 3. Summary of Risk Analysis Results

### 3.1 System Hazard Summary

**Total System Hazards Identified:** 8

| ID | Description | Severity | Modules |
|----|-------------|----------|---------|
| SYS-HAZ-001 | Incorrect triage recommendation | Critical | triage-engine, user-interfaces |
| SYS-HAZ-002 | Wrong patient data displayed | **Catastrophic** | user-interfaces |
| SYS-HAZ-003 | Clinical rules fail to load | Critical | triage-engine, infrastructure |
| SYS-HAZ-004 | Urgent condition not identified | Critical | triage-engine, user-interfaces |
| SYS-HAZ-005 | Wrong patient executes action | Critical | actions-module |
| SYS-HAZ-006 | SQL injection compromises database | Critical | infrastructure |
| SYS-HAZ-007 | Interview data lost or corrupted | Serious | triage-engine |
| SYS-HAZ-008 | Consent not properly recorded | Serious | user-interfaces |

### 3.2 Module Hazard Summary

| Module | Hazards | Failure Modes | Controls |
|--------|---------|---------------|----------|
| Triage Engine | 8 | 11 | 10 |
| Actions Module | 8 | 9 | 9 |
| Infrastructure | 6 | 8 | 9 |
| User Interfaces | 11 | 12 | 10 |
| **Total** | **33** | **40** | **38** |

### 3.3 Failure Mode Categories

| Category | Count | Percentage |
|----------|-------|------------|
| software_defect | 6 | 15% |
| data_integrity | 7 | 17.5% |
| communication_failure | 4 | 10% |
| external_dependency | 6 | 15% |
| authentication_failure | 3 | 7.5% |
| configuration_error | 5 | 12.5% |
| synchronization_error | 4 | 10% |
| user_error | 3 | 7.5% |
| usability_issue | 1 | 2.5% |
| security_breach | 1 | 2.5% |

### 3.4 Risk Control Summary

**Total System Risk Controls:** 14

| Control Type | Count | Percentage |
|--------------|-------|------------|
| Inherent Safety by Design | 3 | 21% |
| Protective Measures | 9 | 64% |
| Information for Safety | 2 | 14% |

**Verification Status (Module Controls):**

| Status | Count |
|--------|-------|
| Verified | 21 |
| Partial | 12 |
| Unverified | 5 |

### 3.5 Safety Characteristic Coverage

**Applicable Safety Characteristics (ISO 14971 Annex C):** 7 of 16 assessed

| Characteristic | Annex C Ref | Status |
|----------------|-------------|--------|
| Intended use definition | C.2.1 | Assessed |
| Interpretive device | C.2.12 | Applicable - triage interpretation |
| Software incorporated | C.2.19 | Applicable - SaMD |
| Training required | C.2.26 | Applicable - practitioner training |
| Human factors critical | C.2.28 | Applicable - patient data entry |
| Misuse susceptibility | C.2.30 | Applicable - false symptoms |
| Critical data stored | C.2.31 | Applicable - clinical data |
| Essential performance | C.2.33 | Applicable - triage accuracy |

## 4. Overall Residual Risk Evaluation

### 4.1 Aggregate Residual Risk Assessment

**Status:** Cannot complete - clinical assessments pending

The following elements are required to complete residual risk evaluation:

1. Severity level assignment for each system hazard (S1-S5)
2. Probability level assignment for each system hazard (P1-P5)
3. Risk level determination (Low/Medium/High)
4. Benefit-risk justification for Medium risks

### 4.2 Benefits of Intended Use

**Clinical Benefits:**

1. **Efficient triage** - Patients receive appropriate level of care recommendation based on symptoms
2. **Reduced wait times** - Automated triage reduces burden on healthcare providers
3. **Consistent assessment** - Standardized clinical rules ensure consistent triage decisions
4. **24/7 availability** - Patients can access triage services at any time
5. **Self-care guidance** - Low-urgency patients receive self-care advice, reducing unnecessary visits

**Evidence Sources:**

- Clinical evaluation: D095 Product Description
- Intended use analysis: D490
- User intent documentation: intended-use.json

### 4.3 Benefit-Risk Comparison (Preliminary)

| Consideration | Assessment |
|---------------|------------|
| Clinical benefits to patient | High - efficient care routing |
| Aggregate residual risk | **Cannot assess** - clinical input needed |
| Availability of alternatives | Manual triage, telephone triage |
| Risk vs. no treatment | Triage improves care access vs. untriaged patients |

### 4.4 Risk Mitigation Through Contraindications

The following contraindications significantly reduce residual risk by excluding high-risk populations:

1. **Life-threatening symptoms** - Directed to call 112 immediately
2. **Cognitively impaired users** - Cannot use system correctly
3. **Insufficient language proficiency** - Cannot describe symptoms accurately

**Warning Implementation:**
- "Vid livshotande eller akuta besvär - ring 112" prominently displayed
- Alarming exits lock interview and direct to emergency care

### 4.5 Residual Risk Disclosure Requirements

**Residual Risks Requiring User Disclosure:**

| Risk | Severity | Disclosure Location | Implementation |
|------|----------|---------------------|----------------|
| System cannot diagnose | N/A | IFU | Device does not diagnose |
| Practitioner must use own assessment | Serious | IFU, UI | Warning text implemented |
| Life-threatening symptoms excluded | Critical | Contraindications | Warning text implemented |
| Language proficiency required | Serious | Contraindications | Documented |

## 5. Production and Post-Production Activities

### 5.1 Information Collection System

**System Established:** Yes (per Risk Management Procedure)

| Information Source | Collection Method | Responsible |
|--------------------|-------------------|-------------|
| Customer complaints | Complaint handling system | Customer Support |
| Field service reports | Service management | Operations |
| Adverse event reports | Vigilance system | Regulatory Affairs |
| Production quality data | QMS metrics | QA |
| User feedback | App feedback, surveys | Product Management |
| Scientific literature | Literature review | Clinical Team |
| Medical content issues | Content review process | Medical Content Team |

### 5.2 Information Review Process

| Element | Status | Evidence |
|---------|--------|----------|
| Review procedure documented | Yes | Risk Management Procedure |
| Review frequency defined | Yes | See RMP Section 8.2 |
| Review criteria established | Yes | Trigger-based review |
| Responsible personnel assigned | Yes | See RMP Section 4 |

### 5.3 Action Triggers

| Trigger | Response | Timeline |
|---------|----------|----------|
| Safety-related complaint | Immediate review | Within 24 hours |
| Adverse event report | Immediate review | Within 24 hours |
| Medical content defect | Emergency review | Within 24 hours |
| Production trend | Scheduled review | Within 1 week |
| Design change | Pre-implementation | Before release |

### 5.4 Post-Production Monitoring Confirmation

| Requirement | Status | Evidence |
|-------------|--------|----------|
| Complaint handling procedure | In place | QMS procedure |
| Vigilance reporting procedure | In place | QMS procedure |
| CAPA process | In place | QMS procedure |
| PMS plan | In place | PMS procedure |
| Medical content review process | In place | Content Studio process |

## 6. Outstanding Issues and Action Items

### 6.1 Critical Gaps (Blocking Release)

| Gap ID | Description | Owner | Status |
|--------|-------------|-------|--------|
| SYS-GAP-001 | SQL injection protection (escapeString) not verified | Security Team | **Critical - requires security testing** |
| SYS-GAP-002 | Patient-ui test coverage at 7.7% (52/671+ files) | Frontend Team | **Critical - requires test development** |
| CLINICAL-001 | Hazard severity/probability assessments incomplete | Clinical Risk Team | **Blocking - requires clinical input** |

### 6.2 High Priority Gaps

| Gap ID | Description | Owner | Due Date |
|--------|-------------|-------|----------|
| SYS-GAP-003 | No integration test for interview-to-rule-engine data integrity | Backend Team | TBD |
| SYS-GAP-004 | Urgency indicator visibility not usability tested | UX Team | TBD |
| SYS-GAP-005 | Rule package interaction testing incomplete | QA Team | TBD |

### 6.3 Assessment

**Gaps Prevent Release:** Yes

The following must be completed before release:

1. Complete clinical hazard assessments (severity/probability for all system hazards)
2. Verify SQL injection protection through security testing
3. Achieve minimum test coverage for safety-critical UI components
4. Complete integration testing for data integrity

## 7. Conclusions and Recommendations

### 7.1 Risk Management Plan Implementation

The Risk Management Plan has been **substantially implemented** with the following status:

- Hazard identification: Complete (code-based extraction)
- Failure mode identification: Complete (40 failure modes across 4 modules)
- Risk control identification: Complete (52 controls mapped)
- Safety characteristic assessment: Complete (16 Annex C characteristics)
- Verification status documentation: Complete (per control)
- Clinical risk assessments: **Incomplete** (requires clinical team)

### 7.2 Overall Residual Risk

The overall residual risk for Triage24 **cannot be determined** until:

1. Clinical severity and probability assessments are completed
2. Critical verification gaps are closed
3. Benefit-risk analysis is finalized with clinical input

**Preliminary Assessment:**
Based on implemented controls and code analysis:

- 1 catastrophic hazard identified (SYS-HAZ-002: wrong patient data) with protective controls
- 5 critical hazards identified with multiple layers of controls
- Contraindications exclude highest-risk populations
- Practitioner oversight provides additional safety layer

### 7.3 Post-Production Readiness

Appropriate methods are in place to collect and review production and post-production information. The organization is prepared to take appropriate action based on post-production findings.

### 7.4 Release Recommendation

**Recommendation:** Not Approved - Actions Required

**Required Actions Before Release:**

1. **Complete clinical hazard assessments** - Fill hazard-assessments.json with clinical severity/probability ratings
2. **Security testing for SQL injection** - Verify escapeString protection in object-storage
3. **Increase patient-ui test coverage** - Target 80%+ for safety-critical modules
4. **Integration testing** - Verify interview-to-rule-engine data integrity
5. **Usability testing** - Verify urgency indicator visibility

**Estimated Effort:** 10-14 hours clinical team + 2-4 weeks development

**Statement:**
Based on the review of risk management activities documented in this report, Triage24 version 1.0 is **not recommended** for commercial release until the blocking items above are resolved. Upon resolution, a revised Risk Management Report shall be issued with final acceptability determination.

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
| Top Management / PRRC | | | |

**Note:** Release authorization pending resolution of blocking items.

---

## Appendix A: Risk Management File Contents

| Document | Version | Date | Status |
|----------|---------|------|--------|
| Risk Management Plan | 1.0 | 2026-03-06 | Draft |
| Intended Use | 1.0 | 2026-03-06 | Complete |
| Safety Characteristics | 1.0 | 2026-03-06 | Complete |
| Risk Criteria | 1.0 | 2026-03-06 | Complete |
| System Risk File | 1.0 | 2026-03-06 | Complete |
| Hazard Assessments | 0.1 | 2026-03-06 | Awaiting clinical input |
| Risk Management Report | 1.0 | 2026-03-06 | Draft - conditional |

## Appendix B: Extraction Metadata

| Attribute | Value |
|-----------|-------|
| System ID | Triage24 |
| Extraction Date | 2026-03-06 |
| Software Items Analyzed | 7 |
| Modules Aggregated | 4 |
| Source Repositories | interviewer4, rule-engine-module, actions, object-storage, patient-ui, triage-ui, triage-clinic-ui |
| Standard | ISO 14971:2019, IEC 62304:2006+AMD1:2015 |

## Appendix C: System Hazard to Control Traceability

| Hazard | Controls | Verification Status |
|--------|----------|---------------------|
| SYS-HAZ-001 | SYS-RC-001, SYS-RC-002, SYS-RC-003 | Partial |
| SYS-HAZ-002 | SYS-RC-004, SYS-RC-005 | Verified |
| SYS-HAZ-003 | SYS-RC-006, SYS-RC-007 | Verified |
| SYS-HAZ-004 | SYS-RC-001, SYS-RC-008 | Partial |
| SYS-HAZ-005 | SYS-RC-004, SYS-RC-009 | Verified |
| SYS-HAZ-006 | SYS-RC-010, SYS-RC-011 | **Unverified** |
| SYS-HAZ-007 | SYS-RC-012, SYS-RC-013 | Partial |
| SYS-HAZ-008 | SYS-RC-014 | Partial |

## Appendix D: Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | 2026-03-06 | Generated from code analysis | Initial report - conditional pending clinical input |
