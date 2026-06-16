---
id:
title: "Extract Software Detailed Design from Code"
version:
author:
effective_date:
type: "Prompt"
category: "extraction"
standard: "IEC 62304"
clause: "5.4"
inputs: ["source code", "type definitions", "function signatures", "code comments", "architecture.json"]
outputs: ["detailed-design.json", "unit specifications"]
software_class: "C"
process: "[Software Development Process](../../Canvases/Software%20Development%20Process.canvas)"
requirements:
owner: "[Head of Quality Management](../../Assets/Head%20of%20Quality%20Management.md)"
---

# Extract Software Detailed Design from Code

## Context

IEC 62304 clause 5.4 requires detailed design documentation for **Class C software only**. This includes:
- **5.4.1** Subdivide software into software units
- **5.4.2** Detailed design for each software unit
- **5.4.3** Detailed design for interfaces
- **5.4.4** Verification that detailed design implements architecture

In code-as-truth, the source code IS the detailed design. This prompt extracts structured documentation from code to demonstrate compliance.

## Applicability

**This prompt applies to Class C software systems only.**

For Class A and B systems, architecture-level documentation (5.3) is sufficient.

## Inputs

### From Architecture Extraction
- `architecture.json` — Software item hierarchy
- Software units identified at architecture level

### Source Code Analysis
- Function/method signatures
- Type definitions and data structures
- Algorithm implementations
- Control flow patterns
- Error handling patterns

### Code Documentation
- Inline comments
- JSDoc/Docstrings/XML comments
- README files per module

### Static Analysis (if available)
- Cyclomatic complexity
- Dependency graphs
- Call graphs

## Instructions

### 1. Identify Software Units (5.4.1)

For each software item from architecture, subdivide to unit level:

a) **Unit Identification**
   - Functions/methods that are not further subdivided
   - Classes as collections of related units
   - Module-level constants and configurations

b) **Unit Boundaries**
   - Clear input/output contracts
   - Single responsibility
   - Testable in isolation

### 2. Extract Detailed Design per Unit (5.4.2)

For each software unit, document:

a) **Purpose and Responsibility**
   - What the unit does
   - Why it exists
   - Preconditions and postconditions

b) **Algorithm Description**
   - Logic flow (pseudocode or description)
   - Decision points and branches
   - Loop constructs and termination conditions
   - Recursion (if any)

c) **Data Handling**
   - Input parameters (types, ranges, validation)
   - Output values (types, possible values)
   - Internal state modifications
   - Data transformations

d) **Error Handling**
   - Expected error conditions
   - Error detection mechanisms
   - Error response behavior
   - Exception propagation

e) **Resource Management**
   - Memory allocation/deallocation
   - File handles
   - Network connections
   - Concurrency primitives (locks, semaphores)

f) **Timing and Sequencing**
   - Execution order dependencies
   - Timing constraints
   - Synchronization requirements

### 3. Extract Interface Detailed Design (5.4.3)

For each interface between units:

a) **Interface Specification**
   - Function signatures
   - Parameter types and constraints
   - Return types and possible values
   - Side effects

b) **Protocol**
   - Call sequence requirements
   - State machine (if stateful)
   - Synchronous vs asynchronous behavior

c) **Error Communication**
   - Error codes/exceptions
   - Error recovery expectations

### 4. Verify Design Implements Architecture (5.4.4)

Document evidence that:
- Units implement the software architecture
- No contradiction with architecture
- All architectural interfaces are realized

## Output Schema

```json
{
  "extraction_metadata": {
    "repository": "<repo-url>",
    "commit": "<commit-hash>",
    "extracted_at": "<ISO-8601-timestamp>",
    "software_safety_class": "C",
    "architecture_reference": "<architecture.json commit>"
  },
  "software_units": [
    {
      "unit_id": "SU-<hierarchy>",
      "parent_item": "<SI-id from architecture>",
      "name": "<unit name>",
      "path": "<file-path>",
      "line_range": [0, 0],
      "unit_type": "function | method | class | module | constant",
      "purpose": "<what this unit does>",
      "responsibility": "<why this unit exists>",
      "safety_class": "C",
      "safety_rationale": "<why Class C>",

      "signature": {
        "declaration": "<full signature>",
        "parameters": [
          {
            "name": "<param-name>",
            "type": "<type>",
            "description": "<purpose>",
            "constraints": {
              "required": true,
              "valid_range": "<range or values>",
              "validation": "<how validated>"
            }
          }
        ],
        "returns": {
          "type": "<return-type>",
          "description": "<what is returned>",
          "possible_values": ["<value descriptions>"]
        },
        "throws": [
          {
            "exception": "<exception-type>",
            "condition": "<when thrown>",
            "handling_expectation": "<how caller should handle>"
          }
        ]
      },

      "algorithm": {
        "description": "<natural language description>",
        "pseudocode": "<structured pseudocode>",
        "complexity": {
          "time": "<O-notation>",
          "space": "<O-notation>"
        },
        "decision_points": [
          {
            "condition": "<condition description>",
            "true_branch": "<what happens if true>",
            "false_branch": "<what happens if false>"
          }
        ],
        "loops": [
          {
            "type": "for | while | do-while | recursion",
            "description": "<what it iterates>",
            "termination": "<how it terminates>",
            "invariant": "<loop invariant if applicable>"
          }
        ]
      },

      "data_handling": {
        "inputs": ["<input descriptions>"],
        "outputs": ["<output descriptions>"],
        "state_modifications": [
          {
            "state": "<what state is modified>",
            "modification": "<how it's modified>"
          }
        ],
        "transformations": ["<data transformation descriptions>"]
      },

      "error_handling": {
        "expected_errors": [
          {
            "error_condition": "<what can go wrong>",
            "detection": "<how detected>",
            "response": "<what the unit does>",
            "propagation": "handled | thrown | logged"
          }
        ],
        "defensive_measures": ["<defensive programming techniques>"]
      },

      "resource_management": {
        "memory": {
          "allocations": ["<what is allocated>"],
          "deallocations": ["<what is freed>"],
          "ownership": "<who owns allocated resources>"
        },
        "external_resources": [
          {
            "resource_type": "file | network | database | hardware",
            "acquisition": "<how acquired>",
            "release": "<how released>",
            "failure_handling": "<what if acquisition fails>"
          }
        ],
        "concurrency": {
          "thread_safety": "safe | unsafe | requires_lock",
          "locks_acquired": ["<lock names>"],
          "deadlock_prevention": "<mechanism if any>"
        }
      },

      "timing": {
        "execution_constraints": "<timing requirements>",
        "blocking_operations": ["<operations that block>"],
        "async_behavior": "<async patterns used>"
      },

      "dependencies": {
        "internal_units": ["<SU-id>"],
        "external_soup": ["<SOUP-id>"],
        "call_sequence": ["<required call order>"]
      },

      "verification": {
        "test_coverage": ["<test-ids>"],
        "review_status": "reviewed | pending",
        "static_analysis": {
          "cyclomatic_complexity": 0,
          "issues": ["<static analysis findings>"]
        }
      }
    }
  ],

  "unit_interfaces": [
    {
      "interface_id": "UI-<sequence>",
      "name": "<interface name>",
      "provider_unit": "<SU-id>",
      "consumer_units": ["<SU-id>"],
      "interface_type": "function_call | callback | event | shared_state",

      "specification": {
        "signature": "<interface signature>",
        "parameters": [
          {
            "name": "<param>",
            "type": "<type>",
            "direction": "in | out | inout",
            "constraints": "<validation rules>"
          }
        ],
        "return_value": {
          "type": "<type>",
          "semantics": "<meaning of return>"
        }
      },

      "protocol": {
        "preconditions": ["<what must be true before call>"],
        "postconditions": ["<what is true after call>"],
        "invariants": ["<what remains true>"],
        "call_sequence": "<required sequence if any>",
        "state_machine": {
          "states": ["<state names>"],
          "transitions": [
            {
              "from": "<state>",
              "to": "<state>",
              "trigger": "<what causes transition>"
            }
          ]
        }
      },

      "error_contract": {
        "error_codes": [
          {
            "code": "<error code/exception>",
            "meaning": "<what it indicates>",
            "recovery": "<expected caller response>"
          }
        ]
      },

      "side_effects": ["<observable side effects>"]
    }
  ],

  "architecture_verification": {
    "architecture_commit": "<commit of architecture.json>",
    "coverage": [
      {
        "architectural_item": "<SI-id>",
        "implementing_units": ["<SU-id>"],
        "coverage_status": "complete | partial | missing",
        "notes": "<verification notes>"
      }
    ],
    "contradictions": [
      {
        "description": "<what contradicts>",
        "architectural_element": "<SI-id or IF-id>",
        "detailed_design_element": "<SU-id or UI-id>",
        "resolution": "<how to resolve>"
      }
    ],
    "verification_evidence": {
      "method": "analysis | review | traceability",
      "reviewed_by": "<reviewer>",
      "reviewed_at": "<ISO-timestamp>",
      "conclusion": "implements_architecture | has_gaps | has_contradictions"
    }
  },

  "metrics": {
    "total_units": 0,
    "total_interfaces": 0,
    "average_complexity": 0,
    "units_by_type": {
      "function": 0,
      "method": 0,
      "class": 0
    },
    "coverage_summary": {
      "units_with_tests": 0,
      "units_without_tests": 0,
      "units_reviewed": 0
    }
  },

  "gaps": [
    {
      "gap_type": "missing_documentation | high_complexity | untested | unclear_algorithm",
      "unit_id": "<SU-id>",
      "description": "<what is missing or problematic>",
      "recommendation": "<action to address>"
    }
  ]
}
```

## Compliance Mapping

| Output Element | IEC 62304 Clause | Evidence Provided |
|----------------|------------------|-------------------|
| `software_units` | 5.4.1 | Subdivision into software units |
| `software_units[].algorithm` | 5.4.2 | Detailed design for correct implementation |
| `software_units[].signature` | 5.4.2 | Design detail for implementation |
| `software_units[].error_handling` | 5.4.2 | Fault handling design |
| `software_units[].resource_management` | 5.4.2 | Resource management design |
| `unit_interfaces` | 5.4.3 | Interface detailed design |
| `unit_interfaces[].specification` | 5.4.3 | Interface contracts |
| `unit_interfaces[].protocol` | 5.4.3 | Interface behavior |
| `architecture_verification` | 5.4.4 | Verification against architecture |
| `architecture_verification.contradictions` | 5.4.4 b) | Freedom from contradictions |

## Examples

### Input: TypeScript Function

```typescript
// src/vitals/heartRateAnalyzer.ts

/**
 * Analyzes a heart rate reading and determines clinical significance.
 *
 * @param reading - Heart rate in beats per minute
 * @returns Analysis result with status and recommendations
 * @throws ValidationError if reading is outside physiological range
 */
export function analyzeHeartRate(reading: number): HeartRateAnalysis {
  // Validate input range (30-300 bpm physiologically possible)
  if (reading < 30 || reading > 300) {
    throw new ValidationError(
      `Heart rate ${reading} outside physiological range [30-300]`
    );
  }

  // Determine status based on clinical thresholds
  let status: HeartRateStatus;
  let requiresAlarm = false;

  if (reading < 40) {
    status = 'CRITICAL_LOW';
    requiresAlarm = true;
  } else if (reading < 60) {
    status = 'BRADYCARDIA';
  } else if (reading <= 100) {
    status = 'NORMAL';
  } else if (reading <= 150) {
    status = 'ELEVATED';
  } else if (reading <= 180) {
    status = 'TACHYCARDIA';
  } else {
    status = 'CRITICAL_HIGH';
    requiresAlarm = true;
  }

  return {
    reading,
    status,
    requiresAlarm,
    timestamp: new Date(),
    thresholds: HEART_RATE_THRESHOLDS
  };
}
```

### Output: Extracted Detailed Design

```json
{
  "unit_id": "SU-VITALS-HR-001",
  "parent_item": "SI-VITALS-ANALYZER",
  "name": "analyzeHeartRate",
  "path": "src/vitals/heartRateAnalyzer.ts",
  "line_range": [8, 42],
  "unit_type": "function",
  "purpose": "Analyzes a heart rate reading and determines clinical significance",
  "responsibility": "Classification of heart rate values into clinical categories with alarm triggering",
  "safety_class": "C",
  "safety_rationale": "Failure to correctly classify critical heart rates could delay clinical intervention",

  "signature": {
    "declaration": "function analyzeHeartRate(reading: number): HeartRateAnalysis",
    "parameters": [
      {
        "name": "reading",
        "type": "number",
        "description": "Heart rate in beats per minute",
        "constraints": {
          "required": true,
          "valid_range": "30-300 bpm (physiological range)",
          "validation": "Range check with ValidationError on failure"
        }
      }
    ],
    "returns": {
      "type": "HeartRateAnalysis",
      "description": "Analysis result with status, alarm flag, and metadata",
      "possible_values": ["Object with status: CRITICAL_LOW|BRADYCARDIA|NORMAL|ELEVATED|TACHYCARDIA|CRITICAL_HIGH"]
    },
    "throws": [
      {
        "exception": "ValidationError",
        "condition": "reading < 30 or reading > 300",
        "handling_expectation": "Caller should handle as invalid input, do not process further"
      }
    ]
  },

  "algorithm": {
    "description": "1. Validate input is within physiological range. 2. Compare reading against clinical thresholds. 3. Determine status category. 4. Set alarm flag for critical values. 5. Return analysis object.",
    "pseudocode": "IF reading < 30 OR reading > 300 THEN THROW ValidationError\nIF reading < 40 THEN status = CRITICAL_LOW, alarm = true\nELSE IF reading < 60 THEN status = BRADYCARDIA\nELSE IF reading <= 100 THEN status = NORMAL\nELSE IF reading <= 150 THEN status = ELEVATED\nELSE IF reading <= 180 THEN status = TACHYCARDIA\nELSE status = CRITICAL_HIGH, alarm = true\nRETURN {reading, status, requiresAlarm, timestamp, thresholds}",
    "complexity": {
      "time": "O(1)",
      "space": "O(1)"
    },
    "decision_points": [
      {
        "condition": "reading < 30 OR reading > 300",
        "true_branch": "Throw ValidationError",
        "false_branch": "Continue to threshold classification"
      },
      {
        "condition": "reading < 40",
        "true_branch": "Set CRITICAL_LOW status with alarm",
        "false_branch": "Check next threshold"
      }
    ],
    "loops": []
  },

  "error_handling": {
    "expected_errors": [
      {
        "error_condition": "Input outside physiological range",
        "detection": "Explicit range check at function entry",
        "response": "Throw ValidationError with descriptive message",
        "propagation": "thrown"
      }
    ],
    "defensive_measures": [
      "Input validation before processing",
      "Explicit threshold values (no magic numbers)",
      "Complete coverage of all possible ranges in if-else chain"
    ]
  },

  "resource_management": {
    "memory": {
      "allocations": ["HeartRateAnalysis return object", "Date object for timestamp"],
      "deallocations": ["Managed by garbage collector"],
      "ownership": "Caller owns returned object"
    },
    "external_resources": [],
    "concurrency": {
      "thread_safety": "safe",
      "locks_acquired": [],
      "deadlock_prevention": "No locks used, pure function"
    }
  }
}
```

## Validation Criteria

- [ ] All software units from architecture are decomposed to unit level 🆔 wgC6Xr
- [ ] Each unit has complete signature documentation 🆔 joSkNM
- [ ] Algorithm description matches code logic 🆔 XAwggk
- [ ] All decision points are documented 🆔 CHBSi0
- [ ] Error conditions and handling are specified 🆔 fNcesD
- [ ] Resource management is documented for units that allocate 🆔 sb5cPe
- [ ] Interfaces between units are fully specified 🆔 nasJrF
- [ ] Detailed design traces to architecture without contradiction 🆔 NfqyO7
- [ ] High-complexity units are flagged for review 🆔 lnVfXU
- [ ] All Class C acceptance criteria (5.5.4) are addressable from design 🆔 lM26EU
