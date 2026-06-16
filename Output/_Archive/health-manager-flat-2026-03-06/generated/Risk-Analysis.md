---
id: 725af57
title: "Risk Analysis"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Report"
document_id: "RA-health-manager-1.0"
process: "../../Canvases/Risk Management Process.canvas"
requirements: "../../Requirements/ISO_14971_Requirements.md"
owner: "../../Assets/Head of Risk Management.md"
---

# Risk Analysis - Health Manager

## 1. Document Information

### 1.1 Purpose
This document presents the systematic risk analysis for the Health Manager appointment management system, identifying hazards, estimating and evaluating risks, and documenting risk control measures per ISO 14971:2019.

### 1.2 Scope

| Attribute | Value |
|-----------|-------|
| Product | Health Manager |
| Repository | /home/jakob/Noncomplicity/Repos/health-manager |
| Commit | 16e0273739eda65f73af5410323e1caf53362f16 |
| Analysis Date | 2026-03-06 |
| Standard | ISO 14971:2019 |

### 1.3 Risk Acceptability Criteria

Risk levels are evaluated using a severity-probability matrix:

| Severity | Probability | Risk Level |
|----------|-------------|------------|
| Critical | Remote-Incredible | ALARP |
| Critical | Improbable+ | Unacceptable |
| Serious | Improbable-Incredible | ALARP |
| Serious | Remote+ | Unacceptable |
| Minor/Negligible | Any | Acceptable |

## 2. Intended Use

### 2.1 Medical Function

Health Manager is a healthcare appointment management system enabling patient consultations, clinical data recording, handover between practitioners, EHR integration (TakeCare), e-prescription management, and payment processing. Supports asynchronous, drop-in, and scheduled appointment workflows.

| Element | Description |
|---------|-------------|
| Medical Function | Appointment management, clinical data recording, practitioner handover |
| Intended Users | Clinicians (doctors, nurses), Patients, Administrative staff, System integrators |
| Use Environment | Cloud-based SaaS platform accessed via web browsers and mobile applications |
| Patient Population | General population seeking healthcare consultations |
| Operating Principle | Spring Boot microservice with JPA persistence, JMS messaging, REST API, role-based access control |

### 2.2 Key Integrations

- **Directory** - Practitioner and patient identity data
- **MeetingPlace** - Video consultations and chat
- **Notifier** - Push notifications
- **TakeCare EHR** - Electronic health record integration
- **E-prescription services** - Pharmacy prescription transmission
- **Klarna** - Payment gateway

## 3. Characteristics Related to Safety

| ID | Characteristic | Limits/Criteria | Source |
|----|----------------|-----------------|--------|
| SC-001 | Clinical data integrity | Appointment notes, diagnoses, prescriptions must persist accurately through handovers, system restarts, and exports. Database constraints enforce referential integrity. | Appointment.java, Note.java |
| SC-002 | Appointment ownership tracking | State transitions must be atomic and logged. UNIQUE constraint on appointment_id prevents concurrent ownership claims. | AppointmentHandoverServiceImpl.java, AppointmentHandoverProcess.java |
| SC-003 | Authorization and access control | Only authorized practitioners and patients access medical records. Care provider boundaries enforced unless consent given. | AppointmentAuthorizer.java |
| SC-004 | Notification delivery reliability | Practitioners notified of handovers and critical events via dual-channel (JMS + NotifierClient). Failures logged. | AppointmentHandoverServiceImpl.java |
| SC-005 | Audit trail completeness | All clinical actions, handovers, and data access logged for medico-legal accountability. | AuditLoggingService.java, AppointmentHandoverEvent.java |
| SC-006 | Input validation constraints | Handover parameters validated: careUnitId (varchar 50), practitionerRole (varchar 50), practitionerId (varchar 50 with DirectoryService validation), notes (varchar 1024 max). | AppointmentHandoverServiceImpl.java |
| SC-007 | Integration data consistency | Clinical notes exported to TakeCare must match internal records. XML sanitization and transformation applied. | TakeCareNoteServiceImpl.java, NoteXmlSanitizer.java |
| SC-008 | Prescription safety | E-prescriptions accurately transmitted to pharmacy systems with retry logic. | Prescription.java, ExternalPrescriptionServiceImpl.java |

## 4. Hazard Identification

### 4.1 Hazard Summary

| Hazard ID | Hazard Description | Category | Situations |
|-----------|-------------------|----------|------------|
| HAZ-001 | Notification delivery failure - practitioner not informed of pending handover | availability | HS-001 |
| HAZ-002 | JMS event publication failure - external systems not synchronized | integration | HS-002 |
| HAZ-003 | Database transaction failure during handover - partial state update | clinical_data | HS-003 |
| HAZ-004 | Invalid practitioner validation bypass - handover to non-existent practitioner | security | HS-004 |
| HAZ-005 | Cross-care-provider data exposure without consent | security | HS-005 |
| HAZ-006 | Concurrent handover race condition - multiple practitioners accept | clinical_data | HS-006 |
| HAZ-007 | Handover to unavailable care unit | availability | HS-007 |
| HAZ-008 | Patient chat message delivery failure | user_interface | HS-008 |
| HAZ-009 | Invalid state machine transition | clinical_data | HS-009 |
| HAZ-010 | Audit log persistence failure | clinical_data | HS-010 |
| HAZ-011 | Shift validation failure - practitioner accepts without active shift | security | HS-011 |
| HAZ-012 | Handover notes truncation - clinical context exceeds limit | clinical_data | HS-012 |
| HAZ-013 | Unauthorized access to clinical data | security | HS-013 |
| HAZ-014 | EHR export failure - notes not transferred to TakeCare | integration | HS-014 |
| HAZ-015 | Prescription transmission error | integration | HS-015 |
| HAZ-016 | Payment processing failure | integration | HS-016 |
| HAZ-017 | Data sanitization bypass - malicious injection | security | HS-017 |

## 5. Hazardous Situations and Risk Estimation

### 5.1 Risk Estimation Summary

| Situation ID | Hazard | Potential Harm | Severity | Probability | Initial Risk |
|--------------|--------|----------------|----------|-------------|--------------|
| HS-001 | HAZ-001 | Delayed diagnosis or treatment; patient deterioration | Serious | Remote | ALARP |
| HS-002 | HAZ-002 | Care coordination failure; conflicting clinical decisions | Minor | Remote | Acceptable |
| HS-003 | HAZ-003 | No practitioner takes responsibility; patient abandoned | Critical | Incredible | Acceptable |
| HS-004 | HAZ-004 | Patient encounter with no responsible clinician | Critical | Improbable | ALARP |
| HS-005 | HAZ-005 | Privacy violation; regulatory penalties | Minor | Improbable | Acceptable |
| HS-006 | HAZ-006 | Duplicate care; conflicting treatments | Minor | Improbable | Acceptable |
| HS-007 | HAZ-007 | Delayed access to care; patient deterioration | Serious | Improbable | ALARP |
| HS-008 | HAZ-008 | Missed clinical communications; delayed response | Minor | Remote | Acceptable |
| HS-009 | HAZ-009 | Appointment workflow blocked; patient unable to receive care | Minor | Improbable | Acceptable |
| HS-010 | HAZ-010 | Inability to investigate adverse events; compliance failure | Negligible | Remote | Acceptable |
| HS-011 | HAZ-011 | Care continuity gap; practitioner not available | Minor | Remote | Acceptable |
| HS-012 | HAZ-012 | Treatment error due to incomplete information | Minor | Remote | Acceptable |
| HS-013 | HAZ-013 | Privacy violation; compromised patient safety | Serious | Improbable | ALARP |
| HS-014 | HAZ-014 | Fragmented medical record; potential treatment errors | Minor | Remote | Acceptable |
| HS-015 | HAZ-015 | Delayed medication access; symptom progression | Minor | Remote | Acceptable |
| HS-016 | HAZ-016 | Financial harm; administrative burden | Negligible | Remote | Acceptable |
| HS-017 | HAZ-017 | System compromise; data breach; clinical data altered | Minor | Improbable | Acceptable |

### 5.2 Detailed Hazardous Situations

#### HS-001: Practitioner Not Notified of Pending Handover

**Hazard:** HAZ-001 - Notification delivery failure

**Situation:** Practitioner proposes handover but target practitioner never receives notification, leaving appointment unattended.

**Sequence:**
1. Practitioner A initiates handover
2. NotifierClient.sendClinicNotification() fails due to network/service outage
3. Target practitioner B not aware of pending handover
4. Appointment remains in PROPOSED state with no responder
5. Patient condition deteriorates without clinical attention

**Potential Harm:** Delayed diagnosis or treatment; patient deterioration; adverse clinical outcome

**Risk Estimation:**
- **Severity:** Serious - Missing clinical oversight for acute patient could lead to hospitalization or permanent injury
- **Probability:** Remote - Dual notification paths (NotifierClient + JMS events) reduce likelihood
- **Initial Risk:** ALARP

**Source:** AppointmentHandoverServiceImpl.java:84-89

---

#### HS-003: Database Transaction Corrupts Appointment Ownership

**Hazard:** HAZ-003 - Database transaction failure during handover

**Situation:** Database transaction fails during handover acceptance after state change, corrupting appointment ownership.

**Sequence:**
1. acceptHandover() called
2. handoverProcess fields updated
3. appointmentRepository.save() throws SQLException
4. Partial update persisted
5. Both practitioners believe they own appointment OR appointment orphaned

**Potential Harm:** No practitioner takes responsibility; patient abandoned; severe delay in care

**Risk Estimation:**
- **Severity:** Critical - Loss of practitioner accountability could result in complete absence of care for urgent patient
- **Probability:** Incredible - @Transactional annotation ensures atomicity, UNIQUE constraint prevents concurrent accepts
- **Initial Risk:** Acceptable

**Source:** AppointmentHandoverServiceImpl.java:104-130

---

#### HS-004: Handover to Non-Existent Practitioner

**Hazard:** HAZ-004 - Invalid practitioner validation bypass

**Situation:** DirectoryService.isValidPractitioner() returns false positive, allowing handover to non-existent or inactive practitioner.

**Sequence:**
1. Handover proposed to practitionerId 'abc123'
2. DirectoryService incorrectly validates
3. Handover accepted by system
4. No actual practitioner receives assignment
5. Appointment orphaned

**Potential Harm:** Patient encounter with no responsible clinician; complete absence of medical care

**Risk Estimation:**
- **Severity:** Critical - Patient scheduled for care but no practitioner exists to provide it
- **Probability:** Improbable - DirectoryService is authoritative source, validation at propose AND accept stages
- **Initial Risk:** ALARP

**Source:** AppointmentHandoverServiceImpl.java:270-274

---

#### HS-007: Handover to Non-Operational Care Unit

**Hazard:** HAZ-007 - Handover to unavailable care unit

**Situation:** CareUnitRegistry returns stale or invalid data, handover directed to care unit no longer operational.

**Sequence:**
1. Care unit closes for emergency
2. Registry not updated immediately
3. Handover proposed to closed unit
4. Validation passes on stale data
5. No practitioners available - appointment unattended

**Potential Harm:** Delayed access to care; patient deterioration while awaiting assignment

**Risk Estimation:**
- **Severity:** Serious - Patient expecting care but transferred to non-operational unit, delay could be hours
- **Probability:** Improbable - CareUnitRegistry actively maintained, validation checks presence
- **Initial Risk:** ALARP

**Source:** AppointmentHandoverServiceImpl.java:287-289

---

#### HS-013: Unauthorized Access to Clinical Data

**Hazard:** HAZ-013 - Authentication or authorization bypass

**Situation:** JWT validation bypassed or authorization check fails, allowing unauthorized practitioner to view patient medical records.

**Sequence:**
1. Attacker obtains expired/forged JWT
2. AppointmentAuthorizer.hasReadAccessToAppointment() fails to reject
3. Sensitive patient data exposed
4. Privacy breach; GDPR violation
5. Patient safety risk if attacker acts on data

**Potential Harm:** Privacy violation; potential for medical identity theft; compromised patient safety if data misused

**Risk Estimation:**
- **Severity:** Serious - Unauthorized access to medical records with potential physical harm if data used maliciously
- **Probability:** Improbable - Multi-layered authorization: JWT validation, role checks, care unit matching, consent checks
- **Initial Risk:** ALARP

**Source:** AppointmentAuthorizer.java:48-154

## 6. Risk Evaluation

### 6.1 Risks Requiring Control

The following ALARP risks require risk control measures to reduce risk to acceptable levels:

| Situation ID | Initial Risk | Severity | Probability | Required Action |
|--------------|--------------|----------|-------------|-----------------|
| HS-001 | ALARP | Serious | Remote | Implement redundant notification |
| HS-004 | ALARP | Critical | Improbable | Strengthen practitioner validation |
| HS-007 | ALARP | Serious | Improbable | Validate care unit availability |
| HS-013 | ALARP | Serious | Improbable | Enforce multi-layer authorization |

All other situations (HS-002, HS-003, HS-005, HS-006, HS-008 through HS-012, HS-014 through HS-017) have initial risk levels of Acceptable and do not require mandatory controls, though protective measures are implemented for defense-in-depth.

## 7. Risk Control

### 7.1 Risk Control Summary by Type

**Inherent Safety (5 controls):** RC-003, RC-004, RC-011, RC-017, RC-023

**Protective Measures (19 controls):** RC-001, RC-005, RC-007, RC-008, RC-010, RC-012, RC-013, RC-015, RC-021, RC-024, RC-025, RC-026, RC-028, RC-029, RC-030, RC-031, RC-033, RC-034

**Information for Safety (10 controls):** RC-002, RC-006, RC-009, RC-014, RC-016, RC-018, RC-020, RC-022, RC-027, RC-032

### 7.2 Critical Risk Controls

#### RC-001: Redundant Notification Channels

**Controlled Situation:** HS-001

**Control Type:** Protective Measure (ISO 14971 clause 7.1 b)

**Description:** Dual-channel notification architecture using NotifierClient push notifications AND JMS event publication to appointmentHandoverEventTopic.

**Implementation:**
- **Mechanism:** dual_channel_notification
- **Code:** AppointmentHandoverServiceImpl.java:84-89
- **Primary channel:** NotifierClient.sendClinicNotification()
- **Secondary channel:** appointmentEventSender.sendHandoverEvent()

**Verification:**
- Unit test verification: verify(notifierClient).sendClinicNotification(expectedNotification)
- Unit test verification: verify(appointmentEventSender).sendHandoverEvent()

**Effectiveness:** Reduces - If one channel fails, the other provides notification

**Residual Risk:** RR-001 (Acceptable with disclosure)

---

#### RC-003: Transaction Atomicity

**Controlled Situation:** HS-003

**Control Type:** Inherent Safety (ISO 14971 clause 7.1 a)

**Description:** @Transactional annotation ensures atomicity of handover acceptance - all database updates commit or rollback together, preventing partial state updates.

**Implementation:**
- **Mechanism:** transaction_management
- **Code:** AppointmentHandoverServiceImpl.java:47

**Verification:**
- Code review confirms @Transactional annotation present
- Database ACID guarantees verified

**Effectiveness:** Eliminates - Impossible to have partial database update

**Residual Risk:** Acceptable (no residual risk)

---

#### RC-004: Database Constraint Prevents Concurrent Handovers

**Controlled Situation:** HS-003, HS-006

**Control Type:** Inherent Safety (ISO 14971 clause 7.1 a)

**Description:** UNIQUE constraint on appointment_id in appointment_handover_process table prevents multiple concurrent handover processes for same appointment.

**Implementation:**
- **Mechanism:** database_constraint
- **Code:** Database schema DDL (SWR-4.1)

**Verification:**
- Database schema review confirms UNIQUE constraint
- Second concurrent transaction will fail with constraint violation

**Effectiveness:** Eliminates - Database enforces single active handover per appointment

**Residual Risk:** Acceptable (no residual risk)

---

#### RC-005: Practitioner Validation

**Controlled Situation:** HS-004

**Control Type:** Protective Measure (ISO 14971 clause 7.1 b)

**Description:** DirectoryService.isValidPractitioner() validation checks practitioner exists and is active before allowing handover.

**Implementation:**
- **Mechanism:** validation
- **Code:** AppointmentHandoverServiceImpl.java:271

**Verification:**
- Test: handoverAppointmentPropose_CaseFailOnHandoverToInvalidPractitionerId()

**Effectiveness:** Reduces - Validation prevents most invalid handovers, but dependent on DirectoryService data quality

**Residual Risk:** RR-002 (Acceptable)

---

#### RC-007: Cross-Care-Provider Feature Flag

**Controlled Situation:** HS-005

**Control Type:** Protective Measure (ISO 14971 clause 7.1 b)

**Description:** CustomizationService checks ENABLE_HANDOVERS_ACROSS_CARE_PROVIDERS feature flag before allowing cross-provider handovers.

**Implementation:**
- **Mechanism:** feature_flag
- **Code:** AppointmentHandoverServiceImpl.java:263

**Verification:**
- Test: handoverAppointmentPropose_CaseFailOnDifferentCareProvider()

**Effectiveness:** Eliminates - Feature disabled by default, requires explicit configuration

**Residual Risk:** Acceptable (no residual risk when properly configured)

---

#### RC-013: Care Unit Availability Validation

**Controlled Situation:** HS-007

**Control Type:** Protective Measure (ISO 14971 clause 7.1 b)

**Description:** CareUnitRegistry.getCareUnit(handoverCareUnitId).isPresent() validates care unit exists before allowing handover.

**Implementation:**
- **Mechanism:** validation
- **Code:** AppointmentHandoverServiceImpl.java:287-289

**Verification:**
- Test: handoverAppointmentPropose_CaseFailOnNonExistingInvalidCareUnit()

**Effectiveness:** Reduces - Validation prevents handovers to non-existent units, but cannot detect units that become unavailable after validation

**Residual Risk:** RR-003 (Acceptable)

---

#### RC-024: Multi-Layer Authorization

**Controlled Situation:** HS-013

**Control Type:** Protective Measure (ISO 14971 clause 7.1 b)

**Description:** AppointmentAuthorizer.hasReadAccessToAppointment() enforces JWT validation, role checks, care unit matching, and consent verification.

**Implementation:**
- **Mechanism:** authorization
- **Code:** AppointmentAuthorizer.java:48-99
- **Layers:** JWT validation, user.isPractitioner() check, care unit boundary enforcement, consent verification (ACCESS_MEDICAL_RECORDS)

**Verification:**
- Authorization integration tests verify multi-layer checks

**Effectiveness:** Reduces - Multiple independent checks reduce probability of unauthorized access

**Residual Risk:** Acceptable (authorization depth-in-defense sufficient)

### 7.3 Supporting Risk Controls

The following controls provide defense-in-depth for situations that already have acceptable risk levels:

| Control ID | Situation | Type | Mechanism | Status |
|------------|-----------|------|-----------|--------|
| RC-002 | HS-002 | Information | Info-level logging of status transitions | Implemented |
| RC-006 | HS-004 | Information | Error message for invalid practitioner | Implemented |
| RC-008 | HS-005 | Protective | Care provider boundary validation | Implemented |
| RC-009 | HS-005 | Information | Error message for cross-provider attempt | Implemented |
| RC-010 | HS-005 | Protective | Consent verification (ACCESS_MEDICAL_RECORDS) | Implemented |
| RC-011 | HS-006 | Inherent | UNIQUE constraint prevents race condition | Implemented |
| RC-012 | HS-006 | Protective | State machine validation | Implemented |
| RC-014 | HS-007 | Information | Error message for invalid care unit | Implemented |
| RC-015 | HS-008 | Protective | HandoverMessage sent to patient chat | Implemented |
| RC-016 | HS-008 | Information | Internationalized chat message | Implemented |
| RC-017 | HS-009 | Inherent | State machine enforces valid transitions | Implemented |
| RC-018 | HS-009 | Information | Error messages for state violations | Implemented |
| RC-019 | HS-010 | Protective | appointment_handover_event table persistence | Implemented |
| RC-020 | HS-010 | Information | Info-level logging (secondary audit) | Implemented |
| RC-021 | HS-011 | Protective | Active shift validation | Implemented |
| RC-022 | HS-011 | Information | Error message for shift validation failure | Implemented |
| RC-023 | HS-012 | Inherent | Database varchar(1024) constraint | Implemented |
| RC-025 | HS-013 | Protective | Care provider boundary enforcement | Implemented |
| RC-026 | HS-014 | Protective | TakeCare retry logic | Implemented |
| RC-027 | HS-014 | Information | takecare_note_log_event audit table | Implemented |
| RC-028 | HS-014 | Protective | NoteXmlSanitizer prevents format errors | Implemented |
| RC-029 | HS-015 | Protective | ExternalPrescriptionService retry logic | Implemented |
| RC-030 | HS-015 | Protective | Prescription entity local persistence | Implemented |
| RC-031 | HS-016 | Protective | PaymentCreditService transaction management | Implemented |
| RC-032 | HS-016 | Information | CreditEvent audit trail | Implemented |
| RC-033 | HS-017 | Protective | NoteXmlSanitizer strips malicious content | Implemented |
| RC-034 | HS-013 | Protective | AuditLoggingService logs practitioner access | Implemented |

## 8. Residual Risk Evaluation

### 8.1 Residual Risk Summary

| Situation | Initial Risk | Controls | Residual Risk | Acceptable? | Disclosure |
|-----------|--------------|----------|---------------|-------------|------------|
| HS-001 | ALARP (Serious/Remote) | RC-001 | Acceptable (Serious/Incredible) | Yes (w/ disclosure) | Required |
| HS-002 | Acceptable (Minor/Remote) | RC-002 | Acceptable | Yes | No |
| HS-003 | Acceptable (Critical/Incredible) | RC-003, RC-004 | Acceptable | Yes | No |
| HS-004 | ALARP (Critical/Improbable) | RC-005, RC-006 | Acceptable (Negligible/Improbable) | Yes | No |
| HS-005 | Acceptable (Minor/Improbable) | RC-007, RC-008, RC-009, RC-010 | Acceptable | Yes | No |
| HS-006 | Acceptable (Minor/Improbable) | RC-011, RC-012 | Acceptable | Yes | No |
| HS-007 | ALARP (Serious/Improbable) | RC-013, RC-014 | Acceptable (Negligible/Improbable) | Yes | No |
| HS-008 | Acceptable (Minor/Remote) | RC-015, RC-016 | Acceptable | Yes | No |
| HS-009 | Acceptable (Minor/Improbable) | RC-017, RC-018 | Acceptable | Yes | No |
| HS-010 | Acceptable (Negligible/Remote) | RC-019, RC-020 | Acceptable | Yes | No |
| HS-011 | Acceptable (Minor/Remote) | RC-021, RC-022 | Acceptable | Yes | No |
| HS-012 | Acceptable (Minor/Remote) | RC-023 | Acceptable (Negligible/Remote) | Yes (w/ disclosure) | Required |
| HS-013 | ALARP (Serious/Improbable) | RC-024, RC-025, RC-034 | Acceptable | Yes | No |
| HS-014 | Acceptable (Minor/Remote) | RC-026, RC-027, RC-028 | Acceptable (Negligible/Remote) | Yes (w/ disclosure) | Required |
| HS-015 | Acceptable (Minor/Remote) | RC-029, RC-030 | Acceptable (Negligible/Remote) | Yes (w/ disclosure) | Required |
| HS-016 | Acceptable (Negligible/Remote) | RC-031, RC-032 | Acceptable | Yes | No |
| HS-017 | Acceptable (Minor/Improbable) | RC-033 | Acceptable | Yes | No |

### 8.2 Residual Risks Requiring Disclosure

#### RR-001: Infrastructure Failure May Delay Handover Notifications

**Original Situation:** HS-001

**Controls:** RC-001 (Redundant notification channels)

**Residual Description:** If both NotifierClient and JMS messaging fail simultaneously (e.g., infrastructure outage), target practitioner may not be notified. Requires manual follow-up or on-call system escalation.

**Residual Risk:**
- **Severity:** Minor (reduced from Serious due to rarity of dual-channel failure)
- **Probability:** Incredible
- **Risk Level:** Acceptable

**Disclosure Text:** "In rare cases of system infrastructure failure, handover notifications may be delayed. Care units should maintain backup communication channels (phone, pager) for urgent handovers."

---

#### RR-004: Handover Notes Character Limit

**Original Situation:** HS-012

**Controls:** RC-023 (Database varchar constraint)

**Residual Description:** Critical information entered beyond 1024 character limit will be truncated. Practitioners must be trained to prioritize critical information at beginning of handover notes and reference full clinical record for details.

**Residual Risk:**
- **Severity:** Negligible (reduced from Minor - full record available)
- **Probability:** Remote
- **Risk Level:** Acceptable

**Disclosure Text:** "Handover notes are limited to 1024 characters. For complex cases, practitioners should prioritize critical safety information and reference the full clinical record in the appointment notes."

---

#### RR-005: EHR Export Failures

**Original Situation:** HS-014

**Controls:** RC-026, RC-027, RC-028 (Retry logic, logging, sanitization)

**Residual Description:** If TakeCare EHR system is offline for extended period, clinical notes may not be exported. Requires monitoring of export failures and manual export procedures for critical clinical documentation.

**Residual Risk:**
- **Severity:** Negligible (notes accessible in Health Manager)
- **Probability:** Remote
- **Risk Level:** Acceptable

**Disclosure Text:** "Clinical notes are exported to external EHR systems. In case of integration failures, notes remain accessible within the appointment management system and can be manually exported."

---

#### RR-006: E-Prescription Service Unavailability

**Original Situation:** HS-015

**Controls:** RC-029, RC-030 (Retry logic, local persistence)

**Residual Description:** If e-prescription service is unavailable, prescriptions may not reach pharmacy. Requires fallback to manual prescribing methods (paper prescription, phone) until service restored.

**Residual Risk:**
- **Severity:** Negligible (alternative prescribing methods available)
- **Probability:** Remote
- **Risk Level:** Acceptable

**Disclosure Text:** "E-prescriptions are transmitted electronically. In rare cases of system unavailability, practitioners may issue paper prescriptions or contact the pharmacy directly."

## 9. Completeness of Risk Control

### 9.1 Risk Control Completeness Check

| Check Item | Status | Evidence |
|------------|--------|----------|
| All identified hazards have hazardous situations | ✓ | Section 5 - 17 hazards, 17 situations |
| All hazardous situations have risk estimation | ✓ | Section 5.1 - all have S/P/risk level |
| All unacceptable/ALARP risks have controls | ✓ | Section 7 - HS-001, HS-004, HS-007, HS-013 controlled |
| All controls have implementation evidence | ✓ | Section 7.2, 7.3 - code locations specified |
| All controls have verification approach | Partial | Section 10 - gaps identified |
| All residual risks evaluated | ✓ | Section 8.1 - all 17 situations evaluated |
| New hazards from controls analyzed | ✓ | No new hazards introduced by controls |
| Disclosures identified | ✓ | Section 8.2 - 4 disclosures required |

### 9.2 Risk Coverage Matrix

| Hazard | Situations | Controls Applied | Residual Accepted |
|--------|------------|------------------|-------------------|
| HAZ-001 | HS-001 | RC-001 | ✓ (w/ disclosure) |
| HAZ-002 | HS-002 | RC-002 | ✓ |
| HAZ-003 | HS-003 | RC-003, RC-004 | ✓ |
| HAZ-004 | HS-004 | RC-005, RC-006 | ✓ |
| HAZ-005 | HS-005 | RC-007, RC-008, RC-009, RC-010 | ✓ |
| HAZ-006 | HS-006 | RC-011, RC-012 | ✓ |
| HAZ-007 | HS-007 | RC-013, RC-014 | ✓ |
| HAZ-008 | HS-008 | RC-015, RC-016 | ✓ |
| HAZ-009 | HS-009 | RC-017, RC-018 | ✓ |
| HAZ-010 | HS-010 | RC-019, RC-020 | ✓ |
| HAZ-011 | HS-011 | RC-021, RC-022 | ✓ |
| HAZ-012 | HS-012 | RC-023 | ✓ (w/ disclosure) |
| HAZ-013 | HS-013 | RC-024, RC-025, RC-034 | ✓ |
| HAZ-014 | HS-014 | RC-026, RC-027, RC-028 | ✓ (w/ disclosure) |
| HAZ-015 | HS-015 | RC-029, RC-030 | ✓ (w/ disclosure) |
| HAZ-016 | HS-016 | RC-031, RC-032 | ✓ |
| HAZ-017 | HS-017 | RC-033 | ✓ |

## 10. Gaps and Recommendations

### 10.1 Testing Gaps

| Gap ID | Type | Description | Affected Controls | Recommended Action |
|--------|------|-------------|-------------------|-------------------|
| GAP-001 | Untested control | Secondary logging audit trail not verified through automated testing | RC-002, RC-020 | Add integration tests verifying log statements with correct format |
| GAP-002 | Untested control | TakeCare EHR export controls lack automated verification | RC-026, RC-027, RC-028 | Implement integration tests with TakeCare test environment, add contract tests for XML format |
| GAP-003 | Untested control | Prescription transmission controls not verified | RC-029, RC-030 | Add integration tests with e-prescription service, implement monitoring/alerting for failures |
| GAP-004 | Untested control | Payment processing controls lack verification | RC-031, RC-032 | Implement integration tests with Klarna test environment, add reconciliation tests |
| GAP-005 | Untested control | XML sanitization not verified through automated testing | RC-033 | Add security unit tests with OWASP XSS vectors, perform penetration testing |
| GAP-006 | Untested control | Audit logging for appointment access not verified | RC-034 | Add integration tests verifying AuditLogDto messages sent to auditLogQueue |
| GAP-007 | Missing verification | No verification that UNIQUE constraint enforced under concurrent load | RC-004, RC-011 | Add load test simulating concurrent handover accepts from multiple practitioners |

### 10.2 Process Gaps

| Gap ID | Type | Description | Affected Residual Risks | Recommended Action |
|--------|------|-------------|------------------------|-------------------|
| GAP-008 | Undocumented procedures | No SOPs for handling notification, export, or prescription failures | RR-001, RR-005, RR-006 | Create SOPs for: (1) Manual handover notification, (2) Manual EHR export, (3) Manual prescription issuance. Train clinical staff on fallback procedures. |

## 11. Traceability

### 11.1 Hazard to Residual Risk Chain

```
HAZ-001 (Notification failure)
└── HS-001 (Practitioner not notified of handover)
    └── RC-001 (Redundant notification channels)
        └── RR-001 (Infrastructure failure) → Acceptable w/ disclosure

HAZ-003 (Database transaction failure)
└── HS-003 (Partial state update)
    ├── RC-003 (Transaction atomicity) → Eliminates
    └── RC-004 (UNIQUE constraint) → Eliminates
        → Acceptable (no residual)

HAZ-004 (Invalid practitioner)
└── HS-004 (Handover to non-existent practitioner)
    ├── RC-005 (Practitioner validation)
    └── RC-006 (Error message)
        └── RR-002 (DirectoryService data quality) → Acceptable

HAZ-007 (Unavailable care unit)
└── HS-007 (Handover to non-operational unit)
    ├── RC-013 (Care unit validation)
    └── RC-014 (Error message)
        └── RR-003 (Care unit becomes unavailable post-validation) → Acceptable

HAZ-012 (Notes truncation)
└── HS-012 (Critical info exceeds limit)
    └── RC-023 (varchar constraint)
        └── RR-004 (Truncation of critical info) → Acceptable w/ disclosure

HAZ-013 (Unauthorized access)
└── HS-013 (Authorization bypass)
    ├── RC-024 (Multi-layer authorization)
    ├── RC-025 (Care provider boundaries)
    └── RC-034 (Audit logging)
        → Acceptable (depth-in-defense sufficient)

HAZ-014 (EHR export failure)
└── HS-014 (Notes not transferred to TakeCare)
    ├── RC-026 (Retry logic)
    ├── RC-027 (Log failures)
    └── RC-028 (XML sanitization)
        └── RR-005 (Extended TakeCare outage) → Acceptable w/ disclosure

HAZ-015 (Prescription transmission error)
└── HS-015 (Prescription not received by pharmacy)
    ├── RC-029 (Retry logic)
    └── RC-030 (Local persistence)
        └── RR-006 (E-prescription service unavailable) → Acceptable w/ disclosure
```

### 11.2 ISO 14971 Compliance Mapping

| Document Section | ISO 14971 Clause | Evidence Provided |
|------------------|------------------|-------------------|
| Section 1 | 5.1 | Risk analysis documentation structure |
| Section 2.1 | 5.2 | Intended use and medical function |
| Section 2.2 | 5.2 | Key integrations (part of use environment) |
| Section 3 | 5.3 | Characteristics related to safety (8 characteristics) |
| Section 4 | 5.4 | Hazard identification (17 hazards) |
| Section 5 | 5.4 | Hazardous situation identification (17 situations) |
| Section 5 | 5.5 | Risk estimation (severity, probability, rationale) |
| Section 6 | 6 | Risk evaluation (acceptability decisions) |
| Section 7 | 7.1 | Risk control option analysis (inherent/protective/information) |
| Section 7 | 7.2 | Implementation and verification of controls (34 controls) |
| Section 8 | 7.3 | Residual risk evaluation (all 17 situations) |
| Section 8.2 | 7.4 | Benefit-risk analysis (implicit - clinical utility justifies residual risks) |
| Section 7.2-7.3 | 7.5 | Risks from control measures (none identified) |
| Section 9 | 7.6 | Completeness of risk control (coverage matrix) |
| Section 11 | 4.5 | Risk management file traceability |

---

## Appendix A: Extraction Metadata

| Attribute | Value |
|-----------|-------|
| Repository | /home/jakob/Noncomplicity/Repos/health-manager |
| Commit | 16e0273739eda65f73af5410323e1caf53362f16 |
| Extraction Date | 2026-03-06T00:00:00Z |
| Extractor Version | 1.0 |
| Standard | ISO 14971:2019 |

## Appendix B: Summary Statistics

| Metric | Count |
|--------|-------|
| Hazards Identified | 17 |
| Hazardous Situations | 17 |
| Initial ALARP Risks | 4 |
| Initial Acceptable Risks | 13 |
| Risk Controls Implemented | 34 |
| - Inherent Safety | 5 |
| - Protective Measures | 19 |
| - Information for Safety | 10 |
| Residual Risks (all acceptable) | 17 |
| Disclosures Required | 4 |
| Testing Gaps Identified | 7 |
| Process Gaps Identified | 1 |
