---
id: 725af57
title: "Technical Description"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "TechnicalDescription"
process: "[Document and Record Control](../../../Canvases/Document%20and%20Record%20Control.canvas)"
requirements:
  - "[IEC 82304-1:2016 Clause 7.2.3](../../../Requirements/IEC%2082304-1.md)"
owner: "[Regulatory Affairs](../../../Assets/Regulatory%20Affairs.md)"
---

# Technical Description

## health-manager v1.0.0-SNAPSHOT

**Manufacturer:** Platform24 / Alerisx
**Contact:** SE-116 23 Stockholm, Sweden
**Website:** https://platform24.se
**Document Version:** 1.0
**Effective Date:** 2026-03-06

**Audience:** This document is intended for responsible organizations (healthcare IT administrators, system integrators) deploying and maintaining this health software product.

---

## Table of Contents

1. [Product Overview](#1-product-overview)
2. [Platform Requirements](#2-platform-requirements)
3. [Transport and Storage](#3-transport-and-storage)
4. [Software Characteristics](#4-software-characteristics)
5. [Installation Requirements](#5-installation-requirements)
6. [Maintenance Requirements](#6-maintenance-requirements)
7. [Security Configuration](#7-security-configuration)
8. [IT-Network Requirements](#8-it-network-requirements)
9. [Appendices](#9-appendices)

---

## 1. Product Overview

**Product Name:** health-manager
**Version:** 1.0.0-SNAPSHOT
**Manufacturer:** Platform24 / Alerisx
**Intended Use:** Healthcare encounter management system enabling practitioners to manage patient appointments, consultations, medical records, prescriptions, referrals, and handovers within a digital health platform.

### 1.1 Document Scope

This Technical Description provides information required by responsible organizations to:
- Assess compatibility with existing IT infrastructure
- Plan installation and deployment
- Configure security settings
- Maintain the software in operation
- Integrate with IT-networks

### 1.2 Related Documents

| Document | Purpose |
|----------|---------|
| Instructions for Use | End-user documentation for healthcare professionals |
| Software Requirements Specification | Detailed functional and technical requirements |
| Software Architecture Document | System architecture and design decisions |
| Risk Analysis (Handover Feature) | Risk assessment for appointment handover functionality |

### 1.3 Medical Device Classification

**Software Safety Classification:** Class A (highest safety level)
**Regulatory Framework:** EU MDR 2017/745, IEC 62304 Class B, IEC 82304-1
**Medical Indication:** General healthcare - supports primary care consultations, follow-up appointments, asynchronous consultations, and care coordination across practitioners and care units.

---

## 2. Platform Requirements

### 2.1 Software Platform Requirements

| Type | Name | Minimum Version | Notes |
|------|------|-----------------|-------|
| Operating System | Linux, Windows, macOS | Any version supporting Java 21 | Containerized deployment recommended via Kubernetes |
| Runtime | Java Runtime Environment (JRE) | 21 | Java 21 LTS required |
| Framework | Spring Boot | 2.x | Embedded in application JAR |
| Database | MySQL or MariaDB | 5.7+ (MySQL) / 10.3+ (MariaDB) | Production database server required |
| Message Broker | Apache Artemis (JMS) | 2.x | External JMS broker required for event-driven messaging |
| Container Runtime | Kubernetes | 1.20+ | Recommended deployment platform |
| Web Server | None (embedded) | N/A | Spring Boot embedded Tomcat |

### 2.2 Hardware Platform Requirements

#### Minimum Requirements (Per Application Instance)

| Component | Specification |
|-----------|---------------|
| Processor | Multi-core x86_64 processor (2 cores minimum) |
| Memory | 2GB heap memory, 4GB total system RAM |
| Storage | 1GB for application JAR and logs |
| Network | 100 Mbps network interface with connectivity to database, message broker, and external services |

#### Recommended Requirements (Production Deployment)

| Component | Specification |
|-----------|---------------|
| Processor | 4+ core x86_64 processor |
| Memory | 4GB heap memory, 8GB total system RAM |
| Storage | 10GB for application, logs, and temporary files |
| Network | 1 Gbps network interface with low-latency connectivity to database and services |

#### Database Server Requirements

| Component | Specification |
|-----------|---------------|
| Processor | 4+ core x86_64 processor |
| Memory | 8GB+ RAM for query caching and buffer pools |
| Storage | 100GB+ SSD storage (scales with appointment and clinical note volume) |
| Network | 1 Gbps network interface |

**Note:** Hardware requirements scale with concurrent user load. The system is designed to handle 10-100 concurrent database connections per instance and process 10,000 appointments per day.

### 2.3 Supported Platforms

| Platform | Supported Versions | Notes |
|----------|-------------------|-------|
| Kubernetes | 1.20 or later | Recommended deployment platform with Helm charts |
| Docker | 19.03 or later | Container runtime for application instances |
| MySQL | 5.7, 8.0 | Production database backend |
| MariaDB | 10.3, 10.4, 10.5, 10.6 | Production database backend (alternative to MySQL) |
| Apache Artemis | 2.x | JMS message broker for event-driven integration |

---

## 3. Transport and Storage

### 3.1 Media Storage Conditions

**Distribution Method:** Digital distribution only. The software is not distributed on physical media.

### 3.2 Digital Distribution

**Distribution Channel:** GitLab CI/CD pipeline builds container images and pushes to container registry.

**Delivery Method:**
- Container images distributed via secure container registry
- Helm charts for Kubernetes deployment
- GitLab repository access for source code and configuration

**Integrity Verification:**
- Container image SHA256 digests provided
- GitLab repository uses commit hashes for version tracking
- TLS/HTTPS used for all download channels

**Storage Requirements for Downloaded Software:**
- Application container image: ~500MB
- Application JAR (if deployed without containers): ~200MB
- Store in secure, backed-up location with access controls

**Environmental Conditions for Storage Media:**
- Digital storage: Standard data center environmental controls
- Temperature: 10-35°C (50-95°F) for server hardware
- Humidity: 20-80% relative humidity (non-condensing)
- No special storage conditions required for software files

---

## 4. Software Characteristics

### 4.1 Data Characteristics

The health-manager system processes clinical and administrative healthcare data with the following characteristics:

| Characteristic | Range | Accuracy | Precision | Units |
|---------------|-------|----------|-----------|-------|
| Patient Personal Number | 10-12 digit Swedish personnummer | Exact | N/A | Text |
| Appointment Date/Time | Valid ISO 8601 timestamps | Exact | Second | DateTime |
| Practitioner Credentials | Healthcare professional IDs | Exact | N/A | Text |
| Clinical Notes | Free text, 0-65,535 characters | N/A | N/A | Text (varchar/text) |
| Handover Notes | Free text, 0-1,024 characters | N/A | N/A | Text (varchar) |
| Diagnoses (ICD-10) | Valid ICD-10 codes | Exact | N/A | Text |
| Prescriptions | Medication codes and dosages | Exact | As per medication database | Text/Numeric |
| Payment Amounts | 0-10,000 SEK typical | 2 decimal places | 0.01 SEK | Currency (SEK) |

**Data Retention:**
- Clinical data: Retained per Swedish healthcare record retention requirements (typically 10 years after last patient contact)
- Audit logs: Retained per organizational policy (minimum 3 years recommended)
- Appointment data: Retained indefinitely unless explicitly deleted via data erasure procedures

**Data Classification:**
- Patient health data: Secrecy classification C, Sensitivity level 2 (highest protection)
- Audit events: Immutable, append-only
- Clinical notes: Encrypted in transit, access-controlled

### 4.2 Performance Characteristics

| Characteristic | Specification |
|---------------|---------------|
| Concurrent Users | Up to 100 concurrent practitioners per instance |
| Database Connections | 10-100 connections (configurable pool size) |
| Appointments per Day | 10,000 appointments processable per day per instance |
| Response Time (API) | <2 seconds for 95% of requests under normal load |
| Query Timeout | 30 seconds maximum per database query |
| Remote Service Timeout | 10 seconds connect timeout, 5 minutes read timeout |
| Cache Performance | In-memory caching with configurable TTL and max entries |

**Scalability:**
- Horizontal scaling: Deploy multiple application instances behind load balancer
- Database scaling: Master-slave replication supported
- Message broker: Topic-based pub/sub supports multiple consumers

---

## 5. Installation Requirements

### 5.1 Special Installation Requirements

**Prerequisites:**
- Java 21 JRE installed and configured in system PATH
- Access to MySQL/MariaDB database server with administrative privileges
- Access to Apache Artemis JMS broker
- Network connectivity to external services (Directory, Bookings, MeetingPlace, Interviewer, Notifier, Actions, etc.)
- Valid JWT signing secret configured
- TLS/SSL certificates for HTTPS communication (if not behind load balancer/ingress)

**Integration Dependencies (Required):**
The following external services MUST be accessible for the health-manager to operate:

1. **Directory Service** - Practitioner and patient identity verification
2. **Bookings Service** - Appointment scheduling integration
3. **MeetingPlace Service** - Video consultation rooms and messaging
4. **Interviewer Service** - Patient triage questionnaires
5. **Notifier Service** - Push notifications and SMS alerts
6. **Actions Service** - Task management
7. **Medication Search Service** - Medication database lookup
8. **Care Planner Service** - Care planning and coordination
9. **Service Requests Service** - Service request management
10. **Object Storage Service** - File and document storage

**Optional Integrations:**
- Alfa E-Recept (Swedish e-prescription system)
- TakeCare Xchange (Swedish regional health records)
- Klarna Payment Service

### 5.2 Installation Restrictions

**Not Supported:**
- Installation on operating systems without Java 21 support
- Deployment without access to MySQL/MariaDB database
- Deployment without JMS message broker
- Air-gapped environments (requires internet connectivity for external service integrations)

**Security Restrictions:**
- Database user must have CREATE, ALTER, DROP, INSERT, UPDATE, DELETE, SELECT privileges for schema migrations
- Application service account must have appropriate IAM/RBAC permissions in Kubernetes
- Firewall rules must allow outbound HTTPS connections to external services

**Configuration Restrictions:**
- Database connection pool: Minimum 10, maximum 100 connections (configurable but recommended range)
- Query timeout: 30 seconds (not configurable, hardcoded for safety)
- JWT secret: Must be securely generated and stored (minimum 256-bit entropy recommended)

### 5.3 Pre-Installation Checklist

- [ ] Java 21 JRE installed and verified (`java -version`) 🆔 fAove4
- [ ] MySQL/MariaDB database server accessible and administrative credentials available 🆔 J0BBze
- [ ] Apache Artemis JMS broker accessible and connection details available 🆔 RMIZoZ
- [ ] Network firewall rules configured to allow required outbound connections 🆔 i0aYq1
- [ ] TLS certificates available (if not using Kubernetes ingress/load balancer) 🆔 uyBhxm
- [ ] Database schema created (Flyway migrations will populate schema on first startup) 🆔 dKDsLI
- [ ] Configuration file prepared with correct service URLs, database credentials, JWT secret 🆔 112ypY
- [ ] Kubernetes cluster prepared with appropriate namespaces and resource quotas 🆔 UJsOJp
- [ ] Helm charts reviewed and values.yaml customized for environment 🆔 3BPNGn
- [ ] External service endpoints verified and tested (Directory, Bookings, etc.) 🆔 SrorqF
- [ ] Monitoring and logging infrastructure ready (Prometheus, log aggregation) 🆔 BopSN9

---

## 6. Maintenance Requirements

### 6.1 Log File Maintenance

The health-manager service generates application logs that must be monitored and rotated to prevent disk exhaustion.

| Log | Location | Action | Frequency |
|-----|----------|--------|-----------|
| Application logs | `/var/log/health-manager/application.log` (or configured path) | Monitor for errors, rotate when >100MB | Daily review, rotate weekly |
| Audit logs | Database table `audit_log` (or equivalent) | Review for security events, archive after retention period | Weekly review, archive quarterly |
| JMS message logs | Artemis broker logs | Monitor for message delivery failures | Daily review |
| Database query logs | MySQL slow query log | Review for performance issues, optimize queries | Weekly review |
| Spring Boot Actuator logs | `/actuator/logfile` endpoint | Monitor health check failures | Continuous monitoring via Prometheus |

**Log Rotation Recommendations:**
- Application logs: Compress and archive after 7 days, delete after 90 days
- Audit logs: Export to secure archive storage, retain per regulatory requirements (3+ years)
- Configure log levels appropriately (INFO for production, DEBUG for troubleshooting only)

### 6.2 Database Maintenance

| Task | Frequency | Procedure |
|------|-----------|-----------|
| Database backup | Daily | Full backup of MySQL/MariaDB database, store in secure offsite location |
| Transaction log backup | Hourly | Incremental transaction log backup for point-in-time recovery |
| Index optimization | Monthly | Run `OPTIMIZE TABLE` on appointment, clinical note, and audit tables |
| Schema migration verification | After each deployment | Verify Flyway migration success via `/actuator/flyway` endpoint |
| Connection pool monitoring | Continuous | Monitor active connections via `/actuator/metrics/hikari.connections.active` |
| Query performance analysis | Weekly | Review slow query log and optimize problematic queries |

**Database Growth Estimates:**
- Appointments table: ~1KB per appointment record
- Clinical notes: Variable (1KB - 64KB per note)
- Audit events: ~500 bytes per event
- Total growth: ~10GB per 100,000 appointments with full clinical documentation

### 6.3 Storage Media Maintenance

**Requirements:**
- Database storage: Monitor disk usage, maintain minimum 20% free space
- Application logs: Rotate and archive to prevent disk exhaustion
- Temporary files: Clear temporary upload directories weekly

**Maintenance Procedures:**
- Monitor disk I/O performance for database storage
- Upgrade to SSD storage if disk I/O becomes bottleneck
- Scale database storage capacity based on appointment volume growth

### 6.4 Software Updates

Refer to the manufacturer's release notes and update procedures for:

**Security Patches:**
- Apply security patches within 30 days of release for high-severity vulnerabilities
- Emergency patches for critical vulnerabilities deployed within 7 days
- Test patches in non-production environment before production deployment

**Feature Updates:**
- Review release notes for breaking changes and API versioning
- Plan update during maintenance window to minimize disruption
- Test database schema migrations in non-production environment
- Coordinate with external service updates (Directory, Bookings, etc.)

**Bug Fixes:**
- Apply bug fixes per organizational change control procedures
- Monitor GitLab CI/CD pipeline for dependency check failures indicating vulnerabilities

**Update Procedure:**
1. Review release notes and migration guide
2. Backup database before deployment
3. Deploy to test environment and verify functionality
4. Schedule maintenance window for production deployment
5. Deploy using Kubernetes rolling update strategy (zero-downtime)
6. Monitor application health via `/actuator/health` endpoint
7. Verify database schema version via `/actuator/flyway` endpoint
8. Rollback if critical issues detected

---

## 7. Security Configuration

### 7.1 Configurable Security Options

The following security options can be configured by the responsible organization:

#### Network/Ports Configuration

| Option | Description | Default | Recommendations |
|--------|-------------|---------|-----------------|
| `server.port` | HTTP listener port | 8080 | Use 8080 for containerized deployments, 443 for direct TLS |
| `management.server.port` | Actuator/metrics port | 8081 | Separate port for monitoring, restrict access to internal network only |
| TLS/SSL enabled | Enable HTTPS | Disabled (delegated to ingress) | Enable if not behind load balancer, use TLS 1.2+ |
| Allowed origins (CORS) | Cross-origin request sources | Not configured | Restrict to known frontend domains only |

#### Encryption Settings

| Option | Description | Default | Recommendations |
|--------|-------------|---------|-----------------|
| Database encryption | Encrypt database connections | TLS if supported by DB | Enable TLS for database connections in production |
| Data-at-rest encryption | Database tablespace encryption | Not configured | Enable MySQL/MariaDB tablespace encryption for sensitive data |
| JWT signing algorithm | JWT token signature algorithm | HS256 (symmetric) | Use RS256 (asymmetric) for production, rotate keys annually |
| External service TLS | TLS for HTTP clients | Enabled (HTTPS URLs) | Verify TLS certificate validation enabled, no self-signed certs |

#### Authentication/Login Settings

| Option | Description | Default | Recommendations |
|--------|-------------|---------|-----------------|
| `jwt.secret` | JWT signing secret | Not configured (required) | Generate strong secret (256-bit), store securely (Kubernetes secret) |
| `jwt.issuer` | JWT token issuer | Not configured | Set to organizational identity provider |
| JWT token expiration | Token validity duration | Configured by issuer | Recommend 1-hour access tokens, 24-hour refresh tokens |
| Role-based access control | RBAC enforcement | Enabled (hardcoded) | Do not disable, roles: PRACTITIONER, PATIENT, ADMIN, SECRETARY, SYSTEM |

#### Audit/Logging Settings

| Option | Description | Default | Recommendations |
|--------|-------------|---------|-----------------|
| Audit logging | Log security-sensitive operations | Enabled (`@Audit` annotations) | Do not disable, required for compliance |
| Audit throttle | Rate limit audit logs | `Throttle.HOURLY` for sensitive ops | Keep default to prevent audit log flooding |
| Log level | Application logging verbosity | INFO | Use INFO for production, DEBUG only for troubleshooting |
| Sensitive data masking | Mask patient data in logs | Not configured | Enable logging filters to mask personnummer, names in logs |

### 7.2 Security Failure Behavior

When a security failure is detected, the health-manager service implements the following behavior:

**Authentication Failure:**
- **Description:** JWT token validation fails (expired, invalid signature, missing claims)
- **Behavior:** HTTP 401 Unauthorized response returned, request rejected, no data access granted
- **Patient Care Impact:** Practitioner cannot access appointment until re-authenticated, may delay consultation
- **Data Impact:** No data breach, access denied by design
- **Clinical Workflow Impact:** Practitioner must re-authenticate, typically <30 seconds delay

**Authorization Failure:**
- **Description:** User lacks required role or patient relationship for requested operation
- **Behavior:** HTTP 403 Forbidden response returned, audit log created, request rejected
- **Patient Care Impact:** Minimal if practitioner attempts to access wrong patient; proper access controls prevent unauthorized viewing
- **Data Impact:** No data breach, access denied by design
- **Clinical Workflow Impact:** Practitioner redirected to authorized patient list

**Database Connection Failure:**
- **Description:** Connection pool exhausted or database unreachable
- **Behavior:** HTTP 503 Service Unavailable response returned after connection timeout (5 seconds), request fails, circuit breaker may trip
- **Patient Care Impact:** **HIGH** - Practitioners cannot access appointments, consultations blocked
- **Data Impact:** No data loss, transactions rolled back automatically
- **Clinical Workflow Impact:** **CRITICAL** - System unavailable until database restored, practitioners must use backup clinical workflow

**External Service Authentication Failure (e.g., TakeCare x509 certificate):**
- **Description:** Client certificate validation fails for TakeCare Xchange integration
- **Behavior:** SOAP request fails, error logged, clinical note export fails gracefully
- **Patient Care Impact:** Low - Clinical note not exported to regional EHR, practitioner can export manually later
- **Data Impact:** No data loss, export retried on next attempt
- **Clinical Workflow Impact:** Minor - Practitioner must verify note exported or export manually

**JMS Message Broker Failure:**
- **Description:** Apache Artemis connection fails or message delivery times out
- **Behavior:** Event publishing fails, error logged, application continues processing but notifications may not be sent
- **Patient Care Impact:** Moderate - Practitioners may not receive handover notifications, patients may not receive appointment updates
- **Data Impact:** No data loss, events persisted to database, can be republished
- **Clinical Workflow Impact:** Moderate - Manual notification fallback required until JMS restored

### 7.3 Platform Change Instructions

When changes occur to the underlying platform (operating system patches, antivirus updates, firewall changes, firmware updates), the responsible organization must:

**Operating System Patches:**
1. Test patches in non-production environment first
2. Verify Java 21 JRE compatibility after OS patch
3. Monitor application health after production deployment
4. Verify database connectivity and external service integrations

**Antivirus/Malware Protection Updates:**
1. Exclude Java process and application JAR from real-time scanning (performance)
2. Schedule antivirus scans during low-traffic periods
3. Monitor application performance for impact of antivirus scanning

**Firewall Changes:**
1. Verify required outbound HTTPS connections remain allowed (see section 8.2 for port list)
2. Test connectivity to external services after firewall changes
3. Monitor application logs for connection timeout errors
4. Do not block database port (3306 MySQL/MariaDB) or JMS port (61616 Artemis)

**Database Server Updates (MySQL/MariaDB):**
1. Verify Hibernate compatibility with new database version
2. Test Flyway schema migrations in non-production environment
3. Backup database before production upgrade
4. Monitor query performance after upgrade (query optimizer changes)

**Java Runtime Updates:**
1. Only use Java 21 LTS versions (do not upgrade to Java 22+ without manufacturer approval)
2. Test application with new Java patch version before production deployment
3. Monitor for JVM garbage collection performance changes
4. Verify TLS/SSL cipher suites remain compatible

**Container Runtime/Kubernetes Updates:**
1. Test application deployment on new Kubernetes version in non-production
2. Verify Helm chart compatibility
3. Monitor pod startup times and resource utilization after update
4. Verify service mesh (if used) compatibility

**Impact Assessment Required:**
- If any platform change causes application health check failures, rollback immediately
- If query timeouts increase significantly, review database performance
- If external service integrations fail, verify network connectivity and TLS certificates
- If JMS message delivery fails, verify Artemis broker connectivity

---

## 8. IT-Network Requirements

### 8.1 Network Characteristics Required

For the software to achieve its intended purpose, the IT-network must provide:

| Characteristic | Requirement |
|---------------|-------------|
| Bandwidth | Minimum 100 Mbps per application instance for typical load; 1 Gbps recommended for production with video consultation traffic |
| Latency | <50ms latency to database server, <100ms latency to external services within same region |
| Availability | 99.9% uptime recommended (maximum 8.76 hours downtime per year) |
| Packet Loss | <0.1% packet loss for database and JMS connections |
| DNS Resolution | Reliable DNS resolution for external service hostnames |
| Time Synchronization | NTP time synchronization within ±1 second for audit log timestamps |

**Network Stability:**
- Network interruptions >10 seconds may cause database connection pool exhaustion
- Temporary network outages <10 seconds handled gracefully by connection retry logic
- Long-running transactions may timeout during network instability (30 second query timeout)

### 8.2 Technical Specifications

#### Network Ports

| Port | Protocol | Direction | Purpose |
|------|----------|-----------|---------|
| 8080 | HTTP/HTTPS | Inbound | Application REST API (if not behind load balancer) |
| 8081 | HTTP | Inbound | Spring Boot Actuator / Prometheus metrics (internal monitoring only) |
| 3306 | TCP (MySQL) | Outbound | Database connection (MySQL/MariaDB) |
| 61616 | TCP (JMS) | Outbound | Apache Artemis message broker (default port) |
| 443 | HTTPS | Outbound | External service integrations (Directory, Bookings, MeetingPlace, Interviewer, Notifier, Actions, etc.) |
| 443 | HTTPS | Outbound | Alfa E-Recept API (Swedish e-prescription system) |
| 443 | HTTPS | Outbound | TakeCare Xchange SOAP API (Swedish regional health records) |
| 443 | HTTPS | Outbound | Klarna Payment API |
| 443 | HTTPS | Outbound | Medication Search Service API |
| 443 | HTTPS | Outbound | Object Storage Service (via ZeroMQ over HTTPS) |
| 123 | UDP (NTP) | Outbound | Time synchronization |
| 53 | UDP (DNS) | Outbound | DNS resolution |

**Firewall Rules:**
- Allow inbound on port 8080 (or load balancer port) from authorized networks only
- Allow inbound on port 8081 from monitoring systems only (internal network)
- Allow outbound on port 3306 to database server IP/hostname
- Allow outbound on port 61616 to JMS broker IP/hostname
- Allow outbound on port 443 to all external service hostnames (wildcard or specific domains)

#### Protocols Used

**Application Layer:**
- HTTP/1.1 and HTTP/2 for REST API
- HTTPS/TLS 1.2+ for all external service communication
- WebSocket for real-time features (if applicable)
- SOAP over HTTPS for TakeCare Xchange integration
- JMS (Java Message Service) over TCP for event messaging

**Transport Layer:**
- TCP for database, JMS, and HTTP/HTTPS connections
- UDP for DNS and NTP

**Security Protocols:**
- TLS 1.2 or TLS 1.3 for encrypted communication
- X.509 certificates for TakeCare Xchange client authentication
- JWT (JSON Web Tokens) for API authentication and authorization

#### Network Services Required

**Internal Services:**
- DNS server for hostname resolution
- NTP server for time synchronization
- Database server (MySQL/MariaDB)
- JMS message broker (Apache Artemis)

**External Services (Internet Connectivity):**
- Directory Service
- Bookings Service
- MeetingPlace Service
- Interviewer Service
- Notifier Service
- Actions Service
- Medication Search Service
- Care Planner Service
- Service Requests Service
- Object Storage Service
- Alfa E-Recept (optional)
- TakeCare Xchange (optional)
- Klarna Payment Service (optional)

### 8.3 Security Specifications

| Aspect | Requirement |
|--------|-------------|
| Encryption | TLS 1.2 or TLS 1.3 for all external HTTPS connections; TLS for database connections recommended |
| Authentication | JWT bearer token authentication for API access; X.509 client certificate for TakeCare integration; Database username/password authentication |
| Malware Protection | Antivirus software on server infrastructure recommended; Application-level input validation and SQL injection protection via parameterized queries |
| Intrusion Detection | Network-level IDS/IPS recommended to detect anomalous traffic patterns |
| Certificate Validation | TLS certificate validation enabled, no self-signed certificates allowed for production external services |
| Network Segmentation | Application tier, database tier, and external integrations should be network-segmented with firewall rules |

### 8.4 Information Flow

| From | To | Data Type | Protocol |
|------|------|-----------|----------|
| Client (Frontend) | health-manager REST API | Appointment requests, clinical data updates | HTTPS/TLS |
| health-manager | MySQL/MariaDB Database | Appointment data, clinical notes, audit logs | TCP (MySQL protocol) with optional TLS |
| health-manager | Apache Artemis JMS | Appointment events, handover events, notifications | TCP (JMS protocol) |
| health-manager | Directory Service | Practitioner/patient identity verification | HTTPS REST API |
| health-manager | Bookings Service | Appointment scheduling integration | HTTPS REST API |
| health-manager | MeetingPlace Service | Video consultation room creation, messages | HTTPS REST API |
| health-manager | Interviewer Service | Patient triage questionnaire data | HTTPS REST API |
| health-manager | Notifier Service | Push notification requests, SMS requests | HTTPS REST API |
| health-manager | Alfa E-Recept | Electronic prescription submissions | HTTPS REST API |
| health-manager | TakeCare Xchange | Clinical note exports (case notes) | HTTPS SOAP with X.509 client certificate |
| health-manager | Klarna Payment Service | Payment processing requests | HTTPS REST API |
| health-manager | Medication Search Service | Medication database queries | HTTPS REST API |
| health-manager | Object Storage Service | File upload/download | HTTPS (ZeroMQ) |
| Prometheus | health-manager Actuator | Metrics scraping | HTTP (port 8081) |

### 8.5 Network Failure Hazards

The following hazardous situations may result from IT-network failures:

| Failure Scenario | Hazardous Situation | Mitigation |
|-----------------|---------------------|------------|
| **Complete network outage** | Practitioners cannot access patient appointments, consultations cannot be conducted, prescriptions cannot be submitted | **Hazard Severity: HIGH**. Mitigation: Implement network redundancy, maintain backup clinical workflow procedures, configure automatic failover to backup network path. Recovery time objective: <15 minutes. |
| **Database connection failure** | Appointments cannot be created, updated, or retrieved; clinical notes cannot be saved | **Hazard Severity: HIGH**. Mitigation: Database connection pool retry logic (5-second timeout), database server high availability (master-slave replication), backup database server with automatic failover. Recovery time objective: <5 minutes. |
| **JMS broker failure** | Handover notifications not delivered, practitioners not alerted to new appointments, patient notifications not sent | **Hazard Severity: MODERATE**. Mitigation: JMS broker high availability cluster, message persistence to disk, manual notification fallback procedures. Recovery time objective: <30 minutes. |
| **Directory Service failure** | Practitioner credentials cannot be validated, patient identity verification fails, authorization checks fail | **Hazard Severity: HIGH**. Mitigation: Cache practitioner credentials with TTL, implement graceful degradation with cached authorization data, establish maximum cache age (e.g., 1 hour). Recovery time objective: <15 minutes. |
| **Notifier Service failure** | Practitioners not notified of handover requests, patients not notified of appointment updates | **Hazard Severity: MODERATE**. Mitigation: Retry logic for notification requests, manual notification fallback (email, phone call), visual indicators in practitioner UI for pending handovers. Recovery time objective: <1 hour. |
| **Alfa E-Recept failure** | Electronic prescriptions cannot be submitted to national prescription system | **Hazard Severity: MODERATE**. Mitigation: Queue prescriptions for retry when service restored, manual prescription fallback (paper prescription), alert practitioner to submission failure. Recovery time objective: <4 hours. |
| **TakeCare Xchange failure** | Clinical notes cannot be exported to regional health record system | **Hazard Severity: LOW**. Mitigation: Queue note exports for retry, manual export fallback, alert practitioner to export failure. Recovery time objective: <24 hours. |
| **Network latency spike (>500ms)** | Database query timeouts, application response times exceed acceptable limits, practitioners experience delays | **Hazard Severity: LOW**. Mitigation: Query timeout protection (30 seconds), connection timeout protection (5 seconds), cache frequently accessed data, optimize database queries. |
| **Intermittent connectivity** | Database connections fail intermittently, external service requests fail sporadically | **Hazard Severity: MODERATE**. Mitigation: Connection retry logic, circuit breaker pattern for external services, health check monitoring to detect intermittent failures. |
| **DNS resolution failure** | External services unreachable, database hostname not resolved | **Hazard Severity: HIGH**. Mitigation: Use IP addresses for critical services, configure backup DNS servers, cache DNS resolution results. Recovery time objective: <5 minutes. |

### 8.6 Important Warnings for Responsible Organizations

⚠️ **Warning**: Execution of this health software on an IT-network could result in previously unidentified risks to patients, users, or third parties.

The responsible organization is advised to:
1. **Identify, analyze, evaluate, and control** risks specific to your IT-network configuration and healthcare environment
2. **Recognize that subsequent changes** to the IT-network (hardware, software, configuration, topology) could introduce new risks requiring additional risk analysis
3. **Establish network monitoring** to detect failures and performance degradation that could impact patient care
4. **Implement redundancy** for critical network infrastructure (database server, message broker, internet connectivity)
5. **Define backup clinical workflows** for use when the health-manager service is unavailable due to network failures
6. **Test disaster recovery procedures** regularly to ensure rapid restoration of service
7. **Coordinate with external service providers** (Directory, Bookings, etc.) to understand their availability SLAs and failure modes

### 8.7 Network Changes Requiring Risk Assessment

The following types of network changes may affect the safety and security of this software and require formal risk assessment:

- [x] **Changes in IT-network configuration** (firewall rules, routing tables, VLANs, network segmentation)
- [x] **Addition of hardware or software to the IT-network** (new servers, network appliances, intrusion detection systems, load balancers)
- [x] **Removal of items from the IT-network** (decommissioned servers, retired network segments)
- [x] **Updates to hardware or software on the IT-network** (operating system patches, firmware updates, network device updates)
- [x] **Upgrades to hardware or software on the IT-network** (database server version upgrades, Kubernetes version upgrades, Java runtime upgrades)
- [x] **Changes to external service endpoints** (URL changes, IP address changes, API version upgrades)
- [x] **Changes to network bandwidth or latency** (network provider changes, geographic relocation of services)
- [x] **Changes to security infrastructure** (TLS certificate updates, JWT signing key rotation, firewall vendor changes)

**Risk Assessment Procedure:**
1. Document the planned network change
2. Analyze potential impact on health-manager service availability and performance
3. Identify new failure modes introduced by the change
4. Evaluate risk severity and probability using organizational risk matrix
5. Implement risk controls (testing, monitoring, rollback procedures)
6. Test change in non-production environment
7. Monitor production deployment for adverse effects
8. Update risk analysis documentation

The responsible organization should assess the impact of such changes on the health software product and coordinate with the manufacturer if significant risks are identified.

---

## 9. Appendices

### 9.1 Glossary

| Term | Definition |
|------|------------|
| **Responsible Organization** | Entity accountable for the use and proper operation of the health software product within their healthcare environment (e.g., hospital IT department, clinic management) |
| **Appointment Handover** | Transfer of responsibility for a patient appointment from one healthcare practitioner to another within the health-manager system |
| **Care Unit** | Organizational unit within a healthcare provider (e.g., clinic, department) that manages practitioners and patient appointments |
| **Safety Class A** | Highest software safety classification level per Platform24 catalog-info.yaml, indicating critical role in patient care |
| **IEC 62304 Class B** | Medical device software safety class per IEC 62304:2006, indicating moderate risk to patient safety |
| **Personnummer** | Swedish personal identity number (10-12 digits) used to uniquely identify patients |
| **E-Recept** | Swedish national electronic prescription system (Alfa E-Recept integration) |
| **TakeCare Xchange** | Swedish regional health information exchange for clinical note sharing between healthcare providers |
| **JMS (Java Message Service)** | Enterprise messaging API for event-driven communication via Apache Artemis broker |
| **JWT (JSON Web Token)** | Token-based authentication and authorization mechanism used for API security |
| **RBAC (Role-Based Access Control)** | Security model assigning permissions based on user roles (PRACTITIONER, PATIENT, ADMIN, SECRETARY, SYSTEM) |
| **QueryDSL** | Type-safe query construction framework for building dynamic database queries |
| **Flyway** | Database schema migration tool for versioned schema changes |

### 9.2 Referenced Standards

| Standard | Title |
|----------|-------|
| IEC 82304-1:2016 | Health software — Part 1: General requirements for product safety |
| IEC 62304:2006+AMD1:2015 | Medical device software — Software life cycle processes |
| ISO 13485:2016 | Medical devices — Quality management systems — Requirements for regulatory purposes |
| ISO 14971:2019 | Medical devices — Application of risk management to medical devices |
| EU MDR 2017/745 | Medical Device Regulation (European Union) |
| GDPR (EU 2016/679) | General Data Protection Regulation |
| Patientdatalagen (SFS 2008:355) | Swedish Patient Data Act |

### 9.3 Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-03-06 | Platform24 / Alerisx | Initial Technical Description for health-manager v1.0.0-SNAPSHOT |

---

## Document Control

**Document ID:** [Auto-populated on git commit]
**Classification:** Technical Documentation (Confidential)
**Review Cycle:** Annual review or upon significant system changes

---

*This document is part of the regulatory documentation for health-manager.*
*IEC 82304-1:2016 Clause 7.2.3 Compliant*
