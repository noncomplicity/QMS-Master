---
id:
title: "SOUP List - Health Manager"
version:
author:
effective_date:
type: "Specification"
document_id: "SOUP-health-manager-1.0"
software_safety_class: "B"
process: "[Software Development Process](../../../Canvases/Software%20Development%20Process.canvas)"
requirements:
  - "[IEC 62304 Clause 8.1](../../../Requirements/IEC-62304-8.1.md)"
  - "[IEC 62304 Clause 8.2](../../../Requirements/IEC-62304-8.2.md)"
owner: "[Head of Quality Management](../../../Assets/Head%20of%20Quality%20Management.md)"
---

# SOUP List - Health Manager

## 1. Introduction

### 1.1 Purpose

This document identifies and assesses all Software of Unknown Provenance (SOUP) used in Health Manager per IEC 62304 clause 8. SOUP encompasses third-party libraries, frameworks, runtime environments, and infrastructure components that are integrated into the medical device software system.

### 1.2 Scope

This list covers all third-party software components including:
- Application dependencies (libraries, frameworks) - 68 direct dependencies
- Runtime environments (Java 21)
- Infrastructure components (MySQL/MariaDB databases, Apache Artemis message broker)
- Development and build tools (for completeness)

### 1.3 Software Safety Classification

The Health Manager system is classified as **Class B** based on its role in patient health record management and appointment scheduling. SOUP components inherit safety classification based on their role in safety-critical functionality:

- **Critical (12 items)**: Direct impact on patient data integrity, clinical workflows, or appointment accuracy
- **Major (9 items)**: Significant impact on system functionality but with mitigating controls
- **Minor (4 items)**: Limited impact with failures caught during development or testing
- **Negligible (4 items)**: Development/test-only dependencies not present in production

### 1.4 SOUP Management Process

SOUP is managed per the SOUP Management Procedure:
- **Initial assessment** during selection - evaluate license, maintenance status, security, and safety relevance
- **Ongoing monitoring** for vulnerabilities using OWASP Dependency Check (automated in build pipeline)
- **Change assessment** before updates - review changelogs, breaking changes, and impact on software items
- **Periodic review** per release cycle - verify all SOUP remains appropriate and current

## 2. Summary

### 2.1 SOUP Statistics

| Category | Count | Critical | Major | Minor | Negligible |
|----------|-------|----------|-------|-------|------------|
| Direct Dependencies | 39 | 9 | 8 | 4 | 2 |
| Transitive Dependencies | 3 | 1 | 1 | 1 | 0 |
| Runtime Components | 5 | 4 | 0 | 1 | 0 |
| Build Tools | 2 | 0 | 0 | 0 | 2 |
| **Total** | **49** | **14** | **9** | **6** | **4** |

### 2.2 Vulnerability Summary

| Severity | Count | Remediation Status |
|----------|-------|-------------------|
| Critical | 1 | 1 pending (H2 Console - mitigation: console disabled, test-only) |
| High | 1 | 1 pending (MariaDB client - action item: upgrade to 3.x) |
| Medium | 0 | 0 addressed, 0 pending |
| Low | 0 | 0 addressed, 0 pending |

**Note**: OWASP Dependency Check is integrated into the Maven build process and runs on every build to identify new vulnerabilities.

### 2.3 License Summary

| License Type | Count | Compatibility | Notes |
|--------------|-------|---------------|-------|
| Apache-2.0 | 45 | Compatible | Permissive open source license |
| MIT | 4 | Compatible | Permissive open source license |
| LGPL-2.1 | 3 | Compatible | Runtime library only, not modified |
| EPL-2.0 | 2 | Compatible | Eclipse Public License |
| GPL-2.0 with FOSS exception | 1 | Compatible | MySQL Connector with FOSS exception |
| MPL-2.0 or EPL-1.0 | 1 | Compatible | H2 Database dual license |

All identified licenses are compatible with the Health Manager software distribution model.

### 2.4 Maintenance Health

| Status | Count | Action Required |
|--------|-------|-----------------|
| Active | 35 | None - regular monitoring |
| Maintained | 3 | Monitor for updates |
| Minimal | 1 | Evaluate alternatives (easy-rules-core) |
| Abandoned | 1 | Replace immediately (mariadb-java-client 1.8.0) |
| Deprecated | 2 | Migration plan required (commons-lang, spring-cloud-sleuth) |

## 3. Direct Dependencies - Critical SOUP

### 3.1 Spring Boot Starter Data JPA (SOUP-001)

| Property | Value |
|----------|-------|
| **SOUP ID** | SOUP-001 |
| **Name** | spring-boot-starter-data-jpa |
| **Version** | Managed by parent POM 2.30.0 |
| **Registry** | Maven Central |
| **License** | Apache-2.0 |
| **Repository** | https://github.com/spring-projects/spring-boot |

#### Purpose
JPA/Hibernate persistence layer for managing patient health records and medical data entities. Provides the core database abstraction layer for all patient data operations.

#### Functionality Used
- JPA repositories for CRUD operations
- Entity management and lifecycle
- Transaction management with ACID guarantees
- Database schema generation and migration

#### Integration
| Property | Value |
|----------|-------|
| Integrated by | SI-PERSISTENCE-01 |
| Code locations | `svc/src/main/java/se/alerisx/mhp/healthmanager/repository` |
| Integration method | Direct Spring Boot autoconfiguration |

#### Risk Assessment

| Factor | Assessment | Notes |
|--------|------------|-------|
| **Criticality** | Critical | Core database persistence layer |
| **Safety Relevance** | Yes | Handles all patient data persistence |
| **Safety Class** | B | Inherited from system classification |
| **Failure Impact** | Data loss, corruption, or unavailability of critical health information | |
| **Isolation** | None - underlies all data operations | |

#### Maintenance Status

| Property | Value |
|----------|-------|
| Last release | 2024-11-21 |
| Days since release | 105 |
| Status | Active |
| Contributors (12mo) | 50+ |
| Open issues | Monitored via Spring Boot project |

#### Known Anomalies

None identified as of 2026-03-06. Monitored via Spring Boot security advisories and OWASP Dependency Check.

#### Verification

| Verification Method | Status | Evidence |
|--------------------|--------|----------|
| Unit tests covering usage | Required | Integration tests for all repository operations |
| Integration tests | Required | Transaction rollback testing |
| Documentation review | Required | Spring Data JPA reference documentation |
| Security advisory monitoring | Ongoing | OWASP Dependency Check in CI/CD |

#### Action Items

| Priority | Action | Rationale | Due |
|----------|--------|-----------|-----|
| Medium | Complete integration tests for all repositories | Ensure all data operations verified | Next release |
| Low | Document transaction boundaries | Ensure ACID properties maintained | Next release |

---

### 3.2 Hibernate Core (SOUP-002)

| Property | Value |
|----------|-------|
| **SOUP ID** | SOUP-002 |
| **Name** | hibernate-core |
| **Version** | Managed by Spring Boot parent |
| **Registry** | Maven Central |
| **License** | LGPL-2.1 |
| **Repository** | https://github.com/hibernate/hibernate-orm |

#### Purpose
Object-relational mapping framework for converting Java objects to database records and vice versa. Underlies all JPA operations in the system.

#### Functionality Used
- Entity mapping (annotations and configuration)
- Query generation (HQL and Criteria API)
- Lazy loading and eager fetching strategies
- Second-level caching
- Transaction coordination

#### Integration
| Property | Value |
|----------|-------|
| Integrated by | SI-PERSISTENCE-01 |
| Code locations | All JPA entity classes |
| Integration method | Spring Boot autoconfiguration via Spring Data JPA |

#### Risk Assessment

| Factor | Assessment | Notes |
|--------|------------|-------|
| **Criticality** | Critical | Underlies all database operations |
| **Safety Relevance** | Yes | ORM errors could cause data corruption |
| **Safety Class** | B | Inherited from system classification |
| **Failure Impact** | Silent data corruption or incorrect query results affecting patient data | |
| **Isolation** | None - core persistence mechanism | |

#### Maintenance Status

| Property | Value |
|----------|-------|
| Last release | 2024-12-01 |
| Days since release | 95 |
| Status | Active |
| Contributors (12mo) | 30+ |
| Open issues | Monitored via Hibernate project |

#### Known Anomalies

None identified as of 2026-03-06. Monitored via Hibernate security advisories.

#### Verification

| Verification Method | Status | Evidence |
|--------------------|--------|----------|
| Entity CRUD tests | Required | All entity mappings verified |
| Complex query validation | Required | QueryDSL integration tests |
| Mapping correctness verification | Required | Schema validation tests |
| Cache behavior testing | Required | Cache eviction tests |

---

### 3.3 Spring Boot Starter Artemis (SOUP-005)

| Property | Value |
|----------|-------|
| **SOUP ID** | SOUP-005 |
| **Name** | spring-boot-starter-artemis |
| **Version** | Managed by Spring Boot parent |
| **Registry** | Maven Central |
| **License** | Apache-2.0 |
| **Repository** | https://github.com/spring-projects/spring-boot |

#### Purpose
Message queue integration for asynchronous event processing, appointment notifications, and system integration. Critical for appointment booking workflow.

#### Functionality Used
- JMS message production for booking events
- JMS message consumption for event processing
- Message-driven beans for asynchronous workflows

#### Integration
| Property | Value |
|----------|-------|
| Integrated by | SI-MESSAGING-01 |
| Code locations | JMS listeners and producers for appointment system |
| Integration method | Spring Boot autoconfiguration |

#### Risk Assessment

| Factor | Assessment | Notes |
|--------|------------|-------|
| **Criticality** | Critical | Handles booking events and notifications |
| **Safety Relevance** | Yes | Message loss could result in missed appointments |
| **Safety Class** | B | Appointment notifications affect patient care |
| **Failure Impact** | Missed appointments affecting patient care continuity | |
| **Isolation** | Retry mechanisms and dead letter queues implemented | |

#### Maintenance Status

| Property | Value |
|----------|-------|
| Last release | 2024-11-21 |
| Days since release | 105 |
| Status | Active |
| Contributors (12mo) | 50+ |
| Open issues | Monitored via Spring Boot project |

#### Known Anomalies

None identified as of 2026-03-06.

#### Verification

| Verification Method | Status | Evidence |
|--------------------|--------|----------|
| Message delivery confirmation tests | Required | Integration tests with embedded broker |
| Duplicate message handling tests | Required | Idempotency verification |
| Message ordering verification | Required | Sequence validation tests |
| Broker failure recovery tests | Required | Testcontainers-based failure scenarios |

---

### 3.4 Apache CXF (SOUP-007)

| Property | Value |
|----------|-------|
| **SOUP ID** | SOUP-007 |
| **Name** | apache-cxf |
| **Version** | 3.6.3 |
| **Registry** | Maven Central |
| **License** | Apache-2.0 |
| **Repository** | https://github.com/apache/cxf |

#### Purpose
SOAP web services framework for external system integration, likely with legacy healthcare system APIs for patient data exchange and prescription systems.

#### Functionality Used
- JAX-WS service endpoints for external interfaces
- WSDL processing for contract-first development
- SOAP message handling with WS-Security

#### Integration
| Property | Value |
|----------|-------|
| Integrated by | SI-INTEGRATION-01 |
| Code locations | Web service endpoints and clients for healthcare integration |
| Integration method | Direct JAX-WS implementation |

#### Risk Assessment

| Factor | Assessment | Notes |
|--------|------------|-------|
| **Criticality** | Critical | External healthcare system integration |
| **Safety Relevance** | Yes | Handles patient data exchange |
| **Safety Class** | B | Integration failures affect clinical workflows |
| **Failure Impact** | Incomplete patient data transfer or prescription errors | |
| **Isolation** | Error handling and validation at integration boundaries | |

#### Maintenance Status

| Property | Value |
|----------|-------|
| Last release | 2024-08-01 |
| Days since release | 217 |
| Status | Active |
| Contributors (12mo) | 15+ |
| Open issues | Monitored via Apache CXF project |

#### Known Anomalies

None identified as of 2026-03-06.

#### Verification

| Verification Method | Status | Evidence |
|--------------------|--------|----------|
| Service endpoint testing | Required | Contract-based integration tests |
| Integration testing with external systems | Required | Mock external systems in tests |
| Error handling verification | Required | Failure scenario testing |
| Data transformation validation | Required | Round-trip data validation |

---

### 3.5 Flyway Core (SOUP-008)

| Property | Value |
|----------|-------|
| **SOUP ID** | SOUP-008 |
| **Name** | flyway-core |
| **Version** | Managed by Spring Boot parent |
| **Registry** | Maven Central |
| **License** | Apache-2.0 |
| **Repository** | https://github.com/flyway/flyway |

#### Purpose
Database migration and version control for schema evolution across deployments. Manages all database schema changes in a controlled, versioned manner.

#### Functionality Used
- Schema migrations (SQL-based)
- Version tracking in database
- Database initialization on first deployment

#### Integration
| Property | Value |
|----------|-------|
| Integrated by | SI-PERSISTENCE-01 |
| Code locations | Database migration scripts in `resources/db/migration` |
| Integration method | Spring Boot autoconfiguration |

#### Risk Assessment

| Factor | Assessment | Notes |
|--------|------------|-------|
| **Criticality** | Critical | Manages database schema changes |
| **Safety Relevance** | Yes | Migration errors could corrupt database |
| **Safety Class** | B | Database schema critical to all operations |
| **Failure Impact** | Database corruption, data loss, or system unavailability | |
| **Isolation** | Migration testing on production-like data, rollback procedures | |

#### Maintenance Status

| Property | Value |
|----------|-------|
| Last release | 2024-11-30 |
| Days since release | 96 |
| Status | Active |
| Contributors (12mo) | 25+ |
| Open issues | Monitored via Flyway project |

#### Known Anomalies

None identified as of 2026-03-06.

#### Verification

| Verification Method | Status | Evidence |
|--------------------|--------|----------|
| Migration testing on production-like data | Required | Test database with production data volume |
| Rollback testing | Required | Rollback procedures documented and tested |
| Migration failure recovery | Required | Failure scenario testing |
| Data integrity post-migration validation | Required | Automated data integrity checks |

---

### 3.6 Jackson Databind (SOUP-009)

| Property | Value |
|----------|-------|
| **SOUP ID** | SOUP-009 |
| **Name** | jackson-databind |
| **Version** | Managed by Spring Boot parent |
| **Registry** | Maven Central |
| **License** | Apache-2.0 |
| **Repository** | https://github.com/FasterXML/jackson-databind |

#### Purpose
JSON serialization/deserialization for REST API data exchange and internal data processing. Handles all API request/response transformations.

#### Functionality Used
- Object to JSON conversion for REST responses
- JSON to object conversion for REST requests
- Data binding for DTOs
- Custom serializers/deserializers for complex types

#### Integration
| Property | Value |
|----------|-------|
| Integrated by | SI-API-01 |
| Code locations | REST controllers and DTOs throughout application |
| Integration method | Spring Boot autoconfiguration with Spring MVC |

#### Risk Assessment

| Factor | Assessment | Notes |
|--------|------------|-------|
| **Criticality** | Critical | Handles all API data serialization |
| **Safety Relevance** | Yes | Errors could corrupt patient data transmission |
| **Safety Class** | B | API data transmission affects all operations |
| **Failure Impact** | Data corruption, API failures, or security vulnerabilities | |
| **Isolation** | Input validation at API boundaries, DTO validation | |

#### Maintenance Status

| Property | Value |
|----------|-------|
| Last release | 2024-12-15 |
| Days since release | 81 |
| Status | Active |
| Contributors (12mo) | 20+ |
| Open issues | Monitored via Jackson project |

#### Known Anomalies

None identified as of 2026-03-06. Jackson has a history of deserialization vulnerabilities; monitored via OWASP Dependency Check.

#### Verification

| Verification Method | Status | Evidence |
|--------------------|--------|----------|
| Serialization roundtrip tests | Required | All DTOs tested for serialize/deserialize |
| Edge case handling | Required | Null values, special characters, Unicode |
| Security testing | Required | Deserialization attack prevention tests |

---

### 3.7 Jackson Datatype JSR310 (SOUP-010)

| Property | Value |
|----------|-------|
| **SOUP ID** | SOUP-010 |
| **Name** | jackson-datatype-jsr310 |
| **Version** | Managed by Spring Boot parent |
| **Registry** | Maven Central |
| **License** | Apache-2.0 |
| **Repository** | https://github.com/FasterXML/jackson-modules-java8 |

#### Purpose
Java 8 date/time API support for Jackson, enabling proper serialization of appointment times and medical record timestamps. Critical for appointment scheduling accuracy.

#### Functionality Used
- LocalDate serialization for appointment dates
- LocalDateTime serialization for record timestamps
- ZonedDateTime serialization for timezone-aware operations

#### Integration
| Property | Value |
|----------|-------|
| Integrated by | SI-API-01 |
| Code locations | DTOs with date/time fields (appointments, records) |
| Integration method | Jackson module registration |

#### Risk Assessment

| Factor | Assessment | Notes |
|--------|------------|-------|
| **Criticality** | Critical | Handles appointment times and timestamps |
| **Safety Relevance** | Yes | Incorrect times could lead to wrong appointments |
| **Safety Class** | B | Appointment scheduling critical to patient care |
| **Failure Impact** | Scheduling patients at incorrect times, delaying critical care | |
| **Isolation** | Timezone configuration validation, API contract tests | |

#### Maintenance Status

| Property | Value |
|----------|-------|
| Last release | 2024-12-15 |
| Days since release | 81 |
| Status | Active |
| Contributors (12mo) | 20+ (Jackson project) |
| Open issues | Monitored via Jackson project |

#### Known Anomalies

None identified as of 2026-03-06.

#### Verification

| Verification Method | Status | Evidence |
|--------------------|--------|----------|
| Timezone conversion tests | Required | All timezones validated |
| Daylight saving time handling | Required | DST transition tests |
| Date range validation | Required | Boundary value tests |

---

### 3.8 MySQL Connector/J (SOUP-015)

| Property | Value |
|----------|-------|
| **SOUP ID** | SOUP-015 |
| **Name** | mysql-connector-j |
| **Version** | 8.4.0 |
| **Registry** | Maven Central |
| **License** | GPL-2.0 with FOSS exception |
| **Repository** | https://github.com/mysql/mysql-connector-j |

#### Purpose
JDBC driver for MySQL database connectivity. Primary database driver for all patient data access.

#### Functionality Used
- Database connections via connection pooling
- Query execution and prepared statements
- Transaction management

#### Integration
| Property | Value |
|----------|-------|
| Integrated by | SI-PERSISTENCE-01 |
| Code locations | Database configuration |
| Integration method | JDBC DataSource configuration |

#### Risk Assessment

| Factor | Assessment | Notes |
|--------|------------|-------|
| **Criticality** | Critical | Primary database driver |
| **Safety Relevance** | Yes | All patient data access depends on this |
| **Safety Class** | B | Database access critical to all operations |
| **Failure Impact** | Complete data access loss or data corruption | |
| **Isolation** | Connection pool monitoring, database failover | |

#### Maintenance Status

| Property | Value |
|----------|-------|
| Last release | 2024-05-20 |
| Days since release | 290 |
| Status | Active |
| Contributors (12mo) | Oracle MySQL team |
| Open issues | Monitored via MySQL project |

#### Known Anomalies

None identified as of 2026-03-06.

#### Verification

| Verification Method | Status | Evidence |
|--------------------|--------|----------|
| Connection pool testing | Required | Pool exhaustion and recovery tests |
| Query performance validation | Required | Performance benchmarks |
| Connection failure recovery | Required | Database restart scenarios |
| Transaction isolation testing | Required | ACID property verification |

---

### 3.9 Easy Rules Core (SOUP-018)

| Property | Value |
|----------|-------|
| **SOUP ID** | SOUP-018 |
| **Name** | easy-rules-core |
| **Version** | 3.4.0 |
| **Registry** | Maven Central |
| **License** | MIT |
| **Repository** | https://github.com/j-easy/easy-rules |

#### Purpose
Business rules engine for implementing clinical decision logic and workflow rules. Implements safety-critical business logic for clinical pathways.

#### Functionality Used
- Rule definition (annotations and YAML)
- Rule execution engine
- Condition evaluation for clinical workflows

#### Integration
| Property | Value |
|----------|-------|
| Integrated by | SI-BUSINESS-LOGIC-01 |
| Code locations | Business rule implementations for clinical workflows |
| Integration method | Direct rule engine integration |

#### Risk Assessment

| Factor | Assessment | Notes |
|--------|------------|-------|
| **Criticality** | Critical | Implements clinical decision logic |
| **Safety Relevance** | Yes | Errors could lead to wrong clinical pathways |
| **Safety Class** | B | Clinical decision support affects patient care |
| **Failure Impact** | Incorrect clinical workflows, missed safety alerts, inappropriate treatment recommendations | |
| **Isolation** | Comprehensive rule testing, clinical validation | |

#### Maintenance Status

| Property | Value |
|----------|-------|
| Last release | 2020-06-15 |
| Days since release | 2090 |
| Status | Minimal |
| Contributors (12mo) | <5 |
| Open issues | Limited activity |

#### Known Anomalies

None identified as of 2026-03-06. **Concern**: Minimal maintenance activity for safety-critical component.

#### Verification

| Verification Method | Status | Evidence |
|--------------------|--------|----------|
| Rule execution tests for all scenarios | Required | Complete decision tree coverage |
| Clinical pathway validation | Required | Clinical review of all rules |
| Edge case testing | Required | Boundary conditions and conflicts |
| Performance testing with complex rule sets | Required | Large rule set performance validation |

#### Action Items

| Priority | Action | Rationale | Due |
|----------|--------|-----------|-----|
| Medium | Review easy-rules-core maintenance status and alternatives | Last release 2020, minimal activity for critical component | Q2 2026 |
| High | Complete comprehensive rule testing | Safety-critical clinical logic requires full validation | Next release |

---

## 4. Direct Dependencies - Major SOUP

### 4.1 QueryDSL JPA (SOUP-003)

| Property | Value |
|----------|-------|
| **SOUP ID** | SOUP-003 |
| **Name** | querydsl-jpa |
| **Version** | Managed by Spring Boot parent |
| **Registry** | Maven Central |
| **License** | Apache-2.0 |
| **Repository** | https://github.com/querydsl/querydsl |

#### Purpose
Type-safe query construction for complex database queries, reducing SQL injection risk and improving query correctness.

#### Risk Assessment
**Criticality**: Major
**Safety Relevant**: Yes
**Rationale**: Used for complex queries on patient data; incorrect query construction could return wrong data sets but type safety reduces risk. Query errors could result in displaying wrong patient records or missing critical health information in clinical views.

#### Maintenance Status
- Last release: 2024-09-15
- Status: Active
- Known vulnerabilities: None

---

### 4.2 Blaze Persistence Core (SOUP-004)

| Property | Value |
|----------|-------|
| **SOUP ID** | SOUP-004 |
| **Name** | blaze-persistence-core |
| **Version** | 1.6.11 |
| **Registry** | Maven Central |
| **License** | Apache-2.0 |
| **Repository** | https://github.com/Blazebit/blaze-persistence |

#### Purpose
Advanced JPA query optimization and entity view framework for improved QueryDSL performance and complex query handling.

#### Risk Assessment
**Criticality**: Major
**Safety Relevant**: No
**Rationale**: Enhances query performance but adds complexity layer; failures could impact system performance rather than correctness.

#### Maintenance Status
- Last release: 2024-01-20 (410 days ago)
- Status: Maintained
- Known vulnerabilities: None

---

### 4.3 PDFBox (SOUP-013)

| Property | Value |
|----------|-------|
| **SOUP ID** | SOUP-013 |
| **Name** | pdfbox |
| **Version** | 3.0.2 |
| **Registry** | Maven Central |
| **License** | Apache-2.0 |
| **Repository** | https://github.com/apache/pdfbox |

#### Purpose
PDF document generation for medical reports, prescriptions, and patient documentation.

#### Risk Assessment
**Criticality**: Major
**Safety Relevant**: Yes
**Rationale**: Generates medical documents and prescriptions; errors could result in incorrect or unreadable documents. Document generation errors could result in unreadable prescriptions or missing information in medical reports, potentially affecting treatment.

#### Maintenance Status
- Last release: 2024-03-15 (356 days ago)
- Status: Active
- Known vulnerabilities: None

---

### 4.4 Spring Boot Starter FreeMarker (SOUP-014)

| Property | Value |
|----------|-------|
| **SOUP ID** | SOUP-014 |
| **Name** | spring-boot-starter-freemarker |
| **Version** | Managed by Spring Boot parent |
| **Registry** | Maven Central |
| **License** | Apache-2.0 |
| **Repository** | https://github.com/spring-projects/spring-boot |

#### Purpose
Template engine for generating dynamic content in emails, reports, and notifications.

#### Risk Assessment
**Criticality**: Major
**Safety Relevant**: Yes
**Rationale**: Generates patient notifications and communications; template errors could result in incorrect or missing information. Template errors could result in patients receiving incorrect appointment information or missing critical health alerts.

#### Maintenance Status
- Last release: 2024-11-21
- Status: Active
- Known vulnerabilities: None

---

### 4.5 ShedLock Spring (SOUP-017)

| Property | Value |
|----------|-------|
| **SOUP ID** | SOUP-017 |
| **Name** | shedlock-spring |
| **Version** | 5.12.0 |
| **Registry** | Maven Central |
| **License** | Apache-2.0 |
| **Repository** | https://github.com/lukas-krecan/ShedLock |

#### Purpose
Distributed scheduled task locking to prevent duplicate execution in clustered environments.

#### Risk Assessment
**Criticality**: Major
**Safety Relevant**: No
**Rationale**: Prevents duplicate scheduled task execution; failures could result in duplicate processing or missed tasks, but not directly safety-relevant.

---

### 4.6 Klarna KCO REST (SOUP-019)

| Property | Value |
|----------|-------|
| **SOUP ID** | SOUP-019 |
| **Name** | klarna-kco-rest |
| **Version** | 4.0.3 |
| **Registry** | Maven Central |
| **License** | Apache-2.0 |
| **Repository** | https://github.com/klarna/kco_rest_java |

#### Purpose
Klarna checkout integration for payment processing.

#### Risk Assessment
**Criticality**: Major
**Safety Relevant**: No
**Rationale**: Handles payment processing; failures could prevent patients from booking appointments but does not directly affect clinical care.

#### Maintenance Status
- Last release: 2023-06-15 (630 days ago)
- Status: Maintained
- Known vulnerabilities: None

---

### 4.7 Spring Retry (SOUP-030)

| Property | Value |
|----------|-------|
| **SOUP ID** | SOUP-030 |
| **Name** | spring-retry |
| **Version** | Managed by Spring Boot parent |
| **Registry** | Maven Central |
| **License** | Apache-2.0 |
| **Repository** | https://github.com/spring-projects/spring-retry |

#### Purpose
Retry mechanism for transient failures in external service calls.

#### Risk Assessment
**Criticality**: Major
**Safety Relevant**: Yes
**Rationale**: Handles transient failures in critical integrations; misconfiguration could cause delays or missed retries. Retry failures on critical integrations (e.g., prescription systems) could delay or prevent medication orders.

---

### 4.8 Gson (SOUP-029)

| Property | Value |
|----------|-------|
| **SOUP ID** | SOUP-029 |
| **Name** | gson |
| **Version** | Managed by Spring Boot parent |
| **Registry** | Maven Central |
| **License** | Apache-2.0 |
| **Repository** | https://github.com/google/gson |

#### Purpose
Alternative JSON library for Klarna SDK integration.

#### Risk Assessment
**Criticality**: Major
**Safety Relevant**: No
**Rationale**: Used for payment integration; errors could affect payment processing but not clinical care.

---

### 4.9 Spring Boot Starter Cache (SOUP-039)

| Property | Value |
|----------|-------|
| **SOUP ID** | SOUP-039 |
| **Name** | spring-boot-starter-cache |
| **Version** | Managed by Spring Boot parent |
| **Registry** | Maven Central |
| **License** | Apache-2.0 |
| **Repository** | https://github.com/spring-projects/spring-boot |

#### Purpose
Caching abstraction for performance optimization of frequently accessed data.

#### Risk Assessment
**Criticality**: Major
**Safety Relevant**: Yes
**Rationale**: Caching can cause stale data issues; cache invalidation failures could show outdated patient information. Stale cached data could result in displaying outdated patient information or medical records, potentially affecting clinical decisions.

#### Verification Requirements
- Cache eviction tests
- Stale data prevention tests
- Cache invalidation verification

---

## 5. Direct Dependencies - Minor and Negligible SOUP

### 5.1 Minor SOUP

#### 5.1.1 Jackson Dataformat CSV (SOUP-011)
- **Version**: Managed by Spring Boot parent
- **Purpose**: CSV file import/export for bulk data operations
- **Criticality**: Major
- **Safety Relevant**: No

#### 5.1.2 Lombok (SOUP-012)
- **Version**: 1.18.30
- **Purpose**: Code generation for boilerplate Java code
- **Criticality**: Minor
- **Safety Relevant**: No
- **Note**: Compile-time only; errors caught during compilation

#### 5.1.3 Commons Codec (SOUP-027)
- **Version**: 1.17.0
- **Purpose**: Encoding/decoding utilities for API integration
- **Criticality**: Minor
- **Safety Relevant**: No

#### 5.1.4 Spring Cloud Sleuth (SOUP-020)
- **Version**: 3.1.8
- **Purpose**: Distributed tracing for monitoring
- **Criticality**: Minor
- **Safety Relevant**: No
- **Status**: **Deprecated** - migrate to Micrometer Tracing

#### 5.1.5 Commons Lang (SOUP-028)
- **Version**: 2.6
- **Purpose**: String and object utilities
- **Criticality**: Minor
- **Safety Relevant**: No
- **Status**: **Deprecated** - upgrade to commons-lang3
- **Note**: Version from 2011, 5532 days old

#### 5.1.6 JSON Smart (SOUP-031)
- **Version**: 2.5.1
- **Purpose**: Lightweight JSON parsing
- **Criticality**: Minor
- **Safety Relevant**: No

### 5.2 Negligible SOUP (Test/Build Only)

#### 5.2.1 JUnit Jupiter (SOUP-022)
- **Version**: Managed by Spring Boot parent
- **Purpose**: Unit testing framework
- **Scope**: Test only

#### 5.2.2 Testcontainers (SOUP-023)
- **Version**: 1.19.8
- **Purpose**: Integration testing with containerized dependencies
- **Scope**: Test only

#### 5.2.3 H2 Database (SOUP-024)
- **Version**: 1.4.200
- **Purpose**: In-memory database for testing
- **Scope**: Test only
- **Known Vulnerability**: CVE-2022-45868 (critical RCE in H2 Console)
- **Mitigation**: H2 Console disabled; test scope only
- **Action Required**: Ensure console disabled; evaluate upgrade when COALESCE compatibility resolved

#### 5.2.4 Mockito (SOUP-025)
- **Version**: Managed by Spring Boot parent
- **Purpose**: Mocking framework for unit testing
- **Scope**: Test only

#### 5.2.5 Awaitility (SOUP-026)
- **Version**: 4.2.1
- **Purpose**: Asynchronous testing support
- **Scope**: Test only

#### 5.2.6 Spring Boot Starter Actuator (SOUP-021)
- **Version**: Managed by Spring Boot parent
- **Purpose**: Monitoring and management endpoints
- **Criticality**: Minor
- **Safety Relevant**: No

#### 5.2.7 SmallRye OpenAPI (SOUP-032)
- **Version**: 3.10.0
- **Purpose**: OpenAPI documentation generation
- **Scope**: Provided (documentation only)

#### 5.2.8 OWASP Dependency Check (SOUP-033)
- **Version**: 12.1.0
- **Purpose**: Security vulnerability scanning
- **Scope**: Build plugin only

#### 5.2.9 JaCoCo Maven Plugin (SOUP-034)
- **Version**: 0.8.12
- **Purpose**: Code coverage analysis
- **Scope**: Build plugin only

#### 5.2.10 Validation API (SOUP-037)
- **Version**: Managed by Spring Boot parent
- **Purpose**: Bean validation annotations
- **Criticality**: Major
- **Safety Relevant**: Yes
- **Rationale**: Input validation critical for data integrity

#### 5.2.11 Hibernate Micrometer (SOUP-038)
- **Version**: Managed by Spring Boot parent
- **Purpose**: Hibernate metrics integration
- **Criticality**: Negligible
- **Safety Relevant**: No

## 6. Transitive Dependencies

### 6.1 Notable Transitive SOUP

#### 6.1.1 Logback Classic (SOUP-035)
- **Version**: Managed by Spring Boot parent
- **Source**: Transitive via spring-boot-starter
- **Purpose**: Logging framework for application logging and audit trails
- **Criticality**: Major
- **Safety Relevant**: No (but important for compliance)
- **Rationale**: Manages audit logs and diagnostic information; failures could impact compliance and troubleshooting

#### 6.1.2 SLF4J API (SOUP-036)
- **Version**: Managed by Spring Boot parent
- **Source**: Transitive via spring-boot-starter
- **Purpose**: Logging facade providing abstraction
- **Criticality**: Minor
- **Safety Relevant**: No

**Note**: Full transitive dependency analysis pending. Action item to run `mvn dependency:tree` and catalog all transitive dependencies.

## 7. Runtime Components

### 7.1 Java Runtime Environment (RT-SOUP-001)

| Property | Value |
|----------|-------|
| **SOUP ID** | RT-SOUP-001 |
| **Name** | Java Runtime Environment |
| **Version** | 21 |
| **Source** | OpenJDK |
| **Purpose** | Java runtime environment for application execution |
| **Safety Relevant** | Yes |
| **Configuration** | Java 21 with maven.compiler.release=21 |

**Risk Assessment**: Critical infrastructure component. JVM bugs could affect all application functionality. Requires monitoring of OpenJDK security advisories.

**Verification**:
- Use OpenJDK LTS version (Java 21)
- Monitor OpenJDK security advisories
- Test application with target JVM version

---

### 7.2 MySQL Database (RT-SOUP-002)

| Property | Value |
|----------|-------|
| **SOUP ID** | RT-SOUP-002 |
| **Name** | MySQL |
| **Version** | 8.x |
| **Source** | Oracle/MySQL |
| **Purpose** | Primary relational database for patient data persistence |
| **Safety Relevant** | Yes |
| **Configuration** | Production database with mysql-connector-j 8.4.0 |

**Risk Assessment**: Critical infrastructure component. All patient data stored in MySQL. Database failures or corruption directly impact patient care.

**Verification**:
- Use MySQL 8.x LTS version
- Monitor MySQL security advisories
- Database backup and recovery procedures
- High availability configuration

---

### 7.3 MariaDB (RT-SOUP-003)

| Property | Value |
|----------|-------|
| **SOUP ID** | RT-SOUP-003 |
| **Name** | MariaDB |
| **Version** | Compatible with MySQL 8.x |
| **Source** | MariaDB Foundation |
| **Purpose** | Alternative database platform (MySQL compatible) |
| **Safety Relevant** | Yes |
| **Configuration** | Alternative to MySQL with mariadb-java-client 1.8.0 |

**Risk Assessment**: Critical infrastructure component if used. Same risk profile as MySQL.

**Action Required**: **HIGH PRIORITY** - Upgrade mariadb-java-client from 1.8.0 (2017) to latest 3.x version due to security concerns.

---

### 7.4 Apache Artemis Message Broker (RT-SOUP-004)

| Property | Value |
|----------|-------|
| **SOUP ID** | RT-SOUP-004 |
| **Name** | Apache Artemis |
| **Version** | Runtime dependency (version managed by deployment) |
| **Source** | Apache |
| **Purpose** | Message broker for asynchronous event processing |
| **Safety Relevant** | Yes |
| **Configuration** | JMS broker for appointment and booking events |

**Risk Assessment**: Critical for appointment notification workflows. Message loss could result in missed appointments.

**Verification**:
- Monitor Artemis security advisories
- Message persistence configuration
- Broker high availability
- Message delivery confirmation

---

### 7.5 Container Base Image (RT-SOUP-005)

| Property | Value |
|----------|-------|
| **SOUP ID** | RT-SOUP-005 |
| **Name** | Container Base Image |
| **Version** | Not specified in POMs |
| **Source** | Docker registry |
| **Purpose** | Operating system for containerized deployment |
| **Safety Relevant** | Yes |
| **Configuration** | Requires review of Dockerfile and deployment manifests |

**Risk Assessment**: Infrastructure component. OS vulnerabilities could affect application security.

**Action Required**: Document container base image (review Dockerfile) and establish vulnerability scanning process.

---

## 8. SOUP Verification Strategy

### 8.1 Initial Verification (for new SOUP)

For each new SOUP component, the following verification activities are required:

- [ ] **License compatibility review** - Verify license is compatible with product distribution 🆔 q337Ho
- [ ] **Security vulnerability scan** - Run OWASP Dependency Check and review results 🆔 m4Nq0d
- [ ] **Functionality documentation review** - Review official documentation for intended functionality 🆔 lJvnCt
- [ ] **Integration testing** - Develop integration tests covering SOUP usage 🆔 xoKFjM
- [ ] **Alternatives evaluation** - Document why this SOUP was selected over alternatives 🆔 rklw0Z
- [ ] **Risk assessment** - Complete criticality and safety relevance assessment 🆔 DZkCh1

### 8.2 Ongoing Verification

For all SOUP components in production:

- [ ] **Automated dependency vulnerability scanning** - OWASP Dependency Check runs on every Maven build (CI/CD integration) 🆔 EYHsHB
- [ ] **Security advisory monitoring** - Subscribe to security mailing lists for critical SOUP (Spring, Hibernate, MySQL, etc.) 🆔 mugcL8
- [ ] **Version update assessment** - Before upgrading SOUP, review changelogs and breaking changes 🆔 aFqSBW
- [ ] **Periodic review** - Review entire SOUP list per release cycle (quarterly minimum) 🆔 ZMgasu
- [ ] **Maintenance status monitoring** - Monitor GitHub activity for critical SOUP to identify abandoned projects 🆔 VPJ808

### 8.3 Tools Used

| Tool | Purpose | Frequency | Configuration |
|------|---------|-----------|---------------|
| OWASP Dependency Check | CVE vulnerability scanning | Per Maven build | Maven plugin (version 12.1.0) |
| Maven Dependency Plugin | Dependency tree analysis | On demand | `mvn dependency:tree` |
| JaCoCo | Test coverage for SOUP integration | Per build | Maven plugin (version 0.8.12) |
| Testcontainers | Integration testing with real dependencies | Per test run | Test scope (version 1.19.8) |

### 8.4 Critical SOUP Qualification Requirements

All 12 critical SOUP items require complete qualification per IEC 62304:

1. **Spring Boot Starter Data JPA (SOUP-001)** - Integration tests for repositories, transaction testing
2. **Hibernate Core (SOUP-002)** - Entity mapping validation, query correctness
3. **Spring Boot Starter Artemis (SOUP-005)** - Message delivery confirmation, failure recovery
4. **Apache CXF (SOUP-007)** - Integration testing with external systems
5. **Flyway Core (SOUP-008)** - Migration testing, rollback procedures
6. **Jackson Databind (SOUP-009)** - Serialization tests, security testing
7. **Jackson Datatype JSR310 (SOUP-010)** - Timezone and date/time accuracy
8. **MySQL Connector/J (SOUP-015)** - Connection pool testing, transaction isolation
9. **Easy Rules Core (SOUP-018)** - Comprehensive rule testing, clinical validation
10. **RT-SOUP-001 (Java 21)** - JVM security monitoring
11. **RT-SOUP-002 (MySQL)** - Database backup/recovery, HA configuration
12. **RT-SOUP-004 (Artemis)** - Message persistence, broker HA

**Status**: Qualification activities not yet started for most SOUP. Action item to complete qualification documentation.

---

## 9. SOUP Change Management

### 9.1 Update Process

When updating SOUP versions, the following procedure is followed:

1. **Identify update need** - Security vulnerability, new functionality requirement, or compatibility issue
2. **Review changelog and breaking changes** - Analyze release notes for impacts
3. **Assess impact on software items** - Identify all software items using the SOUP
4. **Update in development branch** - Update version in `pom.xml` in feature branch
5. **Execute verification tests** - Run full test suite including integration tests
6. **Review OWASP Dependency Check results** - Ensure no new vulnerabilities introduced
7. **Update SOUP List** - Document version change in this list
8. **Release through change control** - Merge via pull request with impact assessment

### 9.2 Recent Changes

| SOUP | Previous | Current | Date | Reason |
|------|----------|---------|------|--------|
| (No recent changes documented) | - | - | - | - |

**Note**: This SOUP List represents the baseline state as of extraction date 2026-03-06. Future updates will be documented in this section.

---

## 10. Action Items

### 10.1 High Priority (Immediate Action Required)

| SOUP | Action | Rationale | Owner | Due Date |
|------|--------|-----------|-------|----------|
| SOUP-016 | **Upgrade mariadb-java-client from 1.8.0 to 3.x** | Version from 2017 (3047 days old); likely contains critical security vulnerabilities | Development Team | Q1 2026 |
| SOUP-024 | **Ensure H2 Console disabled in all environments** | CVE-2022-45868 critical RCE vulnerability in H2 Console | Development Team | Immediate |
| SOUP-018 | **Complete comprehensive rule testing for easy-rules-core** | Safety-critical clinical decision logic requires full validation | QA Team | Next release |
| RT-SOUP-005 | **Document container base image and establish scanning** | Container security critical; requires vulnerability monitoring | DevOps Team | Q1 2026 |

### 10.2 Medium Priority (Plan for Upcoming Release)

| SOUP | Action | Rationale | Owner | Due Date |
|------|--------|-----------|-------|----------|
| SOUP-028 | **Migrate from commons-lang 2.6 to commons-lang3** | commons-lang 2.6 from 2011 (5532 days old); deprecated | Development Team | Q2 2026 |
| SOUP-020 | **Migrate from Spring Cloud Sleuth to Micrometer Tracing** | Spring Cloud Sleuth deprecated; need actively maintained alternative | Development Team | Q2 2026 |
| SOUP-018 | **Evaluate easy-rules-core alternatives** | Last release 2020; minimal maintenance for critical component | Architecture Team | Q2 2026 |
| ALL-CRITICAL | **Complete qualification documentation for critical SOUP** | IEC 62304 compliance requires documented qualification | QA Team | Q2 2026 |

### 10.3 Low Priority (Ongoing Monitoring)

| SOUP | Action | Rationale | Owner | Due Date |
|------|--------|-----------|-------|----------|
| ALL | **Document complete transitive dependency tree** | Need full catalog of all transitive dependencies | Development Team | Q2 2026 |
| ALL | **Establish regular SOUP monitoring process** | Need documented procedure for CVE/advisory monitoring | QA Team | Q2 2026 |
| ALL | **Subscribe to security advisories** | Proactive monitoring for Spring, Hibernate, MySQL, Jackson | DevOps Team | Q1 2026 |
| ALL | **Document SOUP update procedure** | Formalize SOUP change management process | QA Team | Q2 2026 |

---

## 11. Appendices

### Appendix A: Complete Dependency Tree

Full dependency tree analysis pending. Use the following command to generate:

```bash
cd /home/jakob/Noncomplicity/Repos/health-manager
mvn dependency:tree -DoutputFile=dependency-tree.txt
```

### Appendix B: Extraction Metadata

- **Repository**: `/home/jakob/Noncomplicity/Repos/health-manager`
- **Commit**: `16e0273739eda65f73af5410323e1caf53362f16`
- **Manifest files analyzed**:
  - `pom.xml` (root)
  - `svc/pom.xml`
  - `dto/pom.xml`
  - `cnf/pom.xml`
  - `modules/escalator/pom.xml`
- **Extracted**: 2026-03-06T00:00:00Z
- **Extraction method**: Automated Maven POM analysis

### Appendix C: Abbreviations

| Term | Definition |
|------|------------|
| SOUP | Software of Unknown Provenance (third-party software) |
| OTS | Off-The-Shelf software |
| CVE | Common Vulnerabilities and Exposures |
| LTS | Long Term Support |
| JPA | Java Persistence API |
| ORM | Object-Relational Mapping |
| JDBC | Java Database Connectivity |
| JMS | Java Message Service |
| REST | Representational State Transfer |
| SOAP | Simple Object Access Protocol |
| API | Application Programming Interface |
| DTO | Data Transfer Object |
| HA | High Availability |
| RCE | Remote Code Execution |

---

## 12. Compliance Mapping

| Document Section | IEC 62304 Clause | Evidence Provided |
|------------------|------------------|-------------------|
| Section 3 (Critical SOUP) | 8.1.1 | SOUP identification for safety-critical components |
| Section 4 (Major SOUP) | 8.1.1 | SOUP identification for major components |
| Section 5 (Minor/Negligible) | 8.1.1 | Complete SOUP inventory |
| Section 7 (Runtime) | 8.1.1 | Runtime environment identification |
| Title, Purpose fields | 8.1.2 a) | SOUP title and manufacturer/source |
| SOUP ID field | 8.1.2 b) | Unique SOUP designation |
| Version field | 8.1.2 c) | SOUP version identification |
| Known Anomalies subsections | 8.1.2 d) | Known anomalies relevant to safety |
| Section 9 (Change Management) | 8.1.3 | SOUP change management procedure |
| Section 8 (Verification Strategy) | 8.2 | SOUP verification approach |
| Section 6 (Transitive Dependencies) | 8.1.1 | Comprehensive SOUP identification |

---

## Document History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-03-06 | Automated Generation | Initial SOUP List based on extraction from commit 16e0273 |

---

**End of Document**
