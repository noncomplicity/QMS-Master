---
id:
title: "Software Architecture Description - health-manager"
version:
author:
effective_date:
type: "Specification"
document_id: "SAD-health-manager-1.0"
software_safety_class: "B"
process: "[Software Development Process](../../../Canvases/Software%20Development%20Process.canvas)"
requirements: "[Software Requirements Specification](SRS.md)"
owner: "[Head of Software Development](../../../Assets/Head%20of%20Software%20Development.md)"
---

# Software Architecture Description

## 1. Introduction

### 1.1 Purpose
This document describes the software architecture of the Health Manager Service, defining the decomposition into software items and their relationships per IEC 62304:2006+A1:2015 clause 5.3. This architecture supports the design, implementation, verification, and maintenance activities for the medical device software system.

### 1.2 Scope
This Software Architecture Description covers the complete Health Manager Service microservice, a Spring Boot application providing medical appointment management, clinical consultation workflows, practitioner scheduling, patient interactions, and integration with external healthcare systems. The architecture encompasses:

- REST API layers (v1, v2, v3)
- Service layer business logic
- Domain model and persistence layer
- External service integration clients
- Event messaging infrastructure
- Security and authorization components
- Configuration and deployment architecture

### 1.3 Software Safety Classification
This software system is classified as **Class B** per IEC 62304:2006+A1:2015.

**Rationale:** The Health Manager Service manages medical appointments, clinical consultations, prescriptions, and medical records. While not directly controlling life-sustaining functions, failures or errors could result in:
- Delayed or missed medical care due to incorrect appointment scheduling
- Wrong practitioner assignment affecting treatment quality
- Lost or corrupted medical records impacting clinical decisions
- Incorrect prescription data affecting medication management
- Privacy breaches of sensitive health information

These potential hazards constitute possible INJURY to patients as defined by IEC 62304, therefore Class B classification is appropriate.

### 1.4 Architectural Overview
The Health Manager Service employs a layered microservice architecture following Domain-Driven Design principles:

**Architectural Pattern:** Layered architecture with clear separation of concerns
- **Presentation Layer:** RESTful HTTP API controllers with OpenAPI documentation
- **Service Layer:** Business logic orchestration and workflow management
- **Domain Layer:** Core entity model with JPA/Hibernate persistence
- **Integration Layer:** External service clients and event publishers
- **Security Layer:** Spring Security with JWT authentication and role-based authorization
- **Infrastructure Layer:** Configuration, caching, monitoring, and cross-cutting concerns

**Key Architectural Decisions:**
1. **Multi-version API support:** v1, v2, v3 endpoints enable backward compatibility during system evolution
2. **Event-driven integration:** JMS messaging via Apache Artemis for asynchronous event publication to external systems
3. **CQRS-inspired patterns:** Separate query paths (QueryDSL) for complex clinic list views from command operations
4. **State machine pattern:** Appointment handover implemented as explicit state machine with validation and audit trail
5. **Repository pattern:** JPA repositories provide abstraction over database operations
6. **Feature flags:** Configuration-driven feature enablement for safe deployment of new functionality

**Technology Stack:**
- Runtime: Java 21 (OpenJDK)
- Framework: Spring Boot (managed by parent POM 2.30.0)
- Persistence: Spring Data JPA with Hibernate ORM
- Database: MySQL/MariaDB with Flyway migrations
- Messaging: JMS (Apache Artemis)
- Query DSL: QueryDSL and Blaze Persistence for complex queries
- Build: Maven with multi-module structure
- API Documentation: OpenAPI 3.x via SmallRye

### 1.5 Referenced Documents
| Document | Relationship |
|----------|--------------|
| Software Requirements Specification (SRS) | Requirements this architecture implements |
| Risk Management File | Safety requirements driving architecture |
| SOUP List | Third-party component catalog |
| Handover Architecture Documentation | Detailed design for appointment handover feature |
| Software Requirements Document | Business requirements and use cases |

## 2. Software Item Hierarchy

### 2.1 System Decomposition

```
Health Manager Service (SOFTWARE SYSTEM) [SI-01]
├── REST API Layer (SUBSYSTEM) [SI-01-01]
│   ├── v1 Controllers (MODULE) [SI-01-01-01]
│   │   ├── AppointmentController (UNIT) [SI-01-01-01-01]
│   │   ├── ClinicAppointmentController (UNIT) [SI-01-01-01-02]
│   │   ├── PatientAppointmentController (UNIT) [SI-01-01-01-03]
│   │   ├── AppointmentNoteController (UNIT) [SI-01-01-01-04]
│   │   ├── PractitionerController (UNIT) [SI-01-01-01-05]
│   │   ├── EpisodeOfCareController (UNIT) [SI-01-01-01-06]
│   │   ├── ReferralController (UNIT) [SI-01-01-01-07]
│   │   └── BillingListController (UNIT) [SI-01-01-01-08]
│   ├── v2 Controllers (MODULE) [SI-01-01-02]
│   ├── v3 Controllers (MODULE) [SI-01-01-03]
│   └── System Controllers (MODULE) [SI-01-01-04]
├── Service Layer (SUBSYSTEM) [SI-01-02]
│   ├── AppointmentHandoverService (MODULE) [SI-01-02-01]
│   ├── UpdateAppointmentService (MODULE) [SI-01-02-02]
│   ├── PatientAppointmentService (MODULE) [SI-01-02-03]
│   ├── RequestAppointmentV3Service (MODULE) [SI-01-02-04]
│   ├── ConsultationService (MODULE) [SI-01-02-05]
│   ├── ClinicAppointmentListService (MODULE) [SI-01-02-06]
│   ├── AppointmentNoteService (MODULE) [SI-01-02-07]
│   ├── PractitionerService (MODULE) [SI-01-02-08]
│   ├── EpisodeOfCareService (MODULE) [SI-01-02-09]
│   ├── ReferralService (MODULE) [SI-01-02-10]
│   ├── BillingListReportGenerator (MODULE) [SI-01-02-11]
│   └── DataErasureService (MODULE) [SI-01-02-12]
├── Domain & Persistence Layer (SUBSYSTEM) [SI-01-03]
│   ├── Domain Entities (MODULE) [SI-01-03-01]
│   │   ├── Appointment (UNIT)
│   │   ├── AppointmentHandoverProcess (UNIT)
│   │   ├── AppointmentHandoverEvent (UNIT)
│   │   ├── Consultation (UNIT)
│   │   ├── EpisodeOfCare (UNIT)
│   │   ├── AppointmentNote (UNIT)
│   │   ├── Practitioner (UNIT)
│   │   └── Shift (UNIT)
│   ├── Repository Interfaces (MODULE) [SI-01-03-02]
│   │   ├── AppointmentRepository (UNIT)
│   │   ├── AppointmentHandoverProcessRepository (UNIT)
│   │   ├── AppointmentHandoverEventRepository (UNIT)
│   │   ├── ConsultationRepository (UNIT)
│   │   └── QueryableAppointmentRepository (UNIT)
│   └── Database Migration Scripts (MODULE) [SI-01-03-03]
├── Integration Layer (SUBSYSTEM) [SI-01-04]
│   ├── External Service Clients (MODULE) [SI-01-04-01]
│   │   ├── DirectoryService (UNIT)
│   │   ├── NotifierClient (UNIT)
│   │   ├── MeetingPlaceService (UNIT)
│   │   ├── BookingsService (UNIT)
│   │   └── InterviewerService (UNIT)
│   └── Event Publishers (MODULE) [SI-01-04-02]
│       ├── AppointmentEventSender (UNIT)
│       └── JMS Topic Publishers (UNIT)
├── Security Layer (SUBSYSTEM) [SI-01-05]
│   ├── Authentication (MODULE) [SI-01-05-01]
│   │   └── JWT Authentication Handler (UNIT)
│   ├── Authorization (MODULE) [SI-01-05-02]
│   │   ├── AppointmentAuthorizer (UNIT)
│   │   └── RoleBasedAccessControl (UNIT)
│   └── Audit Logging (MODULE) [SI-01-05-03]
│       └── AuditEventRecorder (UNIT)
├── Configuration Layer (SUBSYSTEM) [SI-01-06]
│   ├── CustomizationService (MODULE) [SI-01-06-01]
│   ├── CareUnitRegistry (MODULE) [SI-01-06-02]
│   ├── OriginRegistry (MODULE) [SI-01-06-03]
│   └── PartnerRegistry (MODULE) [SI-01-06-04]
├── Cross-Cutting Concerns (SUBSYSTEM) [SI-01-07]
│   ├── Caching (MODULE) [SI-01-07-01]
│   ├── Monitoring & Metrics (MODULE) [SI-01-07-02]
│   ├── Distributed Tracing (MODULE) [SI-01-07-03]
│   └── Scheduled Tasks (MODULE) [SI-01-07-04]
└── Data Transfer Objects (SUBSYSTEM) [SI-01-08]
    └── DTO Module (MODULE) [SI-01-08-01]
        └── API Contract DTOs (UNIT)
```

### 2.2 Item Classification Summary

| Item ID | Name | Level | Safety Class | Rationale |
|---------|------|-------|--------------|-----------|
| SI-01 | Health Manager Service | System | B | Complete medical appointment management system |
| SI-01-01 | REST API Layer | Subsystem | B | API failures could prevent appointment access |
| SI-01-01-01 | v1 Controllers | Module | B | Primary API version handling core operations |
| SI-01-01-01-01 | AppointmentController | Unit | B | Controls appointment CRUD and handover operations |
| SI-01-01-01-02 | ClinicAppointmentController | Unit | B | Practitioner appointment list queries |
| SI-01-01-01-03 | PatientAppointmentController | Unit | B | Patient appointment access |
| SI-01-01-01-04 | AppointmentNoteController | Unit | B | Clinical note management |
| SI-01-01-01-05 | PractitionerController | Unit | B | Practitioner status and profile management |
| SI-01-01-01-06 | EpisodeOfCareController | Unit | B | Episode of care management |
| SI-01-01-01-07 | ReferralController | Unit | B | Medical referral processing |
| SI-01-01-01-08 | BillingListController | Unit | A | Billing reports (administrative, no direct clinical impact) |
| SI-01-02 | Service Layer | Subsystem | B | Business logic orchestration |
| SI-01-02-01 | AppointmentHandoverService | Module | B | Handover state machine (critical for care continuity) |
| SI-01-02-02 | UpdateAppointmentService | Module | B | Appointment update orchestration |
| SI-01-02-03 | PatientAppointmentService | Module | B | Patient appointment retrieval with privacy controls |
| SI-01-02-04 | RequestAppointmentV3Service | Module | B | Appointment request processing |
| SI-01-02-05 | ConsultationService | Module | B | Clinical consultation management |
| SI-01-02-06 | ClinicAppointmentListService | Module | B | Complex query building for clinic views |
| SI-01-02-07 | AppointmentNoteService | Module | B | Clinical note CRUD operations |
| SI-01-02-08 | PractitionerService | Module | B | Practitioner management |
| SI-01-02-09 | EpisodeOfCareService | Module | B | Episode management |
| SI-01-02-10 | ReferralService | Module | B | Referral processing |
| SI-01-02-11 | BillingListReportGenerator | Module | A | Report generation (administrative) |
| SI-01-02-12 | DataErasureService | Module | B | GDPR compliance (data integrity critical) |
| SI-01-03 | Domain & Persistence Layer | Subsystem | B | Data integrity foundation |
| SI-01-03-01 | Domain Entities | Module | B | Core data model with JPA mapping |
| SI-01-03-02 | Repository Interfaces | Module | B | Database abstraction layer |
| SI-01-03-03 | Database Migration Scripts | Module | B | Schema evolution (data integrity risk) |
| SI-01-04 | Integration Layer | Subsystem | B | External system integration |
| SI-01-04-01 | External Service Clients | Module | B | HTTP clients for external services |
| SI-01-04-02 | Event Publishers | Module | B | JMS event publication |
| SI-01-05 | Security Layer | Subsystem | B | Authentication and authorization |
| SI-01-05-01 | Authentication | Module | B | JWT authentication |
| SI-01-05-02 | Authorization | Module | B | Fine-grained access control |
| SI-01-05-03 | Audit Logging | Module | B | Compliance audit trail |
| SI-01-06 | Configuration Layer | Subsystem | B | Runtime configuration management |
| SI-01-07 | Cross-Cutting Concerns | Subsystem | A | Performance and monitoring (non-safety) |
| SI-01-08 | Data Transfer Objects | Subsystem | B | API contracts defining data exchange |

## 3. Software Item Specifications

### 3.1 Health Manager Service (SI-01)

#### 3.1.1 Identification
| Property | Value |
|----------|-------|
| Item ID | SI-01 |
| Path | `svc/src/main/java/se/alerisx/mhp/manager` |
| Level | System |
| Safety Class | B |

#### 3.1.2 Purpose
Complete medical appointment and healthcare management microservice providing core clinical workflow functionality for a telemedicine platform.

#### 3.1.3 Responsibilities
- Manage patient appointments from creation through completion
- Orchestrate practitioner-to-practitioner handover workflows
- Track clinical consultations and episode of care
- Manage medical notes, prescriptions, and referrals
- Coordinate with external healthcare systems via REST and JMS
- Enforce role-based access control and audit logging
- Provide RESTful HTTP APIs for web and mobile clients
- Maintain data integrity through transactional operations

#### 3.1.4 Interfaces Provided

| Interface | Type | Consumers | Description |
|-----------|------|-----------|-------------|
| IF-001 | REST API | Web clients, mobile apps | v1 REST API endpoints |
| IF-002 | REST API | Web clients, mobile apps | v2 REST API endpoints |
| IF-003 | REST API | Web clients, mobile apps | v3 REST API endpoints |
| IF-004 | REST API | Admin clients | System administration endpoints |

#### 3.1.5 Interfaces Consumed

| Interface | Provider | Purpose |
|-----------|----------|---------|
| IF-010 | Directory Service | Patient/practitioner lookups |
| IF-011 | Notifier Service | Push notifications |
| IF-012 | MeetingPlace Service | Patient-practitioner chat |
| IF-013 | Bookings Service | Appointment scheduling |
| IF-014 | Interviewer Service | Medical questionnaires |
| IF-015 | Configuration Service | System configuration |
| IF-016 | MySQL/MariaDB | Data persistence |
| IF-017 | Apache Artemis | JMS messaging |
| IF-018 | JWT Issuer | Authentication tokens |
| IF-019 | Prometheus | Metrics collection |

#### 3.1.6 Dependencies

**Internal Dependencies:** None (top-level system)

**External Dependencies (SOUP):**
| SOUP | Version | Usage |
|------|---------|-------|
| Spring Boot | 2.30.0 (parent) | Application framework |
| Hibernate | Managed | ORM for JPA entities |
| MySQL Connector | 8.4.0 | JDBC driver |
| MariaDB Connector | 1.8.0 | Alternative JDBC driver |
| Apache Artemis | Managed | JMS messaging |
| Jackson | Managed | JSON serialization |
| QueryDSL | Managed | Type-safe queries |
| Flyway | Managed | Database migrations |

#### 3.1.7 Safety Considerations
**Safety Function:** Manages scheduling and tracking of medical appointments, clinical consultations, and practitioner assignments affecting patient care delivery.

**Failure Modes:**
- Appointment data loss or corruption
- Incorrect practitioner assignment
- Lost handover notifications
- Failed external system integration
- Unauthorized data access
- Stale cached data

**Mitigation:**
- Database transactions with ACID guarantees
- Optimistic locking for concurrent updates
- Immutable audit event trail
- Retry mechanisms for external calls
- Fine-grained authorization checks
- Cache TTLs to prevent stale data
- Comprehensive input validation

#### 3.1.8 Requirements Implemented
| Requirement | Description |
|-------------|-------------|
| SRS-APPT-001 | Create Appointment |
| SRS-APPT-002 | Retrieve Appointment by ID |
| SRS-APPT-003 | Update Appointment Status |
| SRS-APPT-004 | Appointment State Tracking with Timestamps |
| SRS-HAND-001 to SRS-HAND-005 | Complete handover workflow |
| SRS-CONS-001 to SRS-CONS-002 | Consultation management |
| SRS-SEC-001 to SRS-SEC-004 | Security requirements |
| SRS-INT-001 to SRS-INT-006 | External integrations |
| SRS-DATA-001 to SRS-DATA-005 | Data persistence |

---

### 3.2 REST API Layer (SI-01-01)

#### 3.2.1 Identification
| Property | Value |
|----------|-------|
| Item ID | SI-01-01 |
| Path | `svc/src/main/java/se/alerisx/mhp/manager/controller` |
| Level | Subsystem |
| Safety Class | B |

#### 3.2.2 Purpose
Expose RESTful HTTP endpoints for appointment management, clinical workflows, and administrative operations across multiple API versions (v1, v2, v3).

#### 3.2.3 Responsibilities
- Accept and validate HTTP requests
- Deserialize JSON request bodies to DTOs
- Delegate business logic to service layer
- Serialize response DTOs to JSON
- Apply security annotations (@Secured, @PreAuthorize)
- Apply audit annotations (@Audit)
- Generate OpenAPI documentation
- Handle API versioning and backward compatibility
- Return appropriate HTTP status codes and error responses

#### 3.2.4 Interfaces Provided

| Interface | Type | Consumers | Description |
|-----------|------|-----------|-------------|
| IF-001 | REST/HTTP | External clients | v1 API endpoints at /v1/ |
| IF-002 | REST/HTTP | External clients | v2 API endpoints at /v2/ |
| IF-003 | REST/HTTP | External clients | v3 API endpoints at /v3/ |
| IF-004 | REST/HTTP | Admin clients | System endpoints at /v1/system/ |

**Interface Definition: IF-001 (v1 API)**
```java
@RestController
@RequestMapping("/v1/appointments")
@Tag(name = "Appointment API V1")
public class AppointmentController {

    @PostMapping
    @Secured({"ROLE_PRACTITIONER", "ROLE_PATIENT", "ROLE_SYSTEM"})
    ResponseEntity<AppointmentDTO> createAppointment(@RequestBody CreateAppointmentDTO dto);

    @GetMapping("/{id}")
    @PreAuthorize("@appointmentAuthorizer.authorize(#id, principal)")
    @Audit(throttle = ThrottleStrategy.HOURLY)
    ResponseEntity<AppointmentDTO> getAppointment(@PathVariable String id);

    @PutMapping("/{id}")
    @Secured({"ROLE_PRACTITIONER", "ROLE_SYSTEM"})
    ResponseEntity<AppointmentDTO> updateAppointment(
        @PathVariable String id,
        @RequestBody UpdateAppointmentDTO dto
    );

    @PostMapping("/{id}/handover")
    @Secured("ROLE_PRACTITIONER")
    ResponseEntity<AppointmentDTO> handoverAppointment(
        @PathVariable String id,
        @RequestBody HandoverRequestDTO dto
    );
}
```

#### 3.2.5 Interfaces Consumed

| Interface | Provider | Purpose |
|-----------|----------|---------|
| Service interfaces | SI-01-02 | Business logic delegation |

#### 3.2.6 Dependencies

**Internal Dependencies:**
| Dependency | Coupling | Rationale |
|------------|----------|-----------|
| SI-01-02 | Loose | Controllers delegate to services via interfaces |
| SI-01-08 | Loose | DTOs define API contracts |

**External Dependencies (SOUP):**
| SOUP | Version | Usage |
|------|---------|-------|
| Spring MVC | Managed | @RestController, @RequestMapping |
| Jackson | Managed | JSON serialization/deserialization |
| Spring Security | Managed | @Secured, @PreAuthorize annotations |
| OpenAPI | 3.10.0 | @Tag, @Operation annotations |

#### 3.2.7 Safety Considerations
**Safety Function:** Gateway for all external access to appointment and clinical data; input validation critical to prevent invalid data entry.

**Failure Modes:**
- Malformed input bypassing validation
- Unauthorized access to endpoints
- Serialization errors corrupting data
- API version incompatibility

**Mitigation:**
- Bean validation annotations (@Valid, @NotNull, etc.)
- Spring Security enforcement before controller invocation
- Jackson configured with fail-on-unknown-properties
- API versioning allows gradual migration
- Global exception handler for consistent error responses

#### 3.2.8 Requirements Implemented
| Requirement | Description |
|-------------|-------------|
| SRS-UI-001 | RESTful API Endpoints |
| SRS-UI-002 | OpenAPI Documentation |
| SRS-SEC-001 | Role-Based Access Control |
| SRS-SEC-004 | Audit Logging with Throttling |

---

### 3.3 AppointmentHandoverService (SI-01-02-01)

#### 3.3.1 Identification
| Property | Value |
|----------|-------|
| Item ID | SI-01-02-01 |
| Path | `svc/src/main/java/se/alerisx/mhp/manager/service/impl/AppointmentHandoverServiceImpl.java` |
| Level | Module |
| Safety Class | B |

#### 3.3.2 Purpose
Implement appointment handover state machine enabling practitioner-to-practitioner transfer of clinical responsibility with validation, notifications, and audit trail.

#### 3.3.3 Responsibilities
- Validate handover preconditions (feature flags, care unit eligibility, practitioner validity)
- Enforce handover state machine transitions (ENABLED → PROPOSED → ACCEPTED/DENIED/CANCELLED)
- Persist handover process and event records with optimistic locking
- Send notifications to involved practitioners via NotifierClient
- Post handover messages to patient-practitioner meetings
- Publish JMS events to appointmentHandoverEventTopic
- Create immutable audit events for regulatory compliance
- Perform cross-care-provider validation when feature enabled
- Update appointment practitioner assignment on acceptance
- Validate practitioner IDs against Directory Service

#### 3.3.4 Interfaces Provided

| Interface | Type | Consumers | Description |
|-----------|------|-----------|-------------|
| AppointmentHandoverService | Java Interface | UpdateAppointmentService | Handover operations |

**Interface Definition:**
```java
public interface AppointmentHandoverService {
    void proposeHandover(String appointmentId, HandoverRequest request);
    void acceptHandover(String appointmentId, String practitionerId);
    void denyHandover(String appointmentId, String practitionerId);
    void cancelHandover(String appointmentId, String practitionerId);
}
```

#### 3.3.5 Interfaces Consumed

| Interface | Provider | Purpose |
|-----------|----------|---------|
| AppointmentRepository | SI-01-03-02 | Load/save appointment entities |
| AppointmentHandoverProcessRepository | SI-01-03-02 | Load/save handover process |
| AppointmentHandoverEventRepository | SI-01-03-02 | Persist audit events |
| DirectoryService | SI-01-04-01 | Validate practitioner IDs |
| NotifierClient | SI-01-04-01 | Send push notifications |
| MeetingPlaceService | SI-01-04-01 | Post chat messages |
| AppointmentEventSender | SI-01-04-02 | Publish JMS events |
| CustomizationService | SI-01-06-01 | Check feature flags |
| CareUnitRegistry | SI-01-06-02 | Validate care units |

#### 3.3.6 Dependencies

**Internal Dependencies:**
| Dependency | Coupling | Rationale |
|------------|----------|-----------|
| SI-01-03-02 | Loose | Repository pattern abstracts persistence |
| SI-01-04-01 | Loose | Service clients behind interfaces |
| SI-01-04-02 | Loose | Event sender abstraction |
| SI-01-06 | Loose | Configuration services |

**External Dependencies (SOUP):**
| SOUP | Version | Usage |
|------|---------|-------|
| Spring Data JPA | Managed | @Transactional annotations |
| Hibernate | Managed | Optimistic locking (@Version) |

#### 3.3.7 Safety Considerations
**Safety Function:** Manages transfer of clinical responsibility between practitioners; failures could create ambiguous accountability for patient care.

**Failure Modes:**
- Invalid state transitions creating orphaned handovers
- Notification failures leaving practitioners unaware
- Lost audit events compromising compliance
- Race conditions in concurrent handover operations
- Cross-care-provider handover to unauthorized organization

**Mitigation:**
- State machine validation rejecting invalid transitions
- Retry logic for notification failures (via spring-retry)
- Database transactions ensuring atomicity of handover + event creation
- Optimistic locking preventing concurrent modification
- Feature flag for cross-care-provider transfers
- Practitioner ID validation against Directory Service
- Immutable audit events (append-only)

#### 3.3.8 Requirements Implemented
| Requirement | Description |
|-------------|-------------|
| SRS-HAND-001 | Propose Appointment Handover |
| SRS-HAND-002 | Accept Appointment Handover |
| SRS-HAND-003 | Deny or Cancel Appointment Handover |
| SRS-HAND-004 | Handover State Machine Enforcement |
| SRS-HAND-005 | Handover Authorization Rules |
| SRS-SAF-001 | Handover Cross-Care-Provider Validation |
| SRS-SAF-002 | Practitioner Validation for Handover |
| SRS-SAF-003 | Appointment State Validation for Handover |
| SRS-SAF-004 | Handover Ownership Validation |
| SRS-SAF-005 | Mandatory Practitioner Role for Handover |
| SRS-AUD-002 | Handover Event Audit Logging |

---

### 3.4 Domain & Persistence Layer (SI-01-03)

#### 3.4.1 Identification
| Property | Value |
|----------|-------|
| Item ID | SI-01-03 |
| Path | `svc/src/main/java/se/alerisx/mhp/manager/domain` |
| Level | Subsystem |
| Safety Class | B |

#### 3.4.2 Purpose
Define core domain model entities and provide persistence abstraction through repository pattern using JPA/Hibernate.

#### 3.4.3 Responsibilities
- Define JPA entity mappings for clinical data
- Implement repository interfaces for CRUD operations
- Provide QueryDSL predicates for complex queries
- Manage database schema evolution via Flyway migrations
- Enforce data integrity constraints at entity level
- Support optimistic locking for concurrent updates
- Define database indexes for performance
- Implement custom queries using @Query annotations

#### 3.4.4 Interfaces Provided

| Interface | Type | Consumers | Description |
|-----------|------|-----------|-------------|
| Repository interfaces | Java Interface | Service layer | Data access abstraction |

**Key Entity: Appointment**
```java
@Entity
@Table(name = "appointment")
public class Appointment {
    @Id
    private String id;

    @Version
    private Integer version;

    @Column(name = "patient_id", nullable = false)
    private String patientId;

    @Column(name = "practitioner_id")
    private String practitionerId;

    @Column(name = "care_unit_id", nullable = false)
    private String careUnitId;

    @Enumerated(EnumType.STRING)
    @Column(name = "type", nullable = false)
    private AppointmentType type;

    @Column(name = "notified_at", columnDefinition = "TIMESTAMP(6)")
    private Instant notifiedAt;

    @Column(name = "queued_at", columnDefinition = "TIMESTAMP(6)")
    private Instant queuedAt;

    @Column(name = "started_at", columnDefinition = "TIMESTAMP(6)")
    private Instant startedAt;

    @Column(name = "ended_at", columnDefinition = "TIMESTAMP(6)")
    private Instant endedAt;

    @Column(name = "cancelled_at", columnDefinition = "TIMESTAMP(6)")
    private Instant cancelledAt;

    // ... additional fields
}
```

**Key Entity: AppointmentHandoverProcess**
```java
@Entity
@Table(name = "appointment_handover_process")
public class AppointmentHandoverProcess {
    @Id
    @Column(name = "id", length = 50)
    private String id;

    @Version
    @Column(name = "version")
    private Integer version;

    @Column(name = "appointment_id", length = 50, unique = true, nullable = false)
    private String appointmentId;

    @Column(name = "owner_id", length = 50, nullable = false)
    private String ownerId;

    @Column(name = "owner_role", length = 50, nullable = false)
    private String ownerRole;

    @Enumerated(EnumType.STRING)
    @Column(name = "handover_status", length = 50, nullable = false)
    private HandoverStatus handoverStatus;

    @Column(name = "handover_care_unit_id", length = 50)
    private String handoverCareUnitId;

    @Column(name = "handover_practitioner_id", length = 50)
    private String handoverPractitionerId;

    @Column(name = "handover_notes", length = 1024)
    private String handoverNotes;

    @Column(name = "created_at", columnDefinition = "TIMESTAMP(6)")
    private Instant createdAt;
}
```

**Repository Interface:**
```java
@Repository
public interface AppointmentRepository extends JpaRepository<Appointment, String> {

    @Query("SELECT a FROM Appointment a WHERE a.patientId = :patientId " +
           "AND a.cancelledAt IS NULL ORDER BY a.createdAt DESC")
    List<Appointment> findActiveByPatientId(@Param("patientId") String patientId);

    @Query("SELECT a FROM Appointment a WHERE a.practitionerId = :practitionerId " +
           "AND a.careUnitId = :careUnitId AND a.endedAt IS NULL")
    List<Appointment> findActiveBypractitionerAndCareUnit(
        @Param("practitionerId") String practitionerId,
        @Param("careUnitId") String careUnitId
    );
}
```

#### 3.4.5 Interfaces Consumed

| Interface | Provider | Purpose |
|-----------|----------|---------|
| IF-016 | MySQL/MariaDB | JDBC database connection |

#### 3.4.6 Dependencies

**Internal Dependencies:** None

**External Dependencies (SOUP):**
| SOUP | Version | Usage |
|------|---------|-------|
| Hibernate | Managed | ORM implementation |
| Spring Data JPA | Managed | Repository infrastructure |
| MySQL Connector | 8.4.0 | JDBC driver |
| Flyway | Managed | Schema migrations |
| QueryDSL | Managed | Type-safe queries |

#### 3.4.7 Safety Considerations
**Safety Function:** Persistence layer for all clinical data; data loss or corruption directly impacts patient care.

**Failure Modes:**
- Data loss due to transaction failures
- Concurrent update conflicts
- Schema migration failures
- Database connection exhaustion
- SQL injection via unsafe queries

**Mitigation:**
- ACID transactions via @Transactional
- Optimistic locking with @Version
- Flyway migration validation before startup
- HikariCP connection pooling with limits
- Prepared statements and JPA parameterization
- Database backup and recovery procedures (operational)
- Query timeout enforcement (30s default)

#### 3.4.8 Requirements Implemented
| Requirement | Description |
|-------------|-------------|
| SRS-DATA-001 | MySQL/MariaDB Persistence |
| SRS-DATA-002 | Flyway Database Migration |
| SRS-DATA-003 | Appointment Handover Process Data Schema |
| SRS-DATA-004 | UTF-8 Character Encoding |
| SRS-DATA-005 | Optimistic Locking with Versioning |
| SRS-AUD-001 | Immutable Audit Event Storage |

---

### 3.5 Security Layer (SI-01-05)

#### 3.5.1 Identification
| Property | Value |
|----------|-------|
| Item ID | SI-01-05 |
| Path | `svc/src/main/java/se/alerisx/mhp/manager/security` |
| Level | Subsystem |
| Safety Class | B |

#### 3.5.2 Purpose
Enforce authentication, authorization, and audit logging to protect patient health information and ensure regulatory compliance.

#### 3.5.3 Responsibilities
- Validate JWT tokens from authentication provider
- Extract user identity and roles from JWT claims
- Enforce role-based access control via @Secured annotations
- Implement fine-grained appointment-level authorization
- Cache authorization checks for performance
- Record audit events for sensitive operations
- Throttle audit logs to prevent flooding
- Integrate with consent service for patient data access

#### 3.5.4 Interfaces Provided

| Interface | Type | Consumers | Description |
|-----------|------|-----------|-------------|
| AppointmentAuthorizer | Spring Bean | Spring Security | Fine-grained authorization |
| AuditEventRecorder | Annotation | Controllers | Audit logging |

**Interface Definition: AppointmentAuthorizer**
```java
@Component
public class AppointmentAuthorizer {

    @Cacheable(cacheNames = "authorizationCache", key = "#appointmentId + #principal.name")
    public boolean authorize(String appointmentId, Authentication principal) {
        String username = principal.getName();
        Collection<String> roles = principal.getAuthorities().stream()
            .map(GrantedAuthority::getAuthority)
            .collect(Collectors.toList());

        if (roles.contains("ROLE_SYSTEM") || roles.contains("ROLE_ADMIN")) {
            return true; // System and admin have unrestricted access
        }

        Appointment appointment = appointmentRepository.findById(appointmentId)
            .orElseThrow(() -> new EntityNotFoundException("Appointment not found"));

        if (roles.contains("ROLE_PATIENT")) {
            return appointment.getPatientId().equals(username);
        }

        if (roles.contains("ROLE_PRACTITIONER")) {
            // Check if practitioner is in same care unit or has consent
            return isPractitionerAuthorized(appointment, username);
        }

        if (roles.contains("ROLE_PRACTITIONER_LIMITED_ACCESS")) {
            // Check if patient ID is in 'pids' claim
            List<String> allowedPatients = extractPidsFromToken(principal);
            return allowedPatients.contains(appointment.getPatientId());
        }

        return false;
    }

    private boolean isPractitionerAuthorized(Appointment appt, String practitionerId) {
        // Check care unit membership
        if (careUnitRegistry.isPractitionerInCareUnit(practitionerId, appt.getCareUnitId())) {
            return true;
        }
        // Check consent
        return consentService.hasConsent(practitionerId, appt.getPatientId());
    }
}
```

#### 3.5.5 Interfaces Consumed

| Interface | Provider | Purpose |
|-----------|----------|---------|
| IF-018 | JWT Issuer | Validate authentication tokens |
| CareUnitRegistry | SI-01-06-02 | Check care unit membership |
| ConsentV2Service | External | Check consent grants |

#### 3.5.6 Dependencies

**Internal Dependencies:**
| Dependency | Coupling | Rationale |
|------------|----------|-----------|
| SI-01-03-02 | Loose | Load appointments for authorization |
| SI-01-06 | Loose | Configuration services |

**External Dependencies (SOUP):**
| SOUP | Version | Usage |
|------|---------|-------|
| Spring Security | Managed | @Secured, @PreAuthorize, Authentication |
| Spring Cache | Managed | @Cacheable for authorization cache |

#### 3.5.7 Safety Considerations
**Safety Function:** Protects patient health information from unauthorized access; prevents privacy breaches.

**Failure Modes:**
- Unauthorized data access due to authorization bypass
- Privilege escalation through role manipulation
- Missing audit trail for compliance investigations
- Cache poisoning showing wrong authorization results

**Mitigation:**
- Spring Security framework enforcement before method invocation
- JWT signature validation preventing token tampering
- Role checking at multiple levels (controller + service)
- Cache TTL (configurable) preventing stale authorization decisions
- Immutable audit events stored in database
- Fine-grained authorization checking individual appointments
- Consent integration for cross-care-unit access

#### 3.5.8 Requirements Implemented
| Requirement | Description |
|-------------|-------------|
| SRS-SEC-001 | Role-Based Access Control (RBAC) |
| SRS-SEC-002 | Appointment-Level Authorization |
| SRS-SEC-003 | JWT-Based Authentication |
| SRS-SEC-004 | Audit Logging with Throttling |

---

## 4. SOUP/OTS Components

### 4.1 SOUP Summary

| ID | Name | Version | Purpose | Safety Class | Integration |
|----|------|---------|---------|--------------|-------------|
| SOUP-001 | spring-boot-starter-data-jpa | Managed by parent 2.30.0 | JPA persistence layer | Critical | Direct dependency, used throughout persistence layer |
| SOUP-002 | hibernate-core | Managed | ORM implementation | Critical | Transitive via Spring Data JPA |
| SOUP-003 | querydsl-jpa | Managed | Type-safe queries | Major | Direct dependency in repository layer |
| SOUP-004 | blaze-persistence-core | 1.6.11 | Query optimization | Major | Direct dependency for complex queries |
| SOUP-005 | spring-boot-starter-artemis | Managed | JMS messaging | Critical | Direct dependency for event publishing |
| SOUP-006 | artemis-jms-server | Managed | Embedded broker | Minor | Test scope only |
| SOUP-007 | apache-cxf | 3.6.3 | SOAP web services | Critical | External healthcare system integration |
| SOUP-008 | flyway-core | Managed | Database migrations | Critical | Transitive via Spring Boot |
| SOUP-009 | jackson-databind | Managed | JSON serialization | Critical | REST API data exchange |
| SOUP-010 | jackson-datatype-jsr310 | Managed | Java 8 date/time | Critical | Appointment timestamp handling |
| SOUP-011 | jackson-dataformat-csv | Managed | CSV import/export | Major | Reporting functionality |
| SOUP-012 | lombok | 1.18.30 | Code generation | Minor | Compile-time only |
| SOUP-013 | pdfbox | 3.0.2 | PDF generation | Major | Medical documents |
| SOUP-014 | spring-boot-starter-freemarker | Managed | Template engine | Major | Notifications and reports |
| SOUP-015 | mysql-connector-j | 8.4.0 | MySQL JDBC driver | Critical | Primary database connection |
| SOUP-016 | mariadb-java-client | 1.8.0 | MariaDB JDBC driver | Critical | Alternative database driver |
| SOUP-017 | shedlock-spring | 5.12.0 | Distributed locking | Major | Scheduled task coordination |
| SOUP-018 | easy-rules-core | 3.4.0 | Business rules engine | Critical | Clinical decision logic |
| SOUP-019 | klarna-kco-rest | 4.0.3 | Payment processing | Major | Billing integration |
| SOUP-020 | spring-cloud-sleuth | 3.1.8 | Distributed tracing | Minor | Monitoring and debugging |
| SOUP-021 | spring-boot-starter-actuator | Managed | Health monitoring | Minor | Operations monitoring |

### 4.2 SOUP Details

#### 4.2.1 spring-boot-starter-data-jpa (SOUP-001)

| Property | Value |
|----------|-------|
| Name | org.springframework.boot:spring-boot-starter-data-jpa |
| Version | Managed by parent POM 2.30.0 |
| Source | https://github.com/spring-projects/spring-boot |
| License | Apache-2.0 |
| Safety Class | Critical |

**Purpose:**
Provides JPA/Hibernate persistence framework for managing patient health records, appointments, and clinical data in relational database.

**Functionality Used:**
- JPA repository interfaces (@Repository)
- Entity management (@Entity, @Table, @Column)
- Transaction management (@Transactional)
- Query methods (findBy*, @Query)
- Optimistic locking (@Version)
- Database schema validation

**Integration Points:**
- Used by: SI-01-03 (Domain & Persistence Layer)
- Integration method: Direct dependency, Spring Boot auto-configuration

**Isolation:**
Database transaction boundaries contain failures; rollback prevents partial updates. Connection pooling limits resource exhaustion.

**Verification:**
- Integration tests using TestContainers with MySQL
- Repository method tests validating CRUD operations
- Transaction rollback tests
- Optimistic locking conflict tests

**Known Anomalies:**
None identified. Spring Boot manages version compatibility between Spring Data JPA and Hibernate.

---

#### 4.2.2 hibernate-core (SOUP-002)

| Property | Value |
|----------|-------|
| Name | org.hibernate:hibernate-core |
| Version | Managed by Spring Boot parent |
| Source | https://github.com/hibernate/hibernate-orm |
| License | LGPL-2.1 (runtime library use, not derivative) |
| Safety Class | Critical |

**Purpose:**
ORM framework converting Java objects to database records, handling complex entity relationships and query generation.

**Functionality Used:**
- Entity mapping annotations (@Entity, @ManyToOne, @OneToMany)
- Lazy loading of associations
- Caching (first-level, query cache)
- HQL and Criteria queries
- Schema validation against database
- Optimistic locking enforcement
- Transaction coordination with JTA

**Integration Points:**
- Used by: SI-01-03 (Domain & Persistence Layer)
- Integration method: Transitive dependency via Spring Data JPA

**Isolation:**
Hibernate session boundaries isolate database operations. Connection pool prevents runaway connections. Query timeout enforcement (30s) prevents long-running queries.

**Verification:**
- Entity mapping correctness validated via integration tests
- Complex query result validation
- Lazy loading behavior tests
- Cache eviction tests
- Transaction isolation level tests

**Known Anomalies:**
- N+1 query problem mitigated via @EntityGraph and batch fetch size configuration
- LazyInitializationException handled via DTOs at API boundary

---

#### 4.2.3 jackson-databind (SOUP-009)

| Property | Value |
|----------|-------|
| Name | com.fasterxml.jackson.core:jackson-databind |
| Version | Managed by Spring Boot parent |
| Source | https://github.com/FasterXML/jackson-databind |
| License | Apache-2.0 |
| Safety Class | Critical |

**Purpose:**
JSON serialization/deserialization for REST API data exchange, converting between DTOs and JSON payloads.

**Functionality Used:**
- Object to JSON serialization
- JSON to object deserialization
- Custom serializers/deserializers for domain types
- Date/time formatting via jackson-datatype-jsr310
- Polymorphic type handling
- Null handling strategies

**Integration Points:**
- Used by: SI-01-01 (REST API Layer)
- Integration method: Spring MVC auto-configured message converters

**Isolation:**
Deserialization failures return HTTP 400 Bad Request, preventing invalid data entry. Fail-on-unknown-properties configured to reject malformed input.

**Verification:**
- Serialization roundtrip tests for all DTOs
- Edge case testing (null values, special characters, unicode)
- Security testing for deserialization vulnerabilities
- Date/time timezone handling tests

**Known Anomalies:**
- Security advisories monitored via OWASP dependency check
- Polymorphic deserialization disabled except for known safe types

---

#### 4.2.4 flyway-core (SOUP-008)

| Property | Value |
|----------|-------|
| Name | org.flywaydb:flyway-core |
| Version | Managed by Spring Boot parent |
| Source | https://github.com/flyway/flyway |
| License | Apache-2.0 |
| Safety Class | Critical |

**Purpose:**
Database schema version control and migration, managing schema evolution across deployments.

**Functionality Used:**
- SQL migration script execution
- Version tracking in flyway_schema_history_v2 table
- Baseline on migrate for existing databases
- Migration validation on startup
- Checksums for script integrity

**Integration Points:**
- Used by: SI-01-03 (Domain & Persistence Layer)
- Integration method: Spring Boot auto-configuration runs on startup

**Isolation:**
Flyway runs migrations in database transactions; failures cause rollback. Application startup fails if migrations fail, preventing operation with incorrect schema.

**Verification:**
- Migration testing on production-like dataset
- Rollback script testing where applicable
- Migration ordering validation
- Data integrity checks post-migration
- Idempotency testing for repeatable migrations

**Known Anomalies:**
- Baseline version set to 1 to handle existing production databases
- ignore-missing-migrations enabled for historical migrations

---

#### 4.2.5 easy-rules-core (SOUP-018)

| Property | Value |
|----------|-------|
| Name | org.jeasy:easy-rules-core |
| Version | 3.4.0 |
| Source | https://github.com/j-easy/easy-rules |
| License | MIT |
| Safety Class | Critical |

**Purpose:**
Business rules engine for implementing clinical decision logic, workflow rules, and clinical pathway management.

**Functionality Used:**
- Rule definition via Java annotations
- Rule execution with facts
- Condition evaluation
- Action invocation
- Rule priority ordering
- Composite rules

**Integration Points:**
- Used by: SI-01-02 (Service Layer)
- Integration method: Direct instantiation of RulesEngine, Spring-managed rule beans

**Isolation:**
Rules operate on isolated fact objects; exceptions in rule actions caught and logged without system failure.

**Verification:**
- Rule execution tests for all clinical pathways
- Edge case testing (boundary values)
- Rule priority ordering validation
- Performance testing with complex rule sets
- Clinical pathway validation against specifications

**Known Anomalies:**
- Last release in 2020; project has minimal maintenance
- Consider migration to actively maintained alternative
- Current usage validated and tested thoroughly

---

## 5. Interface Definitions

### 5.1 External Interfaces

#### 5.1.1 REST API v1 (IF-001)

| Property | Value |
|----------|-------|
| Type | REST API / HTTPS |
| Provider | SI-01-01-01 (v1 Controllers) |
| Consumers | Web clients, mobile apps, external systems |
| Protocol | HTTPS |
| Authentication | JWT Bearer tokens |
| Authorization | Role-based (@Secured annotations) |

**Endpoints:**

| Method | Path | Description | Roles |
|--------|------|-------------|-------|
| POST | /v1/appointments | Create appointment | PRACTITIONER, PATIENT, SYSTEM |
| GET | /v1/appointments/{id} | Get appointment by ID | PRACTITIONER, PATIENT, SYSTEM |
| PUT | /v1/appointments/{id} | Update appointment | PRACTITIONER, SYSTEM |
| POST | /v1/appointments/{id}/handover | Propose handover | PRACTITIONER |
| PUT | /v1/appointments/{id}/handover/accept | Accept handover | PRACTITIONER |
| PUT | /v1/appointments/{id}/handover/deny | Deny handover | PRACTITIONER |
| PUT | /v1/appointments/{id}/handover/cancel | Cancel handover | PRACTITIONER |
| GET | /v1/appointments/clinic | List clinic appointments | PRACTITIONER, SECRETARY |
| GET | /v1/appointments/patient/{patientId} | List patient appointments | PATIENT, PRACTITIONER |
| POST | /v1/consultations | Create consultation | PRACTITIONER |
| GET | /v1/consultations/{id} | Get consultation | PRACTITIONER |

**Data Schemas:**

Appointment DTO:
```json
{
  "id": "string",
  "patientId": "string",
  "practitionerId": "string",
  "careUnitId": "string",
  "type": "CONSULTATION | DROP_IN | SCHEDULED",
  "status": "string",
  "notifiedAt": "ISO-8601 timestamp",
  "queuedAt": "ISO-8601 timestamp",
  "startedAt": "ISO-8601 timestamp",
  "endedAt": "ISO-8601 timestamp",
  "handoverStatus": "HANDOVER_ENABLED | HANDOVER_PROPOSED | HANDOVER_ACCEPTED | HANDOVER_DENIED | HANDOVER_CANCELLED"
}
```

Handover Request DTO:
```json
{
  "targetCareUnitId": "string (required)",
  "targetPractitionerRole": "string (required)",
  "targetPractitionerCapability": "string (optional)",
  "targetPractitionerId": "string (optional)",
  "handoverNotes": "string (max 1024 chars, optional)",
  "practitionerRole": "string (required)"
}
```

**Error Handling:**

| Error Code | Meaning | Client Action |
|------------|---------|---------------|
| 400 | Bad Request - validation failure | Fix request payload |
| 401 | Unauthorized - missing/invalid JWT | Re-authenticate |
| 403 | Forbidden - insufficient permissions | Request access or use different account |
| 404 | Not Found - entity doesn't exist | Check entity ID |
| 409 | Conflict - concurrent update | Retry with latest version |
| 500 | Internal Server Error | Report to support |

**Rate Limiting:** None currently implemented (consider for future)

**Versioning:** Path-based versioning (/v1/, /v2/, /v3/)

---

### 5.2 Internal Interfaces

#### 5.2.1 Service Layer Interface (Business Services)

**Purpose:** Service layer exposes business operations to controllers

**Type:** Java interfaces with Spring @Service implementations

**Example Interface:**
```java
public interface UpdateAppointmentService {
    AppointmentDTO updateAppointment(String id, UpdateAppointmentDTO dto);
    AppointmentDTO handoverAppointment(String id, HandoverAction action, HandoverRequestDTO dto);
}
```

**Coupling:** Loose coupling via dependency injection

**Transaction Boundaries:** Service methods annotated with @Transactional define transaction boundaries

---

#### 5.2.2 Repository Interface (Data Access)

**Purpose:** Repository layer abstracts database operations

**Type:** Spring Data JPA repository interfaces

**Example Interface:**
```java
@Repository
public interface AppointmentRepository extends JpaRepository<Appointment, String> {
    List<Appointment> findByPatientIdAndCancelledAtIsNull(String patientId);

    @Query("SELECT a FROM Appointment a WHERE a.practitionerId = :practId " +
           "AND a.startedAt IS NOT NULL AND a.endedAt IS NULL")
    List<Appointment> findActiveByPractitioner(@Param("practId") String practitionerId);
}
```

**Coupling:** Loose coupling via Spring Data abstractions

---

#### 5.2.3 External Service Client Interface

**Purpose:** Integration with external healthcare services

**Type:** Java interfaces with HTTP client implementations

**Example Interface:**
```java
public interface DirectoryService {
    PatientDTO getPatient(String patientId);
    PractitionerDTO getPractitioner(String practitionerId);
    boolean validatePractitionerId(String practitionerId);
    List<String> getPractitionerRelationships(String practitionerId);
}
```

**Error Handling:**
- Retry on transient failures (via @Retryable)
- Circuit breaker pattern (consider Resilience4j)
- Fallback to cached data where appropriate
- Timeouts: 10s connect, 5min read

---

#### 5.2.4 Event Publisher Interface (JMS)

**Purpose:** Publish domain events to message broker

**Type:** Java interface with JMS implementation

**Example Interface:**
```java
public interface AppointmentEventSender {
    void sendAppointmentCreated(AppointmentEvent event);
    void sendAppointmentUpdated(AppointmentEvent event);
    void sendHandoverProposed(AppointmentHandoverEvent event);
    void sendHandoverAccepted(AppointmentHandoverEvent event);
}
```

**Topics:**
- appointmentEventTopic
- appointmentHandoverEventTopic
- consultationEventTopic

**Delivery Guarantees:** At-least-once delivery via persistent messages

---

## 6. Data Architecture

### 6.1 Data Stores

| Store | Type | Purpose | Software Items |
|-------|------|---------|----------------|
| health_manager DB | MySQL 8.x | Primary data persistence | SI-01-03 |
| flyway_schema_history_v2 | MySQL table | Schema version tracking | SI-01-03-03 |
| Authorization cache | In-memory (Caffeine) | Authorization decision caching | SI-01-05-02 |
| Clinic list cache | In-memory (Caffeine) | Practitioner appointment list caching | SI-01-02-06 |

### 6.2 Data Flows

**Appointment Creation Flow:**
```
[Client]
  → POST /v1/appointments (HTTPS/JSON)
  → AppointmentController (validate JWT, check roles)
  → UpdateAppointmentService (@Transactional)
    → AppointmentRepository (save entity)
    → AppointmentEventSender (publish JMS event)
    → [MySQL DB] (persist)
    → [Artemis Broker] (publish event)
  ← AppointmentDTO (JSON response)
[Client]
```

**Appointment Handover Flow:**
```
[Practitioner A]
  → POST /v1/appointments/{id}/handover
  → AppointmentController
  → UpdateAppointmentService
    → AppointmentHandoverService (@Transactional)
      → CustomizationService (check feature flags)
      → CareUnitRegistry (validate care units)
      → DirectoryService (validate practitioner ID)
      → AppointmentHandoverProcessRepository (save process)
      → AppointmentHandoverEventRepository (save audit event)
      → NotifierClient (notify target practitioner)
      → MeetingPlaceService (post message)
      → AppointmentEventSender (publish event)
  ← AppointmentDTO
[Practitioner A]

[Practitioner B] receives notification
  → PUT /v1/appointments/{id}/handover/accept
  → AppointmentController
  → UpdateAppointmentService
    → AppointmentHandoverService (@Transactional)
      → AppointmentRepository (update practitionerId)
      → AppointmentHandoverProcessRepository (update status)
      → AppointmentHandoverEventRepository (save audit event)
      → NotifierClient (notify original practitioner)
      → MeetingPlaceService (post message)
      → AppointmentEventSender (publish event)
  ← AppointmentDTO
[Practitioner B]
```

**Patient Appointment Query Flow:**
```
[Patient]
  → GET /v1/appointments/patient/{patientId}
  → PatientAppointmentController
    → AppointmentAuthorizer (check patient owns appointments)
    → PatientAppointmentService
      → AppointmentRepository (query by patientId)
      → [Authorization Cache] (cache hit/miss)
  ← List<AppointmentDTO>
[Patient]
```

### 6.3 Data Retention and Protection

**Data Retention:**
- Active appointments: Retained indefinitely while appointment active
- Completed appointments: Retained per regulatory requirements (typically 7-10 years)
- Audit events: Immutable, retained per regulatory requirements
- Handover process records: Retained with appointment data

**Data Protection:**
- Database encryption at rest (MySQL transparent encryption)
- TLS 1.2+ for data in transit
- Role-based access control at API and database levels
- Audit logging of all data access
- Backup encryption
- GDPR data erasure via SystemDataErasureController

**Personal Data:**
- Patient ID (pseudonym, not direct identifier)
- Practitioner ID (pseudonym)
- Clinical notes contain free text (protected by encryption and access control)
- Handover notes (max 1024 chars, not for sensitive clinical info)

---

## 7. Safety Architecture

### 7.1 Safety-Critical Components

| Component | Safety Class | Safety Function |
|-----------|--------------|-----------------|
| SI-01-02-01 (AppointmentHandoverService) | B | Manages clinical responsibility transfer |
| SI-01-03-01 (Appointment entity) | B | Stores core appointment data |
| SI-01-03-01 (AppointmentHandoverProcess) | B | Tracks handover state and ownership |
| SI-01-03-03 (Database migrations) | B | Schema changes affecting data integrity |
| SI-01-05-02 (AppointmentAuthorizer) | B | Protects patient privacy |
| SI-01-02-07 (AppointmentNoteService) | B | Clinical documentation management |
| SI-01-04-02 (Event publishers) | B | Notification delivery for care coordination |

### 7.2 Segregation Mechanisms

**Mechanism 1: Transaction Boundaries**
- Type: Database transaction isolation
- Purpose: Prevent partial updates and ensure data consistency
- Protected items: SI-01-02 (all service methods), SI-01-03 (persistence layer)
- Implementation: Spring @Transactional with REQUIRED propagation, MySQL REPEATABLE_READ isolation

**Mechanism 2: Role-Based Access Control**
- Type: Authentication and authorization layer
- Purpose: Prevent unauthorized access to patient data
- Protected items: SI-01-01 (all controllers), SI-01-05 (security layer)
- Implementation: Spring Security with @Secured annotations, AppointmentAuthorizer for fine-grained checks

**Mechanism 3: Optimistic Locking**
- Type: Concurrency control
- Purpose: Prevent lost updates in concurrent scenarios
- Protected items: Appointment entity, AppointmentHandoverProcess entity
- Implementation: JPA @Version annotation with automatic version checking

**Mechanism 4: Audit Trail**
- Type: Immutable event logging
- Purpose: Regulatory compliance and incident investigation
- Protected items: SI-01-03-01 (AppointmentHandoverEvent entity)
- Implementation: Append-only database table, no update or delete operations

**Mechanism 5: Input Validation**
- Type: Data validation layer
- Purpose: Prevent invalid data entry corrupting clinical records
- Protected items: SI-01-01 (REST API), SI-01-08 (DTOs)
- Implementation: Bean Validation annotations (@NotNull, @Size, @Valid), Spring MVC validation

### 7.3 Failure Propagation Controls

| Failure Type | Source | Control | Outcome |
|--------------|--------|---------|---------|
| Database connection failure | SI-01-03 | Connection pool exhaustion protection, connection timeout | HTTP 500, service unavailable, retry after reconnection |
| External service timeout | SI-01-04-01 | HTTP client timeout (10s connect, 5min read), retry logic | Fallback behavior or error response, notification failure logged |
| JMS broker unavailable | SI-01-04-02 | Connection retry, message persistence | Events queued for retry, eventual delivery |
| Concurrent update conflict | SI-01-03-01 | Optimistic locking exception | HTTP 409 Conflict, client retries with latest version |
| Authentication failure | SI-01-05-01 | JWT validation failure | HTTP 401 Unauthorized, client must re-authenticate |
| Authorization failure | SI-01-05-02 | Access denied | HTTP 403 Forbidden, operation blocked |
| Invalid handover state | SI-01-02-01 | State machine validation | BusinessException, HTTP 400 with error message |
| Transaction rollback | SI-01-02 | Spring transaction management | All database changes rolled back, exception propagated |

### 7.4 Defensive Programming

**Key defensive programming techniques:**

1. **Null Safety:**
   - Use of `Optional<>` for potentially absent values
   - Null checks before dereferencing
   - Lombok @NonNull annotations on critical fields

2. **Exception Handling:**
   - Global exception handler (@ControllerAdvice) for consistent error responses
   - Checked exceptions for business rule violations
   - Runtime exceptions for programming errors
   - Never catch Exception or Throwable generically

3. **Validation:**
   - Bean Validation annotations on all DTOs
   - Custom validators for complex business rules
   - Pre-condition checks (Objects.requireNonNull, Preconditions.checkArgument)
   - Database constraints enforcing data integrity

4. **Immutability:**
   - Immutable DTOs where possible
   - Immutable audit events (no setters)
   - Collections returned as unmodifiable views
   - Value objects are immutable

5. **Fail-Fast:**
   - Application startup fails if configuration invalid
   - Flyway migration failure prevents startup
   - Bean creation failures halt application
   - Early validation of inputs

6. **Logging:**
   - Structured logging with MDC (request ID, user ID)
   - Error logging with stack traces
   - Business event logging at INFO level
   - Sensitive data redacted from logs

---

## 8. Deployment Architecture

### 8.1 Deployment Topology

```
[Production Environment]

├── [Load Balancer / Ingress Controller]
│   └── TLS termination, routing
│
├── [Health Manager Service Pods] (n instances, horizontal scaling)
│   ├── Pod 1: health-manager-svc:8080
│   ├── Pod 2: health-manager-svc:8080
│   └── Pod n: health-manager-svc:8080
│   └── Software Items: SI-01 (complete system)
│
├── [MySQL Database Cluster]
│   ├── Primary: MySQL 8.x (read-write)
│   └── Replicas: MySQL 8.x (read-only)
│   └── Used by: SI-01-03 (persistence layer)
│
├── [Apache Artemis Message Broker]
│   ├── Broker 1 (clustered)
│   └── Broker 2 (clustered)
│   └── Used by: SI-01-04-02 (event publishers)
│
├── [External Services]
│   ├── Directory Service (HTTPS)
│   ├── Notifier Service (HTTPS)
│   ├── MeetingPlace Service (HTTPS)
│   ├── Bookings Service (HTTPS)
│   ├── Interviewer Service (HTTPS)
│   └── Configuration Service (HTTPS/ZeroMQ)
│
└── [Monitoring Stack]
    ├── Prometheus (metrics scraping)
    ├── Grafana (dashboards)
    └── Distributed tracing (Sleuth/Zipkin)
```

### 8.2 Environment Configuration

| Environment | Purpose | Differences |
|-------------|---------|-------------|
| Development | Local development | H2 in-memory DB, embedded Artemis, mock external services, debug logging |
| Test | Automated testing | TestContainers (MySQL, Artemis), test profiles, integration test data |
| Staging | Pre-production validation | Production-like configuration, test data, relaxed rate limits, feature flags for testing |
| Production | Live system | Full security hardening, real external services, encrypted secrets, performance tuning, monitoring alerts |

**Configuration Management:**
- application.yml (defaults)
- application-{profile}.yml (environment-specific overrides)
- Environment variables (secrets, external URLs)
- Configuration Service (runtime feature flags, care unit registry)

**Secret Management:**
- JWT signing keys (external secret store)
- Database passwords (external secret store)
- API keys for external services (external secret store)
- TLS certificates (cert-manager or external PKI)

### 8.3 Containerization

**Docker Image:**
- Base image: eclipse-temurin:21-jre-alpine (OpenJDK 21 runtime)
- Application JAR: health-manager-svc.jar
- Non-root user execution
- Health check endpoint: /actuator/health
- Minimal attack surface (no shell, no package manager)

**Resource Limits:**
- Memory: 2GB (request), 4GB (limit)
- CPU: 1 core (request), 2 cores (limit)
- Ephemeral storage: 1GB

**Health Checks:**
- Liveness probe: /actuator/health/liveness (Spring Boot Actuator)
- Readiness probe: /actuator/health/readiness (checks DB, JMS connections)
- Startup probe: /actuator/health (allows slow startup)

---

## 9. Traceability

### 9.1 Requirements to Architecture

| Requirement | Implementing Items |
|-------------|-------------------|
| SRS-APPT-001 | SI-01-01-01-01, SI-01-02-02 |
| SRS-APPT-002 | SI-01-01-01-01, SI-01-02-03, SI-01-05-02 |
| SRS-APPT-003 | SI-01-01-01-01, SI-01-02-02 |
| SRS-APPT-004 | SI-01-03-01 (Appointment entity) |
| SRS-HAND-001 | SI-01-01-01-01, SI-01-02-01, SI-01-02-02 |
| SRS-HAND-002 | SI-01-02-01, SI-01-04-01 (NotifierClient, MeetingPlaceService) |
| SRS-HAND-003 | SI-01-02-01 |
| SRS-HAND-004 | SI-01-02-01 |
| SRS-HAND-005 | SI-01-02-01, SI-01-06-01, SI-01-06-02 |
| SRS-CONS-001 | SI-01-01-01 (ConsultationController), SI-01-02-05 |
| SRS-CONS-002 | SI-01-01-01 (ConsultationController), SI-01-02-05 |
| SRS-SEC-001 | SI-01-05-01, SI-01-05-02 |
| SRS-SEC-002 | SI-01-05-02 (AppointmentAuthorizer) |
| SRS-SEC-003 | SI-01-05-01 (JWT Authentication) |
| SRS-SEC-004 | SI-01-05-03 (Audit logging) |
| SRS-INT-001 | SI-01-04-01 (DirectoryService) |
| SRS-INT-002 | SI-01-04-01 (NotifierClient) |
| SRS-INT-003 | SI-01-04-01 (MeetingPlaceService) |
| SRS-INT-004 | SI-01-04-02 (JMS publishers) |
| SRS-INT-005 | SI-01-04-01 (BookingsService) |
| SRS-INT-006 | SI-01-06 (Configuration layer) |
| SRS-PERF-001 | SI-01-03 (HikariCP configuration) |
| SRS-PERF-002 | SI-01-03 (JPA query timeout) |
| SRS-PERF-003 | SI-01-03 (Hibernate query cache) |
| SRS-PERF-004 | SI-01-07-01 (Caching layer) |
| SRS-PERF-006 | SI-01-04-01 (HTTP client timeout) |
| SRS-DATA-001 | SI-01-03 (Persistence layer) |
| SRS-DATA-002 | SI-01-03-03 (Flyway migrations) |
| SRS-DATA-003 | SI-01-03-01 (AppointmentHandoverProcess entity) |
| SRS-DATA-004 | SI-01-03 (MySQL utf8mb4 charset) |
| SRS-DATA-005 | SI-01-03-01 (Optimistic locking) |
| SRS-AUD-001 | SI-01-03-01 (AppointmentHandoverEvent entity) |
| SRS-AUD-002 | SI-01-02-01 (Handover service logging) |
| SRS-OPER-001 | SI-01-06 (Configuration layer) |
| SRS-OPER-002 | SI-01-07-02 (Actuator endpoints) |
| SRS-OPER-003 | SI-01-07-04 (Scheduled tasks) |
| SRS-OPER-004 | SI-01-06 (Feature flags) |
| SRS-UI-001 | SI-01-01 (REST API layer) |
| SRS-UI-002 | SI-01-01 (OpenAPI annotations) |
| SRS-UI-003 | SI-01-02 (TranslationService) |
| SRS-SAF-001 | SI-01-02-01, SI-01-06-02 |
| SRS-SAF-002 | SI-01-02-01, SI-01-04-01 (DirectoryService) |
| SRS-SAF-003 | SI-01-02-01 |
| SRS-SAF-004 | SI-01-02-01 |
| SRS-SAF-005 | SI-01-02-01 |
| SRS-REG-001 | SI-01-02-12 (DataErasureService) |

### 9.2 Architecture to SOUP

| Software Item | SOUP Dependencies |
|---------------|-------------------|
| SI-01-01 (REST API) | SOUP-001 (Spring Boot), SOUP-009 (Jackson), SOUP-032 (OpenAPI) |
| SI-01-02 (Service Layer) | SOUP-001 (Spring Boot), SOUP-018 (Easy Rules), SOUP-030 (Spring Retry) |
| SI-01-02-01 (Handover Service) | SOUP-001, SOUP-002 (Hibernate) |
| SI-01-02-06 (Clinic List Service) | SOUP-003 (QueryDSL), SOUP-004 (Blaze Persistence) |
| SI-01-03 (Persistence) | SOUP-001, SOUP-002, SOUP-003, SOUP-008 (Flyway), SOUP-015 (MySQL Connector), SOUP-016 (MariaDB Connector) |
| SI-01-04-01 (Service Clients) | SOUP-007 (Apache CXF), SOUP-030 (Spring Retry) |
| SI-01-04-02 (Event Publishers) | SOUP-005 (Artemis), SOUP-006 (Artemis Server - test only) |
| SI-01-05 (Security) | SOUP-001 (Spring Security) |
| SI-01-07-01 (Caching) | SOUP-039 (Spring Cache) |
| SI-01-07-02 (Monitoring) | SOUP-021 (Actuator), SOUP-020 (Sleuth) |

### 9.3 Architecture to Tests

| Software Item | Test Coverage |
|---------------|---------------|
| SI-01-02-01 | No dedicated tests found (gap identified in requirements.json) |
| SI-01-02-03 | svc/src/test/java/se/alerisx/mhp/manager/service/impl/GetAppointmentServiceImplTest.java |
| SI-01-03-02 (Repositories) | Integration tests via TestContainers |
| SI-01-05-02 (AppointmentAuthorizer) | No dedicated tests found (gap identified) |
| SI-01-08 (SafeCharacterService) | svc/src/test/java/se/alerisx/mhp/manager/service/impl/SafeCharacterServiceImplTest.java |

---

## 10. Appendices

### Appendix A: Extraction Metadata

**Source Repository:**
- Repository: /home/jakob/Noncomplicity/Repos/health-manager
- Commit: 16e0273739eda65f73af5410323e1caf53362f16
- Extracted: 2026-03-06T09:10:18Z

**Extraction Process:**
- Automated extraction from source code
- Manual enrichment of safety classifications
- Requirements traceability from extracted requirements.json
- SOUP catalog from extracted soup-list.json
- Architecture hierarchy from extracted architecture.json

**Data Sources:**
- Java source files (controllers, services, repositories, entities)
- Maven POM files (dependencies)
- Configuration files (application.yml)
- Documentation (handover_architecture.md, software_requirements.md)
- Test files (unit and integration tests)

### Appendix B: Glossary

| Term | Definition |
|------|------------|
| SOFTWARE ITEM | IEC 62304: Any identifiable part of a software system, including system, subsystem, module, or unit |
| SOFTWARE UNIT | IEC 62304: Lowest level software item, not subdivided further |
| SOUP | Software of Unknown Provenance: software item integrated into medical device software but not developed under documented lifecycle per IEC 62304 |
| Appointment | Healthcare encounter between patient and practitioner, scheduled or on-demand |
| Handover | Transfer of clinical responsibility for an appointment from one practitioner to another |
| Episode of Care | Collection of related appointments for ongoing treatment of a condition |
| Consultation | Clinical interaction during an appointment, includes assessment and treatment |
| Care Unit | Organizational unit within healthcare provider (e.g., cardiology department) |
| Practitioner | Healthcare professional providing medical services |
| JWT | JSON Web Token: authentication token containing user claims |
| JMS | Java Message Service: API for asynchronous messaging |
| JPA | Java Persistence API: ORM specification for Java |
| DTO | Data Transfer Object: serializable object for API data exchange |

### Appendix C: Acronyms

| Acronym | Expansion |
|---------|-----------|
| API | Application Programming Interface |
| CRUD | Create, Read, Update, Delete |
| CSV | Comma-Separated Values |
| GDPR | General Data Protection Regulation |
| HTTP | Hypertext Transfer Protocol |
| HTTPS | HTTP Secure (over TLS) |
| IEC | International Electrotechnical Commission |
| ISO | International Organization for Standardization |
| JDBC | Java Database Connectivity |
| JSON | JavaScript Object Notation |
| MDR | Medical Device Regulation (EU 2017/745) |
| ORM | Object-Relational Mapping |
| PDF | Portable Document Format |
| REST | Representational State Transfer |
| RBAC | Role-Based Access Control |
| SAD | Software Architecture Description |
| SOAP | Simple Object Access Protocol |
| SQL | Structured Query Language |
| SRS | Software Requirements Specification |
| TLS | Transport Layer Security |
| URL | Uniform Resource Locator |
| UTF | Unicode Transformation Format |
| V&V | Verification and Validation |

### Appendix D: Document History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-03-06 | Automated extraction | Initial generation from source code |

### Appendix E: Open Issues and Gaps

**Testing Gaps (from requirements.json):**
1. AppointmentHandoverService has no dedicated integration tests despite being safety-critical (Class B)
2. AppointmentAuthorizer security component lacks test coverage
3. JMS event publishing (AppointmentEventSender) verification method unclear

**Architecture Documentation Gaps:**
1. SOUP hazard analysis not documented
2. Performance requirements not explicitly specified (response times, throughput)
3. Backup and recovery procedures not documented
4. Disaster recovery plan not specified
5. Data retention policies not formalized

**SOUP Management Gaps:**
1. mariadb-java-client 1.8.0 extremely outdated (2017), likely has vulnerabilities - upgrade required
2. H2 Database 1.4.200 has CVE-2022-45868 (critical RCE) - ensure console disabled, consider upgrade
3. commons-lang 2.6 deprecated (2011) - migrate to commons-lang3
4. Spring Cloud Sleuth 3.1.8 deprecated - migrate to Micrometer Tracing
5. easy-rules-core 3.4.0 minimal maintenance since 2020 - evaluate alternatives
6. Transitive dependencies not fully cataloged

**Recommendation:** Address testing gaps for safety-critical components as highest priority. Complete SOUP qualification activities per IEC 62304:2006 clause 8.1.2.

---

*End of Software Architecture Description*
