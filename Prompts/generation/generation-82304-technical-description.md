---
id: 725af57
title: "generation 82304 technical description"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Prompt"
category: "generation"
standard: "IEC 82304-1"
clause: "7.2.3"
inputs: ["accompanying-docs.json", "system-requirements.json", "architecture.json"]
outputs: ["Technical_Description.md"]
software_class: "all"
process: "[Software Development Process](../../Canvases/Software%20Development%20Process.canvas)"
requirements:
owner: "[Head of Quality Management](../../Assets/Head%20of%20Quality%20Management.md)"
---

# Generate Technical Description

## Context

IEC 82304-1 clause 7.2.3 requires manufacturers to provide a Technical Description containing all data essential for safe and secure operation, including platform requirements, maintenance requirements, security configuration options, and IT-network specifications.

The Technical Description is intended for responsible organizations (e.g., hospital IT departments) who deploy and maintain the health software product.

## Inputs

- `accompanying-docs.json` — Extracted technical content
- `system-requirements.json` — System requirements
- `architecture.json` — Software architecture (optional, for additional detail)

## Instructions

Generate a markdown document with the following structure, populating content from the extracted JSON data.

## Output Document Structure

```markdown
---
id: 725af57
title: "generation 82304 technical description"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "TechnicalDescription"
process: "[Document and Record Control](../Canvases/Document%20and%20Record%20Control.canvas)"
requirements:
owner: "[Regulatory Affairs](../Assets/Regulatory%20Affairs.md)"
---

# Technical Description

## [Product Name] v[Version]

**Manufacturer:** [Manufacturer Name]
**Document Version:** [TD Version]
**Effective Date:** [Date]

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

**Product Name:** [Name]
**Version:** [Version]
**Manufacturer:** [Manufacturer]
**Intended Use:** [Brief intended use statement]

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
| Instructions for Use | End-user documentation |
| [Other documents] | [Purpose] |

---

## 2. Platform Requirements

### 2.1 Software Platform Requirements

[From technical_description.platform_requirements.software]

| Type | Name | Minimum Version | Notes |
|------|------|-----------------|-------|
| Operating System | [OS] | [Version] | [Notes] |
| Runtime | [Runtime] | [Version] | [Notes] |
| Database | [DB] | [Version] | [Notes] |

### 2.2 Hardware Platform Requirements

[From technical_description.platform_requirements.hardware]

#### Minimum Requirements

| Component | Specification |
|-----------|---------------|
| Processor | [CPU] |
| Memory | [RAM] |
| Storage | [Storage] |
| Network | [Network] |

#### Recommended Requirements

| Component | Specification |
|-----------|---------------|
| Processor | [CPU] |
| Memory | [RAM] |
| Storage | [Storage] |
| Network | [Network] |

### 2.3 Supported Platforms

[From installation.supported_platforms_detail]

| Platform | Supported Versions | Notes |
|----------|-------------------|-------|
| [Platform] | [Versions] | [Notes] |

---

## 3. Transport and Storage

[From technical_description.transport_and_storage]

### 3.1 Media Storage Conditions

If the software is distributed on physical media:

| Condition | Permissible Range |
|-----------|------------------|
| Temperature | [Temperature range] |
| Humidity | [Humidity range] |
| Other | [Other conditions] |

### 3.2 Digital Distribution

[Distribution method and integrity verification]

---

## 4. Software Characteristics

[From technical_description.software_characteristics]

### 4.1 Data Characteristics

| Characteristic | Range | Accuracy | Precision | Units |
|---------------|-------|----------|-----------|-------|
| [Characteristic] | [Range] | [Accuracy] | [Precision] | [Units] |

### 4.2 Performance Characteristics

[From system_requirements.platform_requirements.load_requirements]

| Characteristic | Specification |
|---------------|---------------|
| Concurrent Users | [Number] |
| Transactions/Second | [TPS] |
| Data Volume | [Volume] |
| Response Time | [Time] |

---

## 5. Installation Requirements

[From technical_description.installation_requirements]

### 5.1 Special Installation Requirements

[From installation_requirements.special_requirements]

- [Requirement 1]
- [Requirement 2]

### 5.2 Installation Restrictions

[From installation_requirements.restrictions]

- [Restriction 1]
- [Restriction 2]

### 5.3 Pre-Installation Checklist

- [ ] Hardware meets minimum requirements 🆔 oDsLyI
- [ ] Operating system is supported version 🆔 lFO0sH
- [ ] Required software dependencies are installed 🆔 8RcIbA
- [ ] Network configuration is complete 🆔 kP7BnR
- [ ] Security certificates are available (if required) 🆔 Au76HT
- [ ] Database is provisioned (if required) 🆔 LHSELQ

---

## 6. Maintenance Requirements

[From technical_description.maintenance_requirements]

### 6.1 Log File Maintenance

[From maintenance_requirements.log_files]

| Log | Location | Action | Frequency |
|-----|----------|--------|-----------|
| [Log name] | [Location] | [Check/Clear/Rotate] | [Frequency] |

### 6.2 Database Maintenance

[From maintenance_requirements.database_maintenance]

| Task | Frequency | Procedure |
|------|-----------|-----------|
| [Task] | [Frequency] | [Procedure] |

### 6.3 Storage Media Maintenance

[From maintenance_requirements.storage_media]

- Requirements: [Requirements]
- Maintenance: [Maintenance needed]

### 6.4 Software Updates

Refer to the manufacturer's release notes and update procedures for:
- Security patches
- Feature updates
- Bug fixes

---

## 7. Security Configuration

[From technical_description.security_configuration]

### 7.1 Configurable Security Options

The following security options can be configured by the responsible organization:

[From security_configuration.configurable_options]

#### Network/Ports Configuration

| Option | Description | Default | Recommendations |
|--------|-------------|---------|-----------------|
| [Option] | [Description] | [Default] | [Recommendations] |

#### Encryption Settings

| Option | Description | Default | Recommendations |
|--------|-------------|---------|-----------------|
| [Option] | [Description] | [Default] | [Recommendations] |

#### Authentication/Login Settings

| Option | Description | Default | Recommendations |
|--------|-------------|---------|-----------------|
| [Option] | [Description] | [Default] | [Recommendations] |

#### Audit/Logging Settings

| Option | Description | Default | Recommendations |
|--------|-------------|---------|-----------------|
| [Option] | [Description] | [Default] | [Recommendations] |

### 7.2 Security Failure Behavior

[From security_configuration.security_failure_behavior]

When a security failure is detected:

**Description:** [What the software does]

**Impact Assessment:**
- Patient Care Impact: [Impact]
- Data Impact: [Impact]
- Clinical Workflow Impact: [Impact]

### 7.3 Platform Change Instructions

[From security_configuration.platform_change_instructions]

When changes occur to the underlying platform (operating system patches, antivirus updates, firewall changes, firmware updates):

[Instructions for handling platform changes]

---

## 8. IT-Network Requirements

[From technical_description.it_network]

### 8.1 Network Characteristics Required

[From it_network.network_characteristics]

For the software to achieve its intended purpose, the IT-network must provide:

| Characteristic | Requirement |
|---------------|-------------|
| Bandwidth | [Bandwidth requirements] |
| Latency | [Latency requirements] |
| Availability | [Availability requirements] |
| [Other] | [Other requirements] |

### 8.2 Technical Specifications

#### Network Ports

[From it_network.technical_specifications.ports]

| Port | Protocol | Direction | Purpose |
|------|----------|-----------|---------|
| [Port] | [Protocol] | [Inbound/Outbound/Both] | [Purpose] |

#### Protocols Used

[From it_network.technical_specifications.protocols]

- [Protocol 1]
- [Protocol 2]

#### Network Services Required

[From it_network.technical_specifications.services]

- [Service 1]
- [Service 2]

### 8.3 Security Specifications

[From it_network.security_specifications]

| Aspect | Requirement |
|--------|-------------|
| Encryption | [Encryption requirements] |
| Authentication | [Network auth requirements] |
| Malware Protection | [Malware protection requirements] |

### 8.4 Information Flow

[From it_network.information_flow]

| From | To | Data Type | Protocol |
|------|------|-----------|----------|
| [Source] | [Destination] | [Data type] | [Protocol] |

### 8.5 Network Failure Hazards

[From it_network.network_failure_hazards]

The following hazardous situations may result from IT-network failures:

| Failure Scenario | Hazardous Situation | Mitigation |
|-----------------|---------------------|------------|
| [Failure] | [Hazard] | [Mitigation] |

### 8.6 Important Warnings for Responsible Organizations

[From it_network.responsible_organization_warnings]

⚠️ **Warning**: Execution of this health software on an IT-network could result in previously unidentified risks to patients, users, or third parties.

The responsible organization is advised to:
1. Identify, analyze, evaluate, and control these risks
2. Recognize that subsequent changes to the IT-network could introduce new risks requiring additional analysis

### 8.7 Network Changes Requiring Risk Assessment

[From it_network.network_change_types]

The following types of network changes may affect the safety and security of this software:

- [ ] Changes in IT-network configuration 🆔 3TQzZZ
- [ ] Addition of hardware or software to the IT-network 🆔 bjk9lt
- [ ] Removal of items from the IT-network 🆔 kbtdl0
- [ ] Updates to hardware or software on the IT-network 🆔 g1WFO6
- [ ] Upgrades to hardware or software on the IT-network 🆔 j1uiTG

The responsible organization should assess the impact of such changes on the health software product.

---

## 9. Appendices

### 9.1 Glossary

| Term | Definition |
|------|------------|
| Responsible Organization | Entity accountable for the use and proper operation of the health software product |
| [Other terms] | [Definitions] |

### 9.2 Referenced Standards

| Standard | Title |
|----------|-------|
| IEC 82304-1:2016 | Health software — Part 1: General requirements for product safety |
| IEC 62304:2006+AMD1:2015 | Medical device software — Software life cycle processes |

### 9.3 Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| [Version] | [Date] | [Author] | [Changes] |

---

## Document Control

**Document ID:** [ID]
**Classification:** [Classification]
**Review Cycle:** [Cycle]

---

*This document is part of the regulatory documentation for [Product Name].*
*IEC 82304-1:2016 Clause 7.2.3 Compliant*
```

## Compliance Checklist

Before finalizing the generated Technical Description, verify:

- [ ] **7.2.3.1 a)** - Software platform requirements documented 🆔 pRbEBL
- [ ] **7.2.3.1 b)** - Hardware platform requirements documented (detailed) 🆔 q2aT6o
- [ ] **7.2.3.1 c)** - Transport and storage conditions documented 🆔 ZBO6WS
- [ ] **7.2.3.1 d)** - Software characteristics (range, accuracy, precision) documented 🆔 CsxkPA
- [ ] **7.2.3.1 e)** - Special installation requirements and restrictions documented 🆔 vHZXV1
- [ ] **7.2.3.1 f)** - Maintenance requirements documented 🆔 0nUnfI
- [ ] **7.2.3.1 g)** - Security configuration options documented 🆔 hjeHok
- [ ] **7.2.3.1 g)** - Security failure behavior documented with impact assessment 🆔 eXOMjx
- [ ] **7.2.3.1** - Platform change instructions provided 🆔 QLME2C
- [ ] **7.2.3.2 a)** - IT-network characteristics required documented 🆔 QPojKz
- [ ] **7.2.3.2 b)** - IT-network technical specifications (ports, protocols) documented 🆔 7ZUZkp
- [ ] **7.2.3.2 b)** - IT-network security specifications documented 🆔 rQ0hRA
- [ ] **7.2.3.2 c)** - Information flow between systems documented 🆔 MAempV
- [ ] **7.2.3.2** - Network failure hazards documented 🆔 j98D1A
- [ ] **7.2.3.2** - Responsible organization warned about network risks 🆔 IotJ37
- [ ] **7.2.3.2** - Responsible organization advised to assess risks 🆔 kCc0mk
- [ ] **7.2.3.2** - Network change types requiring assessment listed 🆔 VclJHX

## Validation Criteria

- [ ] Document follows the prescribed structure 🆔 76ditQ
- [ ] All sections are populated from extracted data 🆔 dWyydY
- [ ] Missing data is flagged with [TODO] markers 🆔 Ay0gQl
- [ ] Technical specifications are accurate and complete 🆔 GKgBKc
- [ ] Security configuration options are actionable 🆔 xOMf2L
- [ ] IT-network requirements are specific enough for implementation 🆔 0Kd8tx
- [ ] Warnings are prominent and clear 🆔 NT45G9
- [ ] Document is appropriate for technical audience (IT administrators) 🆔 zKmkGG
- [ ] All regulatory requirements from 7.2.3 are addressed 🆔 CKoOT1
