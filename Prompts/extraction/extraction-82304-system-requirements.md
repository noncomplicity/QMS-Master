---
id: 725af57
title: "extraction 82304 system requirements"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Prompt"
category: "extraction"
standard: "IEC 82304-1"
clause: "4.5"
inputs: ["use-requirements.json", "architecture code", "config files", "security implementations", "i18n files"]
outputs: ["system-requirements.json"]
software_class: "all"
process: "[Software Development Process](../../Canvases/Software%20Development%20Process.canvas)"
requirements:
owner: "[Head of Quality Management](../../Assets/Head%20of%20Quality%20Management.md)"
---

# Extract Health Software Product System Requirements

## Context

IEC 82304-1 clause 4.5 requires manufacturers to specify and document system requirements for the health software product. System requirements bridge use requirements (what users need) and software requirements (how it's implemented).

System requirements must meet the use requirements established in clause 4.2 and include specific elements like interoperability, security features, and platform requirements.

## Relationship to Other Requirements

```
Use Requirements (82304-1 4.2)
    ↓ [system requirements must meet use requirements]
System Requirements (82304-1 4.5) ← THIS PROMPT
    ↓ [primary design input for software development]
Software Requirements (62304 5.2)
```

## Inputs

### From Previous Extraction
- `use-requirements.json` — Use requirements from clause 4.2 extraction

### Architecture and Design
- **System architecture files**: High-level design documents
- **API specifications**: OpenAPI, GraphQL schemas
- **Database schemas**: Data model definitions
- **Integration configs**: External system connections

### Platform and Infrastructure
- **Container configs**: Docker, Kubernetes manifests
- **Infrastructure code**: Terraform, CloudFormation
- **Build configs**: Webpack, Vite, build tool configs
- **Dependency manifests**: package.json, requirements.txt

### Security Implementation
- **Auth implementations**: Login, session, token handling
- **Authorization code**: Permission checks, RBAC
- **Encryption configs**: TLS, key management
- **Audit logging**: Event tracking code

### Localization
- **i18n files**: Translation files, locale configs
- **Locale handling**: Date, number, currency formatting

### UI Specifications
- **Design system**: Component library, style guides
- **Accessibility**: ARIA, a11y implementations
- **Responsive design**: Breakpoints, layouts

## Instructions

### 1. Extract Interoperability Requirements (4.5 a)

Document how the system interoperates with other systems:
- Standards supported (HL7 FHIR, DICOM, etc.)
- API protocols (REST, GraphQL, SOAP)
- Data exchange formats (JSON, XML, CSV)
- Integration patterns (sync, async, event-driven)

### 2. Extract Localization Requirements (4.5 b)

Document language and locale support:
- Supported languages
- Locale-specific formatting (dates, numbers, currency)
- RTL support if applicable
- Translation coverage

### 3. Link Risk Control Measures (4.5 c)

From risk analysis, document system-level controls:
- Safety-critical functions
- Fail-safe behaviors
- Warning/alarm generation
- Data validation rules

### 4. Extract User Interface Specification (4.5 d)

Document UI system requirements:
- Design system/component library
- Interaction patterns
- Accessibility compliance (WCAG level)
- Responsive breakpoints
- Performance requirements (load time, responsiveness)

### 5. Extract Platform Requirements (4.5 e)

Document software and hardware platforms:
- Operating system requirements
- Runtime dependencies (Node.js, Python, JVM versions)
- Memory requirements
- Storage requirements
- CPU/performance requirements
- Expected load and scaling requirements

### 6. Extract Security Compromise Detection (4.5 f)

Document features for detecting security compromises:
- Intrusion detection
- Anomaly logging
- Failed authentication tracking
- Audit trail mechanisms
- Alert mechanisms

### 7. Extract Essential Function Protection (4.5 g)

Document how essential functions remain protected:
- Graceful degradation
- Fallback mechanisms
- Core function isolation
- Recovery procedures

### 8. Extract Configuration Recovery (4.5 h)

Document configuration management:
- Backup mechanisms
- Restore procedures
- Config versioning
- Privileged user access requirements

### 9. Verify Requirements (4.6)

For each requirement, verify:
- No contradictions (4.6 a)
- Unambiguous (4.6 b)
- Testable (4.6 c)
- Uniquely identifiable (4.6 d)

## Output Schema

```json
{
  "extraction_metadata": {
    "repository": "<repo-url>",
    "commit": "<commit-hash>",
    "extracted_at": "<ISO-8601-timestamp>",
    "extractor_version": "1.0",
    "standard": "IEC 82304-1:2016",
    "use_requirements_reference": "<use-requirements.json commit>"
  },

  "interoperability_requirements": [
    {
      "req_id": "SYS-INT-<seq>",
      "title": "<short title>",
      "description": "<what interoperability is required>",
      "standard": "<standard name if applicable>",
      "protocol": "<communication protocol>",
      "data_format": "<data format>",
      "direction": "inbound | outbound | bidirectional",
      "systems": ["<external systems>"],
      "implementation": {
        "files": ["<implementation files>"],
        "endpoints": ["<API endpoints>"]
      },
      "source": {
        "files": ["<source files>"]
      },
      "verification": {
        "method": "testing | inspection | analysis",
        "test_cases": ["<test references>"]
      }
    }
  ],

  "localization_requirements": {
    "supported_languages": [
      {
        "code": "<ISO 639-1 code>",
        "name": "<language name>",
        "coverage": "<percentage or description>",
        "rtl": false
      }
    ],
    "locale_formatting": {
      "dates": ["<supported formats>"],
      "numbers": ["<supported formats>"],
      "currency": ["<supported currencies>"]
    },
    "translation_mechanism": "<how translations are managed>",
    "source": {
      "files": ["<i18n source files>"]
    }
  },

  "risk_control_requirements": [
    {
      "req_id": "SYS-RC-<seq>",
      "title": "<short title>",
      "description": "<risk control measure at system level>",
      "hazard_reference": "<hazard ID from risk analysis>",
      "control_type": "prevention | detection | mitigation",
      "implementation": {
        "mechanism": "<how implemented>",
        "files": ["<implementation files>"]
      },
      "verification": {
        "method": "<verification method>",
        "evidence": "<verification evidence>"
      },
      "source": {
        "files": ["<source files>"]
      }
    }
  ],

  "user_interface_requirements": {
    "design_system": {
      "name": "<design system name>",
      "version": "<version>",
      "documentation": "<url or path>"
    },
    "components": [
      {
        "req_id": "SYS-UI-<seq>",
        "component": "<component name>",
        "purpose": "<what it does>",
        "accessibility": ["<a11y features>"],
        "source": {"files": ["<component files>"]}
      }
    ],
    "accessibility": {
      "standard": "WCAG 2.1 | WCAG 2.2 | Section 508",
      "level": "A | AA | AAA",
      "features": ["<specific features>"],
      "testing": "<how tested>"
    },
    "responsive_design": {
      "breakpoints": [
        {
          "name": "mobile | tablet | desktop",
          "min_width": "<pixels>",
          "max_width": "<pixels>"
        }
      ]
    },
    "performance": {
      "initial_load_time": "<target>",
      "interaction_response": "<target>",
      "animation_fps": "<target>"
    },
    "source": {
      "files": ["<UI source files>"]
    }
  },

  "platform_requirements": {
    "software_platforms": [
      {
        "req_id": "SYS-PLAT-<seq>",
        "type": "operating_system | runtime | browser | database",
        "name": "<platform name>",
        "minimum_version": "<version>",
        "recommended_version": "<version>",
        "rationale": "<why required>",
        "source": {"files": ["<config files>"]}
      }
    ],
    "hardware_requirements": {
      "minimum": {
        "cpu": "<CPU requirement>",
        "memory": "<RAM requirement>",
        "storage": "<storage requirement>",
        "network": "<network requirement>"
      },
      "recommended": {
        "cpu": "<CPU requirement>",
        "memory": "<RAM requirement>",
        "storage": "<storage requirement>",
        "network": "<network requirement>"
      }
    },
    "load_requirements": {
      "concurrent_users": "<expected concurrent users>",
      "transactions_per_second": "<expected TPS>",
      "data_volume": "<expected data volume>",
      "scaling_strategy": "horizontal | vertical | auto"
    },
    "source": {
      "files": ["<infrastructure files>"]
    }
  },

  "security_detection_requirements": [
    {
      "req_id": "SYS-SEC-DET-<seq>",
      "title": "<short title>",
      "description": "<what security event is detected>",
      "detection_mechanism": "<how detected>",
      "logging": {
        "event_type": "<event name>",
        "data_captured": ["<data elements>"],
        "retention": "<retention period>"
      },
      "alerting": {
        "enabled": true,
        "channels": ["<notification channels>"],
        "severity": "info | warning | critical"
      },
      "response": "<action taken on detection>",
      "source": {
        "files": ["<implementation files>"]
      }
    }
  ],

  "essential_function_protection": [
    {
      "req_id": "SYS-EFP-<seq>",
      "essential_function": "<function name>",
      "description": "<what the function does>",
      "protection_mechanism": "<how protected when security compromised>",
      "degradation_behavior": "<how it degrades gracefully>",
      "fallback": "<fallback mechanism if any>",
      "recovery": "<recovery procedure>",
      "source": {
        "files": ["<implementation files>"]
      }
    }
  ],

  "configuration_recovery": {
    "backup": {
      "mechanism": "<backup method>",
      "frequency": "<backup frequency>",
      "storage": "<where stored>",
      "retention": "<how long kept>"
    },
    "restore": {
      "procedure": "<restore steps>",
      "rto": "<recovery time objective>",
      "rpo": "<recovery point objective>"
    },
    "versioning": {
      "enabled": true,
      "mechanism": "<how versions tracked>",
      "rollback_capability": true
    },
    "privileged_access": {
      "required_role": "<role required for recovery>",
      "authentication": "<additional auth required>",
      "audit_logging": true
    },
    "source": {
      "files": ["<config management files>"]
    }
  },

  "verification_status": {
    "total_requirements": 0,
    "verified_requirements": 0,
    "requirements_by_status": {
      "verified": 0,
      "pending_verification": 0,
      "failed_verification": 0
    },
    "verification_issues": [
      {
        "req_id": "<requirement ID>",
        "issue_type": "contradiction | ambiguous | untestable | not_unique",
        "description": "<issue description>",
        "recommendation": "<how to fix>"
      }
    ]
  },

  "traceability": {
    "to_use_requirements": [
      {
        "system_requirement": "<SYS-* id>",
        "use_requirements": ["<USE-* ids>"],
        "coverage": "full | partial"
      }
    ],
    "to_software_requirements": [
      {
        "system_requirement": "<SYS-* id>",
        "software_requirements": ["<SRS-* ids>"]
      }
    ],
    "gaps": [
      {
        "use_requirement": "<USE-* id>",
        "gap": "<missing system requirement>",
        "recommendation": "<action to address>"
      }
    ]
  }
}
```

## Compliance Mapping

| Output Element | IEC 82304-1 Clause | Evidence Provided |
|----------------|-------------------|-------------------|
| `interoperability_requirements` | 4.5 a) | Inter-operability |
| `localization_requirements` | 4.5 b) | Localization and language support |
| `risk_control_requirements` | 4.5 c) | Risk control measures at system level |
| `user_interface_requirements` | 4.5 d) | User interface specification |
| `platform_requirements` | 4.5 e) | Platform requirements for expected load/performance |
| `security_detection_requirements` | 4.5 f) | Security compromise detection |
| `essential_function_protection` | 4.5 g) | Essential function protection |
| `configuration_recovery` | 4.5 h) | Configuration retention and recovery |
| `verification_status` | 4.6 | Requirement verification |
| `traceability.to_use_requirements` | 4.5 | System requirements meet use requirements |

## Examples

### Input: Docker Compose Configuration

```yaml
# docker-compose.yml
services:
  app:
    image: healthtrack:${VERSION}
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgres://...
    deploy:
      resources:
        limits:
          memory: 512M
          cpus: '0.5'
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000/health"]
      interval: 30s
```

### Input: i18n Configuration

```typescript
// src/i18n/config.ts
export const supportedLocales = ['en', 'de', 'fr', 'es'] as const;
export const defaultLocale = 'en';

export const dateFormats = {
  en: 'MM/DD/YYYY',
  de: 'DD.MM.YYYY',
  fr: 'DD/MM/YYYY',
  es: 'DD/MM/YYYY'
};
```

### Output: Extracted System Requirements (partial)

```json
{
  "localization_requirements": {
    "supported_languages": [
      {"code": "en", "name": "English", "coverage": "100%"},
      {"code": "de", "name": "German", "coverage": "100%"},
      {"code": "fr", "name": "French", "coverage": "100%"},
      {"code": "es", "name": "Spanish", "coverage": "100%"}
    ],
    "locale_formatting": {
      "dates": ["MM/DD/YYYY", "DD.MM.YYYY", "DD/MM/YYYY"]
    },
    "source": {
      "files": ["src/i18n/config.ts"]
    }
  },

  "platform_requirements": {
    "hardware_requirements": {
      "minimum": {
        "memory": "512M",
        "cpu": "0.5 cores"
      }
    },
    "source": {
      "files": ["docker-compose.yml"]
    }
  },

  "security_detection_requirements": [
    {
      "req_id": "SYS-SEC-DET-001",
      "title": "Health Check Monitoring",
      "description": "System monitors application health at 30-second intervals",
      "detection_mechanism": "HTTP health endpoint polling",
      "alerting": {
        "enabled": true,
        "channels": ["container orchestrator"],
        "severity": "critical"
      },
      "source": {
        "files": ["docker-compose.yml"]
      }
    }
  ]
}
```

## Validation Criteria

- [ ] All use requirements have corresponding system requirements 🆔 xhgZEl
- [ ] Interoperability standards and protocols are documented 🆔 6TtkSU
- [ ] Supported languages and locales are specified 🆔 ZMvZji
- [ ] Risk control measures trace to risk analysis 🆔 OIMlb7
- [ ] UI accessibility requirements specify compliance level 🆔 GK1UhV
- [ ] Platform requirements include minimum and recommended specs 🆔 sTgYUZ
- [ ] Security detection mechanisms are documented 🆔 CJwHy7
- [ ] Essential function protection addresses security compromise scenarios 🆔 q2fbLt
- [ ] Configuration recovery procedures are defined 🆔 WRzEOd
- [ ] All requirements are uniquely identified (SYS-* IDs) 🆔 AwAJEL
- [ ] Verification issues are documented with recommendations 🆔 2IRrSS
