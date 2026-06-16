---
id:
title: "Aggregate System Verification Documentation"
version:
author:
effective_date:
type: "Prompt"
category: "aggregation"
level: "system"
standard: "IEC 62304"
clause: "5.5, 5.6, 5.7, 9"
inputs: ["item-verification.json (multiple)", "module-verification.json (multiple)"]
outputs: ["system-verification.json"]
software_class: "all"
process: "[Software Development Process](../../../Canvases/Software%20Development%20Process.canvas)"
requirements: "[IEC 62304](../../../Requirements/IEC_62304_Requirements.md)"
owner: "[Head of Quality Management](../../../Assets/Head%20of%20Quality%20Management.md)"
---

# Aggregate System Verification Documentation

## Critical Distinction: Documentation vs Execution

> **This prompt aggregates test case documentation to the system level and documents system test cases.**
> **This prompt does NOT report execution results, pass/fail status, or coverage metrics.**

See `_documentation-vs-execution.md` for the full rationale.

**What we aggregate/document:**
- Test case documentation from all modules/items
- System-level test cases
- System requirement-to-test traceability
- Risk control-to-test traceability
- Complete verification gaps across the system

**What we do NOT include:**
- ❌ Pass/fail status (execution result)
- ❌ Coverage percentages (requires execution)
- ❌ Execution timestamps (runtime metadata)
- ❌ "Release readiness" based on test results
- ❌ Anomaly lists with resolution status (execution evidence)

## Context

IEC 62304 clauses 5.5-5.7 require verification documentation at unit, integration, and system levels. For multi-item systems, verification documentation must be aggregated to demonstrate complete verification planning and test case coverage.

**Regulatory purpose:** Document that test cases exist to verify all system requirements and risk controls. This aggregation provides the complete traceability matrix from system requirements to test cases. Execution evidence is maintained separately by CI/CD systems.

This aggregation prompt:
1. Collects verification documentation from all modules and items
2. Aggregates test case counts and traceability
3. Documents system-level test cases
4. Creates system requirement verification traceability
5. Identifies verification gaps
6. Produces `system-verification.json`

## Inputs

### Required
- **`items/<item>/extracted/item-verification.json`** — Test case documentation from each item
- **`system-manifest.json`** — System structure defining modules and items
- **`system/extracted/system-requirements.json`** — System requirements

### Optional (enriches output)
- **`modules/<module>/aggregated/module-verification.json`** — Module-level aggregations
- **`system/extracted/system-risk-file.json`** — System-level risk controls

### NOT Inputs (Execution Artifacts)
The following are explicitly **out of scope**:
- ~~CI/CD reports~~
- ~~JUnit/pytest execution reports~~
- ~~Coverage reports~~
- ~~Anomaly/defect databases~~

## Instructions

### 1. Collect Verification Documentation

For each software item defined in `system-manifest.json`:
1. Load `item-verification.json`
2. Validate hierarchy metadata matches manifest
3. Collect all test case documentation

### 2. Aggregate Test Case Counts

Consolidate test case documentation across all items:
- Total test cases documented
- Test cases by type (unit, integration)
- Test cases by acceptance criteria type

### 3. Aggregate Requirement Traceability

Combine requirement-to-test traceability:
- System requirements → Item requirements → Test cases
- Complete traceability chain
- Identify untraced requirements

### 4. Aggregate Risk Control Traceability

Combine risk control-to-test traceability:
- System-level controls → Test cases
- Item-level controls → Test cases
- Identify untraced controls

### 5. Document System Test Cases

Document cross-item and system-level test cases:
- System integration test cases
- End-to-end test cases
- Validation test cases
- Test specifications for each

### 6. Create Traceability Matrix

Build complete verification traceability:
- System requirement → Item requirement → Test case
- Risk control → Test case
- Component → Test case

### 7. Identify Verification Gaps

Aggregate and identify gaps:
- System requirements without test cases
- Risk controls without test cases
- Components without test cases
- Missing acceptance criteria types

## Output Schema

```json
{
  "aggregation_metadata": {
    "aggregated_at": "<ISO-8601 timestamp>",
    "aggregator_version": "2.1",
    "standard": "IEC 62304:2006+AMD1:2015",
    "hierarchy": {
      "level": "system",
      "system_id": "<system-id>",
      "system_name": "<system name>",
      "child_modules": ["<module-ids>"],
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

  "summary": {
    "total_test_cases": 0,
    "unit_test_cases": 0,
    "integration_test_cases": 0,
    "system_test_cases": 0,
    "system_requirements_total": 0,
    "system_requirements_with_tests": 0,
    "system_requirements_without_tests": 0,
    "risk_controls_total": 0,
    "risk_controls_with_tests": 0,
    "risk_controls_without_tests": 0,
    "verification_gaps_count": 0
  },

  "test_documentation_by_item": [
    {
      "item_id": "<item-id>",
      "item_name": "<name>",
      "test_cases": {
        "unit_test_cases": 0,
        "integration_test_cases": 0,
        "total": 0
      },
      "requirement_traceability": {
        "requirements_total": 0,
        "requirements_with_tests": 0,
        "requirements_without_tests": 0
      },
      "risk_control_traceability": {
        "controls_total": 0,
        "controls_with_tests": 0,
        "controls_without_tests": 0
      },
      "acceptance_criteria_coverage": {
        "functional": 0,
        "event_sequence": 0,
        "data_flow": 0,
        "fault_handling": 0,
        "boundary": 0,
        "initialization": 0
      }
    }
  ],

  "test_documentation_by_module": [
    {
      "module_id": "<module-id>",
      "module_name": "<name>",
      "items": ["<item-ids>"],
      "total_test_cases": 0,
      "integration_test_cases": 0,
      "requirements_with_tests": 0,
      "requirements_without_tests": 0,
      "controls_with_tests": 0,
      "controls_without_tests": 0
    }
  ],

  "system_test_documentation": {
    "system_test_cases": [
      {
        "id": "SYS-<sys>-ST-<seq>",
        "test_name": "<test name>",
        "test_file": "<path>:<line-start>-<line-end>",
        "test_type": "system | e2e | acceptance | validation",
        "requirements_verified": ["SYS-<sys>-REQ-<seq>"],
        "test_specification": {
          "description": "<what this test verifies>",
          "input_stimuli": "<inputs>",
          "expected_outcome": "<expected>",
          "pass_fail_criteria": "<criteria>"
        }
      }
    ],

    "end_to_end_test_cases": [
      {
        "id": "SYS-<sys>-E2E-<seq>",
        "test_name": "<flow name>",
        "test_file": "<path>:<line>",
        "description": "<end-to-end scenario>",
        "items_involved": ["<item-ids>"],
        "test_specification": {
          "description": "<what this test verifies>",
          "input_stimuli": "<inputs>",
          "expected_outcome": "<expected>",
          "pass_fail_criteria": "<criteria>"
        }
      }
    ]
  },

  "system_requirement_traceability": {
    "requirements": [
      {
        "requirement_id": "SYS-<sys>-REQ-<seq>",
        "requirement_title": "<requirement title>",
        "requirement_type": "functional | interface | performance | safety | security",
        "traceability_status": "traced | partial | untraced",
        "item_requirements": [
          {
            "item_id": "<item-id>",
            "item_req_id": "ITEM-<item>-REQ-<seq>"
          }
        ],
        "test_cases": [
          {
            "test_id": "<test ID>",
            "test_level": "unit | integration | system",
            "item_id": "<item-id or null for system tests>"
          }
        ]
      }
    ],

    "traceability_summary": {
      "total_requirements": 0,
      "traced": 0,
      "partial": 0,
      "untraced": 0
    }
  },

  "risk_control_traceability": {
    "controls": [
      {
        "control_id": "SYS-<sys>-RC-<seq>",
        "control_description": "<control>",
        "hazard_id": "SYS-<sys>-HAZ-<seq>",
        "traceability_status": "traced | partial | untraced",
        "test_cases": [
          {
            "test_id": "<test ID>",
            "test_level": "unit | integration | system",
            "item_id": "<item-id or null>"
          }
        ]
      }
    ],

    "traceability_summary": {
      "total_controls": 0,
      "traced": 0,
      "partial": 0,
      "untraced": 0
    }
  },

  "acceptance_criteria_coverage_system": {
    "by_type": {
      "functional": {
        "items_with_coverage": 0,
        "items_total": 0,
        "test_case_count": 0
      },
      "event_sequence": {
        "items_with_coverage": 0,
        "items_total": 0,
        "test_case_count": 0
      },
      "data_flow": {
        "items_with_coverage": 0,
        "items_total": 0,
        "test_case_count": 0
      },
      "fault_handling": {
        "items_with_coverage": 0,
        "items_total": 0,
        "test_case_count": 0
      },
      "boundary": {
        "items_with_coverage": 0,
        "items_total": 0,
        "test_case_count": 0
      },
      "initialization": {
        "items_with_coverage": 0,
        "items_total": 0,
        "test_case_count": 0
      }
    },
    "missing_criteria_by_item": [
      {
        "item_id": "<item-id>",
        "missing_criteria": ["<criteria types>"]
      }
    ]
  },

  "verification_gaps": [
    {
      "id": "SYS-<sys>-VER-GAP-<seq>",
      "gap_type": "untraced_system_requirement | untraced_risk_control | missing_system_test | missing_acceptance_criteria",
      "description": "<what test case is missing>",
      "affected_items": ["<item-ids>"],
      "affected_element": "<ID of requirement, control>",
      "iec_62304_clause": "<clause>",
      "priority": "high | medium | low",
      "recommendation": "<what test case should be added>"
    }
  ],

  "traceability_matrix": {
    "system_requirement_to_item_requirement": [
      {
        "system_req": "SYS-<sys>-REQ-<seq>",
        "item_reqs": ["ITEM-<item>-REQ-<seq>"],
        "status": "complete | partial | missing"
      }
    ],
    "requirement_to_test": [
      {
        "requirement_id": "<req ID>",
        "test_ids": ["<test IDs>"]
      }
    ],
    "risk_to_test": [
      {
        "risk_control_id": "SYS-<sys>-RC-<seq>",
        "test_ids": ["<test IDs>"]
      }
    ],
    "component_to_test": [
      {
        "component_id": "ITEM-<item>-COMP-<seq>",
        "test_ids": ["<test IDs>"]
      }
    ]
  }
}
```

## Compliance Mapping

| Output Element | IEC 62304 Clause | Documentation Provided |
|----------------|------------------|------------------------|
| `test_documentation_by_item[]` | 5.5.1-5.5.5 | Unit test case documentation |
| `test_documentation_by_item[].acceptance_criteria_coverage` | 5.5.3, 5.5.4 | Acceptance criteria coverage |
| `test_documentation_by_module[]` | 5.6.1-5.6.7 | Integration test documentation |
| `system_test_documentation.system_test_cases[]` | 5.7.1, 5.7.5 | System test documentation |
| `system_requirement_traceability` | 5.7.4 | Requirements traceability |
| `risk_control_traceability` | 7.2 | Risk control verification traceability |
| `verification_gaps[]` | 5.7.1 | Verification gap identification |
| `traceability_matrix` | 5.1.1 | Complete traceability |

## Aggregation Rules

### Test Case Count Aggregation
```
System total = Σ(item unit tests) + Σ(item integration tests) + Σ(module integration tests) + system tests
```

### Traceability Status Rules
- **traced**: Test case(s) exist and are linked
- **partial**: Some test cases exist but gaps remain
- **untraced**: No test case exists

### Gap Priority Rules
| Gap Type | Default Priority |
|----------|------------------|
| Untraced safety requirement | high |
| Untraced risk control for serious/critical hazard | high |
| Missing system test for critical function | high |
| Untraced functional requirement | medium |
| Missing acceptance criteria type | medium |
| Untraced low-risk control | low |

## Validation Criteria

- [ ] All items from system-manifest.json are included 🆔 txVKEL
- [ ] Test case counts match item documentation 🆔 RY6lLV
- [ ] System requirements have traceability to test cases 🆔 J42SvL
- [ ] Risk controls have traceability to test cases 🆔 N3IOAH
- [ ] Acceptance criteria coverage documented per item 🆔 M1TBgo
- [ ] Gaps identified and prioritized 🆔 PiBKBx
- [ ] Full traceability matrix complete 🆔 vJ6ctt
- [ ] **NO pass/fail status fields** in output 🆔 PWRN9n
- [ ] **NO coverage percentage fields** in output 🆔 Zo5iWd
- [ ] **NO execution timestamps** in output 🆔 KrHZag
- [ ] **NO anomaly/defect lists** in output 🆔 ARngsI
- [ ] **NO "release readiness" assessment** in output 🆔 tqgI8W
