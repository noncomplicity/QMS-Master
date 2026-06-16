---
id:
title: "Extract Health Software Product Validation Evidence"
version:
author:
effective_date:
type: "Prompt"
category: "extraction"
standard: "IEC 82304-1"
clause: "6"
inputs: ["use-requirements.json", "test results", "validation records", "user acceptance testing", "clinical evaluation"]
outputs: ["validation.json"]
software_class: "all"
process: "[Software Development Process](../../Canvases/Software%20Development%20Process.canvas)"
requirements:
owner: "[Head of Quality Management](../../Assets/Head%20of%20Quality%20Management.md)"
---

# Extract Health Software Product Validation Evidence

## Context

IEC 82304-1 clause 6 requires product validation—confirmation that the health software product meets its use requirements and is suitable for its intended use. This is distinct from IEC 62304 verification (confirming software meets software requirements).

```
Verification (IEC 62304)     → Software meets software requirements
Validation (IEC 82304-1)     → Product meets use requirements (fit for intended use)
```

Validation demonstrates the product works as intended for real users in real operational environments.

## Inputs

### From Previous Extractions
- `use-requirements.json` — Use requirements to validate against
- `system-requirements.json` — System requirements
- `verification.json` — Software verification results (from 62304)

### Validation Planning
- **Validation plan documents**: Scope, methods, criteria
- **Test protocols**: Acceptance test procedures
- **Environment specifications**: Validation environment setup

### Validation Execution
- **Acceptance test results**: User acceptance testing records
- **Usability test results**: User testing data
- **Clinical evaluation data**: Clinical performance evidence
- **Field trial results**: Beta testing, pilot deployments
- **Staging/UAT environments**: Test environment configs

### Personnel Records
- **Validation team composition**: Who performed validation
- **Training records**: Team qualifications
- **Independence documentation**: Separation from design team

### Risk Management
- **Risk management file**: Residual risk evaluation
- **SOUP anomaly evaluation**: Third-party component risks

## Instructions

### 1. Extract Validation Plan Elements (6.1)

Document the validation plan:

a) **Validation Scope and Activities (6.1 a)**
   - Which use requirements are in scope
   - What validation activities are planned
   - Validation phases and milestones

b) **Constraints (6.1 b)**
   - Technical feasibility limits
   - Resource constraints
   - Time constraints
   - Environment limitations

c) **Validation Methods (6.1 c)**
   - Methods per requirement (inspection, analysis, demonstration, simulation, testing)
   - Input information required
   - Acceptance criteria for each requirement

d) **Enabling Systems (6.1 d)**
   - Operating environments used
   - Hardware platforms
   - Software platforms
   - Test tools and frameworks

e) **Personnel Qualification (6.1 e)**
   - Required qualifications
   - Training completed
   - Team member roles

f) **Independence Level (6.1 f)**
   - Separation from design team
   - Organizational independence
   - Process independence

### 2. Extract Validation Execution Evidence (6.2)

Document validation activities performed:
- Readiness confirmation
- Activities performed per plan
- Deviations from plan (with justification)
- Anomalies found and resolution
- Re-validation after modifications

### 3. Extract Validation Results (6.3)

For each use requirement, document:
- Validation method used
- Validation result (pass/fail)
- Evidence of meeting requirement
- Residual risk assessment
- Anomalies and their disposition

### 4. Extract Re-Validation Evidence (8.3)

For software maintenance changes:
- Affected requirements
- Re-validation scope
- Re-validation results
- Platform compatibility confirmation

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

  "validation_plan": {
    "plan_id": "<plan identifier>",
    "version": "<plan version>",
    "date": "<plan date>",

    "scope": {
      "description": "<validation scope description>",
      "use_requirements_covered": ["<USE-* ids>"],
      "exclusions": [
        {
          "requirement": "<USE-* id>",
          "reason": "<why excluded>"
        }
      ],
      "phases": [
        {
          "phase": "<phase name>",
          "activities": ["<activities>"],
          "entry_criteria": ["<criteria>"],
          "exit_criteria": ["<criteria>"]
        }
      ]
    },

    "constraints": [
      {
        "constraint_type": "technical | cost | time | resource | contractual",
        "description": "<constraint description>",
        "impact": "<impact on validation>",
        "mitigation": "<how addressed>"
      }
    ],

    "methods": [
      {
        "method_id": "VM-<seq>",
        "method_type": "inspection | analysis | analogy | demonstration | simulation | peer_review | testing | certification",
        "description": "<method description>",
        "applicable_to": ["<USE-* ids>"],
        "input_information": ["<required inputs>"],
        "acceptance_criteria": ["<criteria>"],
        "tools": ["<tools used>"]
      }
    ],

    "enabling_systems": {
      "operating_environments": [
        {
          "name": "<environment name>",
          "purpose": "<staging | uat | production-like>",
          "configuration": "<config description>",
          "os": "<operating system>",
          "hardware": "<hardware specs>",
          "software_dependencies": ["<dependencies>"]
        }
      ],
      "test_tools": [
        {
          "name": "<tool name>",
          "version": "<version>",
          "purpose": "<what it's used for>"
        }
      ]
    },

    "personnel": {
      "qualification_requirements": [
        {
          "role": "<role>",
          "required_qualifications": ["<qualifications>"],
          "required_training": ["<training>"]
        }
      ],
      "team_members": [
        {
          "name": "<name or ID>",
          "role": "<role>",
          "qualifications": ["<qualifications>"],
          "training_completed": ["<training>"],
          "training_date": "<date>"
        }
      ]
    },

    "independence": {
      "level": "organizational | process | none",
      "description": "<how independence is achieved>",
      "design_team_overlap": ["<any overlap and justification>"],
      "verification_method": "<how independence is verified>"
    },

    "source": {
      "files": ["<plan source files>"]
    }
  },

  "validation_execution": {
    "execution_id": "<execution identifier>",
    "start_date": "<start date>",
    "end_date": "<end date>",
    "status": "planned | in_progress | completed | blocked",

    "readiness_confirmation": {
      "confirmed": true,
      "confirmed_by": "<who confirmed>",
      "confirmed_date": "<date>",
      "criteria_met": [
        {
          "criterion": "<readiness criterion>",
          "evidence": "<evidence>"
        }
      ]
    },

    "activities_performed": [
      {
        "activity_id": "VA-<seq>",
        "activity_name": "<activity name>",
        "method_used": "<VM-* id>",
        "requirements_addressed": ["<USE-* ids>"],
        "environment": "<environment used>",
        "executed_by": "<who executed>",
        "execution_date": "<date>",
        "status": "passed | failed | blocked | not_run",
        "evidence": {
          "files": ["<evidence files>"],
          "description": "<evidence description>"
        }
      }
    ],

    "deviations": [
      {
        "deviation_id": "DEV-<seq>",
        "description": "<what deviated from plan>",
        "justification": "<why deviation was necessary>",
        "impact": "<impact on validation>",
        "approved_by": "<who approved>"
      }
    ],

    "anomalies": [
      {
        "anomaly_id": "ANOM-<seq>",
        "description": "<anomaly description>",
        "severity": "critical | major | minor",
        "requirement_affected": "<USE-* id>",
        "detected_date": "<date>",
        "resolution": {
          "action": "<action taken>",
          "resolved_date": "<date>",
          "revalidation_required": true,
          "revalidation_reference": "<VA-* id>"
        }
      }
    ]
  },

  "validation_results": {
    "summary": {
      "total_requirements": 0,
      "requirements_validated": 0,
      "requirements_passed": 0,
      "requirements_failed": 0,
      "requirements_not_validated": 0
    },

    "requirement_results": [
      {
        "use_requirement": "<USE-* id>",
        "validation_method": "<VM-* id>",
        "validation_activity": "<VA-* id>",
        "result": "pass | fail | not_validated",
        "evidence": {
          "description": "<evidence description>",
          "files": ["<evidence files>"],
          "test_data": "<test data reference>"
        },
        "notes": "<additional notes>"
      }
    ],

    "residual_risk_evaluation": {
      "evaluated": true,
      "evaluation_date": "<date>",
      "evaluated_by": "<who evaluated>",
      "conclusion": "acceptable | not_acceptable",
      "rationale": "<rationale for conclusion>",
      "risk_file_reference": "<risk management file reference>"
    },

    "anomaly_summary": {
      "total_anomalies": 0,
      "open_anomalies": 0,
      "closed_anomalies": 0,
      "anomalies_by_severity": {
        "critical": 0,
        "major": 0,
        "minor": 0
      }
    }
  },

  "validation_report": {
    "report_id": "<report identifier>",
    "report_date": "<date>",
    "version": "<version>",

    "traceability": {
      "coverage": [
        {
          "use_requirement": "<USE-* id>",
          "validation_activities": ["<VA-* ids>"],
          "result": "<pass | fail>",
          "coverage_status": "complete | partial | none"
        }
      ],
      "gaps": [
        {
          "use_requirement": "<USE-* id>",
          "gap_description": "<what is not covered>",
          "reason": "<why not covered>",
          "mitigation": "<how addressed>"
        }
      ]
    },

    "compliance_statement": {
      "meets_use_requirements": true,
      "exceptions": ["<any exceptions>"],
      "conditions": ["<any conditions>"]
    },

    "residual_risk_statement": {
      "acceptable": true,
      "rationale": "<rationale>",
      "reference": "<risk management file reference>"
    },

    "team": [
      {
        "name": "<name>",
        "role": "<role>",
        "affiliation": "<organization>"
      }
    ],

    "conclusion": {
      "validated_for_intended_use": true,
      "summary": "<validation conclusion summary>",
      "recommendations": ["<any recommendations>"],
      "conditions_for_release": ["<any conditions>"]
    }
  },

  "revalidation_history": [
    {
      "revalidation_id": "REVAL-<seq>",
      "trigger": "<what triggered revalidation>",
      "change_reference": "<change request/PR reference>",
      "scope": {
        "affected_requirements": ["<USE-* ids>"],
        "revalidation_activities": ["<VA-* ids>"]
      },
      "date": "<date>",
      "result": "pass | fail",
      "platform_compatibility_confirmed": true,
      "source": {
        "files": ["<evidence files>"]
      }
    }
  ],

  "source_files": {
    "validation_plan": ["<plan files>"],
    "test_protocols": ["<protocol files>"],
    "test_results": ["<result files>"],
    "environment_configs": ["<config files>"],
    "training_records": ["<training files>"]
  }
}
```

## Compliance Mapping

| Output Element | IEC 82304-1 Clause | Evidence Provided |
|----------------|-------------------|-------------------|
| `validation_plan.scope` | 6.1 a) | Validation scope and activities |
| `validation_plan.constraints` | 6.1 b) | Constraints on validation |
| `validation_plan.methods` | 6.1 c) | Validation methods and criteria |
| `validation_plan.enabling_systems` | 6.1 d) | Operating environments and tools |
| `validation_plan.personnel` | 6.1 e) | Personnel qualification |
| `validation_plan.independence` | 6.1 f) | Independence from design team |
| `validation_execution.readiness_confirmation` | 6.2 | Readiness for validation |
| `validation_execution.activities_performed` | 6.2 | Validation activities |
| `validation_execution.deviations` | 6.2 | Deviations with justification |
| `validation_execution.anomalies` | 6.2 | Anomalies found |
| `validation_report.traceability` | 6.3 a) | Traceability to use requirements |
| `validation_report.compliance_statement` | 6.3 b) | Evidence of meeting requirements |
| `validation_results.residual_risk_evaluation` | 6.3 c) | Residual risk acceptable |
| `validation_report.team` | 6.3 | Validation team members |
| `validation_report.conclusion` | 6.3 | Validation conclusion |
| `revalidation_history` | 8.3 | Re-validation evidence |

## Examples

### Input: Test Results File

```
# Acceptance Test Results
Test Suite: Patient Data Entry Validation
Environment: Staging (iOS 16.5, iPhone 14)
Tester: Jane Smith (QA Lead)
Date: 2024-03-15

## Test Cases

### TC-001: Blood Pressure Entry
Requirement: USE-IF-001
Steps: Enter BP reading 120/80
Expected: Reading accepted and displayed
Result: PASS
Evidence: Screenshot attached

### TC-002: Invalid BP Rejection
Requirement: USE-IF-001
Steps: Enter BP reading 500/400
Expected: Error message displayed
Result: PASS
Evidence: Screenshot attached
```

### Output: Extracted Validation (partial)

```json
{
  "validation_execution": {
    "activities_performed": [
      {
        "activity_id": "VA-001",
        "activity_name": "Patient Data Entry Validation",
        "method_used": "VM-TESTING",
        "requirements_addressed": ["USE-IF-001"],
        "environment": "Staging (iOS 16.5, iPhone 14)",
        "executed_by": "Jane Smith (QA Lead)",
        "execution_date": "2024-03-15",
        "status": "passed",
        "evidence": {
          "description": "Acceptance test results with screenshots"
        }
      }
    ]
  },

  "validation_results": {
    "requirement_results": [
      {
        "use_requirement": "USE-IF-001",
        "validation_method": "VM-TESTING",
        "validation_activity": "VA-001",
        "result": "pass",
        "evidence": {
          "description": "Blood pressure entry accepts valid values (120/80) and rejects invalid values (500/400) with appropriate error message"
        }
      }
    ]
  }
}
```

## Validation Criteria

- [ ] All use requirements are included in validation scope or exclusions justified 🆔 lma7Zp
- [ ] Validation methods are appropriate for each requirement type 🆔 OkJwmp
- [ ] Acceptance criteria are defined for each requirement 🆔 FGsFJ6
- [ ] Enabling systems match intended operational environment 🆔 ydAjYE
- [ ] Personnel qualifications are documented 🆔 B2CEra
- [ ] Independence from design team is established 🆔 hCFnCr
- [ ] Validation activities are traceable to use requirements 🆔 ZCHkfO
- [ ] Anomalies are documented with resolution 🆔 lORTKp
- [ ] Residual risk evaluation is documented 🆔 W6tJ05
- [ ] Validation conclusion states fitness for intended use 🆔 Xtk5cj
- [ ] Re-validation scope is appropriate to changes made 🆔 olOG6w
