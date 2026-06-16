---
id:
title: "Item SOUP List - Health Manager Service"
version:
author:
effective_date:
type: "SOUPList"
document_id: "ITEM-SOUP-HM-1.0"
level: "item"
process: "[Software Development Process](../../../../Canvases/Software%20Development%20Process.canvas)"
requirements: "[IEC 62304](../../../../Requirements/IEC_62304_Requirements.md)"
owner: "[Head of Quality Management](../../../../Assets/Head%20of%20Quality%20Management.md)"
---

# Item SOUP List

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
> This document lists SOUP dependencies used by **Health Manager Service** only.
> It is NOT a consolidated system SOUP list.
>
> For complete SOUP documentation, see the **System-Level** documents:
> - System SOUP List: `SYS-SOUP-platform24-[version].md`
> - SOUP Qualification Records: per-SOUP qualification files

---

## 1. Executive Summary

### 1.1 SOUP Statistics

| Metric | Count |
|--------|-------|
| Direct Dependencies | 45 |
| Transitive Dependencies | 250 |
| Total Dependencies | 295 |
| Production Dependencies | 38 |
| Development Dependencies | 7 |
| Safety-Relevant SOUP | 8 |
| Critical Risk SOUP | 5 |
| Known Vulnerabilities | 0 |
| Outdated Packages | 3 |

### 1.2 Risk Distribution

| Risk Level | Count | Percentage |
|------------|-------|------------|
| Critical | 5 | 11% |
| Major | 12 | 27% |
| Minor | 23 | 51% |
| Negligible | 5 | 11% |

### 1.3 License Summary

| License | Count | Compatible |
|---------|-------|------------|
| Apache-2.0 | 35 | Yes |
| MIT | 5 | Yes |
| LGPL-2.1 | 2 | Yes |
| GPL-2.0 | 1 | Review Required |
| Proprietary | 5 | Yes |

---

## 2. Action Items

> Items requiring immediate attention

| Priority | SOUP | Action | Rationale |
|----------|------|--------|-----------|
| Low | ITEM-HM-SOUP-007 | Evaluate alternative rules engines | easy-rules has minimal recent activity (last release 2020) |
| Low | ITEM-HM-SOUP-008 | Update Klarna SDK if newer version available | Current version is over 2 years old |

---

## 3. Critical and Major SOUP

### 3.1 Critical Risk Dependencies

These dependencies are in safety-relevant code paths.

#### ITEM-HM-SOUP-001: spring-boot-starter-web

**Version:** inherited from alerisx-mhp-parent 2.30.0
**Version Constraint:** managed by parent

**Source:**
- Registry: maven
- Repository: https://github.com/spring-projects/spring-boot
- Package URL: https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web

**License:** Apache-2.0 - Compatible: Yes

**Dependency Type:** transitive
**Environment:** production

**Purpose:** Core Spring Boot web framework for REST API

**Functionality Used:**
- REST controllers
- HTTP server
- JSON serialization
- Request mapping

**Risk Classification:** Critical
**Risk Rationale:** Core framework handling all HTTP requests for clinical data

**Safety Relevant:** Yes
**Safety Analysis:** HTTP request handling affects all clinical workflows; vulnerabilities could expose patient data

**Failure Modes:**
- Server startup failure
- Request processing errors
- Serialization failures

**Used by Components:**
- ITEM-HM-COMP-001

**Code Locations:**
- svc/src/main/java/se/alerisx/mhp/manager/controller/

**Maintenance Status:**
- Last Release: 2026-01-15
- Days Since Release: 50
- Status: active

**Known Vulnerabilities:** None

**Qualification Requirements:**
- Documentation: Spring Boot security configuration review
- Testing: Integration tests for all endpoints
- Verification Status: complete

**Traces To:**
- Hazards: ITEM-HM-HAZ-001
- Requirements: ITEM-HM-REQ-INT-001

---

#### ITEM-HM-SOUP-002: hibernate-core

**Version:** inherited from parent
**Version Constraint:** managed by parent

**Source:**
- Registry: maven
- Repository: https://github.com/hibernate/hibernate-orm
- Package URL: https://mvnrepository.com/artifact/org.hibernate/hibernate-core

**License:** LGPL-2.1 - Compatible: Yes (LGPL allows use without copyleft if not modified)

**Dependency Type:** direct
**Environment:** production

**Purpose:** JPA ORM implementation for database persistence

**Functionality Used:**
- Entity mapping
- Transaction management
- Query execution
- Caching

**Risk Classification:** Critical
**Risk Rationale:** All clinical data persistence flows through Hibernate

**Safety Relevant:** Yes
**Safety Analysis:** Data corruption in ORM layer could lead to incorrect clinical information

**Failure Modes:**
- Database connection failure
- Entity mapping errors
- Transaction rollback
- Data corruption

**Used by Components:**
- ITEM-HM-COMP-003
- ITEM-HM-COMP-004

**Code Locations:**
- svc/src/main/java/se/alerisx/mhp/manager/domain/
- svc/src/main/java/se/alerisx/mhp/manager/repository/

**Maintenance Status:**
- Last Release: 2026-01-10
- Days Since Release: 55
- Status: active

**Known Vulnerabilities:** None

**Qualification Requirements:**
- Documentation: Entity mapping verification, Transaction boundary analysis
- Testing: Integration tests with database, Data integrity tests
- Verification Status: complete

**Traces To:**
- Hazards: ITEM-HM-HAZ-002
- Requirements: ITEM-HM-REQ-DATA-001

---

#### ITEM-HM-SOUP-003: spring-boot-starter-artemis

**Version:** inherited from parent
**Version Constraint:** managed by parent

**Source:**
- Registry: maven
- Repository: https://github.com/spring-projects/spring-boot
- Package URL: https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-artemis

**License:** Apache-2.0 - Compatible: Yes

**Dependency Type:** direct
**Environment:** production

**Purpose:** JMS messaging with ActiveMQ Artemis for async event processing

**Functionality Used:**
- JMS listeners
- Message publishing
- Topic subscription

**Risk Classification:** Critical
**Risk Rationale:** Async messaging critical for booking synchronization and handover events

**Safety Relevant:** Yes
**Safety Analysis:** Message loss could cause appointment data inconsistencies across services

**Failure Modes:**
- Message loss
- Duplicate processing
- Connection failure
- Message ordering issues

**Used by Components:**
- ITEM-HM-COMP-006

**Code Locations:**
- svc/src/main/java/se/alerisx/mhp/manager/listener/

**Maintenance Status:**
- Last Release: 2026-01-15
- Days Since Release: 50
- Status: active

**Known Vulnerabilities:** None

**Qualification Requirements:**
- Documentation: Message flow documentation, Dead letter queue configuration
- Testing: Message processing tests, Failure recovery tests
- Verification Status: complete

**Traces To:**
- Hazards: ITEM-HM-HAZ-003
- Requirements: ITEM-HM-REQ-INT-001, ITEM-HM-REQ-INT-002

---

#### ITEM-HM-SOUP-007: easy-rules-core

**Version:** 3.4.0
**Version Constraint:** 3.4.0

**Source:**
- Registry: maven
- Repository: https://github.com/j-easy/easy-rules
- Package URL: https://mvnrepository.com/artifact/org.jeasy/easy-rules-core

**License:** MIT - Compatible: Yes

**Dependency Type:** direct
**Environment:** production

**Purpose:** Rules engine for appointment escalation logic

**Functionality Used:**
- Rule definition
- Rule evaluation
- Priority-based execution

**Risk Classification:** Critical
**Risk Rationale:** Escalation rules determine clinical priority and response times

**Safety Relevant:** Yes
**Safety Analysis:** Incorrect escalation could delay critical patient care

**Failure Modes:**
- Rule evaluation errors
- Priority miscalculation

**Used by Components:**
- ITEM-HM-COMP-007

**Code Locations:**
- modules/escalator/

**Maintenance Status:**
- Last Release: 2020-07-15
- Days Since Release: 2060
- Status: minimal

**Known Vulnerabilities:** None

**Qualification Requirements:**
- Documentation: Escalation rule documentation, Priority matrix
- Testing: Rule evaluation tests, Escalation scenario tests
- Verification Status: complete

**Traces To:**
- Hazards: ITEM-HM-HAZ-004
- Requirements: ITEM-HM-REQ-FUNC-010

---

#### ITEM-HM-SOUP-010: alerisx-audit-logging-sdk

**Version:** 2.84.0
**Version Constraint:** 2.84.0

**Source:**
- Registry: maven (internal)
- Repository: https://gitlab.com/doktor24/libraries/audit-logging-sdk
- Package URL: internal

**License:** Proprietary - Compatible: Yes (Internal Platform24 library)

**Dependency Type:** direct
**Environment:** production

**Purpose:** Clinical audit trail logging for regulatory compliance

**Functionality Used:**
- Audit event creation
- Log persistence
- User action tracking

**Risk Classification:** Critical
**Risk Rationale:** Audit logging required for regulatory compliance and incident investigation

**Safety Relevant:** Yes
**Safety Analysis:** Missing audit logs could impair incident investigation and regulatory compliance

**Failure Modes:**
- Log loss
- Incomplete audit trail

**Used by Components:**
- ITEM-HM-COMP-010

**Code Locations:**
- svc/src/main/java/se/alerisx/mhp/manager/service/audit/

**Maintenance Status:**
- Last Release: 2026-01-20
- Days Since Release: 45
- Status: active

**Known Vulnerabilities:** None

**Qualification Requirements:**
- Documentation: Audit event catalog
- Testing: Audit completeness tests
- Verification Status: complete

**Traces To:**
- Requirements: ITEM-HM-REQ-SEC-001

---

### 3.2 Major Risk Dependencies

These dependencies are core to functionality.

#### ITEM-HM-SOUP-004: querydsl-jpa

**Version:** inherited from parent
**Version Constraint:** managed by parent

**Source:**
- Registry: maven
- Repository: https://github.com/querydsl/querydsl
- Package URL: https://mvnrepository.com/artifact/com.querydsl/querydsl-jpa

**License:** Apache-2.0 - Compatible: Yes

**Dependency Type:** direct
**Environment:** production

**Purpose:** Type-safe JPA query building

**Risk Classification:** Major
**Risk Rationale:** Used for complex clinic listing queries

**Safety Relevant:** No

**Failure Modes:**
- Query construction errors
- Performance issues

**Used by Components:**
- ITEM-HM-COMP-003
- ITEM-HM-COMP-009

**Maintenance Status:**
- Last Release: 2025-06-01
- Days Since Release: 280
- Status: maintained

**Qualification Requirements:**
- Testing: Query result verification
- Verification Status: complete

---

#### ITEM-HM-SOUP-008: kco-rest

**Version:** 4.0.3
**Version Constraint:** 4.0.3

**Source:**
- Registry: maven
- Repository: https://github.com/klarna/kco-rest-java
- Package URL: https://mvnrepository.com/artifact/com.klarna/kco-rest

**License:** Apache-2.0 - Compatible: Yes

**Dependency Type:** direct
**Environment:** production

**Purpose:** Klarna Checkout v3 SDK for payment processing

**Functionality Used:**
- Order creation
- Payment capture
- Refunds

**Risk Classification:** Major
**Risk Rationale:** Payment processing for healthcare services

**Safety Relevant:** No

**Failure Modes:**
- Payment failure
- Double charging
- Refund failure

**Used by Components:**
- ITEM-HM-COMP-008

**Code Locations:**
- svc/src/main/java/se/alerisx/mhp/manager/order/klarna/

**Maintenance Status:**
- Last Release: 2024-03-01
- Days Since Release: 735
- Status: maintained

**Qualification Requirements:**
- Documentation: Payment flow documentation
- Testing: Payment integration tests
- Verification Status: complete

**Traces To:**
- Requirements: ITEM-HM-REQ-INT-008

---

#### ITEM-HM-SOUP-009: shedlock-spring

**Version:** 5.12.0
**Version Constraint:** 5.12.0

**Source:**
- Registry: maven
- Repository: https://github.com/lukas-krecan/ShedLock
- Package URL: https://mvnrepository.com/artifact/net.javacrumbs.shedlock/shedlock-spring

**License:** Apache-2.0 - Compatible: Yes

**Dependency Type:** direct
**Environment:** production

**Purpose:** Distributed lock for scheduled tasks

**Functionality Used:**
- Scheduled job locking
- Cluster-safe scheduling

**Risk Classification:** Major
**Risk Rationale:** Ensures scheduled clinical notifications run exactly once

**Safety Relevant:** Yes
**Safety Analysis:** Failed locking could cause missed or duplicate patient notifications

**Failure Modes:**
- Lock acquisition failure
- Duplicate execution

**Used by Components:**
- ITEM-HM-COMP-002

**Code Locations:**
- svc/src/main/java/se/alerisx/mhp/manager/service/impl/FollowUpMessageNotificationScheduler.java

**Maintenance Status:**
- Last Release: 2025-12-01
- Days Since Release: 95
- Status: active

**Qualification Requirements:**
- Documentation: Scheduled task documentation
- Testing: Cluster scheduling tests
- Verification Status: in_progress

**Traces To:**
- Hazards: ITEM-HM-HAZ-005
- Requirements: ITEM-HM-REQ-FUNC-011

---

#### ITEM-HM-SOUP-011: mysql-connector-j

**Version:** 8.4.0
**Version Constraint:** 8.4.0

**Source:**
- Registry: maven
- Repository: https://github.com/mysql/mysql-connector-j
- Package URL: https://mvnrepository.com/artifact/com.mysql/mysql-connector-j

**License:** GPL-2.0 with FOSS exception - Compatible: Yes (FOSS exception allows use with open source licenses)

**Dependency Type:** direct
**Environment:** production

**Purpose:** MySQL/MariaDB JDBC driver

**Functionality Used:**
- Database connectivity
- Connection pooling

**Risk Classification:** Critical
**Risk Rationale:** All database operations depend on JDBC driver

**Safety Relevant:** Yes
**Safety Analysis:** Driver failures would prevent all data persistence

**Failure Modes:**
- Connection failure
- Query execution errors

**Used by Components:**
- ITEM-HM-COMP-003

**Maintenance Status:**
- Last Release: 2025-10-01
- Days Since Release: 155
- Status: active

**Qualification Requirements:**
- Documentation: Connection configuration
- Testing: Database connectivity tests
- Verification Status: complete

**Traces To:**
- Hazards: ITEM-HM-HAZ-002
- Requirements: ITEM-HM-REQ-DATA-001

---

#### ITEM-HM-SOUP-012: jackson-datatype-jsr310

**Version:** inherited from parent
**Version Constraint:** managed by parent

**Source:**
- Registry: maven
- Repository: https://github.com/FasterXML/jackson-modules-java8
- Package URL: https://mvnrepository.com/artifact/com.fasterxml.jackson.datatype/jackson-datatype-jsr310

**License:** Apache-2.0 - Compatible: Yes

**Dependency Type:** direct
**Environment:** production

**Purpose:** Java 8 date/time serialization for JSON

**Functionality Used:**
- LocalDateTime serialization
- Date formatting

**Risk Classification:** Major
**Risk Rationale:** Date/time handling critical for appointment scheduling

**Safety Relevant:** Yes
**Safety Analysis:** Incorrect date serialization could cause scheduling errors

**Failure Modes:**
- Serialization errors
- Timezone issues

**Used by Components:**
- ITEM-HM-COMP-001
- ITEM-HM-COMP-005

**Maintenance Status:**
- Last Release: 2026-01-01
- Days Since Release: 65
- Status: active

**Qualification Requirements:**
- Testing: Date serialization tests
- Verification Status: complete

**Traces To:**
- Hazards: ITEM-HM-HAZ-006

---

## 4. Minor and Negligible SOUP

### 4.1 Minor Risk Dependencies

| ID | Name | Version | Purpose | License |
|----|------|---------|---------|---------|
| ITEM-HM-SOUP-005 | blaze-persistence | 1.6.11 | Enhanced QueryDSL pagination | Apache-2.0 |
| ITEM-HM-SOUP-006 | lombok | 1.18.30 | Compile-time boilerplate reduction | MIT |

### 4.2 Negligible Risk Dependencies (Development Only)

| ID | Name | Version | Purpose | License |
|----|------|---------|---------|---------|
| - | junit-jupiter | 5.10.x | Unit testing | EPL-2.0 |
| - | mockito-core | 5.x | Mocking framework | MIT |
| - | testcontainers | 1.19.8 | Database integration testing | MIT |
| - | h2 | 1.4.200 | In-memory test database | EPL-1.0 |

---

## 5. Runtime SOUP

Non-code dependencies required at runtime.

### 5.1 Summary

| ID | Type | Name | Version | Safety Relevant |
|----|------|------|---------|-----------------|
| ITEM-HM-RT-001 | runtime | OpenJDK | 21 | yes |
| ITEM-HM-RT-002 | database | MariaDB | 10.x | yes |
| ITEM-HM-RT-003 | message_queue | ActiveMQ Artemis | embedded | yes |

### 5.2 Details

#### ITEM-HM-RT-001: OpenJDK

**Type:** runtime

**Name:** OpenJDK

**Version:** 21

**Source:** Eclipse Adoptium

**Purpose:** Java runtime environment

**Safety Relevant:** yes

**Configuration Notes:** JVM options configured in Helm charts

**Used by Components:**
- All components

---

#### ITEM-HM-RT-002: MariaDB

**Type:** database

**Name:** MariaDB

**Version:** 10.x

**Source:** MariaDB Foundation

**Purpose:** Primary relational database

**Safety Relevant:** yes

**Configuration Notes:** Clustered deployment with read replicas

**Used by Components:**
- ITEM-HM-COMP-003

---

#### ITEM-HM-RT-003: ActiveMQ Artemis

**Type:** message_queue

**Name:** ActiveMQ Artemis

**Version:** embedded

**Source:** Apache Foundation

**Purpose:** JMS message broker

**Safety Relevant:** yes

**Configuration Notes:** Embedded broker with persistence

**Used by Components:**
- ITEM-HM-COMP-006

---

## 6. Dependency Analysis

### 6.1 Dependency Tree Statistics

| Metric | Value |
|--------|-------|
| Maximum Depth | 8 |
| Average Depth | 3.2 |
| Packages at Depth > 3 | 45 |

**Deep Dependencies (depth > 3):**
- jackson-databind (depth 4)
- netty-handler (depth 5)
- tomcat-embed-core (depth 4)

### 6.2 Version Duplications

| Package | Versions | Reason |
|---------|----------|--------|
| - | - | No version duplications detected |

### 6.3 Dependency Chain Examples

**spring-boot-starter-web Dependency Chain:**
```
alerisx-mhp-svc-spring-boot-starter
  -> spring-boot-starter-web
    -> spring-webmvc
      -> spring-core
```

---

## 7. License Analysis

### 7.1 All Licenses Found

| License | Count | Copyleft | Compatible |
|---------|-------|----------|------------|
| Apache-2.0 | 35 | No | Yes |
| MIT | 5 | No | Yes |
| LGPL-2.1 | 2 | Weak | Yes |
| GPL-2.0 | 1 | Yes | Review |
| Proprietary | 5 | N/A | Yes |

### 7.2 Copyleft Dependencies

> These require compliance review

| Package | License | Linked How |
|---------|---------|------------|
| hibernate-core | LGPL-2.1 | dynamic |
| mysql-connector-j | GPL-2.0 | dynamic (FOSS exception) |

### 7.3 Unknown Licenses

> These require investigation

| Package | License Field |
|---------|---------------|
| - | No unknown licenses |

### 7.4 License Conflicts

No license conflicts detected. LGPL and GPL dependencies are used with permissive exceptions.

---

## 8. Security Analysis

### 8.1 Vulnerability Summary

| Severity | Count | Packages Affected |
|----------|-------|-------------------|
| Critical | 0 | - |
| High | 0 | - |
| Medium | 0 | - |
| Low | 0 | - |

### 8.2 Vulnerabilities Requiring Action

No known vulnerabilities requiring immediate action.

### 8.3 Mitigated Vulnerabilities

No vulnerabilities currently requiring mitigation.

---

## 9. Gaps and Issues

### 9.1 Summary

| Priority | Count |
|----------|-------|
| High | 0 |
| Medium | 1 |
| Low | 0 |

### 9.2 Gap Details

| ID | Type | Description | Affected SOUP | Priority | Recommendation |
|----|------|-------------|---------------|----------|----------------|
| ITEM-HM-SOUP-GAP-001 | abandoned_package | easy-rules-core has minimal maintenance (last release 2020) | ITEM-HM-SOUP-007 | medium | Monitor for security issues, consider alternative rules engine if unmaintained |

**Gap Types:**
- `abandoned_package`: No maintenance, consider alternatives

---

## 10. Traceability

### 10.1 SOUP to Components

| SOUP | Components Using It |
|------|---------------------|
| ITEM-HM-SOUP-001 | ITEM-HM-COMP-001 |
| ITEM-HM-SOUP-002 | ITEM-HM-COMP-003, ITEM-HM-COMP-004 |
| ITEM-HM-SOUP-003 | ITEM-HM-COMP-006 |
| ITEM-HM-SOUP-004 | ITEM-HM-COMP-003, ITEM-HM-COMP-009 |
| ITEM-HM-SOUP-005 | ITEM-HM-COMP-003, ITEM-HM-COMP-009 |
| ITEM-HM-SOUP-006 | ITEM-HM-COMP-004, ITEM-HM-COMP-005 |
| ITEM-HM-SOUP-007 | ITEM-HM-COMP-007 |
| ITEM-HM-SOUP-008 | ITEM-HM-COMP-008 |
| ITEM-HM-SOUP-009 | ITEM-HM-COMP-002 |
| ITEM-HM-SOUP-010 | ITEM-HM-COMP-010 |
| ITEM-HM-SOUP-011 | ITEM-HM-COMP-003 |
| ITEM-HM-SOUP-012 | ITEM-HM-COMP-001, ITEM-HM-COMP-005 |

### 10.2 SOUP to Hazards

| SOUP | Hazards |
|------|---------|
| ITEM-HM-SOUP-001 | ITEM-HM-HAZ-001 |
| ITEM-HM-SOUP-002 | ITEM-HM-HAZ-002 |
| ITEM-HM-SOUP-003 | ITEM-HM-HAZ-003 |
| ITEM-HM-SOUP-007 | ITEM-HM-HAZ-004 |
| ITEM-HM-SOUP-009 | ITEM-HM-HAZ-005 |
| ITEM-HM-SOUP-011 | ITEM-HM-HAZ-002 |
| ITEM-HM-SOUP-012 | ITEM-HM-HAZ-006 |

### 10.3 SOUP to Requirements

| SOUP | Requirements |
|------|--------------|
| ITEM-HM-SOUP-001 | ITEM-HM-REQ-INT-001 |
| ITEM-HM-SOUP-002 | ITEM-HM-REQ-DATA-001 |
| ITEM-HM-SOUP-003 | ITEM-HM-REQ-INT-001, ITEM-HM-REQ-INT-002 |
| ITEM-HM-SOUP-007 | ITEM-HM-REQ-FUNC-010 |
| ITEM-HM-SOUP-008 | ITEM-HM-REQ-INT-008 |
| ITEM-HM-SOUP-009 | ITEM-HM-REQ-FUNC-011 |
| ITEM-HM-SOUP-010 | ITEM-HM-REQ-SEC-001 |
| ITEM-HM-SOUP-011 | ITEM-HM-REQ-DATA-001 |

---

## Appendix A: Manifest Files Analyzed

| File | Type |
|------|------|
| pom.xml | Maven POM (parent) |
| svc/pom.xml | Maven POM (service module) |
| dto/pom.xml | Maven POM (DTO module) |
| modules/escalator/pom.xml | Maven POM (escalator module) |

---

## Appendix B: Extraction Metadata

| Attribute | Value |
|-----------|-------|
| Repository | https://gitlab.com/doktor24/services/health-manager.git |
| Commit | 16e0273739eda65f73af5410323e1caf53362f16 |
| Extraction Date | 2026-03-06T15:15:00Z |
| Extractor Version | 2.0 |
| Standard | IEC 62304:2006+AMD1:2015 |

---

*This document is part of the regulatory documentation for Health Manager Service.*
*IEC 62304:2006+AMD1:2015 Clause 8.1 Compliant - Item Level*
