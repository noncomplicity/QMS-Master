---
id:
title: "Generate Instructions for Use"
version:
author:
effective_date:
type: "Prompt"
category: "generation"
standard: "IEC 82304-1"
clause: "7.2.2"
inputs: ["accompanying-docs.json", "use-requirements.json"]
outputs: ["Instructions_for_Use.md"]
software_class: "all"
process: "[Software Development Process](../../Canvases/Software%20Development%20Process.canvas)"
requirements:
owner: "[Head of Quality Management](../../Assets/Head%20of%20Quality%20Management.md)"
---

# Generate Instructions for Use

## Context

IEC 82304-1 clause 7.2.2 requires manufacturers to provide Instructions for Use (IFU) containing all information necessary for proper operation of the health software product. This prompt generates a compliant IFU document from extracted data.

## Inputs

- `accompanying-docs.json` — Extracted IFU content from code artifacts
- `use-requirements.json` — Use requirements for cross-reference

## Instructions

Generate a markdown document with the following structure, populating content from the extracted JSON data.

## Output Document Structure

```markdown
---
id:
title: "Instructions for Use - [Product Name]"
version:
author:
effective_date:
type: "InstructionsForUse"
process: "[Document and Record Control](../Canvases/Document%20and%20Record%20Control.canvas)"
requirements:
owner: "[Regulatory Affairs](../Assets/Regulatory%20Affairs.md)"
---

# Instructions for Use

## [Product Name] v[Version]

**Manufacturer:** [Manufacturer Name]
**Document Version:** [IFU Version]
**Effective Date:** [Date]

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

**Product Name:** [Name]
**Version:** [Version]
**Manufacturer:** [Manufacturer]
**Contact:** [Contact Information]
**Website:** [Website URL]

---

## 2. Intended Use

### 2.1 Purpose

[Intended use statement from intended_use.purpose]

### 2.2 Intended Users

[For each user type from user_profile.intended_users:]

**[User Type]**
- Description: [Description]
- Required Skills: [Skills]
- Required Training: [Training]
- Required Knowledge: [Knowledge]

### 2.3 Intended Environment

[From operational_environment:]
- Platforms: [Supported platforms]
- Connectivity: [Network requirements]
- Physical Environment: [Where it can be used]

### 2.4 Essential Functions

[For each function from software_description.essential_functions:]
- **[Function Name]**: [Description]

### 2.5 Contraindications

[From software_description.contraindications]
- [Contraindication 1]
- [Contraindication 2]

### 2.6 Known Limitations

[From software_description.limitations and known_issues]
- [Limitation/Known Issue 1]
- [Limitation/Known Issue 2]

---

## 3. Warnings and Precautions

### 3.1 Safety Warnings

[From warnings_and_notices.general_warnings where type=safety]

⚠️ **[SEVERITY]**: [Warning text]

[Explanation if provided]

### 3.2 Security Warnings

[From warnings_and_notices.general_warnings where type=security]

🔒 **[SEVERITY]**: [Warning text]

[Explanation if provided]

### 3.3 Security Options

[From software_description.security_options]

| Option | Description | Default Setting |
|--------|-------------|-----------------|
| [Option] | [Description] | [Default] |

---

## 4. System Requirements

### 4.1 Operating Systems

[From installation.system_requirements.operating_systems]

| Operating System | Minimum Version |
|-----------------|-----------------|
| [OS] | [Version] |

### 4.2 Hardware Requirements

[From installation.system_requirements.hardware]

**Minimum:**
- Processor: [CPU]
- Memory: [RAM]
- Storage: [Storage]
- Display: [Display]

**Recommended:**
- Processor: [CPU]
- Memory: [RAM]
- Storage: [Storage]

### 4.3 Software Dependencies

[From installation.system_requirements.software_dependencies]

| Dependency | Version | Purpose |
|------------|---------|---------|
| [Name] | [Version] | [Purpose] |

### 4.4 Network Requirements

[From operational_environment.network_requirements]

- Connectivity: [Online/Offline/Hybrid]
- Protocols: [Protocols]
- Bandwidth: [Requirements]

---

## 5. Installation

### 5.1 Who Can Install

[From installation.installer_requirement_statement]

### 5.2 Pre-Installation Checklist

Before installing, ensure:
- [ ] System meets minimum requirements (see Section 4) 🆔 HbmVTy
- [ ] [Other prerequisites from installation] 🆔 pIRuIp

### 5.3 Security Configuration at Installation

[From installation.security_options_at_install]

| Option | Description | Recommended Setting |
|--------|-------------|---------------------|
| [Option] | [Description] | [Recommended] |

### 5.4 Installation Steps

[From installation.installation_steps]

1. [Step 1 action]
   - [Notes if any]
2. [Step 2 action]
   - [Notes if any]

[Or reference: For detailed installation instructions, see [installation_reference]]

---

## 6. Getting Started

### 6.1 Starting the Software

[From startup_procedure.steps]

1. [Step 1 action]
   - Expected result: [What user should see]
2. [Step 2 action]
   - Expected result: [What user should see]

### 6.2 Initial Configuration

[From startup_procedure.initial_configuration]

| Setting | Description | Action Required |
|---------|-------------|-----------------|
| [Setting] | [Description] | [Action] |

### 6.3 First-Time Use

[From startup_procedure.first_run_experience]

---

## 7. Using the Software

### 7.1 Controls

[From operating_instructions.controls]

| Control | Function | Location |
|---------|----------|----------|
| [Control] | [Function] | [Location] |

### 7.2 Displays and Indicators

[From operating_instructions.displays]

| Display | Meaning | Possible Values |
|---------|---------|-----------------|
| [Display] | [Meaning] | [Values] |

### 7.3 Signals

[From operating_instructions.signals]

| Signal | Type | Meaning |
|--------|------|---------|
| [Signal] | [Visual/Auditory/Haptic] | [Meaning] |

### 7.4 Common Operations

[From operating_instructions.operation_sequences]

**[Operation Name]**
1. [Step 1]
2. [Step 2]
3. [Step 3]

### 7.5 Symbols and Icons

[From operating_instructions.symbols]

| Symbol | Meaning |
|--------|---------|
| [Symbol/Icon] | [Meaning] |

### 7.6 Abbreviations

[From operating_instructions.abbreviations]

| Abbreviation | Meaning |
|--------------|---------|
| [Abbrev] | [Full meaning] |

---

## 8. Messages and Troubleshooting

### 8.1 System Messages

[From messages.system_messages]

| Message | Meaning | Action Required |
|---------|---------|-----------------|
| [Message] | [Explanation] | [User action] |

### 8.2 Error Messages

[From messages.error_messages]

| Error | Code | Cause | Solution |
|-------|------|-------|----------|
| [Message] | [Code] | [Cause] | [User action] |

### 8.3 Fault Messages

[From messages.fault_messages]

| Fault | Cause | Action | Support Required? |
|-------|-------|--------|-------------------|
| [Message] | [Cause] | [User action] | [Yes/No] |

---

## 9. Shutting Down

### 9.1 Normal Shutdown

[From shutdown_procedure.steps]

1. [Step 1 action]
   - [Notes if any]
2. [Step 2 action]

### 9.2 Data Preservation

[From shutdown_procedure.data_preservation]

### 9.3 Session Handling

[From shutdown_procedure.session_handling]

---

## 10. Decommissioning

### 10.1 Before Decommissioning

Before decommissioning this software, consider:
[From decommissioning.privacy_considerations and security_considerations]

### 10.2 Exporting Your Data

[From decommissioning.data_export]

- Export formats available: [formats]
- How to export: [procedure]

### 10.3 Deleting Your Data

[From decommissioning.data_deletion]

⚠️ **Warning**: [irreversibility_warning]

- Deletion method: [method]
- Verification: [how to verify deletion]

### 10.4 Decommissioning Steps

[From decommissioning.steps]

1. [Step 1]
2. [Step 2]

---

## 11. Technical Description

For detailed technical specifications, including:
- Platform requirements
- Network specifications
- Security configuration options
- IT-network integration requirements

See: [technical_description_reference.location]

[How to access: technical_description_reference.how_to_access]

---

## 12. Support and Contact

**Manufacturer:** [Manufacturer Name]
**Website:** [Website]
**Contact:** [Contact info]

### Reporting Issues

[From feedback_collection if available, or general contact]

### Security Vulnerabilities

[From SECURITY.md or security contact]

---

## Document Control

| Version | Date | Changes |
|---------|------|---------|
| [Version] | [Date] | [Changes] |

---

*This document is part of the regulatory documentation for [Product Name].*
*IEC 82304-1:2016 Clause 7.2.2 Compliant*
```

## Compliance Checklist

Before finalizing the generated IFU, verify:

- [ ] **7.2.2.1** - All information for proper operation is included 🆔 HCslwf
- [ ] **7.2.2.2 a)** - Intended use is defined 🆔 FCg4Bi
- [ ] **7.2.2.2 b)** - Essential functions are described 🆔 sPQgtC
- [ ] **7.2.2.2 c)** - Security options are listed 🆔 epkQ07
- [ ] **7.2.2.2 d)** - Known issues, limitations, contraindications are documented 🆔 lqZ8PF
- [ ] **7.2.2.3** - All safety and security warnings are included 🆔 bGfzxL
- [ ] **7.2.2.3** - Contextual warnings precede relevant instructions 🆔 xshc48
- [ ] **7.2.2.4 a)** - Installer requirements stated 🆔 aQpMSF
- [ ] **7.2.2.4 b)** - System requirements documented 🆔 C9ih0E
- [ ] **7.2.2.4 c)** - Security options at installation documented 🆔 TtKDP9
- [ ] **7.2.2.4 d)** - Critical dependencies identified 🆔 ZzchqM
- [ ] **7.2.2.4 e)** - Configuration requirements documented 🆔 nb44ta
- [ ] **7.2.2.4 f)** - Interface requirements documented 🆔 McaV5J
- [ ] **7.2.2.4 g)** - Supported platforms detailed 🆔 IG6G4B
- [ ] **7.2.2.4 h)** - Installation instructions provided or referenced 🆔 IgzXox
- [ ] **7.2.2.5** - Start-up procedure documented 🆔 3smpnp
- [ ] **7.2.2.6** - Shutdown procedure documented 🆔 5UTaZp
- [ ] **7.2.2.7** - Operating instructions include controls, displays, signals 🆔 KfyMOS
- [ ] **7.2.2.7** - Symbols and abbreviations explained 🆔 wXZRAa
- [ ] **7.2.2.8** - All messages listed with explanations and actions 🆔 Vn6lkz
- [ ] **7.2.2.9** - Decommissioning procedure documented 🆔 L9eRJf
- [ ] **7.2.2.9** - Data export and deletion procedures included 🆔 KzfS0d
- [ ] **7.2.2.10** - Reference to technical description included 🆔 XkEH6z

## Validation Criteria

- [ ] Document follows the prescribed structure 🆔 tAGE4U
- [ ] All sections are populated from extracted data 🆔 6EqlhY
- [ ] Missing data is flagged with [TODO] markers 🆔 VJb7IL
- [ ] Warnings are properly formatted and prominent 🆔 NQcW0m
- [ ] Cross-references to technical description are correct 🆔 Ax4dJQ
- [ ] Document is readable by intended users (appropriate language level) 🆔 uo68wN
- [ ] All regulatory requirements from 7.2.2 are addressed 🆔 6Gkyml
