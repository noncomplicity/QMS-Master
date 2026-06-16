---
id:
title: "Extract Verification Evidence from Code"
version:
author:
effective_date:
type: "Prompt"
category: "extraction"
standard: "IEC 62304"
clause: "5.5, 5.6, 5.7"
inputs: ["test files", "test results", "CI/CD logs", "coverage reports"]
outputs: ["verification.json", "traceability matrix entries"]
software_class: "all"
process: "[Software Development Process](../../Canvases/Software%20Development%20Process.canvas)"
requirements:
owner: "[Head of Quality Management](../../Assets/Head%20of%20Quality%20Management.md)"
---

# Extract Verification Evidence from Code

## Context

IEC 62304 clauses 5.5-5.7 require verification at multiple levels:
- **5.5** Software Unit Verification (Class B, C)
- **5.6** Software Integration Testing (Class B, C)
- **5.7** Software System Testing (Class A, B, C)

This prompt extracts verification evidence from test suites, CI/CD pipelines, and test artifacts to demonstrate compliance.

## Inputs

### Test Code
- Unit test files (Jest, pytest, JUnit, etc.)
- Integration test files
- End-to-end test files
- Test fixtures and mocks

### Test Results
- Test execution reports (JUnit XML, JSON)
- CI/CD pipeline logs
- Test coverage reports
- Performance test results

### Test Configuration
- Test framework configuration
- CI/CD workflow definitions
- Test environment specifications
- Test data management

### Existing Documentation
- Test plans
- Test case specifications
- Anomaly/defect logs

## Instructions

### 1. Extract Unit Verification Evidence (5.5)

For Class B and C software:

a) **Identify Unit Test Coverage**
   - Map test files to software units
   - Calculate coverage metrics (line, branch, function)
   - Identify untested units

b) **Extract Acceptance Criteria** (5.5.3, 5.5.4)
   For each unit, identify tests covering:
   - Functional correctness
   - Proper event sequence
   - Data and control flow
   - Resource allocation
   - Fault handling (error definition, isolation, recovery)
   - Variable initialization
   - Self-diagnostics
   - Memory management and overflow handling
   - Boundary conditions

c) **Document Verification Results** (5.5.5)
   - Test pass/fail status
   - Coverage metrics
   - Identified anomalies

### 2. Extract Integration Test Evidence (5.6)

For Class B and C software:

a) **Map Integration Tests** (5.6.1, 5.6.2)
   - Identify which software items are tested together
   - Document integration sequence
   - Verify integration plan adherence

b) **Integration Test Content** (5.6.4)
   Tests should cover:
   - Required functionality
   - Risk control measure implementation
   - Timing and performance behavior
   - Internal and external interface functioning
   - Abnormal conditions and foreseeable misuse

c) **Regression Testing** (5.6.6)
   - Identify regression test suite
   - Document regression test execution history

d) **Integration Test Records** (5.6.7)
   - Pass/fail results with anomaly lists
   - Sufficient detail for test repeatability
   - Tester identification

### 3. Extract System Test Evidence (5.7)

For all classes (A, B, C):

a) **Requirements Coverage** (5.7.1)
   - Map tests to software requirements
   - Verify all requirements have tests
   - Document input stimuli, expected outcomes, pass/fail criteria

b) **System Test Records** (5.7.5)
   - Test case procedures with actions and expected results
   - Pass/fail results with anomaly lists
   - Software version tested
   - Hardware/software test configuration
   - Test tools used
   - Test date
   - Tester identity

c) **Traceability** (5.7.4)
   - Requirements → Tests mapping
   - Tests → Results mapping
   - Gaps in coverage

### 4. Extract Problem Resolution Evidence (5.7.2, 5.6.8)

- Anomalies found during testing
- Problem report references
- Resolution status

## Output Schema

```json
{
  "extraction_metadata": {
    "repository": "<repo-url>",
    "commit": "<commit-hash>",
    "extracted_at": "<ISO-8601-timestamp>",
    "test_framework": "<jest | pytest | junit | etc>",
    "ci_system": "<github-actions | gitlab-ci | jenkins | etc>"
  },
  "summary": {
    "total_tests": 0,
    "passed": 0,
    "failed": 0,
    "skipped": 0,
    "coverage": {
      "lines": 0.0,
      "branches": 0.0,
      "functions": 0.0
    },
    "requirements_covered": 0,
    "requirements_total": 0
  },
  "unit_verification": {
    "software_units_tested": [
      {
        "unit_id": "<SI-unit-id>",
        "unit_path": "<file-path>",
        "test_files": ["<test-file-path>"],
        "tests": [
          {
            "test_id": "UT-<sequence>",
            "test_name": "<test function/description>",
            "test_type": "unit",
            "acceptance_criteria_type": "functional | event_sequence | data_flow | fault_handling | boundary | memory | initialization",
            "status": "passed | failed | skipped",
            "duration_ms": 0,
            "assertions": 0
          }
        ],
        "coverage": {
          "lines": 0.0,
          "branches": 0.0,
          "functions": 0.0
        },
        "acceptance_criteria_coverage": {
          "functional": true,
          "event_sequence": false,
          "data_flow": true,
          "fault_handling": true,
          "boundary_conditions": true,
          "memory_management": false,
          "initialization": true
        }
      }
    ],
    "untested_units": ["<SI-unit-id>"]
  },
  "integration_verification": {
    "integration_tests": [
      {
        "test_id": "IT-<sequence>",
        "test_name": "<test description>",
        "test_file": "<file-path>",
        "integrated_items": ["<SI-id>"],
        "test_type": "integration",
        "coverage_type": "functionality | risk_control | timing | interface | abnormal_condition",
        "status": "passed | failed | skipped",
        "duration_ms": 0
      }
    ],
    "regression_tests": [
      {
        "test_id": "RT-<sequence>",
        "test_name": "<test description>",
        "last_executed": "<ISO-timestamp>",
        "status": "passed | failed"
      }
    ],
    "interface_tests": [
      {
        "interface_id": "IF-<id>",
        "tests": ["IT-<id>"],
        "status": "covered | partial | missing"
      }
    ]
  },
  "system_verification": {
    "system_tests": [
      {
        "test_id": "ST-<sequence>",
        "test_name": "<test description>",
        "test_file": "<file-path>",
        "test_type": "system | e2e | acceptance",
        "requirements_verified": ["<SRS-id>"],
        "input_stimuli": "<description of test inputs>",
        "expected_outcome": "<description of expected behavior>",
        "pass_fail_criteria": "<specific criteria>",
        "status": "passed | failed | skipped",
        "duration_ms": 0,
        "test_configuration": {
          "software_version": "<version>",
          "hardware_config": "<description>",
          "test_tools": ["<tool-name>"]
        },
        "executed_by": "<tester-id or CI>",
        "executed_at": "<ISO-timestamp>"
      }
    ],
    "requirements_traceability": [
      {
        "requirement_id": "<SRS-id>",
        "tests": ["<ST-id>"],
        "coverage_status": "covered | partial | missing",
        "verification_status": "passed | failed | not_executed"
      }
    ]
  },
  "anomalies": [
    {
      "anomaly_id": "ANO-<sequence>",
      "source_test": "<test-id>",
      "discovery_phase": "unit | integration | system",
      "description": "<what went wrong>",
      "severity": "critical | major | minor",
      "status": "open | resolved | deferred",
      "problem_report": "<PR-id if exists>",
      "resolution": "<how resolved>"
    }
  ],
  "verification_gaps": [
    {
      "gap_type": "untested_requirement | missing_acceptance_criteria | insufficient_coverage | missing_regression",
      "description": "<what is missing>",
      "affected_items": ["<id>"],
      "recommendation": "<action to close gap>"
    }
  ]
}
```

## Compliance Mapping

| Output Element | IEC 62304 Clause | Evidence Provided |
|----------------|------------------|-------------------|
| `unit_verification.tests` | 5.5.1-5.5.5 | Unit implementation and verification |
| `unit_verification.acceptance_criteria_coverage` | 5.5.3, 5.5.4 | Unit acceptance criteria (Class B/C) |
| `integration_verification.integration_tests` | 5.6.1-5.6.4 | Integration testing |
| `integration_verification.regression_tests` | 5.6.6 | Regression tests |
| `integration_verification` records | 5.6.7 | Integration test record contents |
| `system_verification.system_tests` | 5.7.1 | System test establishment |
| `system_verification.requirements_traceability` | 5.7.4 | System testing evaluation |
| `system_verification.system_tests` details | 5.7.5 | System test record contents |
| `anomalies` | 5.6.8, 5.7.2 | Problem resolution process |

## Examples

### Input: Jest Test File

```typescript
// tests/vitals/validation.test.ts
describe('VitalSignValidation', () => {
  // Functional test
  it('validates heart rate within normal range', () => {
    expect(validateHeartRate(75)).toBe(true);
  });

  // Boundary condition test
  it('rejects heart rate below minimum', () => {
    expect(validateHeartRate(29)).toBe(false);
  });

  // Fault handling test
  it('handles null input gracefully', () => {
    expect(() => validateHeartRate(null)).toThrow(ValidationError);
  });
});
```

### Output: Extracted Verification Entry

```json
{
  "unit_id": "SI-VITALS-VALIDATION",
  "unit_path": "src/vitals/validation.ts",
  "test_files": ["tests/vitals/validation.test.ts"],
  "tests": [
    {
      "test_id": "UT-001",
      "test_name": "validates heart rate within normal range",
      "test_type": "unit",
      "acceptance_criteria_type": "functional",
      "status": "passed",
      "duration_ms": 5
    },
    {
      "test_id": "UT-002",
      "test_name": "rejects heart rate below minimum",
      "test_type": "unit",
      "acceptance_criteria_type": "boundary",
      "status": "passed",
      "duration_ms": 3
    },
    {
      "test_id": "UT-003",
      "test_name": "handles null input gracefully",
      "test_type": "unit",
      "acceptance_criteria_type": "fault_handling",
      "status": "passed",
      "duration_ms": 4
    }
  ],
  "acceptance_criteria_coverage": {
    "functional": true,
    "boundary_conditions": true,
    "fault_handling": true,
    "event_sequence": false,
    "data_flow": false,
    "memory_management": false,
    "initialization": false
  }
}
```

## Validation Criteria

- [ ] All software units have associated test files identified 🆔 8Kahuw
- [ ] Unit tests cover acceptance criteria per 5.5.3/5.5.4 🆔 eFNlCq
- [ ] Integration tests cover all software item interfaces 🆔 NhIQeu
- [ ] System tests trace to software requirements 🆔 KZr1eC
- [ ] Test records include all required fields per 5.7.5 🆔 iCr0Q1
- [ ] Anomalies are documented with severity and resolution 🆔 zSCWq4
- [ ] Coverage gaps are identified with recommendations 🆔 ajNRtp
- [ ] Regression test suite is identified 🆔 Jx05C0
- [ ] All Class C acceptance criteria types are addressed 🆔 YI5xqr
