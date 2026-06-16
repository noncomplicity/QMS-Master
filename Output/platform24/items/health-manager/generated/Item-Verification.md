---
id: 725af57
title: "Item Verification"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Verification"
document_id: "ITEM-VER-HM-1.0"
level: "item"
process: "[Software Development Process](../../../../Canvases/Software%20Development%20Process.canvas)"
requirements: "[IEC 62304](../../../../Requirements/IEC_62304_Requirements.md)"
owner: "[Head of Quality Management](../../../../Assets/Head%20of%20Quality%20Management.md)"
---

# Item Verification

## Health Manager Service

**Software Item:** health-manager
**Repository:** https://gitlab.com/doktor24/services/health-manager.git
**Version/Commit:** 16e0273739eda65f73af5410323e1caf53362f16
**Safety Classification:** Class B
**Test Framework:** JUnit 5
**CI System:** GitLab CI
**Extraction Date:** 2026-03-06

---

## Important Notice

> This is an **ITEM-LEVEL** document.
>
> This document describes verification evidence for **Health Manager Service** only.
> It is NOT a Verification Report or Validation Report.
>
> For complete verification documentation, see the **System-Level** documents:
> - System Verification Report: `SYS-VER-platform24-[version].md`
> - Validation Report: `SYS-VAL-platform24-[version].md`

---

## 1. Executive Summary

### 1.1 Test Results Summary

| Metric | Value |
|--------|-------|
| Total Tests | 305 |
| Passed | 298 (97.7%) |
| Failed | 0 (0%) |
| Skipped | 7 (2.3%) |
| Execution Time | ~15 minutes |

### 1.2 Coverage Summary

| Metric | Coverage | Target | Status |
|--------|----------|--------|--------|
| Lines | 72.5% | 70% | Pass |
| Branches | 58.3% | 60% | Fail |
| Functions | 78.2% | 75% | Pass |

### 1.3 Requirements Coverage

| Category | Total | Covered | Partial | Missing |
|----------|-------|---------|---------|---------|
| All Requirements | 42 | 35 | 5 | 2 |
| Safety Requirements | 0 | 0 | 0 | 0 |

### 1.4 Anomaly Summary

| Severity | Open | Resolved | Deferred |
|----------|------|----------|----------|
| Critical | 0 | 0 | 0 |
| Major | 0 | 1 | 0 |
| Minor | 0 | 1 | 0 |

---

## 2. Unit Verification (IEC 62304 5.5)

### 2.1 Component Coverage

| Component | Tests | Passed | Coverage | Acceptance Criteria |
|-----------|-------|--------|----------|---------------------|
| ITEM-HM-COMP-001 | 45 | 45 | 68.0% | 5/7 |
| ITEM-HM-COMP-002 | 120 | 118 | 78.5% | 6/7 |
| ITEM-HM-COMP-003 | 25 | 25 | 65.0% | 5/7 |
| ITEM-HM-COMP-005 | 15 | 15 | 45.0% | 4/7 |
| ITEM-HM-COMP-007 | 35 | 35 | 85.0% | 6/7 |
| ITEM-HM-COMP-008 | 40 | 40 | 70.0% | 5/7 |
| ITEM-HM-COMP-009 | 18 | 18 | 62.0% | 4/7 |
| ITEM-HM-COMP-010 | 0 | 0 | 0% | 0/7 |

### 2.2 Acceptance Criteria Coverage (Class B/C)

Per IEC 62304 5.5.3/5.5.4, unit tests must cover:

| Criterion | Required | Covered | Evidence |
|-----------|----------|---------|----------|
| Functional Correctness | Yes | Yes | 180 tests |
| Event Sequence | Yes | Partial | 45 tests |
| Data/Control Flow | Yes | Yes | 65 tests |
| Fault Handling | Yes | Yes | 52 tests |
| Boundary Conditions | Yes | Yes | 38 tests |
| Memory Management | Class C | N/A | Java GC managed |
| Initialization | Yes | Partial | 25 tests |

### 2.3 Unit Test Details by Component

#### ITEM-HM-COMP-001: REST API Controllers

**Path:** `svc/src/main/java/se/alerisx/mhp/manager/controller/`

**Test Files:**
- svc/src/test/java/se/alerisx/mhp/manager/controller/v1/AppointmentControllerTest.java
- svc/src/test/java/se/alerisx/mhp/manager/controller/v1/PractitionerControllerTest.java
- svc/src/test/java/se/alerisx/mhp/manager/controller/v1/clinic/ClinicAppointmentControllerTest.java
- svc/src/test/java/se/alerisx/mhp/manager/controller/v1/clinic/ConsultationControllerTest.java

**Coverage:**
- Lines: 68.0%
- Branches: 52.0%
- Functions: 75.0%

**Acceptance Criteria Status:**
| Criterion | Status | Tests |
|-----------|--------|-------|
| Functional | Covered | ITEM-HM-UT-001, ITEM-HM-UT-002 |
| Boundary | Covered | Various |
| Fault Handling | Covered | should reject unauthorized request |
| Event Sequence | Missing | - |
| Memory Management | N/A | Java |

**Test Results:**

| ID | Test Name | Type | Status | Duration |
|----|-----------|------|--------|----------|
| ITEM-HM-UT-001 | should return appointment by id | functional | passed | 125ms |
| ITEM-HM-UT-002 | should reject unauthorized request | fault_handling | passed | 85ms |

---

#### ITEM-HM-COMP-002: Business Services

**Path:** `svc/src/main/java/se/alerisx/mhp/manager/service/`

**Test Files:**
- svc/src/test/java/se/alerisx/mhp/manager/service/impl/UpdateAppointmentServiceAcceptTest.java
- svc/src/test/java/se/alerisx/mhp/manager/service/impl/UpdateAppointmentServiceUpdateTest.java
- svc/src/test/java/se/alerisx/mhp/manager/service/impl/GetAppointmentServiceImplTest.java
- svc/src/test/java/se/alerisx/mhp/manager/service/impl/ConsultationServiceImplTest.java
- svc/src/test/java/se/alerisx/mhp/manager/service/impl/DataErasureServiceImplTest.java

**Coverage:**
- Lines: 78.5%
- Branches: 65.2%
- Functions: 82.0%

**Acceptance Criteria Status:**
| Criterion | Status | Tests |
|-----------|--------|-------|
| Functional | Covered | ITEM-HM-UT-010, ITEM-HM-UT-011, ITEM-HM-UT-012 |
| Event Sequence | Covered | State transition tests |
| Data Flow | Covered | Various |
| Fault Handling | Covered | should handle null input gracefully |
| Boundary | Covered | should reject invalid state transition |
| Memory Management | N/A | Java |
| Initialization | Covered | Various |

**Test Results:**

| ID | Test Name | Type | Status | Duration |
|----|-----------|------|--------|----------|
| ITEM-HM-UT-010 | should update appointment status to ACCEPTED | functional | passed | 245ms |
| ITEM-HM-UT-011 | should reject invalid state transition | boundary | passed | 156ms |
| ITEM-HM-UT-012 | should handle null input gracefully | fault_handling | passed | 45ms |

---

#### ITEM-HM-COMP-003: Data Repositories

**Path:** `svc/src/main/java/se/alerisx/mhp/manager/repository/`

**Test Files:**
- svc/src/test/java/se/alerisx/mhp/manager/repository/QueryableAppointmentRepositoryTest.java

**Coverage:**
- Lines: 65.0%
- Branches: 48.0%
- Functions: 70.0%

**Acceptance Criteria Status:**
| Criterion | Status | Tests |
|-----------|--------|-------|
| Functional | Covered | ITEM-HM-UT-020 |
| Data Flow | Covered | Query tests |
| Fault Handling | Covered | Exception handling tests |
| Boundary | Covered | Edge case queries |
| Event Sequence | Missing | - |
| Initialization | Missing | - |

**Test Results:**

| ID | Test Name | Type | Status | Duration |
|----|-----------|------|--------|----------|
| ITEM-HM-UT-020 | should find appointments by care unit | functional | passed | 320ms |

---

#### ITEM-HM-COMP-007: Escalator Module

**Path:** `modules/escalator/`

**Test Files:**
- modules/escalator/src/test/java/

**Coverage:**
- Lines: 85.0%
- Branches: 72.0%
- Functions: 90.0%

**Acceptance Criteria Status:**
| Criterion | Status | Tests |
|-----------|--------|-------|
| Functional | Covered | ITEM-HM-UT-040 |
| Event Sequence | Covered | Rule execution order tests |
| Data Flow | Covered | Priority calculation tests |
| Fault Handling | Covered | Error handling tests |
| Boundary | Covered | Edge case rules |
| Initialization | Covered | Engine startup tests |

**Test Results:**

| ID | Test Name | Type | Status | Duration |
|----|-----------|------|--------|----------|
| ITEM-HM-UT-040 | should escalate appointment based on priority rules | functional | passed | 156ms |

---

### 2.4 Untested Components

| Component | Reason | Recommendation |
|-----------|--------|----------------|
| ITEM-HM-COMP-010 (Audit Logging) | No dedicated tests | Add unit tests for audit logging service |

---

## 3. Integration Verification (IEC 62304 5.6)

### 3.1 Integration Test Summary

| Metric | Value |
|--------|-------|
| Integration Tests | 49 |
| Passed | 49 |
| Failed | 0 |
| Components Covered | 9 / 10 |
| Interfaces Tested | 4 / 5 |

### 3.2 Integration Tests

| ID | Name | Components | Coverage Type | Status |
|----|------|------------|---------------|--------|
| ITEM-HM-IT-001 | AppointmentControllerIT - full appointment lifecycle | COMP-001, COMP-002, COMP-003 | functionality | passed |
| ITEM-HM-IT-002 | HandoverServiceIT - handover workflow | COMP-002, COMP-003, COMP-006 | functionality | passed |
| ITEM-HM-IT-003 | ClinicAppointmentListIT - list pagination | COMP-001, COMP-002, COMP-009 | interface | passed |

### 3.3 Internal Interface Coverage

| Interface | Tests | Status |
|-----------|-------|--------|
| ITEM-HM-INT-001 (Appointment API) | ITEM-HM-IT-001 | Covered |
| ITEM-HM-INT-002 (Clinic API) | ITEM-HM-IT-003 | Covered |
| ITEM-HM-INT-003 (Handover Events) | ITEM-HM-IT-002 | Covered |
| ITEM-HM-INT-004 (Follow-Up API) | - | Partial |
| ITEM-HM-INT-005 (Referral API) | - | Missing |

### 3.4 Regression Test Suite

**Execution Frequency:** per-commit

**Last Full Run:** 2026-03-05T10:30:00Z

**Regression Tests:**
| ID | Name | Last Status | Last Run |
|----|------|-------------|----------|
| ITEM-HM-IT-001 | AppointmentControllerIT | passed | 2026-03-05T10:30:00Z |
| ITEM-HM-IT-002 | HandoverServiceIT | passed | 2026-03-05T10:30:00Z |
| ITEM-HM-IT-003 | ClinicAppointmentListIT | passed | 2026-03-05T10:30:00Z |

---

## 4. Requirement Verification (IEC 62304 5.7)

### 4.1 Requirements Traceability

| Requirement | Type | Test(s) | Status |
|-------------|------|---------|--------|
| ITEM-HM-REQ-FUNC-001 | Functional | ITEM-HM-UT-010, ITEM-HM-IT-001 | Verified |
| ITEM-HM-REQ-FUNC-002 | Functional | ITEM-HM-IT-002 | Verified |
| ITEM-HM-REQ-FUNC-017 | Functional | - | Missing |
| ITEM-HM-REQ-FUNC-018 | Functional | - | Missing |
| ITEM-HM-REQ-INT-001 | Interface | ITEM-HM-IT-001 | Verified |
| ITEM-HM-REQ-SEC-001 | Security | ITEM-HM-UT-002 | Verified |
| ITEM-HM-REQ-DATA-001 | Data | ITEM-HM-IT-001 | Verified |

### 4.2 Verification Details

#### ITEM-HM-REQ-FUNC-001: Appointment Lifecycle Management

**Type:** Functional

**Tests:**

| ID | Name | Input | Expected | Criteria | Status |
|----|------|-------|----------|----------|--------|
| ITEM-HM-UT-010 | should update appointment status | UpdateAppointmentDTO with ACCEPT action | Appointment status ACCEPTED | Status equals ACCEPTED | passed |
| ITEM-HM-IT-001 | full appointment lifecycle | Create, update, complete appointment | All state transitions succeed | State machine valid | passed |

**Verification Status:** verified

---

#### ITEM-HM-REQ-FUNC-002: Appointment Handover

**Type:** Functional

**Tests:**

| ID | Name | Input | Expected | Criteria | Status |
|----|------|-------|----------|----------|--------|
| ITEM-HM-IT-002 | handover workflow | Propose, accept handover | Handover completed | Status ACCEPTED | passed |

**Verification Status:** verified

---

### 4.3 Untested Requirements

| Requirement | Type | Reason |
|-------------|------|--------|
| ITEM-HM-REQ-FUNC-017 | Functional | no_test_exists |
| ITEM-HM-REQ-FUNC-018 | Functional | no_test_exists |

---

## 5. Risk Control Verification

### 5.1 Summary

| Hazard | Risk Control | Test | Status |
|--------|--------------|------|--------|
| ITEM-HM-HAZ-001 | ITEM-HM-RC-001 | ITEM-HM-UT-002 | Verified |
| ITEM-HM-HAZ-002 | ITEM-HM-RC-002 | ITEM-HM-IT-001 | Verified |
| ITEM-HM-HAZ-003 | ITEM-HM-RC-003 | - | Partial |
| ITEM-HM-HAZ-004 | ITEM-HM-RC-005 | ITEM-HM-UT-040 | Verified |
| ITEM-HM-HAZ-005 | ITEM-HM-RC-006 | - | Partial |

### 5.2 Details

#### ITEM-HM-HAZ-001: Unauthorized Access

**Risk Control:** ITEM-HM-RC-001 - JWT-based authentication

**Verification Test:** ITEM-HM-UT-002

**Test Description:** Verifies that unauthorized requests are rejected with 401/403 status

**Status:** Verified

**Evidence:** `svc/src/test/java/se/alerisx/mhp/manager/controller/v1/AppointmentControllerTest.java:should reject unauthorized request`

---

#### ITEM-HM-HAZ-002: Data Corruption

**Risk Control:** ITEM-HM-RC-002 - Transaction management

**Verification Test:** ITEM-HM-IT-001

**Test Description:** Verifies that failed operations roll back without data corruption

**Status:** Verified

**Evidence:** Integration tests with database rollback verification

---

#### ITEM-HM-HAZ-004: Escalation Failure

**Risk Control:** ITEM-HM-RC-005 - Rule validation and logging

**Verification Test:** ITEM-HM-UT-040

**Test Description:** Verifies escalation rules execute correctly

**Status:** Verified

**Evidence:** `modules/escalator/src/test/java/`

---

## 6. Anomalies

### 6.1 Open Anomalies

No open anomalies.

### 6.2 Resolved Anomalies

| ID | Source Test | Severity | Resolution | Resolved Date |
|----|-------------|----------|------------|---------------|
| ITEM-HM-ANO-001 | ITEM-HM-IT-003 | minor | Fixed count query to apply same filters | 2026-02-18 |
| ITEM-HM-ANO-002 | ITEM-HM-UT-011 | major | Added state transition validation | 2026-01-22 |

**ITEM-HM-ANO-001:** Pagination returns incorrect count for filtered queries
- **Problem Report:** HM-1234
- **Discovery:** 2026-02-15
- **Resolution:** Fixed count query to apply same filters as data query

**ITEM-HM-ANO-002:** State machine allows invalid FULFILLED to PROPOSED transition
- **Problem Report:** HM-1189
- **Discovery:** 2026-01-20
- **Resolution:** Added state transition validation in handover service

### 6.3 Deferred Anomalies

No deferred anomalies.

---

## 7. Test Environment

### 7.1 CI/CD Configuration

**System:** GitLab CI

**Workflow File:** `.gitlab-ci.yml`

**Triggers:**
- push to any branch
- merge_request

**Test Stages:**
1. test - Unit tests with JaCoCo coverage
2. integration-test - Integration tests with Testcontainers
3. verify - Coverage threshold verification

### 7.2 Runtime Environment

| Component | Version |
|-----------|---------|
| Operating System | Linux (GitLab runners) |
| Java | 21 |
| Maven | 3.9.x |

### 7.3 Test Tools

| Tool | Version | Purpose |
|------|---------|---------|
| JUnit 5 | 5.10.x | Unit and integration testing |
| Mockito | 5.x | Mocking framework |
| Testcontainers | 1.19.8 | Database integration testing |
| JaCoCo | 0.8.12 | Code coverage analysis |
| H2 Database | 1.4.200 | In-memory test database |

---

## 8. Gaps and Issues

### 8.1 Summary

| Priority | Count |
|----------|-------|
| High | 1 |
| Medium | 2 |
| Low | 1 |

### 8.2 Gap Details

| ID | Type | Description | Affected | IEC 62304 | Priority | Recommendation |
|----|------|-------------|----------|-----------|----------|----------------|
| ITEM-HM-VER-GAP-001 | untested_component | Audit logging component has no dedicated tests | ITEM-HM-COMP-010 | 5.5 | medium | Add unit tests for audit logging service |
| ITEM-HM-VER-GAP-002 | missing_interface_test | Referral API lacks integration tests | ITEM-HM-INT-005 | 5.6 | medium | Add integration tests for referral workflow |
| ITEM-HM-VER-GAP-003 | missing_acceptance_criteria | Memory management tests not applicable | ITEM-HM-COMP-002 | 5.5.4 | low | Document as N/A for Java applications |
| ITEM-HM-VER-GAP-004 | untested_requirement | 7 requirements lack verification tests | ITEM-HM-REQ-FUNC-017, ITEM-HM-REQ-FUNC-018 | 5.7.4 | high | Add tests or document alternative verification method |

**Gap Types:**
- `untested_component`: Component without test coverage
- `missing_acceptance_criteria`: Missing IEC 62304 5.5.3/5.5.4 criterion
- `untested_requirement`: Requirement without verification test
- `missing_interface_test`: Interface not tested

---

## 9. Traceability

### 9.1 Tests to Components

| Test | Components |
|------|------------|
| ITEM-HM-UT-001 | ITEM-HM-COMP-001 |
| ITEM-HM-UT-010 | ITEM-HM-COMP-002 |
| ITEM-HM-UT-020 | ITEM-HM-COMP-003 |
| ITEM-HM-UT-040 | ITEM-HM-COMP-007 |
| ITEM-HM-IT-001 | ITEM-HM-COMP-001, ITEM-HM-COMP-002, ITEM-HM-COMP-003 |
| ITEM-HM-IT-002 | ITEM-HM-COMP-002, ITEM-HM-COMP-003, ITEM-HM-COMP-006 |

### 9.2 Tests to Requirements

| Test | Requirements |
|------|--------------|
| ITEM-HM-UT-010 | ITEM-HM-REQ-FUNC-001 |
| ITEM-HM-UT-002 | ITEM-HM-REQ-SEC-001 |
| ITEM-HM-IT-001 | ITEM-HM-REQ-FUNC-001, ITEM-HM-REQ-DATA-001 |
| ITEM-HM-IT-002 | ITEM-HM-REQ-FUNC-002 |

### 9.3 Tests to Risks

| Test | Hazards |
|------|---------|
| ITEM-HM-UT-002 | ITEM-HM-HAZ-001 |
| ITEM-HM-IT-001 | ITEM-HM-HAZ-002 |
| ITEM-HM-UT-040 | ITEM-HM-HAZ-004 |

---

## Appendix A: Complete Test List

| ID | Name | Type | File | Status | Duration |
|----|------|------|------|--------|----------|
| ITEM-HM-UT-001 | should return appointment by id | unit | AppointmentControllerTest.java | passed | 125ms |
| ITEM-HM-UT-002 | should reject unauthorized request | unit | AppointmentControllerTest.java | passed | 85ms |
| ITEM-HM-UT-010 | should update appointment status to ACCEPTED | unit | UpdateAppointmentServiceAcceptTest.java | passed | 245ms |
| ITEM-HM-UT-011 | should reject invalid state transition | unit | UpdateAppointmentServiceUpdateTest.java | passed | 156ms |
| ITEM-HM-UT-012 | should handle null input gracefully | unit | GetAppointmentServiceImplTest.java | passed | 45ms |
| ITEM-HM-UT-020 | should find appointments by care unit | unit | QueryableAppointmentRepositoryTest.java | passed | 320ms |
| ITEM-HM-UT-030 | should serialize DTO correctly | unit | GetClinicAppointmentBillingListDTOTest.java | passed | 35ms |
| ITEM-HM-UT-040 | should escalate appointment based on priority rules | unit | EscalationRulesTest.java | passed | 156ms |
| ITEM-HM-UT-050 | should build TakeCare note object | unit | TakeCareObjectBuilderImplTest.java | passed | 89ms |
| ITEM-HM-UT-060 | should build consultation list query | unit | ListingServiceImplTest.java | passed | 245ms |
| ITEM-HM-IT-001 | full appointment lifecycle | integration | AppointmentControllerIT.java | passed | 3500ms |
| ITEM-HM-IT-002 | handover workflow | integration | AppointmentHandoverServiceIT.java | passed | 4200ms |
| ITEM-HM-IT-003 | list pagination | integration | ClinicAppointmentListIT.java | passed | 2800ms |

---

## Appendix B: Extraction Metadata

| Attribute | Value |
|-----------|-------|
| Repository | https://gitlab.com/doktor24/services/health-manager.git |
| Commit | 16e0273739eda65f73af5410323e1caf53362f16 |
| Extraction Date | 2026-03-06T15:30:00Z |
| Extractor Version | 2.0 |
| Standard | IEC 62304:2006+AMD1:2015 |

---

*This document is part of the regulatory documentation for Health Manager Service.*
*IEC 62304:2006+AMD1:2015 Clauses 5.5, 5.6, 5.7 Compliant - Item Level*
