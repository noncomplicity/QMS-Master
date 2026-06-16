---
id: 725af57
title: "extraction 14971 risks"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Prompt"
category: "extraction"
standard: "ISO 14971"
clause: "5, 6, 7"
inputs: ["source code", "type definitions", "error handlers", "validation logic", "safety controls", "commit history", "PR descriptions"]
outputs: ["risks.json", "hazard traceability entries"]
software_class: "all"
process: "[[Risk Management Process.canvas]]"
requirements: "[[ISO 14971 Risk management for medical devices]]"
owner: "[[Head of Risk Management]]"
---

# Extract Risk Management Data from Code

## Context

ISO 14971 requires manufacturers to identify hazards, estimate risks, and implement risk controls throughout the product lifecycle. In a code-as-truth paradigm, risk-related information is extracted from implemented safety controls, validation logic, error handling, and defensive programming patterns.

This prompt instructs AI workers to analyze source code and derive risk management artifacts that reflect actual hazard mitigations, ensuring the Risk Management File documents what is genuinely implemented.

## Inputs

Analyze the following artifacts:

### Source Code — Safety Controls
- **Validation logic**: Input validation, range checks, format verification
- **Error handlers**: Try/catch blocks, error boundaries, fallback behaviors
- **Defensive programming**: Null checks, type guards, assertions
- **Safety interlocks**: Conditional blocks preventing unsafe operations
- **Timeout/retry logic**: Mechanisms preventing hung states

### Source Code — Clinical Logic
- **Algorithms**: Calculations affecting clinical decisions
- **Thresholds**: Constants defining normal/abnormal/critical ranges
- **Alarms/warnings**: Code triggering user alerts
- **Data transformations**: Conversions that could introduce errors

### Source Code — Security Controls
- **Authentication**: Login, session management, token handling
- **Authorization**: Permission checks, role-based access
- **Audit logging**: Security event recording
- **Encryption**: Data protection mechanisms

### Git History
- **Commit messages**: Look for "fix", "safety", "security", "prevent", "validate"
- **PR descriptions**: Extract risk rationale, safety justifications
- **Issue references**: Link to bug reports describing hazardous situations

### Existing Documentation
- **Error messages**: User-facing warnings reveal anticipated hazards
- **API documentation**: Documented constraints and limitations
- **Test descriptions**: Safety-related test names reveal risk scenarios

## Instructions

### 1. Identify Intended Use Context (Clause 5.2)

Extract from code and documentation:
- What clinical function does the software perform?
- Who are the intended users? (from user role definitions, auth logic)
- What is the use environment? (from platform requirements, deployment configs)
- What are the operating principles? (from core algorithms)

### 2. Identify Characteristics Related to Safety (Clause 5.3)

Scan for:
- **Data accuracy requirements**: Precision, rounding, unit conversions
- **Timing constraints**: Timeouts, refresh rates, real-time requirements
- **Availability requirements**: Uptime, failover, degraded modes
- **User interface factors**: Display clarity, input validation, confirmation dialogs
- **Data integrity**: Checksums, validation, consistency checks

### 3. Identify Hazards (Clause 5.4)

For each safety-related code pattern, identify the underlying hazard:

| Code Pattern | Potential Hazard Category |
|--------------|---------------------------|
| Input validation | Invalid data entering system |
| Range checks | Values outside safe operating limits |
| Null/undefined checks | Missing critical data |
| Type guards | Data type mismatches |
| Timeout handling | System unresponsive/hung |
| Error boundaries | Cascading failures |
| Auth checks | Unauthorized access |
| Encryption | Data exposure |
| Alarm thresholds | Missed critical conditions |
| Calculation validation | Incorrect clinical values |

### 4. Identify Hazardous Situations (Clause 5.4)

For each hazard, determine:
- What sequence of events leads to harm exposure?
- What user actions could trigger the hazardous situation?
- What environmental conditions contribute?
- What failure modes are possible?

### 5. Estimate Risk (Clause 5.5)

For each hazardous situation, assess based on code evidence:

**Severity indicators in code:**
- Critical alarms → Potentially life-threatening
- Warnings → Moderate harm potential
- Info messages → Low severity
- Clinical data handling → Higher severity
- Administrative functions → Lower severity

**Probability indicators in code:**
- Multiple validation layers → Lower probability
- Single point of failure → Higher probability
- Defensive programming → Lower probability
- Complex logic paths → Higher probability

### 6. Identify Risk Control Measures (Clause 7)

Extract implemented controls:

**Inherent safety by design (7.1 a):**
- Safe defaults in configuration
- Limited input ranges
- Fail-safe states

**Protective measures (7.1 b):**
- Validation routines
- Error handling
- Redundancy/fallbacks
- Interlocks

**Information for safety (7.1 c):**
- Warning messages
- Error messages
- User guidance in UI
- Documentation references

### 7. Identify Residual Risks

Flag areas where:
- Validation is incomplete
- Error handling logs but doesn't prevent
- Warnings are shown but operation proceeds
- User can override safety controls

### 8. Link to Verification Evidence

For each risk control:
- Identify test files that verify the control
- Link to test cases that simulate hazardous conditions
- Note any untested safety paths

## Output Schema

Generate a JSON file with this structure:

```json
{
  "extraction_metadata": {
    "repository": "<repo-url>",
    "commit": "<commit-hash>",
    "extracted_at": "<ISO-8601-timestamp>",
    "extractor_version": "1.0",
    "standard": "ISO 14971:2019"
  },
  "intended_use": {
    "medical_function": "<description>",
    "intended_users": ["<user-type>"],
    "use_environment": "<description>",
    "operating_principle": "<description>",
    "source": {
      "files": ["<file-path>"],
      "documentation": ["<doc-reference>"]
    }
  },
  "safety_characteristics": [
    {
      "char_id": "SC-<sequence>",
      "characteristic": "<description>",
      "limits": "<defined limits if applicable>",
      "source": {
        "files": ["<file-path>:<line-range>"]
      }
    }
  ],
  "hazards": [
    {
      "hazard_id": "HAZ-<sequence>",
      "hazard": "<potential source of harm>",
      "hazard_category": "clinical_data | calculation | timing | availability | security | user_interface | integration",
      "source": {
        "files": ["<file-path>:<line-range>"],
        "commits": ["<commit-hash>"],
        "prs": ["<pr-number>"]
      }
    }
  ],
  "hazardous_situations": [
    {
      "situation_id": "HS-<sequence>",
      "hazard_id": "<HAZ-reference>",
      "situation": "<circumstance where people/property exposed to hazard>",
      "foreseeable_sequence": "<sequence of events leading to situation>",
      "potential_harm": "<injury or damage that could result>",
      "severity": "negligible | minor | serious | critical | catastrophic",
      "severity_rationale": "<evidence from code>",
      "probability": "incredible | improbable | remote | occasional | probable | frequent",
      "probability_rationale": "<evidence from code>",
      "initial_risk_level": "acceptable | ALARP | unacceptable",
      "source": {
        "files": ["<file-path>:<line-range>"]
      }
    }
  ],
  "risk_controls": [
    {
      "control_id": "RC-<sequence>",
      "situation_id": "<HS-reference>",
      "control_type": "inherent_safety | protective_measure | information_for_safety",
      "control_description": "<what the control does>",
      "implementation": {
        "files": ["<file-path>:<line-range>"],
        "mechanism": "<validation | error_handling | interlock | warning | etc.>"
      },
      "verification": {
        "test_files": ["<test-file-path>"],
        "test_names": ["<test function/description>"],
        "verified": true | false
      },
      "effectiveness": "eliminates | reduces | informs",
      "residual_risk_level": "acceptable | ALARP | unacceptable"
    }
  ],
  "residual_risks": [
    {
      "residual_id": "RR-<sequence>",
      "situation_id": "<HS-reference>",
      "control_ids": ["<RC-reference>"],
      "residual_description": "<remaining risk after controls>",
      "residual_severity": "<severity>",
      "residual_probability": "<probability>",
      "acceptability": "acceptable | acceptable_with_disclosure | unacceptable",
      "disclosure_required": true | false,
      "disclosure_text": "<text for user documentation if required>"
    }
  ],
  "gaps": [
    {
      "gap_id": "GAP-<sequence>",
      "type": "uncontrolled_hazard | untested_control | undocumented_residual | missing_verification",
      "description": "<what is missing>",
      "affected_items": ["<hazard_id | situation_id | control_id>"],
      "recommended_action": "<suggested remediation>"
    }
  ],
  "traceability": {
    "hazard_to_situation": {"<hazard_id>": ["<situation_id>"]},
    "situation_to_control": {"<situation_id>": ["<control_id>"]},
    "control_to_verification": {"<control_id>": ["<test_reference>"]}
  }
}
```

## Compliance Mapping

| Output Element | ISO 14971 Clause | Evidence Provided |
|----------------|------------------|-------------------|
| `intended_use` | 5.2 | Intended use and reasonably foreseeable misuse |
| `safety_characteristics` | 5.3 | Characteristics related to safety |
| `hazards` | 5.4 | Identification of hazards |
| `hazardous_situations` | 5.4 | Identification of hazardous situations |
| `hazardous_situations[].severity` | 5.5 | Risk estimation - severity |
| `hazardous_situations[].probability` | 5.5 | Risk estimation - probability |
| `hazardous_situations[].initial_risk_level` | 6 | Risk evaluation |
| `risk_controls[].control_type` | 7.1 | Risk control option analysis |
| `risk_controls[].implementation` | 7.2 | Implementation of risk control measures |
| `risk_controls[].verification` | 7.2 | Verification of risk control implementation |
| `residual_risks` | 7.3 | Residual risk evaluation |
| `residual_risks[].disclosure_text` | 8 | Information for safety disclosure |
| `traceability` | 4.5 | Risk management file traceability |

## Examples

### Input: Validation Function

```typescript
// src/vitals/validation.ts
export function validateHeartRate(bpm: number): ValidationResult {
  if (bpm === null || bpm === undefined) {
    return { valid: false, error: "Heart rate reading is missing" };
  }
  if (bpm < 30 || bpm > 250) {
    return { valid: false, error: `Heart rate ${bpm} outside valid range (30-250 bpm)` };
  }
  if (bpm < 40 || bpm > 180) {
    return {
      valid: true,
      warning: "CRITICAL: Heart rate outside normal range - verify patient status"
    };
  }
  return { valid: true };
}
```

### Output: Extracted Risk Data

```json
{
  "hazards": [
    {
      "hazard_id": "HAZ-001",
      "hazard": "Missing heart rate data",
      "hazard_category": "clinical_data",
      "source": {"files": ["src/vitals/validation.ts:3-5"]}
    },
    {
      "hazard_id": "HAZ-002",
      "hazard": "Heart rate value outside physiological range",
      "hazard_category": "clinical_data",
      "source": {"files": ["src/vitals/validation.ts:6-8"]}
    }
  ],
  "hazardous_situations": [
    {
      "situation_id": "HS-001",
      "hazard_id": "HAZ-001",
      "situation": "Clinician views patient record with missing heart rate, unaware data is absent",
      "foreseeable_sequence": "Sensor disconnection → null value transmitted → displayed as blank → clinician assumes no recent reading",
      "potential_harm": "Delayed detection of cardiac event",
      "severity": "serious",
      "severity_rationale": "Missing cardiac data could delay intervention for arrhythmia",
      "probability": "remote",
      "probability_rationale": "Validation rejects null values with explicit error message",
      "initial_risk_level": "ALARP"
    }
  ],
  "risk_controls": [
    {
      "control_id": "RC-001",
      "situation_id": "HS-001",
      "control_type": "protective_measure",
      "control_description": "Null check rejects missing heart rate values with error message",
      "implementation": {
        "files": ["src/vitals/validation.ts:3-5"],
        "mechanism": "validation"
      },
      "effectiveness": "reduces",
      "residual_risk_level": "acceptable"
    },
    {
      "control_id": "RC-002",
      "situation_id": "HS-002",
      "control_type": "information_for_safety",
      "control_description": "Critical warning displayed for heart rate outside 40-180 bpm range",
      "implementation": {
        "files": ["src/vitals/validation.ts:9-13"],
        "mechanism": "warning"
      },
      "effectiveness": "informs",
      "residual_risk_level": "acceptable"
    }
  ]
}
```

### Input: Error Boundary

```typescript
// src/components/PatientDashboard.tsx
class PatientDashboardErrorBoundary extends React.Component {
  state = { hasError: false, error: null };

  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }

  componentDidCatch(error, errorInfo) {
    logSafetyEvent('DASHBOARD_CRASH', { error, errorInfo, patientId: this.props.patientId });
    notifyOnCallStaff('Patient dashboard crashed - manual monitoring required');
  }

  render() {
    if (this.state.hasError) {
      return (
        <CriticalAlert>
          Dashboard unavailable. Patient requires manual monitoring.
          <ContactSupport errorId={this.state.error.id} />
        </CriticalAlert>
      );
    }
    return this.props.children;
  }
}
```

### Output: Extracted Risk Control

```json
{
  "risk_controls": [
    {
      "control_id": "RC-010",
      "situation_id": "HS-005",
      "control_type": "protective_measure",
      "control_description": "Error boundary catches dashboard crashes, logs safety event, and notifies on-call staff",
      "implementation": {
        "files": ["src/components/PatientDashboard.tsx:1-25"],
        "mechanism": "error_handling"
      },
      "effectiveness": "reduces",
      "residual_risk_level": "ALARP"
    },
    {
      "control_id": "RC-011",
      "situation_id": "HS-005",
      "control_type": "information_for_safety",
      "control_description": "Critical alert displayed instructing manual monitoring when dashboard fails",
      "implementation": {
        "files": ["src/components/PatientDashboard.tsx:15-20"],
        "mechanism": "warning"
      },
      "effectiveness": "informs",
      "residual_risk_level": "acceptable"
    }
  ],
  "residual_risks": [
    {
      "residual_id": "RR-003",
      "situation_id": "HS-005",
      "control_ids": ["RC-010", "RC-011"],
      "residual_description": "Dashboard failure requires manual monitoring; delay possible before staff responds to notification",
      "acceptability": "acceptable_with_disclosure",
      "disclosure_required": true,
      "disclosure_text": "In the event of a system failure, clinical staff will be notified and manual patient monitoring procedures should be initiated."
    }
  ]
}
```

## Validation Criteria

- [ ] Every safety-related code pattern (validation, error handling, alarm) has an associated hazard 🆔 faJh5x
- [ ] All hazards are linked to at least one hazardous situation 🆔 p5BHW8
- [ ] All hazardous situations have severity and probability estimates with rationale from code 🆔 7V7NUq
- [ ] Risk controls reference specific code locations implementing the control 🆔 NM0CDI
- [ ] Control types correctly categorized (inherent_safety, protective_measure, information_for_safety) 🆔 TLNg0e
- [ ] Residual risks identified for situations where controls do not eliminate the hazard 🆔 eH016F
- [ ] Traceability links connect hazards → situations → controls → verification 🆔 fJUxBb
- [ ] Gaps identify missing controls, tests, or documentation 🆔 lHMIVG
- [ ] Security controls are included as risk controls for confidentiality/integrity hazards 🆔 PWLwe1
