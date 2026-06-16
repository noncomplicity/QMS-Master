---
id:
title: "Extract Health Software Product Use Requirements"
version:
author:
effective_date:
type: "Prompt"
category: "extraction"
standard: "IEC 82304-1"
clause: "4.2"
inputs: ["README", "package.json", "documentation", "UI code", "config files", "deployment manifests"]
outputs: ["use-requirements.json"]
software_class: "all"
process: "[Software Development Process](../../Canvases/Software%20Development%20Process.canvas)"
requirements:
owner: "[Head of Quality Management](../../Assets/Head%20of%20Quality%20Management.md)"
---

# Extract Health Software Product Use Requirements

## Context

IEC 82304-1 clause 4.2 requires manufacturers to determine and document use requirements for the health software product. These are product-level requirements that define how the software will be used, by whom, and in what context—distinct from the technical software requirements covered by IEC 62304.

In a code-as-truth paradigm, use requirements are extracted from product documentation, configuration, and user-facing code to ensure documentation reflects actual product capabilities.

## Relationship to IEC 62304

Use requirements (82304-1 clause 4.2) feed into system requirements (clause 4.5), which then become inputs for software requirements (62304 clause 5.2):

```
Use Requirements (82304-1 4.2)
    ↓
System Requirements (82304-1 4.5)
    ↓
Software Requirements (62304 5.2)
```

## Inputs

Analyze the following artifacts:

### Product Documentation
- **README.md**: Product description, intended use, getting started
- **package.json / pyproject.toml**: Product metadata, description
- **CHANGELOG.md**: Feature history, capability evolution
- **User guides**: Detailed usage documentation

### Configuration Files
- **Environment configs**: Supported deployment environments
- **Feature flags**: Configurable capabilities
- **Security configs**: Authentication, authorization settings
- **i18n files**: Supported languages/locales

### User Interface Code
- **UI components**: User-facing functionality
- **Form validations**: User input constraints
- **Error messages**: User feedback mechanisms
- **Accessibility features**: A11y support

### Deployment Artifacts
- **Docker/container configs**: Platform requirements
- **CI/CD pipelines**: Distribution mechanisms
- **Infrastructure as code**: Operational environment

### Regulatory/Compliance
- **Privacy policies**: Data handling requirements
- **Terms of service**: Usage restrictions
- **Compliance documentation**: Regulatory claims

## Instructions

### 1. Extract Intended Use (4.2 a)

Document the product's intended purpose:
- What health-related function does it perform?
- What clinical/health workflow does it support?
- What health outcomes does it aim to achieve?

### 2. Extract User Profile

Identify intended users:
- Professional users (clinicians, administrators)
- Patient/consumer users
- Technical operators
- Required skills, training, knowledge per user type

### 3. Extract Operational Environment

Document where the software operates:
- Supported platforms (OS, browsers, devices)
- Network requirements (online/offline, connectivity)
- Integration dependencies (other systems, APIs)
- Physical environment (clinical, home, mobile)

### 4. Extract Interface Requirements (4.2 b)

Document user interface requirements:
- UI paradigms (web, mobile, desktop, CLI)
- Interaction patterns
- Display requirements
- Input methods supported

### 5. Extract Immunity Requirements (4.2 c)

Document protection from unintended influence:
- Resource isolation (memory, CPU, storage limits)
- Process isolation
- Sandboxing mechanisms
- Failure containment

### 6. Extract Privacy and Security Requirements (4.2 d)

Document security controls:
- **Authorized use**: Access control mechanisms
- **Person authentication**: Login, identity verification
- **Health data integrity**: Data validation, checksums
- **Health data authenticity**: Audit trails, signatures
- **Communication protection**: Encryption, TLS
- **Malware protection**: Security scanning, input validation

### 7. Extract Accompanying Document Requirements (4.2 e)

Identify required documentation:
- Instructions for use content requirements
- Technical documentation requirements
- Training material requirements
- Regulatory labeling requirements

### 8. Extract Lifecycle Support Requirements (4.2 f)

Document support for:
- **f.1) Upgrades**: Migration paths, data compatibility
- **f.2) Rollback**: Version downgrade capability
- **f.3) Security updates**: Patch delivery mechanism
- **f.4) Distribution integrity**: Signing, checksums, secure delivery
- **f.5) Decommissioning**: Data export, deletion, retention

### 9. Extract Regulatory Requirements (4.2 g)

Identify applicable regulations:
- Jurisdictional requirements (MDR, FDA, etc.)
- Data protection (GDPR, HIPAA)
- Protected information rules
- Labeling requirements

### 10. Verify Requirements (4.3)

For each extracted requirement, verify:
- Is it clearly defined?
- Can the product demonstrably meet it?
- Is there evidence of implementation?

## Output Schema

```json
{
  "extraction_metadata": {
    "repository": "<repo-url>",
    "commit": "<commit-hash>",
    "extracted_at": "<ISO-8601-timestamp>",
    "extractor_version": "1.0",
    "standard": "IEC 82304-1:2016"
  },

  "product_identification": {
    "name": "<product name>",
    "version": "<version>",
    "manufacturer": "<manufacturer name>",
    "contact": "<contact info>",
    "website": "<url>"
  },

  "intended_use": {
    "purpose": "<health-related purpose statement>",
    "medical_indication": "<intended medical indication if any>",
    "health_function": "<what health function it performs>",
    "clinical_workflow": "<workflow it supports>",
    "claims": ["<clinical/health claims made>"],
    "contraindications": ["<situations where not to use>"],
    "source": {
      "files": ["<source files>"],
      "evidence": "<quoted text or reference>"
    }
  },

  "user_profile": {
    "intended_users": [
      {
        "user_type": "healthcare_professional | patient | caregiver | administrator | technical_operator",
        "description": "<description of this user type>",
        "required_skills": ["<skills>"],
        "required_training": ["<training>"],
        "required_knowledge": ["<knowledge>"]
      }
    ],
    "restrictions": ["<who should NOT use this>"],
    "source": {
      "files": ["<source files>"],
      "evidence": "<evidence>"
    }
  },

  "operational_environment": {
    "platforms": [
      {
        "type": "web | mobile | desktop | embedded",
        "os": ["<supported OS>"],
        "browsers": ["<supported browsers>"],
        "versions": "<version requirements>",
        "hardware_requirements": "<hardware specs>"
      }
    ],
    "network_requirements": {
      "connectivity": "online | offline | hybrid",
      "protocols": ["<protocols used>"],
      "bandwidth": "<requirements>",
      "latency": "<requirements>"
    },
    "integration_dependencies": [
      {
        "system": "<external system>",
        "purpose": "<why integrated>",
        "required": true
      }
    ],
    "physical_environment": ["clinical | home | mobile | any"],
    "source": {
      "files": ["<source files>"]
    }
  },

  "interface_requirements": [
    {
      "req_id": "USE-IF-<seq>",
      "description": "<interface requirement>",
      "ui_type": "graphical | command_line | api | voice",
      "interaction_pattern": "<how users interact>",
      "display_requirements": "<what must be displayed>",
      "input_methods": ["<supported input methods>"],
      "accessibility": ["<a11y features>"],
      "source": {
        "files": ["<source files>"]
      }
    }
  ],

  "immunity_requirements": [
    {
      "req_id": "USE-IM-<seq>",
      "threat": "<unintended influence type>",
      "protection_mechanism": "<how protected>",
      "implementation": "<where implemented>",
      "source": {
        "files": ["<source files>"]
      }
    }
  ],

  "security_requirements": {
    "authorized_use": {
      "access_control_type": "RBAC | ABAC | ACL | none",
      "roles": ["<defined roles>"],
      "permissions": ["<permission types>"],
      "source": {"files": ["<source files>"]}
    },
    "authentication": {
      "methods": ["password | mfa | sso | biometric | certificate"],
      "session_management": "<session handling>",
      "password_policy": "<policy if applicable>",
      "source": {"files": ["<source files>"]}
    },
    "data_integrity": {
      "validation_mechanisms": ["<validation types>"],
      "checksum_algorithms": ["<algorithms>"],
      "source": {"files": ["<source files>"]}
    },
    "data_authenticity": {
      "audit_trail": true,
      "audit_events": ["<events logged>"],
      "digital_signatures": true,
      "source": {"files": ["<source files>"]}
    },
    "communication_protection": {
      "encryption_in_transit": "TLS | other | none",
      "tls_version": "<minimum version>",
      "encryption_at_rest": "AES | other | none",
      "source": {"files": ["<source files>"]}
    },
    "malware_protection": {
      "input_validation": ["<validation mechanisms>"],
      "dependency_scanning": true,
      "source": {"files": ["<source files>"]}
    }
  },

  "privacy_requirements": [
    {
      "req_id": "USE-PR-<seq>",
      "data_type": "<type of personal/health data>",
      "protection_mechanism": "<how protected>",
      "legal_basis": "<GDPR basis or equivalent>",
      "retention_period": "<how long retained>",
      "deletion_capability": true,
      "source": {"files": ["<source files>"]}
    }
  ],

  "lifecycle_requirements": {
    "upgrade_support": {
      "supported": true,
      "data_migration": "<how data is migrated>",
      "backward_compatibility": "<compatibility approach>",
      "source": {"files": ["<source files>"]}
    },
    "rollback_support": {
      "supported": true,
      "mechanism": "<how rollback works>",
      "data_handling": "<what happens to data>",
      "source": {"files": ["<source files>"]}
    },
    "security_updates": {
      "delivery_mechanism": "<how delivered>",
      "frequency": "<update frequency>",
      "notification": "<how users are notified>",
      "source": {"files": ["<source files>"]}
    },
    "distribution_integrity": {
      "signing": true,
      "checksum": true,
      "secure_delivery": "<delivery mechanism>",
      "source": {"files": ["<source files>"]}
    },
    "decommissioning": {
      "data_export": {
        "supported": true,
        "formats": ["<export formats>"]
      },
      "data_deletion": {
        "supported": true,
        "method": "<deletion method>",
        "verification": "<how verified>"
      },
      "data_retention": {
        "requirements": "<retention requirements>",
        "duration": "<retention period>"
      },
      "source": {"files": ["<source files>"]}
    }
  },

  "regulatory_requirements": [
    {
      "req_id": "USE-REG-<seq>",
      "jurisdiction": "<region/country>",
      "regulation": "<regulation name>",
      "requirement": "<specific requirement>",
      "implementation": "<how implemented>",
      "evidence": "<compliance evidence>",
      "source": {"files": ["<source files>"]}
    }
  ],

  "verification_status": {
    "requirements_defined": true,
    "requirements_achievable": true,
    "verification_method": "inspection | analysis | testing",
    "gaps": [
      {
        "requirement_id": "<req_id>",
        "gap_type": "undefined | unverifiable | not_implemented",
        "description": "<what is missing>",
        "recommendation": "<how to address>"
      }
    ]
  },

  "traceability": {
    "to_system_requirements": [
      {
        "use_requirement": "<USE-* id>",
        "system_requirements": ["<SYS-* ids>"]
      }
    ],
    "to_software_requirements": [
      {
        "use_requirement": "<USE-* id>",
        "software_requirements": ["<SRS-* ids>"]
      }
    ]
  }
}
```

## Compliance Mapping

| Output Element | IEC 82304-1 Clause | Evidence Provided |
|----------------|-------------------|-------------------|
| `intended_use` | 4.2 a) | Requirements addressing intended use |
| `user_profile` | 4.2 a) | Intended user profile |
| `operational_environment` | 4.2 a) | Intended operational environment |
| `interface_requirements` | 4.2 b) | Interface requirements including UI |
| `immunity_requirements` | 4.2 c) | Immunity from unintended influence |
| `security_requirements.authorized_use` | 4.2 d) | Authorized use |
| `security_requirements.authentication` | 4.2 d) | Person authentication |
| `security_requirements.data_integrity` | 4.2 d) | Health data integrity |
| `security_requirements.data_authenticity` | 4.2 d) | Health data authenticity |
| `security_requirements.communication_protection` | 4.2 d) | Communication protection |
| `security_requirements.malware_protection` | 4.2 d) | Malware protection |
| `lifecycle_requirements.upgrade_support` | 4.2 f.1) | Upgrade support |
| `lifecycle_requirements.rollback_support` | 4.2 f.2) | Rollback capability |
| `lifecycle_requirements.security_updates` | 4.2 f.3) | Security patches |
| `lifecycle_requirements.distribution_integrity` | 4.2 f.4) | Distribution integrity |
| `lifecycle_requirements.decommissioning` | 4.2 f.5) | Decommissioning |
| `regulatory_requirements` | 4.2 g) | Regulatory requirements |
| `verification_status` | 4.3 | Verification of use requirements |

## Examples

### Input: README.md

```markdown
# HealthTrack Pro

A mobile application for patients to track daily health metrics
and share them securely with their healthcare providers.

## Features
- Track blood pressure, heart rate, weight, and blood glucose
- Set medication reminders
- Share reports with your doctor via secure link
- Works offline, syncs when connected

## Requirements
- iOS 14+ or Android 10+
- Internet connection for sync (works offline)
```

### Input: package.json

```json
{
  "name": "healthtrack-pro",
  "version": "2.1.0",
  "description": "Patient health metrics tracking app"
}
```

### Output: Extracted Use Requirements (partial)

```json
{
  "intended_use": {
    "purpose": "Enable patients to track daily health metrics and share them securely with healthcare providers",
    "health_function": "Health data collection, monitoring, and secure sharing",
    "clinical_workflow": "Patient self-monitoring with provider data sharing",
    "claims": [
      "Track blood pressure, heart rate, weight, blood glucose",
      "Set medication reminders",
      "Secure sharing with healthcare providers"
    ],
    "source": {
      "files": ["README.md"],
      "evidence": "Patient health metrics tracking app"
    }
  },

  "user_profile": {
    "intended_users": [
      {
        "user_type": "patient",
        "description": "Patients tracking their own health metrics",
        "required_skills": ["Basic smartphone operation"],
        "required_training": ["None - consumer app"],
        "required_knowledge": ["Understanding of own health conditions"]
      },
      {
        "user_type": "healthcare_professional",
        "description": "Healthcare providers receiving shared reports",
        "required_skills": ["Clinical data interpretation"],
        "required_training": ["Medical training"],
        "required_knowledge": ["Patient context"]
      }
    ]
  },

  "operational_environment": {
    "platforms": [
      {
        "type": "mobile",
        "os": ["iOS 14+", "Android 10+"],
        "hardware_requirements": "Standard smartphone"
      }
    ],
    "network_requirements": {
      "connectivity": "hybrid",
      "protocols": ["HTTPS"],
      "bandwidth": "Low - syncs when available"
    }
  },

  "security_requirements": {
    "communication_protection": {
      "encryption_in_transit": "TLS",
      "source": {"files": ["README.md"], "evidence": "share them securely"}
    }
  },

  "lifecycle_requirements": {
    "upgrade_support": {
      "supported": true,
      "data_migration": "Local data preserved on app update"
    }
  }
}
```

## Validation Criteria

- [ ] Intended use is clearly documented with health-related purpose 🆔 XGkYPo
- [ ] All user types are identified with required competencies 🆔 z4m0Mv
- [ ] Operational environment (platforms, connectivity) is specified 🆔 8Hw6jv
- [ ] Security requirements cover all 82304-1 4.2 d) elements 🆔 VozRYb
- [ ] Privacy requirements address health data handling 🆔 Xkvfl0
- [ ] Lifecycle support (upgrade, rollback, decommission) is documented 🆔 mQu6ic
- [ ] Applicable regulatory requirements are identified 🆔 IQYYkl
- [ ] Requirements are traceable to system/software requirements 🆔 vrTyiG
- [ ] Gaps are identified with recommendations 🆔 194WRo
