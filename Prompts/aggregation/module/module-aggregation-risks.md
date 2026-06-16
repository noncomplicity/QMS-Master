---
id:
title: "Aggregate Module Risks"
version:
author:
effective_date:
type: "Prompt"
category: "aggregation"
level: "module"
standard: "IEC 62304, ISO 14971"
clause: "7"
inputs: ["item-risk-contribution.json (multiple)", "module-architecture.json", "system-manifest.json"]
outputs: ["module-risk.json"]
software_class: "B,C"
process: "[Risk Management Process](../../../Canvases/Risk%20Management%20Process.canvas)"
requirements: "[ISO 14971](../../../Requirements/ISO_14971_Requirements.md)"
owner: "[Head of Risk Management](../../../Assets/Head%20of%20Risk%20Management.md)"
---

# Aggregate Module Risks

## Context

IEC 62304 clause 7 requires risk management integration throughout software development. For modules composed of multiple software items, this aggregation prompt combines item-level risk contributions into a module-level risk view.

**Key concept:** A Module represents the **USE perspective** — it groups user intents and core functionalities (what users do). Software items represent the **IMPLEMENTATION perspective** — they are deployable code units (how it's built). This prompt bridges these perspectives by:

1. Collecting `item-risk-contribution.json` from all software items
2. Defining core functionalities that enable user intents
3. Linking failure modes to core functionalities
4. Identifying cross-item hazards within the module
5. Producing `module-risk.json`

Module-level risks then feed into:
- System-level risk file aggregation
- Risk Management Report (RMR) at system level
- Module integration testing prioritization

**Note**: Full risk acceptability decisions are made at system level. Module-level aggregation focuses on identifying and documenting risks, not making acceptability decisions.

## Key Concepts

### Core Functionality
A **CoreFunctionality** is a software capability that enables a user intent. It:
- Has source files in one or more software items
- Can have failure modes associated with it
- Bridges the gap between user needs and implementation

### Failure Mode Categories
Failure modes are categorized by the control discipline responsible for addressing them:

| Discipline | Categories | Control Type |
|------------|------------|--------------|
| UI Design (IEC 62366) | `user_error`, `usability_issue` | `ui_design` |
| Software Design (IEC 62304) | `software_defect`, `software_crash`, `data_integrity` | `software_design` |
| Implementation | `configuration_error`, `deployment_error` | `implementation_control` |
| Interface Design | `interface_mismatch`, `communication_failure`, `synchronization_error` | `interface_design` |
| Operations | `infrastructure_failure`, `resource_exhaustion`, `external_dependency` | `operational_control` |
| Security | `security_breach`, `authentication_failure` | `security_design` |

## Inputs

### Required
- **`items/<item>/extracted/item-risk-contribution.json`** — Risk contributions from each software item
- **`module-architecture.json`** — Module architecture showing inter-item interfaces
- **`system-manifest.json`** — System structure defining module membership

### Optional (enriches output)
- **`items/<item>/extracted/item-requirements.json`** — For traceability
- **Integration test results** — For control verification
- **Incident reports** — Historical risk data

### Configuration Required
- **module_id** — Identifier for this module
- **module_name** — Human-readable module name
- **parent_system** — System this module belongs to

## Instructions

### 1. Collect Item Risk Contributions

For each software item belonging to this module (from `system-manifest.json`):
1. Load `item-risk-contribution.json`
2. Validate hierarchy metadata matches expected module
3. Collect all hazard contributions, risk controls, failure modes, interface risks
4. Collect `software_item` definitions from each item

### 2. Define Core Functionalities

Identify the core functionalities this module provides. These are software capabilities that:
- Enable specific user intents (defined at system level)
- Have source code in one or more software items
- Can experience failure modes

For each core functionality:
```json
{
  "id": "CF-<seq>",
  "name": "<functionality name>",
  "description": "<what this capability does>",
  "software_items": ["ITEM-<id>"],
  "source_files": ["<item>/<path>"],
  "failure_modes": ["<fm-ids that affect this functionality>"]
}
```

**Examples of core functionalities:**
- "Appointment Booking API" — REST endpoints for creating appointments
- "Escalation Rules Engine" — Evaluates rules and triggers actions
- "Handover State Machine" — Manages state transitions for patient handover

### 4. Map Item Hazards to Module Hazards

Analyze relationships between item hazards:
- **Independent hazards** — Item hazard affects only that item
- **Contributing hazards** — Multiple items contribute to same outcome
- **Cascading hazards** — One item's hazard triggers another's

Create module hazard list:
```
MOD-SCP-HAZ-001: "Care plan data corruption"
  Contributing items:
    ← ITEM-HM-HAZ-001: "State machine error corrupts handover"
    ← ITEM-API-HAZ-002: "Race condition in appointment update"
  Cascade path: health-manager → appointment-api → notification-service
```

### 5. Identify Cross-Item Hazards

Analyze module-architecture.json for hazards arising from item interactions:

**Interface hazards:**
- Data format mismatches
- Protocol version incompatibilities
- Timing/ordering dependencies
- Authentication/authorization gaps

**Data flow hazards:**
- Data corruption during transfer
- Data loss in async processing
- Stale data propagation
- Privacy data leakage

**Failure propagation:**
- Cascade failures across items
- Resource exhaustion spreading
- Error handling gaps at boundaries

### 6. Aggregate and Enhance Failure Modes

Collect failure modes from all software items and enhance them with:
- `software_item_id` — Which software item this failure originates in
- `core_functionality_id` — Which core functionality is affected
- `contributes_to_hazards` — Which module hazards this failure can cause

```json
{
  "id": "MOD-<mod>-FM-<seq>",
  "software_item_id": "ITEM-<id>",
  "name": "<failure mode name>",
  "category": "<from item extraction>",
  "core_functionality_id": "CF-<seq>",
  "effect_on_software_item": "<local effect>",
  "effect_on_system": "<broader effect>",
  "detection_mechanism": "<how detected>",
  "recovery_mechanism": "<how recovered>",
  "contributes_to_hazards": ["MOD-<mod>-HAZ-<seq>"],
  "source": {"files": ["<path>:<lines>"]}
}
```

### 7. Aggregate Risk Controls

Combine controls from all items:
- **Item-level controls** — Controls implemented within individual items
- **Cross-item controls** — Controls that span multiple items (e.g., end-to-end encryption)
- **Module-level controls** — Controls added at module integration (e.g., circuit breakers)

For each control:
- Track which hazards it mitigates
- Note implementation location(s)
- Document verification status
- Map control type based on failure mode category

### 8. Identify Control Gaps

Find hazards without adequate controls:
- Item hazards with no item-level control
- Cross-item hazards with no integration control
- Controls that exist but aren't verified

Flag gaps for:
- Module-level control implementation
- Escalation to system-level controls
- Documentation in IFU (user controls)

### 9. Consolidate Upstream Control Requirements

Aggregate `risk_controls_required_upstream` from all items:
- User training needs
- Operational procedure requirements
- Infrastructure requirements
- Documentation requirements

Group by control type and consolidate duplicates.

### 10. Assess Integration Risk

For each inter-item interface from module-architecture.json:
- What happens if the interface fails?
- What happens if data is corrupted?
- What happens if response is delayed?
- Are there retry/fallback mechanisms?

### 11. Create Traceability

Link module risks to:
- **Upward**: System hazards this module contributes to
- **Downward**: Item hazards aggregated here
- **Lateral**: Integration tests verifying controls
- **Functional**: Core functionalities affected by each failure mode

## Output Schema

Generate a JSON file with this structure:

```json
{
  "extraction_metadata": {
    "aggregated_at": "<ISO-8601 timestamp>",
    "aggregator_version": "3.0",
    "standard": "IEC 62304:2006+AMD1:2015 / ISO 14971:2019",
    "hierarchy": {
      "level": "module",
      "module_id": "<module-id>",
      "module_name": "<module name>",
      "parent_system": "<system-id>",
      "child_items": ["<item-ids>"]
    },
    "source_extractions": [
      {
        "item_id": "<item-id>",
        "commit": "<commit hash>",
        "extracted_at": "<timestamp>"
      }
    ]
  },

  "software_items": [
    {
      "id": "ITEM-<id>",
      "name": "<repository/service name>",
      "description": "<what this service does>",
      "safety_class": "A | B | C",
      "version": "<version>",
      "repository_url": "<git remote URL>",
      "core_functionalities": ["CF-<seq>"]
    }
  ],

  "core_functionalities": [
    {
      "id": "CF-<seq>",
      "name": "<functionality name>",
      "description": "<what this capability does>",
      "software_items": ["ITEM-<id>"],
      "source_files": ["<item>/<path>"],
      "failure_modes": ["MOD-<mod>-FM-<seq>"]
    }
  ],

  "module_summary": {
    "module_id": "<module-id>",
    "total_hazards": "<count>",
    "item_hazards": "<count from items>",
    "cross_item_hazards": "<count identified at module level>",
    "total_controls": "<count>",
    "uncontrolled_hazards": "<count>",
    "control_gaps": "<count>",
    "core_functionalities": "<count>",
    "failure_modes": "<count>"
  },

  "module_hazards": [
    {
      "id": "MOD-<mod>-HAZ-<seq>",
      "description": "<module-level hazard description>",
      "hazard_type": "item | cross_item | integration",

      "failure_mode_ids": ["MOD-<mod>-FM-<seq>"],

      "contributing_items": [
        {
          "item_id": "<item-id>",
          "item_hazard_id": "ITEM-<item>-HAZ-<seq>",
          "contribution": "<how this item contributes>"
        }
      ],

      "potential_harm": "<what could go wrong>",
      "severity_contribution": "negligible | minor | serious | critical | catastrophic",
      "severity_rationale": "<why this severity>",

      "cascade_path": {
        "exists": true | false,
        "path": ["<item-id> → <item-id> → ..."],
        "amplification": "<does hazard get worse as it cascades>"
      },

      "source": {
        "files": ["<item>/<path>:<lines>"],
        "interfaces": ["MOD-<mod>-INT-<seq>"]
      }
    }
  ],

  "cross_item_hazards": [
    {
      "id": "MOD-<mod>-XIH-<seq>",
      "description": "<hazard arising from item interactions>",
      "hazard_category": "interface | data_flow | timing | security | resource",
      "items_involved": ["<item-ids>"],
      "interface_id": "MOD-<mod>-INT-<seq>",

      "scenario": "<how the hazardous situation arises>",
      "potential_harm": "<what could go wrong>",
      "severity_contribution": "negligible | minor | serious | critical | catastrophic",

      "existing_controls": ["MOD-<mod>-RC-<seq>"],
      "control_effectiveness": "adequate | partial | inadequate | none"
    }
  ],

  "risk_controls": {
    "from_items": [
      {
        "id": "MOD-<mod>-RC-ITEM-<seq>",
        "source_item": "<item-id>",
        "source_control_id": "ITEM-<item>-RC-<seq>",
        "description": "<control description>",
        "control_type": "inherent_safety | protective_measure | information_for_safety",
        "mitigates": ["MOD-<mod>-HAZ-<seq>"],
        "implementation": "<where/how implemented>",
        "verification_status": "verified | partial | unverified",
        "verification_evidence": "<test IDs or inspection reference>"
      }
    ],

    "module_level": [
      {
        "id": "MOD-<mod>-RC-MOD-<seq>",
        "description": "<control added at module integration>",
        "control_type": "inherent_safety | protective_measure | information_for_safety",
        "mitigates": ["MOD-<mod>-HAZ-<seq>", "MOD-<mod>-XIH-<seq>"],
        "implementation": "<where/how implemented>",
        "items_involved": ["<item-ids>"],
        "verification_status": "verified | partial | unverified",
        "verification_evidence": "<integration test IDs>"
      }
    ],

    "required_upstream": [
      {
        "id": "MOD-<mod>-RC-UP-<seq>",
        "description": "<control requiring user/organizational action>",
        "control_type": "information_for_safety",
        "source_items": ["<items that identified this need>"],
        "mitigates": ["MOD-<mod>-HAZ-<seq>"],
        "recommended_implementation": "IFU | training | operational_procedure | infrastructure",
        "rationale": "<why this can't be automated>"
      }
    ]
  },

  "failure_modes": [
    {
      "id": "MOD-<mod>-FM-<seq>",
      "software_item_id": "ITEM-<id>",
      "name": "<failure mode name>",
      "category": "user_error | usability_issue | software_defect | software_crash | data_integrity | configuration_error | deployment_error | interface_mismatch | communication_failure | synchronization_error | infrastructure_failure | resource_exhaustion | external_dependency | security_breach | authentication_failure",
      "core_functionality_id": "CF-<seq>",
      "effect_on_software_item": "<impact within the software item>",
      "effect_on_system": "<potential system-level impact>",
      "detection_mechanism": "<how failure is detected>",
      "recovery_mechanism": "<how system recovers>",
      "contributes_to_hazards": ["MOD-<mod>-HAZ-<seq>"],
      "source_item_fm": "ITEM-<item>-FM-<seq>",
      "source": {
        "files": ["<item>/<path>:<lines>"]
      }
    }
  ],

  "interface_risk_assessment": [
    {
      "interface_id": "MOD-<mod>-INT-<seq>",
      "interface_name": "<interface name>",
      "source_item": "<item-id>",
      "target_item": "<item-id>",

      "failure_scenarios": [
        {
          "scenario": "interface_unavailable | data_corruption | timeout | auth_failure",
          "likelihood": "rare | unlikely | possible | likely | frequent",
          "impact": "<what happens>",
          "existing_handling": "<retry, fallback, circuit breaker, etc>",
          "residual_concern": "<remaining risk>"
        }
      ],

      "overall_risk_level": "low | medium | high",
      "mitigation_adequacy": "adequate | needs_improvement | inadequate"
    }
  ],

  "control_gaps": [
    {
      "id": "MOD-<mod>-GAP-<seq>",
      "type": "uncontrolled_hazard | partial_control | unverified_control | missing_integration_control",
      "description": "<what's missing>",
      "affected_hazards": ["MOD-<mod>-HAZ-<seq>"],
      "affected_items": ["<item-ids>"],
      "recommended_action": "<what should be done>",
      "recommended_level": "item | module | system",
      "priority": "high | medium | low"
    }
  ],

  "traceability": {
    "to_system_hazards": [
      {
        "module_hazard_id": "MOD-<mod>-HAZ-<seq>",
        "system_hazard_ids": ["SYS-<sys>-HAZ-<seq>"],
        "contribution_type": "direct | contributing | enabling"
      }
    ],
    "to_item_hazards": [
      {
        "module_hazard_id": "MOD-<mod>-HAZ-<seq>",
        "item_hazard_ids": ["ITEM-<item>-HAZ-<seq>"]
      }
    ],
    "to_integration_tests": [
      {
        "control_id": "MOD-<mod>-RC-<seq>",
        "test_ids": ["MOD-<mod>-IT-<seq>"],
        "verification_status": "verified | partial | unverified"
      }
    ]
  }
}
```

## Compliance Mapping

| Output Element | IEC 62304/ISO 14971 Clause | Evidence Provided |
|----------------|----------------------------|-------------------|
| `software_items[]` | IEC 62304 5.1.1 | Software item identification |
| `core_functionalities[]` | IEC 62304 5.2 | Software functionality documentation |
| `module_hazards[]` | ISO 14971 5.4 | Hazard identification |
| `module_hazards[].failure_mode_ids` | ISO 14971 5.4 | Failure mode to hazard traceability |
| `module_hazards[].severity_contribution` | ISO 14971 5.5 | Severity estimation |
| `cross_item_hazards[]` | IEC 62304 7.1.2 | Interface risks |
| `risk_controls.from_items[]` | ISO 14971 7.1 | Control option analysis |
| `risk_controls.module_level[]` | ISO 14971 7.1 | Integration controls |
| `risk_controls[].verification_status` | ISO 14971 7.2 | Implementation verification |
| `failure_modes[]` | IEC 62304 7.1.3 | Failure mode analysis |
| `failure_modes[].category` | ISO 14971 7.1 | Control discipline mapping |
| `failure_modes[].core_functionality_id` | IEC 62304 7.1 | Functionality traceability |
| `failure_modes[].contributes_to_hazards` | ISO 14971 5.4 | Failure to hazard traceability |
| `interface_risk_assessment[]` | IEC 62304 7.1.2 | Interface risk analysis |
| `control_gaps[]` | IEC 62304 7.3 | Risk control completeness |
| `traceability` | IEC 62304 7.3.3 | Risk management traceability |

## Aggregation Rules

### Severity Aggregation
Use the **highest** severity from contributing items:
```
ITEM-A severity: minor
ITEM-B severity: serious
→ Module hazard severity: serious
```

### Cascade Amplification
When hazards cascade across items:
- Document the cascade path
- Assess if severity increases through cascade
- Consider combined probability

### Control Aggregation
- **Deduplicate** identical controls from different items
- **Combine** partial controls into complete coverage
- **Layer** item + module controls for defense in depth

### Verification Status
Use the **weakest** status when controls span items:
```
ITEM-A control: verified
ITEM-B control: partial
→ Combined: partial
```

### Gap Prioritization
| Gap Type | Default Priority |
|----------|------------------|
| Uncontrolled high-severity hazard | High |
| Unverified control for high-severity | High |
| Cross-item hazard without integration control | High |
| Uncontrolled medium-severity hazard | Medium |
| Partial control coverage | Medium |
| Unverified low-severity control | Low |

## Validation Criteria

- [ ] All items from system-manifest.json are included 🆔 x5lfYT
- [ ] Software items collected from all item extractions 🆔 5LR7Ug
- [ ] Core functionalities defined with source file references 🆔 Md1TzN
- [ ] Each core functionality links to its software items 🆔 yjr2pd
- [ ] Each item hazard maps to a module hazard 🆔 9sn8Sc
- [ ] Each module hazard has `failure_mode_ids` populated 🆔 X3gJyH
- [ ] Each failure mode has valid `category` from defined list 🆔 xBemdb
- [ ] Each failure mode has `software_item_id` and `core_functionality_id` 🆔 JPo1sH
- [ ] Each failure mode has `contributes_to_hazards` populated 🆔 6A2Ffw
- [ ] Cross-item hazards identified for each inter-item interface 🆔 u84sZC
- [ ] Interface risk assessment completed for all interfaces 🆔 JVzu4w
- [ ] Cascade paths documented where applicable 🆔 fVrzrS
- [ ] Controls from all items are aggregated 🆔 AFiiC1
- [ ] Module-level controls documented where added 🆔 QHTh1b
- [ ] Control gaps identified with recommended actions 🆔 IlF3vJ
- [ ] Upstream control requirements consolidated 🆔 3PvhAC
- [ ] Traceability to item hazards is complete 🆔 ksaOwW
- [ ] Verification status documented for all controls 🆔 w4v41s
