---
id:
title: "Risk Management Plan - health-manager"
version:
author:
effective_date:
type: "Plan"
document_id: "RMP-health-manager-1.0"
regulatory_class: "MDR Class IIa (medical device software)"
process: "[Risk Management Process](../../../Canvases/Risk%20Management%20Process.canvas)"
requirements: "[ISO 14971](../../../Requirements/ISO_14971_Requirements.md)"
owner: "[Head of Risk Management](../../../Assets/Head%20of%20Risk%20Management.md)"
---

# Risk Management Plan

## 1. Purpose and Scope

### 1.1 Purpose
This Risk Management Plan establishes the framework for systematic identification, evaluation, control, and monitoring of risks associated with health-manager throughout its lifecycle.

### 1.2 Product Identification
| Attribute | Value |
|-----------|-------|
| Product Name | health-manager |
| Product Version | 1.0 (commit 16e0273739eda65f73af5410323e1caf53362f16) |
| Regulatory Classification | MDR Class IIa (medical device software) |
| Intended Use | Healthcare appointment management system enabling patient consultations, clinical data recording, handover between practitioners, EHR integration (TakeCare), e-prescription management, and payment processing. Supports asynchronous, drop-in, and scheduled appointment workflows. |

### 1.3 Scope of Risk Management Activities
This plan covers the following lifecycle phases:
- [x] Design and Development
- [x] Verification and Validation
- [x] Production
- [x] Post-Production (including post-market surveillance)
- [ ] Decommissioning 🆔 S3Zlsr

### 1.4 Boundaries
**Included:**
- Spring Boot microservice application with JPA persistence
- Appointment lifecycle management and state machine
- Practitioner handover functionality
- Clinical notes recording and management
- EHR integration (TakeCare) for clinical documentation export
- E-prescription transmission to pharmacy systems
- Payment processing via Klarna gateway
- Role-based access control and authorization
- Integration with external services: Directory, MeetingPlace, Notifier
- Database persistence (MySQL/MariaDB)
- JMS messaging for event distribution
- REST API for client applications
- Audit logging and traceability

**Excluded:**
- Directory service (practitioner/patient master data) - external dependency
- MeetingPlace service (video consultations) - external dependency
- Notifier service (push notifications) - external dependency
- TakeCare EHR system - external integration point
- E-prescription service - external integration point
- Klarna payment gateway - external integration point
- Client applications (web browsers, mobile apps) - separate risk analysis required
- Network infrastructure and cloud hosting platform
- Operating system and JVM runtime

## 2. Referenced Documents

| Document | Version | Relationship |
|----------|---------|--------------|
| ISO 14971:2019 | 3rd Edition | Governing standard |
| [QMS Risk Management Procedure](../../../Assets/Risk%20Management%20Procedure.md) | Current | Process definition |
| [Software Requirements Specification](../../../Templates/Header-Specification.md) | 1.0 | Parent requirements |
| risk_analysis_handover.md | Current | Detailed risk analysis for handover feature |
| handover_architecture.md | Current | Architecture documentation |
| software_requirements.md | Current | System requirements |
| ISO 13485:2016 | 3rd Edition | QMS standard |
| IEC 62304:2006+A1:2015 | 2nd Edition | Software lifecycle standard |
| EU MDR 2017/745 | Current | Medical device regulation |

## 3. Definitions and Abbreviations

| Term | Definition |
|------|------------|
| ALARP | As Low As Reasonably Practicable |
| API | Application Programming Interface |
| EHR | Electronic Health Record |
| FMEA | Failure Mode and Effects Analysis |
| GDPR | General Data Protection Regulation |
| Harm | Injury or damage to the health of people, or damage to property or the environment |
| Hazard | Potential source of harm |
| Hazardous Situation | Circumstance in which people, property or environment are exposed to one or more hazards |
| JMS | Java Message Service |
| JWT | JSON Web Token |
| MDR | Medical Device Regulation (EU 2017/745) |
| Residual Risk | Risk remaining after risk control measures have been implemented |
| REST | Representational State Transfer |
| Risk | Combination of probability of occurrence of harm and severity of that harm |
| Risk Control | Process in which decisions are made and measures implemented to reduce or maintain risk within specified levels |
| Severity | Measure of the possible consequences of a hazard |
| SOP | Standard Operating Procedure |
| SQL | Structured Query Language |
| XSS | Cross-Site Scripting |
| XXE | XML External Entity |

## 4. Responsibilities and Authorities

### 4.1 Risk Management Team

| Role | Name | Responsibilities |
|------|------|------------------|
| Risk Management Lead | TBD | Overall risk management activities, risk file maintenance, risk acceptability decisions |
| Software Architect | TBD | Hazard identification, design risk controls, architecture decisions |
| Development Lead | TBD | Risk control implementation, code-level controls, technical verification |
| Quality Assurance Lead | TBD | Risk control verification, testing, compliance review |
| Clinical Expert | TBD | Clinical harm assessment, severity evaluation, usability assessment |
| Regulatory Affairs | TBD | Regulatory acceptability review, submission preparation |
| Information Security Officer | TBD | Security risk assessment, data protection controls |

### 4.2 Approval Authorities

| Activity | Authority | Required Evidence |
|----------|-----------|-------------------|
| Risk Management Plan approval | Risk Management Lead + Top Management | Signed RMP |
| Risk acceptability decisions (individual risks) | Risk Management Lead | Risk evaluation records |
| Benefit-risk decisions (ALARP risks) | Risk Management Lead + Clinical Expert | B-R analysis documentation |
| Risk Management Report approval | Risk Management Lead + Quality Assurance Lead + Top Management | Signed RMR |
| Release for commercial distribution | Top Management | Completed RMR with acceptable overall residual risk |

### 4.3 Competency Requirements
Personnel performing risk management activities shall have:
- Knowledge of the medical device software and its intended use
- Understanding of applicable standards (ISO 14971, IEC 62304, ISO 13485, EU MDR)
- Training in risk management techniques (FMEA, fault tree analysis, use case analysis)
- Experience in software development lifecycle and testing methodologies
- Understanding of healthcare workflows and clinical context
- Familiarity with Spring Boot, JPA, JMS, REST API design
- Knowledge of security principles (authentication, authorization, encryption)
- Understanding of database design and transaction management

## 5. Risk Acceptability Criteria

### 5.1 Severity Categories

| Level | Category | Description | Examples |
|-------|----------|-------------|----------|
| S1 | Negligible | No injury; inconvenience or administrative burden | Billing error correctable through customer service; log entry missing; UI annoyance |
| S2 | Minor | Temporary injury not requiring professional medical intervention OR privacy breach without direct harm | Skin irritation, temporary confusion; unauthorized access to non-sensitive data; delayed medication access with alternative available |
| S3 | Serious | Injury requiring professional medical intervention OR significant privacy breach | Incorrect treatment requiring correction; misdiagnosis requiring follow-up; care delay causing hospitalization; unauthorized access to sensitive medical records |
| S4 | Critical | Permanent impairment or life-threatening injury | Permanent organ damage; severe adverse drug reaction; complete loss of care access for acute condition |
| S5 | Catastrophic | Death | Patient death |

### 5.2 Probability Categories

| Level | Category | Description | Quantitative Range |
|-------|----------|-------------|-------------------|
| P1 | Incredible | Essentially impossible; no known occurrence in similar systems | < 10⁻⁶ per use |
| P2 | Improbable | Very unlikely; might occur in exceptional circumstances | 10⁻⁶ to 10⁻⁴ per use |
| P3 | Remote | Unlikely but possible during product lifetime | 10⁻⁴ to 10⁻² per use |
| P4 | Occasional | May occur several times during product lifetime | 10⁻² to 10⁻¹ per use |
| P5 | Probable | Expected to occur frequently during product lifetime | > 10⁻¹ per use |

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
2. Apply worst-case probability assumption (P4 Occasional) unless evidence supports lower probability
3. Implement risk controls to reduce severity or provide evidence for lower probability
4. Document rationale in risk evaluation
5. Consider post-production monitoring to gather probability data

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
- Significant residual risks are disclosed to users in labeling
- Post-production monitoring plan is in place

## 6. Risk Management Activities

### 6.1 Risk Analysis Activities

| Activity | Method | Responsible | Output |
|----------|--------|-------------|--------|
| Intended use analysis | Document review, stakeholder interviews, code analysis | Risk Management Lead + Clinical Expert | Intended use documentation, use environment description, user profiles |
| Safety characteristics identification | Code review, architecture analysis, requirements analysis | Software Architect + Risk Team | Safety characteristics list with limits |
| Hazard identification | FMEA, fault injection analysis, code pattern analysis, historical data review | Risk Team | Hazard list with categories |
| Hazardous situation identification | Fault tree analysis, use case analysis, failure scenario modeling | Risk Team | Hazardous situation list with sequences |
| Risk estimation | Risk matrix method, expert judgment, historical data | Risk Team | Initial risk estimates (severity + probability) |

### 6.2 Risk Evaluation Activities

| Activity | Method | Responsible | Output |
|----------|--------|-------------|--------|
| Individual risk evaluation | Compare to acceptability matrix | Risk Management Lead | Evaluation records (acceptable/ALARP/unacceptable) |
| ALARP determination | Practicability analysis, cost-benefit analysis, state-of-the-art review | Risk Team | ALARP justification documentation |
| Benefit-risk analysis | Structured comparison, clinical literature review | Clinical Expert + Risk Management Lead | B-R documentation for ALARP risks |

### 6.3 Risk Control Activities

| Activity | Method | Responsible | Output |
|----------|--------|-------------|--------|
| Control option analysis | Priority order per ISO 14971 clause 7.1, design patterns, best practices | Software Architect + Risk Team | Selected control measures with rationale |
| Control implementation | Design changes, code implementation, configuration | Development Lead | Implemented controls in codebase |
| Control verification | Unit tests, integration tests, code review, penetration testing | Quality Assurance Lead | Verification records and test reports |
| Residual risk evaluation | Post-control assessment using risk matrix | Risk Management Lead | Residual risk records |
| New hazard assessment | Review control implementations for introduced hazards | Risk Team | Updated hazard list if new hazards found |

### 6.4 Risk Control Priority Order

Risk controls shall be considered in this priority order (ISO 14971 clause 7.1):
1. **Inherent safety by design** — Eliminate hazard or reduce risk through design
   - Use of database transactions (@Transactional) to prevent partial updates
   - UNIQUE constraints to prevent duplicate records
   - Input validation constraints (varchar length limits)
   - State machine validation to enforce valid transitions
   - Strong typing and compile-time checks
2. **Protective measures** — Guards, interlocks, automatic controls, monitoring
   - Authorization checks (AppointmentAuthorizer)
   - Validation against external authoritative sources (DirectoryService, CareUnitRegistry)
   - Feature flags to control access (ENABLE_HANDOVERS_ACROSS_CARE_PROVIDERS)
   - Redundant notification channels (NotifierClient + JMS)
   - Error handling and retry logic
   - XML sanitization (NoteXmlSanitizer)
   - Audit logging (AuditLoggingService, appointment_handover_event)
3. **Information for safety** — Warnings, instructions, training, labeling
   - Error messages informing users of validation failures
   - Logging statements for operational monitoring
   - User interface notifications (chat messages to patients)
   - Training materials and SOPs for practitioners
   - Disclosure of residual risks in user documentation

## 7. Verification of Risk Control Measures

### 7.1 Verification Methods

| Control Type | Verification Method | Evidence Required |
|--------------|---------------------|-------------------|
| Inherent safety (design) | Design review, code review | Review records, architecture diagrams |
| Inherent safety (database) | Database schema review, constraint testing | DDL scripts, constraint violation tests |
| Protective measures (validation) | Unit tests, integration tests | Test reports with pass/fail results |
| Protective measures (authorization) | Security testing, penetration testing | Test reports, security scan results |
| Protective measures (integration) | Integration tests with external systems | Test reports, contract tests |
| Information for safety (errors) | Code review, user acceptance testing | Error message catalog, UAT results |
| Information for safety (logging) | Log capture tests, production monitoring | Log analysis, alerting configuration |
| Information for safety (training) | Training completion records, competency assessment | Training logs, assessment results |

### 7.2 Effectiveness Criteria

Risk control measures are considered effective when:
- Implementation verification confirms correct implementation (code review passed, tests pass)
- Testing demonstrates control functions as specified under normal and fault conditions
- Residual risk is reduced to acceptable or ALARP level
- No new uncontrolled hazards introduced (or new hazards have acceptable residual risk)
- Control does not adversely impact usability or introduce new use errors

### 7.3 Verification Status

The following verification activities shall be completed before release:

**Completed (based on extraction data):**
- Unit tests for handover propose/accept/deny/cancel flows
- Integration tests for notification delivery (NotifierClient, JMS)
- Unit tests for state machine validation
- Integration tests for authorization (AppointmentAuthorizer)
- Unit tests for input validation (practitioner, care unit)
- Database constraint verification (UNIQUE on appointment_id)
- Error message verification in exception handling

**Requires completion (based on gaps analysis):**
- Integration tests with TakeCare EHR system (GAP-002)
- Integration tests with e-prescription service (GAP-003)
- Integration tests with Klarna payment gateway (GAP-004)
- Security unit tests with malicious payloads for XML sanitization (GAP-005)
- Audit logging verification tests (GAP-001, GAP-006)
- Load tests for concurrent handover acceptance (GAP-007)

## 8. Production and Post-Production Activities

### 8.1 Information Collection

The following information shall be collected and reviewed for risk management relevance:

| Information Source | Collection Method | Review Frequency | Responsible |
|-------------------|-------------------|------------------|-------------|
| Customer complaints | Support ticketing system, email | Weekly | Risk Management Lead |
| Field service reports | Integration monitoring logs | Daily | Operations Team |
| Vigilance/adverse event reports | Regulatory reporting system | Immediately upon receipt | Regulatory Affairs |
| Production quality data | Application logs, error rates | Daily | Operations Team |
| User feedback | In-app feedback, user surveys | Monthly | Product Manager |
| Scientific literature updates | Literature search | Quarterly | Clinical Expert |
| Integration failure logs | TakeCare, e-prescription, payment logs | Daily | Operations Team |
| Security incidents | Security monitoring, penetration test results | Immediately upon detection | Information Security Officer |
| Audit log analysis | Audit trail reviews | Monthly | Quality Assurance Lead |

### 8.2 Review Triggers

Risk management file shall be reviewed when:
- Safety-related complaint received (within 24 hours)
- Adverse event reported (immediately)
- Vigilance report filed with regulatory authority (within 48 hours)
- Production trend indicates potential safety issue (e.g., spike in handover failures, notification failures)
- Design change implemented affecting safety-critical functionality
- New hazard information becomes available from any source
- External system integration change (TakeCare, e-prescription, Klarna)
- Security vulnerability discovered or exploited
- Annual review cycle (mandatory minimum)
- Before each major release (version x.0)

### 8.3 Action Process

When post-production information indicates:
- **Previously unrecognized hazard** → Update risk analysis, perform risk evaluation, implement controls if needed, update Risk Management Report
- **Risk no longer acceptable** (e.g., probability higher than estimated) → Implement additional controls, verify effectiveness, update residual risk evaluation
- **State of the art changed** → Review existing controls for adequacy, implement enhanced controls if available
- **Overall residual risk changed** → Update benefit-risk analysis, reassess acceptability, inform users of changes
- **Control effectiveness in question** → Investigate root cause, enhance or replace control, re-verify

### 8.4 Post-Market Surveillance Plan

Post-market surveillance shall include:
- Proactive monitoring of application error logs and exception rates
- Periodic review of audit logs for authorization failures or suspicious access patterns
- Monthly analysis of integration failures (EHR export, prescription transmission, payment processing)
- Quarterly analysis of complaint trends and user feedback
- Annual comprehensive risk file review
- Participation in industry safety information exchanges
- Monitoring of regulatory guidance updates and standards revisions

## 9. Risk Management File Contents

The Risk Management File shall contain:

| Document | Status | Location |
|----------|--------|----------|
| Risk Management Plan (this document) | Draft v1.0 | `/Output/health-manager/generated/RMP.md` |
| Intended Use Statement | Extracted | Included in `risks.json` |
| Safety Characteristics | Extracted | Included in `risks.json` (8 characteristics) |
| Hazard Analysis | Extracted | `risks.json` (17 hazards identified) |
| Hazardous Situation Analysis | Extracted | `risks.json` (17 situations identified) |
| Risk Evaluation Records | Extracted | Included in `risks.json` (initial risk levels) |
| Risk Control Records | Extracted | `risks.json` (34 controls identified) |
| Risk Control Verification Records | Partial | Test results referenced in `risks.json`; additional verification needed per gaps analysis |
| Residual Risk Evaluation | Extracted | `risks.json` (6 residual risks documented) |
| Overall Residual Risk Evaluation | Planned | To be completed in Risk Management Report |
| Risk Management Report | Planned | To be generated after verification completion |
| Source Risk Analysis (handover feature) | Complete | `risk_analysis_handover.md` |
| Architecture Documentation | Complete | `handover_architecture.md` |
| Software Requirements Specification | Complete | `software_requirements.md` |
| Traceability Matrix | Extracted | Included in `risks.json` |
| Gap Analysis | Extracted | `risks.json` (8 gaps identified) |

## 10. Plan Maintenance

### 10.1 Review Schedule
This plan shall be reviewed:
- At start of each development phase (design, implementation, verification, validation, release)
- When significant design changes occur affecting risk profile
- When new features are added or existing features modified
- Annually during production/post-production (minimum)
- When regulatory requirements change (EU MDR updates, standards revisions)
- After adverse events or serious complaints
- After external integration changes

### 10.2 Revision Process
Plan revisions shall follow the QMS change control procedure:
1. Change request initiated by any team member
2. Impact assessment performed by Risk Management Lead
3. Revisions drafted with rationale documented
4. Review and approval by authorities per Section 4.2
5. Version incremented and change history updated
6. Affected documents updated accordingly
7. Training provided on significant changes

### 10.3 Change History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | 2026-03-06 | Claude (generated) | Initial plan generated from extracted risks.json |

## 11. Approval

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Risk Management Lead | | | |
| Quality Assurance Lead | | | |
| Regulatory Affairs | | | |
| Top Management | | | |

---

## Appendix A: Extraction Metadata

- **Repository:** `/home/jakob/Noncomplicity/Repos/health-manager`
- **Commit:** `16e0273739eda65f73af5410323e1caf53362f16`
- **Extracted:** `2026-03-06T00:00:00Z`
- **Extractor version:** `1.0`
- **Standard:** ISO 14971:2019

## Appendix B: Risk Summary

### B.1 Safety Characteristics Summary

| ID | Characteristic | Limits |
|----|----------------|--------|
| SC-001 | Clinical data integrity | Data must persist through handovers, system restarts, and integration exports. Database constraints enforce referential integrity. |
| SC-002 | Appointment ownership tracking | State transitions must be atomic and logged. Handover process enforces UNIQUE constraint. |
| SC-003 | Authorization and access control | Care provider boundaries enforced unless explicit consent given. |
| SC-004 | Notification delivery reliability | Dual-channel notification (JMS + NotifierClient). Failures logged. |
| SC-005 | Audit trail completeness | All clinical actions, handovers, and data access must be logged. |
| SC-006 | Input validation constraints | Handover parameters validated to prevent invalid states. |
| SC-007 | Integration data consistency | Clinical notes exported to TakeCare EHR must match internal records. |
| SC-008 | Prescription safety | E-prescriptions must be accurately transmitted to pharmacy systems. |

### B.2 Hazard Categories

| Category | Count | Examples |
|----------|-------|----------|
| Clinical Data | 5 | Transaction failure, data corruption, audit trail loss, invalid state, notes truncation |
| Security | 4 | Unauthorized access, validation bypass, cross-provider exposure, injection attacks |
| Availability | 2 | Notification delivery failure, unavailable care unit |
| Integration | 4 | JMS event failure, EHR export failure, prescription transmission error, payment processing error |
| User Interface | 1 | Patient chat message delivery failure |

### B.3 Risk Control Summary

| Control Type | Count | Examples |
|--------------|-------|----------|
| Inherent Safety | 5 | @Transactional, UNIQUE constraints, varchar limits, state machine validation |
| Protective Measures | 21 | Authorization checks, DirectoryService validation, feature flags, retry logic, audit logging |
| Information for Safety | 8 | Error messages, logging statements, user notifications, training |

### B.4 Residual Risks Requiring Disclosure

The following residual risks shall be disclosed in user documentation:

1. **RR-001:** In rare cases of system infrastructure failure, handover notifications may be delayed. Care units should maintain backup communication channels (phone, pager) for urgent handovers.

2. **RR-004:** Handover notes are limited to 1024 characters. For complex cases, practitioners should prioritize critical safety information and reference the full clinical record in the appointment notes.

3. **RR-005:** Clinical notes are exported to external EHR systems. In case of integration failures, notes remain accessible within the appointment management system and can be manually exported.

4. **RR-006:** E-prescriptions are transmitted electronically. In rare cases of system unavailability, practitioners may issue paper prescriptions or contact the pharmacy directly.

### B.5 Outstanding Verification Gaps

The following verification activities must be completed before release (see Section 7.3 and gaps analysis):

- **GAP-002:** Integration tests with TakeCare EHR system
- **GAP-003:** Integration tests with e-prescription service
- **GAP-004:** Integration tests with Klarna payment gateway
- **GAP-005:** Security unit tests with malicious payloads for XML sanitization
- **GAP-001, GAP-006:** Audit logging verification tests
- **GAP-007:** Load tests for concurrent handover acceptance
- **GAP-008:** Standard operating procedures for manual fallback when integrations fail

## Appendix C: Compliance Mapping

| Document Section | ISO 14971 Clause | Evidence Provided |
|------------------|------------------|-------------------|
| Section 1 (Purpose and Scope) | 4.4 a) | Scope of planned activities |
| Section 4 (Responsibilities) | 4.4 b) | Assignment of responsibilities and authorities |
| Section 6 (Risk Management Activities) | 4.4 c) | Requirements for review of risk analysis, evaluation, and control activities |
| Section 5 (Risk Acceptability) | 4.4 d) | Criteria for risk acceptability including when probability cannot be estimated |
| Section 5.5 (Overall Residual Risk) | 4.4 e) | Method to evaluate overall residual risk acceptability |
| Section 7 (Verification) | 4.4 f) | Verification activities planned for risk control measures |
| Section 8 (Post-Production) | 4.4 g) | Production and post-production information collection and review |
| Section 4.3 (Competency) | 4.3 | Competence of personnel performing risk management activities |
| Section 9 (Risk File Contents) | 4.5 | Risk management file establishment and maintenance |

---

**End of Risk Management Plan**
