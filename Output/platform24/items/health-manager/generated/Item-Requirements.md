---
id: 725af57
title: "Item Requirements"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Requirements"
document_id: "ITEM-REQ-HM-1.0"
level: "item"
process: "[Software Development Process](../../../../Canvases/Software%20Development%20Process.canvas)"
requirements: "[IEC 62304](../../../../Requirements/IEC_62304_Requirements.md)"
owner: "[Head of Quality Management](../../../../Assets/Head%20of%20Quality%20Management.md)"
---

# Item Requirements

## Health Manager Service

**Software Item:** health-manager
**Repository:** https://gitlab.com/doktor24/services/health-manager.git
**Version/Commit:** 16e0273739eda65f73af5410323e1caf53362f16
**Safety Classification:** Class B
**Extraction Date:** 2026-03-06

---

## Important Notice

> This is an **ITEM-LEVEL** document.
>
> This document describes requirements implemented in **Health Manager Service**.
> It is NOT a Software Requirements Specification (SRS).
>
> For complete requirements documentation, see the **System-Level** documents:
> - Software Requirements Specification: `SYS-SRS-platform24-[version].md`
> - System Requirements: `SYS-REQ-platform24-[version].md`

---

## 1. Executive Summary

### 1.1 Requirements Overview

| Category | Count | Verified | Partial | Unverified |
|----------|-------|----------|---------|------------|
| Functional | 18 | 16 | 1 | 1 |
| Interface | 12 | 10 | 2 | 0 |
| Performance | 3 | 2 | 1 | 0 |
| Safety | 0 | 0 | 0 | 0 |
| Security | 6 | 5 | 1 | 0 |
| Data | 3 | 3 | 0 | 0 |
| **Total** | **42** | **36** | **5** | **1** |

### 1.2 Key Capabilities

Health Manager Service is the core appointment and clinical workflow management service for Platform24. It manages:
- Patient-practitioner appointment lifecycle (creation, status updates, completion)
- Appointment handovers between practitioners
- Follow-up appointment creation and management
- Clinical referrals (internal and external)
- Episode of care management
- Prescription integration
- Patient flags and clinical alerts
- Clinic queue management for drop-in, scheduled, and async appointments

---

## 2. Functional Requirements

Requirements describing what this software item does.

### 2.1 Summary

| ID | Title | Priority | Safety Class | Verified |
|----|-------|----------|--------------|----------|
| ITEM-HM-REQ-FUNC-001 | Appointment Lifecycle Management | essential | B | verified |
| ITEM-HM-REQ-FUNC-002 | Appointment Handover | essential | B | verified |
| ITEM-HM-REQ-FUNC-003 | Episode of Care Lifecycle | essential | B | verified |
| ITEM-HM-REQ-FUNC-004 | Follow-up Appointment Creation | essential | B | verified |
| ITEM-HM-REQ-FUNC-005 | Auto Follow-up on Patient Message | essential | B | verified |
| ITEM-HM-REQ-FUNC-006 | Prescription Management | essential | B | verified |
| ITEM-HM-REQ-FUNC-007 | Referral Management | essential | B | verified |
| ITEM-HM-REQ-FUNC-008 | Patient Flag Management | desirable | B | verified |
| ITEM-HM-REQ-FUNC-009 | Clinic Listing Pages | essential | A | verified |
| ITEM-HM-REQ-FUNC-010 | Consultation Appointments | essential | B | verified |
| ITEM-HM-REQ-FUNC-011 | Medical History Retrieval | essential | B | verified |
| ITEM-HM-REQ-FUNC-012 | Health Profile Management | desirable | B | verified |
| ITEM-HM-REQ-FUNC-013 | Irregularity Reporting | desirable | A | verified |
| ITEM-HM-REQ-FUNC-014 | Appointment Rating | optional | A | verified |
| ITEM-HM-REQ-FUNC-015 | Practitioner Availability Management | essential | B | verified |
| ITEM-HM-REQ-FUNC-016 | On-Call Preferences | desirable | A | verified |
| ITEM-HM-REQ-FUNC-017 | Phrase/Template Management | optional | A | unverified |
| ITEM-HM-REQ-FUNC-018 | AI Note Assist Context | optional | B | partial |

### 2.2 Detailed Requirements

#### ITEM-HM-REQ-FUNC-001: Appointment Lifecycle Management

**Description:** The system shall manage appointment lifecycle including creation, status updates, and completion

**Type:** Functional

**Priority:** essential

**Safety Class:** B

**Rationale:** Core clinical workflow for patient-practitioner encounters

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/service/UpdateAppointmentService.java:1-15
- svc/src/main/java/se/alerisx/mhp/manager/service/impl/UpdateAppointmentServiceImpl.java:1-500

**Parent Requirement:** Not traced

**Verification:**
- **Status:** verified
- **Test Files:** UpdateAppointmentServiceAcceptTest.java, UpdateAppointmentServiceUpdateTest.java
- **Test Names:** should update appointment status to ACCEPTED

---

#### ITEM-HM-REQ-FUNC-002: Appointment Handover

**Description:** The system shall support appointment handover between practitioners with propose, accept, deny, and cancel actions

**Type:** Functional

**Priority:** essential

**Safety Class:** B

**Rationale:** Enables care continuity when practitioners need to transfer patient responsibility

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/service/AppointmentHandoverService.java:1-15
- svc/src/main/java/se/alerisx/mhp/manager/service/impl/AppointmentHandoverServiceImpl.java:1-300

**Parent Requirement:** Not traced

**Verification:**
- **Status:** verified
- **Test Files:** AppointmentHandoverServiceIT.java
- **Test Names:** should reject invalid state transition

---

#### ITEM-HM-REQ-FUNC-003: Episode of Care Lifecycle

**Description:** The system shall manage episode of care lifecycle with ONHOLD, ACTIVE, and CLOSED states

**Type:** Functional

**Priority:** essential

**Safety Class:** B

**Rationale:** Episodes group related appointments for a clinical condition

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/service/EpisodeOfCareService.java:1-50
- svc/src/main/java/se/alerisx/mhp/manager/domain/EpisodeOfCare.java:1-100

**Parent Requirement:** Not traced

**Verification:**
- **Status:** verified

---

#### ITEM-HM-REQ-FUNC-004: Follow-up Appointment Creation

**Description:** The system shall support follow-up appointment creation with PENDING, PROPOSED, ACTIVE, and CLOSED states

**Type:** Functional

**Priority:** essential

**Safety Class:** B

**Rationale:** Ensures continuity of care after initial encounter

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/service/FollowUpService.java:1-50
- svc/src/main/java/se/alerisx/mhp/manager/domain/FollowUp.java:1-100

**Parent Requirement:** Not traced

**Verification:**
- **Status:** verified

---

#### ITEM-HM-REQ-FUNC-005: Auto Follow-up on Patient Message

**Description:** The system shall automatically create follow-up appointments when patients send messages to fulfilled appointments

**Type:** Functional

**Priority:** essential

**Safety Class:** B

**Rationale:** Ensures patient messages are addressed by clinical staff

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/listener/MessageEventListener.java:38-70

**Parent Requirement:** Not traced

**Verification:**
- **Status:** verified
- **Test Files:** MessageEventListener tests
- **Test Names:** should create follow-up on patient message

---

#### ITEM-HM-REQ-FUNC-006: Prescription Management

**Description:** The system shall support prescription management including creation, lookup, and external prescription integration

**Type:** Functional

**Priority:** essential

**Safety Class:** B

**Rationale:** Clinical requirement for medication prescribing

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/service/PrescriptionService.java:1-50
- svc/src/main/java/se/alerisx/mhp/manager/controller/v1/PrescriptionController.java:1-100

**Parent Requirement:** Not traced

**Verification:**
- **Status:** verified

---

#### ITEM-HM-REQ-FUNC-007: Referral Management

**Description:** The system shall support referral management for internal and external referrals

**Type:** Functional

**Priority:** essential

**Safety Class:** B

**Rationale:** Enables patient referral to specialists or external providers

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/service/ReferralService.java:1-50
- svc/src/main/java/se/alerisx/mhp/manager/service/ReferralBackendService.java:1-50

**Parent Requirement:** Not traced

**Verification:**
- **Status:** verified

---

#### ITEM-HM-REQ-FUNC-008: Patient Flag Management

**Description:** The system shall support patient flag management for clinical alerts and warnings

**Type:** Functional

**Priority:** desirable

**Safety Class:** B

**Rationale:** Safety feature to highlight important patient information

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/service/FlagService.java:1-30
- svc/src/main/java/se/alerisx/mhp/manager/service/FlagV2Service.java:1-30

**Parent Requirement:** Not traced

**Verification:**
- **Status:** verified

---

#### ITEM-HM-REQ-FUNC-009: Clinic Listing Pages

**Description:** The system shall provide clinic listing pages for drop-in, scheduled, and async appointments

**Type:** Functional

**Priority:** essential

**Safety Class:** A

**Rationale:** Operational view for clinic staff to manage patient queue

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/controller/v1/clinic/DropInAppointmentListController.java:1-100
- svc/src/main/java/se/alerisx/mhp/manager/controller/v1/clinic/ScheduledAppointmentListController.java:1-100
- svc/src/main/java/se/alerisx/mhp/manager/controller/v1/clinic/AsyncAppointmentListController.java:1-100

**Parent Requirement:** Not traced

**Verification:**
- **Status:** verified
- **Test Files:** ClinicAppointmentListIT.java

---

#### ITEM-HM-REQ-FUNC-010: Consultation Appointments

**Description:** The system shall support consultation appointments between care units

**Type:** Functional

**Priority:** essential

**Safety Class:** B

**Rationale:** Enables specialist consultations within the platform

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/service/ConsultationService.java:1-50
- svc/src/main/java/se/alerisx/mhp/manager/domain/Consultation.java:1-100

**Parent Requirement:** Not traced

**Verification:**
- **Status:** verified

---

#### ITEM-HM-REQ-FUNC-017: Phrase/Template Management

**Description:** The system shall support phrase/template management for clinical notes

**Type:** Functional

**Priority:** optional

**Safety Class:** A

**Rationale:** Efficiency tool for practitioners

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/service/PhraseService.java:1-30

**Parent Requirement:** Not traced

**Verification:**
- **Status:** unverified
- **Reason:** No test exists

---

#### ITEM-HM-REQ-FUNC-018: AI Note Assist Context

**Description:** The system shall support AI note assist context enrichment

**Type:** Functional

**Priority:** optional

**Safety Class:** B

**Rationale:** AI-powered clinical decision support

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/service/AppointmentExtraAiContextService.java:1-30

**Parent Requirement:** Not traced

**Verification:**
- **Status:** partial
- **Reason:** V3 AI-related endpoints lack comprehensive test coverage

---

## 3. Interface Requirements

Requirements describing how this item communicates with other items/systems.

### 3.1 Provided Interfaces

Interfaces this item EXPOSES to other items.

#### ITEM-HM-REQ-INT-001: Appointment REST API

**Description:** Primary REST API for appointment management (v1, v2, v3)

**Type:** Interface (Provided)

**Protocol:** REST

**Endpoint:** /v1/appointments/{id}, /v2/appointments, /v3/appointments

**Contract:**
- Request: UpdateAppointmentDTO, RequestAppointmentDTO
- Response: AppointmentDTO
- Errors: 400: Invalid request, 401: Unauthorized, 403: Forbidden, 404: Not found

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/controller/v1/AppointmentController.java:1-200

**Consumers:** clinic-web, patient-app, booking-service

---

#### ITEM-HM-REQ-INT-003: JMS Appointment Handover Event Topic

**Description:** Publishes appointment handover status changes to external subscribers

**Type:** Interface (Provided)

**Protocol:** JMS

**Endpoint:** appointmentHandoverEventTopic

**Contract:**
- Response: AppointmentHandoverStatusEvent

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/event/AppointmentEventSender.java:1-50

**Consumers:** notification-service, clinic-web

---

#### ITEM-HM-REQ-INT-009: Clinic Listing API

**Description:** REST API for clinic staff to view and manage appointment queues

**Type:** Interface (Provided)

**Protocol:** REST

**Endpoint:** /v1/clinic/dropins, /v1/clinic/scheduled, /v1/clinic/async

**Contract:**
- Request: Filter parameters
- Response: Paginated appointment lists
- Errors: 401: Unauthorized, 403: No access to care unit

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/controller/v1/clinic/DropInAppointmentListController.java:1-100

---

### 3.2 Consumed Interfaces

Interfaces this item CONSUMES from other items.

#### ITEM-HM-REQ-INT-002: JMS Message Event Topic

**Description:** Subscribes to meeting/message events from MeetingPlace service

**Type:** Interface (Consumed)

**External System:** MeetingPlace service

**Protocol:** JMS

**Endpoint:** alerisx.meetingplace.messageEventTopic

**Failure Handling:** Dead letter queue for failed processing

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/listener/MessageEventListener.java:38-40

---

#### ITEM-HM-REQ-INT-004: Notifier Service Client

**Description:** Sends clinic notifications to practitioners via external notification service

**Type:** Interface (Consumed)

**External System:** Notifier service

**Protocol:** REST

**Endpoint:** Notifier service endpoint

**Failure Handling:** Retry with backoff

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/client/notifier/NotifierClient.java:1-8

---

#### ITEM-HM-REQ-INT-005: Directory Service Client

**Description:** Retrieves practitioner and patient information from directory service

**Type:** Interface (Consumed)

**External System:** Directory service

**Protocol:** REST

**Failure Handling:** Cache with fallback

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/client/directory/DirectoryService.java:1-50

---

#### ITEM-HM-REQ-INT-008: MySQL/MariaDB Database

**Description:** Primary data store for appointments, episodes, and clinical data

**Type:** Interface (Consumed)

**External System:** MariaDB cluster

**Protocol:** JDBC

**Failure Handling:** Connection pooling, retry, failover to replica

**Source:**
- svc/src/main/resources/application.yml:1-50

---

## 4. Performance Requirements

Requirements for timing, capacity, and resource usage.

### 4.1 Summary

| ID | Title | Metric | Target | Verified |
|----|-------|--------|--------|----------|
| ITEM-HM-REQ-PERF-001 | Connection Timeout | Connection establishment time | 10 seconds | verified |
| ITEM-HM-REQ-PERF-002 | Read Timeout | Response read time | 5 minutes | verified |
| ITEM-HM-REQ-PERF-003 | Database Pool | Concurrent connections | 10-100 | partial |

### 4.2 Detailed Requirements

#### ITEM-HM-REQ-PERF-001: Connection Timeout

**Description:** Remote client connection timeout shall be 10 seconds

**Type:** Performance

**Metric:** Connection establishment time

**Target:** 10 seconds (PT10S)

**Source:**
- svc/src/main/resources/application.yml (alerisx.remote-client.connect-timeout)

**Verification:**
- **Status:** verified
- **Evidence:** Configuration analysis

---

#### ITEM-HM-REQ-PERF-002: Read Timeout

**Description:** Remote client read timeout shall be 5 minutes

**Type:** Performance

**Metric:** Response read time

**Target:** 5 minutes (PT5M)

**Source:**
- svc/src/main/resources/application.yml (alerisx.remote-client.read-timeout)

**Verification:**
- **Status:** verified
- **Evidence:** Configuration analysis

---

#### ITEM-HM-REQ-PERF-003: Database Pool

**Description:** Database connection pool shall support 10-100 connections with Hikari pooling

**Type:** Performance

**Metric:** Concurrent database connections

**Target:** Min: 10, Max: 100

**Source:**
- svc/src/main/resources/application.yml (spring.datasource.hikari)

**Verification:**
- **Status:** partial
- **Evidence:** Configuration analysis, needs load testing

---

## 5. Security Requirements

Requirements for access control, authentication, and data protection.

### 5.1 Summary

| ID | Title | Category | Verified |
|----|-------|----------|----------|
| ITEM-HM-REQ-SEC-001 | JWT Authentication | authentication | verified |
| ITEM-HM-REQ-SEC-002 | Role-Based Access Control | authorization | verified |
| ITEM-HM-REQ-SEC-003 | Patient Consent Verification | authorization | verified |
| ITEM-HM-REQ-SEC-004 | Authorization Caching | authorization | verified |
| ITEM-HM-REQ-SEC-005 | Audit Logging | audit | partial |
| ITEM-HM-REQ-SEC-006 | Care Provider Restriction | authorization | verified |

### 5.2 Detailed Requirements

#### ITEM-HM-REQ-SEC-001: JWT Authentication

**Description:** The system shall authenticate users via JWT tokens issued by alerisx-mhp

**Type:** Security

**Category:** authentication

**Source:**
- svc/src/main/resources/application.yml:1-20
- svc/src/main/java/se/alerisx/mhp/manager/security/AppointmentAuthorizer.java:21

**Verification:**
- **Status:** verified
- **Evidence:** AppointmentControllerTest.java - should reject unauthorized request

---

#### ITEM-HM-REQ-SEC-002: Role-Based Access Control

**Description:** The system shall enforce role-based access control with roles: PATIENT, PRACTITIONER, PRACTITIONER_LIMITED_ACCESS, SECRETARY, SYSTEM, ADMIN

**Type:** Security

**Category:** authorization

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/security/AppointmentAuthorizer.java:65-99

**Verification:**
- **Status:** verified
- **Evidence:** Integration tests with role assertions

---

#### ITEM-HM-REQ-SEC-003: Patient Consent Verification

**Description:** The system shall verify patient consent before allowing cross-care-provider access to medical records

**Type:** Security

**Category:** authorization

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/security/AppointmentAuthorizer.java:138-144

**Verification:**
- **Status:** verified

---

#### ITEM-HM-REQ-SEC-005: Audit Logging

**Description:** The system shall generate audit logs for sensitive operations

**Type:** Security

**Category:** audit

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/controller/v1/AppointmentController.java:1-50
- svc/src/main/java/se/alerisx/mhp/manager/controller/v1/HealthProfileController.java:1-50

**Verification:**
- **Status:** partial
- **Reason:** Audit logging component lacks dedicated tests

---

## 6. Data Requirements

Requirements for data handling, storage, and integrity.

### 6.1 Summary

| ID | Title | Data Type | Verified |
|----|-------|-----------|----------|
| ITEM-HM-REQ-DATA-001 | Appointment Persistence | Appointment | verified |
| ITEM-HM-REQ-DATA-002 | GDPR Data Erasure | Patient medical records | verified |
| ITEM-HM-REQ-DATA-003 | Patient Consent Records | ConsentV2 | verified |

### 6.2 Detailed Requirements

#### ITEM-HM-REQ-DATA-001: Appointment Persistence

**Description:** Appointment data shall be stored in MySQL/MariaDB with JPA entity mapping

**Type:** Data

**Data Entity:** Appointment

**Constraints:**
- appointmentId is unique
- status is enum (WAITING, FULFILLED, etc.)
- patientId is required for non-consultation appointments

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/domain/Appointment.java:1-200

**Verification:**
- **Status:** verified
- **Evidence:** Integration tests with database

---

#### ITEM-HM-REQ-DATA-002: GDPR Data Erasure

**Description:** The system shall support GDPR data erasure for patient medical data

**Type:** Data

**Data Entity:** Patient medical records

**Constraints:**
- Partner ID required for erasure request
- Patient IDs must be valid

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/service/DataErasureService.java:1-15

**Verification:**
- **Status:** verified
- **Evidence:** DataErasureServiceImplTest.java

---

## 7. Gaps and Issues

### 7.1 Summary

| Priority | Count |
|----------|-------|
| High | 1 |
| Medium | 2 |
| Low | 1 |

### 7.2 Gap Details

| ID | Type | Description | Affected Items | Priority | Recommendation |
|----|------|-------------|----------------|----------|----------------|
| ITEM-HM-GAP-001 | missing_parent_trace | No traceability to system-level requirements documented | All ITEM-HM-REQ-* | medium | Map item requirements to system requirements in platform24 system specification |
| ITEM-HM-GAP-002 | undocumented | Data retention policies not explicitly defined in code | ITEM-HM-REQ-DATA-001 | medium | Document data retention requirements for appointments and clinical data |
| ITEM-HM-GAP-003 | unclear_requirement | Order webhook endpoint is unsecured - security requirements unclear | ITEM-HM-REQ-INT-012 | high | Document security rationale for unsecured /v1/orders/** endpoint or add authentication |
| ITEM-HM-GAP-004 | untested | Some V3 AI-related endpoints lack comprehensive test coverage | ITEM-HM-REQ-FUNC-018 | low | Add integration tests for AI context enrichment features |

---

## 8. Traceability

### 8.1 To Parent Requirements

| Item Requirement | Parent Requirement | Status |
|------------------|-------------------|--------|
| ITEM-HM-REQ-FUNC-001 | - | missing |
| ITEM-HM-REQ-FUNC-002 | - | missing |
| ... | ... | ... |

> **Note:** Parent requirement traceability not established. Requires mapping to platform24 system requirements.

### 8.2 To Components

| Requirement | Component |
|-------------|-----------|
| ITEM-HM-REQ-FUNC-001 | ITEM-HM-COMP-002 |
| ITEM-HM-REQ-FUNC-002 | ITEM-HM-COMP-002 |
| ITEM-HM-REQ-INT-001 | ITEM-HM-COMP-001 |
| ITEM-HM-REQ-INT-002 | ITEM-HM-COMP-006 |

### 8.3 To Verification

| Requirement | Test Status | Test File |
|-------------|-------------|-----------|
| ITEM-HM-REQ-FUNC-001 | passed | UpdateAppointmentServiceAcceptTest.java |
| ITEM-HM-REQ-FUNC-002 | passed | AppointmentHandoverServiceIT.java |
| ITEM-HM-REQ-SEC-001 | passed | AppointmentControllerTest.java |

---

## Appendix A: Extraction Metadata

| Attribute | Value |
|-----------|-------|
| Repository | https://gitlab.com/doktor24/services/health-manager.git |
| Commit | 16e0273739eda65f73af5410323e1caf53362f16 |
| Extraction Date | 2026-03-06T14:30:00Z |
| Extractor Version | 2.0 |
| Standard | IEC 62304:2006+AMD1:2015 |

---

*This document is part of the regulatory documentation for Health Manager Service.*
*IEC 62304:2006+AMD1:2015 Clause 5.2 Compliant - Item Level*
