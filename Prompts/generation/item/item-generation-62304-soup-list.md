---
id: 725af57
title: "item generation 62304 soup list"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "Prompt"
category: "generation"
level: "item"
standard: "IEC 62304"
clause: "8.1"
inputs: ["item-soup.json"]
outputs: ["Item-SOUP-List.md"]
software_class: "all"
process: "[Software Development Process](../../../Canvases/Software%20Development%20Process.canvas)"
requirements: "[IEC 62304](../../../Requirements/IEC_62304_Requirements.md)"
owner: "[Head of Quality Management](../../../Assets/Head%20of%20Quality%20Management.md)"
---

# Generate Item SOUP List Document

## Context

This prompt generates a human-readable document listing all Software of Unknown Provenance (SOUP) used by a software item. It is explicitly **NOT** a complete system SOUP list — that document is generated at the system level after deduplication.

**Purpose of this document:**
- Document all third-party dependencies in this repository
- Track version and license information
- Assess risk classification for each SOUP
- Identify security vulnerabilities
- Support system-level SOUP aggregation
- Identify qualification requirements

## Inputs

### Required
- **`item-soup.json`** — Output from item-extraction-62304-soup

### Optional
- **`item-architecture.json`** — For component-to-SOUP mapping
- **Vulnerability databases** — For current CVE information
- **License compatibility matrix** — For license assessment

## Instructions

Generate a markdown document following the output structure below. The document should:
1. Clearly state this is an item-level document, not a system SOUP list
2. Summarize dependency statistics
3. Detail each SOUP with all required information
4. Highlight vulnerabilities and risks
5. Include license analysis

## Output Document Structure

```markdown
---
id: 725af57
title: "item generation 62304 soup list"
version: 1
author: "Jakob"
effective_date: 2026-06-16
type: "SOUPList"
document_id: "ITEM-SOUP-[item-id]-[version]"
level: "item"
process: "[Software Development Process](../../Canvases/Software%20Development%20Process.canvas)"
requirements: "[IEC 62304](../../Requirements/IEC_62304_Requirements.md)"
owner: "[Head of Quality Management](../../Assets/Head%20of%20Quality%20Management.md)"
---

# Item SOUP List

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
> This document lists SOUP dependencies used by **[Item Name]** only.
> It is NOT a consolidated system SOUP list.
>
> For complete SOUP documentation, see the **System-Level** documents:
> - System SOUP List: `SYS-SOUP-[system]-[version].md`
> - SOUP Qualification Records: per-SOUP qualification files

---

## 1. Executive Summary

### 1.1 SOUP Statistics

| Metric | Count |
|--------|-------|
| Direct Dependencies | [n] |
| Transitive Dependencies | [n] |
| Total Dependencies | [n] |
| Production Dependencies | [n] |
| Development Dependencies | [n] |
| Safety-Relevant SOUP | [n] |
| Critical Risk SOUP | [n] |
| Known Vulnerabilities | [n] |
| Outdated Packages | [n] |

### 1.2 Risk Distribution

| Risk Level | Count | Percentage |
|------------|-------|------------|
| Critical | [n] | [%] |
| Major | [n] | [%] |
| Minor | [n] | [%] |
| Negligible | [n] | [%] |

### 1.3 License Summary

| License | Count | Compatible |
|---------|-------|------------|
| MIT | [n] | Yes |
| Apache-2.0 | [n] | Yes |
| BSD-3-Clause | [n] | Yes |
| GPL-3.0 | [n] | Review Required |

---

## 2. Action Items

> Items requiring immediate attention

| Priority | SOUP | Action | Rationale |
|----------|------|--------|-----------|
| High | [SOUP-001] | [action] | [rationale] |
| High | [SOUP-002] | [action] | [rationale] |
| Medium | [SOUP-003] | [action] | [rationale] |

---

## 3. Critical and Major SOUP

### 3.1 Critical Risk Dependencies

These dependencies are in safety-relevant code paths.

#### [ITEM-XX-SOUP-001]: [Package Name]

**Version:** [resolved version]
**Version Constraint:** [constraint from manifest]

**Source:**
- Registry: [npm | pypi | maven | etc]
- Repository: [URL]
- Package URL: [URL]

**License:** [SPDX identifier] - [Compatible: Yes/No]

**Dependency Type:** [direct | transitive]
**Environment:** [production | development]

**Purpose:** [why this package is used]

**Functionality Used:**
- [feature 1]
- [feature 2]

**Risk Classification:** Critical
**Risk Rationale:** [justification]

**Safety Relevant:** Yes
**Safety Analysis:** [how it affects safety]

**Failure Modes:**
- [failure mode 1]
- [failure mode 2]

**Used by Components:**
- [ITEM-XX-COMP-001]
- [ITEM-XX-COMP-002]

**Code Locations:**
- `[path:line]`
- `[path:line]`

**Maintenance Status:**
- Last Release: [date]
- Days Since Release: [n]
- Status: [active | maintained | minimal | abandoned | deprecated]

**Known Vulnerabilities:**
| CVE | Severity | Description | Fixed In | Affects Us | Mitigation |
|-----|----------|-------------|----------|------------|------------|
| [CVE-XXXX-XXXXX] | [high] | [description] | [version] | [yes/no] | [mitigation] |

**Qualification Requirements:**
- Documentation: [required artifacts]
- Testing: [testing approach]
- Verification Status: [not_started | in_progress | complete]

**Traces To:**
- Hazards: [ITEM-XX-HAZ-xxx]
- Requirements: [ITEM-XX-REQ-xxx]

---

[Repeat for each critical SOUP]

---

### 3.2 Major Risk Dependencies

These dependencies are core to functionality.

#### [ITEM-XX-SOUP-010]: [Package Name]

[Same structure as critical, abbreviated where appropriate]

---

[Repeat for each major SOUP]

---

## 4. Minor and Negligible SOUP

### 4.1 Minor Risk Dependencies

| ID | Name | Version | Purpose | License |
|----|------|---------|---------|---------|
| [ITEM-XX-SOUP-020] | [name] | [version] | [purpose] | [license] |

### 4.2 Negligible Risk Dependencies (Development Only)

| ID | Name | Version | Purpose | License |
|----|------|---------|---------|---------|
| [ITEM-XX-SOUP-050] | [name] | [version] | [purpose] | [license] |

---

## 5. Runtime SOUP

Non-code dependencies required at runtime.

### 5.1 Summary

| ID | Type | Name | Version | Safety Relevant |
|----|------|------|---------|-----------------|
| [ITEM-XX-RT-001] | [os | runtime | database] | [name] | [version] | [yes/no] |

### 5.2 Details

#### [ITEM-XX-RT-001]: [Component Name]

**Type:** [os | runtime | database | message_queue | cache]

**Name:** [component name]

**Version:** [version]

**Source:** [vendor or distribution]

**Purpose:** [why required]

**Safety Relevant:** [yes | no]

**Configuration Notes:** [relevant settings]

**Used by Components:**
- [ITEM-XX-COMP-xxx]

---

## 6. Dependency Analysis

### 6.1 Dependency Tree Statistics

| Metric | Value |
|--------|-------|
| Maximum Depth | [n] |
| Average Depth | [n.n] |
| Packages at Depth > 3 | [n] |

**Deep Dependencies (depth > 3):**
- [package] (depth [n])
- [package] (depth [n])

### 6.2 Version Duplications

| Package | Versions | Reason |
|---------|----------|--------|
| [package] | [v1, v2] | [why multiple versions] |

### 6.3 Dependency Chain Examples

**[Critical SOUP] Dependency Chain:**
```
[root package] → [intermediate] → [intermediate] → [target SOUP]
```

---

## 7. License Analysis

### 7.1 All Licenses Found

| License | Count | Copyleft | Compatible |
|---------|-------|----------|------------|
| MIT | [n] | No | Yes |
| Apache-2.0 | [n] | No | Yes |
| GPL-3.0 | [n] | Yes | Review |
| Unknown | [n] | ? | Review |

### 7.2 Copyleft Dependencies

> These require compliance review

| Package | License | Linked How |
|---------|---------|------------|
| [package] | [GPL-3.0] | [static | dynamic | devOnly] |

### 7.3 Unknown Licenses

> These require investigation

| Package | License Field |
|---------|---------------|
| [package] | [raw license text] |

### 7.4 License Conflicts

[If any conflicts exist, describe them]

---

## 8. Security Analysis

### 8.1 Vulnerability Summary

| Severity | Count | Packages Affected |
|----------|-------|-------------------|
| Critical | [n] | [packages] |
| High | [n] | [packages] |
| Medium | [n] | [packages] |
| Low | [n] | [packages] |

### 8.2 Vulnerabilities Requiring Action

| CVE | SOUP | Severity | Status | Action |
|-----|------|----------|--------|--------|
| [CVE-XXXX-XXXXX] | [package] | [critical] | [unfixed] | [upgrade to X.Y.Z] |

### 8.3 Mitigated Vulnerabilities

| CVE | SOUP | Mitigation |
|-----|------|------------|
| [CVE-XXXX-XXXXX] | [package] | [mitigation approach] |

---

## 9. Gaps and Issues

### 9.1 Summary

| Priority | Count |
|----------|-------|
| High | [n] |
| Medium | [n] |
| Low | [n] |

### 9.2 Gap Details

| ID | Type | Description | Affected SOUP | Priority | Recommendation |
|----|------|-------------|---------------|----------|----------------|
| [ITEM-XX-SOUP-GAP-001] | [type] | [description] | [SOUP IDs] | [priority] | [recommendation] |

**Gap Types:**
- `missing_version_lock`: No lock file, versions may drift
- `vulnerable_dependency`: Known CVE without fix or mitigation
- `abandoned_package`: No maintenance, consider alternatives
- `missing_license`: License not identified
- `excessive_depth`: Deeply nested, hard to track

---

## 10. Traceability

### 10.1 SOUP to Components

| SOUP | Components Using It |
|------|---------------------|
| [ITEM-XX-SOUP-001] | [COMP-001], [COMP-002] |

### 10.2 SOUP to Hazards

| SOUP | Hazards |
|------|---------|
| [ITEM-XX-SOUP-001] | [HAZ-001] |

### 10.3 SOUP to Requirements

| SOUP | Requirements |
|------|--------------|
| [ITEM-XX-SOUP-001] | [REQ-001] |

---

## Appendix A: Manifest Files Analyzed

| File | Type |
|------|------|
| [package.json] | [npm manifest] |
| [package-lock.json] | [npm lock file] |

---

## Appendix B: Extraction Metadata

| Attribute | Value |
|-----------|-------|
| Repository | [repository] |
| Commit | [commit] |
| Extraction Date | [extracted_at] |
| Extractor Version | [extractor_version] |
| Standard | [standard] |

---

*This document is part of the regulatory documentation for [Item Name].*
*IEC 62304:2006+AMD1:2015 Clause 8.1 Compliant — Item Level*
```

## Compliance Checklist

Before finalizing the document:

- [ ] Document clearly states it is item-level, not system SOUP list 🆔 Lrrzey
- [ ] All direct dependencies documented 🆔 lwnlre
- [ ] Version information from lock file 🆔 1ZwHlx
- [ ] License identified for all direct dependencies 🆔 BTWV7j
- [ ] Risk classification with rationale 🆔 Fh1lFn
- [ ] Safety-relevant SOUP has safety analysis 🆔 ABYEp6
- [ ] Known vulnerabilities documented with CVEs 🆔 5cuKph
- [ ] Qualification requirements specified 🆔 G8ELxc
- [ ] Runtime dependencies included 🆔 0qwbh7
- [ ] Action items prioritized 🆔 8G5b6N

## Validation Criteria

- [ ] Document follows the prescribed structure 🆔 GT6TIu
- [ ] All sections populated from extracted JSON 🆔 PDSWBO
- [ ] Missing data flagged with [TODO] markers 🆔 UQ2oqZ
- [ ] Important notice about document scope is prominent 🆔 1hCiiK
- [ ] References to system-level documents are included 🆔 E6sUD5
- [ ] Document is readable by both developers and QA 🆔 4blXt2
