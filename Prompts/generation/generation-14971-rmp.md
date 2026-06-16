---
id: 725af57
title: "generation 14971 rmp"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Prompt"
category: "generation"
standard: "ISO 14971"
clause: "4.4"
inputs: ["risks.json (from extraction)", "intended use documentation", "product classification"]
outputs: ["Risk Management Plan document"]
software_class: "all"
process: "[Risk Management Process](../../Canvases/Risk%20Management%20Process.canvas)"
requirements: "[ISO 14971 Risk management for medical devices](../../Requirements/ISO_14971_Requirements.md)"
owner: "[Head of Risk Management](../../Assets/Head%20of%20Risk%20Management.md)"
---

# Generate Risk Management Plan

## Context

The Risk Management Plan (RMP) is a mandatory deliverable per ISO 14971 clause 4.4. It defines the scope, responsibilities, criteria, and activities for risk management before detailed risk analysis begins. This prompt transforms extracted risk data into a compliant RMP document.

## Inputs

### Required
- `risks.json` — Output from [extraction-14971-risks](../extraction/extraction-14971-risks.md)
- Product/software name and version

### Optional (enriches output)
- Intended use documentation
- Regulatory classification (MDR class, FDA product code)
- Quality Management System procedures
- Previous risk management files (for product revisions)

## Instructions

1. **Validate Input Data**
   - Verify risks.json contains `intended_use` and `safety_characteristics`
   - Check for product identification information
   - Confirm extraction metadata is present

2. **Define Scope**
   - Identify product lifecycle phases covered
   - Define boundaries of risk management activities
   - Specify inclusions and exclusions

3. **Establish Risk Acceptability Criteria**
   - Define severity categories (from extraction or standard scale)
   - Define probability categories
   - Create risk acceptability matrix
   - Define ALARP criteria

4. **Assign Responsibilities**
   - Map roles to risk management activities
   - Define review and approval authorities
   - Establish competency requirements

5. **Plan Verification Activities**
   - Define how risk controls will be verified
   - Establish effectiveness criteria
   - Plan post-production monitoring

6. **Apply Document Template**
   - Use standard RMP structure (see Output Schema)
   - Populate frontmatter with correct metadata
   - Include approval signatures section

## Output Schema

Generate a markdown document with this structure:

```markdown
---
id: 725af57
title: "generation 14971 rmp"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Plan"
document_id: "RMP-[product]-[version]"
regulatory_class: "[MDR Class | FDA Class]"
process: "[Risk Management Process](../../Canvases/Risk%20Management%20Process.canvas)"
requirements: "[ISO 14971](../../Requirements/ISO_14971_Requirements.md)"
owner: "[Head of Risk Management](../../Assets/Head%20of%20Risk%20Management.md)"
---

# Risk Management Plan

## 1. Purpose and Scope

### 1.1 Purpose
This Risk Management Plan establishes the framework for systematic identification, evaluation, control, and monitoring of risks associated with [Product Name] throughout its lifecycle.

### 1.2 Product Identification
| Attribute | Value |
|-----------|-------|
| Product Name | [Name] |
| Product Version | [Version] |
| Regulatory Classification | [Class] |
| Intended Use | [Brief intended use statement] |

### 1.3 Scope of Risk Management Activities
This plan covers the following lifecycle phases:
- [ ] Design and Development 🆔 TWtOo3
- [ ] Verification and Validation 🆔 8KaJLS
- [ ] Production 🆔 mtIpgo
- [ ] Post-Production (including post-market surveillance) 🆔 QsuPam
- [ ] Decommissioning 🆔 9s9rDZ

### 1.4 Boundaries
**Included:**
- [List of included components/functions]

**Excluded:**
- [List of excluded items with rationale]

## 2. Referenced Documents

| Document | Version | Relationship |
|----------|---------|--------------|
| ISO 14971:2019 | 3rd Edition | Governing standard |
| [QMS Risk Management Procedure] | x.x | Process definition |
| [System Requirements Specification] | x.x | Parent requirements |
| [Intended Use Statement] | x.x | Use context |
| ... | ... | ... |

## 3. Definitions and Abbreviations

| Term | Definition |
|------|------------|
| ALARP | As Low As Reasonably Practicable |
| Harm | Injury or damage to the health of people, or damage to property or the environment |
| Hazard | Potential source of harm |
| Hazardous Situation | Circumstance in which people, property or environment are exposed to one or more hazards |
| Residual Risk | Risk remaining after risk control measures have been implemented |
| Risk | Combination of probability of occurrence of harm and severity of that harm |
| Risk Control | Process in which decisions are made and measures implemented to reduce or maintain risk within specified levels |
| Severity | Measure of the possible consequences of a hazard |
| ... | ... |

## 4. Responsibilities and Authorities

### 4.1 Risk Management Team

| Role | Name | Responsibilities |
|------|------|------------------|
| Risk Management Lead | [Name] | Overall risk management activities, risk file maintenance |
| Design Engineer | [Name/TBD] | Hazard identification, risk control implementation |
| Quality Assurance | [Name/TBD] | Risk control verification, compliance review |
| Clinical Expert | [Name/TBD] | Clinical harm assessment, severity evaluation |
| Regulatory Affairs | [Name/TBD] | Regulatory acceptability review |

### 4.2 Approval Authorities

| Activity | Authority | Required Evidence |
|----------|-----------|-------------------|
| Risk Management Plan approval | [Role] | Signed RMP |
| Risk acceptability decisions | [Role] | Risk evaluation records |
| Benefit-risk decisions | [Role] | B-R analysis documentation |
| Risk Management Report approval | [Role] | Signed RMR |
| Release for commercial distribution | [Role] | Completed RMR |

### 4.3 Competency Requirements
Personnel performing risk management activities shall have:
- Knowledge of the medical device and its intended use
- Understanding of applicable standards (ISO 14971, IEC 62304)
- Training in risk management techniques
- [Product-specific competencies]

## 5. Risk Acceptability Criteria

### 5.1 Severity Categories

| Level | Category | Description | Examples |
|-------|----------|-------------|----------|
| S1 | Negligible | Inconvenience or temporary discomfort | Minor UI annoyance |
| S2 | Minor | Temporary injury not requiring medical intervention | Skin irritation, temporary confusion |
| S3 | Serious | Injury requiring medical intervention | Incorrect treatment delay, misdiagnosis requiring correction |
| S4 | Critical | Permanent impairment or life-threatening | Permanent injury, organ damage |
| S5 | Catastrophic | Death | Patient death |

### 5.2 Probability Categories

| Level | Category | Description | Quantitative Range |
|-------|----------|-------------|-------------------|
| P1 | Incredible | Essentially impossible | < 10⁻⁶ |
| P2 | Improbable | Very unlikely, no known occurrences | 10⁻⁶ to 10⁻⁴ |
| P3 | Remote | Unlikely but possible | 10⁻⁴ to 10⁻² |
| P4 | Occasional | May occur during product lifetime | 10⁻² to 10⁻¹ |
| P5 | Probable | Expected to occur | > 10⁻¹ |

### 5.3 Risk Acceptability Matrix

|  | P1 Incredible | P2 Improbable | P3 Remote | P4 Occasional | P5 Probable |
|--|---------------|---------------|-----------|---------------|-------------|
| **S5 Catastrophic** | ALARP | Unacceptable | Unacceptable | Unacceptable | Unacceptable |
| **S4 Critical** | Acceptable | ALARP | Unacceptable | Unacceptable | Unacceptable |
| **S3 Serious** | Acceptable | ALARP | ALARP | Unacceptable | Unacceptable |
| **S2 Minor** | Acceptable | Acceptable | ALARP | ALARP | Unacceptable |
| **S1 Negligible** | Acceptable | Acceptable | Acceptable | Acceptable | ALARP |

**Legend:**
- **Acceptable** — Risk is acceptable without further risk control measures
- **ALARP** — Risk must be reduced As Low As Reasonably Practicable; benefit-risk analysis may be required
- **Unacceptable** — Risk must be reduced; product cannot be released with this risk level

### 5.4 Criteria When Probability Cannot Be Estimated

When probability of occurrence cannot be estimated (per ISO 14971 clause 4.4 d):
1. Document the consequences (potential harms)
2. Apply worst-case probability assumption (P5 Probable) unless evidence supports lower probability
3. Implement risk controls to reduce severity or provide evidence for lower probability
4. Document rationale in risk evaluation

### 5.5 Overall Residual Risk Acceptability

The overall residual risk shall be evaluated by:
1. Reviewing all individual residual risks
2. Considering cumulative effects of multiple residual risks
3. Comparing aggregate risk against product benefits
4. Documenting acceptability decision in Risk Management Report

Criteria for acceptable overall residual risk:
- No individual residual risk is classified as "Unacceptable"
- ALARP residual risks have documented benefit-risk analysis
- Benefits of intended use outweigh aggregate residual risk
- Significant residual risks are disclosed to users

## 6. Risk Management Activities

### 6.1 Risk Analysis Activities

| Activity | Method | Responsible | Output |
|----------|--------|-------------|--------|
| Intended use analysis | Document review, code analysis | Design Lead | Intended use documentation |
| Hazard identification | FMEA, code pattern analysis | Risk Team | Hazard list |
| Hazardous situation identification | Fault tree, use case analysis | Risk Team | Situation list |
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
| Control verification | Testing, review | QA | Verification records |
| Residual risk evaluation | Post-control assessment | Risk Team | Residual risk records |

### 6.4 Risk Control Priority Order

Risk controls shall be considered in this priority order (ISO 14971 clause 7.1):
1. **Inherent safety by design** — Eliminate hazard or reduce risk through design
2. **Protective measures** — Guards, interlocks, automatic controls
3. **Information for safety** — Warnings, instructions, training

## 7. Verification of Risk Control Measures

### 7.1 Verification Methods

| Control Type | Verification Method | Evidence Required |
|--------------|---------------------|-------------------|
| Inherent safety | Design review, code review | Review records |
| Protective measures (software) | Unit tests, integration tests | Test reports |
| Protective measures (system) | System testing | Test reports |
| Information for safety | Labeling review, usability testing | Review/test records |

### 7.2 Effectiveness Criteria

Risk control measures are considered effective when:
- Implementation verification confirms correct implementation
- Testing demonstrates control functions as specified
- Residual risk is reduced to acceptable or ALARP level
- No new hazards introduced (or new hazards are controlled)

## 8. Production and Post-Production Activities

### 8.1 Information Collection

The following information shall be collected and reviewed for risk management relevance:
- Customer complaints
- Field service reports
- Vigilance/adverse event reports
- Production quality data
- User feedback
- Scientific literature updates

### 8.2 Review Triggers

Risk management file shall be reviewed when:
- Safety-related complaint received
- Adverse event reported
- Production trend indicates potential safety issue
- Design change implemented
- New hazard information becomes available
- Annual review cycle

### 8.3 Action Process

When post-production information indicates:
- Previously unrecognized hazard → Update risk analysis
- Risk no longer acceptable → Implement additional controls
- State of the art changed → Review existing controls
- Overall residual risk changed → Update benefit-risk analysis

## 9. Risk Management File Contents

The Risk Management File shall contain:

| Document | Status | Location |
|----------|--------|----------|
| Risk Management Plan (this document) | [status] | [link] |
| Intended Use Statement | [status] | [link] |
| Risk Analysis | [status] | [link] |
| Risk Evaluation Records | [status] | [link] |
| Risk Control Implementation Records | [status] | [link] |
| Risk Control Verification Records | [status] | [link] |
| Overall Residual Risk Evaluation | [status] | [link] |
| Risk Management Report | [status] | [link] |

## 10. Plan Maintenance

### 10.1 Review Schedule
This plan shall be reviewed:
- At start of each development phase
- When significant design changes occur
- Annually during production/post-production
- When regulatory requirements change

### 10.2 Change History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| [version] | [date] | [author] | Initial plan |
| ... | ... | ... | ... |

## 11. Approval

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Risk Management Lead | | | |
| Quality Assurance | | | |
| Regulatory Affairs | | | |
| Top Management | | | |

---

## Appendix A: Extraction Metadata
- Repository: [repo]
- Commit: [hash]
- Extracted: [timestamp]
- Standard: ISO 14971:2019
```

## Compliance Mapping

| Document Section | ISO 14971 Clause | Evidence Provided |
|------------------|------------------|-------------------|
| Section 1 | 4.4 a) | Scope of planned activities |
| Section 4 | 4.4 b) | Assignment of responsibilities |
| Section 6.1-6.3, 7 | 4.4 c) | Requirements for review of activities |
| Section 5 | 4.4 d) | Criteria for risk acceptability |
| Section 5.5 | 4.4 e) | Method to evaluate overall residual risk |
| Section 7 | 4.4 f) | Verification activities for risk controls |
| Section 8 | 4.4 g) | Production/post-production activities |
| Section 4.3 | 4.3 | Competence of personnel |
| Section 9 | 4.5 | Risk management file contents |

## Examples

### Input: risks.json (partial)

```json
{
  "intended_use": {
    "medical_function": "Patient vital signs monitoring and alerting",
    "intended_users": ["Nurses", "Physicians"],
    "use_environment": "Hospital ward, ICU"
  },
  "safety_characteristics": [
    {
      "char_id": "SC-001",
      "characteristic": "Heart rate display accuracy",
      "limits": "±2 bpm"
    }
  ]
}
```

### Output: Generated RMP Section

```markdown
### 1.2 Product Identification
| Attribute | Value |
|-----------|-------|
| Product Name | Vital Signs Monitor |
| Product Version | 2.0 |
| Regulatory Classification | MDR Class IIb |
| Intended Use | Patient vital signs monitoring and alerting for nurses and physicians in hospital ward and ICU settings |

### 1.3 Scope of Risk Management Activities
This plan covers the following lifecycle phases:
- [x] Design and Development
- [x] Verification and Validation
- [x] Production
- [x] Post-Production (including post-market surveillance)
- [ ] Decommissioning 🆔 VxzPMj
```

## Validation Criteria

- [ ] All ISO 14971 clause 4.4 elements (a-g) are addressed 🆔 NPl09i
- [ ] Risk acceptability matrix is defined with clear categories 🆔 M8AFTU
- [ ] Responsibilities are assigned with names or roles 🆔 3kO13U
- [ ] Verification activities are planned for each control type 🆔 VStIN8
- [ ] Post-production monitoring activities are defined 🆔 qpiy0u
- [ ] Risk management file contents are enumerated 🆔 nPw822
- [ ] Document has approval signature section 🆔 y4tNSI
- [ ] Criteria for probability estimation uncertainty addressed 🆔 96B5Zf
- [ ] ALARP decision process is defined 🆔 f12R70
- [ ] Overall residual risk evaluation method is specified 🆔 NH4Gw4
