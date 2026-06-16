---
id:
title: "Generate Software Architecture Description"
version:
author:
effective_date:
type: "Prompt"
category: "generation"
standard: "IEC 62304"
clause: "5.3"
inputs: ["architecture.json (from extraction)", "soup-list.json", "requirements.json"]
outputs: ["Software Architecture Description (SAD) document"]
software_class: "all"
process: "[Software Development Process](../Canvases/Software%20Development%20Process.canvas)"
requirements:
owner: "[Head of Quality Management](../Assets/Head%20of%20Quality%20Management.md)"
---

# Generate Software Architecture Description

## Context

The Software Architecture Description (SAD) documents the software system's decomposition into software items per IEC 62304 clause 5.3. This prompt transforms extracted architecture data into a compliant SAD suitable for design review and regulatory submission.

## Inputs

### Required
- `architecture.json` — Output from [extraction-62304-architecture](../extraction/extraction-62304-architecture.md)
- `soup-list.json` — Output from [extraction-62304-soup](../extraction/extraction-62304-soup.md)

### Optional (enriches output)
- `requirements.json` — Links components to requirements
- System architecture (hardware/software boundaries)
- Risk management file (safety segregation rationale)

## Instructions

1. **Validate Input Data**
   - Verify architecture.json contains complete software item hierarchy
   - Confirm SOUP components are catalogued
   - Check interface definitions are complete

2. **Structure Component Documentation**
   For each software item:
   - Document purpose and responsibility
   - Define interfaces provided and consumed
   - Specify dependencies
   - Assign and justify safety classification

3. **Generate Diagrams**
   Create text-based diagram definitions for:
   - Component hierarchy (tree structure)
   - Dependency graph (what depends on what)
   - Data flow diagrams (for safety-critical paths)
   - Deployment topology

4. **Document SOUP Integration**
   For each SOUP component:
   - How it's integrated
   - What functionality is used
   - Isolation mechanisms
   - Failure handling

5. **Address Safety Segregation**
   Document:
   - How safety-critical components are isolated
   - Failure containment mechanisms
   - Defensive programming approaches

6. **Establish Traceability**
   Link:
   - Components to requirements they implement
   - Components to SOUP they depend on
   - Components to tests that verify them

## Output Schema

Generate a markdown document with this structure:

```markdown
---
id:
title: "Software Architecture Description - [Product Name]"
version:
author:
effective_date:
type: "Specification"
document_id: "SAD-[product]-[version]"
software_safety_class: "A | B | C"
process: "[Software Development Process](../Canvases/Software%20Development%20Process.canvas)"
requirements:
owner: "[Head of Software Development](../Assets/Head%20of%20Software%20Development.md)"
---

# Software Architecture Description

## 1. Introduction

### 1.1 Purpose
This document describes the software architecture of [Product Name], defining the decomposition into software items and their relationships per IEC 62304.

### 1.2 Scope
[Boundaries of the architecture description]

### 1.3 Software Safety Classification
This software system is classified as **Class [A/B/C]** per IEC 62304:2006+A1:2015.

### 1.4 Architectural Overview
[High-level description of the architecture pattern and key decisions]

### 1.5 Referenced Documents
| Document | Relationship |
|----------|--------------|
| Software Requirements Specification | Requirements this architecture implements |
| Risk Management File | Safety requirements driving architecture |
| SOUP List | Third-party component catalog |

## 2. Software Item Hierarchy

### 2.1 System Decomposition

```
[Product Name] (SOFTWARE SYSTEM)
├── [Subsystem 1] (SOFTWARE ITEM)
│   ├── [Module 1.1] (SOFTWARE ITEM)
│   │   ├── [Unit 1.1.1] (SOFTWARE UNIT)
│   │   └── [Unit 1.1.2] (SOFTWARE UNIT)
│   └── [Module 1.2] (SOFTWARE ITEM)
├── [Subsystem 2] (SOFTWARE ITEM)
│   └── ...
└── [Subsystem N] (SOFTWARE ITEM)
```

### 2.2 Item Classification Summary

| Item ID | Name | Level | Safety Class | Rationale |
|---------|------|-------|--------------|-----------|
| SI-01 | [name] | Subsystem | B | [rationale] |
| SI-01-01 | [name] | Module | B | Inherits from parent |
| ... | ... | ... | ... | ... |

## 3. Software Item Specifications

### 3.1 [Software Item Name] (SI-xx)

#### 3.1.1 Identification
| Property | Value |
|----------|-------|
| Item ID | SI-xx |
| Path | `src/[path]` |
| Level | Subsystem / Module / Unit |
| Safety Class | A / B / C |

#### 3.1.2 Purpose
[Description of what this item does and why it exists]

#### 3.1.3 Responsibilities
- [Primary responsibility]
- [Secondary responsibility]
- ...

#### 3.1.4 Interfaces Provided

| Interface | Type | Consumers | Description |
|-----------|------|-----------|-------------|
| IF-xx-01 | API | [list] | [description] |
| IF-xx-02 | Event | [list] | [description] |

**Interface Definition: IF-xx-01**
```typescript
// Or appropriate language
interface [InterfaceName] {
  // ...
}
```

#### 3.1.5 Interfaces Consumed

| Interface | Provider | Purpose |
|-----------|----------|---------|
| IF-yy-01 | SI-yy | [why needed] |

#### 3.1.6 Dependencies

**Internal Dependencies:**
| Dependency | Coupling | Rationale |
|------------|----------|-----------|
| SI-yy | Loose | [why] |

**External Dependencies (SOUP):**
| SOUP | Version | Usage |
|------|---------|-------|
| [package] | [ver] | [what for] |

#### 3.1.7 Safety Considerations
[If safety class B or C]
- **Safety Function:** [what safety role does this item play]
- **Failure Modes:** [how could it fail]
- **Mitigation:** [how failures are handled]

#### 3.1.8 Requirements Implemented
| Requirement | Description |
|-------------|-------------|
| SRS-xxx | [title] |

---

[Repeat Section 3.x for each significant software item]

## 4. SOUP/OTS Components

### 4.1 SOUP Summary

| ID | Name | Version | Purpose | Safety Class | Integration |
|----|------|---------|---------|--------------|-------------|
| SOUP-001 | [name] | [ver] | [purpose] | [class] | [how integrated] |
| ... | ... | ... | ... | ... | ... |

### 4.2 SOUP Details

#### 4.2.1 [SOUP Name] (SOUP-xxx)

| Property | Value |
|----------|-------|
| Name | [package name] |
| Version | [version] |
| Source | [registry/URL] |
| License | [license type] |
| Safety Class | [class] |

**Purpose:**
[Why this SOUP is used]

**Functionality Used:**
- [Specific function/API 1]
- [Specific function/API 2]

**Integration Points:**
- Used by: [SI-xx, SI-yy]
- Integration method: [direct import, wrapper, etc.]

**Isolation:**
[How failures in this SOUP are contained]

**Verification:**
[How correct behavior is verified]

**Known Anomalies:**
[Any known issues and workarounds]

---

## 5. Interface Definitions

### 5.1 External Interfaces

#### 5.1.1 [External Interface Name] (IF-EXT-xx)

| Property | Value |
|----------|-------|
| Type | REST API / gRPC / etc. |
| Provider | [SI-xx] |
| Consumers | [External system names] |
| Protocol | HTTPS |
| Authentication | [method] |

**Endpoints:**
| Method | Path | Description |
|--------|------|-------------|
| POST | /api/v1/... | [description] |

**Data Schemas:**
```json
{
  // Request/Response schemas
}
```

**Error Handling:**
| Error Code | Meaning | Client Action |
|------------|---------|---------------|
| 400 | ... | ... |
| 500 | ... | ... |

### 5.2 Internal Interfaces

[Similar structure for internal component interfaces]

## 6. Data Architecture

### 6.1 Data Stores
| Store | Type | Purpose | Software Items |
|-------|------|---------|----------------|
| [name] | PostgreSQL | [purpose] | SI-xx, SI-yy |

### 6.2 Data Flows
[Diagram or description of significant data flows]

```
[User] → [API Layer] → [Service Layer] → [Database]
                    ↘ [Message Queue] → [Async Processor]
```

### 6.3 Data Retention and Protection
[Relevant data handling architecture decisions]

## 7. Safety Architecture

### 7.1 Safety-Critical Components

| Component | Safety Class | Safety Function |
|-----------|--------------|-----------------|
| SI-xx | C | [what safety role] |
| SI-yy | B | [what safety role] |

### 7.2 Segregation Mechanisms

**[Mechanism 1]: [Name]**
- Type: Process isolation / Container / Namespace
- Purpose: [what it protects]
- Protected items: [SI-xx, SI-yy]

### 7.3 Failure Propagation Controls

| Failure Type | Source | Control | Outcome |
|--------------|--------|---------|---------|
| [type] | SI-xx | [mechanism] | [safe state] |

### 7.4 Defensive Programming

[Key defensive programming techniques employed]

## 8. Deployment Architecture

### 8.1 Deployment Topology

```
[Production Environment]
├── [Load Balancer]
│   └── [API Servers] (n instances)
│       └── SI-01, SI-02
├── [Database Cluster]
│   └── SI-03
└── [Background Workers]
    └── SI-04
```

### 8.2 Environment Configuration

| Environment | Purpose | Differences |
|-------------|---------|-------------|
| Development | Local dev | Mock external services |
| Staging | Pre-prod testing | Production-like, test data |
| Production | Live system | Full security, real data |

## 9. Traceability

### 9.1 Requirements to Architecture

| Requirement | Implementing Items |
|-------------|-------------------|
| SRS-001 | SI-01, SI-02 |
| SRS-002 | SI-03 |
| ... | ... |

### 9.2 Architecture to SOUP

| Software Item | SOUP Dependencies |
|---------------|-------------------|
| SI-01 | SOUP-001, SOUP-003 |
| SI-02 | SOUP-002 |
| ... | ... |

## 10. Appendices

### Appendix A: Extraction Metadata
- Repository: [repo]
- Commit: [hash]
- Extracted: [timestamp]

### Appendix B: Glossary
| Term | Definition |
|------|------------|
| SOFTWARE ITEM | IEC 62304: Any identifiable part of a software system |
| SOFTWARE UNIT | IEC 62304: Lowest level item, not subdivided |
| SOUP | Software of Unknown Provenance |
```

## Compliance Mapping

| Document Section | IEC 62304 Clause | Evidence Provided |
|------------------|------------------|-------------------|
| Section 2 | 5.3.1 | Software architecture showing decomposition |
| Section 3 | 5.3.2 | Software items identified to item level |
| Section 5 | 5.3.3 | Interfaces between software items |
| Section 4 | 5.3.4 | SOUP functional/performance requirements |
| Section 3.x.1 (Safety Class) | 5.3.5 | Item-level safety classification |
| Section 7 | 5.3.6 | Safety-critical item segregation |

## Validation Criteria

- [ ] All software items from extraction are documented 🆔 OoiSzK
- [ ] Software items form complete hierarchy from system to units 🆔 E4eZXe
- [ ] Each item has interfaces defined (provided and consumed) 🆔 pYYxCe
- [ ] All SOUP dependencies are documented with integration details 🆔 D47Enp
- [ ] Safety classifications are assigned to all items 🆔 i5813J
- [ ] Safety-critical items have segregation mechanisms documented 🆔 97ENCu
- [ ] Deployment topology matches runtime architecture 🆔 ZXvEmg
- [x] Traceability to requirements is complete 🆔 2ZeH1o ✅ 2026-03-20
- [x] Document frontmatter is complete 🆔 f0SfHj ✅ 2026-03-20
