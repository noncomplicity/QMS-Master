---
id: 725af57
title: "Item Risk Contribution"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "RiskContribution"
document_id: "ITEM-RISK-HM-1.0"
level: "item"
process: "[Risk Management Process](../../../../Canvases/Risk%20Management%20Process.canvas)"
requirements: "[IEC 62304](../../../../Requirements/IEC_62304_Requirements.md)"
owner: "[Head of Risk Management](../../../../Assets/Head%20of%20Risk%20Management.md)"
---

# Item Risk Contribution

## Health Manager Service

**Software Item:** health-manager
**Repository:** https://gitlab.com/doktor24/services/health-manager.git
**Version/Commit:** 16e0273739eda65f73af5410323e1caf53362f16
**Safety Classification:** Class B
**Extraction Date:** 2026-03-06

---

## Important Notice

> **This is an ITEM-LEVEL document.**
>
> This document describes how **Health Manager Service** contributes to the system risk profile.
> It is NOT a Risk Management Report (RMR).
>
> For complete risk management documentation, see the **System-Level** documents:
> - Risk Management Plan: `SYS-RMP-platform24-[version].md`
> - Risk Analysis: `SYS-RA-platform24-[version].md`
> - Risk Management Report: `SYS-RMR-platform24-[version].md`

---

## 1. Executive Summary

### 1.1 Risk Contribution Summary

| Metric | Count |
|--------|-------|
| Hazard Contributions | 7 |
| Risk Controls Implemented | 9 |
| Controls Requiring Upstream Implementation | 3 |
| Failure Modes Documented | 4 |
| Interfaces with Safety Impact | 3 |
| Verification Gaps | 4 |

### 1.2 Safety Classification Rationale

**Classification:** Class B

**Rationale:** Software manages clinical appointment workflows and patient-practitioner interactions. Failures could result in delayed care or incorrect care assignment, but direct physiological harm is unlikely as clinical decisions are made by practitioners.

---

## 2. Hazard Contributions

This section documents hazards that **Health Manager Service** can cause or contribute to.

### 2.1 Summary

| ID | Description | Severity Contribution | Controls |
|----|-------------|-----------------------|----------|
| ITEM-HM-HAZ-001 | Unauthorized access to patient data | serious | RC-001, RC-009 |
| ITEM-HM-HAZ-002 | Patient appointment data corruption | serious | RC-002, RC-003, RC-009 |
| ITEM-HM-HAZ-003 | Handover to unavailable practitioner | serious | RC-003, RC-004 |
| ITEM-HM-HAZ-004 | Critical appointment not escalated | critical | RC-005 |
| ITEM-HM-HAZ-005 | Follow-up notification not sent | serious | RC-006 |
| ITEM-HM-HAZ-006 | Appointment at incorrect time | minor | RC-007 |
| ITEM-HM-HAZ-007 | JMS message loss causing inconsistency | serious | RC-008 |

### 2.2 Detailed Hazard Analysis

#### ITEM-HM-HAZ-001: Unauthorized Access to Patient Appointment Data

**Description:** Unauthorized access to patient appointment data

**Cause:** Authentication bypass or authorization failure in REST API

**Potential Harm:** Privacy breach, patient confidentiality violation

**Severity Contribution:** serious

**Affected Functions:**
- AppointmentController.getAppointment
- ClinicAppointmentController.getClinicAppointments

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/controller/v1/AppointmentController.java:45-120
- svc/src/main/java/se/alerisx/mhp/manager/controller/v1/clinic/ClinicAppointmentController.java:30-150

**Related Controls:**
- ITEM-HM-RC-001: JWT-based authentication
- ITEM-HM-RC-009: Clinical audit logging

---

#### ITEM-HM-HAZ-002: Patient Appointment Data Corruption or Loss

**Description:** Patient appointment data corruption or loss

**Cause:** Database transaction failure, concurrent modification, or ORM mapping errors

**Potential Harm:** Lost clinical context, incorrect treatment decisions based on missing data

**Severity Contribution:** serious

**Affected Functions:**
- UpdateAppointmentService.update
- Hibernate entity persistence

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/service/impl/UpdateAppointmentServiceImpl.java:1-500
- svc/src/main/java/se/alerisx/mhp/manager/domain/Appointment.java:1-200

**Related Controls:**
- ITEM-HM-RC-002: Database transaction management
- ITEM-HM-RC-003: Optimistic locking
- ITEM-HM-RC-009: Clinical audit logging

---

#### ITEM-HM-HAZ-003: Handover to Unavailable or Incorrect Practitioner

**Description:** Handover to unavailable or incorrect practitioner

**Cause:** Race condition in handover acceptance, stale practitioner availability data

**Potential Harm:** Delayed patient care, patient assigned to wrong specialist

**Severity Contribution:** serious

**Affected Functions:**
- AppointmentHandoverService.propose
- AppointmentHandoverService.accept

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/service/impl/AppointmentHandoverServiceImpl.java:1-300

**Related Controls:**
- ITEM-HM-RC-003: Optimistic locking
- ITEM-HM-RC-004: Handover state machine validation

---

#### ITEM-HM-HAZ-004: Critical Appointment Not Escalated in Time

**Description:** Critical appointment not escalated in time

**Cause:** Escalation rules engine failure or incorrect rule evaluation

**Potential Harm:** Delayed emergency care, patient deterioration

**Severity Contribution:** critical

**Affected Functions:**
- EscalationService.evaluate
- easy-rules rule engine

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/service/impl/EscalationService.java:1-150
- modules/escalator/src/main/java/

**Related Controls:**
- ITEM-HM-RC-005: Escalation rule validation and logging

---

#### ITEM-HM-HAZ-005: Follow-up Notification Not Sent to Patient

**Description:** Follow-up notification not sent to patient

**Cause:** Scheduled job failure, distributed lock contention, message loss

**Potential Harm:** Patient misses follow-up, condition worsens

**Severity Contribution:** serious

**Affected Functions:**
- FollowUpMessageNotificationScheduler.sendNotifications

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/service/impl/FollowUpMessageNotificationScheduler.java:1-100

**Related Controls:**
- ITEM-HM-RC-006: ShedLock distributed locking

---

#### ITEM-HM-HAZ-006: Appointment Scheduled at Incorrect Time

**Description:** Appointment scheduled at incorrect time due to timezone error

**Cause:** Date/time serialization error, timezone conversion failure

**Potential Harm:** Patient misses appointment, delayed care

**Severity Contribution:** minor

**Affected Functions:**
- JSON serialization
- Date handling in DTOs

**Source:**
- dto/src/main/java/se/alerisx/mhp/manager/dto/AppointmentDTO.java
- svc/src/main/java/se/alerisx/mhp/manager/mapper/

**Related Controls:**
- ITEM-HM-RC-007: JSR-310 date/time handling

---

#### ITEM-HM-HAZ-007: JMS Message Loss Causing Appointment Data Inconsistency

**Description:** JMS message loss causing appointment data inconsistency

**Cause:** Message broker failure, listener exception, transaction rollback after publish

**Potential Harm:** Booking data out of sync across services, double-booking

**Severity Contribution:** serious

**Affected Functions:**
- SynchronizedBookingListener.onMessage
- JMS publishers

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/listener/SynchronizedBookingListener.java:1-100

**Related Controls:**
- ITEM-HM-RC-008: JMS dead letter queue

---

## 3. Risk Controls Implemented

This section documents risk controls implemented **within this software item**.

### 3.1 Summary by Control Type

| Type | Count | Verified | Partial | Unverified |
|------|-------|----------|---------|------------|
| Inherent Safety | 3 | 3 | 0 | 0 |
| Protective Measure | 5 | 2 | 3 | 0 |
| Information for Safety | 1 | 0 | 1 | 0 |

### 3.2 Detailed Controls

#### ITEM-HM-RC-001: JWT-based Authentication and Role-based Authorization

**Type:** inherent_safety

**Description:** JWT-based authentication and role-based authorization

**Mitigates:**
- ITEM-HM-HAZ-001: Unauthorized access to patient data

**Implementation:** Spring Security with JWT validation, role-based access checks via @PreAuthorize

**Source:**
- Spring Security configuration via parent starter

**Verification:**
- **Status:** verified
- **Evidence:** svc/src/test/java/se/alerisx/mhp/manager/controller/v1/AppointmentControllerTest.java - should reject unauthorized request

---

#### ITEM-HM-RC-002: Database Transaction Management

**Type:** protective_measure

**Description:** Database transaction management with rollback on failure

**Mitigates:**
- ITEM-HM-HAZ-002: Patient data corruption

**Implementation:** Spring @Transactional with default rollback on RuntimeException

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/service/impl/UpdateAppointmentServiceImpl.java

**Verification:**
- **Status:** verified
- **Evidence:** svc/src/test/java/se/alerisx/mhp/manager/service/impl/UpdateAppointmentServiceImplTest.java - should rollback on exception

---

#### ITEM-HM-RC-003: Optimistic Locking

**Type:** protective_measure

**Description:** Optimistic locking for concurrent modification detection

**Mitigates:**
- ITEM-HM-HAZ-002: Patient data corruption
- ITEM-HM-HAZ-003: Handover to unavailable practitioner

**Implementation:** JPA @Version annotation on entities, OptimisticLockException handling

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/domain/Appointment.java

**Verification:**
- **Status:** partial
- **Evidence:** No explicit test coverage for concurrent modification scenarios

---

#### ITEM-HM-RC-004: Handover State Machine Validation

**Type:** inherent_safety

**Description:** Handover state machine validation

**Mitigates:**
- ITEM-HM-HAZ-003: Handover to unavailable practitioner

**Implementation:** State transition validation before handover operations

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/service/impl/AppointmentHandoverServiceImpl.java

**Verification:**
- **Status:** verified
- **Evidence:** svc/src/test/java/se/alerisx/mhp/manager/service/impl/AppointmentHandoverServiceIT.java - should reject invalid state transition

---

#### ITEM-HM-RC-005: Escalation Rule Validation and Logging

**Type:** protective_measure

**Description:** Escalation rule validation and logging

**Mitigates:**
- ITEM-HM-HAZ-004: Critical appointment not escalated

**Implementation:** Rule evaluation logging, default escalation on rule engine failure

**Source:**
- modules/escalator/src/main/java/

**Verification:**
- **Status:** verified
- **Evidence:** modules/escalator/src/test/java/ - should escalate on rule engine error

---

#### ITEM-HM-RC-006: ShedLock Distributed Locking

**Type:** protective_measure

**Description:** ShedLock distributed locking for scheduled tasks

**Mitigates:**
- ITEM-HM-HAZ-005: Follow-up notification not sent

**Implementation:** Database-backed distributed lock ensures exactly-once execution

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/service/impl/FollowUpMessageNotificationScheduler.java

**Verification:**
- **Status:** partial
- **Evidence:** No explicit cluster environment tests

---

#### ITEM-HM-RC-007: JSR-310 Date/Time Handling

**Type:** inherent_safety

**Description:** JSR-310 date/time handling with explicit timezone

**Mitigates:**
- ITEM-HM-HAZ-006: Appointment at incorrect time

**Implementation:** Use of LocalDateTime/ZonedDateTime, Jackson JSR-310 module

**Source:**
- dto/src/main/java/se/alerisx/mhp/manager/dto/

**Verification:**
- **Status:** verified
- **Evidence:** dto/src/test/java/ - should serialize dates correctly

---

#### ITEM-HM-RC-008: JMS Dead Letter Queue

**Type:** protective_measure

**Description:** JMS dead letter queue for failed message processing

**Mitigates:**
- ITEM-HM-HAZ-007: JMS message loss

**Implementation:** Artemis DLQ configuration, message retry with backoff

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/listener/

**Verification:**
- **Status:** partial
- **Evidence:** No explicit failure scenario tests

---

#### ITEM-HM-RC-009: Clinical Audit Logging

**Type:** information_for_safety

**Description:** Clinical audit logging for all patient data access

**Mitigates:**
- ITEM-HM-HAZ-001: Unauthorized access (detection)
- ITEM-HM-HAZ-002: Data corruption (investigation)

**Implementation:** AuditLoggingService captures all clinical data operations

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/service/audit/AuditLoggingService.java

**Verification:**
- **Status:** partial
- **Evidence:** No explicit audit completeness tests

---

## 4. Controls Requiring Upstream Implementation

These risk controls **cannot be fully implemented at the item level** and must be addressed at module or system level.

### 4.1 Summary

| ID | Description | Recommended Level | Control Type |
|----|-------------|-------------------|--------------|
| ITEM-HM-RC-UP-001 | Practitioner training on handover workflow | system | information_for_safety |
| ITEM-HM-RC-UP-002 | Monitoring alerts for escalation failures | system | protective_measure |
| ITEM-HM-RC-UP-003 | JMS broker high availability configuration | module | inherent_safety |

### 4.2 Detailed Upstream Controls

#### ITEM-HM-RC-UP-001: Practitioner Training on Handover Workflow

**Description:** Practitioner training on handover workflow

**Rationale (why item cannot implement):** Software validates state transitions but cannot prevent incorrect clinical decisions

**Recommended Level:** system

**Control Type:** information_for_safety

**Mitigates:**
- ITEM-HM-HAZ-003: Handover to unavailable practitioner

**Suggested Implementation:**
- Training materials for practitioners on handover workflow
- Warning prompts in UI when handover may cause delay
- Periodic compliance audits

---

#### ITEM-HM-RC-UP-002: Monitoring Alerts for Escalation Failures

**Description:** Monitoring alerts for escalation failures

**Rationale (why item cannot implement):** Software logs failures but organizational response required

**Recommended Level:** system

**Control Type:** protective_measure

**Mitigates:**
- ITEM-HM-HAZ-004: Critical appointment not escalated

**Suggested Implementation:**
- Real-time monitoring dashboard for escalation events
- Alert escalation to on-call clinical supervisor
- Automated fallback escalation path

---

#### ITEM-HM-RC-UP-003: JMS Broker High Availability Configuration

**Description:** JMS broker high availability configuration

**Rationale (why item cannot implement):** Message durability depends on infrastructure configuration

**Recommended Level:** module

**Control Type:** inherent_safety

**Mitigates:**
- ITEM-HM-HAZ-007: JMS message loss

**Suggested Implementation:**
- Artemis clustering configuration
- Message persistence to shared storage
- Automatic broker failover

---

## 5. Failure Modes

### 5.1 Summary

| ID | Component | Failure Mode | Effect | Detection |
|----|-----------|--------------|--------|-----------|
| ITEM-HM-FM-001 | COMP-002 | Service exception | Transaction rollback | Logging, alerts |
| ITEM-HM-FM-002 | COMP-006 | JMS listener timeout | Message requeue | DLQ monitoring |
| ITEM-HM-FM-003 | COMP-003 | Database connection exhaustion | Request failures | Health checks |
| ITEM-HM-FM-004 | COMP-007 | Rules engine error | Incorrect escalation | Audit trail |

### 5.2 Detailed Failure Mode Analysis

#### ITEM-HM-FM-001: Service Exception

**Component:** ITEM-HM-COMP-002 (Business Services)

**Failure Mode:** Service throws uncaught exception

**Effect:** Transaction rollback, HTTP 500 response

**Detection:** Exception logging, monitoring alerts

**Recovery:** Automatic retry by client, manual intervention for persistent issues

**Related Hazards:**
- ITEM-HM-HAZ-002

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/service/impl/

---

#### ITEM-HM-FM-002: JMS Listener Timeout

**Component:** ITEM-HM-COMP-006 (JMS Listeners)

**Failure Mode:** JMS listener processing timeout

**Effect:** Message requeued, potential duplicate processing

**Detection:** Message age monitoring, DLQ monitoring

**Recovery:** Manual DLQ processing, deduplication in downstream consumers

**Related Hazards:**
- ITEM-HM-HAZ-007

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/listener/

---

#### ITEM-HM-FM-003: Database Connection Exhaustion

**Component:** ITEM-HM-COMP-003 (Repositories)

**Failure Mode:** Database connection exhaustion

**Effect:** Connection timeout, request failures

**Detection:** Connection pool monitoring, health checks

**Recovery:** Connection pool auto-recovery, pod restart if persistent

**Related Hazards:**
- ITEM-HM-HAZ-002

**Source:**
- Database connection pool configuration

---

#### ITEM-HM-FM-004: Rules Engine Error

**Component:** ITEM-HM-COMP-007 (Escalator)

**Failure Mode:** Rules engine evaluation error

**Effect:** Appointment not escalated or incorrectly escalated

**Detection:** Rule evaluation logging, escalation audit trail

**Recovery:** Manual escalation review, rule correction

**Related Hazards:**
- ITEM-HM-HAZ-004

**Source:**
- modules/escalator/src/main/java/

---

## 6. Interfaces with Safety Impact

### 6.1 Summary

| ID | Interface | Direction | Safety Impact | Failure Handling |
|----|-----------|-----------|---------------|------------------|
| ITEM-HM-IF-001 | Bookings Service | consumed | Scheduling accuracy | Retry, cache fallback |
| ITEM-HM-IF-002 | Booking Events (JMS) | consumed | Data consistency | DLQ, reconciliation |
| ITEM-HM-IF-003 | Handover Events (JMS) | provided | Notification delivery | Transactional outbox |

### 6.2 Detailed Interface Analysis

#### ITEM-HM-IF-001: Bookings Service

**Type:** REST

**Direction:** consumed

**Safety Impact:** Booking slot availability affects appointment scheduling accuracy

**Failure Handling:** Retry with exponential backoff, cache fallback for read operations

**Timeout:** 5000ms

**Retry Policy:** 3 retries with exponential backoff

**Source:**
- Service client configuration

---

#### ITEM-HM-IF-002: Synchronized Booking Events

**Type:** JMS

**Direction:** consumed

**Safety Impact:** Event loss causes data inconsistency between services

**Failure Handling:** Dead letter queue, manual reconciliation process

**Timeout:** N/A (async)

**Retry Policy:** Artemis built-in retry

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/listener/SynchronizedBookingListener.java

---

#### ITEM-HM-IF-003: Handover Events

**Type:** JMS

**Direction:** provided

**Safety Impact:** Event loss causes handover notification failure

**Failure Handling:** Transactional outbox pattern, at-least-once delivery

**Timeout:** N/A (async)

**Retry Policy:** Artemis built-in retry

**Source:**
- svc/src/main/java/se/alerisx/mhp/manager/service/impl/AppointmentHandoverServiceImpl.java

---

## 7. Verification Gaps

### 7.1 Summary

| Priority | Count |
|----------|-------|
| High | 1 |
| Medium | 3 |
| Low | 0 |

### 7.2 Gap Details

| ID | Type | Description | Affected Items | Priority | Recommendation |
|----|------|-------------|----------------|----------|----------------|
| ITEM-HM-GAP-001 | unverified_control | Optimistic locking lacks explicit test coverage | ITEM-HM-RC-003 | medium | Add concurrent modification test scenarios |
| ITEM-HM-GAP-002 | unverified_control | ShedLock lacks cluster environment tests | ITEM-HM-RC-006 | medium | Add multi-instance scheduling tests |
| ITEM-HM-GAP-003 | unverified_control | JMS DLQ lacks failure scenario tests | ITEM-HM-RC-008 | high | Add JMS failure simulation tests |
| ITEM-HM-GAP-004 | unverified_control | Audit logging lacks completeness verification | ITEM-HM-RC-009 | medium | Add audit coverage tests for all clinical operations |

---

## 8. Aggregation Notes

Information to assist module/system-level aggregation:

### 8.1 Cross-Item Interfaces

These interfaces likely connect to other software items:

- Bookings Service API
- Directory2 Service API
- MeetingPlace Service API
- TakeCare EHR integration
- Notification service

### 8.2 Shared Hazards

These hazards likely exist in other items too:

- Unauthorized access to patient data (common across all clinical services)
- Data inconsistency from JMS message loss (common for services using Artemis)
- Timezone handling errors (common for scheduling services)

### 8.3 System-Level Controls Needed

These controls must be coordinated at system level:

- Unified authentication/authorization across all services
- Central audit log aggregation and monitoring
- JMS broker high availability configuration
- Practitioner training program for clinical workflows
- Escalation monitoring dashboard

---

## 9. Traceability

### 9.1 Hazard to Source Code

| Hazard ID | Source Files |
|-----------|--------------|
| ITEM-HM-HAZ-001 | svc/src/main/java/se/alerisx/mhp/manager/controller/ |
| ITEM-HM-HAZ-002 | svc/src/main/java/se/alerisx/mhp/manager/service/impl/, svc/src/main/java/se/alerisx/mhp/manager/domain/ |
| ITEM-HM-HAZ-003 | svc/src/main/java/se/alerisx/mhp/manager/service/impl/AppointmentHandoverServiceImpl.java |
| ITEM-HM-HAZ-004 | modules/escalator/src/main/java/ |
| ITEM-HM-HAZ-005 | svc/src/main/java/se/alerisx/mhp/manager/service/impl/FollowUpMessageNotificationScheduler.java |
| ITEM-HM-HAZ-006 | dto/src/main/java/se/alerisx/mhp/manager/dto/ |
| ITEM-HM-HAZ-007 | svc/src/main/java/se/alerisx/mhp/manager/listener/ |

### 9.2 Control to Source Code

| Control ID | Source Files |
|------------|--------------|
| ITEM-HM-RC-001 | Spring Security configuration |
| ITEM-HM-RC-002 | svc/src/main/java/se/alerisx/mhp/manager/service/impl/UpdateAppointmentServiceImpl.java |
| ITEM-HM-RC-003 | svc/src/main/java/se/alerisx/mhp/manager/domain/Appointment.java |
| ITEM-HM-RC-004 | svc/src/main/java/se/alerisx/mhp/manager/service/impl/AppointmentHandoverServiceImpl.java |
| ITEM-HM-RC-005 | modules/escalator/src/main/java/ |
| ITEM-HM-RC-006 | svc/src/main/java/se/alerisx/mhp/manager/service/impl/FollowUpMessageNotificationScheduler.java |
| ITEM-HM-RC-007 | dto/src/main/java/se/alerisx/mhp/manager/dto/ |
| ITEM-HM-RC-008 | svc/src/main/java/se/alerisx/mhp/manager/listener/ |
| ITEM-HM-RC-009 | svc/src/main/java/se/alerisx/mhp/manager/service/audit/AuditLoggingService.java |

### 9.3 Control to Verification

| Control ID | Verification Status | Test Evidence |
|------------|---------------------|---------------|
| ITEM-HM-RC-001 | verified | AppointmentControllerTest.java |
| ITEM-HM-RC-002 | verified | UpdateAppointmentServiceImplTest.java |
| ITEM-HM-RC-003 | partial | No explicit tests |
| ITEM-HM-RC-004 | verified | AppointmentHandoverServiceIT.java |
| ITEM-HM-RC-005 | verified | EscalationRulesTest.java |
| ITEM-HM-RC-006 | partial | No cluster tests |
| ITEM-HM-RC-007 | verified | DTO serialization tests |
| ITEM-HM-RC-008 | partial | No failure tests |
| ITEM-HM-RC-009 | partial | No completeness tests |

---

## Appendix A: Extraction Metadata

| Attribute | Value |
|-----------|-------|
| Repository | https://gitlab.com/doktor24/services/health-manager.git |
| Commit | 16e0273739eda65f73af5410323e1caf53362f16 |
| Extraction Date | 2026-03-06T15:45:00Z |
| Extractor Version | 2.0 |
| Standard | IEC 62304:2006+AMD1:2015 |

## Appendix B: Document History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | 2026-03-06 | Automated Extraction | Initial extraction |

---

*This document is part of the regulatory documentation for Health Manager Service.*
*IEC 62304:2006+AMD1:2015 Clause 7 Compliant - Item Level*
