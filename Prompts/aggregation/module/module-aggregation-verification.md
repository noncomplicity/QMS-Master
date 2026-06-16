---
id:
title: "Aggregate Module Verification Documentation"
version:
author:
effective_date:
type: "Prompt"
category: "aggregation"
level: "module"
standard: "IEC 62304"
clause: "5.6, 5.7"
inputs: ["item-verification.json (multiple)", "module-architecture.json", "system-manifest.json"]
outputs: ["module-verification.json"]
software_class: "all"
process: "[Software Development Process](../../../Canvases/Software%20Development%20Process.canvas)"
requirements: "[IEC 62304](../../../Requirements/IEC_62304_Requirements.md)"
owner: "[Head of Quality Management](../../../Assets/Head%20of%20Quality%20Management.md)"
---

# Aggregate Module Verification Documentation

## Critical Distinction: Documentation vs Execution

> **This prompt aggregates test case documentation from items and documents integration test cases.**
> **This prompt does NOT report execution results, pass/fail status, or coverage metrics.**

See `_documentation-vs-execution.md` for the full rationale.

**What we aggregate/document:**
- Test case existence from all items in the module
- Integration test cases between items
- Requirement-to-test traceability
- Risk control-to-test traceability
- Verification gaps (missing test cases)

**What we do NOT include:**
- ❌ Pass/fail status (execution result)
- ❌ Coverage percentages (requires execution)
- ❌ Execution timestamps (runtime metadata)
- ❌ Test duration (runtime metric)
- ❌ "Release readiness" based on test results

## Context

IEC 62304 clauses 5.6 and 5.7 require documentation of verification activities. For modules composed of multiple software items, this aggregation prompt combines item-level verification documentation and adds module-level integration test documentation.

**Regulatory purpose:** Document that test cases exist to verify software requirements and risk controls across all items in the module. This is documentation of verification planning and test case design, not execution evidence.

This aggregation prompt:
1. Collects `item-verification.json` from all software items in the module
2. Summarizes test case counts and traceability from each item
3. Documents module-level integration test cases
4. Identifies verification gaps (missing test cases)
5. Produces `module-verification.json`

## Inputs

### Required
- **`items/<item>/extracted/item-verification.json`** — Test case documentation from each software item
- **`module-architecture.json`** — Module architecture showing integration points
- **`system-manifest.json`** — System structure defining module membership

### Optional (enriches output)
- **`module-risk.json`** — Risk information for verification prioritization

### NOT Inputs (Execution Artifacts)
The following are explicitly **out of scope**:
- ~~CI/CD reports~~
- ~~JUnit/pytest execution reports~~
- ~~Coverage reports~~
- ~~Static analysis results~~

### Configuration Required
- **module_id** — Identifier for this module
- **module_name** — Human-readable module name
- **parent_system** — System this module belongs to

## Instructions

### 1. Collect Item Verification Documentation

For each software item belonging to this module (from `system-manifest.json`):
1. Load `item-verification.json`
2. Validate hierarchy metadata matches expected module
3. Collect test case documentation

### 2. Summarize Item-Level Test Documentation

For each item, summarize:
- Number of unit test cases documented
- Number of integration test cases documented
- Requirements with traced test cases
- Requirements without traced test cases
- Risk controls with traced test cases
- Risk controls without traced test cases

### 3. Identify Integration Test Requirements

From module-architecture.json, identify what needs integration testing:
- Inter-item interfaces
- Shared data stores
- Message queues/event flows
- End-to-end data flows

For each integration point:
- Does an integration test case exist?
- What scenarios should be tested?
- What test cases are missing?

### 4. Document Integration Test Cases

For integration test cases that exist:
- Test case identification
- What interface/integration point it tests
- Items involved
- Test specification (inputs, expected outcomes)
- Source file location

### 5. Aggregate Requirement Traceability

Combine requirement-to-test traceability from all items:
- Which requirements have test cases?
- Which requirements lack test cases?
- What is the traceability status?

### 6. Aggregate Risk Control Traceability

Combine risk control-to-test traceability from all items:
- Which controls have test cases?
- Which controls lack test cases?
- What is the traceability status?

### 7. Identify Verification Gaps

Gap types to identify:
- **Untraced requirements** — Requirements without test cases
- **Untraced controls** — Risk controls without test cases
- **Missing integration tests** — Inter-item interfaces without test cases
- **Missing acceptance criteria** — Acceptance criteria types not covered

### 8. Create Traceability

Link verification to:
- **Upward**: System-level test cases
- **Downward**: Item-level test cases
- **Lateral**: Requirements and risk controls being verified

## Output Schema

```json
{
  "extraction_metadata": {
    "aggregated_at": "<ISO-8601 timestamp>",
    "aggregator_version": "2.1",
    "standard": "IEC 62304:2006+AMD1:2015",
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

  "module_summary": {
    "module_id": "<module-id>",
    "total_test_cases": "<count across all items + integration>",
    "unit_test_cases": "<count>",
    "integration_test_cases": "<count>",
    "requirements_with_tests": "<count>",
    "requirements_without_tests": "<count>",
    "risk_controls_with_tests": "<count>",
    "risk_controls_without_tests": "<count>",
    "verification_gaps_count": "<count>"
  },

  "item_verification_summary": [
    {
      "item_id": "<item-id>",
      "item_name": "<human-readable name>",
      "test_cases": {
        "unit_test_cases": "<count>",
        "integration_test_cases": "<count>",
        "total": "<count>"
      },
      "requirement_traceability": {
        "requirements_total": "<count>",
        "requirements_with_tests": "<count>",
        "requirements_without_tests": "<count>"
      },
      "risk_control_traceability": {
        "controls_total": "<count>",
        "controls_with_tests": "<count>",
        "controls_without_tests": "<count>"
      },
      "acceptance_criteria_coverage": {
        "functional": "<test count>",
        "fault_handling": "<test count>",
        "boundary": "<test count>",
        "data_flow": "<test count>",
        "event_sequence": "<test count>",
        "initialization": "<test count>"
      },
      "gaps_from_item": ["ITEM-<item>-VER-GAP-<seq>"]
    }
  ],

  "integration_test_documentation": {
    "integration_test_suites": [
      {
        "id": "MOD-<mod>-ITS-<seq>",
        "name": "<test suite name>",
        "description": "<what it tests>",
        "interfaces_covered": ["MOD-<mod>-INT-<seq>"],
        "test_file": "<path to test file>",
        "test_case_count": "<count>"
      }
    ],

    "integration_test_cases": [
      {
        "id": "MOD-<mod>-IT-<seq>",
        "suite_id": "MOD-<mod>-ITS-<seq>",
        "test_name": "<test name>",
        "test_file": "<path>:<line-start>-<line-end>",
        "interfaces_tested": ["MOD-<mod>-INT-<seq>"],
        "items_involved": ["<item-ids>"],
        "test_specification": {
          "description": "<what this test verifies>",
          "input_setup": "<test inputs>",
          "expected_outcome": "<expected behavior>",
          "pass_fail_criteria": "<assertion criteria>"
        }
      }
    ],

    "interface_test_coverage": [
      {
        "interface_id": "MOD-<mod>-INT-<seq>",
        "interface_name": "<interface name>",
        "traceability_status": "traced | partial | untraced",
        "test_cases": ["MOD-<mod>-IT-<seq>"],
        "scenarios_with_tests": ["<scenario descriptions>"],
        "scenarios_without_tests": ["<untested scenarios>"]
      }
    ]
  },

  "requirement_traceability_aggregated": {
    "total_requirements": "<count across items>",
    "requirements_with_tests": "<count>",
    "requirements_without_tests": "<count>",

    "by_item": [
      {
        "item_id": "<item-id>",
        "requirements_total": "<count>",
        "requirements_with_tests": "<count>",
        "requirements_without_tests": "<count>"
      }
    ],

    "untraced_requirements": [
      {
        "requirement_id": "ITEM-<item>-REQ-<seq>",
        "item_id": "<item-id>",
        "description": "<requirement description>",
        "priority": "high | medium | low"
      }
    ]
  },

  "risk_control_traceability_aggregated": {
    "total_controls": "<count across items>",
    "controls_with_tests": "<count>",
    "controls_without_tests": "<count>",

    "by_hazard": [
      {
        "hazard_id": "MOD-<mod>-HAZ-<seq>",
        "hazard_description": "<hazard>",
        "controls": [
          {
            "control_id": "MOD-<mod>-RC-<seq>",
            "control_description": "<control>",
            "traceability_status": "traced | partial | untraced",
            "test_cases": ["<test IDs>"]
          }
        ]
      }
    ],

    "untraced_controls": [
      {
        "control_id": "MOD-<mod>-RC-<seq>",
        "hazard_id": "MOD-<mod>-HAZ-<seq>",
        "control_description": "<control>",
        "priority": "high | medium | low"
      }
    ]
  },

  "data_flow_test_coverage": [
    {
      "flow_id": "MOD-<mod>-FLOW-<seq>",
      "flow_name": "<data flow name>",
      "traceability_status": "traced | partial | untraced",
      "test_cases": ["MOD-<mod>-IT-<seq>"],
      "validation_points_with_tests": ["<validation point IDs>"],
      "validation_points_without_tests": ["<untested points>"]
    }
  ],

  "verification_gaps": [
    {
      "id": "MOD-<mod>-VGAP-<seq>",
      "gap_type": "untraced_requirement | untraced_control | missing_integration_test | missing_acceptance_criteria",
      "description": "<what test case is missing>",
      "affected_items": ["<item-ids>"],
      "affected_interfaces": ["MOD-<mod>-INT-<seq>"],
      "affected_element": "<ID of requirement, control, or interface>",
      "iec_62304_clause": "<applicable clause>",
      "recommendation": "<what test case should be added>",
      "priority": "high | medium | low"
    }
  ],

  "traceability": {
    "to_system_verification": [],
    "to_item_verification": [
      {
        "item_id": "<item-id>",
        "item_test_case_ids": ["ITEM-<item>-UT-<seq>", "ITEM-<item>-IT-<seq>"]
      }
    ],
    "to_requirements": [
      {
        "test_id": "MOD-<mod>-IT-<seq>",
        "requirement_ids": ["ITEM-<item>-REQ-<seq>"]
      }
    ],
    "to_risk_controls": [
      {
        "test_id": "MOD-<mod>-IT-<seq>",
        "control_ids": ["MOD-<mod>-RC-<seq>"]
      }
    ]
  }
}
```

## Compliance Mapping

| Output Element | IEC 62304 Clause | Documentation Provided |
|----------------|------------------|------------------------|
| `item_verification_summary[]` | 5.5.3 | Unit test case documentation |
| `integration_test_documentation.integration_test_cases[]` | 5.6.3 | Integration test case documentation |
| `integration_test_documentation.interface_test_coverage[]` | 5.6.4 | Interface test coverage documentation |
| `requirement_traceability_aggregated` | 5.7.4 | Requirement-to-test traceability |
| `risk_control_traceability_aggregated` | 7.3.1 | Risk control test traceability |
| `data_flow_test_coverage[]` | 5.6.6 | Data flow verification documentation |
| `verification_gaps[]` | 5.7.1 | Verification gap identification |
| `traceability` | 5.1.1 | Complete traceability |

## Aggregation Rules

### Test Case Count Aggregation
Sum test cases across items:
```
Module total = Σ(item unit tests) + Σ(item integration tests) + module integration tests
```

### Traceability Status Rules
- **traced**: Test case exists and is linked
- **partial**: Some test cases exist but gaps remain
- **untraced**: No test case exists

### Gap Priority Rules
| Gap Type | Default Priority |
|----------|------------------|
| Untraced high-risk control | high |
| Missing integration test for critical interface | high |
| Untraced safety requirement | high |
| Untraced functional requirement | medium |
| Missing acceptance criteria type | medium |
| Untraced low-risk control | low |

## Validation Criteria

- [ ] All items from system-manifest.json are included 🆔 pZlxMM
- [ ] Item test case summaries match source data 🆔 QtQqBG
- [ ] Integration test cases documented for inter-item interfaces 🆔 Aih0Uw
- [ ] Requirement traceability aggregated across items 🆔 1TcgG3
- [ ] Risk control traceability aggregated 🆔 kEfcM8
- [ ] Gaps identified and prioritized 🆔 skfEk6
- [ ] **NO pass/fail status fields** in output 🆔 TfNWSn
- [ ] **NO coverage percentage fields** in output 🆔 il3ZN1
- [ ] **NO execution timestamps** in output 🆔 fkZdkA
- [ ] **NO "verification_status: verified"** claims 🆔 ZqaOoe
- [ ] Traceability links complete 🆔 WidFFP
