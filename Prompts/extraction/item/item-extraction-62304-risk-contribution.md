---
id:
title: "Extract Software Item Risk Contribution"
version:
author:
effective_date:
type: "Prompt"
category: "extraction"
level: "item"
standard: "IEC 62304, ISO 14971"
clause: "7"
inputs: ["source code", "configuration files", "test files", "existing documentation"]
outputs: ["item-risk-contribution.json"]
software_class: "B,C"
process: "[Risk Management Process](../../../Canvases/Risk%20Management%20Process.canvas)"
requirements: "[IEC 62304](../../../Requirements/IEC_62304_Requirements.md)"
owner: "[Head of Risk Management](../../../Assets/Head%20of%20Risk%20Management.md)"
---

# Extract Software Item Risk Contribution

## Context

IEC 62304 clause 7 requires software risk management as part of the overall product risk management process defined in ISO 14971.

**Critical distinction:** A software item (single repository) represents the **IMPLEMENTATION perspective** — it is a deployable code unit where failures occur. This is distinct from the **USE perspective** (modules, user intents, core functionalities) which is defined at module/system level.

This prompt extracts:
- **Software item definition** — the deployable unit with safety classification
- **Failure modes** — how this code can fail, categorized by discipline
- **Hazard contributions** — risks this software item can cause or propagate
- **Risk controls implemented** — controls implemented in this codebase
- **Risk controls required upstream** — controls that must be implemented at module/system level
- **Interfaces with safety impact** — external interfaces affecting safety

This output is **NOT** a Risk Management Report. The RMR is generated at system level by aggregating all item contributions.

## Key Concepts

### Software Item (IEC 62304)
A **SoftwareItem** is a deployable software unit — a code repository or service. This is where code actually runs and where failures occur. Each repository = one software item.

### Failure Mode Categories
Each failure mode must be categorized. The category determines which control discipline is responsible:

| Discipline | Categories |
|------------|------------|
| **UI Design** (IEC 62366) | `user_error`, `usability_issue` |
| **Software Design** (IEC 62304) | `software_defect`, `software_crash`, `data_integrity` |
| **Implementation/Deployment** | `configuration_error`, `deployment_error` |
| **Interface Design** | `interface_mismatch`, `communication_failure`, `synchronization_error` |
| **Operations** | `infrastructure_failure`, `resource_exhaustion`, `external_dependency` |
| **Security Design** | `security_breach`, `authentication_failure` |

## Inputs

### Required
- **Source code** — Implementation files showing safety-relevant logic
- **Configuration files** — Settings affecting safety behavior

### Optional (enriches output)
- **Test files** — Verification of risk controls
- **Existing documentation** — Architecture docs, requirement specs
- **item-requirements.json** — Previously extracted requirements
- **item-architecture.json** — Previously extracted architecture

### Hierarchy Context
- **item_id** — Identifier for this software item
- **parent_module** — Module this item belongs to (if known)
- **parent_system** — System this item belongs to (if known)

## Instructions

### 1. Define the Software Item

Extract the software item definition from repository metadata:

```json
{
  "id": "ITEM-<short-id>",
  "name": "<repository name>",
  "description": "<what this service does>",
  "safety_class": "A | B | C",
  "safety_class_rationale": "<why this classification>",
  "version": "<from pom.xml, package.json, etc>",
  "repository_url": "<git remote URL>"
}
```

### 2. Identify Safety-Relevant Code (Clause 7.1.1)

Search for code patterns that could affect patient safety:
- Clinical calculations or decisions
- Data validation (or lack thereof)
- Authorization and access control
- State machines with clinical significance
- External system integrations
- Error handling and failure recovery
- Timeouts and retry logic
- Data persistence and integrity

### 3. Extract Hazard Contributions (Clause 7.1.2)

For each safety-relevant code area, identify:
- What could go wrong (failure mode)
- What harm could result (potential harm)
- How severe the harm could be (severity contribution)
- What causes the failure (cause)

**Note:** Full severity/probability assessment happens at system level. At item level, we document the contribution.

### 4. Document Risk Controls Implemented (Clause 7.2)

For each hazard contribution, identify controls implemented in this codebase:
- Input validation
- State validation
- Defensive programming
- Error handling
- Logging/audit trails
- Access controls
- Timeout handling
- Retry mechanisms

Classify each control:
- `inherent_safety` — Design eliminates the hazard
- `protective_measure` — Code detects and handles the hazard
- `information_for_safety` — Logs, alerts, or user warnings

### 5. Identify Controls Required Upstream (Clause 7.2)

Some hazards cannot be fully controlled at the item level:
- User training requirements
- Operational procedures
- System-level monitoring
- Integration with other items
- Hardware/infrastructure requirements

Document these as `risk_controls_required_upstream`.

### 6. Document Failure Modes (Clause 7.1.3)

For key components, document failure modes with **categorization**:

**Required fields:**
- `id`: Unique identifier (ITEM-<item>-FM-<seq>)
- `name`: Short name for the failure mode
- `category`: One of the 16 defined categories (see Key Concepts above)
- `effect_on_software_item`: What happens within this service
- `effect_on_system`: Potential impact on the broader system
- `detection_mechanism`: How the failure is detected
- `recovery_mechanism`: How the system recovers

**Category selection guide:**
| If the failure is... | Category |
|---------------------|----------|
| User makes a mistake | `user_error` |
| UI is confusing | `usability_issue` |
| Bug in logic/algorithm | `software_defect` |
| Unhandled exception/crash | `software_crash` |
| Data corruption/loss | `data_integrity` |
| Wrong config/settings | `configuration_error` |
| Deployment/migration issue | `deployment_error` |
| API incompatibility | `interface_mismatch` |
| Message loss/protocol error | `communication_failure` |
| Race condition/timing | `synchronization_error` |
| Database/queue failure | `infrastructure_failure` |
| Memory/CPU exhaustion | `resource_exhaustion` |
| Third-party unavailable | `external_dependency` |
| Unauthorized access | `security_breach` |
| Auth bypass/token issue | `authentication_failure` |

### 7. Identify Interfaces with Safety Impact (Clause 7.1.4)

For each external interface, assess:
- Direction (inbound/outbound)
- Safety impact (what happens if it fails)
- Failure handling (how code handles interface failure)

### 8. Document Verification Status

For each risk control, note:
- Is it tested?
- Test file/function reference
- Verification gaps

## Output Schema

Generate a JSON file with this structure:

```json
{
  "extraction_metadata": {
    "repository": "<git remote URL>",
    "commit": "<commit hash>",
    "extracted_at": "<ISO-8601 timestamp>",
    "extractor_version": "3.0",
    "standard": "IEC 62304:2006+AMD1:2015 / ISO 14971:2019",
    "hierarchy": {
      "level": "item",
      "item_id": "<software-item-id>",
      "item_name": "<human-readable name>",
      "parent_module": "<module-id or null>",
      "parent_system": "<system-id or null>"
    }
  },

  "software_item": {
    "id": "ITEM-<short-id>",
    "name": "<repository/service name>",
    "description": "<what this service does>",
    "safety_class": "A | B | C",
    "safety_class_rationale": "<why this classification>",
    "version": "<from pom.xml, package.json, etc>",
    "repository_url": "<git remote URL>",
    "technology_stack": ["<languages, frameworks>"],
    "deployment_type": "microservice | monolith | library | batch_job"
  },

  "item_risk_contribution": {
    "item_id": "<software-item-id>",

    "hazard_contributions": [
      {
        "id": "ITEM-<item>-HAZ-<seq>",
        "description": "<what could go wrong>",
        "cause": "<root cause in this codebase>",
        "potential_harm": "<harm to patient/user>",
        "severity_contribution": "negligible | minor | serious | critical | catastrophic",
        "affected_functions": ["<function names>"],
        "source": {
          "files": ["<path>:<line-range>"]
        }
      }
    ],

    "risk_controls_implemented": [
      {
        "id": "ITEM-<item>-RC-<seq>",
        "description": "<what the control does>",
        "control_type": "inherent_safety | protective_measure | information_for_safety",
        "mitigates": ["ITEM-<item>-HAZ-<seq>"],
        "implementation": "<how it's implemented>",
        "verification_status": "verified | partial | unverified",
        "verification_evidence": {
          "test_file": "<test file path>",
          "test_function": "<test function name>"
        },
        "source": {
          "files": ["<path>:<line-range>"]
        }
      }
    ],

    "risk_controls_required_upstream": [
      {
        "id": "ITEM-<item>-RC-UP-<seq>",
        "description": "<control needed but not implementable here>",
        "rationale": "<why this item cannot implement it>",
        "recommended_level": "module | system",
        "control_type": "inherent_safety | protective_measure | information_for_safety",
        "mitigates": ["ITEM-<item>-HAZ-<seq>"]
      }
    ],

    "failure_modes": [
      {
        "id": "ITEM-<item>-FM-<seq>",
        "name": "<short descriptive name>",
        "category": "user_error | usability_issue | software_defect | software_crash | data_integrity | configuration_error | deployment_error | interface_mismatch | communication_failure | synchronization_error | infrastructure_failure | resource_exhaustion | external_dependency | security_breach | authentication_failure",
        "component": "<component/class where failure originates>",
        "effect_on_software_item": "<what happens within this service>",
        "effect_on_system": "<potential impact on broader system>",
        "detection_mechanism": "<how failure is detected: logging, monitoring, error response>",
        "recovery_mechanism": "<how system recovers: retry, fallback, manual intervention>",
        "related_hazards": ["ITEM-<item>-HAZ-<seq>"],
        "source": {
          "files": ["<path>:<line-range>"]
        }
      }
    ],

    "interfaces_with_safety_impact": [
      {
        "id": "ITEM-<item>-IF-<seq>",
        "interface": "<interface name>",
        "type": "REST | JMS | database | file | other",
        "direction": "inbound | outbound | bidirectional",
        "safety_impact": "<what happens if it fails>",
        "failure_handling": "<how code handles failure>",
        "timeout": "<timeout value if applicable>",
        "retry_policy": "<retry policy if applicable>",
        "source": {
          "files": ["<path>:<line-range>"]
        }
      }
    ],

    "verification_gaps": [
      {
        "id": "ITEM-<item>-GAP-<seq>",
        "type": "untested_control | unverified_interface | missing_failure_test",
        "description": "<what's missing>",
        "affected_items": ["<hazard or control IDs>"],
        "priority": "high | medium | low",
        "recommendation": "<what should be done>"
      }
    ]
  },

  "aggregation_hints": {
    "cross_item_interfaces": [
      "<interface names that connect to other items>"
    ],
    "shared_hazards": [
      "<hazards that likely exist in other items too>"
    ],
    "system_level_controls_needed": [
      "<controls that must be coordinated at system level>"
    ]
  }
}
```

## Compliance Mapping

| Output Element | IEC 62304 / ISO 14971 Clause | Evidence Provided |
|----------------|------------------------------|-------------------|
| `software_item` | IEC 62304 5.1.1 | Software item definition |
| `software_item.safety_class` | IEC 62304 4.3 | Safety classification |
| `hazard_contributions[]` | ISO 14971 5.4 | Hazard identification |
| `hazard_contributions[].cause` | ISO 14971 5.4 | Hazardous situation identification |
| `hazard_contributions[].potential_harm` | ISO 14971 5.4 | Potential harm documentation |
| `risk_controls_implemented[]` | ISO 14971 7.1 | Risk control measures |
| `risk_controls_implemented[].control_type` | ISO 14971 7.1 | Control priority order |
| `risk_controls_implemented[].verification_status` | ISO 14971 7.2 | Verification of risk controls |
| `failure_modes[]` | IEC 62304 7.1.3 | Failure mode analysis |
| `failure_modes[].category` | ISO 14971 7.1 | Control discipline mapping |
| `interfaces_with_safety_impact[]` | IEC 62304 7.1.2 | Interface risk analysis |
| `verification_gaps[]` | ISO 14971 7.2 | Gaps in risk control verification |

## What This Output Does NOT Include

This is an **item-level** extraction. It does NOT include:

| Excluded | Reason | Where It Lives |
|----------|--------|----------------|
| Risk acceptability criteria | Organizational decision | System RMP |
| Full severity/probability matrix | Requires system context | System Risk Analysis |
| Residual risk acceptability | System-level assessment | System RMR |
| Benefit-risk analysis | Clinical/regulatory decision | System RMR |
| Overall risk conclusions | Requires all items | System RMR |
| Post-market surveillance plan | System-level activity | System RMP |

## Examples

### Input: Java service with handover functionality

```java
// HandoverService.java
@Service
public class HandoverService {

    public void proposeHandover(Appointment appt, Practitioner target) {
        // Validation - risk control
        if (!appt.canBeHandedOver()) {
            throw new InvalidStateException("Cannot handover");
        }

        // State change - hazard contribution
        appt.setState(HANDOVER_PROPOSED);
        appt.setTargetPractitioner(target);

        // External notification - interface with safety impact
        notificationService.notifyHandover(appt, target);
    }
}
```

### Output: item-risk-contribution.json (partial)

```json
{
  "software_item": {
    "id": "ITEM-HM",
    "name": "health-manager",
    "description": "Core service for appointment and care plan management",
    "safety_class": "B",
    "safety_class_rationale": "Software that can contribute to hazardous situations but is not sole cause of harm",
    "version": "1.0.0",
    "repository_url": "https://github.com/platform24/health-manager",
    "technology_stack": ["Java 17", "Spring Boot", "MariaDB", "JMS"],
    "deployment_type": "microservice"
  },

  "item_risk_contribution": {
    "item_id": "health-manager",

    "hazard_contributions": [
      {
        "id": "ITEM-HM-HAZ-001",
        "description": "Handover proposal to unavailable practitioner",
        "cause": "Target practitioner availability not validated",
        "potential_harm": "Patient care delayed, wrong practitioner assigned",
        "severity_contribution": "serious",
        "source": {"files": ["HandoverService.java:15-25"]}
      }
    ],

    "failure_modes": [
      {
        "id": "ITEM-HM-FM-001",
        "name": "Service throws uncaught exception",
        "category": "software_crash",
        "component": "UpdateAppointmentServiceImpl",
        "effect_on_software_item": "Transaction rollback, HTTP 500 response",
        "effect_on_system": "Caller receives error, must retry or handle failure",
        "detection_mechanism": "Exception logging, monitoring alerts",
        "recovery_mechanism": "Automatic retry by client, manual intervention for persistent issues",
        "related_hazards": ["ITEM-HM-HAZ-001"],
        "source": {"files": ["UpdateAppointmentServiceImpl.java:45-80"]}
      },
      {
        "id": "ITEM-HM-FM-002",
        "name": "JMS message processing timeout",
        "category": "communication_failure",
        "component": "AppointmentEventListener",
        "effect_on_software_item": "Message requeued, potential duplicate processing",
        "effect_on_system": "Data inconsistency between services",
        "detection_mechanism": "Message age monitoring, DLQ monitoring",
        "recovery_mechanism": "Manual DLQ processing, deduplication in downstream consumers",
        "related_hazards": [],
        "source": {"files": ["AppointmentEventListener.java:30-55"]}
      }
    ],

    "risk_controls_implemented": [
      {
        "id": "ITEM-HM-RC-001",
        "description": "State validation before handover",
        "control_type": "protective_measure",
        "mitigates": ["ITEM-HM-HAZ-001"],
        "implementation": "canBeHandedOver() validates appointment state",
        "verification_status": "partial",
        "source": {"files": ["HandoverService.java:12-14"]}
      }
    ],

    "risk_controls_required_upstream": [
      {
        "id": "ITEM-HM-RC-UP-001",
        "description": "Practitioner training on handover confirmation",
        "rationale": "Software cannot force user to verify handover details",
        "recommended_level": "system",
        "control_type": "information_for_safety",
        "mitigates": ["ITEM-HM-HAZ-001"]
      }
    ],

    "interfaces_with_safety_impact": [
      {
        "id": "ITEM-HM-IF-001",
        "interface": "NotificationService",
        "type": "REST",
        "direction": "outbound",
        "safety_impact": "If notification fails, practitioner unaware of handover",
        "failure_handling": "Logged, no retry - GAP",
        "source": {"files": ["HandoverService.java:22"]}
      }
    ],

    "verification_gaps": [
      {
        "id": "ITEM-HM-GAP-001",
        "type": "untested_control",
        "description": "State validation logic lacks unit tests",
        "affected_items": ["ITEM-HM-RC-001"],
        "priority": "high",
        "recommendation": "Add unit tests for canBeHandedOver()"
      }
    ]
  }
}
```

## Validation Criteria

- [ ] Software item definition is complete with all required fields 🆔 p64fQ1
- [ ] Safety class is assigned with rationale 🆔 ULycXB
- [ ] All safety-relevant code areas identified 🆔 wOzyjy
- [ ] Each hazard has source file references 🆔 ekCkZe
- [ ] Each control has verification status 🆔 ejSR45
- [ ] Controls are correctly typed (inherent/protective/information) 🆔 oc86Y1
- [ ] Upstream controls are identified where item cannot implement 🆔 ueQUvy
- [ ] Failure modes cover critical components 🆔 qlCplw
- [ ] Each failure mode has a valid `category` from the defined list 🆔 pBW11W
- [ ] Each failure mode has `effect_on_software_item` and `effect_on_system` 🆔 AWk01p
- [ ] External interfaces assessed for safety impact 🆔 hh78bO
- [ ] Verification gaps identified with priority 🆔 NeYApV
- [ ] Hierarchy metadata correctly populated 🆔 DmS3Qg
- [ ] Output explicitly excludes system-level elements (RMR content) 🆔 7s5tyJ
