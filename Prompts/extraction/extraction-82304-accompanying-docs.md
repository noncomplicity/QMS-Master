---
id:
title: "Extract Accompanying Document Content"
version:
author:
effective_date:
type: "Prompt"
category: "extraction"
standard: "IEC 82304-1"
clause: "7"
inputs: ["README", "docs/", "help text", "error messages", "config files", "deployment manifests"]
outputs: ["accompanying-docs.json"]
software_class: "all"
process: "[Software Development Process](../../Canvases/Software%20Development%20Process.canvas)"
requirements:
owner: "[Head of Quality Management](../../Assets/Head%20of%20Quality%20Management.md)"
---

# Extract Accompanying Document Content

## Context

IEC 82304-1 clause 7 requires manufacturers to provide accompanying documents including Instructions for Use (IFU) and Technical Description. This prompt extracts content from code artifacts that feeds into these documents.

In code-as-truth, much IFU and technical description content already exists in:
- README files
- API documentation
- Configuration documentation
- Error message catalogs
- Help text and tooltips
- Deployment guides

This prompt consolidates this content for regulatory document generation.

## Outputs Generated From This Extraction

```
accompanying-docs.json
    ↓ [generation prompts]
├── Instructions for Use (7.2.2)
└── Technical Description (7.2.3)
```

## Inputs

### Product Identity
- **package.json / manifest**: Name, version, manufacturer
- **LICENSE**: License and copyright
- **CHANGELOG**: Version history

### User Documentation
- **README.md**: Product overview, quick start
- **docs/**: User guides, tutorials
- **CONTRIBUTING.md**: Installation from source
- **Help content**: In-app help, tooltips

### Technical Documentation
- **API docs**: OpenAPI, JSDoc, docstrings
- **Architecture docs**: System design
- **Deployment docs**: Installation, configuration
- **Infrastructure code**: Docker, K8s, Terraform

### Runtime Content
- **Error messages**: i18n files, error catalogs
- **UI labels**: Button text, form labels
- **Notifications**: Alert messages, warnings
- **Status messages**: Progress, completion messages

### Security Documentation
- **Security policies**: Auth config, permissions
- **SECURITY.md**: Vulnerability reporting
- **Privacy documentation**: Data handling

## Instructions

### 1. Extract Product Identification (7.1)

Document product identity:
- Manufacturer name and contact
- Product name/trademark
- Version identifier
- Website

### 2. Extract General Accompanying Document Info (7.2.1)

Document:
- All document versions
- Required skills/training for users
- Location/environment restrictions
- System interface requirements
- Platform requirements

### 3. Extract Instructions for Use Content (7.2.2)

#### 3.1 General (7.2.2.1)
- Information for proper operation
- Installation instructions location

#### 3.2 Software Description (7.2.2.2)
- Intended use statement
- Essential functions description
- Security options
- Known issues/limitations
- Contraindications

#### 3.3 Safety/Security Warnings (7.2.2.3)
- All warnings and notices
- Safety-related warnings
- Security-related warnings
- Contextual warnings (per-feature)

#### 3.4 Installation (7.2.2.4)
- Who can install (user vs. authorized person)
- System requirements
- Security options at install time
- Dependencies
- Configuration requirements
- Interface requirements
- Supported platforms
- Installation steps

#### 3.5 Start-up Procedure (7.2.2.5)
- Steps to bring software into operation
- Initial configuration
- First-run experience

#### 3.6 Shutdown Procedure (7.2.2.6)
- Safe shutdown steps
- Data preservation
- Session handling

#### 3.7 Operating Instructions (7.2.2.7)
- Control functions
- Display explanations
- Signal meanings
- Operation sequences
- Symbols and abbreviations

#### 3.8 Messages (7.2.2.8)
- System messages catalog
- Error messages with explanations
- Fault messages with causes
- User actions for each message

#### 3.9 Decommissioning (7.2.2.9)
- Decommissioning steps
- Data export procedures
- Data deletion procedures
- Privacy/security considerations

#### 3.10 Technical Description Reference (7.2.2.10)
- Where to find technical description

### 4. Extract Technical Description Content (7.2.3)

#### 4.1 General (7.2.3.1)
- Platform requirements (software)
- Platform requirements (hardware)
- Supported platforms detail
- Transport/storage conditions
- Software characteristics (ranges, accuracy, precision)
- Installation requirements/restrictions
- Maintenance requirements
- Security configuration options
- Security compromise behavior

#### 4.2 IT-Network Requirements (7.2.3.2)
- Network characteristics required
- Network technical specifications
- Security specifications
- Information flow between systems
- Hazardous situations from network failure
- Warnings about network risks
- Network change impact warnings

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
    "manufacturer": {
      "name": "<manufacturer name>",
      "address": "<address>",
      "contact": "<contact info>",
      "website": "<url>",
      "source": {"files": ["<source files>"]}
    },
    "product": {
      "name": "<product name>",
      "trademark": "<trademark if different>",
      "version": "<version>",
      "release_date": "<date>",
      "source": {"files": ["<source files>"]}
    },
    "document_versions": {
      "ifu_version": "<IFU version>",
      "technical_description_version": "<TD version>"
    }
  },

  "general_requirements": {
    "required_skills": [
      {
        "user_type": "<user type>",
        "skills": ["<required skills>"],
        "training": ["<required training>"],
        "knowledge": ["<required knowledge>"]
      }
    ],
    "location_restrictions": ["<where can/cannot be used>"],
    "environment_restrictions": ["<environmental requirements>"],
    "interface_requirements": ["<system interfaces required>"],
    "platform_requirements_summary": "<platform summary>",
    "source": {"files": ["<source files>"]}
  },

  "instructions_for_use": {
    "software_description": {
      "intended_use": "<intended use statement>",
      "essential_functions": [
        {
          "function": "<function name>",
          "description": "<description>"
        }
      ],
      "security_options": [
        {
          "option": "<security option>",
          "description": "<description>",
          "default": "<default setting>"
        }
      ],
      "known_issues": [
        {
          "issue": "<issue description>",
          "workaround": "<workaround if any>"
        }
      ],
      "limitations": ["<limitations>"],
      "contraindications": ["<contraindications>"],
      "source": {"files": ["<source files>"]}
    },

    "warnings_and_notices": {
      "general_warnings": [
        {
          "warning_id": "WARN-<seq>",
          "type": "safety | security",
          "severity": "danger | warning | caution | notice",
          "text": "<warning text>",
          "explanation": "<explanation if not self-explanatory>"
        }
      ],
      "contextual_warnings": [
        {
          "warning_id": "WARN-<seq>",
          "context": "<where this warning applies>",
          "feature": "<feature it relates to>",
          "type": "safety | security",
          "text": "<warning text>",
          "precedes_action": "<action it precedes>"
        }
      ],
      "source": {"files": ["<source files>"]}
    },

    "installation": {
      "installer_requirement": "user | manufacturer | authorized_person",
      "installer_requirement_statement": "<statement about who can install>",
      "system_requirements": {
        "operating_systems": [
          {
            "os": "<OS name>",
            "version": "<version requirement>"
          }
        ],
        "hardware": {
          "processor": "<CPU requirement>",
          "memory": "<RAM requirement>",
          "storage": "<storage requirement>",
          "display": "<display requirement>",
          "other": ["<other requirements>"]
        },
        "software_dependencies": [
          {
            "name": "<dependency name>",
            "version": "<version>",
            "purpose": "<why needed>"
          }
        ]
      },
      "security_options_at_install": [
        {
          "option": "<option name>",
          "description": "<description>",
          "recommended_setting": "<recommended>"
        }
      ],
      "critical_dependencies": [
        {
          "dependency": "<dependency name>",
          "criticality": "<why critical>"
        }
      ],
      "configuration_requirements": [
        {
          "setting": "<setting name>",
          "description": "<description>",
          "required": true,
          "default": "<default value>"
        }
      ],
      "interface_requirements": [
        {
          "interface": "<interface name>",
          "type": "required | optional",
          "description": "<description>"
        }
      ],
      "supported_platforms_detail": [
        {
          "platform": "<platform name>",
          "versions": "<supported versions>",
          "notes": "<any notes>"
        }
      ],
      "installation_steps": [
        {
          "step": 1,
          "action": "<action description>",
          "notes": "<any notes>"
        }
      ],
      "installation_reference": "<where full instructions are>",
      "source": {"files": ["<source files>"]}
    },

    "startup_procedure": {
      "steps": [
        {
          "step": 1,
          "action": "<action description>",
          "expected_result": "<what user should see>"
        }
      ],
      "initial_configuration": [
        {
          "setting": "<setting>",
          "description": "<description>",
          "action": "<action required>"
        }
      ],
      "first_run_experience": "<description of first run>",
      "source": {"files": ["<source files>"]}
    },

    "shutdown_procedure": {
      "steps": [
        {
          "step": 1,
          "action": "<action description>",
          "notes": "<any notes>"
        }
      ],
      "data_preservation": "<how data is preserved>",
      "session_handling": "<what happens to sessions>",
      "source": {"files": ["<source files>"]}
    },

    "operating_instructions": {
      "controls": [
        {
          "control": "<control name>",
          "function": "<what it does>",
          "location": "<where in UI>"
        }
      ],
      "displays": [
        {
          "display": "<display element>",
          "meaning": "<what it shows>",
          "values": ["<possible values>"]
        }
      ],
      "signals": [
        {
          "signal": "<signal name>",
          "type": "visual | auditory | haptic",
          "meaning": "<what it indicates>"
        }
      ],
      "operation_sequences": [
        {
          "operation": "<operation name>",
          "steps": ["<sequence of steps>"]
        }
      ],
      "symbols": [
        {
          "symbol": "<symbol or icon>",
          "meaning": "<meaning>"
        }
      ],
      "abbreviations": [
        {
          "abbreviation": "<abbreviation>",
          "meaning": "<full meaning>"
        }
      ],
      "source": {"files": ["<source files>"]}
    },

    "messages": {
      "system_messages": [
        {
          "message_id": "MSG-SYS-<seq>",
          "message": "<message text>",
          "type": "informational | confirmation | status",
          "explanation": "<explanation>",
          "user_action": "<action required if any>"
        }
      ],
      "error_messages": [
        {
          "message_id": "MSG-ERR-<seq>",
          "message": "<error message>",
          "code": "<error code if any>",
          "cause": "<what causes this>",
          "user_action": "<what user should do>",
          "severity": "critical | error | warning"
        }
      ],
      "fault_messages": [
        {
          "message_id": "MSG-FAULT-<seq>",
          "message": "<fault message>",
          "cause": "<what causes this>",
          "user_action": "<what user should do>",
          "requires_support": true
        }
      ],
      "source": {"files": ["<source files>"]}
    },

    "decommissioning": {
      "steps": [
        {
          "step": 1,
          "action": "<action description>"
        }
      ],
      "data_export": {
        "available": true,
        "formats": ["<export formats>"],
        "procedure": "<how to export>"
      },
      "data_deletion": {
        "available": true,
        "method": "<deletion method>",
        "verification": "<how to verify deletion>",
        "irreversibility_warning": "<warning about permanent deletion>"
      },
      "privacy_considerations": ["<privacy considerations>"],
      "security_considerations": ["<security considerations>"],
      "source": {"files": ["<source files>"]}
    },

    "technical_description_reference": {
      "location": "<where to find technical description>",
      "how_to_access": "<how to access it>"
    }
  },

  "technical_description": {
    "platform_requirements": {
      "software": [
        {
          "type": "operating_system | runtime | database | other",
          "name": "<name>",
          "version": "<version requirement>",
          "notes": "<additional notes>"
        }
      ],
      "hardware": {
        "minimum": {
          "processor": "<CPU>",
          "memory": "<RAM>",
          "storage": "<storage>",
          "network": "<network>"
        },
        "recommended": {
          "processor": "<CPU>",
          "memory": "<RAM>",
          "storage": "<storage>",
          "network": "<network>"
        }
      },
      "source": {"files": ["<source files>"]}
    },

    "transport_and_storage": {
      "media_conditions": {
        "temperature": "<temperature range>",
        "humidity": "<humidity range>",
        "other": ["<other conditions>"]
      },
      "source": {"files": ["<source files>"]}
    },

    "software_characteristics": [
      {
        "characteristic": "<characteristic name>",
        "range": "<value range>",
        "accuracy": "<accuracy>",
        "precision": "<precision>",
        "units": "<units>"
      }
    ],

    "installation_requirements": {
      "special_requirements": ["<special installation requirements>"],
      "restrictions": ["<installation restrictions>"],
      "source": {"files": ["<source files>"]}
    },

    "maintenance_requirements": {
      "log_files": [
        {
          "log": "<log name>",
          "location": "<location>",
          "maintenance_action": "check | clear | rotate",
          "frequency": "<how often>"
        }
      ],
      "database_maintenance": [
        {
          "task": "<maintenance task>",
          "frequency": "<how often>",
          "procedure": "<procedure>"
        }
      ],
      "storage_media": {
        "requirements": "<storage requirements>",
        "maintenance": "<maintenance needed>"
      },
      "source": {"files": ["<source files>"]}
    },

    "security_configuration": {
      "configurable_options": [
        {
          "option": "<option name>",
          "description": "<description>",
          "available_to": "responsible_organization | user | both",
          "configuration_type": "network_ports | encryption | login | audit",
          "default": "<default setting>",
          "recommendations": "<recommended settings>"
        }
      ],
      "security_failure_behavior": {
        "description": "<what happens when security fails>",
        "patient_care_impact": "<impact on patient care>",
        "data_impact": "<impact on data>",
        "workflow_impact": "<impact on clinical workflow>"
      },
      "platform_change_instructions": "<how to handle OS/software updates>",
      "source": {"files": ["<source files>"]}
    },

    "it_network": {
      "network_characteristics": {
        "required_for_purpose": ["<required characteristics>"],
        "bandwidth": "<bandwidth requirements>",
        "latency": "<latency requirements>",
        "availability": "<availability requirements>"
      },
      "technical_specifications": {
        "ports": [
          {
            "port": "<port number>",
            "protocol": "<protocol>",
            "direction": "inbound | outbound | both",
            "purpose": "<purpose>"
          }
        ],
        "protocols": ["<protocols used>"],
        "services": ["<network services required>"]
      },
      "security_specifications": {
        "encryption": "<encryption requirements>",
        "authentication": "<network auth requirements>",
        "malware_protection": "<malware protection requirements>"
      },
      "information_flow": [
        {
          "from": "<source system>",
          "to": "<destination system>",
          "data_type": "<type of data>",
          "protocol": "<protocol used>"
        }
      ],
      "network_failure_hazards": [
        {
          "hazard_id": "NET-HAZ-<seq>",
          "failure_scenario": "<what network failure>",
          "hazardous_situation": "<resulting hazard>",
          "mitigation": "<how to mitigate>"
        }
      ],
      "responsible_organization_warnings": [
        "Execution on IT-network could result in unidentified risks",
        "Responsible organization should identify, analyze, evaluate, and control these risks",
        "Subsequent network changes could introduce new risks requiring additional analysis"
      ],
      "network_change_types": [
        "Configuration changes",
        "Addition of hardware/software to network",
        "Removal of items from network",
        "Update of hardware/software on network",
        "Upgrade of hardware/software on network"
      ],
      "source": {"files": ["<source files>"]}
    }
  },

  "gaps": [
    {
      "document_section": "<IFU or TD section>",
      "clause": "<82304-1 clause>",
      "gap_description": "<what content is missing>",
      "recommendation": "<how to address>",
      "priority": "high | medium | low"
    }
  ]
}
```

## Compliance Mapping

| Output Element | IEC 82304-1 Clause | Evidence Provided |
|----------------|-------------------|-------------------|
| `product_identification` | 7.1, 7.2.1 a-d) | Product and document identification |
| `general_requirements.required_skills` | 7.2.1 | Skills, training, knowledge |
| `general_requirements.location_restrictions` | 7.2.1 | Location/environment restrictions |
| `instructions_for_use.software_description` | 7.2.2.2 | Software description |
| `instructions_for_use.warnings_and_notices` | 7.2.2.3 | Safety/security warnings |
| `instructions_for_use.installation` | 7.2.2.4 | Installation instructions |
| `instructions_for_use.startup_procedure` | 7.2.2.5 | Start-up procedure |
| `instructions_for_use.shutdown_procedure` | 7.2.2.6 | Shutdown procedure |
| `instructions_for_use.operating_instructions` | 7.2.2.7 | Operating instructions |
| `instructions_for_use.messages` | 7.2.2.8 | System/error/fault messages |
| `instructions_for_use.decommissioning` | 7.2.2.9 | Decommissioning and disposal |
| `technical_description.platform_requirements` | 7.2.3.1 a-b) | Platform requirements |
| `technical_description.transport_and_storage` | 7.2.3.1 c) | Transport/storage conditions |
| `technical_description.software_characteristics` | 7.2.3.1 d) | Characteristics (range, accuracy) |
| `technical_description.installation_requirements` | 7.2.3.1 e) | Special installation requirements |
| `technical_description.maintenance_requirements` | 7.2.3.1 f) | Maintenance requirements |
| `technical_description.security_configuration` | 7.2.3.1 g-h) | Security options |
| `technical_description.it_network` | 7.2.3.2 | IT-network requirements |

## Validation Criteria

- [ ] Product identification includes manufacturer, name, version, contact 🆔 cot8Np
- [ ] Intended use statement is documented 🆔 HGiQZ3
- [ ] Essential functions are described 🆔 h0pqer
- [ ] All warnings are catalogued with severity 🆔 IYC9lq
- [ ] Installation requirements cover all 82304-1 7.2.2.4 elements 🆔 OUfx2I
- [ ] Start-up and shutdown procedures are documented 🆔 F71Lnj
- [ ] Operating instructions cover controls, displays, signals 🆔 UQCZvw
- [ ] All error messages have explanations and user actions 🆔 f18GFj
- [ ] Decommissioning includes data export and deletion 🆔 6j3D03
- [ ] Technical description covers platform requirements 🆔 HCI6Lr
- [ ] Security configuration options are documented 🆔 k2MEBk
- [ ] IT-network requirements include all 82304-1 7.2.3.2 elements 🆔 nkJjT0
- [ ] Gaps are identified with recommendations 🆔 7j8tZW
