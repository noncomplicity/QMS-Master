---
id:
title: "Extract Software Requirements from Code"
version:
author:
effective_date:
type: "Prompt"
category: "extraction"
standard: "IEC 62304"
clause: "5.2"
inputs: ["source code", "type definitions", "API schemas", "commit history", "PR descriptions"]
outputs: ["requirements.json", "requirement traceability entries"]
software_class: "all"
process: "[[Software Development Process.canvas]]"
requirements: "[[EN 62304 5.2 Software requirements analysis]]"
owner: "[[Head of Quality Management]]"
---

# Extract Software Requirements from Code

## Context

IEC 62304 clause 5.2 requires manufacturers to define and document software requirements derived from system requirements. In a code-as-truth paradigm, requirements are extracted from implemented behavior rather than specified upfront.

This prompt instructs AI workers to analyze source code and derive software requirements that reflect actual system capabilities, ensuring documentation matches implementation.

## Inputs

Analyze the following artifacts:

### Source Code
- **Type definitions**: Interfaces, types, schemas that define data structures
- **API endpoints**: Route handlers, controller methods, service interfaces
- **Configuration**: Environment variables, feature flags, constants
- **Error handling**: Error types, validation rules, boundary checks

### Git History
- **Commit messages**: Extract intent and rationale
- **PR descriptions**: Extract user stories, acceptance criteria
- **PR labels/tags**: Identify requirement categories

### Existing Documentation
- **README files**: High-level capability descriptions
- **API documentation**: OpenAPI/Swagger specs, JSDoc comments
- **Test descriptions**: Test names reveal expected behaviors

## Instructions

1. **Identify Software Units**
   - Locate the main architectural boundaries (modules, services, packages)
   - Map each unit to its primary responsibility

2. **Extract Functional Requirements**
   For each software unit, identify:
   - What inputs it accepts (parameters, payloads, events)
   - What outputs it produces (returns, side effects, emitted events)
   - What transformations it performs
   - What business rules it enforces

3. **Extract Non-Functional Requirements**
   Scan for evidence of:
   - Performance constraints (timeouts, rate limits, batch sizes)
   - Security controls per 5.2.2 e):
     - Authentication mechanisms
     - Authorization/access control
     - Audit trail/logging
     - Communication integrity (TLS, encryption)
     - Malware protection measures
   - Reliability mechanisms (retries, circuit breakers, fallbacks)
   - Compatibility requirements (supported platforms, versions)

4. **Extract User Interface Requirements** (5.2.2 f)
   Document:
   - UI components and user interactions
   - Display formats and data presentation
   - User feedback mechanisms
   - Accessibility requirements

5. **Extract Data and Database Requirements** (5.2.2 g)
   Document:
   - Database schemas and data models
   - Data retention policies
   - Data migration requirements
   - Backup and recovery requirements

6. **Extract Operational Requirements** (5.2.2 h-k)
   Document:
   - Installation and deployment procedures
   - IT-network requirements (ports, protocols, topology)
   - Operation and maintenance procedures
   - User maintenance/configuration capabilities
   - Upgrade and rollback procedures

7. **Extract Interface Requirements** (5.2.2 b-c)
   Document:
   - External API contracts (REST, GraphQL, gRPC)
   - Database schemas and queries
   - Third-party service integrations
   - User interface data flows
   - Input/output characteristics (data types, ranges, limits, defaults)

8. **Map to System Requirements**
   For each extracted requirement:
   - Link to originating commit/PR if identifiable
   - Link to relevant test cases that verify the behavior
   - Identify potential system-level requirement it supports

9. **Classify by Safety Relevance** (5.2.3)
   Flag requirements that relate to:
   - Clinical data handling
   - Safety-critical calculations
   - User safety warnings/alarms (5.2.2 d)
   - Data integrity controls
   - Risk control measures implemented in software

10. **Extract Regulatory Requirements** (5.2.2 l)
    Identify requirements from:
    - Jurisdiction-specific regulations (FDA, MDR, etc.)
    - Applicable standards compliance
    - Labeling and IFU requirements
    - Post-market surveillance requirements

## Output Schema

Generate a JSON file with this structure:

```json
{
  "extraction_metadata": {
    "repository": "<repo-url>",
    "commit": "<commit-hash>",
    "extracted_at": "<ISO-8601-timestamp>",
    "extractor_version": "1.0"
  },
  "requirements": [
    {
      "req_id": "SRS-<component>-<sequence>",
      "title": "<short title>",
      "description": "<detailed description of the requirement>",
      "type": "functional | interface | performance | security | safety | ui | data | operational | regulatory",
      "source": {
        "files": ["<file-path>:<line-range>"],
        "commits": ["<commit-hash>"],
        "prs": ["<pr-number>"]
      },
      "verification": {
        "test_files": ["<test-file-path>"],
        "test_names": ["<test function/description>"]
      },
      "safety_class": "A | B | C | unclassified",
      "safety_rationale": "<why this classification>",
      "interfaces": ["<interface-name>"],
      "dependencies": ["<req_id of dependent requirements>"]
    }
  ],
  "soup_references": [
    {
      "name": "<package-name>",
      "version": "<version>",
      "related_requirements": ["<req_id>"]
    }
  ],
  "gaps": [
    {
      "type": "untested | undocumented | unclear_classification",
      "description": "<what is missing>",
      "affected_requirements": ["<req_id>"]
    }
  ]
}
```

## Compliance Mapping

| Output Element | IEC 62304 Clause | Evidence Provided |
|----------------|------------------|-------------------|
| `requirements[].description` | 5.2.1 | Software requirements from system requirements |
| `requirements[].type: functional` | 5.2.2 a) | Functional and capability requirements |
| `requirements[].source.files` | 5.2.2 b) | Software system inputs and outputs |
| `requirements[].interfaces` | 5.2.2 c) | Interfaces between software and other systems |
| `requirements[].type: safety` | 5.2.2 d) | Alarms, warnings, operator messages |
| `requirements[].type: security` | 5.2.2 e) | Security requirements |
| `requirements[].type: ui` | 5.2.2 f) | User interface requirements |
| `requirements[].type: data` | 5.2.2 g) | Data definition and database requirements |
| `requirements[].type: operational` | 5.2.2 h-k) | Installation, operation, maintenance, IT-network |
| `requirements[].type: regulatory` | 5.2.2 l) | Regulatory requirements |
| `requirements[].safety_class` | 5.2.3 | Risk control measures in software |
| `requirements[].verification` | 5.2.6 | Verification of requirements implementation |

## Examples

### Input: TypeScript Interface

```typescript
// src/vitals/types.ts
interface VitalReading {
  patientId: string;
  timestamp: Date;
  heartRate: number;      // bpm, valid range: 30-250
  bloodPressure: {
    systolic: number;     // mmHg, valid range: 50-300
    diastolic: number;    // mmHg, valid range: 30-200
  };
  oxygenSaturation: number; // percentage, valid range: 70-100
}

// Alarm thresholds defined in constants
const CRITICAL_THRESHOLDS = {
  heartRate: { low: 40, high: 180 },
  oxygenSaturation: { low: 88 }
};
```

### Output: Extracted Requirement

```json
{
  "req_id": "SRS-VITALS-001",
  "title": "Vital Signs Data Structure",
  "description": "The system shall accept vital sign readings containing heart rate (30-250 bpm), blood pressure (systolic 50-300 mmHg, diastolic 30-200 mmHg), and oxygen saturation (70-100%) associated with a patient identifier and timestamp.",
  "type": "functional",
  "source": {
    "files": ["src/vitals/types.ts:1-15"],
    "commits": [],
    "prs": []
  },
  "safety_class": "B",
  "safety_rationale": "Vital sign data contributes to clinical decisions; incorrect ranges could mask critical conditions"
}
```

```json
{
  "req_id": "SRS-VITALS-002",
  "title": "Critical Vital Sign Alarming",
  "description": "The system shall trigger critical alarms when heart rate falls below 40 bpm or exceeds 180 bpm, or when oxygen saturation falls below 88%.",
  "type": "safety",
  "source": {
    "files": ["src/vitals/types.ts:17-20"],
    "commits": [],
    "prs": []
  },
  "safety_class": "C",
  "safety_rationale": "Failure to alarm on critical vital signs could result in patient harm"
}
```

## Validation Criteria

- [ ] Every public function/endpoint has at least one associated requirement 🆔 X32R1i
- [ ] All requirements have a `type` classification 🆔 4IpMBh
- [ ] Safety-related requirements are flagged with `safety_class` B or C 🆔 MrYJUX
- [ ] Requirements have traceable `source` references to code locations 🆔 ZITRI7
- [ ] Interface requirements document data formats and valid ranges 🆔 FHrAoY
- [ ] Gap analysis identifies untested or undocumented behaviors 🆔 2R7Rg2
- [ ] SOUP dependencies are catalogued with related requirements 🆔 u839CN
