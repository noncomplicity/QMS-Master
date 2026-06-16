---
id: 725af57
title: "gap analysis risk data"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Prompt"
category: "analysis"
level: "system"
standard: "ISO 14971, IEC 62304"
clause: "4.5, 7"
inputs: ["item-risk-contribution.json (multiple)", "module-risk.json (multiple)", "system-manifest.json"]
outputs: ["risk-data-gaps.json", "risk-data-gaps.md"]
software_class: "all"
process: "[Risk Management Process](../../Canvases/Risk%20Management%20Process.canvas)"
requirements: "[ISO 14971](../../Requirements/ISO_14971_Requirements.md)"
owner: "[Head of Risk Management](../../Assets/Head%20of%20Risk%20Management.md)"
---

# Gap Analysis for Risk Data Completeness

## Purpose

This prompt analyzes automated extractions and identifies what operator input is needed before system-level risk aggregation can run. It produces:

1. **Summary of what was extracted** from code
2. **Gaps requiring operator input** with templates to fill
3. **Validation issues** in existing extractions
4. **Recommended next steps**

Run this AFTER item/module extraction, BEFORE system aggregation.

## Inputs

### Required
- **`system-manifest.json`** — System structure
- **`items/<item>/extracted/item-risk-contribution.json`** — Item extractions
- **`modules/<module>/aggregated/module-risk.json`** — Module aggregations (if available)

### Optional
- **`system/defined/*.json`** — Any pre-defined inputs already created

## Instructions

### 1. Inventory Extracted Data

For each item and module, catalog what was successfully extracted:

**Software Items:**
- [ ] Item ID and name 🆔 ESmYCq
- [ ] Safety class with rationale 🆔 gorTFZ
- [ ] Version and repository URL 🆔 3Qrupz
- [ ] Technology stack 🆔 JaQy0o

**Failure Modes:**
- [ ] Failure mode ID and name 🆔 gLPRbF
- [ ] Category (from 16 defined categories) 🆔 zGlHWo
- [ ] Effect on software item 🆔 pwfmNK
- [ ] Effect on system 🆔 pO4hmG
- [ ] Detection mechanism 🆔 glcdud
- [ ] Recovery mechanism 🆔 Qnjuj5
- [ ] Source file references 🆔 tAC37B

**Hazard Contributions:**
- [ ] Hazard ID and description 🆔 8rinMY
- [ ] Cause 🆔 jKQ18p
- [ ] Potential harm 🆔 j4TW2z
- [ ] Severity contribution 🆔 WGcsAv

**Risk Controls:**
- [ ] Control ID and description 🆔 TMhilS
- [ ] Control type 🆔 h2rPiT
- [ ] Mitigates which hazards 🆔 gJy2Sv
- [ ] Verification status 🆔 6HXPUj

**Core Functionalities (if module-level exists):**
- [ ] Functionality ID and name 🆔 00kV4b
- [ ] Description 🆔 zd4ozg
- [ ] Source files 🆔 lr3gty
- [ ] Linked failure modes 🆔 SVg87b

### 2. Identify Missing USE Perspective Data

These CANNOT be extracted from code and require operator input:

**Intended Uses (IU-*):**
```
Questions for operator:
- What is the clinical purpose of this system?
- What clinical workflows does it support?
- What patient outcomes does it enable?
```

**User Intents (UI-*):**
```
Questions for operator:
- What are users trying to accomplish?
- What tasks do they perform with this system?
- What decisions do they make using this system?
```

**User Intent → Core Functionality Mapping:**
```
Questions for operator:
- Which core functionalities enable each user intent?
- Are there user intents without supporting functionality? (gap)
- Are there functionalities without a user intent? (orphan code)
```

### 3. Identify Missing RISK Perspective Data

**Safety Characteristics (ISO 14971 Annex C):**

For each characteristic, determine if applicable:

| ID | Question | Applicable? | Rationale Needed |
|----|----------|-------------|------------------|
| C.2.1 | What is the intended use? | ? | Yes |
| C.2.11 | Are measurements taken? | ? | Yes |
| C.2.12 | Is the device interpretive? | ? | Yes |
| C.2.13 | Used with other devices? | ? | Yes |
| C.2.19 | Contains software? | Yes (always) | No |
| C.2.26 | Requires special training? | ? | Yes |
| C.2.28 | Dependent on human factors? | ? | Yes |
| C.2.29 | Uses alarm system? | ? | Yes |
| C.2.30 | Can be deliberately misused? | ? | Yes |
| C.2.31 | Holds critical patient data? | ? | Yes |
| C.2.33 | Has essential performance? | ? | Yes |

**Severity Assessments:**
```
For each hazard contribution, operator must assess:
- Actual severity (S1-S5) not just "contribution"
- Severity rationale based on clinical context
- Potential harm description in clinical terms
```

**Probability Assessments:**
```
For each hazard:
- Probability before controls (P1-P5)
- Probability rationale
- Probability after controls
```

**Risk Acceptability:**
```
Operator must define:
- Severity level definitions (S1-S5)
- Probability level definitions (P1-P5)
- Risk matrix (which combinations are acceptable/ALARP/unacceptable)
- Benefit-risk policy
```

### 4. Identify Linking Gaps

**Failure Mode → Safety Characteristic:**
```
Each failure mode should link to at least one safety characteristic.
Missing links indicate either:
- Incomplete safety characteristic assessment
- Failure mode that doesn't relate to identified safety concerns
```

**Failure Mode → Core Functionality:**
```
Each failure mode should link to a core functionality.
Missing links indicate either:
- Orphan failure mode (code that doesn't serve a user intent)
- Missing core functionality definition
```

**Core Functionality → User Intent:**
```
Each core functionality should link to a user intent.
Missing links indicate either:
- Functionality without clear user purpose
- Missing user intent definition
```

**Hazard → Failure Mode:**
```
Each hazard should link to at least one failure mode.
Missing links indicate either:
- Hazard without identified cause (risk analysis gap)
- Failure mode not linked to consequences
```

### 5. Validation Issues

Check for issues in extracted data:

**ID Consistency:**
- Item IDs match manifest
- Module IDs match manifest
- Cross-references resolve

**Category Validity:**
- All failure mode categories are from the defined list
- All control types are valid

**Completeness:**
- All items in manifest have extractions
- All modules in manifest have aggregations (if run)

### 6. Generate Gap Report

## Output Schema

### risk-data-gaps.json

```json
{
  "analysis_metadata": {
    "analyzed_at": "<ISO-8601 timestamp>",
    "system_id": "<system-id>",
    "items_analyzed": ["<item-ids>"],
    "modules_analyzed": ["<module-ids>"]
  },

  "extraction_summary": {
    "software_items": {
      "count": 0,
      "items": [
        {
          "id": "ITEM-<id>",
          "name": "<name>",
          "safety_class": "<class or MISSING>",
          "failure_modes_count": 0,
          "hazard_contributions_count": 0,
          "controls_count": 0
        }
      ]
    },
    "failure_modes": {
      "total": 0,
      "by_category": {
        "software_crash": 0,
        "data_integrity": 0,
        "communication_failure": 0
      },
      "missing_category": [],
      "missing_effect_on_system": []
    },
    "core_functionalities": {
      "total": 0,
      "with_failure_modes": 0,
      "without_failure_modes": []
    }
  },

  "gaps_requiring_input": {
    "intended_uses": {
      "status": "MISSING | PARTIAL | COMPLETE",
      "message": "<what's needed>",
      "template": {
        "intended_uses": [
          {
            "id": "IU-001",
            "name": "<FILL: clinical purpose name>",
            "description": "<FILL: what clinical need this addresses>",
            "user_intents": ["UI-001"]
          }
        ]
      }
    },

    "user_intents": {
      "status": "MISSING | PARTIAL | COMPLETE",
      "message": "<what's needed>",
      "suggested_from_code": [
        {
          "suggested_id": "UI-001",
          "suggested_name": "<inferred from API endpoints>",
          "evidence": "<what code suggests this>",
          "needs_confirmation": true
        }
      ],
      "template": {
        "user_intents": [
          {
            "id": "UI-001",
            "name": "<FILL: what user wants to do>",
            "description": "<FILL: user goal description>",
            "intended_use_id": "IU-001",
            "core_functionalities": ["CF-001"]
          }
        ]
      }
    },

    "safety_characteristics": {
      "status": "MISSING | PARTIAL | COMPLETE",
      "message": "<what's needed>",
      "characteristics_to_assess": [
        {
          "id": "SC-001",
          "category": "software",
          "question": "Does the device contain software?",
          "annex_c_ref": "C.2.19",
          "suggested_applicable": true,
          "suggested_rationale": "System is software-as-medical-device",
          "needs_confirmation": true
        },
        {
          "id": "SC-002",
          "category": "critical_data",
          "question": "Does the device hold data critical to patient care?",
          "annex_c_ref": "C.2.31",
          "suggested_applicable": null,
          "suggested_rationale": null,
          "needs_confirmation": true,
          "evidence_from_code": "<data entities found in code>"
        }
      ]
    },

    "risk_acceptability_criteria": {
      "status": "MISSING",
      "message": "Organization must define severity/probability levels and acceptability matrix",
      "template": {
        "severity_levels": [
          {"level": "S1", "name": "Negligible", "description": "<FILL>"},
          {"level": "S2", "name": "Minor", "description": "<FILL>"},
          {"level": "S3", "name": "Serious", "description": "<FILL>"},
          {"level": "S4", "name": "Critical", "description": "<FILL>"},
          {"level": "S5", "name": "Catastrophic", "description": "<FILL>"}
        ],
        "probability_levels": [
          {"level": "P1", "name": "Incredible", "description": "<FILL>"},
          {"level": "P2", "name": "Remote", "description": "<FILL>"},
          {"level": "P3", "name": "Occasional", "description": "<FILL>"},
          {"level": "P4", "name": "Probable", "description": "<FILL>"},
          {"level": "P5", "name": "Frequent", "description": "<FILL>"}
        ],
        "risk_matrix": {
          "acceptable": ["<FILL: e.g., S1P1, S1P2, S2P1>"],
          "alarp": ["<FILL: e.g., S2P2, S2P3, S3P1>"],
          "unacceptable": ["<FILL: e.g., S4P*, S5P*>"]
        }
      }
    },

    "severity_assessments": {
      "status": "MISSING | PARTIAL | COMPLETE",
      "hazards_needing_assessment": [
        {
          "hazard_id": "ITEM-HM-HAZ-001",
          "description": "<hazard description>",
          "extracted_severity_contribution": "serious",
          "needs": ["severity_level", "severity_rationale", "probability_level", "probability_rationale"]
        }
      ]
    },

    "user_intent_to_functionality_mapping": {
      "status": "MISSING | PARTIAL | COMPLETE",
      "core_functionalities_without_user_intent": ["CF-001", "CF-002"],
      "message": "Map each core functionality to a user intent"
    }
  },

  "linking_gaps": {
    "failure_modes_without_safety_characteristic": [
      {
        "fm_id": "ITEM-HM-FM-001",
        "fm_name": "<name>",
        "suggested_characteristics": ["SC-001"]
      }
    ],
    "failure_modes_without_core_functionality": [
      {
        "fm_id": "ITEM-HM-FM-002",
        "fm_name": "<name>",
        "suggested_functionality": "<inferred from source file>"
      }
    ],
    "hazards_without_failure_modes": [
      {
        "hazard_id": "ITEM-HM-HAZ-003",
        "hazard_description": "<description>",
        "message": "No failure mode linked - identify root cause"
      }
    ]
  },

  "validation_issues": [
    {
      "type": "invalid_category",
      "location": "ITEM-HM-FM-001",
      "issue": "Category 'software_failure' not in defined list",
      "suggestion": "Use 'software_crash' or 'software_defect'"
    },
    {
      "type": "missing_extraction",
      "location": "items/booking-service",
      "issue": "Item in manifest but no extraction found"
    }
  ],

  "recommended_actions": [
    {
      "priority": 1,
      "action": "Define risk acceptability criteria",
      "reason": "Required for all risk evaluations",
      "output_file": "system/defined/risk-criteria.json"
    },
    {
      "priority": 2,
      "action": "Assess safety characteristics",
      "reason": "10 characteristics need applicability decision",
      "output_file": "system/defined/safety-characteristics.json"
    },
    {
      "priority": 3,
      "action": "Define intended uses and user intents",
      "reason": "Required for USE perspective traceability",
      "output_file": "system/defined/intended-use.json"
    },
    {
      "priority": 4,
      "action": "Map core functionalities to user intents",
      "reason": "5 functionalities lack user intent mapping",
      "output_file": "system/defined/functionality-mapping.json"
    },
    {
      "priority": 5,
      "action": "Complete severity/probability assessments",
      "reason": "8 hazards need clinical risk assessment",
      "output_file": "system/defined/risk-assessments.json"
    }
  ],

  "readiness_assessment": {
    "can_run_system_aggregation": false,
    "blocking_gaps": [
      "risk_acceptability_criteria",
      "safety_characteristics",
      "intended_uses"
    ],
    "completion_percentage": 45,
    "estimated_effort": "Operator input needed for 5 areas"
  }
}
```

### risk-data-gaps.md (Human-Readable Report)

Also generate a markdown report summarizing:

```markdown
# Risk Data Gap Analysis Report

**System:** <system-id>
**Analyzed:** <timestamp>
**Readiness:** 45% - Cannot run system aggregation yet

## Extraction Summary

| Item | Safety Class | Failure Modes | Hazards | Controls |
|------|--------------|---------------|---------|----------|
| health-manager | B | 12 | 5 | 8 |
| booking-service | MISSING | 0 | 0 | 0 |

## Gaps Requiring Operator Input

### 1. Risk Acceptability Criteria (BLOCKING)
Organization must define severity/probability levels and risk matrix.
See template in `risk-data-gaps.json`.

### 2. Safety Characteristics (BLOCKING)
10 of 11 characteristics need applicability assessment.
...

### 3. Intended Uses (BLOCKING)
No intended uses defined. Cannot establish USE perspective.
...

## Recommended Actions

1. [ ] Define risk acceptability criteria → `system/defined/risk-criteria.json` 🆔 mPnwNm
2. [ ] Assess safety characteristics → `system/defined/safety-characteristics.json` 🆔 wG0npH
3. [ ] Define intended uses → `system/defined/intended-use.json` 🆔 w8IIwe
4. [ ] Map functionalities to user intents 🆔 PEJnKL
5. [ ] Complete severity/probability assessments 🆔 pxjNEF

## Next Steps

After completing the above:
1. Place files in `system/defined/`
2. Re-run gap analysis to verify completeness
3. Run system aggregation: `system-aggregation-risk-file.md`
```

## Validation Criteria

- [ ] All items in manifest are analyzed 🆔 Jsk1N8
- [ ] All modules in manifest are analyzed (if aggregations exist) 🆔 yaUrax
- [ ] Each gap category has clear status (MISSING/PARTIAL/COMPLETE) 🆔 8hjTSF
- [ ] Templates provided for all MISSING items 🆔 7dAatT
- [ ] Suggested values provided where code gives hints 🆔 EIm2Fg
- [ ] Blocking gaps clearly identified 🆔 ombcuV
- [ ] Recommended actions prioritized 🆔 o3kU97
- [ ] Readiness assessment accurate 🆔 972BX0
