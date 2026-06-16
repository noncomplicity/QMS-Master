---
id:
title: "Generate Software Requirements Specification"
version:
author:
effective_date:
type: "Prompt"
category: "generation"
standard: "IEC 62304"
clause: "5.2"
inputs: ["requirements.json (from extraction)", "system requirements", "risk analysis"]
outputs: ["Software Requirements Specification (SRS) document"]
software_class: "all"
process: "[Software Development Process](../Canvases/Software%20Development%20Process.canvas)"
requirements: "[EN 62304 5.2 Software requirements analysis](../Requirements/EN%2062304%205.2%20Software%20requirements%20analysis.md)"
owner: "[Head of Quality Management](../Assets/Head%20of%20Quality%20Management.md)"
---

# Generate Software Requirements Specification

## Context

The Software Requirements Specification (SRS) is a regulated deliverable per IEC 62304 clause 5.2. This prompt transforms extracted requirements data into a compliant SRS document suitable for regulatory submission and design history file inclusion.

## Inputs

### Required
- `requirements.json` — Output from [extraction-62304-requirements](../extraction/extraction-62304-requirements.md)
- `architecture.json` — Output from [extraction-62304-architecture](../extraction/extraction-62304-architecture.md)

### Optional (enriches output)
- System requirements document (parent requirements to trace to)
- Risk management file (hazards requiring software risk control)
- User needs / intended use documentation
- Usability specification

## Instructions

1. **Validate Input Data**
   - Verify requirements.json contains required fields
   - Check for completeness (no gaps flagged as critical)
   - Ensure safety classifications are assigned

2. **Organize Requirements**
   Group requirements by:
   - Functional requirements (capabilities)
   - Interface requirements (external systems)
   - Performance requirements (timing, capacity)
   - Safety requirements (risk controls)
   - Security requirements (access control, data protection)
   - Usability requirements (user interface)

3. **Establish Traceability**
   For each requirement:
   - Link to parent system requirement (if available)
   - Link to source code implementation
   - Link to verification test(s)
   - Link to risk control measures (if safety-related)

4. **Apply Document Template**
   Use the standard SRS structure (see Output Schema)
   - Populate frontmatter with correct metadata
   - Include revision history from git
   - Generate table of contents

5. **Add Compliance Statements**
   Include required regulatory statements:
   - Software safety classification
   - Standards compliance declaration
   - Scope and boundaries

6. **Generate Traceability Matrix**
   Create bidirectional traceability table:
   - System Req → Software Req → Implementation → Test

## Output Schema

Generate a markdown document with this structure:

```markdown
---
id:
title: "Software Requirements Specification - [Product Name]"
version:
author:
effective_date:
type: "Specification"
document_id: "SRS-[product]-[version]"
software_safety_class: "A | B | C"
process: "[Software Development Process](../Canvases/Software%20Development%20Process.canvas)"
requirements: "[EN 62304 5.2](../Requirements/EN%2062304%205.2%20Software%20requirements%20analysis.md)"
owner: "[Head of Software Development](../Assets/Head%20of%20Software%20Development.md)"
---

# Software Requirements Specification

## 1. Introduction

### 1.1 Purpose
[Generated description of the software system purpose]

### 1.2 Scope
[Boundaries of the software system - what is included/excluded]

### 1.3 Intended Use
[Clinical/medical context and intended users]

### 1.4 Software Safety Classification
This software is classified as **Class [A/B/C]** per IEC 62304:2006+A1:2015.

**Classification Rationale:**
[Generated rationale based on safety analysis]

### 1.5 Definitions and Abbreviations
| Term | Definition |
|------|------------|
| ... | ... |

### 1.6 Referenced Documents
| Document | Version | Relationship |
|----------|---------|--------------|
| System Requirements Specification | x.x | Parent requirements |
| Risk Management File | x.x | Hazard analysis |
| ... | ... | ... |

## 2. Functional Requirements

### 2.1 [Feature Area 1]

#### SRS-[ID]-001: [Requirement Title]
**Description:** [Requirement description]

**Type:** Functional

**Priority:** [Essential | Desirable | Optional]

**Safety Class:** [A | B | C]

**Source:**
- Code: `[file:line]`
- Commit: `[hash]`

**Parent Requirement:** [SYS-xxx or N/A]

**Verification:** [Test reference or verification method]

---

[Repeat for each functional requirement]

## 3. Interface Requirements

### 3.1 External Interfaces

#### SRS-IF-001: [Interface Name]
**Description:** [Interface description]

**Type:** Interface

**Protocol:** [HTTP/REST | gRPC | etc.]

**Data Format:**
```json
{
  // Schema definition
}
```

**Error Handling:** [How errors are communicated]

---

### 3.2 Internal Interfaces
[Internal component interfaces]

## 4. Performance Requirements

### 4.1 Response Time Requirements
[Requirements for system response times]

### 4.2 Capacity Requirements
[Requirements for data volumes, concurrent users, etc.]

### 4.3 Reliability Requirements
[Uptime, availability, recovery requirements]

## 5. Safety Requirements

> These requirements implement risk control measures identified in the Risk Management File.

### 5.1 [Safety Feature Area]

#### SRS-SAF-001: [Safety Requirement Title]
**Description:** [Requirement description]

**Type:** Safety

**Safety Class:** [B | C]

**Related Hazard:** [HAZ-xxx from Risk Management File]

**Risk Control Measure:** [Description of how this mitigates the hazard]

**Verification:** [How this will be verified]

---

## 6. Security Requirements

### 6.1 Authentication and Authorization
[Access control requirements]

### 6.2 Data Protection
[Encryption, data handling requirements]

### 6.3 Audit and Logging
[Security logging requirements]

## 7. Usability Requirements

### 7.1 User Interface Requirements
[UI/UX requirements extracted from code]

### 7.2 Accessibility Requirements
[Accessibility compliance requirements]

## 8. SOUP/OTS Requirements

### 8.1 Third-Party Component Requirements
| SOUP | Version | Functional Requirement | Safety Requirement |
|------|---------|----------------------|-------------------|
| [name] | [ver] | [what it must do] | [safety constraints] |

## 9. Traceability Matrix

### 9.1 System to Software Requirements
| System Req | Software Req | Implementation | Verification |
|------------|--------------|----------------|--------------|
| SYS-001 | SRS-001, SRS-002 | SI-01 | TC-001 |
| ... | ... | ... | ... |

### 9.2 Requirements to Risk Controls
| Hazard | Risk Control | Software Req | Verification |
|--------|--------------|--------------|--------------|
| HAZ-001 | RC-001 | SRS-SAF-001 | TC-SAF-001 |
| ... | ... | ... | ... |

## 10. Appendices

### Appendix A: Requirement Change Log
[Auto-generated from git history]

### Appendix B: Extraction Metadata
- Repository: [repo]
- Commit: [hash]
- Extracted: [timestamp]
```

## Compliance Mapping

| Document Section | IEC 62304 Clause | Evidence Provided |
|------------------|------------------|-------------------|
| Section 1.4 | 4.3 | Software safety classification |
| Section 2-7 (all reqs) | 5.2.1 | Software requirements documentation |
| Section 2 | 5.2.2 a) | Functional and capability requirements |
| Section 3 | 5.2.2 b,c) | Inputs, outputs, interfaces |
| Section 5 | 5.2.2 d) | Alarms, warnings, safety features |
| Section 5 | 5.2.3 | Risk control measure requirements |
| Section 4 | 5.2.4 | Non-functional requirements |
| Section 9 | 5.2.6 | Requirements traceability |

## Examples

### Input: requirements.json (partial)

```json
{
  "requirements": [
    {
      "req_id": "SRS-VITALS-001",
      "title": "Vital Signs Data Validation",
      "description": "The system shall validate vital sign readings are within physiological ranges before storage.",
      "type": "functional",
      "source": {
        "files": ["src/vitals/validation.ts:45-78"],
        "commits": ["abc123"]
      },
      "verification": {
        "test_files": ["tests/vitals/validation.test.ts"],
        "test_names": ["validates heart rate range", "rejects invalid BP"]
      },
      "safety_class": "B"
    }
  ]
}
```

### Output: Generated SRS Section

```markdown
#### SRS-VITALS-001: Vital Signs Data Validation
**Description:** The system shall validate vital sign readings are within physiological ranges before storage.

**Type:** Functional

**Priority:** Essential

**Safety Class:** B

**Source:**
- Code: `src/vitals/validation.ts:45-78`
- Commit: `abc123`

**Verification:**
- Test: `tests/vitals/validation.test.ts`
  - "validates heart rate range"
  - "rejects invalid BP"
```

## Validation Criteria

- [ ] All requirements from extraction are included 🆔 qQPyYO
- [ ] Requirements are grouped by type (functional, interface, safety, etc.) 🆔 eruVUb
- [ ] Each requirement has unique identifier (SRS-xxx-nnn) 🆔 ccoFib
- [ ] Safety requirements link to hazards from risk file 🆔 ohfJ2V
- [ ] Traceability matrix is complete (no orphan requirements) 🆔 Vtq5Ny
- [ ] Document frontmatter is complete 🆔 ZmffhL
- [ ] Software safety class is stated with rationale 🆔 Q2HeFP
- [ ] SOUP functional requirements are included 🆔 EqijGQ
- [ ] All requirements have verification method specified 🆔 GbKsLl
