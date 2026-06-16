---
id: 725af57
title: "system aggregation risk file"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Prompt"
category: "aggregation"
level: "system"
standard: "ISO 14971, IEC 62304, IEC 82304-1"
clause: "4.5, 5.4, 7"
inputs: ["module-risk.json (multiple)", "system-manifest.json", "use-requirements.json"]
outputs: ["system-risk-file.json"]
software_class: "all"
process: "[Risk Management Process](../../../Canvases/Risk%20Management%20Process.canvas)"
requirements: "[ISO 14971](../../../Requirements/ISO_14971_Requirements.md)"
owner: "[Head of Risk Management](../../../Assets/Head%20of%20Risk%20Management.md)"
---

# Aggregate System Risk File

## Context

ISO 14971 clause 4.5 requires a Risk Management File containing all risk management records. For multi-module systems, this file must aggregate risk data from all modules and provide the complete USE and RISK perspectives.

**Key concept:** This is where the three perspectives converge:
- **USE perspective**: Intended uses → User intents → Core functionalities
- **IMPLEMENTATION perspective**: Software items → Failure modes (from modules)
- **RISK perspective**: Safety characteristics → Hazards → Harm → Controls

This aggregation prompt:
1. Defines safety characteristics (ISO 14971 Annex C)
2. Defines intended uses and user intents
3. Aggregates software items and core functionalities from modules
4. Links failure modes to safety characteristics and hazards
5. Creates system-level hazard analysis with complete traceability
6. Applies risk acceptability criteria
7. Produces the complete `system-risk-file.json`

The aggregated risk file is then used to:
- Feed the Risk Management Visualizer
- Generate `RMP.md` — Risk Management Plan
- Generate `Risk-Analysis.md` — Full risk analysis
- Generate `RMR.md` — Risk Management Report

## Key Concepts

### Safety Characteristics (ISO 14971 Annex C)
ISO 14971 Annex C defines characteristics related to safety that must be systematically evaluated. Each characteristic is a question to ask about the medical device. Documenting which characteristics are applicable provides:
- Systematic hazard identification
- Audit trail for why hazards were identified
- Completeness check that no safety concern was overlooked

### Intended Use vs User Intent
- **Intended Use**: The high-level clinical purpose (e.g., "Clinical Appointment Management")
- **User Intent**: What a user is trying to accomplish (e.g., "Book Appointment", "Escalate Critical Case")

### Traceability Chain
The complete chain must be traceable:
```
SafetyCharacteristic → identifies → FailureMode → causes → Hazard → leads to → Harm
                                         ↑                      ↓
                              CoreFunctionality          RiskControl
                                         ↑
                                   UserIntent
                                         ↑
                                   IntendedUse
```

## Inputs

### Required
- **`modules/<module>/aggregated/module-risk.json`** — Module-level risk aggregations with core functionalities and failure modes
- **`system-manifest.json`** — System structure defining modules and items

### Optional (enriches output)
- **`system/defined/use-requirements.json`** — Pre-defined use requirements
- **`system/defined/intended-use.json`** — Pre-defined intended use documentation
- **`system/defined/safety-characteristics.json`** — Pre-defined safety characteristic assessments

### Configuration Required
- **Risk acceptability criteria** — Severity × probability matrix and thresholds
- **Benefit-risk policy** — Criteria for accepting risks based on clinical benefit

## Instructions

### 1. Define Safety Characteristics (ISO 14971 Annex C)

Document which safety characteristics from ISO 14971 Annex C are applicable to this system:

| ID | Category | Question | Applicable? |
|----|----------|----------|-------------|
| C.2.1 | `intended_use` | What is the intended use? | Usually Yes |
| C.2.11 | `measurements` | Are measurements taken? | If calculations |
| C.2.12 | `interpretive` | Is the device interpretive? | If clinical decisions |
| C.2.13 | `device_interaction` | Used with other devices? | If integrations |
| C.2.19 | `software` | Contains software? | Always Yes |
| C.2.26 | `training_required` | Requires special training? | Usually Yes |
| C.2.28 | `human_factors` | Dependent on human factors? | If UI-critical |
| C.2.29 | `alarm_system` | Uses alarm system? | If notifications |
| C.2.30 | `deliberate_misuse` | Can be deliberately misused? | Security-relevant |
| C.2.31 | `critical_data` | Holds critical patient data? | Usually Yes |
| C.2.33 | `essential_performance` | Has essential performance? | If safety-critical functions |

For each applicable characteristic, document:
- Why it's applicable (rationale)
- Which hazards were identified through this characteristic

### 2. Define Intended Uses

Document the high-level clinical purposes of the system:

```json
{
  "id": "IU-<seq>",
  "name": "<intended use name>",
  "description": "<clinical purpose>",
  "user_intents": ["UI-<seq>"]
}
```

### 3. Define User Intents

Document what users are trying to accomplish:

```json
{
  "id": "UI-<seq>",
  "name": "<user intent name>",
  "description": "<what user wants to do>",
  "intended_use_id": "IU-<seq>",
  "core_functionalities": ["CF-<seq>"]
}
```

### 4. Collect Module Risk Data

For each module defined in `system-manifest.json`:
1. Load `module-risk.json`
2. Validate hierarchy metadata matches manifest
3. Collect software items, core functionalities, failure modes, hazards, controls

### 5. Aggregate Software Items and Core Functionalities

Combine from all modules:
- Deduplicate shared software items (items used by multiple modules)
- Collect all core functionalities
- Link core functionalities to user intents

### 6. Link Failure Modes to Safety Characteristics

For each failure mode, determine which safety characteristic(s) it relates to:
- Software failures → `software` (C.2.19)
- Communication failures → `device_interaction` (C.2.13)
- User errors → `human_factors` (C.2.28)
- Data issues → `critical_data` (C.2.31)
- etc.

### 7. Identify System-Level Hazards

Map module hazards to system-level hazards:
- Multiple modules may contribute to the same system hazard
- Some hazards may cascade across modules
- Cross-module interfaces create new hazardous situations

Create system hazard list with traceability to failure modes:
```
SYS-P24-HAZ-001: "Patient receives incorrect care"
  failure_mode_ids: [MOD-SCP-FM-001, MOD-SCP-FM-003]
  safety_characteristics: [SC-001, SC-005]
```

### 8. Analyze Cross-Module Risks

Identify hazardous situations arising from module interactions:
- Data flow between modules (corruption, loss)
- Timing dependencies (race conditions)
- Failure propagation (cascade failures)
- Security boundaries (breach propagation)

### 9. Aggregate Risk Controls

Combine controls from all modules:
- Deduplicate identical controls
- Identify control gaps (hazards without controls)
- Add system-level controls where needed
- Verify control completeness
- Map controls to control types based on failure mode categories

### 10. Apply Risk Acceptability Criteria

For each hazardous situation:
1. Assess severity (from module contributions + system context)
2. Assess probability (considering all modules + system factors)
3. Calculate initial risk level
4. Apply controls
5. Calculate residual risk level
6. Evaluate against acceptability criteria

### 11. Perform Benefit-Risk Analysis

For residual risks not acceptable purely on risk grounds:
1. Document clinical benefit
2. Justify benefit outweighs risk
3. Document disclosure requirements

### 12. Document Upstream Controls

Consolidate `risk_controls_required_upstream` from all modules:
- User training requirements
- Operational procedures
- Infrastructure requirements
- Documentation requirements

### 13. Generate Complete Traceability

Create bidirectional traceability chains:
- **Forward**: IntendedUse → UserIntent → CoreFunctionality → FailureMode → Hazard → Harm
- **Backward**: RiskControl → Hazard → FailureMode → CoreFunctionality → UserIntent
- **Safety**: SafetyCharacteristic → FailureMode → Hazard

## Output Schema

Generate a JSON file with this structure:

```json
{
  "extraction_metadata": {
    "aggregated_at": "<ISO-8601 timestamp>",
    "aggregator_version": "3.0",
    "standard": "ISO 14971:2019 / IEC 62304:2006+AMD1:2015 / IEC 82304-1:2016",
    "hierarchy": {
      "level": "system",
      "system_id": "<system-id>",
      "system_name": "<system name>",
      "child_modules": ["<module-ids>"],
      "child_items": ["<item-ids>"]
    },
    "source_aggregations": [
      {
        "module_id": "<module-id>",
        "aggregated_at": "<timestamp>"
      }
    ]
  },

  "safety_characteristics": [
    {
      "id": "SC-<seq>",
      "category": "intended_use | measurements | interpretive | device_interaction | software | training_required | human_factors | alarm_system | deliberate_misuse | critical_data | essential_performance",
      "name": "<characteristic name>",
      "guidance_question": "<ISO 14971 Annex C question>",
      "annex_c_reference": "C.2.<number>",
      "applicable": true | false,
      "applicability_rationale": "<why applicable or not>",
      "identified_hazards": ["SYS-<sys>-HAZ-<seq>"]
    }
  ],

  "intended_uses": [
    {
      "id": "IU-<seq>",
      "name": "<intended use name>",
      "description": "<clinical purpose>",
      "user_intents": ["UI-<seq>"]
    }
  ],

  "user_intents": [
    {
      "id": "UI-<seq>",
      "name": "<user intent name>",
      "description": "<what user wants to accomplish>",
      "intended_use_id": "IU-<seq>",
      "core_functionalities": ["CF-<seq>"]
    }
  ],

  "software_items": [
    {
      "id": "ITEM-<id>",
      "name": "<repository/service name>",
      "description": "<what this service does>",
      "safety_class": "A | B | C",
      "version": "<version>",
      "repository_url": "<git remote URL>",
      "modules": ["<module-ids>"],
      "core_functionalities": ["CF-<seq>"]
    }
  ],

  "core_functionalities": [
    {
      "id": "CF-<seq>",
      "name": "<functionality name>",
      "description": "<what this capability does>",
      "user_intent_id": "UI-<seq>",
      "software_items": ["ITEM-<id>"],
      "source_files": ["<item>/<path>"],
      "failure_modes": ["<fm-ids>"]
    }
  ],

  "failure_modes": [
    {
      "id": "SYS-<sys>-FM-<seq>",
      "software_item_id": "ITEM-<id>",
      "name": "<failure mode name>",
      "category": "user_error | usability_issue | software_defect | software_crash | data_integrity | configuration_error | deployment_error | interface_mismatch | communication_failure | synchronization_error | infrastructure_failure | resource_exhaustion | external_dependency | security_breach | authentication_failure",
      "core_functionality_id": "CF-<seq>",
      "effect_on_software_item": "<local effect>",
      "effect_on_system": "<system-wide effect>",
      "detection_mechanism": "<how detected>",
      "recovery_mechanism": "<how recovered>",
      "safety_characteristics": ["SC-<seq>"],
      "contributes_to_hazards": ["SYS-<sys>-HAZ-<seq>"],
      "source_module_fm": "MOD-<mod>-FM-<seq>",
      "source": {
        "files": ["<item>/<path>:<lines>"]
      }
    }
  ],

  "risk_acceptability_criteria": {
    "severity_levels": [
      {"level": "S1", "name": "Negligible", "description": "..."},
      {"level": "S2", "name": "Minor", "description": "..."},
      {"level": "S3", "name": "Serious", "description": "..."},
      {"level": "S4", "name": "Critical", "description": "..."},
      {"level": "S5", "name": "Catastrophic", "description": "..."}
    ],
    "probability_levels": [
      {"level": "P1", "name": "Incredible", "description": "..."},
      {"level": "P2", "name": "Remote", "description": "..."},
      {"level": "P3", "name": "Occasional", "description": "..."},
      {"level": "P4", "name": "Probable", "description": "..."},
      {"level": "P5", "name": "Frequent", "description": "..."}
    ],
    "risk_matrix": {
      "acceptable": ["S1P1", "S1P2", "S1P3", "S2P1", "S2P2"],
      "alarp": ["S1P4", "S1P5", "S2P3", "S2P4", "S3P1", "S3P2"],
      "unacceptable": ["S3P3", "S3P4", "S3P5", "S4P*", "S5P*"]
    }
  },

  "intended_use_and_context": {
    "purpose": "<overall system purpose>",
    "intended_users": ["<user types>"],
    "intended_environment": "<operational environment>",
    "contraindications": ["<conditions where product should not be used>"]
  },

  "system_hazards": [
    {
      "id": "SYS-<sys>-HAZ-<seq>",
      "description": "<system-level hazard>",
      "failure_mode_ids": ["SYS-<sys>-FM-<seq>"],
      "safety_characteristic_ids": ["SC-<seq>"],
      "contributing_modules": [
        {
          "module_id": "<module-id>",
          "module_hazard_id": "MOD-<mod>-HAZ-<seq>",
          "contribution": "<how this module contributes>"
        }
      ],
      "hazardous_situation": "<how hazard leads to harm>",
      "potential_harm": "<what harm can occur>",
      "severity": "S1-S5",
      "severity_rationale": "<why this severity>",
      "probability_before_control": "P1-P5",
      "probability_rationale": "<why this probability>",
      "initial_risk_level": "acceptable | alarp | unacceptable"
    }
  ],

  "cross_module_hazards": [
    {
      "id": "SYS-<sys>-XMH-<seq>",
      "description": "<hazard arising from module interactions>",
      "modules_involved": ["<module-ids>"],
      "failure_mode_ids": ["SYS-<sys>-FM-<seq>"],
      "interface": "<interface where hazard arises>",
      "hazardous_situation": "<how hazard leads to harm>",
      "potential_harm": "<what harm can occur>",
      "severity": "S1-S5",
      "probability_before_control": "P1-P5",
      "initial_risk_level": "acceptable | alarp | unacceptable"
    }
  ],

  "risk_controls": {
    "software_controls": [
      {
        "id": "SYS-<sys>-RC-<seq>",
        "description": "<control description>",
        "control_type": "ui_design | software_design | interface_design | security_design | implementation_control | operational_control | monitoring",
        "iso_14971_level": "inherent_safety | protective_measure | information_for_safety",
        "mitigates_hazards": ["SYS-<sys>-HAZ-<seq>"],
        "addresses_failure_modes": ["SYS-<sys>-FM-<seq>"],
        "implementation": "<where/how implemented>",
        "software_items": ["ITEM-<id>"],
        "verification_status": "verified | partial | unverified",
        "verification_evidence": ["<test IDs or references>"]
      }
    ],
    "non_software_controls": [
      {
        "id": "SYS-<sys>-RC-NS-<seq>",
        "description": "<control requiring user/organizational action>",
        "control_type": "training | documentation | labeling | operational_procedure",
        "iso_14971_level": "information_for_safety",
        "mitigates_hazards": ["SYS-<sys>-HAZ-<seq>"],
        "implementation": "IFU | training_program | procedure",
        "verification_status": "verified | partial | unverified"
      }
    ]
  },

  "residual_risk_evaluation": [
    {
      "hazard_id": "SYS-<sys>-HAZ-<seq>",
      "controls_applied": ["SYS-<sys>-RC-<seq>", "SYS-<sys>-RC-NS-<seq>"],
      "probability_after_control": "P1-P5",
      "residual_risk_level": "acceptable | alarp | unacceptable",
      "acceptability_decision": "acceptable | acceptable_with_benefit | unacceptable",
      "benefit_risk_analysis": {
        "required": true | false,
        "clinical_benefit": "<benefit description>",
        "justification": "<why benefit outweighs risk>",
        "disclosure_required": true | false,
        "disclosure_location": "IFU section X"
      }
    }
  ],

  "overall_residual_risk": {
    "evaluation_method": "<how overall risk was assessed>",
    "conclusion": "acceptable | acceptable_with_disclosure | unacceptable",
    "rationale": "<justification for conclusion>",
    "disclosures_required": [
      {
        "risk_id": "SYS-<sys>-HS-<seq>",
        "disclosure_text": "<text for IFU>",
        "location": "IFU section X"
      }
    ]
  },

  "verification_summary": {
    "total_controls": "<count>",
    "verified": "<count>",
    "partial": "<count>",
    "unverified": "<count>",
    "gaps": [
      {
        "id": "SYS-<sys>-GAP-<seq>",
        "description": "<verification gap>",
        "affected_controls": ["<control IDs>"],
        "priority": "high | medium | low",
        "recommendation": "<action needed>"
      }
    ]
  },

  "traceability": {
    "intended_use_to_user_intent": [
      {
        "intended_use_id": "IU-<seq>",
        "user_intent_ids": ["UI-<seq>"]
      }
    ],
    "user_intent_to_core_functionality": [
      {
        "user_intent_id": "UI-<seq>",
        "core_functionality_ids": ["CF-<seq>"]
      }
    ],
    "core_functionality_to_failure_mode": [
      {
        "core_functionality_id": "CF-<seq>",
        "failure_mode_ids": ["SYS-<sys>-FM-<seq>"]
      }
    ],
    "failure_mode_to_hazard": [
      {
        "failure_mode_id": "SYS-<sys>-FM-<seq>",
        "hazard_ids": ["SYS-<sys>-HAZ-<seq>"]
      }
    ],
    "safety_characteristic_to_failure_mode": [
      {
        "safety_characteristic_id": "SC-<seq>",
        "failure_mode_ids": ["SYS-<sys>-FM-<seq>"]
      }
    ],
    "hazard_to_control": [
      {
        "hazard_id": "SYS-<sys>-HAZ-<seq>",
        "control_ids": ["SYS-<sys>-RC-<seq>", "SYS-<sys>-RC-NS-<seq>"]
      }
    ],
    "control_to_verification": [
      {
        "control_id": "SYS-<sys>-RC-<seq>",
        "verification_evidence": ["<test IDs or references>"]
      }
    ]
  }
}
```

## Compliance Mapping

| Output Element | Standard/Clause | Evidence Provided |
|----------------|-----------------|-------------------|
| `safety_characteristics[]` | ISO 14971 Annex C | Systematic hazard identification |
| `safety_characteristics[].applicable` | ISO 14971 5.4 | Applicability assessment |
| `intended_uses[]` | ISO 14971 5.2 | Intended use documentation |
| `user_intents[]` | IEC 62366-1 5.1 | User needs identification |
| `software_items[]` | IEC 62304 5.1.1 | Software item identification |
| `core_functionalities[]` | IEC 62304 5.2 | Software functionality documentation |
| `failure_modes[]` | IEC 62304 7.1.3 | Failure mode analysis |
| `failure_modes[].category` | ISO 14971 7.1 | Control discipline mapping |
| `failure_modes[].safety_characteristics` | ISO 14971 5.4 | Hazard identification traceability |
| `risk_acceptability_criteria` | ISO 14971 4.4 d) | Risk acceptability criteria |
| `intended_use_and_context` | ISO 14971 5.2 | Intended use and misuse |
| `system_hazards[]` | ISO 14971 5.4 | Hazard identification |
| `system_hazards[].failure_mode_ids` | ISO 14971 5.4 | Failure to hazard traceability |
| `system_hazards[].severity` | ISO 14971 5.5 | Risk estimation |
| `system_hazards[].probability_*` | ISO 14971 5.5 | Risk estimation |
| `system_hazards[].initial_risk_level` | ISO 14971 6 | Risk evaluation |
| `risk_controls.software_controls[]` | ISO 14971 7.1 | Software risk control measures |
| `risk_controls.non_software_controls[]` | ISO 14971 7.1 | Non-software risk control measures |
| `risk_controls[].control_type` | ISO 14971 7.1 | Control type classification |
| `risk_controls[].iso_14971_level` | ISO 14971 7.1 | Control priority order |
| `risk_controls[].verification_status` | ISO 14971 7.2 | Implementation verification |
| `residual_risk_evaluation[]` | ISO 14971 7.3 | Residual risk evaluation |
| `residual_risk_evaluation[].benefit_risk_analysis` | ISO 14971 7.4 | Benefit-risk analysis |
| `cross_module_hazards[]` | ISO 14971 7.5 | Risks from risk control measures |
| `overall_residual_risk` | ISO 14971 8 | Overall residual risk evaluation |
| `traceability` | ISO 14971 4.5 | Risk management file traceability |

## Aggregation Rules

### Severity Aggregation
When multiple items contribute to a hazard, use the **highest** severity from any contributor:
```
ITEM-A severity: Serious
ITEM-B severity: Minor
System hazard severity: Serious
```

### Probability Aggregation
Consider **all paths** to the hazardous situation:
- If multiple items can independently cause the hazard, probability increases
- If items must fail together (AND), probability decreases

### Control Aggregation
- **Deduplicate** identical controls from different items
- **Combine** partial controls into complete coverage
- **Identify gaps** where no item provides a control

### Verification Status Aggregation
Use the **weakest** verification status:
```
ITEM-A control: verified
ITEM-B control: partial
Combined status: partial
```

## Validation Criteria

### Safety Characteristics
- [ ] All relevant ISO 14971 Annex C characteristics documented 🆔 AcGWOi
- [ ] Each characteristic has `applicable` with rationale 🆔 D5zdRC
- [ ] Each applicable characteristic lists `identified_hazards` 🆔 9b73rw

### USE Perspective
- [ ] Intended uses defined 🆔 lxRRej
- [ ] User intents defined and linked to intended uses 🆔 VS5vUc
- [ ] Core functionalities linked to user intents 🆔 UTAQ3C

### IMPLEMENTATION Perspective
- [ ] All software items from modules collected 🆔 QHi0HS
- [ ] All failure modes have valid `category` 🆔 uDWCG0
- [ ] All failure modes have `software_item_id` and `core_functionality_id` 🆔 eMpTPu
- [ ] All failure modes have `safety_characteristics` links 🆔 ovvkVk
- [ ] All failure modes have `contributes_to_hazards` links 🆔 kAJM8o

### RISK Perspective
- [ ] Each system hazard has `failure_mode_ids` populated 🆔 tVhVEZ
- [ ] Each system hazard has `safety_characteristic_ids` populated 🆔 Qpych7
- [ ] Risk acceptability criteria are defined 🆔 1NwCMx
- [ ] Every hazard has severity and probability assessment 🆔 4SrTKf
- [ ] Every hazard has residual risk evaluation 🆔 JHeGFN
- [ ] Benefit-risk analysis performed where required 🆔 DV32Zo

### Controls
- [ ] Controls classified by `control_type` based on failure mode categories 🆔 MtFGeD
- [ ] Controls classified by `iso_14971_level` 🆔 Z8if6Z
- [ ] Software and non-software controls separated 🆔 mT1QZk
- [ ] Verification status documented for all controls 🆔 fIUlx3

### Traceability
- [ ] IntendedUse → UserIntent → CoreFunctionality chain complete 🆔 6YuAUd
- [ ] CoreFunctionality → FailureMode → Hazard chain complete 🆔 krW8Pv
- [ ] SafetyCharacteristic → FailureMode chain complete 🆔 SflKLm
- [ ] Hazard → Control → Verification chain complete 🆔 FdRpSj
