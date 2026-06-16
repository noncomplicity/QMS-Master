---
id: 725af57
title: "IFU"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "InstructionsForUse"
process: "[Document and Record Control](../../../Canvases/Document%20and%20Record%20Control.canvas)"
requirements:
  - "[IEC 82304-1:2016](../../../Requirements/IEC%2082304-1.md)"
owner: "[Regulatory Affairs](../../../Assets/Regulatory%20Affairs.md)"
---

# Instructions for Use

## health-manager v1.0.0-SNAPSHOT

**Manufacturer:** Platform24 / Alerisx
**Address:** SE-116 23 Stockholm, Sweden
**Website:** https://platform24.se
**Document Version:** 1.0
**Effective Date:** 2026-03-06

---

## Table of Contents

1. [Product Identification](#1-product-identification)
2. [Intended Use](#2-intended-use)
3. [Warnings and Precautions](#3-warnings-and-precautions)
4. [System Requirements](#4-system-requirements)
5. [Installation](#5-installation)
6. [Getting Started](#6-getting-started)
7. [Using the Software](#7-using-the-software)
8. [Messages and Troubleshooting](#8-messages-and-troubleshooting)
9. [Shutting Down](#9-shutting-down)
10. [Decommissioning](#10-decommissioning)
11. [Technical Description](#11-technical-description)
12. [Support and Contact](#12-support-and-contact)

---

## 1. Product Identification

**Product Name:** health-manager
**Version:** 1.0.0-SNAPSHOT
**Manufacturer:** Platform24 / Alerisx
**Contact:** SE-116 23 Stockholm, Sweden
**Website:** https://platform24.se
**Software Safety Classification:** Class A (highest safety level)

---

## 2. Intended Use

### 2.1 Purpose

Healthcare encounter management system enabling practitioners to manage patient appointments, consultations, medical records, prescriptions, referrals, and handovers within a digital health platform.

The software manages the complete lifecycle of patient healthcare encounters including appointment scheduling, clinical documentation (notes, diagnoses, prescriptions, referrals), practitioner-patient communication, handover of care between practitioners, integration with external health systems, and billing coordination.

### 2.2 Intended Users

**Healthcare Professional**
- **Description:** Licensed healthcare practitioners (doctors, nurses, specialists) providing remote consultations and managing patient encounters
- **Required Skills:**
  - Clinical medical training
  - Digital consultation competency
  - Electronic health record management
  - Video consultation platform operation
- **Required Training:**
  - Medical license and credentials verification
  - Platform-specific clinical workflow training
  - GDPR and patient data protection compliance
  - Handover procedures and care coordination protocols
- **Required Knowledge:**
  - Clinical decision-making and diagnosis
  - Swedish healthcare regulations (Patientdatalagen)
  - Digital prescription practices
  - Care unit operational procedures

**Patient**
- **Description:** Patients seeking remote healthcare services and managing their own appointments
- **Required Skills:**
  - Basic digital literacy
  - Video call capability
  - Ability to describe symptoms and medical history
- **Required Training:**
  - Platform onboarding (self-service)
  - Video consultation preparation guidance
- **Required Knowledge:**
  - Own medical history awareness
  - Understanding of consultation process
  - Payment/billing procedures

**Administrator**
- **Description:** Care unit administrators, secretaries, and support staff managing schedules, billing, and operational tasks
- **Required Skills:**
  - Administrative healthcare operations
  - Schedule management
  - Billing and insurance processing
  - System configuration
- **Required Training:**
  - Platform administrative functions training
  - Care unit configuration procedures
  - Data export and reporting procedures
- **Required Knowledge:**
  - Healthcare billing practices
  - Care unit operational procedures
  - Patient data confidentiality requirements

**Technical Operator**
- **Description:** System administrators and DevOps engineers maintaining the service infrastructure
- **Required Skills:**
  - Java/Spring Boot application management
  - Kubernetes orchestration
  - Database administration (MySQL/MariaDB)
  - JMS messaging systems (Apache Artemis)
  - Monitoring and alerting (Prometheus)
- **Required Training:**
  - Platform24 infrastructure training
  - Incident response procedures
  - Deployment and rollback procedures
  - Security update protocols
- **Required Knowledge:**
  - Healthcare data security requirements
  - GDPR technical compliance
  - System architecture and dependencies
  - Backup and disaster recovery procedures

### 2.3 Intended Environment

**Platforms:** Cloud-based SaaS platform accessed via web browsers and mobile applications

**Connectivity:**
- Online operation required
- Protocols: HTTPS/TLS, WebSocket, JMS (Apache Artemis), WebRTC
- Bandwidth: Sufficient for video consultation streaming
- Latency: 30 second database query timeout, 10 second remote service connect timeout, 5 minute read timeout

**Physical Environment:**
- Clinical settings (hospitals, clinics, care units)
- Home environments (practitioners and patients)
- Mobile environments (tablets, smartphones)

### 2.4 Essential Functions

- **Appointment Management:** Manages patient appointments across multiple modalities (video consultation, asynchronous consultation, scheduled appointments)
- **Clinical Documentation:** Supports clinical documentation including notes, diagnoses, prescriptions, and referrals
- **Handover Management:** Enables secure handover of patient encounters between healthcare practitioners
- **EHR Integration:** Integrates with external health systems (Alfa e-Recept for prescriptions, TakeCare for case notes, Efrikort for patient cards)
- **Notification System:** Provides practitioner notification and alert system for care coordination
- **Medical History Tracking:** Tracks patient medical history and medication records
- **Waiting Room Management:** Supports waiting room management and appointment queuing
- **Video Consultation:** Enables secure video meetings and messaging between practitioners and patients

### 2.5 Contraindications

- **Not for emergency medical situations** requiring immediate care
- **Not a standalone diagnostic system** - clinical judgment required
- **Cross-care-provider handovers disabled by default** (requires explicit feature flag)
- **Not for use without valid practitioner credentials and active shift**

### 2.6 Known Limitations

- Practitioners must have valid medical credentials verified via Directory service
- Practitioners must have active shift to accept appointments
- Patients can only access their own appointments (or related minors)
- Cross-care-provider operations require explicit feature flag authorization
- Limited access roles (ROLE_PRACTITIONER_LIMITED_ACCESS) restricted to specific patient sets
- Handover notes are limited to 1024 characters maximum
- Dependent on external service availability (Directory, MeetingPlace, Notifier, TakeCare, Alfa e-Recept)

---

## 3. Warnings and Precautions

### 3.1 Safety Warnings

**WARNING - Notification Failures**
In rare cases of system infrastructure failure, handover notifications may be delayed. Care units should maintain backup communication channels (phone, pager) for urgent handovers.

**WARNING - Character Limit**
Handover notes are limited to 1024 characters. For complex cases, practitioners should prioritize critical safety information and reference the full clinical record in the appointment notes.

**WARNING - External System Dependencies**
Clinical notes are exported to external EHR systems. In case of integration failures, notes remain accessible within the appointment management system and can be manually exported.

**WARNING - E-Prescription Availability**
E-prescriptions are transmitted electronically. In rare cases of system unavailability, practitioners may issue paper prescriptions or contact the pharmacy directly.

### 3.2 Security Warnings

**WARNING - Authentication Required**
Only authorized practitioners and patients can access sensitive medical records. Care provider boundaries are enforced unless explicit consent is given.

**WARNING - Cross-Care-Provider Data Sharing**
Cross-care-provider handovers require explicit consent and feature flag enablement (ENABLE_HANDOVERS_ACROSS_CARE_PROVIDERS). This feature is disabled by default to protect patient privacy.

**WARNING - Active Shift Required**
Practitioners must have an active shift to accept handovers and take responsibility for patient care.

### 3.3 Security Options

| Option | Description | Default Setting |
|--------|-------------|-----------------|
| ENABLE_HANDOVERS_ACROSS_CARE_PROVIDERS | Allows handovers between different care provider organizations | Disabled |
| JWT Authentication | Token-based authentication with role verification | Enabled (required) |
| Audit Logging | Comprehensive audit trail of all clinical actions | Enabled (required) |
| TLS Encryption | Encrypted communication in transit | Enabled (required) |

---

## 4. System Requirements

### 4.1 Operating Systems

**Platform-independent containerized deployment**

The software runs on Kubernetes clusters and is accessed through standard web browsers supporting:
- Modern HTML5 compliance
- WebRTC for video consultations
- WebSocket support
- JavaScript ES6+

### 4.2 Hardware Requirements

**Minimum:**
- Processor: Modern multi-core CPU capable of running Java 21
- Memory: 2 GB RAM per instance
- Storage: 10 GB for application and logs
- Display: 1280x720 resolution for web interface
- Network: Stable internet connection with sufficient bandwidth for video streaming

**Recommended:**
- Processor: 4+ core CPU
- Memory: 4 GB RAM per instance
- Storage: 50 GB for production workloads
- Display: 1920x1080 resolution
- Network: High-speed internet with low latency

**Infrastructure Requirements (Technical Operators):**
- Kubernetes cluster with auto-scaling capabilities
- MySQL/MariaDB database (10-100 database connections per instance)
- Apache Artemis JMS message broker
- Object storage service (via ZeroMQ)

### 4.3 Software Dependencies

**Runtime Dependencies:**
| Dependency | Version | Purpose |
|------------|---------|---------|
| Java Runtime Environment | 21 | Application runtime |
| Spring Boot Framework | 2.x | Application framework |
| MySQL/MariaDB Database | 8.0+ / 10.5+ | Data persistence |
| Apache Artemis | Latest stable | JMS messaging |
| Kubernetes | 1.20+ | Container orchestration |

**Integration Dependencies:**
| System | Required | Purpose |
|--------|----------|---------|
| Directory Service | Yes | Practitioner and patient identity verification |
| Booking Service | Yes | Appointment scheduling |
| MeetingPlace Service | Yes | Video consultations and messaging |
| Interviewer Service | Yes | Patient triage and medical history |
| Notifier Service | Yes | Push notifications and SMS alerts |
| Actions Service | Yes | Task management |
| Medication Search Service | Yes | Medication database lookup |
| Care Planner Service | Yes | Care planning coordination |
| Object Storage | Yes | File and document storage |
| Alfa e-Recept | No | E-prescription submission (Sweden) |
| TakeCare Xchange | No | Regional EHR integration (Sweden) |
| Klarna Payment | No | Payment processing |

### 4.4 Network Requirements

- **Connectivity:** Online operation required
- **Protocols:** HTTPS/TLS (required), WebSocket, JMS, WebRTC
- **Bandwidth:** Minimum 2 Mbps for video consultations; recommended 5 Mbps
- **Latency:** Low latency recommended for real-time video consultations
- **Firewall:** Must allow outbound HTTPS connections to external services

---

## 5. Installation

### 5.1 Who Can Install

Installation and deployment must be performed by qualified technical operators with:
- Kubernetes administration experience
- Healthcare data security training
- GDPR compliance knowledge
- Platform24 infrastructure authorization

Healthcare professionals, patients, and administrators access the system via web browsers and do not perform installation.

### 5.2 Pre-Installation Checklist

Before installing, ensure:
- [ ] Kubernetes cluster is operational and meets resource requirements 🆔 ZaApDq
- [ ] MySQL/MariaDB database is provisioned with appropriate security configuration 🆔 sR9HPc
- [ ] Apache Artemis JMS broker is deployed and accessible 🆔 WizTnH
- [ ] Object storage service is configured 🆔 HGjiNg
- [ ] All required external service integrations are available (Directory, MeetingPlace, etc.) 🆔 0gbSDD
- [ ] Network security policies allow required protocols 🆔 vtNdla
- [ ] SSL/TLS certificates are provisioned for HTTPS endpoints 🆔 rhdQFJ
- [ ] JWT secret keys are generated and securely stored 🆔 k7TkUB
- [ ] Backup and disaster recovery procedures are documented 🆔 151b4g
- [ ] Monitoring and alerting infrastructure is ready (Prometheus) 🆔 R4YoFt

### 5.3 Security Configuration at Installation

| Option | Description | Recommended Setting |
|--------|-------------|---------------------|
| JWT Secret | Cryptographic key for token signing | Generate strong random key, store in secrets management |
| Database Password | Database authentication | Use strong password, rotate regularly |
| TLS Certificates | HTTPS encryption | Use valid certificates from trusted CA |
| ENABLE_HANDOVERS_ACROSS_CARE_PROVIDERS | Cross-provider data sharing | Disabled (enable only with explicit legal basis) |
| Audit Logging | Comprehensive audit trail | Enabled (required for compliance) |
| Database Connection Pool | Resource management | Min: 10, Max: 100 connections |
| Query Timeout | Prevent runaway queries | 30 seconds |

### 5.4 Installation Steps

Installation is performed via Helm deployment to Kubernetes:

1. **Configure application.yml**
   - Set database connection parameters
   - Configure external service endpoints
   - Set JWT issuer and secret
   - Configure feature flags
   - Set cache configuration

2. **Deploy database schema**
   - Flyway migrations will run automatically on first startup
   - Baseline-on-migrate is enabled for legacy databases

3. **Deploy to Kubernetes**
   - Use Helm charts provided in `charts/` directory
   - Configure resource limits and requests
   - Set up health check endpoints
   - Configure horizontal pod autoscaling

4. **Verify deployment**
   - Check Spring Boot Actuator health endpoints
   - Verify database connectivity
   - Confirm external service integrations
   - Review application logs for startup errors

5. **Configure monitoring**
   - Set up Prometheus metric scraping
   - Configure alerting rules
   - Enable log aggregation
   - Set up audit log archival

For detailed installation instructions, consult the Platform24 Infrastructure Operations Manual.

---

## 6. Getting Started

### 6.1 Starting the Software

**For Healthcare Professionals, Patients, and Administrators:**

The software is accessed via web browser. No local installation required.

1. **Open web browser**
   - Navigate to the platform URL provided by your care unit
   - Expected result: Login page appears

2. **Authenticate**
   - Enter credentials provided during onboarding
   - Expected result: JWT token issued, redirected to dashboard

3. **Verify active shift (practitioners only)**
   - System checks for active shift in Directory service
   - Expected result: Dashboard displays available appointments

**For Technical Operators:**

1. **Verify Kubernetes deployment**
   ```
   kubectl get pods -n <namespace>
   ```
   - Expected result: health-manager pods in Running state

2. **Check health endpoints**
   - Access `/actuator/health`
   - Expected result: HTTP 200 with status UP

3. **Monitor logs**
   - Review application startup logs
   - Expected result: No ERROR level messages, successful bean initialization

### 6.2 Initial Configuration

**For Care Unit Administrators:**

| Setting | Description | Action Required |
|---------|-------------|-----------------|
| Care Unit Profile | Configure care unit details in Directory service | Contact system administrator |
| Practitioner Roles | Define practitioner roles and permissions | Set up in Directory service |
| Feature Flags | Enable/disable optional features | Configure in CustomizationService |
| Notification Channels | Configure SMS and push notification settings | Set up in Notifier service |
| Billing Integration | Connect payment processing (if applicable) | Configure Klarna integration |

### 6.3 First-Time Use

**Healthcare Professionals:**
1. Complete platform training and receive credentials
2. Verify medical license in Directory service
3. Activate first shift to begin accepting appointments
4. Review care unit handover procedures
5. Familiarize yourself with video consultation controls

**Patients:**
1. Create account via patient portal
2. Verify identity (BankID or similar)
3. Complete initial health questionnaire
4. Schedule first appointment
5. Test video consultation setup

**Administrators:**
1. Complete administrative training
2. Receive ROLE_ADMIN or ROLE_SECRETARY credentials
3. Configure care unit operational hours
4. Set up practitioner schedules
5. Test billing and reporting functions

---

## 7. Using the Software

### 7.1 Controls

**RESTful API Controls:**

| Control | Function | Location |
|---------|----------|----------|
| POST /api/v3/appointments | Create new appointment | Appointment Controller |
| PUT /api/v3/appointments/{id} | Update appointment details | Appointment Controller |
| POST /api/v3/appointments/{id}/handover/propose | Propose handover to another practitioner | Handover Controller |
| POST /api/v3/appointments/{id}/handover/accept | Accept a pending handover | Handover Controller |
| POST /api/v3/appointments/{id}/handover/deny | Deny a proposed handover | Handover Controller |
| POST /api/v3/appointments/{id}/handover/cancel | Cancel an active handover | Handover Controller |
| GET /api/v3/appointments | Query appointments with filters | Appointment Controller |
| POST /api/v3/appointments/{id}/notes | Add clinical notes | Notes Controller |
| POST /api/v3/prescriptions | Create e-prescription | Prescription Controller |
| POST /api/v3/referrals | Create referral | Referral Controller |

**Authentication:**
- All API calls require JWT Bearer token in Authorization header
- Tokens obtained via authentication service

### 7.2 Displays and Indicators

**Appointment Status Indicators:**

| Display | Meaning | Possible Values |
|---------|---------|-----------------|
| appointmentStatus | Current appointment state | BOOKED, CHECKED_IN, IN_PROGRESS, COMPLETED, CANCELLED |
| handoverStatus | Handover process state | ENABLED, PROPOSED, ACCEPTED, DENIED, CANCELLED |
| practitionerId | Currently responsible practitioner | UUID from Directory service |
| handoverCareUnitId | Target care unit for handover | Care unit identifier |
| handoverPractitionerRole | Required practitioner role for handover | DOCTOR, NURSE, SPECIALIST |

**System Health Indicators:**

| Display | Meaning | Location |
|---------|---------|----------|
| /actuator/health | Overall system health | Actuator endpoint |
| /actuator/metrics | Performance metrics | Actuator endpoint |
| Database connection pool | Available connections | Monitoring dashboard |
| JMS queue depth | Pending messages | Artemis console |

### 7.3 Signals

**Notification Signals:**

| Signal | Type | Meaning |
|--------|------|---------|
| Push notification | Visual/Auditory | New handover proposed to practitioner |
| SMS alert | Auditory | Urgent appointment requires attention |
| Chat message | Visual | Handover accepted/denied confirmation |
| Email notification | Visual | Appointment summary and follow-up |

**System Signals:**

| Signal | Type | Meaning |
|--------|------|---------|
| HTTP 401 | Response code | Authentication required or token expired |
| HTTP 403 | Response code | Authorization denied for requested action |
| HTTP 409 | Response code | Concurrent modification conflict |
| HTTP 500 | Response code | Internal server error |

### 7.4 Common Operations

**Proposing a Handover:**
1. Select appointment from your queue
2. Verify patient consent for handover (if crossing care provider boundaries)
3. Select target care unit and practitioner role
4. Enter handover notes (max 1024 characters) with critical patient context
5. Submit handover proposal
6. System sends notification to target practitioners
7. Wait for acceptance or denial

**Accepting a Handover:**
1. Receive notification of pending handover
2. Review patient information and handover notes
3. Verify active shift status
4. Accept handover if willing and able to take responsibility
5. System transfers appointment to your queue
6. Patient is notified of practitioner change
7. Begin patient consultation

**Clinical Documentation:**
1. Open appointment from queue
2. Review patient medical history and interview responses
3. Conduct consultation (video or asynchronous)
4. Document clinical findings in notes
5. Record diagnoses, prescriptions, and referrals
6. Finalize appointment
7. System exports notes to TakeCare EHR (if configured)

### 7.5 Symbols and Icons

API-based backend service - symbols and icons managed by frontend applications.

### 7.6 Abbreviations

| Abbreviation | Meaning |
|--------------|---------|
| API | Application Programming Interface |
| EHR | Electronic Health Record |
| GDPR | General Data Protection Regulation |
| IFU | Instructions for Use |
| JMS | Java Message Service |
| JWT | JSON Web Token |
| RBAC | Role-Based Access Control |
| REST | Representational State Transfer |
| SaaS | Software as a Service |
| SMS | Short Message Service |
| TLS | Transport Layer Security |
| UUID | Universally Unique Identifier |

---

## 8. Messages and Troubleshooting

### 8.1 System Messages

| Message | Meaning | Action Required |
|---------|---------|-----------------|
| Handover proposed to {careUnit} {practitionerRole} | Handover successfully initiated | Wait for target practitioner response |
| Handover accepted by {practitionerId} | Handover completed, appointment transferred | No action - informational |
| Handover denied by {practitionerId} | Target practitioner declined handover | Propose to different practitioner or handle personally |
| Appointment assigned to you | New appointment in your queue | Review and begin consultation |
| Patient waiting in virtual room | Patient ready for video consultation | Join video meeting |

### 8.2 Error Messages

| Error | Code | Cause | Solution |
|-------|------|-------|----------|
| Appointment [ID] at careUnit X cannot be handed over to an invalid practitioner Y | 400 | Target practitioner not found in Directory service | Verify practitioner ID is correct and practitioner is active |
| Appointment [ID] at careUnit X cannot be handed over to careUnit at another careProvider Y | 403 | Cross-provider handover attempted without feature flag | Enable ENABLE_HANDOVERS_ACROSS_CARE_PROVIDERS or handover within same care provider |
| Appointment [ID] at careUnit X requires a valid careUnit and practitionerRole for handover | 400 | Invalid care unit or practitioner role specified | Verify care unit exists and practitioner role is valid |
| Appointment [ID] is not clear for handover given practitioner status | 409 | Appointment state does not allow handover | Check appointment status, ensure no active handover already exists |
| Appointment [ID] does not have an active handover process | 404 | Attempted to accept/deny non-existent handover | Verify handover was proposed and is still in PROPOSED state |
| User does not have permission to accept this appointment | 403 | No active shift or wrong care provider | Verify active shift in Directory service, check care provider assignment |
| Authentication token expired | 401 | JWT token validity period exceeded | Re-authenticate to obtain new token |
| Concurrent modification detected | 409 | Another user modified the same appointment | Refresh appointment data and retry operation |

### 8.3 Fault Messages

| Fault | Cause | Action | Support Required? |
|-------|-------|--------|-------------------|
| Database connection timeout | Database overload or network issue | Retry operation; if persistent, contact technical support | Yes |
| External service unavailable (Directory/MeetingPlace/etc.) | Service outage or network partition | Retry operation; check service status page; contact support if extended outage | Yes |
| JMS message publication failed | Message broker overload or misconfiguration | Operation logged for audit; notification may be delayed; contact support | Yes |
| TakeCare EHR export failed | TakeCare service unavailable or authentication issue | Notes remain in system; manual export possible; contact support for integration issues | Yes |
| E-prescription transmission failed | Alfa e-Recept service unavailable | Issue paper prescription as fallback; prescription logged for retry; contact support | Yes |
| Query timeout exceeded (>30 seconds) | Complex query or database performance issue | Simplify query filters; contact support if persistent | Yes |

---

## 9. Shutting Down

### 9.1 Normal Shutdown

**For Healthcare Professionals and Administrators:**

Web-based application - simply close browser or navigate away. Active sessions are managed by JWT token expiration.

**For Technical Operators:**

1. **Graceful Kubernetes pod termination**
   ```
   kubectl scale deployment health-manager --replicas=0
   ```
   - Spring Boot receives SIGTERM signal
   - Completes in-flight requests
   - Closes database connections cleanly
   - Releases JMS resources

2. **Verify shutdown completion**
   - Monitor application logs for graceful shutdown messages
   - Confirm all database connections released
   - Verify no orphaned transactions

### 9.2 Data Preservation

- All clinical data persists in MySQL/MariaDB database
- Audit logs retained according to data retention policy
- JMS messages may be lost if broker is shut down before processing
- Active user sessions terminated; practitioners must re-authenticate on restart

### 9.3 Session Handling

- JWT tokens remain valid until expiration (typically 24 hours)
- Users must re-authenticate after shutdown to obtain new tokens
- Active consultations should be completed before planned maintenance
- Patients in video consultations receive notification if service interruption occurs

---

## 10. Decommissioning

### 10.1 Before Decommissioning

Before decommissioning this software, consider:

**Privacy Considerations:**
- Patient medical records contain sensitive personal health data (GDPR Article 9)
- Legal retention requirements under Swedish Patientdatalagen (typically 10 years)
- Cross-border data transfers if multi-national deployment
- Patient rights to access, rectification, and erasure

**Security Considerations:**
- Secure deletion of cryptographic keys (JWT secrets, TLS certificates)
- Database backups containing patient data must be securely archived or destroyed
- Audit logs required for regulatory compliance must be retained
- Access credentials must be revoked

### 10.2 Exporting Your Data

**Data Export Capabilities:**

- **SQL Database Dump:** Full database export in SQL format
  - Includes all appointments, clinical notes, prescriptions, referrals, audit logs
  - Suitable for migration to replacement system or long-term archival

- **REST API JSON Export:** Programmatic data export via API
  - Partner data export via SystemDataErasureController
  - Appointment-level export with filtering
  - Suitable for selective data migration

**Export Procedure (Technical Operators):**

1. Use `SystemDataErasureController` endpoints for partner-level data export
2. Execute database dump using `mysqldump` or equivalent
3. Verify data integrity of exported files
4. Securely transfer to archival storage or replacement system
5. Maintain chain of custody documentation

### 10.3 Deleting Your Data

**WARNING:** Data deletion is irreversible. Ensure compliance with legal retention requirements before proceeding.

**Deletion Methods:**

- **Patient-level deletion:** Available via SystemDataErasureController (GDPR right to erasure)
- **Partner-level deletion:** Bulk deletion of all data for a care provider organization
- **Database drop:** Complete removal of all data (requires database administrator privileges)

**Deletion Procedure:**

1. Verify legal authority to delete (retention period expired or patient consent)
2. Export data if required for archival
3. Execute deletion via SystemDataErasureController API
4. Verify deletion through audit log
5. Securely wipe database backups containing deleted data

**Verification:**
- Audit logging records all deletion operations with timestamp and requesting user
- Database queries can verify data removal
- Backup retention policy ensures deleted data not restored from old backups

### 10.4 Decommissioning Steps

1. **Notify stakeholders**
   - Inform healthcare professionals, administrators, and patients
   - Provide timeline for service termination
   - Communicate data migration plan

2. **Migrate active data**
   - Export all clinical records to replacement system or archival storage
   - Verify data integrity and completeness
   - Test data import in target system

3. **Disable user access**
   - Revoke all JWT tokens
   - Disable authentication service integration
   - Update DNS to redirect to replacement system

4. **Shut down services**
   - Stop Kubernetes deployments
   - Terminate database connections
   - Shut down JMS message broker

5. **Secure data deletion**
   - Delete data subject to retention expiration
   - Archive data required for legal retention
   - Securely wipe storage media

6. **Revoke credentials and certificates**
   - Delete JWT secret keys
   - Revoke TLS certificates
   - Remove database passwords from secrets management

7. **Document decommissioning**
   - Record date and method of decommissioning
   - Maintain audit trail per regulatory requirements
   - Archive decommissioning documentation

---

## 11. Technical Description

For detailed technical specifications, including:
- Java application architecture and design patterns
- Database schema and entity relationships
- API specifications (OpenAPI documentation)
- Integration interfaces and message formats
- Security architecture (authentication, authorization, encryption)
- Network architecture and deployment topology
- Performance characteristics and scalability limits
- Monitoring and observability instrumentation

**Location:** Technical documentation maintained in GitLab repository:
`https://gitlab.com/doktor24/services/health-manager`

**How to Access:**
- Software architecture: `docs/handover_architecture.md`
- Software requirements: `docs/software_requirements.md`
- API definitions: OpenAPI annotations in controller classes
- Database schema: Flyway migration files in `src/main/resources/db/migration/`
- Configuration reference: `src/main/resources/application.yml`

Contact Platform24 technical support for access to internal documentation repositories.

---

## 12. Support and Contact

**Manufacturer:** Platform24 / Alerisx
**Address:** SE-116 23 Stockholm, Sweden
**Website:** https://platform24.se

### Reporting Issues

For technical support, operational issues, or questions about system behavior:

- **Healthcare Professionals & Administrators:** Contact your care unit's technical support coordinator
- **Technical Operators:** Open GitLab issue in project repository or contact Platform24 DevOps team
- **Patients:** Contact care provider customer service

### Security Vulnerabilities

If you discover a security vulnerability or data protection issue:

1. **Do not disclose publicly** - protect patient data
2. **Contact Platform24 security team immediately** via secure channel
3. **Provide detailed description** of vulnerability and reproduction steps
4. **Include GitLab repository commit hash** if code-related

Security issues are treated with highest priority and investigated immediately.

### Regulatory Inquiries

For questions about regulatory compliance, medical device classification, or safety reports:

- Contact: Platform24 Regulatory Affairs Department
- Email: regulatory@platform24.se
- Subject line: "health-manager - Regulatory Inquiry"

---

## Document Control

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-03-06 | Initial release - generated from codebase analysis |

---

*This document is part of the regulatory documentation for health-manager.*
*IEC 82304-1:2016 Clause 7.2.2 Compliant*
*Software Safety Classification: Class A*
