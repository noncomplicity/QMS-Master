---
id: 725af57
title: "item generation 62304 requirements"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Prompt"
category: "generation"
level: "item"
standard: "IEC 62304"
clause: "5.2"
inputs: ["item-requirements.json"]
outputs: ["Item-Requirements.md"]
software_class: "all"
process: "[Software Development Process](../../../Canvases/Software%20Development%20Process.canvas)"
requirements: "[IEC 62304](../../../Requirements/IEC_62304_Requirements.md)"
owner: "[Head of Quality Management](../../../Assets/Head%20of%20Quality%20Management.md)"
---

# Generate Item Requirements Document

## Context

This prompt generates a human-readable document describing the requirements implemented by a software item. It is explicitly **NOT** a full Software Requirements Specification (SRS) — that document is generated at the system level.

**Purpose of this document:**
- Document requirements implemented in this repository
- Provide source code traceability for each requirement
- Support system-level SRS aggregation
- Enable gap analysis against parent requirements
- Track verification status for each requirement

## Inputs

### Required
- **`item-requirements.json`** — Output from item-extraction-62304-requirements

### Optional
- **`item-architecture.json`** — For component-to-requirement mapping
- **`item-verification.json`** — For test coverage information
- **Parent requirements** — For upward traceability

## Instructions

Generate a markdown document following the output structure below. The document should:
1. Clearly state this is an item-level document, not a system SRS
2. Summarize requirements by type
3. Detail each requirement with source code references
4. Include verification status
5. Identify gaps and issues

## Output Document Structure

```markdown
---
id: 725af57
title: "item generation 62304 requirements"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Requirements"
document_id: "ITEM-REQ-[item-id]-[version]"
level: "item"
process: "[Software Development Process](../../Canvases/Software%20Development%20Process.canvas)"
requirements: "[IEC 62304](../../Requirements/IEC_62304_Requirements.md)"
owner: "[Head of Quality Management](../../Assets/Head%20of%20Quality%20Management.md)"
---

# Item Requirements

## [Item Name]

**Software Item:** [item_id]
**Repository:** [repository URL]
**Version/Commit:** [commit hash]
**Safety Classification:** [Class A/B/C]
**Extraction Date:** [date]

---

## Important Notice

> This is an **ITEM-LEVEL** document.
>
> This document describes requirements implemented in **[Item Name]**.
> It is NOT a Software Requirements Specification (SRS).
>
> For complete requirements documentation, see the **System-Level** documents:
> - Software Requirements Specification: `SYS-SRS-[system]-[version].md`
> - System Requirements: `SYS-REQ-[system]-[version].md`

---

## 1. Executive Summary

### 1.1 Requirements Overview

| Category | Count | Verified | Partial | Unverified |
|----------|-------|----------|---------|------------|
| Functional | [n] | [n] | [n] | [n] |
| Interface | [n] | [n] | [n] | [n] |
| Performance | [n] | [n] | [n] | [n] |
| Safety | [n] | [n] | [n] | [n] |
| Security | [n] | [n] | [n] | [n] |
| Data | [n] | [n] | [n] | [n] |
| **Total** | **[n]** | **[n]** | **[n]** | **[n]** |

### 1.2 Key Capabilities

[Brief description of what this item does, derived from functional requirements]

---

## 2. Functional Requirements

Requirements describing what this software item does.

### 2.1 Summary

| ID | Title | Priority | Safety Class | Verified |
|----|-------|----------|--------------|----------|
| [ITEM-XX-REQ-001] | [title] | [priority] | [class] | [status] |
| ... | ... | ... | ... | ... |

### 2.2 Detailed Requirements

#### [ITEM-XX-REQ-001]: [Requirement Title]

**Description:** [From description]

**Type:** Functional

**Priority:** [essential | desirable | optional]

**Safety Class:** [A | B | C]

**Rationale:** [From rationale, if available]

**Source:**
- [file:line-range]
- [file:line-range]

**Parent Requirement:** [parent_id or "Not traced"]

**Verification:**
- **Status:** [verified | partial | unverified]
- **Test Files:** [test_files]
- **Test Names:** [test_names]

---

[Repeat for each functional requirement]

---

## 3. Interface Requirements

Requirements describing how this item communicates with other items/systems.

### 3.1 Provided Interfaces

Interfaces this item EXPOSES to other items.

#### [ITEM-XX-REQ-IF-001]: [Interface Name]

**Description:** [From description]

**Type:** Interface (Provided)

**Protocol:** [HTTP | JMS | gRPC | etc]

**Endpoint:** [endpoint pattern]

**Contract:**
- Request: [schema]
- Response: [schema]
- Errors: [error codes]

**Source:**
- [file:line-range]

**Consumers:** [known consumer items, if any]

---

### 3.2 Consumed Interfaces

Interfaces this item CONSUMES from other items.

#### [ITEM-XX-REQ-IF-010]: [Interface Name]

**Description:** [From description]

**Type:** Interface (Consumed)

**External System:** [system name]

**Protocol:** [HTTP | JMS | etc]

**Endpoint:** [endpoint]

**Failure Handling:** [how failures are handled]

**Source:**
- [file:line-range]

---

## 4. Performance Requirements

Requirements for timing, capacity, and resource usage.

### 4.1 Summary

| ID | Title | Metric | Target | Verified |
|----|-------|--------|--------|----------|
| [ITEM-XX-REQ-PERF-001] | [title] | [metric] | [target] | [status] |

### 4.2 Detailed Requirements

#### [ITEM-XX-REQ-PERF-001]: [Requirement Title]

**Description:** [From description]

**Type:** Performance

**Metric:** [metric name]

**Target:** [target value]

**Source:**
- [file:line-range]

**Verification:**
- **Status:** [verified | partial | unverified]
- **Evidence:** [test file or measurement]

---

## 5. Safety Requirements

Requirements implementing risk control measures.

### 5.1 Summary

| ID | Title | Related Hazard | Control Type | Verified |
|----|-------|----------------|--------------|----------|
| [ITEM-XX-REQ-SAF-001] | [title] | [hazard ID] | [type] | [status] |

### 5.2 Detailed Requirements

#### [ITEM-XX-REQ-SAF-001]: [Requirement Title]

**Description:** [From description]

**Type:** Safety

**Safety Class:** [B | C]

**Related Hazard:** [ITEM-XX-HAZ-xxx]

**Risk Control Measure:** [description]

**Source:**
- [file:line-range]

**Verification:**
- **Status:** [verified | partial | unverified]
- **Evidence:** [test file]

---

## 6. Security Requirements

Requirements for access control, authentication, and data protection.

### 6.1 Summary

| ID | Title | Category | Verified |
|----|-------|----------|----------|
| [ITEM-XX-REQ-SEC-001] | [title] | [auth | encryption | audit | etc] | [status] |

### 6.2 Detailed Requirements

#### [ITEM-XX-REQ-SEC-001]: [Requirement Title]

**Description:** [From description]

**Type:** Security

**Category:** [authentication | authorization | encryption | audit | input_validation]

**Source:**
- [file:line-range]

**Verification:**
- **Status:** [verified | partial | unverified]
- **Evidence:** [test file]

---

## 7. Data Requirements

Requirements for data handling, storage, and integrity.

### 7.1 Summary

| ID | Title | Data Type | Verified |
|----|-------|-----------|----------|
| [ITEM-XX-REQ-DATA-001] | [title] | [data type] | [status] |

### 7.2 Detailed Requirements

#### [ITEM-XX-REQ-DATA-001]: [Requirement Title]

**Description:** [From description]

**Type:** Data

**Data Entity:** [entity name]

**Constraints:** [validation rules, if any]

**Source:**
- [file:line-range]

---

## 8. Gaps and Issues

### 8.1 Summary

| Priority | Count |
|----------|-------|
| High | [n] |
| Medium | [n] |
| Low | [n] |

### 8.2 Gap Details

| ID | Type | Description | Affected Items | Priority | Recommendation |
|----|------|-------------|----------------|----------|----------------|
| [ITEM-XX-REQ-GAP-001] | [type] | [description] | [IDs] | [priority] | [recommendation] |

---

## 9. Traceability

### 9.1 To Parent Requirements

| Item Requirement | Parent Requirement | Status |
|------------------|-------------------|--------|
| [ITEM-XX-REQ-001] | [parent ID] | [traced | assumed | missing] |

### 9.2 To Components

| Requirement | Component |
|-------------|-----------|
| [ITEM-XX-REQ-001] | [ITEM-XX-COMP-xxx] |

### 9.3 To Verification

| Requirement | Test Status | Test File |
|-------------|-------------|-----------|
| [ITEM-XX-REQ-001] | [status] | [file] |

---

## Appendix A: Extraction Metadata

| Attribute | Value |
|-----------|-------|
| Repository | [repository] |
| Commit | [commit] |
| Extraction Date | [extracted_at] |
| Extractor Version | [extractor_version] |
| Standard | [standard] |

---

*This document is part of the regulatory documentation for [Item Name].*
*IEC 62304:2006+AMD1:2015 Clause 5.2 Compliant — Item Level*
```

## Compliance Checklist

Before finalizing the document:

- [ ] Document clearly states it is item-level, not system SRS 🆔 awRt43
- [ ] All requirements from extraction are included 🆔 WMrK36
- [ ] Requirements are grouped by type 🆔 k4uvSB
- [ ] Each requirement has source code references 🆔 EQXWd9
- [ ] Safety requirements link to hazards 🆔 AsN2KH
- [ ] Interface requirements specify contracts 🆔 Yns5zz
- [ ] Verification status is documented 🆔 yUNlpS
- [ ] Gaps are identified and prioritized 🆔 Pwda9S
- [ ] Traceability to parent requirements attempted 🆔 LOnadT
- [ ] Extraction metadata included 🆔 gskvUO

## Validation Criteria

- [ ] Document follows the prescribed structure 🆔 PkYZkL
- [ ] All sections populated from extracted JSON 🆔 ayFX7N
- [ ] Missing data flagged with [TODO] markers 🆔 M4jjYE
- [ ] Important notice about document scope is prominent 🆔 F3aywk
- [ ] References to system-level documents are included 🆔 HTqh3V
- [ ] Document is readable by software engineers 🆔 rTpWMT
