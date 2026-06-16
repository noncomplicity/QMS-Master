---
id: 725af57
title: "item extraction 62304 verification"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Prompt"
category: "extraction"
level: "item"
standard: "IEC 62304"
clause: "5.5, 5.6, 5.7"
inputs: ["test source files", "test configuration"]
outputs: ["item-verification.json"]
software_class: "all"
process: "[Software Development Process](../../../Canvases/Software%20Development%20Process.canvas)"
requirements: "[IEC 62304](../../../Requirements/IEC_62304_Requirements.md)"
owner: "[Head of Quality Management](../../../Assets/Head%20of%20Quality%20Management.md)"
---

# Extract Software Item Verification Documentation

## Critical Distinction: Documentation vs Execution

> **This prompt documents what test cases EXIST and what they are designed to verify.**
> **This prompt does NOT report test execution results, pass/fail status, or coverage metrics.**

See `_documentation-vs-execution.md` for the full rationale.

**What we extract:**
- Test cases that exist in the codebase (traceable to test files)
- What each test is designed to verify (requirements, controls)
- Test specifications (inputs, expected outcomes) from test code
- Traceability between tests, requirements, and risk controls

**What we do NOT extract:**
- ❌ Pass/fail status (execution result)
- ❌ Coverage percentages (requires execution)
- ❌ Execution timestamps (runtime metadata)
- ❌ Test duration (runtime metric)

## Context

IEC 62304 clauses 5.5-5.7 require documentation of verification activities:
- **5.5** Software Unit Verification (Class B, C)
- **5.6** Software Integration Testing (Class B, C)
- **5.7** Software System Testing (Class A, B, C)

**Regulatory purpose:** Document that test cases exist to verify software requirements and risk controls. The existence and specification of tests is regulatory documentation. The execution of tests is operational evidence maintained separately by CI/CD systems.

**Item-level focus:** This prompt extracts verification documentation **within a single software item** (repository). It documents:
- Unit test cases and their specifications
- Integration test cases between components of this item
- Test-to-requirement traceability
- Test-to-risk-control traceability

## Inputs

### Required

**Test Source Code:**
- Unit test files (Jest, pytest, JUnit, etc.)
- Integration test files
- Component test files
- Test fixtures and test data

**Test Configuration:**
- Test framework configuration
- Test directory structure

### Optional (enriches output)

**Existing Documentation:**
- Test plans (if available)
- Test case specifications (if available)

### NOT Inputs (Execution Artifacts)

The following are explicitly **out of scope**:
- ~~Test execution reports (JUnit XML, JSON)~~
- ~~CI/CD pipeline logs~~
- ~~Coverage reports~~
- ~~Test run artifacts~~

### Hierarchy Context

- **item_id** — Identifier for this software item
- **item_name** — Human-readable name
- **parent_module** — Module this item belongs to (if known)
- **parent_system** — System this item belongs to (if known)

## Instructions

### 1. Identify Test Structure

Map the test organization in this item:
- Test directory structure and locations
- Test file naming conventions
- Test framework(s) used
- Test categories (unit, integration, e2e)

### 2. Extract Unit Test Documentation (5.5)

For each unit test file:

a) **Identify Test Cases**
   - Extract test method/function names
   - Document file path and line numbers
   - Categorize by acceptance criteria type

b) **Extract Test Specifications** (5.5.3, 5.5.4)
   From the test code itself, document:
   - Input setup (from test fixtures, @Before, arrange sections)
   - Expected outcomes (from assertions)
   - What acceptance criteria type it covers:
     - Functional correctness
     - Event sequence
     - Data and control flow
     - Fault handling (error cases)
     - Boundary conditions
     - Initialization

c) **Identify Component Under Test**
   - Which source file(s) does this test exercise
   - Link test to architecture component

### 3. Extract Integration Test Documentation (5.6)

For integration tests within this item:

a) **Identify Integration Test Cases** (5.6.1, 5.6.2)
   - Which components are tested together
   - Integration scope (database, API, etc.)

b) **Document Integration Test Specifications** (5.6.4)
   - What integration scenario is tested
   - Input conditions
   - Expected integrated behavior

c) **Identify Regression Test Suite** (5.6.6)
   - Which tests form the regression suite
   - Test organization for regression

### 4. Create Requirement Traceability (5.7)

Map tests to requirements:

a) **Analyze Test Intent** (5.7.1, 5.7.4)
   - From test names, identify which requirement is being tested
   - From test assertions, confirm requirement coverage
   - Document the traceability link

b) **Document Test Specification** (5.7.5)
   For each test linked to a requirement:
   - Input stimuli (from test setup)
   - Expected outcomes (from assertions)
   - Pass/fail criteria (from assertion logic)

### 5. Create Risk Control Traceability

Map tests to risk controls:
- Identify tests that exercise safety-critical code paths
- Link tests to risk controls they verify
- Document which hazard each test helps mitigate

### 6. Identify Verification Gaps

Document what is missing:
- Requirements without associated test cases
- Risk controls without verification tests
- Components without test coverage
- Missing acceptance criteria types

### 7. Build Traceability Matrix

Create complete linkage:
- **Test → Component**: Which component each test exercises
- **Test → Requirement**: Which requirement each test verifies
- **Test → Risk Control**: Which control each test validates

## Output Schema

```json
{
  "extraction_metadata": {
    "repository": "<git remote URL>",
    "commit": "<commit hash>",
    "extracted_at": "<ISO-8601 timestamp>",
    "extractor_version": "2.1",
    "standard": "IEC 62304:2006+AMD1:2015",
    "test_framework": "<jest | pytest | junit | etc>",
    "hierarchy": {
      "level": "item",
      "item_id": "<software-item-id>",
      "item_name": "<human-readable name>",
      "parent_module": "<module-id or null>",
      "parent_system": "<system-id or null>"
    }
  },

  "test_structure": {
    "test_directories": ["<paths to test directories>"],
    "test_frameworks": ["<frameworks used>"],
    "test_file_count": 0,
    "test_case_count": 0,
    "naming_convention": "<description of test naming pattern>"
  },

  "unit_tests": [
    {
      "id": "ITEM-<item>-UT-<seq>",
      "test_file": "<path>:<line-start>-<line-end>",
      "test_name": "<test function/method name>",
      "component_under_test": "ITEM-<item>-COMP-<seq>",
      "source_file_tested": "<path to source file>",
      "acceptance_criteria_type": "functional | event_sequence | data_flow | fault_handling | boundary | initialization",
      "test_specification": {
        "description": "<what this test verifies>",
        "input_setup": "<description of test inputs from code>",
        "expected_outcome": "<description of assertions>",
        "pass_fail_criteria": "<specific criteria from assertions>"
      }
    }
  ],

  "integration_tests": [
    {
      "id": "ITEM-<item>-IT-<seq>",
      "test_file": "<path>:<line-start>-<line-end>",
      "test_name": "<test function/method name>",
      "integrated_components": ["ITEM-<item>-COMP-<seq>"],
      "integration_type": "database | api | messaging | full_stack",
      "test_specification": {
        "description": "<what integration scenario this tests>",
        "input_setup": "<integration test setup>",
        "expected_outcome": "<expected integrated behavior>",
        "pass_fail_criteria": "<specific criteria>"
      }
    }
  ],

  "regression_suite": {
    "test_ids": ["ITEM-<item>-UT-<seq>", "ITEM-<item>-IT-<seq>"],
    "organization": "<how regression tests are organized>",
    "trigger_configuration": "<when regression runs - from CI config if available>"
  },

  "requirement_traceability": [
    {
      "requirement_id": "ITEM-<item>-REQ-<seq>",
      "requirement_description": "<brief description>",
      "test_cases": [
        {
          "test_id": "ITEM-<item>-UT-<seq>",
          "test_file": "<path>:<line>",
          "verification_rationale": "<why this test verifies the requirement>"
        }
      ],
      "traceability_status": "traced | partial | untraced"
    }
  ],

  "risk_control_traceability": [
    {
      "risk_control_id": "ITEM-<item>-RC-<seq>",
      "control_description": "<brief description>",
      "test_cases": [
        {
          "test_id": "ITEM-<item>-UT-<seq>",
          "test_file": "<path>:<line>",
          "verification_rationale": "<why this test validates the control>"
        }
      ],
      "traceability_status": "traced | partial | untraced"
    }
  ],

  "acceptance_criteria_coverage": {
    "by_type": {
      "functional": {
        "test_count": 0,
        "test_ids": ["ITEM-<item>-UT-<seq>"]
      },
      "event_sequence": {
        "test_count": 0,
        "test_ids": []
      },
      "data_flow": {
        "test_count": 0,
        "test_ids": []
      },
      "fault_handling": {
        "test_count": 0,
        "test_ids": []
      },
      "boundary": {
        "test_count": 0,
        "test_ids": []
      },
      "initialization": {
        "test_count": 0,
        "test_ids": []
      }
    },
    "missing_types": ["<acceptance criteria types with no tests>"]
  },

  "gaps": [
    {
      "id": "ITEM-<item>-VER-GAP-<seq>",
      "gap_type": "untraced_requirement | untraced_control | untested_component | missing_acceptance_criteria",
      "description": "<what is missing>",
      "affected_element": "<ID of requirement, control, or component>",
      "iec_62304_clause": "<applicable clause>",
      "recommendation": "<suggested action>",
      "priority": "high | medium | low"
    }
  ],

  "traceability_summary": {
    "requirements_total": 0,
    "requirements_with_tests": 0,
    "requirements_without_tests": 0,
    "risk_controls_total": 0,
    "risk_controls_with_tests": 0,
    "risk_controls_without_tests": 0,
    "components_total": 0,
    "components_with_tests": 0,
    "components_without_tests": 0
  }
}
```

## Compliance Mapping

| Output Element | IEC 62304 Clause | Documentation Provided |
|----------------|------------------|------------------------|
| `unit_tests[]` | 5.5.1-5.5.5 | Unit test cases documented |
| `unit_tests[].acceptance_criteria_type` | 5.5.3, 5.5.4 | Acceptance criteria types |
| `unit_tests[].test_specification` | 5.5.5 | Unit test documentation |
| `integration_tests[]` | 5.6.1-5.6.4 | Integration test cases documented |
| `regression_suite` | 5.6.6 | Regression test identification |
| `requirement_traceability[]` | 5.7.1, 5.7.4 | Requirement verification traceability |
| `requirement_traceability[].test_cases[].test_specification` | 5.7.5 | Test record content |
| `risk_control_traceability[]` | 5.7.1 | Risk control verification traceability |
| `gaps[]` | 5.5.5, 5.6.8, 5.7.2 | Verification gaps identified |

## Examples

### Input: JUnit Test File

```java
@SpringBootTest
class AppointmentControllerTest {

    @Test
    void shouldRejectUnauthorizedRequest() {
        // Arrange
        var request = MockMvcRequestBuilders.get("/api/appointments/123")
            .contentType(MediaType.APPLICATION_JSON);
        // No Authorization header

        // Act & Assert
        mockMvc.perform(request)
            .andExpect(status().isUnauthorized())
            .andExpect(jsonPath("$.error").value("Unauthorized"));
    }
}
```

### Output: Test Documentation (partial)

```json
{
  "unit_tests": [
    {
      "id": "ITEM-HM-UT-001",
      "test_file": "src/test/java/se/alerisx/mhp/manager/controller/AppointmentControllerTest.java:15-28",
      "test_name": "shouldRejectUnauthorizedRequest",
      "component_under_test": "ITEM-HM-COMP-001",
      "source_file_tested": "src/main/java/se/alerisx/mhp/manager/controller/AppointmentController.java",
      "acceptance_criteria_type": "fault_handling",
      "test_specification": {
        "description": "Verifies unauthorized requests are rejected",
        "input_setup": "GET request to /api/appointments/123 without Authorization header",
        "expected_outcome": "HTTP 401 Unauthorized with error message",
        "pass_fail_criteria": "Response status is 401 AND response body contains error field with value 'Unauthorized'"
      }
    }
  ],
  "risk_control_traceability": [
    {
      "risk_control_id": "ITEM-HM-RC-001",
      "control_description": "JWT authentication required for all API endpoints",
      "test_cases": [
        {
          "test_id": "ITEM-HM-UT-001",
          "test_file": "AppointmentControllerTest.java:15",
          "verification_rationale": "Test confirms unauthorized requests are rejected, validating authentication control"
        }
      ],
      "traceability_status": "traced"
    }
  ]
}
```

## Validation Criteria

- [ ] All test files in test directories are identified 🆔 wRxd5v
- [ ] Test cases documented with file path and line numbers 🆔 v1fjjF
- [ ] Test specifications extracted from test code (not fabricated) 🆔 Z4tt4n
- [ ] Acceptance criteria types assigned based on test content 🆔 XnX3Mt
- [ ] Requirements mapped to tests where traceable 🆔 Nc3jVY
- [ ] Risk controls mapped to tests where applicable 🆔 pQBEub
- [ ] Gaps identified for untraced requirements and controls 🆔 6ZRG9C
- [ ] **NO pass/fail status fields** in output 🆔 fsuf4G
- [ ] **NO coverage percentage fields** in output 🆔 0IWRRM
- [ ] **NO execution timestamps** in output 🆔 mV0iBR
- [ ] **NO duration metrics** in output 🆔 gzD2DM
- [ ] Hierarchy metadata correctly populated 🆔 wAl9My
- [ ] IDs follow `ITEM-<item>-UT-*` and `ITEM-<item>-IT-*` conventions 🆔 qhdh6B
