---
id: 725af57
title: "generation 62304 soup list"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Prompt"
category: "generation"
standard: "IEC 62304"
clause: "8.1"
inputs: ["soup-list.json (from extraction)"]
outputs: ["SOUP List document with risk assessment"]
software_class: "all"
process: "[Software Development Process](../Canvases/Software%20Development%20Process.canvas)"
requirements:
owner: "[Head of Quality Management](../Assets/Head%20of%20Quality%20Management.md)"
---

# Generate SOUP List Document

## Context

IEC 62304 clause 8 requires identification, documentation, and risk assessment of all Software of Unknown Provenance (SOUP). This prompt transforms extracted SOUP data into a formal SOUP List document with risk assessment suitable for regulatory submission.

## Inputs

### Required
- `soup-list.json` — Output from [extraction-62304-soup](../extraction/extraction-62304-soup.md)

### Optional
- `architecture.json` — Provides integration context
- Risk management file — Informs safety classification

## Instructions

1. **Organize SOUP by Category**
   Group SOUP items:
   - Application dependencies (direct)
   - Transitive dependencies (indirect)
   - Runtime components (OS, interpreters)
   - Infrastructure (databases, queues)

2. **Assess Each SOUP Item**
   For each SOUP:
   - Document identification information
   - Evaluate risk based on usage and safety relevance
   - Define verification approach
   - Identify known anomalies

3. **Calculate Risk Metrics**
   Assess:
   - Vulnerability exposure (CVE count, severity)
   - Maintenance health (last update, activity)
   - Criticality to safety functions
   - License compatibility

4. **Generate Action Items**
   Identify SOUP requiring:
   - Version updates (security, compatibility)
   - Additional verification
   - Replacement consideration
   - Monitoring configuration

5. **Create Summary Statistics**
   Calculate:
   - Total SOUP count by category
   - Risk distribution
   - Vulnerability summary
   - License summary

## Output Schema

```markdown
---
id: 725af57
title: "generation 62304 soup list"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Specification"
document_id: "SOUP-[product]-[version]"
software_safety_class: "A | B | C"
process: "[Software Development Process](../Canvases/Software%20Development%20Process.canvas)"
requirements:
owner: "[Head of Quality Management](../Assets/Head%20of%20Quality%20Management.md)"
---

# SOUP List

## 1. Introduction

### 1.1 Purpose
This document identifies and assesses all Software of Unknown Provenance (SOUP) used in [Product Name] per IEC 62304 clause 8.

### 1.2 Scope
This list covers all third-party software components including:
- Application dependencies (libraries, frameworks)
- Runtime environments
- Infrastructure components
- Development and build tools (for completeness)

### 1.3 Software Safety Classification
The parent software system is classified as **Class [A/B/C]**. SOUP components inherit safety classification based on their role in safety-critical functionality.

### 1.4 SOUP Management Process
SOUP is managed per [SOUP Management Procedure](link-to-procedure.md):
- Initial assessment during selection
- Ongoing monitoring for vulnerabilities
- Change assessment before updates
- Periodic review per release cycle

## 2. Summary

### 2.1 SOUP Statistics

| Category | Count | Critical | Major | Minor |
|----------|-------|----------|-------|-------|
| Direct Dependencies | [n] | [n] | [n] | [n] |
| Transitive Dependencies | [n] | [n] | [n] | [n] |
| Runtime Components | [n] | [n] | [n] | [n] |
| Infrastructure | [n] | [n] | [n] | [n] |
| **Total** | **[n]** | **[n]** | **[n]** | **[n]** |

### 2.2 Vulnerability Summary

| Severity | Count | Remediation Status |
|----------|-------|-------------------|
| Critical | [n] | [n] addressed, [n] pending |
| High | [n] | [n] addressed, [n] pending |
| Medium | [n] | [n] addressed, [n] pending |
| Low | [n] | [n] addressed, [n] pending |

### 2.3 License Summary

| License Type | Count | Compatibility |
|--------------|-------|---------------|
| MIT | [n] | Compatible |
| Apache-2.0 | [n] | Compatible |
| GPL-3.0 | [n] | Review Required |
| ... | ... | ... |

### 2.4 Maintenance Health

| Status | Count | Action Required |
|--------|-------|-----------------|
| Active | [n] | None |
| Maintained | [n] | Monitor |
| Minimal | [n] | Evaluate alternatives |
| Abandoned | [n] | Replace |
| Deprecated | [n] | Migration plan |

## 3. Direct Dependencies

### 3.1 [SOUP Name] (SOUP-xxx)

| Property | Value |
|----------|-------|
| **SOUP ID** | SOUP-xxx |
| **Name** | [package-name] |
| **Version** | [version] |
| **Registry** | [npm / pypi / etc.] |
| **License** | [license] |
| **Repository** | [URL] |

#### Purpose
[Why this SOUP is used in the system]

#### Functionality Used
| Function/API | Usage |
|--------------|-------|
| [function] | [how it's used] |
| [module] | [how it's used] |

#### Integration
| Property | Value |
|----------|-------|
| Integrated by | SI-xx, SI-yy |
| Code locations | `src/path/file.ts:line` |
| Integration method | Direct import / Wrapper |

#### Risk Assessment

| Factor | Assessment | Notes |
|--------|------------|-------|
| **Criticality** | Critical / Major / Minor | [rationale] |
| **Safety Relevance** | Yes / No | [if yes, how] |
| **Safety Class** | A / B / C | [inherited from usage] |
| **Failure Impact** | [description] | |
| **Isolation** | [how contained] | |

#### Maintenance Status

| Property | Value |
|----------|-------|
| Last release | [date] |
| Days since release | [n] |
| Status | Active / Maintained / Minimal / Abandoned |
| Contributors (12mo) | [n] |
| Open issues | [n] |

#### Known Anomalies

| CVE / Issue | Severity | Description | Status | Mitigation |
|-------------|----------|-------------|--------|------------|
| CVE-xxxx-xxxxx | High | [description] | Fixed in [ver] | [action] |
| [issue-ref] | Medium | [description] | Open | [workaround] |

*If no known anomalies:* None identified as of [date].

#### Verification

| Verification Method | Status | Evidence |
|--------------------|--------|----------|
| Unit tests covering usage | Complete | [test-file] |
| Integration tests | Complete | [test-file] |
| Documentation review | Complete | [notes] |
| Security advisory monitoring | Ongoing | [process] |

#### Action Items

| Priority | Action | Rationale | Due |
|----------|--------|-----------|-----|
| [High/Med/Low] | [action] | [why] | [when] |

---

[Repeat Section 3.x for each direct dependency]

## 4. Transitive Dependencies

> Transitive dependencies are automatically included by direct dependencies. Only significant transitive dependencies (critical path, known vulnerabilities) are detailed here.

### 4.1 Summary Table

| Name | Version | Pulled by | Criticality | Vulnerabilities |
|------|---------|-----------|-------------|-----------------|
| [name] | [ver] | [parent] | [level] | [count] |
| ... | ... | ... | ... | ... |

### 4.2 Notable Transitive Dependencies

[Detail any transitive dependencies with vulnerabilities or high criticality]

## 5. Runtime Components

### 5.1 Operating System / Base Image

| Property | Value |
|----------|-------|
| **Name** | [e.g., Alpine Linux] |
| **Version** | [version] |
| **Source** | [Docker Hub / etc.] |
| **Purpose** | Container base image |
| **Security** | [hardening measures] |

### 5.2 Runtime Environment

| Property | Value |
|----------|-------|
| **Name** | [e.g., Node.js] |
| **Version** | [version] |
| **LTS Status** | [Active LTS / Maintenance / Current] |
| **EOL Date** | [date] |

### 5.3 Database

[If applicable - database as SOUP]

### 5.4 Other Infrastructure

[Message queues, caches, etc.]

## 6. SOUP Verification Strategy

### 6.1 Initial Verification
- [ ] License compatibility review 🆔 BRKghe
- [ ] Security vulnerability scan 🆔 kYUOpQ
- [ ] Functionality documentation review 🆔 2XLDWy
- [ ] Integration testing 🆔 7rMRzX

### 6.2 Ongoing Verification
- [ ] Automated dependency vulnerability scanning (CI/CD) 🆔 5aTs3S
- [ ] Security advisory monitoring 🆔 5B0l1g
- [ ] Version update assessment before upgrade 🆔 MKYbeH
- [ ] Periodic review (per release) 🆔 x0PCvq

### 6.3 Tools Used
| Tool | Purpose | Frequency |
|------|---------|-----------|
| [tool-name] | Vulnerability scanning | Per commit |
| [tool-name] | License compliance | Per release |

## 7. SOUP Change Management

### 7.1 Update Process
1. Identify update need (security, functionality, compatibility)
2. Review changelog and breaking changes
3. Assess impact on software items
4. Update in development branch
5. Execute verification tests
6. Document in SOUP List
7. Release through change control

### 7.2 Recent Changes

| SOUP | Previous | Current | Date | Reason |
|------|----------|---------|------|--------|
| [name] | [old-ver] | [new-ver] | [date] | [security/feature] |

## 8. Action Items

### 8.1 High Priority

| SOUP | Action | Rationale | Owner | Due |
|------|--------|-----------|-------|-----|
| [name] | [action] | [why urgent] | [who] | [when] |

### 8.2 Medium Priority

| SOUP | Action | Rationale | Owner | Due |
|------|--------|-----------|-------|-----|
| [name] | [action] | [why] | [who] | [when] |

### 8.3 Low Priority / Monitoring

| SOUP | Action | Rationale | Owner | Due |
|------|--------|-----------|-------|-----|
| [name] | [action] | [why] | [who] | [when] |

## 9. Appendices

### Appendix A: Complete Dependency Tree
[Generated dependency tree showing full hierarchy]

### Appendix B: Extraction Metadata
- Repository: [repo]
- Commit: [hash]
- Manifest files: [list]
- Extracted: [timestamp]

### Appendix C: Abbreviations
| Term | Definition |
|------|------------|
| SOUP | Software of Unknown Provenance |
| OTS | Off-The-Shelf software |
| CVE | Common Vulnerabilities and Exposures |
| LTS | Long Term Support |
```

## Compliance Mapping

| Document Section | IEC 62304 Clause | Evidence Provided |
|------------------|------------------|-------------------|
| Section 3 (all items) | 8.1.1 | SOUP identification |
| Title, Purpose | 8.1.2 a) | SOUP title and manufacturer |
| SOUP ID | 8.1.2 b) | Unique SOUP designation |
| Version | 8.1.2 c) | SOUP version |
| Known Anomalies | 8.1.2 d) | Known anomalies relevant to safety |
| Section 7 | 8.1.3 | SOUP change management |
| Verification | 8.2 | SOUP verification |

## Validation Criteria

- [ ] All dependencies from package manifests are included 🆔 diaySO
- [ ] Each SOUP has complete identification (name, version, source) 🆔 fnTrRT
- [ ] Risk classification is assigned to all SOUP 🆔 fArkEx
- [ ] Safety-relevant SOUP items have safety analysis 🆔 qOROgV
- [ ] Known vulnerabilities are documented with CVE IDs 🆔 v7kdnJ
- [ ] License types are documented and compatibility assessed 🆔 lZNroo
- [ ] Verification approach is defined for each SOUP 🆔 LZzyO3
- [ ] Action items are prioritized and assigned 🆔 xkPpub
- [ ] Runtime and infrastructure SOUP are included 🆔 DkH52z
- [ ] Extraction metadata is documented 🆔 tDg68r
