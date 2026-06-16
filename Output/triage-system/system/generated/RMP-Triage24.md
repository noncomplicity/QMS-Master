---
id:
title: "Risk Management Plan - Triage24"
version:
author:
effective_date:
type: "Plan"
document_id: "RMP-Triage24-1.0"
regulatory_class: "MDR Class IIa"
process: "[Risk Management Process](../../Canvases/Risk%20Management%20Process.canvas)"
requirements: "[ISO 14971](../../Requirements/ISO_14971_Requirements.md)"
owner: "[Head of Risk Management](../../Assets/Head%20of%20Risk%20Management.md)"
---

# Risk Management Plan - Triage24

## 1. Purpose and Scope

### 1.1 Purpose

This Risk Management Plan establishes the framework for systematic identification, evaluation, control, and monitoring of risks associated with Triage24 throughout its lifecycle, in accordance with ISO 14971:2019.

### 1.2 Product Identification

| Attribute | Value |
|-----------|-------|
| Product Name | Triage24 |
| Product Version | 1.0 |
| Regulatory Classification | MDR Class IIa (Rule 11) |
| Basic UDI-DI | 735012722P24001LR |
| Software Safety Class | IEC 62304 Class B |
| Intended Use | Provide patients with a recommendation of an appropriate level of care, based on urgency of symptoms reported by the user |

### 1.3 Scope of Risk Management Activities

This plan covers the following lifecycle phases:

- [x] Design and Development
- [x] Verification and Validation
- [x] Production
- [x] Post-Production (including post-market surveillance)
- [ ] Decommissioning 🆔 ZoNNUu

### 1.4 Boundaries

**Included:**
- Triage Engine module (interviewer4, rule-engine-module)
- Actions Module (actions service)
- Infrastructure module (object-storage)
- User Interfaces module (patient-ui, triage-ui, triage-clinic-ui)
- Patient triage workflow
- Assisted triage workflow
- Medical content configuration

**Excluded:**
- Third-party integrations beyond defined interfaces (health-manager, bookings, directory2)
- Network infrastructure managed by deployment environment
- Patient devices (browsers, smartphones)
- Clinical outcomes of treatment decisions made by practitioners

## 2. Referenced Documents

| Document | Version | Relationship |
|----------|---------|--------------|
| ISO 14971:2019 | 3rd Edition | Governing standard |
| IEC 62304:2006+AMD1:2015 | - | Software lifecycle standard |
| IEC 82304-1:2016 | - | Health software product standard |
| EU MDR 2017/745 | - | Regulatory framework |
| D490 - Intended Use Statement | 1.0 | Use context |
| D095 - Product Description | 1.0 | Product definition |
| Risk Management Procedure | 1.0 | Process definition |
| D1884 - Policy for Establishing Criteria for Risk Acceptability | 1.0 | Acceptability criteria |

## 3. Definitions and Abbreviations

| Term | Definition |
|------|------------|
| ALARP | As Low As Reasonably Practicable |
| Harm | Injury or damage to the health of people |
| Hazard | Potential source of harm |
| Hazardous Situation | Circumstance in which people are exposed to one or more hazards |
| Residual Risk | Risk remaining after risk control measures have been implemented |
| Risk | Combination of probability of occurrence of harm and severity of that harm |
| Risk Control | Process in which decisions are made and measures implemented to reduce or maintain risk |
| Severity | Measure of the possible consequences of a hazard |
| SaMD | Software as a Medical Device |
| Triage | Process of determining the priority of patients' treatments based on severity |

## 4. Responsibilities and Authorities

### 4.1 Risk Management Team

| Role | Responsibilities |
|------|------------------|
| Risk Management Lead | Overall risk management activities, risk file maintenance, residual risk evaluation |
| Design Engineering | Hazard identification, risk control implementation, software verification |
| Quality Assurance | Risk control verification, compliance review, document control |
| Clinical Expert | Clinical harm assessment, severity evaluation, benefit-risk analysis |
| Regulatory Affairs | Regulatory acceptability review, MDR compliance |

### 4.2 Approval Authorities

| Activity | Authority | Required Evidence |
|----------|-----------|-------------------|
| Risk Management Plan approval | Risk Management Lead + QA | Signed RMP |
| Risk acceptability decisions | Risk Management Lead | Risk evaluation records |
| Benefit-risk decisions | Clinical Expert + Risk Lead | B-R analysis documentation |
| Risk Management Report approval | Risk Management Lead + QA + RA | Signed RMR |
| Release for commercial distribution | CTO, CMO, CPO, PRRC | Completed RMR, release decision |

### 4.3 Competency Requirements

Personnel performing risk management activities shall have:

- Knowledge of the Triage24 system and its intended use
- Understanding of applicable standards (ISO 14971, IEC 62304, IEC 82304-1)
- Training in risk management techniques (FMEA, fault tree analysis)
- Training in medical device software development
- Understanding of clinical triage processes (for clinical experts)

## 5. Risk Acceptability Criteria

### 5.1 Severity Categories

| Level | Category | Description | Examples |
|-------|----------|-------------|----------|
| S1 | Negligible | Inconvenience or temporary discomfort | Minor delay in triage, UI annoyance |
| S2 | Minor | Temporary injury not requiring medical intervention | Patient anxiety, minor delay not affecting outcome |
| S3 | Serious | Injury requiring medical intervention | Delayed treatment requiring intervention but recoverable |
| S4 | Critical | Permanent impairment or life-threatening | Urgent condition missed, delayed critical care |
| S5 | Catastrophic | Death or permanent life-altering condition | Patient death due to delayed emergency care |

### 5.2 Probability Categories

| Level | Category | Range | Description |
|-------|----------|-------|-------------|
| P1 | Incredible | <10^-6 | Multiple fail-safes required to all fail |
| P2 | Remote | 10^-6 to 10^-4 | Very unlikely, no known occurrences |
| P3 | Occasional | 10^-4 to 10^-2 | May occur several times during device lifetime |
| P4 | Probable | 10^-2 to 10^-1 | Likely to occur during device operation |
| P5 | Frequent | >10^-1 | Expected to occur frequently |

### 5.3 Risk Acceptability Matrix

|  | P1 Incredible | P2 Remote | P3 Occasional | P4 Probable | P5 Frequent |
|--|---------------|-----------|---------------|-------------|-------------|
| **S5 Catastrophic** | Medium | Medium | High | High | High |
| **S4 Critical** | Low | Medium | Medium | High | High |
| **S3 Serious** | Low | Low | Medium | Medium | High |
| **S2 Minor** | Low | Low | Low | Medium | Medium |
| **S1 Negligible** | Low | Low | Low | Low | Low |

**Legend:**
- **Low** — Risk is acceptable with documented rationale
- **Medium** — Risk acceptable when reduced ALARP with benefit-risk justification
- **High** — Risk must be reduced; product cannot be released with this risk level

### 5.4 Criteria When Probability Cannot Be Estimated

When probability of occurrence cannot be estimated:

1. Document the consequences (potential harms)
2. Apply worst-case probability assumption (P4 Probable) unless evidence supports lower
3. Implement risk controls to reduce severity or provide evidence for lower probability
4. Document rationale in risk evaluation

### 5.5 Overall Residual Risk Acceptability

The overall residual risk shall be evaluated by:

1. Reviewing all individual residual risks
2. Considering cumulative effects of multiple residual risks
3. Comparing aggregate risk against product benefits
4. Documenting acceptability decision in Risk Management Report

**Criteria for acceptable overall residual risk:**

- No individual residual risk is classified as "High"
- Medium residual risks have documented benefit-risk analysis
- Benefits of intended use outweigh aggregate residual risk
- Significant residual risks are disclosed to users
- Contraindications clearly defined and communicated

### 5.6 Triage24-Specific Considerations

**Contraindications as Risk Control:**
The following contraindications serve as inherent safety controls by excluding populations where device use could lead to harm:

- Patients with life-threatening symptoms, serious conditions or trauma
- Patients physically or cognitively unable to use digital technology
- Patients with cognitive issues that prevent correct technology use
- Patients lacking sufficient language proficiency (conversational variant)

**Practitioner Override:**
For assisted triage, the practitioner's own clinical assessment must be used together with the triage result. This is documented as an information_for_safety control.

## 6. Risk Management Activities

### 6.1 Risk Analysis Activities

| Activity | Method | Responsible | Output |
|----------|--------|-------------|--------|
| Intended use analysis | Document review, code analysis | Design Lead | Intended use documentation (D490) |
| Safety characteristic assessment | ISO 14971 Annex C analysis | Risk Team | Safety characteristics assessment |
| Hazard identification | Code pattern analysis, FMEA | Risk Team | Hazard list per module |
| Hazardous situation identification | Fault tree, use case analysis | Risk Team | System hazard list |
| Risk estimation | Matrix method, expert judgment | Risk Team | Risk estimates |

### 6.2 Risk Evaluation Activities

| Activity | Method | Responsible | Output |
|----------|--------|-------------|--------|
| Individual risk evaluation | Compare to acceptability matrix | Risk Lead | Evaluation records |
| ALARP determination | Practicability analysis | Risk Team | ALARP justification |
| Benefit-risk analysis | Structured comparison | Clinical + Risk Lead | B-R documentation |

### 6.3 Risk Control Activities

| Activity | Method | Responsible | Output |
|----------|--------|-------------|--------|
| Control option analysis | Priority order per 7.1 | Design Lead | Selected controls |
| Control implementation | Design changes, code changes | Development | Implemented controls |
| Control verification | Testing, code review | QA | Verification records |
| Residual risk evaluation | Post-control assessment | Risk Team | Residual risk records |

### 6.4 Risk Control Priority Order

Risk controls shall be considered in this priority order (ISO 14971 clause 7.1):

1. **Inherent safety by design** — Eliminate hazard or reduce risk through design
   - Input validation, type-safe interfaces, mandatory fields
   - Interview locking on alarming exit
   - Contraindications excluding high-risk populations

2. **Protective measures** — Software mechanisms that detect or correct errors
   - Data integrity checks, transaction management
   - Patient identity verification
   - Rule package validation
   - Access control

3. **Information for safety** — Warnings, instructions, training
   - "Vid livshotande eller akuta besvär - ring 112" warnings
   - Practitioner training requirements
   - Medical content developer training (D361, D1458)

## 7. Verification of Risk Control Measures

### 7.1 Verification Methods

| Control Type | Verification Method | Evidence Required |
|--------------|---------------------|-------------------|
| Inherent safety (code) | Code review, static analysis | Review records |
| Protective measures (unit) | Unit tests | Test reports with pass/fail |
| Protective measures (integration) | Integration tests | Test reports |
| Protective measures (system) | System testing | System test reports |
| Information for safety | Labeling review, usability testing | Review/test records |

### 7.2 Effectiveness Criteria

Risk control measures are considered effective when:

- Implementation verification confirms correct implementation
- Testing demonstrates control functions as specified
- Residual risk is reduced to acceptable or ALARP level
- No new hazards introduced (or new hazards are controlled)
- Reduction factor can be justified based on verification evidence

### 7.3 Verification Status Definitions

| Status | Definition |
|--------|------------|
| Verified | Control implemented and verified through testing/review |
| Partial | Control implemented, verification incomplete or limited |
| Unverified | Control implemented but not yet verified |

## 8. Production and Post-Production Activities

### 8.1 Information Collection

The following information shall be collected and reviewed for risk management relevance:

| Source | Collection Method | Responsible |
|--------|-------------------|-------------|
| Customer complaints | Complaint handling system | Customer Support |
| Field service reports | Service management system | Operations |
| Vigilance/adverse event reports | Vigilance reporting system | Regulatory Affairs |
| Production quality data | Quality management system | QA |
| User feedback | Feedback mechanisms | Product Management |
| Scientific literature | Periodic literature review | Clinical Expert |
| Post-market surveillance | PMS procedure | QA/RA |

### 8.2 Review Triggers

Risk management file shall be reviewed when:

| Trigger | Response | Timeline |
|---------|----------|----------|
| Safety-related complaint | Immediate review | Within 24 hours |
| Adverse event report | Immediate review | Within 24 hours |
| Production trend indicating safety issue | Scheduled review | Within 1 week |
| Design change | Pre-implementation review | Before release |
| Medical content change | Content review process | Before deployment |
| New hazard information | Gap analysis | Within 30 days |
| Annual review cycle | Comprehensive review | Annual |
| Regulatory/standard update | Gap analysis | Within 30 days |

### 8.3 Action Process

When post-production information indicates:

- **Previously unrecognized hazard** → Update risk analysis, implement controls
- **Risk no longer acceptable** → Implement additional controls, consider recall
- **State of the art changed** → Review existing controls, update as needed
- **Overall residual risk changed** → Update benefit-risk analysis
- **Medical content defect** → Emergency content update, notification

## 9. Risk Management File Contents

The Risk Management File shall contain:

| Document | Status | Location |
|----------|--------|----------|
| Risk Management Plan (this document) | Draft | system/generated/RMP-Triage24.md |
| Intended Use Statement | Complete | system/defined/intended-use.json |
| Safety Characteristics Assessment | Complete | system/defined/safety-characteristics.json |
| Risk Acceptability Criteria | Complete | system/defined/risk-criteria.json |
| Item Risk Contributions (7 items) | Complete | items/*/extracted/item-risk-contribution.json |
| Module Risk Aggregations (4 modules) | Complete | modules/*/aggregated/module-risk-aggregation.json |
| System Risk File | Complete | system/aggregated/system-risk-file.json |
| Hazard Assessments | Awaiting clinical input | system/defined/hazard-assessments.json |
| Risk Management Report | Draft | system/generated/RMR-Triage24.md |

## 10. Plan Maintenance

### 10.1 Review Schedule

This plan shall be reviewed:

- At start of each development phase
- When significant design changes occur
- Annually during production/post-production
- When regulatory requirements change
- When medical content significantly changes

### 10.2 Change History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | 2026-03-06 | Generated from code analysis | Initial plan based on code-as-truth extraction |

## 11. Approval

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Risk Management Lead | | | |
| Quality Assurance | | | |
| Regulatory Affairs | | | |
| Top Management | | | |

---

## Appendix A: System Architecture Overview

Triage24 consists of 4 modules and 7 software items:

| Module | Items | Safety Class |
|--------|-------|--------------|
| Triage Engine | interviewer4, rule-engine-module | B |
| Actions Module | actions | B |
| Infrastructure | object-storage | A |
| User Interfaces | patient-ui, triage-ui, triage-clinic-ui | B |

## Appendix B: Extraction Metadata

| Attribute | Value |
|-----------|-------|
| System ID | Triage24 |
| Extraction Date | 2026-03-06 |
| Items Analyzed | 7 |
| Modules Aggregated | 4 |
| Standard | ISO 14971:2019, IEC 62304:2006+AMD1:2015 |
| Generated By | Code-as-truth documentation framework |

## Appendix C: Compliance Mapping

| Document Section | ISO 14971 Clause | Evidence Provided |
|------------------|------------------|-------------------|
| Section 1 | 4.4 a) | Scope of planned activities |
| Section 4 | 4.4 b) | Assignment of responsibilities |
| Section 6, 7 | 4.4 c) | Requirements for review of activities |
| Section 5 | 4.4 d) | Criteria for risk acceptability |
| Section 5.5 | 4.4 e) | Method to evaluate overall residual risk |
| Section 7 | 4.4 f) | Verification activities for risk controls |
| Section 8 | 4.4 g) | Production/post-production activities |
| Section 4.3 | 4.3 | Competence of personnel |
| Section 9 | 4.5 | Risk management file contents |
