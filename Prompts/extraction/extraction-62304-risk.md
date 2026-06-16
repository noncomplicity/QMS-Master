---
id: 725af57
title: "extraction 62304 risk"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Prompt"
category: "extraction"
standard: "IEC 62304"
clause: "7"
inputs: ["source code", "error handling", "safety functions", "commit history", "architecture.json"]
outputs: ["software-risk.json", "risk traceability entries"]
software_class: "B, C"
process: "[Risk Management Process](../../Canvases/Risk%20Management%20Process.canvas)"
requirements:
owner: "[Head of Risk Management](../../Assets/Head%20of%20Risk%20Management.md)"
---

# Extract Software Risk Management Evidence

## Context

IEC 62304 clause 7 requires software-specific risk management activities integrated with ISO 14971. This prompt extracts evidence of:
- Software items contributing to hazardous situations (7.1)
- Risk control measures implemented in software (7.2)
- Verification of risk control measures (7.3)
- Risk management of software changes (7.4)

## Inputs

### From Code Analysis
- Error handling patterns and exception flows
- Validation and boundary checking code
- Safety-critical function identifications
- Defensive programming implementations
- Watchdog and monitoring code
- Alarm and warning implementations

### From Architecture
- `architecture.json` — Software item hierarchy and safety classifications
- Safety segregation mechanisms
- Failure propagation paths

### From Git History
- Commits tagged as safety-related
- PR descriptions mentioning risk or safety
- Change history of safety-critical components

### From SOUP Analysis
- `soup-list.json` — Third-party component anomalies
- SOUP failure modes

## Instructions

### 1. Identify Software Contributing to Hazardous Situations (7.1.1)

Analyze code to identify software items that could contribute to hazards:

a) **Clinical Data Processing**
   - Patient identification functions
   - Vital sign calculations
   - Diagnostic algorithms
   - Treatment recommendations

b) **Control Functions**
   - Device control interfaces
   - Dosage calculations
   - Timing-critical operations
   - Safety interlock implementations

c) **Data Integrity**
   - Database operations on clinical data
   - Data transmission functions
   - Persistence and recovery

d) **User Interface Safety**
   - Alarm presentation
   - Critical value display
   - User confirmation flows

### 2. Identify Potential Causes of Contribution to Hazards (7.1.2)

For each identified software item, document potential causes:

a) **Specification Defects**
   - Missing requirements (gap analysis)
   - Ambiguous requirements
   - Conflicting requirements

b) **Software Defects**
   - Logic errors
   - Boundary condition failures
   - Race conditions
   - Memory issues

c) **SOUP Failures** (7.1.3)
   - Known SOUP anomalies affecting safety
   - SOUP unexpected behavior scenarios
   - SOUP version incompatibilities

d) **Hardware/Software Interaction**
   - Timing dependencies
   - Resource exhaustion
   - Communication failures

e) **Foreseeable Misuse**
   - Invalid input handling
   - Out-of-sequence operations
   - Unauthorized access attempts

### 3. Extract Risk Control Measures (7.2)

Identify implemented risk controls:

a) **Input Validation**
   - Range checks
   - Type validation
   - Sanity checks

b) **Error Handling**
   - Exception handling patterns
   - Graceful degradation
   - Fail-safe defaults

c) **Redundancy**
   - Duplicate calculations
   - Cross-checks
   - Voting mechanisms

d) **Alarms and Warnings**
   - Condition detection
   - User notification
   - Acknowledgment handling

e) **Access Control**
   - Authentication checks
   - Authorization enforcement
   - Audit logging

f) **Data Protection**
   - Integrity checks (CRC, checksums)
   - Encryption
   - Backup mechanisms

### 4. Extract Risk Control Verification Evidence (7.3)

For each risk control:
- Link to verification tests
- Document verification results
- Check for new hazards introduced by controls

### 5. Analyze Changes for Safety Impact (7.4)

For recent changes:
- Identify if change affects safety-critical code
- Check impact on existing risk controls
- Document additional risk controls needed

## Output Schema

```json
{
  "extraction_metadata": {
    "repository": "<repo-url>",
    "commit": "<commit-hash>",
    "extracted_at": "<ISO-8601-timestamp>",
    "software_safety_class": "A | B | C"
  },
  "hazardous_situations": [
    {
      "hazard_id": "HAZ-SW-<sequence>",
      "description": "<hazardous situation description>",
      "potential_harm": "<description of potential harm>",
      "severity": "negligible | minor | serious | critical | catastrophic",
      "contributing_software_items": ["<SI-id>"],
      "code_locations": ["<file:line>"]
    }
  ],
  "software_causes": [
    {
      "cause_id": "CAUSE-<sequence>",
      "hazard_id": "HAZ-SW-<sequence>",
      "software_item": "<SI-id>",
      "cause_type": "specification | defect | soup_failure | hw_interaction | misuse",
      "description": "<how software could cause/contribute to hazard>",
      "code_evidence": {
        "files": ["<file:line>"],
        "patterns": ["<code pattern identified>"]
      },
      "soup_anomalies": [
        {
          "soup_id": "<SOUP-id>",
          "anomaly": "<known anomaly>",
          "relevance": "<how it could contribute>"
        }
      ]
    }
  ],
  "risk_control_measures": [
    {
      "control_id": "RC-SW-<sequence>",
      "cause_id": "CAUSE-<sequence>",
      "hazard_id": "HAZ-SW-<sequence>",
      "control_type": "input_validation | error_handling | redundancy | alarm | access_control | data_protection | segregation",
      "description": "<what the control does>",
      "implementation": {
        "software_item": "<SI-id>",
        "code_location": "<file:line>",
        "implementation_details": "<how it's implemented>"
      },
      "effectiveness": "prevents | reduces_probability | reduces_severity | detects",
      "residual_risk": "<description of any residual risk>",
      "introduces_new_hazard": false,
      "new_hazard_description": "<if true, describe new hazard>"
    }
  ],
  "risk_control_verification": [
    {
      "control_id": "RC-SW-<sequence>",
      "verification_method": "test | analysis | inspection",
      "verification_evidence": {
        "test_ids": ["<test-id>"],
        "test_files": ["<test-file>"],
        "analysis_reference": "<document reference>"
      },
      "verification_status": "verified | partial | not_verified",
      "verification_result": "pass | fail | pending"
    }
  ],
  "software_safety_requirements": [
    {
      "requirement_id": "SRS-SAF-<sequence>",
      "derived_from_hazard": "HAZ-SW-<sequence>",
      "implements_control": "RC-SW-<sequence>",
      "description": "<requirement text>",
      "safety_class": "B | C"
    }
  ],
  "traceability": {
    "hazard_to_cause": [
      {
        "hazard_id": "HAZ-SW-<id>",
        "cause_ids": ["CAUSE-<id>"]
      }
    ],
    "cause_to_control": [
      {
        "cause_id": "CAUSE-<id>",
        "control_ids": ["RC-SW-<id>"]
      }
    ],
    "control_to_requirement": [
      {
        "control_id": "RC-SW-<id>",
        "requirement_id": "SRS-SAF-<id>"
      }
    ],
    "control_to_verification": [
      {
        "control_id": "RC-SW-<id>",
        "verification_test_ids": ["<test-id>"]
      }
    ]
  },
  "change_impact_analysis": [
    {
      "change_reference": "<commit-hash or PR>",
      "change_description": "<what changed>",
      "affects_safety_items": ["<SI-id>"],
      "affects_risk_controls": ["RC-SW-<id>"],
      "new_causes_introduced": false,
      "additional_controls_required": false,
      "analysis_notes": "<assessment>"
    }
  ],
  "gaps": [
    {
      "gap_type": "uncontrolled_cause | unverified_control | missing_traceability",
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
| `hazardous_situations` | 7.1.1 | Software items contributing to hazardous situations |
| `software_causes` | 7.1.2 | Potential causes of contribution to hazard |
| `software_causes[].soup_anomalies` | 7.1.3 | Evaluation of SOUP anomaly lists |
| `software_causes` documentation | 7.1.4 | Documentation of potential causes |
| `risk_control_measures` | 7.2.1 | Risk control measure definition |
| `software_safety_requirements` | 7.2.2 a) | Risk control in software requirements |
| `risk_control_measures[].implementation` | 7.2.2 b,c) | Safety class assignment and development |
| `risk_control_verification` | 7.3.1 | Verification of risk control measures |
| `traceability` | 7.3.3 | Traceability documentation |
| `change_impact_analysis` | 7.4.1 | Analysis of changes for safety |
| `change_impact_analysis` | 7.4.2 | Impact on existing risk controls |

## Examples

### Input: Error Handling Code

```typescript
// src/vitals/heartRateAnalyzer.ts
export function analyzeHeartRate(reading: number): HeartRateStatus {
  // Input validation - RC-SW-001
  if (reading < 0 || reading > 300) {
    logger.warn('Invalid heart rate reading', { reading });
    throw new ValidationError('Heart rate out of physiological range');
  }

  // Critical threshold check - RC-SW-002
  if (reading < 40 || reading > 180) {
    alarmService.triggerCriticalAlarm({
      type: 'HEART_RATE_CRITICAL',
      value: reading,
      threshold: reading < 40 ? 40 : 180
    });
    return { status: 'CRITICAL', requiresImmediate: true };
  }

  // Normal processing...
}
```

### Output: Extracted Risk Evidence

```json
{
  "hazardous_situations": [
    {
      "hazard_id": "HAZ-SW-001",
      "description": "Failure to detect critical heart rate values",
      "potential_harm": "Delayed clinical intervention for patient with critical heart rate",
      "severity": "serious",
      "contributing_software_items": ["SI-VITALS-ANALYZER"],
      "code_locations": ["src/vitals/heartRateAnalyzer.ts:1-20"]
    }
  ],
  "software_causes": [
    {
      "cause_id": "CAUSE-001",
      "hazard_id": "HAZ-SW-001",
      "software_item": "SI-VITALS-ANALYZER",
      "cause_type": "defect",
      "description": "Logic error in threshold comparison could miss critical values",
      "code_evidence": {
        "files": ["src/vitals/heartRateAnalyzer.ts:10-15"],
        "patterns": ["Comparison operators in threshold check"]
      }
    }
  ],
  "risk_control_measures": [
    {
      "control_id": "RC-SW-001",
      "cause_id": "CAUSE-001",
      "hazard_id": "HAZ-SW-001",
      "control_type": "input_validation",
      "description": "Validate heart rate reading is within physiological range (0-300 bpm)",
      "implementation": {
        "software_item": "SI-VITALS-ANALYZER",
        "code_location": "src/vitals/heartRateAnalyzer.ts:3-6",
        "implementation_details": "Range check with ValidationError throw"
      },
      "effectiveness": "prevents",
      "residual_risk": "None - invalid values are rejected",
      "introduces_new_hazard": false
    },
    {
      "control_id": "RC-SW-002",
      "cause_id": "CAUSE-001",
      "hazard_id": "HAZ-SW-001",
      "control_type": "alarm",
      "description": "Trigger critical alarm when heart rate outside 40-180 bpm range",
      "implementation": {
        "software_item": "SI-VITALS-ANALYZER",
        "code_location": "src/vitals/heartRateAnalyzer.ts:8-15",
        "implementation_details": "alarmService.triggerCriticalAlarm() with threshold details"
      },
      "effectiveness": "detects",
      "residual_risk": "Alarm acknowledgment depends on user attention",
      "introduces_new_hazard": false
    }
  ]
}
```

## Validation Criteria

- [ ] All safety-classified software items (B/C) have hazard analysis 🆔 oAfWMw
- [ ] Each hazardous situation has identified software causes 🆔 TOeynG
- [ ] SOUP anomalies are evaluated for safety relevance 🆔 g76vEp
- [ ] Risk control measures are identified for each cause 🆔 Qirs3M
- [ ] Risk controls are linked to software requirements 🆔 6tKhGw
- [ ] Verification evidence exists for each risk control 🆔 2Zrbb0
- [ ] Traceability chain is complete: Hazard → Cause → Control → Requirement → Test 🆔 KvIep6
- [ ] Recent changes affecting safety items are analyzed 🆔 xNMAJB
- [ ] Gaps in risk control or verification are documented 🆔 ne9LAL
