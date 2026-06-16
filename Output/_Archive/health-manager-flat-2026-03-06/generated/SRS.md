---
id: 725af57
title: "SRS"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Specification"
document_id: "SRS-health-manager-1.0"
software_safety_class: "C"
process: "[Software Development Process](../../../Canvases/Software%20Development%20Process.canvas)"
requirements: "[EN 62304 5.2](../../../Requirements/EN%2062304%205.2%20Software%20requirements%20analysis.md)"
owner: "[Head of Software Development](../../../Assets/Head%20of%20Software%20Development.md)"
---

# Software Requirements Specification

## 1. Introduction

### 1.1 Purpose

This Software Requirements Specification (SRS) documents the requirements for the health-manager system, a healthcare appointment management platform that enables patient consultations, clinical data recording, practitioner handover, EHR integration, e-prescription management, and payment processing. The system supports asynchronous, drop-in, and scheduled appointment workflows for cloud-based telemedicine delivery.

### 1.2 Scope

The health-manager system is a Spring Boot microservice that provides:

- **In scope:**
  - Appointment creation, retrieval, and lifecycle management
  - Consultation creation and management
  - Practitioner-to-practitioner handover workflows
  - Role-based access control and authorization
  - Integration with external systems (Directory, MeetingPlace, Notifier, TakeCare EHR, e-prescription services, Bookings, Configuration)
  - JMS event publishing for system integration
  - Clinical data persistence with audit trails
  - Payment processing integration
  - Database schema migration management
  - Monitoring and observability endpoints

- **Out of scope:**
  - Video conferencing functionality (provided by MeetingPlace)
  - Patient and practitioner identity management (provided by Directory)
  - Push notification delivery (provided by Notifier)
  - Electronic health record storage (provided by TakeCare)
  - E-prescription transmission (provided by external pharmacy systems)
  - Payment gateway services (provided by Klarna)
  - User interface rendering (provided by web and mobile clients)

### 1.3 Intended Use

**Medical Function:** Healthcare appointment management system enabling patient consultations, clinical data recording, handover between practitioners, EHR integration (TakeCare), e-prescription management, and payment processing.

**Intended Users:**
- Clinicians (doctors, nurses)
- Patients and their authorized representatives
- Administrative staff (secretaries)
- System integrators

**Use Environment:** Cloud-based SaaS platform accessed via web browsers and mobile applications. Integrates with external systems: Directory (practitioner/patient data), MeetingPlace (video consultations), Notifier (push notifications), TakeCare EHR, e-prescription services, Klarna payment gateway.

**Operating Principle:** Spring Boot microservice with JPA persistence to MySQL/MariaDB, JMS messaging for event distribution, REST API for client access, role-based access control via JWT, transactional integrity via @Transactional annotations.

### 1.4 Software Safety Classification

This software is classified as **Class C** per IEC 62304:2006+A1:2015.

**Classification Rationale:**

The health-manager system is classified as Class C (highest safety classification) based on the following analysis:

1. **Clinical data integrity** - The system stores and retrieves clinical notes, diagnoses, and prescription information. Data corruption or loss could directly impact patient treatment decisions, potentially leading to serious injury or death.

2. **Practitioner responsibility tracking** - The system maintains authoritative records of which practitioner is responsible for patient care at any given time, particularly during handover workflows. Incorrect assignment could result in care gaps where no practitioner responds to deteriorating patient conditions.

3. **Authorization and access control** - The system enforces access boundaries preventing unauthorized viewing of sensitive medical records. Breaches could violate patient privacy and, if combined with malicious intent, could lead to patient harm through medical identity theft or fraud.

4. **Audit trail requirements** - The system provides legally required audit logs for medico-legal investigations and regulatory compliance (MDR, GDPR). Loss of audit trails could prevent root cause analysis of adverse events.

5. **Integration dependencies** - The system integrates with TakeCare EHR for clinical documentation and e-prescription services for medication management. Integration failures could result in fragmented medical records or delayed medication access.

6. **Safety requirements implementation** - Multiple requirements (SRS-SAF-001 through SRS-SAF-005) implement risk controls for identified hazards including cross-care-provider data exposure, practitioner validation, and state machine enforcement.

Per IEC 62304 clause 4.3, software that could contribute to a hazardous situation resulting in death or serious injury is classified as Class C. The health-manager system's role in clinical workflow coordination, data integrity, and practitioner responsibility assignment places it in this category.

### 1.5 Definitions and Abbreviations

| Term | Definition |
|------|------------|
| API | Application Programming Interface |
| ALARP | As Low As Reasonably Practicable |
| EHR | Electronic Health Record |
| GDPR | General Data Protection Regulation |
| HAZ | Hazard identifier |
| HS | Hazardous Situation identifier |
| IEC | International Electrotechnical Commission |
| ISO | International Organization for Standardization |
| JMS | Java Message Service |
| JPA | Java Persistence API |
| JWT | JSON Web Token |
| MDR | Medical Device Regulation (EU 2017/745) |
| ORM | Object-Relational Mapping |
| RBAC | Role-Based Access Control |
| RC | Risk Control identifier |
| REST | Representational State Transfer |
| SOUP | Software of Unknown Provenance |
| SRS | Software Requirements Specification |
| UTC | Coordinated Universal Time |

### 1.6 Referenced Documents

| Document | Version | Relationship |
|----------|---------|--------------|
| IEC 62304:2006+AMD1:2015 | 2015 | Regulatory standard for medical device software |
| ISO 14971:2019 | 2019 | Risk management for medical devices |
| EU MDR 2017/745 | 2017 | Medical Device Regulation |
| ISO 13485:2016 | 2016 | Quality management systems for medical devices |
| requirements.json | 2026-03-06 | Extracted requirements from source code |
| risks.json | 2026-03-06 | Risk analysis and hazard identification |
| handover_architecture.md | - | Handover feature architecture specification |
| software_requirements.md | - | Original software requirements document |

## 2. Functional Requirements

### 2.1 Appointment Management

#### SRS-APPT-001: Create Appointment
**Description:** The system shall allow authenticated practitioners, patients, and system users to create appointments with specified type (CONSULTATION, DROP_IN, SCHEDULED), episode of care, practitioner assignment, and scheduling information.

**Type:** Functional

**Priority:** Essential

**Safety Class:** B

**Safety Rationale:** Appointment creation affects patient scheduling and care delivery; incorrect implementation could delay or prevent access to care.

**Source:**
- Code: `svc/src/main/java/se/alerisx/mhp/manager/controller/v1/AppointmentController.java:92-140`
- Code: `svc/src/main/java/se/alerisx/mhp/manager/domain/Appointment.java:1-150`
- Code: `svc/src/main/java/se/alerisx/mhp/manager/service/UpdateAppointmentService.java`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Parent Requirement:** N/A

**Verification:**
- Test: `svc/src/test/java/se/alerisx/mhp/manager/service/impl/GetAppointmentServiceImplTest.java`

**Dependencies:** None

---

#### SRS-APPT-002: Retrieve Appointment by ID
**Description:** The system shall allow authorized users to retrieve appointment details by appointment ID. Access control shall enforce role-based permissions: practitioners can view appointments in their care unit, patients can view their own appointments, system users have unrestricted access.

**Type:** Functional

**Priority:** Essential

**Safety Class:** B

**Safety Rationale:** Appointment retrieval provides clinical context for care decisions; incorrect data could lead to inappropriate treatment.

**Source:**
- Code: `svc/src/main/java/se/alerisx/mhp/manager/controller/v1/AppointmentController.java:69-90`
- Code: `svc/src/main/java/se/alerisx/mhp/manager/service/GetAppointmentService.java`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Parent Requirement:** N/A

**Verification:**
- Test: `svc/src/test/java/se/alerisx/mhp/manager/service/impl/GetAppointmentServiceImplTest.java`

**Dependencies:** SRS-SEC-001, SRS-SEC-002

---

#### SRS-APPT-003: Update Appointment Status and Attributes
**Description:** The system shall allow authorized practitioners and system users to update appointment status (queued, started, ended, cancelled), practitioner assignment, scheduling time, and other attributes according to business rules and authorization policies.

**Type:** Functional

**Priority:** Essential

**Safety Class:** B

**Safety Rationale:** Appointment updates affect clinical workflow and patient tracking; status changes must be accurate and auditable.

**Source:**
- Code: `svc/src/main/java/se/alerisx/mhp/manager/controller/v1/AppointmentController.java`
- Code: `svc/src/main/java/se/alerisx/mhp/manager/service/UpdateAppointmentService.java`
- Code: `svc/src/main/java/se/alerisx/mhp/manager/service/impl/UpdateAppointmentServiceImpl.java`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Parent Requirement:** N/A

**Verification:** Testing coverage incomplete

**Dependencies:** SRS-SEC-001, SRS-SEC-002, SRS-AUD-001

---

#### SRS-APPT-004: Appointment State Tracking with Timestamps
**Description:** The system shall track appointment state transitions with microsecond-precision timestamps for key events: notifiedAt, queuedAt, bookedAt, startedAt, endedAt, cancelledAt, endFollowUpAt, sendFollowUpMessageAt. Timestamps shall use TIMESTAMP(6) precision in database storage.

**Type:** Functional

**Priority:** Essential

**Safety Class:** C

**Safety Rationale:** Accurate timestamp tracking is critical for clinical documentation, compliance reporting, and audit trails required for patient safety investigations.

**Source:**
- Code: `svc/src/main/java/se/alerisx/mhp/manager/domain/Appointment.java:106-140`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Parent Requirement:** N/A

**Verification:** Database schema validation

**Dependencies:** SRS-AUD-001, SRS-DATA-001

---

### 2.2 Appointment Handover

#### SRS-HAND-001: Propose Appointment Handover
**Description:** The system shall allow a practitioner to propose handing over an appointment to another practitioner or role. The handover proposal shall specify target care unit ID, practitioner role, optional practitioner capability, optional specific practitioner ID, and optional handover notes (max 1024 characters). Handover can only be proposed when the appointment is in HANDOVER_ENABLED status.

**Type:** Functional

**Priority:** Essential

**Safety Class:** B

**Safety Rationale:** Handover initiation affects continuity of care; incorrect handover could result in care gaps or practitioner confusion about responsibility.

**Source:**
- Code: `handover_architecture.md:1-150`
- Code: `software_requirements.md:1-200`
- Code: `svc/src/main/java/se/alerisx/mhp/manager/service/impl/AppointmentHandoverServiceImpl.java`
- Code: `dto/src/main/java/se/alerisx/mhp/manager/dto/UpdateAppointmentDTO.java`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Parent Requirement:** N/A

**Verification:** Testing coverage incomplete

**Dependencies:** SRS-HAND-002, SRS-HAND-003, SRS-SEC-003

**Related Risk Controls:** RC-001, RC-005, RC-006, RC-007, RC-013, RC-014, RC-021, RC-023

---

#### SRS-HAND-002: Accept Appointment Handover
**Description:** The system shall allow the target practitioner to accept a proposed handover. Upon acceptance, the system shall update the appointment's practitioner assignment, change handover status to HANDOVER_ACCEPTED, create an audit event, publish a JMS event, send notifications to the original practitioner, and post a handover message to the patient meeting.

**Type:** Functional

**Priority:** Essential

**Safety Class:** B

**Safety Rationale:** Handover acceptance transfers clinical responsibility; the system must reliably update practitioner assignment and notify all parties to prevent care gaps.

**Source:**
- Code: `software_requirements.md:38-54`
- Code: `handover_architecture.md:144-150`
- Code: `svc/src/main/java/se/alerisx/mhp/manager/service/impl/AppointmentHandoverServiceImpl.java`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Parent Requirement:** N/A

**Verification:** Testing coverage incomplete

**Dependencies:** SRS-HAND-001, SRS-INT-002, SRS-INT-003, SRS-AUD-002

**Related Risk Controls:** RC-001, RC-003, RC-004, RC-011, RC-012, RC-015, RC-016, RC-019

---

#### SRS-HAND-003: Deny or Cancel Appointment Handover
**Description:** The system shall allow the target practitioner to deny a handover proposal or the original practitioner to cancel it. The system shall change status to HANDOVER_DENIED or HANDOVER_CANCELLED, create audit events, publish JMS events, send notifications, and post messages to the meeting. The appointment's practitioner assignment shall remain unchanged.

**Type:** Functional

**Priority:** Essential

**Safety Class:** B

**Safety Rationale:** Handover denial/cancellation must maintain clarity about clinical responsibility to avoid confusion about who is caring for the patient.

**Source:**
- Code: `software_requirements.md:38-54`
- Code: `svc/src/main/java/se/alerisx/mhp/manager/service/impl/AppointmentHandoverServiceImpl.java`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Parent Requirement:** N/A

**Verification:** Testing coverage incomplete

**Dependencies:** SRS-HAND-001, SRS-HAND-002, SRS-AUD-002

---

#### SRS-HAND-004: Handover State Machine Enforcement
**Description:** The system shall enforce a state machine for handover status transitions: HANDOVER_ENABLED → HANDOVER_PROPOSED (via propose action), HANDOVER_PROPOSED → HANDOVER_ACCEPTED (via accept action), HANDOVER_PROPOSED → HANDOVER_DENIED (via deny action), HANDOVER_PROPOSED → HANDOVER_CANCELLED (via cancel action). Invalid state transitions shall be rejected with error messages.

**Type:** Functional

**Priority:** Essential

**Safety Class:** C

**Safety Rationale:** State machine enforcement prevents invalid handover sequences that could create ambiguous clinical responsibility and patient safety risks.

**Source:**
- Code: `software_requirements.md:37-54`
- Code: `handover_architecture.md:144-150`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Parent Requirement:** N/A

**Verification:** Testing coverage incomplete

**Dependencies:** SRS-HAND-001, SRS-HAND-002, SRS-HAND-003

**Related Risk Controls:** RC-012, RC-017, RC-018

---

#### SRS-HAND-005: Handover Authorization Rules
**Description:** The system shall validate handover authorization: (1) handover must be enabled for the care unit via feature flag, (2) target care unit and practitioner role must be valid, (3) cross-care-provider handovers require explicit feature enablement, (4) only the handover owner can cancel a proposed handover, (5) only authorized recipients can accept or deny handovers.

**Type:** Functional

**Priority:** Essential

**Safety Class:** C

**Safety Rationale:** Authorization controls prevent unauthorized handovers that could compromise patient care by transferring responsibility to inappropriate or unqualified practitioners.

**Source:**
- Code: `software_requirements.md:114-127`
- Code: `svc/src/main/java/se/alerisx/mhp/manager/service/impl/AppointmentHandoverServiceImpl.java`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Parent Requirement:** N/A

**Verification:** Testing coverage incomplete

**Dependencies:** SRS-HAND-001, SRS-SEC-003

**Related Risk Controls:** RC-007, RC-008, RC-009, RC-010

---

### 2.3 Consultation Management

#### SRS-CONS-001: Create Consultation
**Description:** The system shall allow practitioners with ROLE_PRACTITIONER to create a consultation associated with an appointment. The consultation shall include care unit ID, practitioner ID, practitioner role, optional practitioner capability, and origin ID. If capabilities are enabled and not optional for the care unit, capability must be provided.

**Type:** Functional

**Priority:** Essential

**Safety Class:** B

**Safety Rationale:** Consultation creation initiates clinical encounters; incorrect practitioner assignment or missing capabilities could affect care quality.

**Source:**
- Code: `svc/src/main/java/se/alerisx/mhp/manager/controller/v1/clinic/ConsultationController.java:37-57`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Parent Requirement:** N/A

**Verification:** Testing coverage incomplete

**Dependencies:** SRS-APPT-001, SRS-SEC-001

---

#### SRS-CONS-002: Retrieve Consultation by Appointment ID
**Description:** The system shall allow practitioners with read access to an appointment to retrieve the associated consultation by consultation appointment ID. Access control shall be enforced via @PreAuthorize annotation.

**Type:** Functional

**Priority:** Essential

**Safety Class:** B

**Safety Rationale:** Consultation retrieval provides clinical context; unauthorized access could breach patient privacy.

**Source:**
- Code: `svc/src/main/java/se/alerisx/mhp/manager/controller/v1/clinic/ConsultationController.java:59-66`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Parent Requirement:** N/A

**Verification:** Testing coverage incomplete

**Dependencies:** SRS-SEC-002, SRS-APPT-002

---

## 3. Interface Requirements

### 3.1 External System Interfaces

#### SRS-INT-001: Directory Service Integration
**Description:** The system shall integrate with Directory Service (directory2) via REST/HTTP to retrieve patient information, validate practitioner IDs, retrieve practitioner names, and manage user relationships. The directory URL shall be configurable via application.yml (alerisx.directory2.url).

**Type:** Interface

**Priority:** Essential

**Safety Class:** B

**Safety Rationale:** Directory integration provides current patient and practitioner data; incorrect data could lead to wrong patient identification or practitioner assignment.

**Source:**
- Code: `svc/src/main/resources/application.yml:100-102`
- Code: `software_requirements.md:64`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Protocol:** HTTP REST

**Data Format:** JSON

**Error Handling:** HTTP status codes, exception handling in service layer

**Verification:** Integration testing with Directory test environment

**Dependencies:** None

**Related Risk Controls:** RC-005

---

#### SRS-INT-002: Notifier Service Integration
**Description:** The system shall integrate with Notifier Service via REST/HTTP to send clinic notifications including handover notifications (type=HANDOVER). Notifications shall include care unit ID, practitioner ID, appointment ID, notification type, and metadata map. The notifier URL shall be configurable (alerisx.notifier.url).

**Type:** Interface

**Priority:** Essential

**Safety Class:** B

**Safety Rationale:** Notifications ensure practitioners are aware of handovers and other critical events; missed notifications could delay response to patient needs.

**Source:**
- Code: `svc/src/main/resources/application.yml:112`
- Code: `software_requirements.md:62`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Protocol:** HTTP REST

**Data Format:** JSON

**Error Handling:** Retry logic, error logging, dual-channel notification (JMS fallback)

**Verification:** Integration testing with Notifier test environment

**Dependencies:** None

**Related Risk Controls:** RC-001

---

#### SRS-INT-003: MeetingPlace Service Integration
**Description:** The system shall integrate with MeetingPlace Service via REST/HTTP to send handover messages to patient-practitioner chat meetings, add practitioners to meetings, and manage meeting participants. The system shall subscribe to messageEventTopic for incoming meeting events. The meetingplace URL shall be configurable (alerisx.meetingplace.url).

**Type:** Interface

**Priority:** Essential

**Safety Class:** B

**Safety Rationale:** Meeting integration enables patient-practitioner communication; failed integration could prevent critical care coordination discussions.

**Source:**
- Code: `svc/src/main/resources/application.yml:84-86`
- Code: `software_requirements.md:63`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Protocol:** HTTP REST, JMS

**Data Format:** JSON

**Error Handling:** Exception handling, logging

**Verification:** Integration testing

**Dependencies:** None

**Related Risk Controls:** RC-015, RC-016

---

#### SRS-INT-004: JMS Topic Publishing for Events
**Description:** The system shall publish domain events to JMS topics using Apache Artemis message broker. Event types include: appointmentHandoverEventTopic, consultationEventTopic, appointmentEventTopic, creditStatusEventTopic, paymentEventTopic, followUpEventTopic, scheduledAppointmentEventTopic, healthManagerEventTopic, shiftEventTopic, noteEventTopic, referralEventTopic, escalationEventTopic, newParticipantInMeetingEventTopic. Events shall be published in pub-sub mode.

**Type:** Interface

**Priority:** Essential

**Safety Class:** B

**Safety Rationale:** Event publishing enables system integration and real-time updates; event loss could prevent downstream systems from responding to critical state changes.

**Source:**
- Code: `svc/src/main/resources/application.yml:56-64`
- Code: `svc/src/main/resources/application.yml:162-177`
- Code: `software_requirements.md:59-68`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Protocol:** JMS (Apache Artemis)

**Data Format:** Serialized Java objects

**Error Handling:** JMS delivery guarantees, error logging

**Verification:** Integration testing with message broker

**Dependencies:** None

**Related Risk Controls:** RC-001, RC-002

---

#### SRS-INT-005: Bookings Service Integration
**Description:** The system shall integrate with Bookings Service via REST/HTTP for appointment scheduling and booking operations. The system shall subscribe to synchronizedBookingV2Topic for booking synchronization events. The bookings URL shall be configurable (alerisx.bookings.url).

**Type:** Interface

**Priority:** Essential

**Safety Class:** B

**Safety Rationale:** Booking integration manages appointment scheduling; integration failures could prevent patients from booking appointments.

**Source:**
- Code: `svc/src/main/resources/application.yml:80-82`
- Code: `svc/src/main/java/se/alerisx/mhp/manager/conf/HealthManagerConfiguration.java:38-41`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Protocol:** HTTP REST, JMS

**Data Format:** JSON

**Error Handling:** Standard HTTP error handling

**Verification:** Integration testing

**Dependencies:** None

---

#### SRS-INT-006: Configuration Service Integration
**Description:** The system shall integrate with System Configuration Service to retrieve care unit registry, origin registry, partner registry, customization settings, capabilities, practitioner roles, options, rules, menu items, notes, and plugin services. Configuration shall support test mode and primary source selection. Object storage integration shall use ZeroMQ protocol.

**Type:** Interface

**Priority:** Essential

**Safety Class:** B

**Safety Rationale:** Configuration controls system behavior including feature flags and clinical workflows; incorrect configuration could enable unsafe features or disable safety controls.

**Source:**
- Code: `svc/src/main/resources/application.yml:114-143`
- Code: `software_requirements.md:66`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Protocol:** ZeroMQ

**Data Format:** Configuration-dependent

**Error Handling:** Fallback to defaults, error logging

**Verification:** Configuration validation testing

**Dependencies:** None

**Related Risk Controls:** RC-007, RC-008, RC-013

---

## 4. Performance Requirements

### 4.1 Response Time Requirements

#### SRS-PERF-002: Query Timeout Enforcement
**Description:** The system shall enforce a 30-second timeout on JPA queries via javax.persistence.query.timeout property. Queries exceeding this timeout shall be cancelled to prevent resource exhaustion.

**Type:** Performance

**Priority:** Essential

**Safety Class:** B

**Safety Rationale:** Query timeouts prevent database lock-ups that could make the system unresponsive and delay critical patient care operations.

**Source:**
- Code: `svc/src/main/resources/application.yml:54`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:** Performance testing with long-running queries

**Dependencies:** SRS-DATA-001

---

#### SRS-PERF-006: Remote Client Timeout Configuration
**Description:** The system shall configure remote HTTP client timeouts with 10-second connect timeout and 5-minute read timeout for all external service integrations (directory, bookings, meetingplace, etc.). Timeouts shall be configurable via alerisx.remote-client properties.

**Type:** Performance

**Priority:** Essential

**Safety Class:** B

**Safety Rationale:** Client timeouts prevent indefinite blocking on external services; timeouts ensure the system remains responsive even when external services are slow or unavailable.

**Source:**
- Code: `svc/src/main/resources/application.yml:74-78`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:** Timeout testing with simulated slow services

**Dependencies:** None

---

### 4.2 Capacity Requirements

#### SRS-PERF-001: Database Connection Pool Configuration
**Description:** The system shall configure HikariCP connection pool with minimum idle connections (default 10), maximum pool size (default 100), and connection timeout (default 5000ms). Connection pool settings shall be environment-configurable via mysql.connections.* properties.

**Type:** Performance

**Priority:** Essential

**Safety Class:** Unclassified

**Safety Rationale:** Connection pool configuration affects system availability and response time but does not directly impact clinical safety.

**Source:**
- Code: `svc/src/main/resources/application.yml:16-21`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:** Load testing with concurrent database access

**Dependencies:** SRS-DATA-001

---

### 4.3 Caching Requirements

#### SRS-PERF-003: Hibernate Query Plan Caching
**Description:** The system shall cache Hibernate query plans (default 512 entries) and query parameter metadata (default 128 entries) to improve query execution performance. Cache sizes shall be configurable via hibernate.query.cache.max.size and hibernate.query.metadata.max.size.

**Type:** Performance

**Priority:** Desirable

**Safety Class:** Unclassified

**Safety Rationale:** Query plan caching improves performance but does not directly affect clinical safety.

**Source:**
- Code: `svc/src/main/resources/application.yml:50-52`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:** Performance benchmarking

**Dependencies:** None

---

#### SRS-PERF-004: Application-Level Caching
**Description:** The system shall implement application-level caching with configurable TTL and max size for: practitioner appointments (clinic list cache), practitioner lookups, care unit lookups, authorization checks, and audit resource reflection. Default cache TTL and sizes shall be configurable via cache.timeToLive and cache.maxSize properties.

**Type:** Performance

**Priority:** Essential

**Safety Class:** B

**Safety Rationale:** Caching improves response times for clinical operations; stale cache data could show outdated appointment status or authorization, but cache TTLs mitigate this risk.

**Source:**
- Code: `svc/src/main/resources/application.yml:185-238`
- Code: `svc/src/main/java/se/alerisx/mhp/manager/security/AppointmentAuthorizer.java:48-58`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:** Cache behavior testing, TTL validation

**Dependencies:** None

---

#### SRS-PERF-007: Asynchronous Processing with Thread Pool
**Description:** The system shall provide a fixed thread pool executor with 9 threads for parallel processing of combiner operations. Thread pool shall be managed as a Spring bean.

**Type:** Performance

**Priority:** Desirable

**Safety Class:** Unclassified

**Safety Rationale:** Thread pool configuration affects system performance but does not directly impact clinical safety.

**Source:**
- Code: `svc/src/main/java/se/alerisx/mhp/manager/conf/HealthManagerConfiguration.java:43-46`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:** Thread pool behavior testing

**Dependencies:** None

---

### 4.4 Testing Performance

#### SRS-PERF-005: Parallel Test Execution
**Description:** The system shall execute unit tests in parallel at class level with 4 threads and unlimited thread count option. Integration tests shall execute in parallel at method level. Parallel execution shall be enabled via JUnit Jupiter parallel execution.

**Type:** Performance

**Priority:** Desirable

**Safety Class:** Unclassified

**Safety Rationale:** Test execution performance does not directly affect production clinical safety.

**Source:**
- Code: `pom.xml:197-202`
- Code: `pom.xml:208-213`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:** Build time measurement

**Dependencies:** None

---

## 5. Safety Requirements

> These requirements implement risk control measures identified in the Risk Management File (risks.json).

### 5.1 Handover Safety Controls

#### SRS-SAF-001: Handover Cross-Care-Provider Validation
**Description:** The system shall prevent handovers across different care providers unless explicitly enabled via feature flag (ENABLE_HANDOVERS_ACROSS_CARE_PROVIDERS). This prevents unintended patient data sharing across organizational boundaries and ensures data governance compliance.

**Type:** Safety

**Priority:** Essential

**Safety Class:** C

**Related Hazard:** HAZ-005 (Cross-care-provider data exposure)

**Related Hazardous Situation:** HS-005 (ENABLE_HANDOVERS_ACROSS_CARE_PROVIDERS check bypassed)

**Risk Control Measure:** RC-007, RC-008, RC-009, RC-010 - Feature flag validation, care provider boundary enforcement, error messaging, consent checking

**Source:**
- Code: `software_requirements.md:122`
- Code: `svc/src/main/java/se/alerisx/mhp/manager/service/impl/AppointmentHandoverServiceImpl.java`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Test: `handoverAppointmentPropose_CaseFailOnDifferentCareProvider()`
- Method: Unit testing, integration testing

---

#### SRS-SAF-002: Practitioner Validation for Handover
**Description:** The system shall validate that the target practitioner ID exists in the Directory Service when a specific practitioner is named in a handover proposal. Invalid practitioner IDs shall be rejected with error message 'Appointment [%s] at careUnit %s cannot be handed over to an invalid practitioner %s'.

**Type:** Safety

**Priority:** Essential

**Safety Class:** C

**Related Hazard:** HAZ-004 (Invalid practitioner validation bypass)

**Related Hazardous Situation:** HS-004 (DirectoryService.isValidPractitioner() returns false positive)

**Risk Control Measure:** RC-005, RC-006 - DirectoryService validation, error messaging

**Source:**
- Code: `software_requirements.md:123`
- Code: `svc/src/main/java/se/alerisx/mhp/manager/service/impl/AppointmentHandoverServiceImpl.java`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Test: `handoverAppointmentPropose_CaseFailOnHandoverToInvalidPractitionerId()`
- Method: Unit testing, integration testing with Directory mock

---

#### SRS-SAF-003: Appointment State Validation for Handover
**Description:** The system shall validate that appointments are in an appropriate state for handover operations. Handovers can only be proposed when handover is enabled. Accept/deny operations require an active (PROPOSED) handover. Invalid state transitions shall be rejected with descriptive error messages.

**Type:** Safety

**Priority:** Essential

**Safety Class:** C

**Related Hazard:** HAZ-009 (Invalid state machine transition)

**Related Hazardous Situation:** HS-009 (Invalid state transition attempted)

**Risk Control Measure:** RC-017, RC-018 - State machine validation, error messaging

**Source:**
- Code: `software_requirements.md:118-119`
- Code: `svc/src/main/java/se/alerisx/mhp/manager/service/impl/AppointmentHandoverServiceImpl.java`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Test: `handoverAppointmentPropose_CaseFailOnAlreadyActiveHandoverProcess()`
- Method: State machine testing

---

#### SRS-SAF-004: Handover Ownership Validation
**Description:** The system shall enforce that only the handover owner (practitioner who initiated the handover) can cancel a proposed handover. Attempts by other practitioners to cancel shall be rejected with error: 'Handover for appointment [%s] at careUnit %s cannot be cancelled by %s because it is owned by %s. Try denying instead.'

**Type:** Safety

**Priority:** Essential

**Safety Class:** B

**Related Hazard:** HAZ-009 (Invalid state machine transition)

**Related Hazardous Situation:** HS-009 (Invalid state transition attempted)

**Risk Control Measure:** Authorization validation in cancel operation

**Source:**
- Code: `software_requirements.md:125`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Method: Unit testing with ownership scenarios

---

#### SRS-SAF-005: Mandatory Practitioner Role for Handover
**Description:** The system shall require that a practitioner role be supplied when initiating a handover. Missing practitionerRole shall be rejected with error: 'A practitionerRole for the practitioner initiating the handover has to be supplied.'

**Type:** Safety

**Priority:** Essential

**Safety Class:** B

**Related Hazard:** HAZ-004 (Invalid practitioner validation bypass)

**Risk Control Measure:** Input validation at handover initiation

**Source:**
- Code: `software_requirements.md:120`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Method: Input validation testing

---

## 6. Security Requirements

### 6.1 Authentication and Authorization

#### SRS-SEC-001: Role-Based Access Control (RBAC)
**Description:** The system shall enforce role-based access control using Spring Security annotations (@Secured, @PreAuthorize). Supported roles include: ROLE_PATIENT, ROLE_PRACTITIONER, ROLE_PRACTITIONER_LIMITED_ACCESS, ROLE_SECRETARY, ROLE_SYSTEM, ROLE_ADMIN, ROLE_SUPERADMIN, ROLE_IRREGULARITY_ASSESSOR, ROLE_ANONYMOUS. Each endpoint shall specify allowed roles.

**Type:** Security

**Priority:** Essential

**Safety Class:** C

**Safety Rationale:** RBAC prevents unauthorized access to patient health information and clinical functions, protecting patient privacy and data integrity.

**Source:**
- Code: `svc/src/main/java/se/alerisx/mhp/manager/controller/v1/AppointmentController.java:53-59`
- Code: `svc/src/main/java/se/alerisx/mhp/manager/controller/v1/AppointmentController.java:69-94`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Method: Security testing, penetration testing

**Dependencies:** None

---

#### SRS-SEC-002: Appointment-Level Authorization
**Description:** The system shall enforce fine-grained authorization for appointment access via AppointmentAuthorizer component. Practitioners can access appointments in their care unit or for which they have explicit consent. Patients can access their own appointments. PRACTITIONER_LIMITED_ACCESS role is restricted to appointments for patients in their 'pids' claim. Authorization checks shall be cached for performance.

**Type:** Security

**Priority:** Essential

**Safety Class:** C

**Safety Rationale:** Fine-grained authorization prevents unauthorized access to individual patient appointments, protecting sensitive health information from disclosure.

**Related Hazard:** HAZ-013 (Unauthorized access to clinical data)

**Related Hazardous Situation:** HS-013 (JWT validation bypassed or authorization check fails)

**Risk Control Measure:** RC-024, RC-025, RC-034 - AppointmentAuthorizer validation, care provider boundary enforcement, audit logging

**Source:**
- Code: `svc/src/main/java/se/alerisx/mhp/manager/security/AppointmentAuthorizer.java:48-99`
- Code: `svc/src/main/java/se/alerisx/mhp/manager/controller/v1/AppointmentController.java:71`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Method: Authorization integration tests

**Dependencies:** SRS-SEC-001

---

#### SRS-SEC-003: JWT-Based Authentication
**Description:** The system shall authenticate users via JWT tokens issued by the configured issuer ('alerisx-mhp'). The JWT shall contain user identity (username), roles, patient IDs (for limited access practitioners), and other claims. JWT validation shall verify token signature, issuer, and expiration.

**Type:** Security

**Priority:** Essential

**Safety Class:** C

**Safety Rationale:** JWT authentication ensures only authenticated users can access the system, preventing unauthorized access to patient health records.

**Source:**
- Code: `svc/src/main/resources/application.yml:1-3`
- Code: `svc/src/main/java/se/alerisx/mhp/manager/controller/v1/AppointmentController.java:76`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Method: Authentication testing, token validation testing

**Dependencies:** None

---

### 6.2 Audit and Logging

#### SRS-SEC-004: Audit Logging with Throttling
**Description:** The system shall log access to sensitive operations using @Audit annotation. Audit logs shall be throttled (HOURLY, DAILY, etc.) to prevent log flooding. Audit events shall capture user identity, operation, resource identifiers, and timestamp.

**Type:** Security

**Priority:** Essential

**Safety Class:** C

**Safety Rationale:** Audit logging creates an immutable record of system access for security investigations, compliance reporting, and patient safety incident analysis.

**Related Hazard:** HAZ-010 (Audit log persistence failure)

**Related Hazardous Situation:** HS-010 (appointmentHandoverEventRepository.save() fails silently)

**Risk Control Measure:** RC-019, RC-020 - Database audit table, application logging

**Source:**
- Code: `svc/src/main/java/se/alerisx/mhp/manager/controller/v1/AppointmentController.java:69`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Method: Audit log verification testing

**Dependencies:** SRS-AUD-001

---

### 6.3 Data Protection

#### SRS-UI-004: Safe Character Validation
**Description:** The system shall validate user input for safe characters to prevent injection attacks. Allowed alphanumeric characters (default: '0-9a-zåäöæøáéëïü') and special characters (default: space, newline, tab, punctuation) shall be configurable via safeCharacters.alphanumeric and safeCharacters.special properties.

**Type:** Security

**Priority:** Essential

**Safety Class:** B

**Safety Rationale:** Input validation prevents injection attacks that could compromise patient data or system integrity.

**Related Hazard:** HAZ-017 (Data sanitization bypass)

**Related Hazardous Situation:** HS-017 (Malicious XML or script injection in clinical notes)

**Risk Control Measure:** RC-033, RC-028 - NoteXmlSanitizer, input validation

**Source:**
- Code: `svc/src/main/resources/application.yml:282-284`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Test: `svc/src/test/java/se/alerisx/mhp/manager/service/impl/SafeCharacterServiceImplTest.java`
- Method: Security testing with malicious payloads

**Dependencies:** SRS-SEC-001

---

## 7. Data Requirements

### 7.1 Data Persistence

#### SRS-DATA-001: MySQL/MariaDB Persistence
**Description:** The system shall persist all entities to MySQL/MariaDB database using Spring Data JPA and Hibernate ORM. Database connection shall use MariaDB JDBC driver (org.mariadb.jdbc.Driver). Hibernate shall use MySQL dialect and validate schema against DDL (ddl-auto: validate). All identifiers shall be globally quoted.

**Type:** Data

**Priority:** Essential

**Safety Class:** C

**Safety Rationale:** Database persistence stores all clinical data including appointments, consultations, and patient information; data loss or corruption could compromise patient safety.

**Source:**
- Code: `svc/src/main/resources/application.yml:10-52`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Method: Database integrity testing, backup/restore testing

**Dependencies:** None

---

#### SRS-DATA-002: Flyway Database Migration
**Description:** The system shall manage database schema versions using Flyway migration tool. Flyway shall baseline at version 1, baseline on migrate, ignore missing migrations, and use custom history table 'flyway_schema_history_v2'. Flyway shall be enabled by default (configurable via flyway.enabled).

**Type:** Data

**Priority:** Essential

**Safety Class:** C

**Safety Rationale:** Database migrations affect schema structure and data integrity; incorrect migrations could cause data loss or corruption affecting patient safety records.

**Related Hazardous Situation:** HS-003 (Database transaction failure)

**Risk Control Measure:** RC-003 - @Transactional annotation ensures atomicity

**Source:**
- Code: `svc/src/main/resources/application.yml:23-29`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Method: Migration testing, rollback testing

**Dependencies:** SRS-DATA-001

---

### 7.2 Data Schema

#### SRS-DATA-003: Appointment Handover Process Data Schema
**Description:** The system shall persist appointment handover processes in table 'appointment_handover_process' with fields: id (varchar(50) PK), created_at (timestamp(6)), version (int), appointment_id (varchar(50) FK UNIQUE), owner_id (varchar(50)), owner_role (varchar(50)), handover_status (varchar(50) indexed), handover_care_unit_id (varchar(50)), handover_practitioner_role (varchar(50)), handover_practitioner_capability (varchar(512) nullable), handover_practitioner_id (varchar(50) nullable), handover_notes (varchar(1024) nullable). Only one active process per appointment.

**Type:** Data

**Priority:** Essential

**Safety Class:** C

**Safety Rationale:** Handover process data tracks clinical responsibility transfer; data integrity is critical to maintain clear accountability for patient care.

**Related Hazard:** HAZ-006 (Concurrent handover race condition)

**Related Hazardous Situation:** HS-006 (Two practitioners simultaneously accept handover)

**Risk Control Measure:** RC-004, RC-011 - UNIQUE constraint on appointment_id

**Source:**
- Code: `software_requirements.md:173-200`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Method: Database schema validation, constraint testing

**Dependencies:** SRS-DATA-001, SRS-HAND-001

---

#### SRS-DATA-004: UTF-8 Character Encoding
**Description:** The system shall use utf8mb4 character set for all database tables to support full Unicode including emojis and international characters. This applies to all tables including appointment_handover_process and appointment_handover_event.

**Type:** Data

**Priority:** Essential

**Safety Class:** Unclassified

**Safety Rationale:** Character encoding supports international patient names and clinical notes but does not directly impact clinical safety.

**Source:**
- Code: `software_requirements.md:199`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Method: Character encoding testing

**Dependencies:** SRS-DATA-001

---

#### SRS-DATA-005: Optimistic Locking with Versioning
**Description:** The system shall implement optimistic locking using version fields in entities (e.g., appointment_handover_process.version). Version shall be incremented on each update. Concurrent update conflicts shall be detected and rejected.

**Type:** Data

**Priority:** Essential

**Safety Class:** B

**Safety Rationale:** Optimistic locking prevents lost updates in concurrent scenarios; prevents race conditions in handover status updates that could create conflicting clinical responsibilities.

**Related Hazard:** HAZ-006 (Concurrent handover race condition)

**Risk Control Measure:** Optimistic locking prevents concurrent modification

**Source:**
- Code: `software_requirements.md:184`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Method: Concurrency testing

**Dependencies:** SRS-DATA-001

---

### 7.3 Audit Data

#### SRS-AUD-001: Immutable Audit Event Storage
**Description:** The system shall store immutable audit events for significant state changes including handover status transitions in table 'appointment_handover_event'. Audit events shall include appointment_id, old_status, new_status, created_by, created_at, practitioner_id, and handover_process_id. Events shall never be updated or deleted (append-only).

**Type:** Data

**Priority:** Essential

**Safety Class:** C

**Safety Rationale:** Immutable audit trail is required for regulatory compliance and patient safety investigations; tampered or lost audit logs could prevent root cause analysis of adverse events.

**Related Hazard:** HAZ-010 (Audit log persistence failure)

**Related Hazardous Situation:** HS-010 (appointmentHandoverEventRepository.save() fails silently)

**Risk Control Measure:** RC-019, RC-020 - Database audit table, application logging

**Source:**
- Code: `software_requirements.md:201-230`
- Code: `handover_architecture.md:103-106`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Method: Audit log completeness testing

**Dependencies:** SRS-DATA-001

---

#### SRS-AUD-002: Handover Event Audit Logging
**Description:** The system shall log handover status changes at INFO level with format: 'Appointment {} has changed handover status from: {} to: {}. By {} {}. Current practitioner: {}'. Warning-level logs shall be created when handover process is null.

**Type:** Data

**Priority:** Essential

**Safety Class:** B

**Safety Rationale:** Application logs provide operational visibility for troubleshooting; missing logs could delay identification of handover process failures.

**Related Hazard:** HAZ-010 (Audit log persistence failure)

**Risk Control Measure:** RC-020 - Application logging as secondary audit mechanism

**Source:**
- Code: `software_requirements.md:159-170`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Method: Log verification testing

**Dependencies:** SRS-AUD-001

---

## 8. Operational Requirements

### 8.1 Configuration Management

#### SRS-OPER-001: Environment-Based Configuration
**Description:** The system shall support environment-based configuration via application.yml and application-local.yml files. Configuration properties shall be overridable via environment variables (e.g., mysql.url, jwt.secret, artemis.url). Sensitive values like passwords and secrets shall be externalized.

**Type:** Operational

**Priority:** Essential

**Safety Class:** B

**Safety Rationale:** Configuration management ensures correct system behavior in different environments; misconfiguration could enable unsafe features or disable security controls.

**Source:**
- Code: `svc/src/main/resources/application.yml:1-373`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Method: Configuration validation testing

**Dependencies:** None

---

#### SRS-OPER-004: Feature Flags
**Description:** The system shall support feature flags for controlling functionality including: FF_ENABLE_SERVICE_REQUESTS_MY_APPTS_2, FF_ENABLE_RECURRING_QUESTIONNAIRES_MY_APPTS_2, FF_ENABLE_MEDICAL_HISTORY_SYNC_FROM_INTERVIEWER (default true), FF_USE_CARE_PROVIDER_SERVICE. Feature flags shall be environment-configurable.

**Type:** Operational

**Priority:** Essential

**Safety Class:** B

**Safety Rationale:** Feature flags control system behavior and can enable/disable safety-relevant features; incorrect flag states could expose untested functionality.

**Source:**
- Code: `svc/src/main/resources/application.yml:368-372`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Method: Feature flag testing

**Dependencies:** SRS-OPER-001

---

### 8.2 Monitoring and Observability

#### SRS-OPER-002: Health Check and Monitoring Endpoints
**Description:** The system shall expose Actuator management endpoints for monitoring: dump, flyway, health, info, heapdump, loggers, metrics, prometheus, trace, threaddump. Endpoints shall be exposed via web interface without security restrictions (management.security.enabled: false).

**Type:** Operational

**Priority:** Essential

**Safety Class:** B

**Safety Rationale:** Health monitoring enables early detection of system issues; unavailable monitoring could delay detection of failures affecting patient care.

**Source:**
- Code: `svc/src/main/resources/application.yml:361-366`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Method: Endpoint availability testing

**Dependencies:** None

---

#### SRS-OPER-005: Distributed Tracing with W3C Trace Context
**Description:** The system shall implement distributed tracing using Spring Sleuth with W3C Trace Context propagation. Trace IDs shall be 128-bit. Trace context shall be propagated to external services and included in logs.

**Type:** Operational

**Priority:** Desirable

**Safety Class:** Unclassified

**Safety Rationale:** Distributed tracing aids troubleshooting but does not directly impact clinical safety.

**Source:**
- Code: `svc/src/main/resources/application.yml:31-35`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Method: Trace context verification

**Dependencies:** None

---

#### SRS-OPER-006: Structured Logging with Log Levels
**Description:** The system shall implement structured logging with configurable log levels: root INFO, Flyway DEBUG, application packages (se.alerisx, se.platform24) DEBUG, security INFO, HTTP client INFO, ActiveMQ WARN, Hibernate SQL INFO. Log levels shall be configurable per package.

**Type:** Operational

**Priority:** Essential

**Safety Class:** B

**Safety Rationale:** Appropriate log levels enable troubleshooting without log flooding; insufficient logging could delay diagnosis of issues affecting patient care.

**Source:**
- Code: `svc/src/main/resources/application.yml:336-359`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Method: Log output verification

**Dependencies:** None

---

### 8.3 Scheduled Operations

#### SRS-OPER-003: Scheduled Background Jobs
**Description:** The system shall execute scheduled background jobs including: scheduled today notifications (default 8am daily cron), waiting room scheduler, async appointment scheduler (default 11:15pm daily), external prescription sync (default every minute), and alert checks for waiting room (default every 15 minutes). Schedulers shall be configurable and enable/disable via feature flags.

**Type:** Operational

**Priority:** Essential

**Safety Class:** B

**Safety Rationale:** Scheduled jobs perform critical functions like appointment reminders and prescription syncing; job failures could prevent patient notifications or medication management.

**Related Hazard:** HAZ-015 (Prescription transmission error)

**Risk Control Measure:** RC-029 - Retry logic in ExternalPrescriptionService

**Source:**
- Code: `svc/src/main/resources/application.yml:146-157`
- Code: `svc/src/main/resources/application.yml:268-272`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Method: Scheduler execution testing

**Dependencies:** None

---

## 9. User Interface Requirements

### 9.1 API Requirements

#### SRS-UI-001: RESTful API Endpoints for Web and Mobile Clients
**Description:** The system shall expose RESTful HTTP APIs at /v1/, /v2/, and /v3/ paths for web and mobile client consumption. APIs shall use JSON for request/response payloads. API versioning shall allow backward compatibility and gradual migration.

**Type:** UI

**Priority:** Essential

**Safety Class:** B

**Safety Rationale:** API interfaces deliver clinical data to user interfaces; API errors could present incorrect information to practitioners or patients.

**Source:**
- Code: `svc/src/main/java/se/alerisx/mhp/manager/controller/v1/AppointmentController.java:47`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Method: API contract testing

**Dependencies:** None

---

#### SRS-UI-002: OpenAPI Documentation
**Description:** The system shall provide OpenAPI (formerly Swagger) documentation for API endpoints using Eclipse MicroProfile OpenAPI annotations. APIs shall be tagged and categorized (e.g., 'Appointment API V1', 'LEGACY'). Deprecated endpoints shall be marked with @Operation(deprecated = true).

**Type:** UI

**Priority:** Desirable

**Safety Class:** Unclassified

**Safety Rationale:** API documentation aids development but does not directly affect clinical safety.

**Source:**
- Code: `svc/src/main/java/se/alerisx/mhp/manager/controller/v1/AppointmentController.java:45-50`
- Code: `svc/src/main/java/se/alerisx/mhp/manager/controller/v1/AppointmentController.java:72`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Method: Documentation review

**Dependencies:** None

---

### 9.2 Internationalization

#### SRS-UI-003: Internationalization (i18n) for User Messages
**Description:** The system shall support internationalized user-facing messages using TranslationService. Message keys for handover include: appointment_handover_proposed, appointment_handover_accepted, appointment_handover_denied, appointment_handover_cancelled. Messages shall support template variables (proposedBy, proposedTo, careUnitName, etc.).

**Type:** UI

**Priority:** Essential

**Safety Class:** B

**Safety Rationale:** Internationalized messages ensure users understand system behavior; mistranslated or missing translations could lead to misunderstanding of clinical workflow.

**Related Hazard:** HAZ-008 (Patient chat message delivery failure)

**Risk Control Measure:** RC-016 - Internationalized chat messages

**Source:**
- Code: `software_requirements.md:135-151`
- Code: `svc/src/main/resources/application.yml:296`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Method: Translation testing

**Dependencies:** None

---

## 10. Regulatory Requirements

### 10.1 GDPR Compliance

#### SRS-REG-001: GDPR Data Erasure
**Description:** The system shall provide data erasure functionality via SystemDataErasureController to comply with GDPR right to erasure. System administrators shall be able to trigger data erasure for specified patient or practitioner identifiers.

**Type:** Regulatory

**Priority:** Essential

**Safety Class:** C

**Safety Rationale:** GDPR compliance is legally required; incorrect erasure could violate privacy regulations or accidentally delete active patient records.

**Source:**
- Code: `svc/src/main/java/se/alerisx/mhp/manager/controller/v1/system/SystemDataErasureController.java`
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Method: Data erasure testing

**Dependencies:** None

---

### 10.2 Medical Device Regulation Compliance

#### SRS-REG-002: Medical Device Regulation (MDR) Compliance
**Description:** The system shall maintain audit trails, traceability, and documentation to comply with EU MDR 2017/745. This includes immutable audit events, version control of configuration, and structured logging of clinical operations.

**Type:** Regulatory

**Priority:** Essential

**Safety Class:** C

**Safety Rationale:** MDR compliance is required for medical device software; non-compliance could prevent market authorization and affect patient access to care.

**Source:**
- Commit: `16e0273739eda65f73af5410323e1caf53362f16`

**Verification:**
- Method: Regulatory audit, documentation review

**Dependencies:** SRS-AUD-001, SRS-AUD-002, SRS-DATA-002

---

## 11. SOUP/OTS Requirements

### 11.1 Third-Party Component Requirements

| SOUP | Version | Functional Requirement | Safety Requirement |
|------|---------|----------------------|-------------------|
| Spring Boot | inherited from alerisx-mhp-parent:2.30.0 | Provides web framework, dependency injection, data access | Must maintain API stability; security patches must be applied |
| Spring Security | inherited from Spring Boot | Provides authentication and authorization | Must prevent authentication bypass; must enforce RBAC correctly |
| Spring Data JPA | inherited from Spring Boot | Provides ORM and repository pattern | Must maintain transaction integrity; must prevent SQL injection |
| Hibernate | inherited from Spring Boot | Provides ORM implementation | Query timeout enforcement; optimistic locking support |
| MariaDB JDBC | specified via mysql.driver property | Database connectivity | Connection pooling; timeout handling |
| Flyway | inherited from Spring Boot | Database schema migration | Migration rollback capability; schema validation |
| HikariCP | inherited from Spring Boot | Database connection pooling | Connection leak detection; timeout configuration |
| Apache Artemis | inherited | JMS messaging | Message delivery guarantees; broker availability |
| QueryDSL | inherited | Type-safe query construction | SQL injection prevention |
| Lombok | 1.18.30 | Code generation (getters, setters, builders) | No runtime impact; compile-time only |
| Blaze Persistence | 1.6.11 | Advanced JPA queries | Query correctness |
| MicroProfile OpenAPI | 3.1.1 | API documentation | Documentation accuracy |
| Apache CXF | 3.6.3 | Web services framework | Security header handling |
| Spring Cloud Sleuth | inherited | Distributed tracing | Trace context propagation |
| Micrometer Prometheus | inherited | Metrics collection | Metrics accuracy |
| Testcontainers | 1.19.8 | Integration testing | Test isolation |

## 12. Traceability Matrix

### 12.1 System to Software Requirements

| System Req | Software Req | Implementation | Verification |
|------------|--------------|----------------|--------------|
| N/A | SRS-APPT-001 | AppointmentController, UpdateAppointmentService | GetAppointmentServiceImplTest |
| N/A | SRS-APPT-002 | AppointmentController, GetAppointmentService | GetAppointmentServiceImplTest |
| N/A | SRS-APPT-003 | AppointmentController, UpdateAppointmentService | Testing incomplete |
| N/A | SRS-APPT-004 | Appointment entity | Database schema validation |
| N/A | SRS-HAND-001 | AppointmentHandoverServiceImpl | Testing incomplete |
| N/A | SRS-HAND-002 | AppointmentHandoverServiceImpl | Testing incomplete |
| N/A | SRS-HAND-003 | AppointmentHandoverServiceImpl | Testing incomplete |
| N/A | SRS-HAND-004 | AppointmentHandoverServiceImpl | Testing incomplete |
| N/A | SRS-HAND-005 | AppointmentHandoverServiceImpl | Testing incomplete |
| N/A | SRS-CONS-001 | ConsultationController | Testing incomplete |
| N/A | SRS-CONS-002 | ConsultationController | Testing incomplete |
| N/A | SRS-SEC-001 | Spring Security configuration | Security testing |
| N/A | SRS-SEC-002 | AppointmentAuthorizer | Authorization tests |
| N/A | SRS-SEC-003 | JWT configuration | Authentication tests |
| N/A | SRS-SEC-004 | @Audit annotation | Audit log verification |

### 12.2 Requirements to Risk Controls

| Hazard | Risk Control | Software Req | Verification |
|--------|--------------|--------------|--------------|
| HAZ-001 | RC-001 | SRS-HAND-002, SRS-INT-002, SRS-INT-004 | Integration testing |
| HAZ-002 | RC-002 | SRS-AUD-002 | Log verification testing |
| HAZ-003 | RC-003, RC-004 | SRS-DATA-002, SRS-DATA-003 | Transaction testing |
| HAZ-004 | RC-005, RC-006 | SRS-SAF-002, SRS-INT-001 | Unit testing |
| HAZ-005 | RC-007, RC-008, RC-009, RC-010 | SRS-SAF-001, SRS-SEC-002 | Integration testing |
| HAZ-006 | RC-011, RC-012 | SRS-DATA-003, SRS-HAND-004 | Concurrency testing |
| HAZ-007 | RC-013, RC-014 | SRS-INT-006 | Integration testing |
| HAZ-008 | RC-015, RC-016 | SRS-INT-003, SRS-UI-003 | Integration testing |
| HAZ-009 | RC-017, RC-018 | SRS-SAF-003, SRS-HAND-004 | State machine testing |
| HAZ-010 | RC-019, RC-020 | SRS-AUD-001, SRS-AUD-002 | Audit testing |
| HAZ-011 | RC-021, RC-022 | SRS-HAND-005 | Authorization testing |
| HAZ-012 | RC-023 | SRS-DATA-003 | Input validation testing |
| HAZ-013 | RC-024, RC-025, RC-034 | SRS-SEC-002, SRS-SEC-004 | Security testing |
| HAZ-014 | RC-026, RC-027, RC-028 | Integration components | Integration testing |
| HAZ-015 | RC-029, RC-030 | SRS-OPER-003 | Integration testing |
| HAZ-016 | RC-031, RC-032 | Payment components | Integration testing |
| HAZ-017 | RC-033 | SRS-UI-004 | Security testing |

### 12.3 Safety Requirements to Hazards

| Safety Requirement | Hazard | Hazardous Situation | Risk Control |
|-------------------|--------|---------------------|--------------|
| SRS-SAF-001 | HAZ-005 | HS-005 | RC-007, RC-008, RC-009, RC-010 |
| SRS-SAF-002 | HAZ-004 | HS-004 | RC-005, RC-006 |
| SRS-SAF-003 | HAZ-009 | HS-009 | RC-017, RC-018 |
| SRS-SAF-004 | HAZ-009 | HS-009 | Ownership validation |
| SRS-SAF-005 | HAZ-004 | HS-004 | Input validation |

## 13. Appendices

### Appendix A: Requirement Change Log

This document was generated from source code extraction on 2026-03-06 at commit `16e0273739eda65f73af5410323e1caf53362f16`. Requirements are traced to source code files and will be updated via Git version control. Change history will be tracked through Git commits and the document's `version` field (auto-populated by GitHub Actions).

### Appendix B: Extraction Metadata

- **Repository:** /home/jakob/Noncomplicity/Repos/health-manager
- **Commit:** 16e0273739eda65f73af5410323e1caf53362f16
- **Extracted:** 2026-03-06T09:10:18Z
- **Extractor Version:** 1.0
- **Standard:** IEC 62304:2006+AMD1:2015
- **Standard Clause:** 5.2 Software requirements analysis
- **Software Name:** health-manager
- **Software Version:** 1.0.0-SNAPSHOT
- **Technology Stack:**
  - Runtime: Java 21
  - Framework: Spring Boot
  - Persistence: Spring Data JPA, Hibernate
  - Database: MySQL/MariaDB
  - Messaging: JMS (Apache Artemis)
  - Build: Maven
  - Query: QueryDSL
  - Migration: Flyway

### Appendix C: Requirements Gaps and Recommendations

The following gaps were identified during requirements extraction and should be addressed:

1. **Testing Coverage Gaps:**
   - No integration tests found for handover functionality (AppointmentHandoverServiceImpl)
   - No tests for AppointmentAuthorizer security component
   - No tests for JMS event publishing (AppointmentEventSender)
   - No security penetration testing or vulnerability scanning results

2. **Documentation Gaps:**
   - Software safety classification not explicitly documented in code or configuration
   - No formal risk analysis document (only handover-specific risk_analysis_handover.md)
   - No software architecture document beyond handover_architecture.md
   - SOUP hazard analysis not documented
   - Verification and validation plan incomplete
   - Backup and recovery procedures not documented
   - Disaster recovery and business continuity plans not documented
   - User training and competency requirements not documented
   - Installation and deployment procedures not formally documented

3. **Requirements Clarifications Needed:**
   - Performance requirements (response times, throughput) not explicitly specified
   - Data retention and archival policies not documented

## 14. Compliance Mapping

| Document Section | IEC 62304 Clause | Evidence Provided |
|------------------|------------------|-------------------|
| Section 1.4 | 4.3 | Software safety classification with rationale |
| Sections 2-10 (all requirements) | 5.2.1 | Complete software requirements documentation |
| Section 2 (Functional Requirements) | 5.2.2 a) | Functional and capability requirements |
| Section 3 (Interface Requirements) | 5.2.2 b), c) | Inputs, outputs, interfaces to external systems |
| Section 5 (Safety Requirements) | 5.2.2 d) | Alarms, warnings, operator error control |
| Section 5 (Safety Requirements) | 5.2.3 | Risk control measure requirements (linked to risks.json) |
| Section 4 (Performance Requirements) | 5.2.4 | Data definition and database requirements |
| Section 6 (Security Requirements) | 5.2.5 | Security requirements including authentication, authorization, audit |
| Section 12 (Traceability Matrix) | 5.2.6 | Requirements traceability to risk controls and verification |

---

**Document Generation Information:**

This Software Requirements Specification was generated according to IEC 62304:2006+AMD1:2015 clause 5.2 requirements. The document is based on automated extraction from source code and manual risk analysis. All requirements are traced to source code implementation and, where applicable, to risk controls and verification methods.

**Revision History:**

The `version`, `author`, and `effective_date` fields in the frontmatter will be auto-populated by GitHub Actions upon commit to the main branch. This document is under change control per the QMS Change Control Procedure.
