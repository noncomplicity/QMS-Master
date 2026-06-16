---
id: 725af57
title: "_documentation vs execution"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Guidance"
process: "[Document and Record Control](../Canvases/Document%20and%20Record%20Control.canvas)"
requirements:
owner: "[Head of Quality Management](../Assets/Head%20of%20Quality%20Management.md)"
---

# Documentation vs Execution: Critical Distinction

## Purpose

This document clarifies a fundamental principle of the code-as-truth framework:

> **We document what EXISTS in code to satisfy regulatory requirements.**
> **We do NOT report execution results or runtime metrics.**

This distinction is critical for:
- Regulatory compliance (documentation requirements vs. evidence of execution)
- Framework scope (what AI extraction can provide vs. what CI/CD provides)
- Avoiding hallucination (AI should not fabricate metrics it cannot verify)

## The Core Principle

### What Extraction DOES

| Regulatory Need | What We Extract | Traceable To |
|-----------------|-----------------|--------------|
| "Document software requirements" (62304 5.2) | Requirements implemented in code | Source files, interfaces, DTOs |
| "Document software architecture" (62304 5.3) | Components, interfaces, data flows | Package structure, class relationships |
| "Document SOUP" (62304 8.1) | Third-party dependencies | pom.xml, package.json, requirements.txt |
| "Document verification activities" (62304 5.5-5.7) | Test cases that exist | Test files, test methods |
| "Document risk controls" (14971 7) | Controls implemented in code | Validation logic, error handling |

### What Extraction Does NOT Do

| NOT Our Scope | Belongs To | Why |
|---------------|------------|-----|
| Test pass/fail results | CI/CD pipeline | Execution happens at runtime |
| Coverage percentages | Coverage tools (JaCoCo, Istanbul) | Requires code execution |
| Execution timestamps | CI/CD logs | Runtime metadata |
| Performance metrics | Load testing tools | Requires running system |
| Vulnerability scan results | Security tools (Snyk, Trivy) | Requires external analysis |

## Verification Documentation: Correct Approach

### WRONG: Documenting Execution Results

```json
{
  "test_id": "ITEM-HM-TC-001",
  "name": "testUnauthorizedAccessRejected",
  "status": "passed",           // ❌ Execution result
  "duration_ms": 45,            // ❌ Execution metric
  "last_executed": "2026-03-05", // ❌ Execution timestamp
  "coverage": 72.5              // ❌ Requires execution
}
```

### CORRECT: Documenting Test Case Existence

```json
{
  "test_id": "ITEM-HM-TC-001",
  "name": "testUnauthorizedAccessRejected",
  "test_file": "AppointmentControllerTest.java:45-67",
  "test_type": "unit",
  "verifies_requirement": "ITEM-HM-REQ-SEC-001",
  "verifies_control": "ITEM-HM-RC-001",
  "test_description": "Verifies that requests without valid JWT are rejected with 401",
  "input_specification": "HTTP request without Authorization header",
  "expected_outcome": "HTTP 401 Unauthorized response",
  "acceptance_criteria_type": "security"
}
```

## Regulatory Mapping

### IEC 62304 Clause 5.7.5 - Test Record Contents

The standard requires test records to include:
- Test identifier ✓ (extracted)
- Input stimuli ✓ (extracted from test code)
- Expected outcomes ✓ (extracted from assertions)
- Pass/fail criteria ✓ (extracted from test logic)
- **Test results** → This comes from CI/CD execution, not extraction

### ISO 14971 - Risk Control Verification

The standard requires:
- Documentation of risk controls ✓ (extracted from code)
- Verification that controls are implemented ✓ (test cases exist)
- **Evidence that controls work** → This requires execution, documented separately

## What AI Extraction CAN Verify

### Test Case Existence
- "A test file exists at `src/test/java/...`"
- "Test method `testX` is defined at line 45"
- "Test imports the class under test `AppointmentController`"

### Test-to-Requirement Traceability
- "Test `testUnauthorizedAccessRejected` tests security behavior"
- "Based on test name and assertions, this verifies authentication"
- "Test covers requirement ITEM-HM-REQ-SEC-001"

### Test-to-Risk Control Traceability
- "Test exercises the JWT validation code path"
- "This test verifies risk control RC-001 (authentication)"

### Acceptance Criteria Coverage
- "Test includes boundary value: empty string input"
- "Test includes error condition: invalid token"
- "Test includes normal flow: valid request"

## What AI Extraction CANNOT Verify

### Execution Results
- ❌ "Test passed" (requires running the test)
- ❌ "72.5% line coverage" (requires coverage tool execution)
- ❌ "Test took 45ms" (requires runtime measurement)

### Quality Metrics
- ❌ "No critical findings" (requires static analysis execution)
- ❌ "0 vulnerabilities" (requires security scan execution)

### Verification Status
- ❌ "Control is verified" (requires test execution evidence)
- ❌ "Requirement is satisfied" (requires traceability + execution)

## Correct Field Terminology

### Use These (Documentation)

| Field | Meaning |
|-------|---------|
| `test_exists` | Boolean: test file/method found in codebase |
| `test_file` | Path and line number of test definition |
| `verifies_requirement` | Which requirement this test is designed to verify |
| `verifies_control` | Which risk control this test exercises |
| `acceptance_criteria_type` | Category of test (functional, boundary, error, etc.) |
| `input_specification` | What inputs the test uses (from test code) |
| `expected_outcome` | What the test asserts (from assertions) |

### Avoid These (Execution)

| Field | Problem |
|-------|---------|
| `status: passed/failed` | Execution result |
| `coverage: 72.5` | Requires execution |
| `duration_ms` | Runtime metric |
| `last_executed` | Execution timestamp |
| `verification_status: verified` | Implies execution happened |

## Summary Table

| Question | Answer | Source |
|----------|--------|--------|
| Does a test exist for requirement X? | Yes/No | Code extraction |
| What does the test check? | Input/Output spec | Test code analysis |
| Did the test pass? | **Out of scope** | CI/CD pipeline |
| What is the coverage? | **Out of scope** | Coverage tools |
| Is the requirement verified? | **Partial** - test exists, execution elsewhere | Code + CI/CD |

## Implications for Prompts

### Extraction Prompts Should

1. Document test case existence with source file references
2. Extract test specifications from test code (inputs, expected outputs)
3. Map tests to requirements and risk controls
4. Identify gaps (requirements without tests, controls without tests)
5. Document acceptance criteria coverage by type

### Extraction Prompts Should NOT

1. Include pass/fail status fields
2. Include coverage percentage fields
3. Include execution timestamps
4. Include duration metrics
5. Claim "verification complete" or "control verified"

### Generation Prompts Should

1. Generate traceability matrices (test → requirement → control)
2. List test cases with their specifications
3. Identify verification gaps
4. Reference where execution evidence should be obtained

### Generation Prompts Should NOT

1. Generate test result summaries
2. Claim overall verification status
3. Report coverage metrics
4. Make statements about test success/failure

## How Execution Evidence Fits In

The complete verification picture requires:

```
┌─────────────────────────────────────────────────────────────────┐
│ REGULATORY DOCUMENTATION (This Framework)                        │
├─────────────────────────────────────────────────────────────────┤
│ • Test cases exist (item-verification.json)                      │
│ • Tests map to requirements (traceability)                       │
│ • Tests map to risk controls (traceability)                      │
│ • Acceptance criteria types covered (analysis)                   │
└─────────────────────────────────────────────────────────────────┘
                              +
┌─────────────────────────────────────────────────────────────────┐
│ EXECUTION EVIDENCE (CI/CD Pipeline - Separate)                   │
├─────────────────────────────────────────────────────────────────┤
│ • JUnit XML reports (test pass/fail)                             │
│ • JaCoCo/Istanbul reports (coverage)                             │
│ • SonarQube reports (static analysis)                            │
│ • Security scan reports (vulnerabilities)                        │
└─────────────────────────────────────────────────────────────────┘
                              =
┌─────────────────────────────────────────────────────────────────┐
│ COMPLETE VERIFICATION EVIDENCE                                   │
├─────────────────────────────────────────────────────────────────┤
│ • Test ITEM-HM-TC-001 exists ✓ (documentation)                   │
│ • Test verifies REQ-SEC-001 ✓ (documentation)                    │
│ • Test passed on commit abc123 ✓ (execution)                     │
│ • Coverage: 85% ✓ (execution)                                    │
└─────────────────────────────────────────────────────────────────┘
```

## Prompt Update Checklist

When updating prompts, verify:

- [ ] No `status: passed/failed` fields in schema 🆔 Dyx9F7
- [ ] No `coverage: X%` fields in schema 🆔 INZwXm
- [ ] No `duration_ms` fields in schema 🆔 NdpgpV
- [ ] No `last_executed` timestamp fields 🆔 Y0lQqp
- [ ] No `verification_status: verified` claims 🆔 rXapqC
- [ ] Inputs do NOT include "test results" or "CI/CD logs" 🆔 5tcn0m
- [ ] Outputs document TEST CASES not TEST RESULTS 🆔 g2S6eS
- [ ] Language uses "test exists" not "test passed" 🆔 7RBi8D
- [ ] Traceability is to code locations, not execution logs 🆔 UbRyg4
